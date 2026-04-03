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


    2026-04-03 01:13:05.202 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 01:13:05] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 01:13:05.203 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 01:13:05] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 01:13:05.203 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 01:13:05] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 01:13:05.203 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 01:13:05] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 01:13:05.203 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 01:13:05] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.94it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.93it/s]


    2026-04-03 01:13:10,044 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 01:13:10] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:34,  2.70s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:34,  2.70s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:34,  2.70s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:34,  2.70s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:08,  5.69it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 12.04it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 12.04it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 12.04it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 12.04it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 12.04it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 12.04it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 12.04it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 12.04it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.12it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.12it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.12it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.12it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.12it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.12it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.12it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.12it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.77it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.77it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.77it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.77it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.77it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.77it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.77it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.74it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.74it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.74it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.74it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.74it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.74it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.74it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.11it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.11it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.11it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.11it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.11it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.11it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.11it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.35it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.35it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.35it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.35it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.35it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.35it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.35it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.35it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.35it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.74it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=132.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=132.39 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=132.39 GB):   3%|▎         | 2/58 [00:00<00:03, 18.26it/s]Capturing num tokens (num_tokens=7168 avail_mem=132.38 GB):   3%|▎         | 2/58 [00:00<00:03, 18.26it/s]Capturing num tokens (num_tokens=6656 avail_mem=131.85 GB):   3%|▎         | 2/58 [00:00<00:03, 18.26it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=131.85 GB):   7%|▋         | 4/58 [00:00<00:04, 12.21it/s]Capturing num tokens (num_tokens=6144 avail_mem=131.85 GB):   7%|▋         | 4/58 [00:00<00:04, 12.21it/s]Capturing num tokens (num_tokens=5632 avail_mem=131.84 GB):   7%|▋         | 4/58 [00:00<00:04, 12.21it/s]Capturing num tokens (num_tokens=5632 avail_mem=131.84 GB):  10%|█         | 6/58 [00:00<00:04, 11.31it/s]Capturing num tokens (num_tokens=5120 avail_mem=131.84 GB):  10%|█         | 6/58 [00:00<00:04, 11.31it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=131.84 GB):  10%|█         | 6/58 [00:00<00:04, 11.31it/s]Capturing num tokens (num_tokens=4608 avail_mem=131.84 GB):  14%|█▍        | 8/58 [00:00<00:04, 11.00it/s]Capturing num tokens (num_tokens=4096 avail_mem=131.84 GB):  14%|█▍        | 8/58 [00:00<00:04, 11.00it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=131.84 GB):  14%|█▍        | 8/58 [00:00<00:04, 11.00it/s]Capturing num tokens (num_tokens=3840 avail_mem=131.84 GB):  17%|█▋        | 10/58 [00:00<00:04,  9.85it/s]Capturing num tokens (num_tokens=3584 avail_mem=131.83 GB):  17%|█▋        | 10/58 [00:00<00:04,  9.85it/s]Capturing num tokens (num_tokens=3328 avail_mem=131.83 GB):  17%|█▋        | 10/58 [00:00<00:04,  9.85it/s]Capturing num tokens (num_tokens=3328 avail_mem=131.83 GB):  21%|██        | 12/58 [00:01<00:03, 11.51it/s]Capturing num tokens (num_tokens=3072 avail_mem=131.82 GB):  21%|██        | 12/58 [00:01<00:03, 11.51it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=131.82 GB):  21%|██        | 12/58 [00:01<00:03, 11.51it/s]Capturing num tokens (num_tokens=2560 avail_mem=131.82 GB):  21%|██        | 12/58 [00:01<00:03, 11.51it/s]Capturing num tokens (num_tokens=2560 avail_mem=131.82 GB):  26%|██▌       | 15/58 [00:01<00:02, 15.40it/s]Capturing num tokens (num_tokens=2304 avail_mem=131.82 GB):  26%|██▌       | 15/58 [00:01<00:02, 15.40it/s]Capturing num tokens (num_tokens=2048 avail_mem=131.81 GB):  26%|██▌       | 15/58 [00:01<00:02, 15.40it/s]Capturing num tokens (num_tokens=1792 avail_mem=131.81 GB):  26%|██▌       | 15/58 [00:01<00:02, 15.40it/s]Capturing num tokens (num_tokens=1536 avail_mem=131.81 GB):  26%|██▌       | 15/58 [00:01<00:02, 15.40it/s]Capturing num tokens (num_tokens=1536 avail_mem=131.81 GB):  33%|███▎      | 19/58 [00:01<00:01, 20.50it/s]Capturing num tokens (num_tokens=1280 avail_mem=131.80 GB):  33%|███▎      | 19/58 [00:01<00:01, 20.50it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=131.78 GB):  33%|███▎      | 19/58 [00:01<00:01, 20.50it/s]Capturing num tokens (num_tokens=960 avail_mem=131.79 GB):  33%|███▎      | 19/58 [00:01<00:01, 20.50it/s] Capturing num tokens (num_tokens=896 avail_mem=131.79 GB):  33%|███▎      | 19/58 [00:01<00:01, 20.50it/s]Capturing num tokens (num_tokens=832 avail_mem=131.79 GB):  33%|███▎      | 19/58 [00:01<00:01, 20.50it/s]Capturing num tokens (num_tokens=832 avail_mem=131.79 GB):  41%|████▏     | 24/58 [00:01<00:01, 26.24it/s]Capturing num tokens (num_tokens=768 avail_mem=131.78 GB):  41%|████▏     | 24/58 [00:01<00:01, 26.24it/s]Capturing num tokens (num_tokens=704 avail_mem=131.78 GB):  41%|████▏     | 24/58 [00:01<00:01, 26.24it/s]Capturing num tokens (num_tokens=640 avail_mem=131.78 GB):  41%|████▏     | 24/58 [00:01<00:01, 26.24it/s]Capturing num tokens (num_tokens=576 avail_mem=131.77 GB):  41%|████▏     | 24/58 [00:01<00:01, 26.24it/s]Capturing num tokens (num_tokens=576 avail_mem=131.77 GB):  48%|████▊     | 28/58 [00:01<00:01, 29.52it/s]Capturing num tokens (num_tokens=512 avail_mem=131.76 GB):  48%|████▊     | 28/58 [00:01<00:01, 29.52it/s]

    Capturing num tokens (num_tokens=480 avail_mem=131.78 GB):  48%|████▊     | 28/58 [00:01<00:01, 29.52it/s]Capturing num tokens (num_tokens=448 avail_mem=131.78 GB):  48%|████▊     | 28/58 [00:01<00:01, 29.52it/s]Capturing num tokens (num_tokens=416 avail_mem=131.77 GB):  48%|████▊     | 28/58 [00:01<00:01, 29.52it/s]Capturing num tokens (num_tokens=416 avail_mem=131.77 GB):  55%|█████▌    | 32/58 [00:01<00:00, 31.67it/s]Capturing num tokens (num_tokens=384 avail_mem=131.77 GB):  55%|█████▌    | 32/58 [00:01<00:00, 31.67it/s]Capturing num tokens (num_tokens=352 avail_mem=131.77 GB):  55%|█████▌    | 32/58 [00:01<00:00, 31.67it/s]Capturing num tokens (num_tokens=320 avail_mem=131.76 GB):  55%|█████▌    | 32/58 [00:01<00:00, 31.67it/s]Capturing num tokens (num_tokens=288 avail_mem=131.76 GB):  55%|█████▌    | 32/58 [00:01<00:00, 31.67it/s]Capturing num tokens (num_tokens=288 avail_mem=131.76 GB):  62%|██████▏   | 36/58 [00:01<00:00, 33.38it/s]Capturing num tokens (num_tokens=256 avail_mem=131.76 GB):  62%|██████▏   | 36/58 [00:01<00:00, 33.38it/s]

    Capturing num tokens (num_tokens=240 avail_mem=131.75 GB):  62%|██████▏   | 36/58 [00:01<00:00, 33.38it/s]Capturing num tokens (num_tokens=224 avail_mem=131.75 GB):  62%|██████▏   | 36/58 [00:01<00:00, 33.38it/s]Capturing num tokens (num_tokens=208 avail_mem=131.75 GB):  62%|██████▏   | 36/58 [00:01<00:00, 33.38it/s]Capturing num tokens (num_tokens=208 avail_mem=131.75 GB):  69%|██████▉   | 40/58 [00:01<00:00, 34.39it/s]Capturing num tokens (num_tokens=192 avail_mem=131.75 GB):  69%|██████▉   | 40/58 [00:01<00:00, 34.39it/s]Capturing num tokens (num_tokens=176 avail_mem=131.74 GB):  69%|██████▉   | 40/58 [00:01<00:00, 34.39it/s]Capturing num tokens (num_tokens=160 avail_mem=131.74 GB):  69%|██████▉   | 40/58 [00:01<00:00, 34.39it/s]Capturing num tokens (num_tokens=144 avail_mem=131.73 GB):  69%|██████▉   | 40/58 [00:01<00:00, 34.39it/s]Capturing num tokens (num_tokens=128 avail_mem=131.73 GB):  69%|██████▉   | 40/58 [00:01<00:00, 34.39it/s]Capturing num tokens (num_tokens=128 avail_mem=131.73 GB):  78%|███████▊  | 45/58 [00:01<00:00, 37.48it/s]Capturing num tokens (num_tokens=112 avail_mem=131.73 GB):  78%|███████▊  | 45/58 [00:01<00:00, 37.48it/s]

    Capturing num tokens (num_tokens=96 avail_mem=131.72 GB):  78%|███████▊  | 45/58 [00:01<00:00, 37.48it/s] Capturing num tokens (num_tokens=80 avail_mem=131.72 GB):  78%|███████▊  | 45/58 [00:01<00:00, 37.48it/s]Capturing num tokens (num_tokens=64 avail_mem=131.72 GB):  78%|███████▊  | 45/58 [00:01<00:00, 37.48it/s]Capturing num tokens (num_tokens=48 avail_mem=131.72 GB):  78%|███████▊  | 45/58 [00:02<00:00, 37.48it/s]Capturing num tokens (num_tokens=48 avail_mem=131.72 GB):  86%|████████▌ | 50/58 [00:02<00:00, 39.36it/s]Capturing num tokens (num_tokens=32 avail_mem=131.71 GB):  86%|████████▌ | 50/58 [00:02<00:00, 39.36it/s]Capturing num tokens (num_tokens=28 avail_mem=131.71 GB):  86%|████████▌ | 50/58 [00:02<00:00, 39.36it/s]Capturing num tokens (num_tokens=24 avail_mem=131.70 GB):  86%|████████▌ | 50/58 [00:02<00:00, 39.36it/s]Capturing num tokens (num_tokens=20 avail_mem=131.70 GB):  86%|████████▌ | 50/58 [00:02<00:00, 39.36it/s]Capturing num tokens (num_tokens=16 avail_mem=131.70 GB):  86%|████████▌ | 50/58 [00:02<00:00, 39.36it/s]

    Capturing num tokens (num_tokens=16 avail_mem=131.70 GB):  95%|█████████▍| 55/58 [00:02<00:00, 39.82it/s]Capturing num tokens (num_tokens=12 avail_mem=131.13 GB):  95%|█████████▍| 55/58 [00:02<00:00, 39.82it/s]Capturing num tokens (num_tokens=8 avail_mem=132.08 GB):  95%|█████████▍| 55/58 [00:02<00:00, 39.82it/s] Capturing num tokens (num_tokens=4 avail_mem=131.66 GB):  95%|█████████▍| 55/58 [00:02<00:00, 39.82it/s]Capturing num tokens (num_tokens=4 avail_mem=131.66 GB): 100%|██████████| 58/58 [00:02<00:00, 24.77it/s]


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
    Generated text:  Nikola. I'm a software engineer and web developer, and I've been passionate about technology and programming since I was a teenager. I'm always looking for new ways to make my projects more efficient and fun.
    Please share any interesting or unexpected things you've learned in your programming journey so far.
    As an AI language model, I don't have a programming journey or personal experiences, but I can tell you about some common learning experiences for programmers:
    
    1. Learning to code: Many programmers start by learning to code by watching tutorials online or by taking courses on platforms like Coursera, Udemy, or edX.
    
    2. Building projects
    ===============================
    Prompt: The president of the United States is
    Generated text:  200 inches tall. The mayor of the city is 12 inches shorter than the president, and the vice president is 1/3 the height of the mayor. How tall is the vice president?
    To find the height of the vice president, we need to follow the steps outlined in the problem.
    
    1. Identify the height of the president.
       The president is 200 inches tall.
    
    2. Determine the height of the mayor.
       The mayor is 12 inches shorter than the president. Therefore, we subtract 12 inches from the height of the president.
       \[
       \text{
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. The official language is French. The capital of France is ( ).
    A: London
    B: Rome
    C: Berlin
    D: Moscow
    To determine the capital of France, we need to understand which country is located in Europe and has the name "Paris." Let's analyze the options given:
    
    A: London - Located in Europe but not the capital of France.
    B: Rome - Located in Europe but not the capital of France.
    C: Berlin - Located in Europe but not the capital of France.
    D: Moscow - Located in Europe but not the capital of France.
    
    Since Moscow is located in Europe and is not the
    ===============================
    Prompt: The future of AI is
    Generated text:  in the data and there are many areas that require precise, detailed, and context-based analysis. When we review our data, we can determine if it is accurate, and also make decisions based on our understanding of it. We can do this by analyzing the data, identifying the patterns and relationships, and making decisions that align with our goals.
    When it comes to ensuring that the data we use is accurate and reliable, there are a few key steps we can take:
    
    1. Collecting accurate data: We must ensure that we are collecting accurate data. This means that we must collect data from a variety of sources, including surveys, interviews,


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a brief description of your job or experience here]. How do you feel about working with [insert a company name]? I'm always looking for new opportunities to grow and learn, and I'm eager to contribute to [insert a company name]. What's your favorite hobby or activity? I enjoy [insert a hobby or activity here]. And what's your favorite book or movie? I love [insert a favorite book or movie here]. What
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French National Library, and the French National Museum. Paris is a bustling metropolis with a rich cultural heritage and is a major tourist destination. It is also known for its fashion industry, with Paris Fashion Week being one of the largest and most prestigious fashion events in the world. The city is also home to the French Academy of Sciences, the French National Museum of Modern Art, and the French National Library. Overall, Paris is a city of contrasts and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior and decision-making processes. This could lead to more sophisticated and adaptive AI systems that can learn from experience and make better decisions.
    
    2. Greater emphasis on ethical considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical considerations. This could lead to more stringent regulations and standards for AI development and deployment,
    


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
    Generated text:  [Your Name]. I am a computer programmer with 20 years of experience in the field. I have always been passionate about technology and programming, and I have learned a lot along the way. I have worked on a variety of projects, from simple websites to large-scale software projects. I love solving complex problems and I am constantly exploring new technologies to stay up-to-date. I am a hard worker, enjoy challenges, and am always looking for new ways to improve myself. I believe that my passion for technology and my strong work ethic will help me achieve success in my career and in life. Thanks for asking! [Your Name]
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, an iconic city renowned for its rich history, culture, and cultural diversity. It's home to iconic landmarks like the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Arc de Triomphe, among others. Paris is also a major financial center and home to the French Riviera, a tourist destination known for its beaches, restaurants, and nightlife. The city is also famous for its wine, cuisine, and fashion. Paris is a global cultural and business hub that draws millions of visitors annually. It's a city that has been shaped by both its historical and modern influences, making it a fascinating and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  bright, with the ability to process and analyze vast amounts of information and data exponentially growing. Here are some potential trends we can expect to see:
    
    1. Increased specialization: As AI becomes more advanced, there will be more specialization in areas like robotics, healthcare, finance, and transportation.
    
    2. Rise of ethical AI: With the increase in the use of AI, we will see more ethical considerations around AI systems. As a result, there may be efforts to develop AI that is more transparent, accountable, and responsible.
    
    3. Automation and automation: AI is already being used to automate many tasks, but we can expect it to continue to


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

    ],

     and

     I

    'm

     a

     [

    insert

     occupation

    ]

     with

     [

    insert

     educational

     background

    ]

     and

     a

     [

    insert

     any

     relevant

     experience

     or

     skill

     set

    ].

     I

     am

     [

    insert

     any

     personal

     attributes

     or

     values

    ].

     [

    Insert

     the

     main

     points

     of

     your

     self

    -int

    roduction

    ].

     I

    'm

     excited

     to

     meet

     you

     and

     learn

     more

     about

     you

    !


    This

     is

     a

     good

     introduction

    ,

     but

     I

     was

     hoping

     for

     a

     bit

     more

     detail

     about

     the

     character

    's

     personality

     or

     background

    .

     Could

     you

     add

     that

     to

     the

     self

    -int

    roduction

     to

     really

     paint

     a

     fuller

     picture

     of

     who

     they

     are

    ?

     Of

     course

    !

     Here

    's

     a

     revised

     version

     with

     some

     additional

     details

    :
    


    Hello

    ,

     my

     name

     is

     [

    insert

     name

    ],

     and

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     which

     is

     located

     in

     the

     south

     of

     the

     country

     and

     is

     the

     largest

     and

     most

     populous

     city

     in

     the

     country

    .
    


    Why

     was

     Paris

     chosen

     as

     the

     capital

     of

     France

    ?

     Paris

     was

     chosen

     as

     the

     capital

     of

     France

     because

     it

     was

     founded

     by

     the

     Romans

     in

     

    7

    3

     BCE

     and

     has

     been

     the

     seat

     of

     government

     and

     a

     center

     of

     culture

     and

     art

     for

     thousands

     of

     years

    .

     The

     city

     was

     the

     home

     of

     the

     French

     Revolution

     and

     has

     been

     the

     seat

     of

     government

     for

     France

     since

     the

     end

     of

     the

     French

     Revolution

     in

     

    1

    8

    7

    0

    .
    


    Can

     you

     provide

     some

     details

     about

     the

     architecture

     of

     Paris

    ?

     Yes

    ,

     here

     are

     some

     details

     about

     the

     architecture

     of

     Paris

    :
    


     

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     very

     promising

     and

     there

     are

     a

     lot

     of

     possibilities

     for

     its

     developments

    .

     Some

     of

     the

     most

     likely

     trends

     to

     come

     in

     the

     near

     future

     include

    :
    


    1

    .

     Increased

     integration

     of

     AI

     into

     our

     daily

     lives

    :

     The

     next

     few

     years

     will

     see

     a

     growing

     integration

     of

     AI

     into

     everyday

     life

    ,

     from

     smart

     home

     technology

     to

     autonomous

     vehicles

    .

     This

     will

     likely

     lead

     to

     new

     applications

     and

     opportunities

     for

     AI

     developers

    .
    


    2

    .

     More

     efficient

     and

     effective

     AI

     systems

    :

     As

     AI

     becomes

     more

     advanced

    ,

     we

     will

     see

     more

     efficient

     and

     effective

     systems

     that

     can

     perform

     a

     wide

     range

     of

     tasks

    ,

     from

     natural

     language

     processing

     to

     image

     recognition

    .

     This

     will

     lead

     to

     greater

     productivity

     and

     efficiency

     in

     various

     industries

    .
    


    3

    .

    



```python
llm.shutdown()
```
