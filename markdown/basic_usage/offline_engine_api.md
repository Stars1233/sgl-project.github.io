# Offline Engine API

SGLang provides a direct inference engine without the need for an HTTP server, especially for use cases where additional HTTP server adds unnecessary complexity or overhead. Here are two general use cases:

- Offline Batch Inference
- Custom Server on Top of the Engine

This document focuses on the offline batch inference, demonstrating four different inference modes:

- Non-streaming synchronous generation
- Streaming synchronous generation
- Non-streaming asynchronous generation
- Streaming asynchronous generation

Additionally, you can easily build a custom server on top of the SGLang offline engine. A detailed example working in a python script can be found in [custom_server](https://github.com/sgl-project/sglang/blob/main/examples/runtime/engine/custom_server.py).



## Nest Asyncio
Note that if you want to use **Offline Engine** in ipython or some other nested loop code, you need to add the following code:
```python
import nest_asyncio

nest_asyncio.apply()

```

## Advanced Usage

The engine supports [vlm inference](https://github.com/sgl-project/sglang/blob/main/examples/runtime/engine/offline_batch_inference_vlm.py) as well as [extracting hidden states](https://github.com/sgl-project/sglang/blob/main/examples/runtime/hidden_states). 

Please see [the examples](https://github.com/sgl-project/sglang/tree/main/examples/runtime/engine) for further use cases.

## Offline Batch Inference

SGLang offline engine supports batch inference with efficient scheduling.


```python
# launch the offline engine
import asyncio

import sglang as sgl
import sglang.test.doc_patch  # noqa: F401
from sglang.utils import async_stream_and_merge, stream_and_merge

llm = sgl.Engine(model_path="qwen/qwen2.5-0.5b-instruct")
```

    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-27 06:35:45.979 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 06:35:45] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 06:35:45.979 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 06:35:45] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 06:35:45.979 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 06:35:45] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 06:35:45.979 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 06:35:45] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 06:35:45.980 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 06:35:45] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.94it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.92it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:02<00:35,  1.53it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:02<00:35,  1.53it/s]Compiling num tokens (num_tokens=6144):   5%|▌         | 3/58 [00:02<00:35,  1.53it/s]

    Compiling num tokens (num_tokens=5632):   5%|▌         | 3/58 [00:02<00:35,  1.53it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:02<00:14,  3.56it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:02<00:14,  3.56it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:02<00:14,  3.56it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:02<00:14,  3.56it/s]Compiling num tokens (num_tokens=3840):  10%|█         | 6/58 [00:02<00:14,  3.56it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.78it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.78it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.78it/s]

    Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.78it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.78it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.78it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:02<00:03, 11.62it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:02<00:03, 11.62it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:02<00:03, 11.62it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:02<00:03, 11.62it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:02<00:03, 11.62it/s]Compiling num tokens (num_tokens=1280):  26%|██▌       | 15/58 [00:02<00:03, 11.62it/s]Compiling num tokens (num_tokens=1024):  26%|██▌       | 15/58 [00:02<00:03, 11.62it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:02<00:02, 18.23it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:02<00:02, 18.23it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:02<00:02, 18.23it/s]

    Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:02<00:02, 18.23it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:02<00:02, 18.23it/s]Compiling num tokens (num_tokens=704):  36%|███▌      | 21/58 [00:03<00:02, 18.23it/s]Compiling num tokens (num_tokens=640):  36%|███▌      | 21/58 [00:03<00:02, 18.23it/s]Compiling num tokens (num_tokens=576):  36%|███▌      | 21/58 [00:03<00:02, 18.23it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:03<00:01, 26.17it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:03<00:01, 26.17it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:03<00:01, 26.17it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:01, 26.17it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:03<00:01, 26.17it/s]Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:03<00:01, 26.17it/s]Compiling num tokens (num_tokens=352):  48%|████▊     | 28/58 [00:03<00:01, 26.17it/s]Compiling num tokens (num_tokens=320):  48%|████▊     | 28/58 [00:03<00:01, 26.17it/s]

    Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 33.28it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 33.28it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:03<00:00, 33.28it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:03<00:00, 33.28it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:03<00:00, 33.28it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:03<00:00, 33.28it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:03<00:00, 33.28it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:03<00:00, 37.18it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:03<00:00, 37.18it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:03<00:00, 37.18it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:03<00:00, 37.18it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:03<00:00, 37.18it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:03<00:00, 37.18it/s]

    Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:03<00:00, 37.18it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:03<00:00, 39.78it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:03<00:00, 39.78it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:03<00:00, 39.78it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:03<00:00, 39.78it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:03<00:00, 39.78it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:03<00:00, 39.78it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:03<00:00, 39.78it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:03<00:00, 44.25it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:03<00:00, 44.25it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:03<00:00, 44.25it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:03<00:00, 44.25it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:03<00:00, 44.25it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:03<00:00, 44.25it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.12it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.75 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.69 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.69 GB):   3%|▎         | 2/58 [00:00<00:04, 11.57it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.69 GB):   3%|▎         | 2/58 [00:00<00:04, 11.57it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=118.68 GB):   3%|▎         | 2/58 [00:00<00:04, 11.57it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.68 GB):   7%|▋         | 4/58 [00:00<00:04, 12.10it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.68 GB):   7%|▋         | 4/58 [00:00<00:04, 12.10it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.67 GB):   7%|▋         | 4/58 [00:00<00:04, 12.10it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=118.67 GB):  10%|█         | 6/58 [00:00<00:03, 13.14it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.68 GB):  10%|█         | 6/58 [00:00<00:03, 13.14it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=118.67 GB):  10%|█         | 6/58 [00:00<00:03, 13.14it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.67 GB):  14%|█▍        | 8/58 [00:00<00:04, 10.05it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.67 GB):  14%|█▍        | 8/58 [00:00<00:04, 10.05it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.66 GB):  14%|█▍        | 8/58 [00:00<00:04, 10.05it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.65 GB):  14%|█▍        | 8/58 [00:00<00:04, 10.05it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.65 GB):  19%|█▉        | 11/58 [00:00<00:03, 13.68it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.64 GB):  19%|█▉        | 11/58 [00:00<00:03, 13.68it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=118.64 GB):  19%|█▉        | 11/58 [00:00<00:03, 13.68it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.64 GB):  19%|█▉        | 11/58 [00:00<00:03, 13.68it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.64 GB):  24%|██▍       | 14/58 [00:00<00:02, 16.89it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.62 GB):  24%|██▍       | 14/58 [00:00<00:02, 16.89it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.62 GB):  24%|██▍       | 14/58 [00:01<00:02, 16.89it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.61 GB):  24%|██▍       | 14/58 [00:01<00:02, 16.89it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.61 GB):  24%|██▍       | 14/58 [00:01<00:02, 16.89it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=118.61 GB):  31%|███       | 18/58 [00:01<00:01, 21.06it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.60 GB):  31%|███       | 18/58 [00:01<00:01, 21.06it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.59 GB):  31%|███       | 18/58 [00:01<00:01, 21.06it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.55 GB):  31%|███       | 18/58 [00:01<00:01, 21.06it/s]Capturing num tokens (num_tokens=960 avail_mem=118.58 GB):  31%|███       | 18/58 [00:01<00:01, 21.06it/s] Capturing num tokens (num_tokens=960 avail_mem=118.58 GB):  38%|███▊      | 22/58 [00:01<00:01, 25.23it/s]Capturing num tokens (num_tokens=896 avail_mem=118.58 GB):  38%|███▊      | 22/58 [00:01<00:01, 25.23it/s]Capturing num tokens (num_tokens=832 avail_mem=118.57 GB):  38%|███▊      | 22/58 [00:01<00:01, 25.23it/s]Capturing num tokens (num_tokens=768 avail_mem=118.56 GB):  38%|███▊      | 22/58 [00:01<00:01, 25.23it/s]Capturing num tokens (num_tokens=704 avail_mem=118.56 GB):  38%|███▊      | 22/58 [00:01<00:01, 25.23it/s]

    Capturing num tokens (num_tokens=704 avail_mem=118.56 GB):  45%|████▍     | 26/58 [00:01<00:01, 28.71it/s]Capturing num tokens (num_tokens=640 avail_mem=118.55 GB):  45%|████▍     | 26/58 [00:01<00:01, 28.71it/s]Capturing num tokens (num_tokens=576 avail_mem=118.55 GB):  45%|████▍     | 26/58 [00:01<00:01, 28.71it/s]Capturing num tokens (num_tokens=512 avail_mem=118.52 GB):  45%|████▍     | 26/58 [00:01<00:01, 28.71it/s]Capturing num tokens (num_tokens=480 avail_mem=118.56 GB):  45%|████▍     | 26/58 [00:01<00:01, 28.71it/s]Capturing num tokens (num_tokens=480 avail_mem=118.56 GB):  52%|█████▏    | 30/58 [00:01<00:00, 31.28it/s]Capturing num tokens (num_tokens=448 avail_mem=118.55 GB):  52%|█████▏    | 30/58 [00:01<00:00, 31.28it/s]Capturing num tokens (num_tokens=416 avail_mem=118.55 GB):  52%|█████▏    | 30/58 [00:01<00:00, 31.28it/s]Capturing num tokens (num_tokens=384 avail_mem=118.54 GB):  52%|█████▏    | 30/58 [00:01<00:00, 31.28it/s]Capturing num tokens (num_tokens=352 avail_mem=118.53 GB):  52%|█████▏    | 30/58 [00:01<00:00, 31.28it/s]

    Capturing num tokens (num_tokens=352 avail_mem=118.53 GB):  59%|█████▊    | 34/58 [00:01<00:00, 32.92it/s]Capturing num tokens (num_tokens=320 avail_mem=118.53 GB):  59%|█████▊    | 34/58 [00:01<00:00, 32.92it/s]Capturing num tokens (num_tokens=288 avail_mem=118.52 GB):  59%|█████▊    | 34/58 [00:01<00:00, 32.92it/s]Capturing num tokens (num_tokens=256 avail_mem=118.51 GB):  59%|█████▊    | 34/58 [00:01<00:00, 32.92it/s]Capturing num tokens (num_tokens=240 avail_mem=118.51 GB):  59%|█████▊    | 34/58 [00:01<00:00, 32.92it/s]Capturing num tokens (num_tokens=240 avail_mem=118.51 GB):  66%|██████▌   | 38/58 [00:01<00:00, 34.32it/s]Capturing num tokens (num_tokens=224 avail_mem=118.50 GB):  66%|██████▌   | 38/58 [00:01<00:00, 34.32it/s]Capturing num tokens (num_tokens=208 avail_mem=118.50 GB):  66%|██████▌   | 38/58 [00:01<00:00, 34.32it/s]Capturing num tokens (num_tokens=192 avail_mem=118.49 GB):  66%|██████▌   | 38/58 [00:01<00:00, 34.32it/s]Capturing num tokens (num_tokens=176 avail_mem=118.48 GB):  66%|██████▌   | 38/58 [00:01<00:00, 34.32it/s]

    Capturing num tokens (num_tokens=176 avail_mem=118.48 GB):  72%|███████▏  | 42/58 [00:01<00:00, 35.00it/s]Capturing num tokens (num_tokens=160 avail_mem=118.47 GB):  72%|███████▏  | 42/58 [00:01<00:00, 35.00it/s]Capturing num tokens (num_tokens=144 avail_mem=118.47 GB):  72%|███████▏  | 42/58 [00:01<00:00, 35.00it/s]Capturing num tokens (num_tokens=128 avail_mem=118.44 GB):  72%|███████▏  | 42/58 [00:01<00:00, 35.00it/s]Capturing num tokens (num_tokens=112 avail_mem=118.44 GB):  72%|███████▏  | 42/58 [00:01<00:00, 35.00it/s]Capturing num tokens (num_tokens=112 avail_mem=118.44 GB):  79%|███████▉  | 46/58 [00:01<00:00, 36.22it/s]Capturing num tokens (num_tokens=96 avail_mem=118.43 GB):  79%|███████▉  | 46/58 [00:01<00:00, 36.22it/s] Capturing num tokens (num_tokens=80 avail_mem=118.42 GB):  79%|███████▉  | 46/58 [00:01<00:00, 36.22it/s]Capturing num tokens (num_tokens=64 avail_mem=118.42 GB):  79%|███████▉  | 46/58 [00:01<00:00, 36.22it/s]Capturing num tokens (num_tokens=48 avail_mem=118.43 GB):  79%|███████▉  | 46/58 [00:01<00:00, 36.22it/s]

    Capturing num tokens (num_tokens=48 avail_mem=118.43 GB):  86%|████████▌ | 50/58 [00:01<00:00, 36.57it/s]Capturing num tokens (num_tokens=32 avail_mem=118.42 GB):  86%|████████▌ | 50/58 [00:01<00:00, 36.57it/s]Capturing num tokens (num_tokens=28 avail_mem=118.41 GB):  86%|████████▌ | 50/58 [00:01<00:00, 36.57it/s]Capturing num tokens (num_tokens=24 avail_mem=118.41 GB):  86%|████████▌ | 50/58 [00:02<00:00, 36.57it/s]Capturing num tokens (num_tokens=20 avail_mem=118.40 GB):  86%|████████▌ | 50/58 [00:02<00:00, 36.57it/s]Capturing num tokens (num_tokens=20 avail_mem=118.40 GB):  93%|█████████▎| 54/58 [00:02<00:00, 37.49it/s]Capturing num tokens (num_tokens=16 avail_mem=118.40 GB):  93%|█████████▎| 54/58 [00:02<00:00, 37.49it/s]Capturing num tokens (num_tokens=12 avail_mem=118.39 GB):  93%|█████████▎| 54/58 [00:02<00:00, 37.49it/s]Capturing num tokens (num_tokens=8 avail_mem=118.39 GB):  93%|█████████▎| 54/58 [00:02<00:00, 37.49it/s] Capturing num tokens (num_tokens=4 avail_mem=118.38 GB):  93%|█████████▎| 54/58 [00:02<00:00, 37.49it/s]

    Capturing num tokens (num_tokens=4 avail_mem=118.38 GB): 100%|██████████| 58/58 [00:02<00:00, 37.47it/s]Capturing num tokens (num_tokens=4 avail_mem=118.38 GB): 100%|██████████| 58/58 [00:02<00:00, 26.81it/s]


