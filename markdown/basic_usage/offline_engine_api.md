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


    2026-04-01 09:17:08.023 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:17:08] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 09:17:08.024 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:17:08] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 09:17:08.024 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:17:08] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 09:17:08.024 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:17:08] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 09:17:08.024 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:17:08] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.58it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.56it/s]


    2026-04-01 09:17:11,127 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 09:17:11] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 12.15it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 12.15it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 12.15it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 12.15it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 12.15it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 12.15it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 12.15it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 12.15it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.31it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.31it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.31it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.31it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.31it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.31it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.31it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.31it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 25.06it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 25.06it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 25.06it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 25.06it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 25.06it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 25.06it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 25.06it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 30.12it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 30.12it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 30.12it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 30.12it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 30.12it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 30.12it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 30.12it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.59it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.59it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.59it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.59it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.59it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.59it/s]

    Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.59it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.85it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.85it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.85it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.85it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.85it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.85it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.85it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.85it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.85it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.88it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.74 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=120.74 GB):   2%|▏         | 1/58 [00:04<03:50,  4.04s/it]Capturing num tokens (num_tokens=7680 avail_mem=120.31 GB):   2%|▏         | 1/58 [00:04<03:50,  4.04s/it]

    Capturing num tokens (num_tokens=7680 avail_mem=120.31 GB):   3%|▎         | 2/58 [00:04<01:43,  1.84s/it]Capturing num tokens (num_tokens=7168 avail_mem=120.34 GB):   3%|▎         | 2/58 [00:04<01:43,  1.84s/it]

    Capturing num tokens (num_tokens=7168 avail_mem=120.34 GB):   5%|▌         | 3/58 [00:04<01:01,  1.12s/it]Capturing num tokens (num_tokens=6656 avail_mem=119.95 GB):   5%|▌         | 3/58 [00:04<01:01,  1.12s/it]Capturing num tokens (num_tokens=6144 avail_mem=119.11 GB):   5%|▌         | 3/58 [00:04<01:01,  1.12s/it]Capturing num tokens (num_tokens=6144 avail_mem=119.11 GB):   9%|▊         | 5/58 [00:04<00:28,  1.85it/s]Capturing num tokens (num_tokens=5632 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:04<00:28,  1.85it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=119.02 GB):   9%|▊         | 5/58 [00:04<00:28,  1.85it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:04<00:28,  1.85it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.01 GB):  14%|█▍        | 8/58 [00:04<00:13,  3.79it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.01 GB):  14%|█▍        | 8/58 [00:04<00:13,  3.79it/s]Capturing num tokens (num_tokens=3840 avail_mem=119.01 GB):  14%|█▍        | 8/58 [00:04<00:13,  3.79it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.00 GB):  14%|█▍        | 8/58 [00:04<00:13,  3.79it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.00 GB):  14%|█▍        | 8/58 [00:04<00:13,  3.79it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.00 GB):  21%|██        | 12/58 [00:05<00:06,  7.01it/s]Capturing num tokens (num_tokens=3072 avail_mem=119.00 GB):  21%|██        | 12/58 [00:05<00:06,  7.01it/s]Capturing num tokens (num_tokens=2816 avail_mem=119.00 GB):  21%|██        | 12/58 [00:05<00:06,  7.01it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=118.99 GB):  21%|██        | 12/58 [00:05<00:06,  7.01it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.99 GB):  21%|██        | 12/58 [00:05<00:06,  7.01it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.98 GB):  21%|██        | 12/58 [00:05<00:06,  7.01it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.98 GB):  29%|██▉       | 17/58 [00:05<00:03, 11.73it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.98 GB):  29%|██▉       | 17/58 [00:05<00:03, 11.73it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.98 GB):  29%|██▉       | 17/58 [00:05<00:03, 11.73it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.97 GB):  29%|██▉       | 17/58 [00:05<00:03, 11.73it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.95 GB):  29%|██▉       | 17/58 [00:05<00:03, 11.73it/s]Capturing num tokens (num_tokens=960 avail_mem=118.97 GB):  29%|██▉       | 17/58 [00:05<00:03, 11.73it/s] Capturing num tokens (num_tokens=960 avail_mem=118.97 GB):  38%|███▊      | 22/58 [00:05<00:02, 16.76it/s]Capturing num tokens (num_tokens=896 avail_mem=118.96 GB):  38%|███▊      | 22/58 [00:05<00:02, 16.76it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  38%|███▊      | 22/58 [00:05<00:02, 16.76it/s]

    Capturing num tokens (num_tokens=768 avail_mem=118.96 GB):  38%|███▊      | 22/58 [00:05<00:02, 16.76it/s]Capturing num tokens (num_tokens=704 avail_mem=118.95 GB):  38%|███▊      | 22/58 [00:05<00:02, 16.76it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  38%|███▊      | 22/58 [00:05<00:02, 16.76it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:05<00:01, 21.86it/s]Capturing num tokens (num_tokens=576 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:05<00:01, 21.86it/s]Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  47%|████▋     | 27/58 [00:05<00:01, 21.86it/s]Capturing num tokens (num_tokens=480 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:05<00:01, 21.86it/s]Capturing num tokens (num_tokens=448 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:05<00:01, 21.86it/s]Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:05<00:01, 21.86it/s]Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  55%|█████▌    | 32/58 [00:05<00:00, 26.37it/s]Capturing num tokens (num_tokens=384 avail_mem=118.94 GB):  55%|█████▌    | 32/58 [00:05<00:00, 26.37it/s]

    Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  55%|█████▌    | 32/58 [00:05<00:00, 26.37it/s]Capturing num tokens (num_tokens=320 avail_mem=118.94 GB):  55%|█████▌    | 32/58 [00:05<00:00, 26.37it/s]Capturing num tokens (num_tokens=288 avail_mem=118.93 GB):  55%|█████▌    | 32/58 [00:05<00:00, 26.37it/s]

    Capturing num tokens (num_tokens=288 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:05<00:01, 21.63it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:05<00:01, 21.63it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:05<00:01, 21.63it/s]Capturing num tokens (num_tokens=224 avail_mem=118.92 GB):  62%|██████▏   | 36/58 [00:05<00:01, 21.63it/s]

    Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  62%|██████▏   | 36/58 [00:05<00:01, 21.63it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  69%|██████▉   | 40/58 [00:05<00:00, 19.16it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  69%|██████▉   | 40/58 [00:05<00:00, 19.16it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  69%|██████▉   | 40/58 [00:06<00:00, 19.16it/s]Capturing num tokens (num_tokens=160 avail_mem=118.91 GB):  69%|██████▉   | 40/58 [00:06<00:00, 19.16it/s]Capturing num tokens (num_tokens=160 avail_mem=118.91 GB):  74%|███████▍  | 43/58 [00:06<00:00, 20.40it/s]Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  74%|███████▍  | 43/58 [00:06<00:00, 20.40it/s]Capturing num tokens (num_tokens=128 avail_mem=118.91 GB):  74%|███████▍  | 43/58 [00:06<00:00, 20.40it/s]Capturing num tokens (num_tokens=112 avail_mem=118.90 GB):  74%|███████▍  | 43/58 [00:06<00:00, 20.40it/s]

    Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  74%|███████▍  | 43/58 [00:06<00:00, 20.40it/s] Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  74%|███████▍  | 43/58 [00:06<00:00, 20.40it/s]Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  83%|████████▎ | 48/58 [00:06<00:00, 25.31it/s]Capturing num tokens (num_tokens=64 avail_mem=118.89 GB):  83%|████████▎ | 48/58 [00:06<00:00, 25.31it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  83%|████████▎ | 48/58 [00:06<00:00, 25.31it/s]Capturing num tokens (num_tokens=32 avail_mem=118.88 GB):  83%|████████▎ | 48/58 [00:06<00:00, 25.31it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  83%|████████▎ | 48/58 [00:06<00:00, 25.31it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  83%|████████▎ | 48/58 [00:06<00:00, 25.31it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  91%|█████████▏| 53/58 [00:06<00:00, 29.82it/s]Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  91%|█████████▏| 53/58 [00:06<00:00, 29.82it/s]Capturing num tokens (num_tokens=16 avail_mem=118.87 GB):  91%|█████████▏| 53/58 [00:06<00:00, 29.82it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  91%|█████████▏| 53/58 [00:06<00:00, 29.82it/s]

    Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  91%|█████████▏| 53/58 [00:06<00:00, 29.82it/s] Capturing num tokens (num_tokens=4 avail_mem=118.86 GB):  91%|█████████▏| 53/58 [00:06<00:00, 29.82it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:06<00:00, 33.49it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:06<00:00,  9.00it/s]


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
    Generated text:  John and I would like to inquire about a specific type of pizza. Can you please provide me with information on the ingredients and toppings for a pizza that can be made in the microwave?
    Certainly! To make a microwave-friendly pizza, you'll want to use a pizza dough that is both soft and flaky, such as a pizza dough made with a whole wheat flour or a pizza dough made with a pastry sheet. You'll also want to use a pizza sauce that is simple and easy to blend, such as a tomato sauce or a pizza sauce made with fresh herbs and vegetables.
    To add some flavor and texture to the pizza, you'll
    ===============================
    Prompt: The president of the United States is
    Generated text:  currently 35 years younger than John, who is 25 years older than Mary. If the president is currently 42 years old, how old will the president be when Mary turns 30 years old? Let's start by defining the variables for the ages of the individuals involved. Let \( M \) be Mary's current age, \( J \) be John's current age, and \( P \) be the president's current age. We are given the following information:
    
    1. The president is currently 35 years younger than John:
       \[
       P = J - 35
       \
    ===============================
    Prompt: The capital of France is
    Generated text: : [1+2] = ?
    
    To determine the capital of France, we need to add the digits of the given number together. The digits of the number 12 are 1 and 2. Adding these digits together, we get:
    
    1 + 2 = 3
    
    Thus, the capital of France is \(\boxed{3}\).
    ===============================
    Prompt: The future of AI is
    Generated text:  fast approaching, and it will have an impact on various aspects of our lives, including education, healthcare, and entertainment. As AI technology continues to evolve, it is important to ensure that it is used in a responsible and ethical manner. One way to do this is through the implementation of ethical AI practices.
    Ethical AI refers to the use of AI technology that is developed and deployed in a way that respects human values, rights, and principles. This includes taking into account the potential impact of AI on society, the ways in which AI can be used to promote ethical behavior, and the ways in which AI can be used to enhance human wellbeing


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French National Library, and the French Academy of Sciences. Paris is a cultural and historical center with a rich history dating back to the Roman Empire and the French Revolution. It is a major transportation hub, with the Eiffel Tower serving as a symbol of the city's importance in global affairs. The city is also known for its cuisine, including its famous croissants and its famous French fries. Paris is a vibrant and dynamic city with a diverse population and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we interact with technology and the world around us. Here are some possible future trends in AI:
    
    1. Increased automation and robotics: As AI technology continues to improve, we can expect to see more automation and robotics in various industries, from manufacturing to healthcare to transportation. This will lead to increased efficiency and productivity, but it will also create new job opportunities and challenges for workers.
    
    2. AI-powered healthcare: AI will play a key role in improving the accuracy and efficiency of medical diagnosis and treatment. This will likely lead to more personalized and effective treatments, while also
    


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
    Generated text:  [Your Name]. I'm a [Your Job/Role] at [Your Company]. I love [Your Passion/Interest]. I'm [Your Age/Generation]. I'm [Your Hobby/Interest/Background]. I'm [Your Personality]. And here's something that you might not know - I'm [Your Unique Feature/Background/Background]. I'm excited to meet you! This is [Your Name] from [Your Company], a [Your Job/Role] at [Your Company], passionate about [Your Passion/Interest]. We're both [Your Age/Generation], so I'll be honest - my background is probably
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, an historic city with a rich cultural heritage. It is located on the River Seine and is home to the Eiffel Tower, the Louvre Museum, and many other famous landmarks. The city is also known for its art, music, and cuisine, and is a cultural center in its own right. Paris is the largest city in France and a major international center for business, politics, and tourism. It has a rich history dating back to the 6th century and continues to be a major cultural and economic center in the country. The city is also home to various cultural institutions, including the Royal Opera House and the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  highly uncertain and can be influenced by a variety of factors, including technological advancements, societal shifts, and economic pressures. Some potential trends in AI include:
    
    1. Increased focus on ethics and transparency: As AI is becoming more capable of complex decision-making, there may be increased emphasis on ethical and transparent practices. This includes developing AI that is designed to avoid bias and ensure fairness, as well as making it clear how AI systems make decisions.
    
    2. Integration with human capabilities: AI is already making significant strides in areas such as healthcare, finance, and transportation. It is likely that AI will continue to integrate with human capabilities, making AI systems more


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

    insert

     name

     here

    ],

     and

     I

     am

     a

     [

    insert

     profession

     or

     education

     here

    ].

     I

     am

     [

    insert

     how

     many

     years

     you

     have

     been

     working

     in

     the

     industry

    /

    field

     here

    ].

     I

     enjoy

     [

    insert

     one

     or

     two

     hobbies

     here

    ].

     I

     am

     [

    insert

     one

     or

     two

     things

     that

     interest

     me

     here

    ].

     I

     am

     excited

     to

     [

    insert

     any

     relevant

     information

     about

     what

     you

     are

     looking

     for

     here

    ,

     such

     as

     "

     meet

     a

     new

     person

    ",

     "

     learn

     more

     about

     a

     new

     business

    ",

     "

     make

     a

     connection

    ",

     etc

    .

     ].

     How

     would

     you

     describe

     yourself

     as

     a

     professional

    ?

     How

     do

     you

     feel

     about

     working

     with

     others

    ?

     What

     do

     you

     like

     about

     your

     work

     environment

    ?

     What

     challenges

     do

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    To

     answer

     this

     question

    ,

     I

     will

     use

     the

     following

     code

     snippet

    :
    


    ```

    python

    


    def

     get

    _cap

    ital

    _city

    ():


       

     capital

    _city

     =

     "

    Paris

    "


       

     return

     capital

    _city

    
    


    capital

    _city

     =

     get

    _cap

    ital

    _city

    ()


    print

    (cap

    ital

    _city

    )


    ``

    `
    


    Output

    :


    ``

    `


    Paris

    


    ```

     
    


    The

     answer

     is

     Paris

    .

     The

     capital

     of

     France

     is

     Paris

    .

     This

     is

     a

     factual

     statement

     about

     the

     city

    's

     name

    .

     
    


    In

     this

     code

     snippet

    ,

     I

     defined

     a

     function

     called

     `

    get

    _cap

    ital

    _city

    ()`

     that

     returns

     the

     name

     of

     the

     capital

     city

    .

     I

     then

     called

     this

     function

     to

     get

     the

     name

     of

     Paris

     and

     printed

     it

    .

     The

     output

     is

     "

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     unpredictable

     and

     uncertain

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

     technology

    's

     evolution

     in

     the

     coming

     years

    .

     Here

     are

     some

     potential

     future

     trends

     in

     artificial

     intelligence

    :
    


    1

    .

     Increased

     precision

     and

     accuracy

    :

     As

     AI

     systems

     become

     more

     complex

     and

     sophisticated

    ,

     they

     are

     likely

     to

     become

     even

     more

     accurate

     in

     their

     predictions

     and

     decisions

    .

     This

     could

     lead

     to

     increased

     precision

     and

     accuracy

     in

     fields

     such

     as

     medicine

    ,

     law

    ,

     and

     finance

    ,

     as

     well

     as

     in

     other

     industries

     where

     precision

     is

     critical

    .
    


    2

    .

     Greater

     integration

     with

     human

     intelligence

    :

     AI

     systems

     are

     becoming

     increasingly

     integrated

     with

     human

     intelligence

    ,

     which

     is

     likely

     to

     further

     enhance

     their

     capabilities

    .

     This

     could

     lead

     to

     the

     development

     of

    



```python
llm.shutdown()
```
