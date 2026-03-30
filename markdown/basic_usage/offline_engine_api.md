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


    2026-03-30 11:50:14.050 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 11:50:14] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 11:50:14.050 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 11:50:14] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 11:50:14.050 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 11:50:14] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 11:50:14.050 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 11:50:14] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 11:50:14.050 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 11:50:14] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.69it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.69it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:08,  2.25s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:08,  2.25s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:08,  2.25s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:08,  2.25s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:24,  2.20it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:24,  2.20it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:24,  2.20it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:24,  2.20it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:24,  2.20it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:24,  2.20it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:24,  2.20it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 14.77it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 14.77it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 14.77it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 14.77it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 14.77it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 14.77it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 14.77it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 14.77it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 21.57it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 21.57it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 21.57it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 21.57it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 21.57it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 21.57it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 21.57it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:02<00:01, 21.57it/s]Compiling num tokens (num_tokens=352):  45%|████▍     | 26/58 [00:02<00:01, 21.57it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:02<00:00, 29.83it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:02<00:00, 29.83it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:02<00:00, 29.83it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:02<00:00, 29.83it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:02<00:00, 29.83it/s]

    Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:02<00:00, 29.83it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:02<00:00, 29.83it/s]Compiling num tokens (num_tokens=192):  59%|█████▊    | 34/58 [00:02<00:00, 29.83it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:02<00:00, 36.42it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:02<00:00, 36.42it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:02<00:00, 36.42it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:02<00:00, 36.42it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:02<00:00, 36.42it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:02<00:00, 36.42it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:02<00:00, 36.42it/s] Compiling num tokens (num_tokens=80):  71%|███████   | 41/58 [00:02<00:00, 36.42it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:03<00:00, 42.55it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:03<00:00, 42.55it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:03<00:00, 42.55it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:03<00:00, 42.55it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:03<00:00, 42.55it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:03<00:00, 42.55it/s]

    Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:03<00:00, 42.55it/s]Compiling num tokens (num_tokens=16):  83%|████████▎ | 48/58 [00:03<00:00, 42.55it/s]Compiling num tokens (num_tokens=12):  83%|████████▎ | 48/58 [00:03<00:00, 42.55it/s]Compiling num tokens (num_tokens=8):  83%|████████▎ | 48/58 [00:03<00:00, 42.55it/s] Compiling num tokens (num_tokens=4):  83%|████████▎ | 48/58 [00:03<00:00, 42.55it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 54.49it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 18.63it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=60.50 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=60.48 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=60.48 GB):   3%|▎         | 2/58 [00:00<00:02, 19.17it/s]Capturing num tokens (num_tokens=7168 avail_mem=60.47 GB):   3%|▎         | 2/58 [00:00<00:02, 19.17it/s]Capturing num tokens (num_tokens=6656 avail_mem=60.47 GB):   3%|▎         | 2/58 [00:00<00:02, 19.17it/s]Capturing num tokens (num_tokens=6144 avail_mem=60.47 GB):   3%|▎         | 2/58 [00:00<00:02, 19.17it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=60.47 GB):   9%|▊         | 5/58 [00:00<00:02, 22.44it/s]Capturing num tokens (num_tokens=5632 avail_mem=60.47 GB):   9%|▊         | 5/58 [00:00<00:02, 22.44it/s]Capturing num tokens (num_tokens=5120 avail_mem=60.47 GB):   9%|▊         | 5/58 [00:00<00:02, 22.44it/s]Capturing num tokens (num_tokens=4608 avail_mem=60.46 GB):   9%|▊         | 5/58 [00:00<00:02, 22.44it/s]Capturing num tokens (num_tokens=4096 avail_mem=60.46 GB):   9%|▊         | 5/58 [00:00<00:02, 22.44it/s]Capturing num tokens (num_tokens=4096 avail_mem=60.46 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.98it/s]Capturing num tokens (num_tokens=3840 avail_mem=60.46 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.98it/s]Capturing num tokens (num_tokens=3584 avail_mem=60.45 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.98it/s]Capturing num tokens (num_tokens=3328 avail_mem=60.45 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.98it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=60.45 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.98it/s]Capturing num tokens (num_tokens=2816 avail_mem=60.45 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.98it/s]Capturing num tokens (num_tokens=2816 avail_mem=60.45 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.72it/s]Capturing num tokens (num_tokens=2560 avail_mem=60.44 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.72it/s]Capturing num tokens (num_tokens=2304 avail_mem=60.44 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.72it/s]Capturing num tokens (num_tokens=2048 avail_mem=60.44 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.72it/s]Capturing num tokens (num_tokens=1792 avail_mem=60.43 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.72it/s]Capturing num tokens (num_tokens=1536 avail_mem=60.43 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.72it/s]Capturing num tokens (num_tokens=1280 avail_mem=60.42 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.72it/s]Capturing num tokens (num_tokens=1280 avail_mem=60.42 GB):  34%|███▍      | 20/58 [00:00<00:00, 39.18it/s]Capturing num tokens (num_tokens=1024 avail_mem=60.41 GB):  34%|███▍      | 20/58 [00:00<00:00, 39.18it/s]Capturing num tokens (num_tokens=960 avail_mem=60.42 GB):  34%|███▍      | 20/58 [00:00<00:00, 39.18it/s] Capturing num tokens (num_tokens=896 avail_mem=60.42 GB):  34%|███▍      | 20/58 [00:00<00:00, 39.18it/s]

    Capturing num tokens (num_tokens=832 avail_mem=60.41 GB):  34%|███▍      | 20/58 [00:00<00:00, 39.18it/s]Capturing num tokens (num_tokens=768 avail_mem=60.41 GB):  34%|███▍      | 20/58 [00:00<00:00, 39.18it/s]Capturing num tokens (num_tokens=704 avail_mem=60.41 GB):  34%|███▍      | 20/58 [00:00<00:00, 39.18it/s]Capturing num tokens (num_tokens=704 avail_mem=60.41 GB):  45%|████▍     | 26/58 [00:00<00:00, 43.73it/s]Capturing num tokens (num_tokens=640 avail_mem=60.40 GB):  45%|████▍     | 26/58 [00:00<00:00, 43.73it/s]Capturing num tokens (num_tokens=576 avail_mem=60.40 GB):  45%|████▍     | 26/58 [00:00<00:00, 43.73it/s]Capturing num tokens (num_tokens=512 avail_mem=60.39 GB):  45%|████▍     | 26/58 [00:00<00:00, 43.73it/s]Capturing num tokens (num_tokens=480 avail_mem=60.41 GB):  45%|████▍     | 26/58 [00:00<00:00, 43.73it/s]Capturing num tokens (num_tokens=448 avail_mem=60.41 GB):  45%|████▍     | 26/58 [00:00<00:00, 43.73it/s]Capturing num tokens (num_tokens=448 avail_mem=60.41 GB):  53%|█████▎    | 31/58 [00:00<00:00, 41.17it/s]Capturing num tokens (num_tokens=416 avail_mem=59.25 GB):  53%|█████▎    | 31/58 [00:00<00:00, 41.17it/s]

    Capturing num tokens (num_tokens=384 avail_mem=59.25 GB):  53%|█████▎    | 31/58 [00:00<00:00, 41.17it/s]Capturing num tokens (num_tokens=352 avail_mem=59.24 GB):  53%|█████▎    | 31/58 [00:01<00:00, 41.17it/s]Capturing num tokens (num_tokens=320 avail_mem=59.24 GB):  53%|█████▎    | 31/58 [00:01<00:00, 41.17it/s]

    Capturing num tokens (num_tokens=288 avail_mem=59.23 GB):  53%|█████▎    | 31/58 [00:01<00:00, 41.17it/s]Capturing num tokens (num_tokens=288 avail_mem=59.23 GB):  62%|██████▏   | 36/58 [00:01<00:00, 23.47it/s]Capturing num tokens (num_tokens=256 avail_mem=59.23 GB):  62%|██████▏   | 36/58 [00:01<00:00, 23.47it/s]Capturing num tokens (num_tokens=240 avail_mem=59.23 GB):  62%|██████▏   | 36/58 [00:01<00:00, 23.47it/s]

    Capturing num tokens (num_tokens=224 avail_mem=59.22 GB):  62%|██████▏   | 36/58 [00:01<00:00, 23.47it/s]Capturing num tokens (num_tokens=208 avail_mem=59.22 GB):  62%|██████▏   | 36/58 [00:01<00:00, 23.47it/s]Capturing num tokens (num_tokens=208 avail_mem=59.22 GB):  69%|██████▉   | 40/58 [00:01<00:00, 19.39it/s]Capturing num tokens (num_tokens=192 avail_mem=59.22 GB):  69%|██████▉   | 40/58 [00:01<00:00, 19.39it/s]

    Capturing num tokens (num_tokens=176 avail_mem=59.22 GB):  69%|██████▉   | 40/58 [00:01<00:00, 19.39it/s]Capturing num tokens (num_tokens=160 avail_mem=59.21 GB):  69%|██████▉   | 40/58 [00:01<00:00, 19.39it/s]

    Capturing num tokens (num_tokens=160 avail_mem=59.21 GB):  74%|███████▍  | 43/58 [00:01<00:00, 15.26it/s]Capturing num tokens (num_tokens=144 avail_mem=59.21 GB):  74%|███████▍  | 43/58 [00:01<00:00, 15.26it/s]Capturing num tokens (num_tokens=128 avail_mem=59.21 GB):  74%|███████▍  | 43/58 [00:02<00:00, 15.26it/s]

    Capturing num tokens (num_tokens=112 avail_mem=59.20 GB):  74%|███████▍  | 43/58 [00:02<00:00, 15.26it/s]Capturing num tokens (num_tokens=112 avail_mem=59.20 GB):  79%|███████▉  | 46/58 [00:02<00:00, 13.52it/s]Capturing num tokens (num_tokens=96 avail_mem=59.20 GB):  79%|███████▉  | 46/58 [00:02<00:00, 13.52it/s] Capturing num tokens (num_tokens=80 avail_mem=59.20 GB):  79%|███████▉  | 46/58 [00:02<00:00, 13.52it/s]

    Capturing num tokens (num_tokens=80 avail_mem=59.20 GB):  83%|████████▎ | 48/58 [00:02<00:00, 13.33it/s]Capturing num tokens (num_tokens=64 avail_mem=59.19 GB):  83%|████████▎ | 48/58 [00:02<00:00, 13.33it/s]Capturing num tokens (num_tokens=48 avail_mem=59.19 GB):  83%|████████▎ | 48/58 [00:02<00:00, 13.33it/s]Capturing num tokens (num_tokens=48 avail_mem=59.19 GB):  86%|████████▌ | 50/58 [00:02<00:00, 13.21it/s]Capturing num tokens (num_tokens=32 avail_mem=59.19 GB):  86%|████████▌ | 50/58 [00:02<00:00, 13.21it/s]

    Capturing num tokens (num_tokens=28 avail_mem=59.18 GB):  86%|████████▌ | 50/58 [00:02<00:00, 13.21it/s]Capturing num tokens (num_tokens=28 avail_mem=59.18 GB):  90%|████████▉ | 52/58 [00:02<00:00, 13.12it/s]Capturing num tokens (num_tokens=24 avail_mem=59.18 GB):  90%|████████▉ | 52/58 [00:02<00:00, 13.12it/s]Capturing num tokens (num_tokens=20 avail_mem=59.17 GB):  90%|████████▉ | 52/58 [00:02<00:00, 13.12it/s]

    Capturing num tokens (num_tokens=20 avail_mem=59.17 GB):  93%|█████████▎| 54/58 [00:02<00:00, 13.31it/s]Capturing num tokens (num_tokens=16 avail_mem=59.17 GB):  93%|█████████▎| 54/58 [00:02<00:00, 13.31it/s]Capturing num tokens (num_tokens=12 avail_mem=59.17 GB):  93%|█████████▎| 54/58 [00:02<00:00, 13.31it/s]Capturing num tokens (num_tokens=12 avail_mem=59.17 GB):  97%|█████████▋| 56/58 [00:02<00:00, 13.03it/s]Capturing num tokens (num_tokens=8 avail_mem=59.17 GB):  97%|█████████▋| 56/58 [00:02<00:00, 13.03it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=59.16 GB):  97%|█████████▋| 56/58 [00:03<00:00, 13.03it/s]Capturing num tokens (num_tokens=4 avail_mem=59.16 GB): 100%|██████████| 58/58 [00:03<00:00, 13.36it/s]Capturing num tokens (num_tokens=4 avail_mem=59.16 GB): 100%|██████████| 58/58 [00:03<00:00, 18.57it/s]


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
    Generated text:  Shane. I'm a computer science student at Duke University. I'm passionate about computer science and technology. I enjoy learning new things, reading books, and trying new programming languages and software applications. I have a great love of learning and don't mind working hard and always challenging myself. I'm always looking for new programming projects and love to solve problems on my own and with others. I also have a talent for algorithm design and have helped my friends solve some very challenging problems.
    What is your favorite programming language and why? I love programming in C++ because of its speed, simplicity, and the many libraries and packages that make it easy
    ===============================
    Prompt: The president of the United States is
    Generated text:  supposed to be older than 36 years old in any year. The vice president is supposed to be older than 36 years old in any year. Given that there are 40 vice presidents in the US, how many years can the president be if the vice president can only serve if the president is at least 36 years old?
    To determine how many years the president can be, we need to consider the constraints given in the problem. Specifically, the president of the United States must be at least 36 years old, and the vice president must also be at least 36 years old in any year.
    
    
    ===============================
    Prompt: The capital of France is
    Generated text:  ______.____
    A. Aachen
    B. Paris
    C. Lille
    D. Amsterdam
    
    To determine the capital of France, we need to think about the largest city in France. The capital of France is typically the largest city in that country, unless it has been merged with another city. The largest city in France is Paris.
    
    Let's go through the options given:
    
    A. Aachen - This is a city in Germany, not France.
    B. Paris - This is the capital of France.
    C. Lille - This is a city in France, not a capital.
    D. Amsterdam - This is a city
    ===============================
    Prompt: The future of AI is
    Generated text:  not entirely certain. The AI field is in the infancy of the time. The technology is in a stage where the use of artificial intelligence is still being explored. Thus, there is a lot of uncertainty surrounding the future of AI. Despite this, the technology is in a stage where it can be used to solve real-world problems. For example, it can be used to predict and prevent accidents, improve customer service, and assist in drug discovery and development. AI is also being used to help in developing new technologies and creating new products.
    
    The AI field is also expanding its scope of applications. It is now being used in the field of medical


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title]
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. 
    
    This statement is factually correct and provides a clear and concise overview of the capital city's location and significance within France. However, it could be expanded to include additional information, such as the country's other major cities, its historical significance, or its role in French culture and politics. For example:
    
    - Paris is the capital of France and the largest city in the country.
    - It is home to the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral.
    - Paris is a major center for art, culture, and commerce, hosting numerous festivals and events throughout the year.
    - The city is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior. This could lead to more sophisticated and personalized AI systems that can better understand and respond to human needs.
    
    2. Greater emphasis on ethical considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical considerations. This could lead to more stringent regulations and guidelines for AI development and deployment.
    
    3. Increased use
    


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
    Generated text:  [insert fictional character's name]. I'm a [insert fictional character's age] year old girl who enjoys [insert one or two things the character loves]. I have [insert any additional details about the character, such as a profession, hobbies, or interests]. If you had to name a single thing that you're most passionate about, it would be [insert one or two things the character likes or enjoys]. Thank you for taking the time to meet me! 🌟✨
    Hello, my name is [insert fictional character's name]. I'm a [insert fictional character's age] year old girl who enjoys [insert one
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as "La Perle" (The Pearl) and the "City of Love."
    
    The problem is that Paris has 400 years of history but only 100 years of the economy, and there are still many people who live outside Paris. With a population of about 2.5 million, Paris is one of the world's most populous cities, and the second largest metropolitan area in the world.
    
    According to the French Wikipedia, Paris is a city of contrasts, with its clock tower and neoclassical architecture, its medieval marketplaces and traditional cafes, its gardens and romantic canals, its
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  exciting and constantly evolving, with numerous trends set to shape the technology in the coming years. Some potential trends include:
    
    1. Increased Integration with Other Technologies: As AI continues to develop, it is likely to become more integrated with other technologies such as machine learning, blockchain, and natural language processing. This integration could lead to new applications of AI in fields such as healthcare, transportation, and manufacturing.
    
    2. Enhanced Transparency and Explainability: AI is becoming more capable of making decisions that are difficult to understand, leading to concerns about transparency and explainability. This could lead to increased efforts to improve the AI systems' transparency and explainability, making


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

    'm

     a

     [

    Type

     of

     Job

    ]

     who

     has

     been

     pursuing

     [

    Objective

    ]

     for

     [

    Number

     of

     Years

    ]

     years

     now

    .

     I

     enjoy

     [

    What

     I

     Do

     Best

    ]

     and

     I

    'm

     always

     looking

     for

     ways

     to

     [

    Describe

     a

     Skill

     or

     Passion

    ].

     I

    'm

     excited

     to

     meet

     you

     and

     discuss

     how

     we

     can

     work

     together

     to

     achieve

     [

    Objective

     or

     Vision

    ].

     Let

    's

     get

     started

    !

     


     [

    Name

    ]

     


     *

     Personal

     Information

     


     *

     Education

     


     *

     Experience

     


     *

     Skills

     


     *

     Inter

    ests

     and

     hobbies

     


     *

     What

     I

     hope

     to

     achieve

     through

     my

     work

     


     *

     Any

     noteworthy

     achievements

     or

     experiences

     to

     share

     


    I

    'm

     not

     just

     a

     "

    job

     applicant

    "

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     known

     for

     its

     iconic

     E

    iff

    el

     Tower

     and

     diverse

     culture

    .

     Paris

     is

     a

     bustling

     met

    ropolis

     with

     a

     rich

     history

     and

     a

     strong

     economy

    .

     The

     city

     is

     home

     to

     many

     famous

     landmarks

     such

     as

     the

     Lou

    vre

     Museum

    ,

     the

     Notre

    -D

    ame

     Cathedral

    ,

     and

     the

     Mus

    ée

     d

    '

    Or

    say

    .

     Paris

     is

     also

     known

     for

     its

     cuisine

    ,

     art

    ,

     and

     fashion

    ,

     and

     has

     a

     large

     immigrant

     population

     that

     brings

     diverse

     cultures

     to

     the

     city

    .

     It

    's

     a

     city

     with

     a

     vibrant

     and

     dynamic

     culture

    ,

     and

     its

     history

     and

     architecture

     continue

     to

     capt

    ivate

     visitors

     from

     around

     the

     world

    .

     Paris

     is

     a

     beautiful

    ,

     picturesque

    ,

     and

     fascinating

     city

     that

     is

     a

     true

     Paris

    ian

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     uncertain

    ,

     but

     there

     are

     several

     potential

     trends

     that

     could

     shape

     the

     technology

    's

     development

    .

     Here

     are

     some

     possible

     future

     trends

     in

     artificial

     intelligence

    :
    


    1

    .

     Increased

     adoption

     of

     AI

     in

     everyday

     life

    :

     One

     of

     the

     most

     significant

     trends

     in

     AI

     is

     the

     increasing

     adoption

     of

     AI

     in

     everyday

     life

    .

     AI

    -powered

     voice

     assistants

     like

     Siri

     and

     Alexa

    ,

     self

    -driving

     cars

    ,

     and

     chat

    bots

     are

     becoming

     increasingly

     prevalent

    ,

     and

     we

     are

     likely

     to

     see

     more

     of

     these

     technologies

     in

     our

     daily

     lives

    .
    


    2

    .

     Advances

     in

     AI

     ethics

     and

     privacy

    :

     AI

     is

     not

     without

     its

     risks

    ,

     and

     as

     with

     any

     technology

    ,

     we

     need

     to

     address

     the

     ethical

     and

     privacy

     concerns

     associated

     with

     it

    .

     Advances

     in

     AI

    



```python
llm.shutdown()
```