### Non-streaming Synchronous Generation


```python
prompts = [
    "Hello, my name is",
    "The president of the United States is",
    "The capital of France is",
    "The future of AI is",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95}

outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print("===============================")
    print(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```

    ===============================
    Prompt: Hello, my name is
    Generated text:  Megan and I am a first year study abroad student at the University of South Carolina. I am majoring in Computer Science and minoring in Linguistics. I have lived in Brazil for the past 2 years and I have taken part in the "Universidade de Sao Paulo" program. I am a full-time student in the University of South Carolina. I am currently studying in the Computer Science and Linguistics program and am aiming to go to university in Spain in May of 2014. My background is in playing basketball and singing. I have a very deep interest in both sports and music. What I love most about computer
    ===============================
    Prompt: The president of the United States is
    Generated text:  getting a new desk and chair. He decides to get a desk that has a square bottom and a triangular top. He wants the bottom to be a square with a side length of 5 feet. The top of the desk will be a right triangle with one leg equal to the side length of the bottom square and the other leg equal to the height of the desk. What is the total area of the desk that will be covered by the desk and the chair?
    To determine the total area of the desk and the chair, we need to calculate the area of the square bottom and the area of the triangular top separately, and then sum these
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It is a historical center and the largest city in France. It is also the capital of the Île-de-France region, which includes most of Paris and many suburbs. It is located in the north of the country, at the northern end of the Mediterranean Sea and at the end of the Seine River, which flows through the city. The city is divided into 5 regions: 3 in the west, 1 in the north and 1 in the east.
    Paris is one of the most famous cities in the world. It is the city of love, of fashion, of science, of philosophy and of art
    ===============================
    Prompt: The future of AI is
    Generated text:  developing rapidly, and many companies are moving to develop new AI technologies. One of the most popular technologies is artificial intelligence (AI), which is an advanced form of machine learning. AI algorithms are trained on a large dataset to recognize patterns and make decisions. The use of AI in various industries has increased significantly in recent years, and it has the potential to revolutionize many aspects of our lives, from healthcare to transportation to education. However, there are also ethical concerns and limitations associated with the use of AI, such as bias, privacy, and transparency. As AI becomes more prevalent, it is important for individuals, organizations, and policymakers to consider


