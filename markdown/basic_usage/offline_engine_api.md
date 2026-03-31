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


    2026-03-31 04:02:21.994 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 04:02:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 04:02:21.994 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 04:02:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 04:02:21.994 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 04:02:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 04:02:21.994 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 04:02:21] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 04:02:21.994 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 04:02:21] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.72it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.71it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:17,  2.41s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:17,  2.41s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:17,  2.41s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:17,  2.41s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:26,  2.05it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:26,  2.05it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:26,  2.05it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:26,  2.05it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:26,  2.05it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:26,  2.05it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:26,  2.05it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.24it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 13.08it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 13.08it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:03, 13.08it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:03, 13.08it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:03, 13.08it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:03, 13.08it/s]

    Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:03, 13.08it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:03, 13.08it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 19.41it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 19.41it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 19.41it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 19.41it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 19.41it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 19.41it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 19.41it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:03<00:01, 23.51it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:03<00:01, 23.51it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:03<00:01, 23.51it/s]

    Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:03<00:01, 23.51it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:03<00:01, 23.51it/s]Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:03<00:01, 23.51it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:03<00:00, 27.37it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:03<00:00, 27.37it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:03<00:00, 27.37it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:03<00:00, 27.37it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:03<00:00, 27.37it/s]Compiling num tokens (num_tokens=192):  62%|██████▏   | 36/58 [00:03<00:00, 27.37it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:03<00:00, 30.35it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:03<00:00, 30.35it/s]

    Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:03<00:00, 30.35it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:03<00:00, 30.35it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:03<00:00, 30.35it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:03<00:00, 30.35it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:03<00:00, 32.43it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:03<00:00, 32.43it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:03<00:00, 32.43it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:03<00:00, 32.43it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:03<00:00, 32.43it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:03<00:00, 32.43it/s]

    Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 35.32it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 35.32it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 35.32it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 35.32it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 35.32it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 35.32it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 35.32it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 35.32it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.19it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=134.91 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=134.88 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=134.88 GB):   3%|▎         | 2/58 [00:00<00:03, 14.81it/s]Capturing num tokens (num_tokens=7168 avail_mem=134.85 GB):   3%|▎         | 2/58 [00:00<00:03, 14.81it/s]Capturing num tokens (num_tokens=6656 avail_mem=134.87 GB):   3%|▎         | 2/58 [00:00<00:03, 14.81it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=134.87 GB):   7%|▋         | 4/58 [00:00<00:04, 11.07it/s]Capturing num tokens (num_tokens=6144 avail_mem=134.86 GB):   7%|▋         | 4/58 [00:00<00:04, 11.07it/s]Capturing num tokens (num_tokens=5632 avail_mem=134.83 GB):   7%|▋         | 4/58 [00:00<00:04, 11.07it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=134.83 GB):  10%|█         | 6/58 [00:00<00:05,  9.63it/s]Capturing num tokens (num_tokens=5120 avail_mem=134.85 GB):  10%|█         | 6/58 [00:00<00:05,  9.63it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=134.84 GB):  10%|█         | 6/58 [00:00<00:05,  9.63it/s]Capturing num tokens (num_tokens=4608 avail_mem=134.84 GB):  14%|█▍        | 8/58 [00:00<00:05,  8.50it/s]Capturing num tokens (num_tokens=4096 avail_mem=134.83 GB):  14%|█▍        | 8/58 [00:00<00:05,  8.50it/s]Capturing num tokens (num_tokens=3840 avail_mem=134.82 GB):  14%|█▍        | 8/58 [00:00<00:05,  8.50it/s]Capturing num tokens (num_tokens=3584 avail_mem=134.81 GB):  14%|█▍        | 8/58 [00:00<00:05,  8.50it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=134.81 GB):  19%|█▉        | 11/58 [00:01<00:03, 11.82it/s]Capturing num tokens (num_tokens=3328 avail_mem=134.81 GB):  19%|█▉        | 11/58 [00:01<00:03, 11.82it/s]Capturing num tokens (num_tokens=3072 avail_mem=134.80 GB):  19%|█▉        | 11/58 [00:01<00:03, 11.82it/s]Capturing num tokens (num_tokens=2816 avail_mem=134.80 GB):  19%|█▉        | 11/58 [00:01<00:03, 11.82it/s]Capturing num tokens (num_tokens=2816 avail_mem=134.80 GB):  24%|██▍       | 14/58 [00:01<00:02, 15.20it/s]Capturing num tokens (num_tokens=2560 avail_mem=134.79 GB):  24%|██▍       | 14/58 [00:01<00:02, 15.20it/s]Capturing num tokens (num_tokens=2304 avail_mem=134.78 GB):  24%|██▍       | 14/58 [00:01<00:02, 15.20it/s]Capturing num tokens (num_tokens=2048 avail_mem=134.78 GB):  24%|██▍       | 14/58 [00:01<00:02, 15.20it/s]Capturing num tokens (num_tokens=1792 avail_mem=134.78 GB):  24%|██▍       | 14/58 [00:01<00:02, 15.20it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=134.78 GB):  24%|██▍       | 14/58 [00:01<00:02, 15.20it/s]Capturing num tokens (num_tokens=1536 avail_mem=134.78 GB):  33%|███▎      | 19/58 [00:01<00:01, 21.57it/s]Capturing num tokens (num_tokens=1280 avail_mem=134.77 GB):  33%|███▎      | 19/58 [00:01<00:01, 21.57it/s]Capturing num tokens (num_tokens=1024 avail_mem=134.75 GB):  33%|███▎      | 19/58 [00:01<00:01, 21.57it/s]Capturing num tokens (num_tokens=960 avail_mem=134.76 GB):  33%|███▎      | 19/58 [00:01<00:01, 21.57it/s] Capturing num tokens (num_tokens=960 avail_mem=134.76 GB):  38%|███▊      | 22/58 [00:01<00:01, 21.65it/s]Capturing num tokens (num_tokens=896 avail_mem=134.74 GB):  38%|███▊      | 22/58 [00:01<00:01, 21.65it/s]Capturing num tokens (num_tokens=832 avail_mem=134.74 GB):  38%|███▊      | 22/58 [00:01<00:01, 21.65it/s]

    Capturing num tokens (num_tokens=768 avail_mem=134.73 GB):  38%|███▊      | 22/58 [00:01<00:01, 21.65it/s]Capturing num tokens (num_tokens=768 avail_mem=134.73 GB):  43%|████▎     | 25/58 [00:01<00:01, 22.58it/s]Capturing num tokens (num_tokens=704 avail_mem=134.73 GB):  43%|████▎     | 25/58 [00:01<00:01, 22.58it/s]Capturing num tokens (num_tokens=640 avail_mem=134.73 GB):  43%|████▎     | 25/58 [00:01<00:01, 22.58it/s]Capturing num tokens (num_tokens=576 avail_mem=134.72 GB):  43%|████▎     | 25/58 [00:01<00:01, 22.58it/s]Capturing num tokens (num_tokens=576 avail_mem=134.72 GB):  48%|████▊     | 28/58 [00:01<00:01, 23.06it/s]Capturing num tokens (num_tokens=512 avail_mem=134.22 GB):  48%|████▊     | 28/58 [00:01<00:01, 23.06it/s]Capturing num tokens (num_tokens=480 avail_mem=134.24 GB):  48%|████▊     | 28/58 [00:01<00:01, 23.06it/s]

    Capturing num tokens (num_tokens=448 avail_mem=134.15 GB):  48%|████▊     | 28/58 [00:01<00:01, 23.06it/s]Capturing num tokens (num_tokens=416 avail_mem=134.08 GB):  48%|████▊     | 28/58 [00:01<00:01, 23.06it/s]Capturing num tokens (num_tokens=384 avail_mem=134.07 GB):  48%|████▊     | 28/58 [00:01<00:01, 23.06it/s]Capturing num tokens (num_tokens=384 avail_mem=134.07 GB):  57%|█████▋    | 33/58 [00:01<00:00, 28.91it/s]Capturing num tokens (num_tokens=352 avail_mem=134.07 GB):  57%|█████▋    | 33/58 [00:01<00:00, 28.91it/s]Capturing num tokens (num_tokens=320 avail_mem=134.06 GB):  57%|█████▋    | 33/58 [00:01<00:00, 28.91it/s]Capturing num tokens (num_tokens=288 avail_mem=134.06 GB):  57%|█████▋    | 33/58 [00:01<00:00, 28.91it/s]Capturing num tokens (num_tokens=256 avail_mem=134.06 GB):  57%|█████▋    | 33/58 [00:01<00:00, 28.91it/s]Capturing num tokens (num_tokens=240 avail_mem=132.98 GB):  57%|█████▋    | 33/58 [00:01<00:00, 28.91it/s]Capturing num tokens (num_tokens=240 avail_mem=132.98 GB):  66%|██████▌   | 38/58 [00:01<00:00, 33.10it/s]Capturing num tokens (num_tokens=224 avail_mem=132.98 GB):  66%|██████▌   | 38/58 [00:01<00:00, 33.10it/s]

    Capturing num tokens (num_tokens=208 avail_mem=128.97 GB):  66%|██████▌   | 38/58 [00:01<00:00, 33.10it/s]Capturing num tokens (num_tokens=192 avail_mem=119.70 GB):  66%|██████▌   | 38/58 [00:01<00:00, 33.10it/s]Capturing num tokens (num_tokens=176 avail_mem=118.78 GB):  66%|██████▌   | 38/58 [00:01<00:00, 33.10it/s]Capturing num tokens (num_tokens=176 avail_mem=118.78 GB):  72%|███████▏  | 42/58 [00:01<00:00, 34.28it/s]Capturing num tokens (num_tokens=160 avail_mem=118.35 GB):  72%|███████▏  | 42/58 [00:01<00:00, 34.28it/s]Capturing num tokens (num_tokens=144 avail_mem=118.34 GB):  72%|███████▏  | 42/58 [00:01<00:00, 34.28it/s]Capturing num tokens (num_tokens=128 avail_mem=118.34 GB):  72%|███████▏  | 42/58 [00:02<00:00, 34.28it/s]Capturing num tokens (num_tokens=112 avail_mem=118.34 GB):  72%|███████▏  | 42/58 [00:02<00:00, 34.28it/s]Capturing num tokens (num_tokens=96 avail_mem=118.33 GB):  72%|███████▏  | 42/58 [00:02<00:00, 34.28it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=118.33 GB):  81%|████████  | 47/58 [00:02<00:00, 36.41it/s]Capturing num tokens (num_tokens=80 avail_mem=118.33 GB):  81%|████████  | 47/58 [00:02<00:00, 36.41it/s]Capturing num tokens (num_tokens=64 avail_mem=118.33 GB):  81%|████████  | 47/58 [00:02<00:00, 36.41it/s]Capturing num tokens (num_tokens=48 avail_mem=118.32 GB):  81%|████████  | 47/58 [00:02<00:00, 36.41it/s]Capturing num tokens (num_tokens=32 avail_mem=118.32 GB):  81%|████████  | 47/58 [00:02<00:00, 36.41it/s]Capturing num tokens (num_tokens=28 avail_mem=118.31 GB):  81%|████████  | 47/58 [00:02<00:00, 36.41it/s]Capturing num tokens (num_tokens=28 avail_mem=118.31 GB):  90%|████████▉ | 52/58 [00:02<00:00, 38.66it/s]Capturing num tokens (num_tokens=24 avail_mem=118.31 GB):  90%|████████▉ | 52/58 [00:02<00:00, 38.66it/s]Capturing num tokens (num_tokens=20 avail_mem=118.31 GB):  90%|████████▉ | 52/58 [00:02<00:00, 38.66it/s]Capturing num tokens (num_tokens=16 avail_mem=118.31 GB):  90%|████████▉ | 52/58 [00:02<00:00, 38.66it/s]Capturing num tokens (num_tokens=12 avail_mem=118.30 GB):  90%|████████▉ | 52/58 [00:02<00:00, 38.66it/s]

    Capturing num tokens (num_tokens=8 avail_mem=118.30 GB):  90%|████████▉ | 52/58 [00:02<00:00, 38.66it/s] Capturing num tokens (num_tokens=8 avail_mem=118.30 GB):  98%|█████████▊| 57/58 [00:02<00:00, 40.29it/s]Capturing num tokens (num_tokens=4 avail_mem=118.30 GB):  98%|█████████▊| 57/58 [00:02<00:00, 40.29it/s]Capturing num tokens (num_tokens=4 avail_mem=118.30 GB): 100%|██████████| 58/58 [00:02<00:00, 24.97it/s]


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
    Generated text:  Richard. I am a famous blogger, a cartoonist, a gaming junkie, and an engineer. But I'm also a native Canadian, who loves to travel and learn new things about the world. I'm an avid social media influencer and a true geek out of love for technology, gaming, and science. I love being on the go, exploring new places, trying new foods, and making friends from all over the world. I'm passionate about learning and sharing new information with others, and I'm always looking for new ways to engage with the world.
    As a result of all this, I have found myself to be a
    ===============================
    Prompt: The president of the United States is
    Generated text:  a person who is either the head of the executive branch of the government or the head of the legislative branch of the government, and the vice president of the United States is a person who is either the head of the executive branch or the head of the legislative branch of the government. If two people are chosen at random to be the vice president and the president of the United States, what is the probability that both will be vice presidents?
    Answer Choices: (A) 1/12 (B) 1/6 (C) 1/3 (D) 1/2 (E) 1
    To solve this
    ===============================
    Prompt: The capital of France is
    Generated text:  located on the banks of the River Seine, near the city of Paris, on the left bank of the Seine. The river is the longest in Europe and its greatest volume is used for the water transport of the country.
    The Seine is more than a mile long and is the longest river in the French Republic. It is also the longest river in Europe, following only the Rhine.
    The origin of the Seine can be traced back to the Bronze Age and the ancient Greeks built the first river embankments in the area. In 431 BC, after the Battle of the Pyrenees, the Gauls
    ===============================
    Prompt: The future of AI is
    Generated text:  uncertain and unexplored. According to the latest surveys, AI is expected to impact nearly every aspect of modern life, with widespread use expected by 2030. In the wake of this exciting development, it is crucial to understand the landscape of AI and its future direction. In this article, we will explore the latest developments in AI and the potential impact on various sectors of society.
    The rapid advancement of AI in recent years has already transformed the way we live and work. However, it is important to note that AI is not a panacea; it is a tool that can be used to solve problems and improve efficiency. While


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a short description of your job or profession]. I enjoy [insert a short description of your hobbies or interests]. I'm always looking for new experiences and learning opportunities. What do you like to do for fun? I enjoy [insert a short description of your hobbies or interests]. I'm always looking for new experiences and learning opportunities. What do you like to do for fun? I enjoy [insert a short description of your hobbies or interests
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as "La Ville-Marie" and "La Ville-Est". It is the largest city in France and the second-largest city in the European Union, with a population of over 2.7 million people. Paris is known for its rich history, art, and culture, and is a major tourist destination. The city is home to many famous landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also home to many important institutions such as the French Academy of Sciences and the French National Library. Paris is a city of contrasts, with its modern architecture and historical landmarks
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some of the potential future trends in AI:
    
    1. Increased automation and robotics: As AI technology continues to advance, we can expect to see more automation and robotics in various industries, such as manufacturing, transportation, and healthcare. This will lead to increased efficiency, productivity, and cost savings for businesses.
    
    2. Enhanced personalization: AI will enable businesses to better understand their customers' needs and preferences, leading to more personalized experiences. This will result in increased customer satisfaction and loyalty, as well as better
    


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
    Generated text:  [Character Name] and I am an experienced [job title or occupation] with over [number of years of experience] years of experience in [specific field or area of expertise]. I have a strong work ethic and I am always looking for ways to [mention an accomplishment or achievement in the field]. I am patient, responsible, and can work well under pressure. I am comfortable in my role and enjoy [mention a hobby or activity that interests me]. I am always looking for ways to improve my skills and continue learning. I am a natural leader, and I thrive in a fast-paced environment. I enjoy [mention an activity or hobby
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    That's correct! Paris is the capital city of France, known for its iconic landmarks such as the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. The city is also famous for its rich history, including several cultural institutions and landmarks. Paris is a cosmopolitan city with a diverse population and numerous international events. Its status as a global cultural and economic center has made it a major player in international affairs. 
    
    So, to sum it up: Paris is the capital of France and one of the world's most important cities. It's home to some of the world's most iconic landmarks and is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to continue to evolve rapidly, with numerous potential directions and opportunities for growth. Here are some potential future trends in AI:
    
    1. Increased Robustness and Efficiency: As AI becomes more integrated into our daily lives, the demand for faster and more efficient AI systems is growing. This could lead to the development of even more powerful and versatile AI systems that can perform a wider range of tasks with greater accuracy and speed.
    
    2. Integration of Human AI: AI systems are becoming increasingly integrated into the human workforce, with more tasks being done by machines than by humans. This could lead to more complex and sophisticated AI systems that are capable of performing


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

    Name

    ],

     and

     I

     am

     a

     [

    Job

     Title

    ]

     with

     [

    Job

     Title

    ]

     certification

    .

     I

     have

     [

    Number

     of

     Years

     in

     the

     Industry

    ]

     years

     of

     experience

     in

     [

    Title

    ],

     and

     I

     have

     a

     passion

     for

     [

    Title

    ]

     and

     love

     to

     [

    Title

    ]

     experiences

    .

     I

     am

     a

     reliable

    ,

     organized

    ,

     and

     dedicated

     professional

     who

     is

     always

     striving

     to

     improve

     my

     skills

     and

     knowledge

    .

     I

     love

     to

     learn

     and

     I

     am

     always

     looking

     for

     new

     opportunities

     to

     grow

     and

     advance

     in

     my

     career

    .

     My

     strongest

     skill

     is

     my

     ability

     to

     work

     independently

     and

     problem

    -s

    olve

     to

     find

     the

     best

     solution

    .

     I

     am

     an

     active

     member

     of

     my

     community

     and

     I

     am

     always

     willing

     to

     lend

     a

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    Paris

     is

     the

     capital

     city

     of

     the

     Republic

     of

     France

    ,

     located

     in

     the

     central

     region

     of

     the

     country

    ,

     on

     the

     left

     bank

     of

     the

     River

     Se

    ine

    .

     It

     is

     the

     largest

     city

     in

     both

     the

     country

     and

     the

     world

    ,

     and

     the

     second

     most

     populous

     city

     in

     the

     world

    ,

     behind

     New

     York

     City

    .

     The

     city

     is

     an

     important

     cultural

    ,

     economic

     and

     political

     center

     in

     Europe

     and

     beyond

    ,

     and

     is

     known

     for

     its

     distinctive

     architecture

    ,

     museums

    ,

     and

     unique

     culinary

     traditions

    .

     Paris

     is

     a

     popular

     destination

     for

     tourism

    ,

     with

     millions

     of

     visitors

     annually

    ,

     and

     it

     is

     also

     considered

     a

     cultural

     and

     intellectual

     hub

     in

     the

     world

    .

     The

     city

     is

     often

     referred

     to

     as

     the

     "

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     very

     promising

     and

     we

     are

     seeing

     a

     number

     of

     potential

     areas

     where

     it

     will

     continue

     to

     evolve

    .

     Here

     are

     some

     of

     the

     key

     trends

     that

     are

     likely

     to

     shape

     the

     future

     of

     AI

    :
    


    1

    .

     Increased

     efficiency

     and

     productivity

    :

     AI

     is

     becoming

     more

     efficient

     and

     productive

     by

     autom

    ating

     tasks

    ,

     optimizing

     processes

    ,

     and

     improving

     decision

    -making

    .

     This

     is

     likely

     to

     lead

     to

     increased

     efficiency

     and

     productivity

     in

     various

     industries

    ,

     from

     manufacturing

     to

     healthcare

    .
    


    2

    .

     Personal

    ization

     and

    定制

    ：

    AI

     is

     capable

     of

     learning

     from

     data

     and

     providing

     personalized

     recommendations

     to

     users

    .

     This

     can

     lead

     to

     increased

     user

     satisfaction

     and

     loyalty

    ,

     as

     well

     as

     more

     efficient

     and

     effective

     use

     of

     resources

    .
    


    3

    .

     Autonomous

     vehicles

    :

    



```python
llm.shutdown()
```
