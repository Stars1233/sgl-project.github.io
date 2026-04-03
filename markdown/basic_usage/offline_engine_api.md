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


    2026-04-03 06:15:04.387 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 06:15:04] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 06:15:04.387 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 06:15:04] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 06:15:04.387 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 06:15:04] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 06:15:04.388 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 06:15:04] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 06:15:04.388 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 06:15:04] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.60it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.59it/s]


    2026-04-03 06:15:09,823 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 06:15:09] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:38,  2.78s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:38,  2.78s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:38,  2.78s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:38,  2.78s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:38,  2.78s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:23,  2.27it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:23,  2.27it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:23,  2.27it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:23,  2.27it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:23,  2.27it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:23,  2.27it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:23,  2.27it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:07,  5.99it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:07,  5.99it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:07,  5.99it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:07,  5.99it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:07,  5.99it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:07,  5.99it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:07,  5.99it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:07,  5.99it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  5.99it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.15it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.15it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.15it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.15it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.15it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.15it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.15it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.15it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.10it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.10it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.10it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.10it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.10it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.10it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.10it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.10it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:01, 24.46it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:01, 24.46it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:01, 24.46it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:01, 24.46it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:01, 24.46it/s]

    Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:01, 24.46it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:01, 24.46it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 29.44it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 29.44it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 29.44it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 29.44it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 29.44it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 29.44it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 29.44it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 34.36it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 34.36it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 34.36it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 34.36it/s]

    Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 34.36it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 34.36it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 34.36it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 38.50it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 38.50it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 38.50it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 38.50it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 38.50it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 38.50it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 38.50it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 38.50it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.44it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=136.75 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=136.72 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=136.72 GB):   3%|▎         | 2/58 [00:00<00:03, 18.55it/s]Capturing num tokens (num_tokens=7168 avail_mem=136.71 GB):   3%|▎         | 2/58 [00:00<00:03, 18.55it/s]Capturing num tokens (num_tokens=6656 avail_mem=136.71 GB):   3%|▎         | 2/58 [00:00<00:03, 18.55it/s]Capturing num tokens (num_tokens=6144 avail_mem=136.71 GB):   3%|▎         | 2/58 [00:00<00:03, 18.55it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=136.71 GB):   9%|▊         | 5/58 [00:00<00:02, 21.92it/s]Capturing num tokens (num_tokens=5632 avail_mem=136.71 GB):   9%|▊         | 5/58 [00:00<00:02, 21.92it/s]Capturing num tokens (num_tokens=5120 avail_mem=136.71 GB):   9%|▊         | 5/58 [00:00<00:02, 21.92it/s]Capturing num tokens (num_tokens=4608 avail_mem=136.70 GB):   9%|▊         | 5/58 [00:00<00:02, 21.92it/s]Capturing num tokens (num_tokens=4096 avail_mem=136.70 GB):   9%|▊         | 5/58 [00:00<00:02, 21.92it/s]Capturing num tokens (num_tokens=4096 avail_mem=136.70 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.63it/s]Capturing num tokens (num_tokens=3840 avail_mem=136.70 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.63it/s]Capturing num tokens (num_tokens=3584 avail_mem=136.70 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.63it/s]Capturing num tokens (num_tokens=3328 avail_mem=136.69 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.63it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=136.69 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.63it/s]Capturing num tokens (num_tokens=2816 avail_mem=136.69 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.63it/s]Capturing num tokens (num_tokens=2816 avail_mem=136.69 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.29it/s]Capturing num tokens (num_tokens=2560 avail_mem=136.68 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.29it/s]Capturing num tokens (num_tokens=2304 avail_mem=136.68 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.29it/s]Capturing num tokens (num_tokens=2048 avail_mem=136.68 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.29it/s]Capturing num tokens (num_tokens=1792 avail_mem=136.67 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.29it/s]Capturing num tokens (num_tokens=1536 avail_mem=136.67 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.29it/s]Capturing num tokens (num_tokens=1536 avail_mem=136.67 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.31it/s]Capturing num tokens (num_tokens=1280 avail_mem=136.67 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.31it/s]Capturing num tokens (num_tokens=1024 avail_mem=136.65 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.31it/s]

    Capturing num tokens (num_tokens=960 avail_mem=136.66 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.31it/s] Capturing num tokens (num_tokens=896 avail_mem=136.66 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.31it/s]Capturing num tokens (num_tokens=832 avail_mem=136.65 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.31it/s]Capturing num tokens (num_tokens=832 avail_mem=136.65 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.02it/s]Capturing num tokens (num_tokens=768 avail_mem=136.65 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.02it/s]Capturing num tokens (num_tokens=704 avail_mem=136.65 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.02it/s]Capturing num tokens (num_tokens=640 avail_mem=136.64 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.02it/s]Capturing num tokens (num_tokens=576 avail_mem=136.64 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.02it/s]Capturing num tokens (num_tokens=512 avail_mem=136.63 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.02it/s]Capturing num tokens (num_tokens=512 avail_mem=136.63 GB):  50%|█████     | 29/58 [00:00<00:00, 40.83it/s]Capturing num tokens (num_tokens=480 avail_mem=136.65 GB):  50%|█████     | 29/58 [00:00<00:00, 40.83it/s]

    Capturing num tokens (num_tokens=448 avail_mem=136.65 GB):  50%|█████     | 29/58 [00:00<00:00, 40.83it/s]Capturing num tokens (num_tokens=416 avail_mem=136.64 GB):  50%|█████     | 29/58 [00:00<00:00, 40.83it/s]Capturing num tokens (num_tokens=384 avail_mem=136.64 GB):  50%|█████     | 29/58 [00:00<00:00, 40.83it/s]Capturing num tokens (num_tokens=352 avail_mem=136.64 GB):  50%|█████     | 29/58 [00:00<00:00, 40.83it/s]Capturing num tokens (num_tokens=352 avail_mem=136.64 GB):  59%|█████▊    | 34/58 [00:00<00:00, 36.27it/s]Capturing num tokens (num_tokens=320 avail_mem=136.63 GB):  59%|█████▊    | 34/58 [00:00<00:00, 36.27it/s]

    Capturing num tokens (num_tokens=288 avail_mem=136.63 GB):  59%|█████▊    | 34/58 [00:01<00:00, 36.27it/s]Capturing num tokens (num_tokens=256 avail_mem=136.17 GB):  59%|█████▊    | 34/58 [00:01<00:00, 36.27it/s]Capturing num tokens (num_tokens=240 avail_mem=136.17 GB):  59%|█████▊    | 34/58 [00:01<00:00, 36.27it/s]Capturing num tokens (num_tokens=240 avail_mem=136.17 GB):  66%|██████▌   | 38/58 [00:01<00:00, 33.12it/s]Capturing num tokens (num_tokens=224 avail_mem=135.56 GB):  66%|██████▌   | 38/58 [00:01<00:00, 33.12it/s]Capturing num tokens (num_tokens=208 avail_mem=133.90 GB):  66%|██████▌   | 38/58 [00:01<00:00, 33.12it/s]Capturing num tokens (num_tokens=192 avail_mem=133.11 GB):  66%|██████▌   | 38/58 [00:01<00:00, 33.12it/s]Capturing num tokens (num_tokens=176 avail_mem=133.11 GB):  66%|██████▌   | 38/58 [00:01<00:00, 33.12it/s]

    Capturing num tokens (num_tokens=176 avail_mem=133.11 GB):  72%|███████▏  | 42/58 [00:01<00:00, 33.93it/s]Capturing num tokens (num_tokens=160 avail_mem=133.10 GB):  72%|███████▏  | 42/58 [00:01<00:00, 33.93it/s]Capturing num tokens (num_tokens=144 avail_mem=133.10 GB):  72%|███████▏  | 42/58 [00:01<00:00, 33.93it/s]Capturing num tokens (num_tokens=128 avail_mem=133.10 GB):  72%|███████▏  | 42/58 [00:01<00:00, 33.93it/s]Capturing num tokens (num_tokens=112 avail_mem=133.09 GB):  72%|███████▏  | 42/58 [00:01<00:00, 33.93it/s]Capturing num tokens (num_tokens=112 avail_mem=133.09 GB):  79%|███████▉  | 46/58 [00:01<00:00, 33.20it/s]

    Capturing num tokens (num_tokens=96 avail_mem=133.09 GB):  79%|███████▉  | 46/58 [00:01<00:00, 33.20it/s] Capturing num tokens (num_tokens=80 avail_mem=133.09 GB):  79%|███████▉  | 46/58 [00:01<00:00, 33.20it/s]Capturing num tokens (num_tokens=64 avail_mem=133.08 GB):  79%|███████▉  | 46/58 [00:01<00:00, 33.20it/s]Capturing num tokens (num_tokens=48 avail_mem=133.08 GB):  79%|███████▉  | 46/58 [00:01<00:00, 33.20it/s]

    Capturing num tokens (num_tokens=48 avail_mem=133.08 GB):  86%|████████▌ | 50/58 [00:01<00:00, 22.93it/s]Capturing num tokens (num_tokens=32 avail_mem=133.07 GB):  86%|████████▌ | 50/58 [00:01<00:00, 22.93it/s]Capturing num tokens (num_tokens=28 avail_mem=133.07 GB):  86%|████████▌ | 50/58 [00:01<00:00, 22.93it/s]Capturing num tokens (num_tokens=24 avail_mem=133.07 GB):  86%|████████▌ | 50/58 [00:01<00:00, 22.93it/s]Capturing num tokens (num_tokens=24 avail_mem=133.07 GB):  91%|█████████▏| 53/58 [00:01<00:00, 23.35it/s]Capturing num tokens (num_tokens=20 avail_mem=133.06 GB):  91%|█████████▏| 53/58 [00:01<00:00, 23.35it/s]Capturing num tokens (num_tokens=16 avail_mem=133.06 GB):  91%|█████████▏| 53/58 [00:01<00:00, 23.35it/s]

    Capturing num tokens (num_tokens=12 avail_mem=133.06 GB):  91%|█████████▏| 53/58 [00:01<00:00, 23.35it/s]Capturing num tokens (num_tokens=12 avail_mem=133.06 GB):  97%|█████████▋| 56/58 [00:01<00:00, 22.20it/s]Capturing num tokens (num_tokens=8 avail_mem=133.06 GB):  97%|█████████▋| 56/58 [00:01<00:00, 22.20it/s] Capturing num tokens (num_tokens=4 avail_mem=133.05 GB):  97%|█████████▋| 56/58 [00:01<00:00, 22.20it/s]Capturing num tokens (num_tokens=4 avail_mem=133.05 GB): 100%|██████████| 58/58 [00:02<00:00, 28.87it/s]


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
    Generated text:  Suleiman. I am a former member of the Jewish Council of Australia (JCA). I have been on the JCA for the past 22 years.
    I have been a member of the JCA for 22 years and I have been on the JCA for the past 22 years.
    I was a member of the Jewish Council of Australia (JCA) from 2012 to 2018.
    I am a member of the Jewish Council of Australia (JCA) as of 2017.
    I am a member of the Jewish Council of Australia (JCA) as
    ===============================
    Prompt: The president of the United States is
    Generated text:  a representative of the ____
    A. Federal government
    B. State government
    C. Local government
    D. The people
    Answer:
    D
    
    In the process of making a rectangular prism, we use different materials to make the base and the top of the prism. The three-dimensional shape with the largest volume that can be made is ____
    A. Rectangular prism
    B. Triangular prism
    C. Cube
    D. Cylinder
    Answer:
    C
    
    Mongolia and Russia are neighbors, bordering China to the west. Which of the following statements about their borders is incorrect? [ ]
    A. The maritime border is 
    ===============================
    Prompt: The capital of France is
    Generated text:  ______.
    A. Paris
    B. Rennes
    C. London
    D. Brussels
    Answer:
    
    A
    
    According to the provisions of the "Regulations on the Protection of Computer Software", which of the following statements about the protection period of software copyrights is correct?
    A. A person can own and use the software within China.
    B. The software copyright is protected for 50 years from the date of publication.
    C. The software copyright is protected for 20 years from the date of publication.
    D. The software copyright is protected for 25 years from the date of publication.
    Answer:
    
    C
    
    The suitable location
    ===============================
    Prompt: The future of AI is
    Generated text:  not just about digitalizing the world. It's about creating new business models and putting an end to the silos in the industrial economy.
    
    What does the future of AI look like?
    
    The future of AI is not just about digitalizing the world. It's about creating new business models and putting an end to the silos in the industrial economy.
    
    AI, or artificial intelligence, is the development of intelligent machines that perform tasks that would typically require human intelligence, such as perception, understanding context, learning, reasoning, and decision-making. AI is not just about making computers do things that used to be done by humans. It is also about


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


    Generated text:  [Name] and I'm a [Age] year old [Occupation]. I'm a [Skill or Hobby] enthusiast who loves to [Describe an activity or hobby you enjoy]. I'm always looking for new experiences and adventures, and I'm always eager to learn and grow. I'm a [Favorite Book, Movie, or Sport] lover who loves to [Describe a favorite hobby or activity]. I'm a [Favorite Color, Animal, or Food] lover who loves to [Describe a favorite hobby or activity]. I'm a [Favorite Music, Sport, or Food] lover who loves to [Describe a favorite hobby or activity
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. It is the largest city in France and the second-largest city in the European Union. It is located on the Seine River and is the seat of government, administration, and culture for the country. Paris is known for its rich history, art, and cuisine, and is a major tourist destination. The city is also home to many famous landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. Paris is a vibrant and dynamic city with a diverse population and a rich cultural heritage. The city is also known for its role in the French Revolution and its influence on French literature and art. Paris
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some possible future trends in AI:
    
    1. Increased automation and robotics: As AI technology continues to advance, we can expect to see more automation and robotics in various industries, including manufacturing, transportation, and healthcare. This will lead to increased efficiency, reduced costs, and improved quality of life for many people.
    
    2. Enhanced human-AI collaboration: As AI becomes more advanced, we can expect to see more collaboration between humans and AI. This will likely involve more complex decision-making, problem-solving, and
    


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
    Generated text:  [name]. I am a creative and visionary artist who creates paintings and sculptures that explore themes of love, loss, and the human experience. I am passionate about using my art to inspire and bring people together. I have a deep understanding of human emotions and how they are expressed through art. My work is characterized by bold colors, rich textures, and unique forms that reflect my personal style. I believe that art is a powerful tool for connection and communication, and I am constantly striving to push the boundaries of what art can be and how it can impact people's lives. I am always looking for new ways to express my artistic vision and to
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    1. How is Paris considered to be a symbol of French culture and identity?
    2. What are some unique features of Paris?
    3. Is Paris located on a specific continent?
    4. Is Paris located within a specific country?
    5. Can you list some of Paris's major landmarks? 1. Paris is considered a symbol of French culture and identity because of its beautiful architecture, rich history, and vibrant culture.
    2. Paris is known for its iconic landmarks such as the Eiffel Tower, the Louvre Museum, the Arc de Triomphe, the Notre-Dame Cathedral, and the Palace of Versailles
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  uncertain, but several trends are likely to shape it. Here are some possible future trends in AI:
    
    1. Increased automation and efficiency: As AI becomes more advanced, it will be able to automate a wide range of tasks, from manufacturing and transportation to customer service and healthcare. This will lead to increased efficiency and productivity, as machines can perform tasks faster and more accurately than humans.
    
    2. Enhanced sense of human-like intelligence: As AI continues to develop, it will become more human-like, with more natural language processing and the ability to understand and respond to human emotions.
    
    3. AI-driven autonomous systems: AI will continue to be used for


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

     of

     the

     character

    ].

     I

    'm

     a

    /an

     [

    insert

     age

    ,

     gender

    ,

     nationality

    ]

     [

    insert

     profession

    ],

     and

     I

    've

     always

     been

     passionate

     about

     [

    insert

     a

     specific

     hobby

     or

     interest

    ].

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

     [

    insert

     something

     related

     to

     the

     hobby

     or

     interest

    ].

     As

     a

     [

    insert

     career

     or

     occupation

    ],

     I

    'm

     always

     eager

     to

     [

    insert

     something

     related

     to

     the

     profession

    ].

     I

    'm

     always

     looking

     to

     [

    insert

     something

     related

     to

     the

     character

     or

     hobby

     that

     I

    'm

     passionate

     about

    ].

     I

     enjoy

     [

    insert

     something

     related

     to

     the

     hobby

     or

     interest

     that

     I

    'm

     passionate

     about

    ],

     and

     I

    'm

     always

     ready

     to

     [

    insert

     a

     positive

     action

     related

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     also

     known

     as

     "

    The

     City

     of

     Light

    ."

     It

     is

     a

     historic

     and

     culturally

     rich

     city

     with

     landmarks

     such

     as

     the

     E

    iff

    el

     Tower

    ,

     Lou

    vre

     Museum

    ,

     and

     Notre

     Dame

     Cathedral

    .

     The

     city

     is

     renowned

     for

     its

     vibrant

     nightlife

    ,

     art

     scene

    ,

     and

     romantic

     atmosphere

    ,

     making

     it

     a

     popular

     tourist

     destination

    .

     Paris

     is

     also

     home

     to

     several

     world

    -ren

    owned

     museums

    ,

     including

     the

     Mus

    ée

     d

    '

    Or

    say

     and

     the

     Mus

    ée

     Rod

    in

    .

     The

     French

     capital

     has

     a

     rich

     history

     dating

     back

     to

     the

     Roman

     Empire

     and

     has

     played

     a

     significant

     role

     in

     the

     development

     of

     modern

     France

    .

     It

     is

     a

     city

     of

     contrasts

    ,

     with

     its

     historic

     architecture

     and

     modern

     urban

    ism

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     shaped

     by

     a

     range

     of

     factors

    ,

     including

     advances

     in

     machine

     learning

    ,

     quantum

     computing

    ,

     and

     natural

     language

     processing

    .

     Here

     are

     some

     potential

     future

     trends

     in

     AI

    :
    


    1

    .

     Greater

     use

     of

     AI

     in

     healthcare

    :

     AI

    -powered

     healthcare

     systems

     are

     already

     being

     used

     to

     diagnose

     and

     treat

     patients

    ,

     and

     there

     is

     growing

     interest

     in

     using

     AI

     to

     improve

     the

     efficiency

     and

     accuracy

     of

     healthcare

     delivery

    .

     In

     the

     future

    ,

     AI

     could

     play

     a

     more

     significant

     role

     in

     diagn

    osing

     diseases

    ,

     predicting

     patient

     outcomes

    ,

     and

     developing

     new

     treatments

    .
    


    2

    .

     Increased

     use

     of

     AI

     in

     manufacturing

    :

     AI

     is

     already

     being

     used

     to

     optimize

     manufacturing

     processes

     and

     improve

     quality

     control

    .

     As

     AI

     technologies

     become

     more

     advanced

    



```python
llm.shutdown()
```