### Streaming Synchronous Generation


```python
prompts = [
    "Write a short, neutral self-introduction for a fictional character. Hello, my name is",
    "Provide a concise factual statement about France’s capital city. The capital of France is",
    "Explain possible future trends in artificial intelligence. The future of AI is",
]

sampling_params = {
    "temperature": 0.2,
    "top_p": 0.9,
}

print("\n=== Testing synchronous streaming generation with overlap removal ===\n")

for prompt in prompts:
    print(f"Prompt: {prompt}")
    merged_output = stream_and_merge(llm, prompt, sampling_params)
    print("Generated text:", merged_output)
    print()
```

    
    === Testing synchronous streaming generation with overlap removal ===
    
    Prompt: Write a short, neutral self-introduction for a fictional character. Hello, my name is


    Generated text:  [Name], and I'm a [Job Title] at [Company Name]. I'm excited to meet you and learn more about your interests and experiences. What can you tell me about yourself? I'm a [insert a few details about your personality or background]. And what's your favorite hobby or activity? I'm always looking for new experiences and adventures, so I'm always up for trying new things. What's your favorite book or movie? I love reading and watching movies, and I'm always on the lookout for new releases. What's your favorite place to go? I love to travel, so I'm always on the lookout
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also home to the French Parliament and the French National Museum of Modern Art. Paris is a cultural and historical center with a rich history dating back to the Roman Empire and the French Revolution. It is a popular tourist destination known for its romantic ambiance, fashion, and cuisine. Paris is also home to many world-renowned museums, including the Louvre, the Musée d'Orsay, and the Musée Rodin. The city is also known for its fashion industry, with many famous designers and bout
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased integration with human intelligence: As AI becomes more sophisticated, it is likely to become more integrated with human intelligence. This could lead to more complex and nuanced AI systems that can better understand and respond to human emotions and behaviors.
    
    2. Greater emphasis on ethical and social considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical and social considerations. This could lead to more responsible and accountable AI systems that are designed to minimize harm and maximize
    


### Non-streaming Asynchronous Generation


```python
prompts = [
    "Write a short, neutral self-introduction for a fictional character. Hello, my name is",
    "Provide a concise factual statement about France’s capital city. The capital of France is",
    "Explain possible future trends in artificial intelligence. The future of AI is",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95}

print("\n=== Testing asynchronous batch generation ===")


async def main():
    outputs = await llm.async_generate(prompts, sampling_params)

    for prompt, output in zip(prompts, outputs):
        print(f"\nPrompt: {prompt}")
        print(f"Generated text: {output['text']}")


asyncio.run(main())
```

    
    === Testing asynchronous batch generation ===


    
    Prompt: Write a short, neutral self-introduction for a fictional character. Hello, my name is
    Generated text:  [Your Name] and I'm a [career path] professional, primarily known for my [specific skill or experience that makes you unique]. I've been working in [industry] for [number] years, and I've always been driven by a passion for [why your career is important to you].
    
    I’m always looking for new challenges and opportunities to grow, and I'm constantly learning and adapting to stay ahead of the curve in my field. I'm a team player and have experience working in teams and collaborative environments. I'm also confident in my ability to communicate effectively and I believe that my skills can be put to use in a
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known for its iconic Eiffel Tower, distinctive gardens, and lively French culture. The city is also famous for its well-preserved medieval architecture, including the Louvre Museum and Notre-Dame Cathedral, which dates back to the 11th century. Despite facing challenges from urban sprawl and climate change, Paris continues to be a major economic and cultural hub of the French Republic. Additionally, the city has a rich history dating back to the 6th century and is home to numerous museums, palaces, and landmarks. The French government has invested heavily in public transportation to improve accessibility and reduce traffic congestion in the city
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to involve several key trends that will shape the technology's development and impact on society:
    
    1. AI will become more pervasive: As AI continues to advance, we can expect to see more and more AI technologies integrated into everyday life. This will likely lead to more widespread adoption of AI, from smart homes to self-driving cars to personalized medicine.
    
    2. AI will become more capable: AI is expected to become increasingly capable, with the ability to process and analyze large amounts of data much faster and more accurately than ever before. This will allow for more complex and intelligent decision-making in a range of domains, from healthcare to finance to transportation.
    
    


### Streaming Asynchronous Generation


```python
prompts = [
    "Write a short, neutral self-introduction for a fictional character. Hello, my name is",
    "Provide a concise factual statement about France’s capital city. The capital of France is",
    "Explain possible future trends in artificial intelligence. The future of AI is",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95}

print("\n=== Testing asynchronous streaming generation (no repeats) ===")


async def main():
    for prompt in prompts:
        print(f"\nPrompt: {prompt}")
        print("Generated text: ", end="", flush=True)

        # Replace direct calls to async_generate with our custom overlap-aware version
        async for cleaned_chunk in async_stream_and_merge(llm, prompt, sampling_params):
            print(cleaned_chunk, end="", flush=True)

        print()  # New line after each prompt


asyncio.run(main())
```

    
    === Testing asynchronous streaming generation (no repeats) ===
    
    Prompt: Write a short, neutral self-introduction for a fictional character. Hello, my name is
    Generated text: 

     [

    Your

     Name

    ]

     and

     I

     am

     an

     experienced

     [

    Skill

    /

    Status

    ],

     skilled

     in

     [

    Skill

    /

    Status

    ],

     and

     have

     a

     track

     record

     of

     [

    Skill

    /

    Status

    ]

     for

     the

     last

     [

    X

    ]

     years

    .

     I

    'm

     currently

     working

     as

     a

     [

    Role

    /

    Position

    ],

     and

     I

    'm

     always

     [

    Performance

    ].

     I

    'm

     passionate

     about

     [

    Purpose

    /

    Interest

    /

    Ad

    vent

    urous

    ness

    ],

     and

     I

    'm

     always

     looking

     for

     new

     challenges

     and

     opportunities

     to

     grow

     and

     improve

     my

     skills

    .

     Whether

     it

    's

     working

     with

     [

    Customers

    /

    Customers

    ],

     [

    Teams

    /

    Teams

    ],

     or

     [

    Topics

    /

    Topics

    ],

     I

    'm

     always

     eager

     to

     learn

     and

     adapt

     to

     new

     situations

    .

     I

     enjoy

     collaborating

     with

     others

     and

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    Fill

     in

     the

     blanks

    :

     The

     capital

     of

     France

     is

     located

     in

     the

     city

     of

     Paris

    ,

     France

    .

     The

     city

     is

     known

     for

     its

     rich

     history

    ,

     iconic

     landmarks

    ,

     and

     vibrant

     culture

    .

     Paris

     is

     a

     cosm

    opolitan

     met

    ropolis

     with

     a

     diverse

     population

    ,

     including

     many

     ethnic

     groups

     and

     individuals

     with

     various

     religious

     beliefs

    .

     The

     city

     is

     home

     to

     numerous

     museums

    ,

     galleries

    ,

     and

     art

     galleries

    ,

     as

     well

     as

     numerous

     historic

     sites

     and

     landmarks

     such

     as

     the

     E

    iff

    el

     Tower

     and

     the

     Lou

    vre

     Museum

    .

     Paris

     is

     also

     famous

     for

     its

     fashion

     industry

    ,

     with

     numerous

     designers

     and

     bout

    iques

    .

     The

     city

    's

     cuisine

    ,

     including

     French

     cuisine

    ,

     also

     plays

     a

     crucial

     role

     in

     its

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     not

     yet

     fully

     understood

    ,

     but

     there

     are

     several

     trends

     that

     are

     likely

     to

     shape

     the

     development

     of

     this

     technology

     in

     the

     coming

     years

    .

     Here

     are

     some

     possible

     future

     trends

     in

     AI

    :
    


    1

    .

     Enhanced

     Machine

     Learning

    :

     As

     AI

     systems

     become

     more

     capable

     of

     processing

     complex

     data

    ,

     they

     will

     be

     able

     to

     learn

     and

     improve

     on

     their

     own

    .

     This

     will

     lead

     to

     more

     accurate

     and

     personalized

     predictions

     and

     decisions

    .
    


    2

    .

     Increased

     Integration

     with

     Human

     Intelligence

    :

     AI

     will

     likely

     become

     even

     more

     integrated

     with

     human

     intelligence

    ,

     providing

     better

     understanding

     of

     human

     behavior

     and

     emotions

    .

     This

     will

     enable

     AI

     systems

     to

     better

     communicate

     with

     and

     understand

     humans

    .
    


    3

    .

     Improved

     Rob

    ust

    ness

    :

     AI

     systems

     will

     become

     more

    



```python
llm.shutdown()
```
