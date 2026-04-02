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


    2026-04-02 00:20:35.712 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:20:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 00:20:35.712 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:20:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 00:20:35.712 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:20:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 00:20:35.712 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:20:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 00:20:35.712 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 00:20:35] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.76it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.76it/s]


    2026-04-02 00:20:38,264 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 00:20:38] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.70it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.70it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.70it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.70it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.70it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.70it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.70it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.70it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.70it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 12.05it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]

    Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.15it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.15it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.15it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.15it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.15it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.15it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.15it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.15it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.83it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.83it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.83it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.83it/s]

    Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.83it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.83it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.83it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 28.45it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 28.45it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 28.45it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 28.45it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 28.45it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 28.45it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 28.45it/s]

    Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 32.39it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 32.39it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 32.39it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 32.39it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 32.39it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 32.39it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 32.39it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 34.76it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 34.76it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 34.76it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 34.76it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 34.76it/s]

    Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 34.76it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 34.76it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 34.76it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:03<00:00, 41.01it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:03<00:00, 41.01it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.38it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=114.68 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=114.68 GB):   2%|▏         | 1/58 [00:00<00:08,  6.35it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=133.21 GB):   2%|▏         | 1/58 [00:00<00:08,  6.35it/s]Capturing num tokens (num_tokens=7680 avail_mem=133.21 GB):   3%|▎         | 2/58 [00:00<00:09,  6.17it/s]Capturing num tokens (num_tokens=7168 avail_mem=133.20 GB):   3%|▎         | 2/58 [00:00<00:09,  6.17it/s]Capturing num tokens (num_tokens=6656 avail_mem=133.20 GB):   3%|▎         | 2/58 [00:00<00:09,  6.17it/s]Capturing num tokens (num_tokens=6144 avail_mem=132.39 GB):   3%|▎         | 2/58 [00:00<00:09,  6.17it/s]Capturing num tokens (num_tokens=6144 avail_mem=132.39 GB):   9%|▊         | 5/58 [00:00<00:04, 12.95it/s]Capturing num tokens (num_tokens=5632 avail_mem=132.38 GB):   9%|▊         | 5/58 [00:00<00:04, 12.95it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=132.38 GB):   9%|▊         | 5/58 [00:00<00:04, 12.95it/s]Capturing num tokens (num_tokens=4608 avail_mem=132.38 GB):   9%|▊         | 5/58 [00:00<00:04, 12.95it/s]Capturing num tokens (num_tokens=4608 avail_mem=132.38 GB):  14%|█▍        | 8/58 [00:00<00:02, 18.05it/s]Capturing num tokens (num_tokens=4096 avail_mem=132.38 GB):  14%|█▍        | 8/58 [00:00<00:02, 18.05it/s]Capturing num tokens (num_tokens=3840 avail_mem=132.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 18.05it/s]Capturing num tokens (num_tokens=3584 avail_mem=132.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 18.05it/s]Capturing num tokens (num_tokens=3328 avail_mem=132.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 18.05it/s]Capturing num tokens (num_tokens=3328 avail_mem=132.37 GB):  21%|██        | 12/58 [00:00<00:01, 24.35it/s]Capturing num tokens (num_tokens=3072 avail_mem=132.36 GB):  21%|██        | 12/58 [00:00<00:01, 24.35it/s]Capturing num tokens (num_tokens=2816 avail_mem=132.36 GB):  21%|██        | 12/58 [00:00<00:01, 24.35it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=132.36 GB):  21%|██        | 12/58 [00:00<00:01, 24.35it/s]Capturing num tokens (num_tokens=2304 avail_mem=132.36 GB):  21%|██        | 12/58 [00:00<00:01, 24.35it/s]Capturing num tokens (num_tokens=2048 avail_mem=132.35 GB):  21%|██        | 12/58 [00:00<00:01, 24.35it/s]Capturing num tokens (num_tokens=2048 avail_mem=132.35 GB):  29%|██▉       | 17/58 [00:00<00:01, 30.76it/s]Capturing num tokens (num_tokens=1792 avail_mem=132.35 GB):  29%|██▉       | 17/58 [00:00<00:01, 30.76it/s]Capturing num tokens (num_tokens=1536 avail_mem=132.35 GB):  29%|██▉       | 17/58 [00:00<00:01, 30.76it/s]Capturing num tokens (num_tokens=1280 avail_mem=132.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 30.76it/s]Capturing num tokens (num_tokens=1024 avail_mem=132.32 GB):  29%|██▉       | 17/58 [00:00<00:01, 30.76it/s]Capturing num tokens (num_tokens=960 avail_mem=132.33 GB):  29%|██▉       | 17/58 [00:00<00:01, 30.76it/s] Capturing num tokens (num_tokens=960 avail_mem=132.33 GB):  38%|███▊      | 22/58 [00:00<00:01, 35.00it/s]Capturing num tokens (num_tokens=896 avail_mem=132.33 GB):  38%|███▊      | 22/58 [00:00<00:01, 35.00it/s]

    Capturing num tokens (num_tokens=832 avail_mem=132.33 GB):  38%|███▊      | 22/58 [00:00<00:01, 35.00it/s]Capturing num tokens (num_tokens=768 avail_mem=132.32 GB):  38%|███▊      | 22/58 [00:00<00:01, 35.00it/s]Capturing num tokens (num_tokens=704 avail_mem=132.32 GB):  38%|███▊      | 22/58 [00:00<00:01, 35.00it/s]Capturing num tokens (num_tokens=640 avail_mem=132.32 GB):  38%|███▊      | 22/58 [00:00<00:01, 35.00it/s]Capturing num tokens (num_tokens=640 avail_mem=132.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.18it/s]Capturing num tokens (num_tokens=576 avail_mem=132.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.18it/s]Capturing num tokens (num_tokens=512 avail_mem=132.30 GB):  47%|████▋     | 27/58 [00:01<00:00, 38.18it/s]Capturing num tokens (num_tokens=480 avail_mem=132.32 GB):  47%|████▋     | 27/58 [00:01<00:00, 38.18it/s]Capturing num tokens (num_tokens=448 avail_mem=132.32 GB):  47%|████▋     | 27/58 [00:01<00:00, 38.18it/s]Capturing num tokens (num_tokens=416 avail_mem=132.32 GB):  47%|████▋     | 27/58 [00:01<00:00, 38.18it/s]Capturing num tokens (num_tokens=416 avail_mem=132.32 GB):  55%|█████▌    | 32/58 [00:01<00:00, 40.37it/s]Capturing num tokens (num_tokens=384 avail_mem=132.31 GB):  55%|█████▌    | 32/58 [00:01<00:00, 40.37it/s]

    Capturing num tokens (num_tokens=352 avail_mem=132.31 GB):  55%|█████▌    | 32/58 [00:01<00:00, 40.37it/s]Capturing num tokens (num_tokens=320 avail_mem=132.30 GB):  55%|█████▌    | 32/58 [00:01<00:00, 40.37it/s]Capturing num tokens (num_tokens=288 avail_mem=132.30 GB):  55%|█████▌    | 32/58 [00:01<00:00, 40.37it/s]Capturing num tokens (num_tokens=256 avail_mem=132.30 GB):  55%|█████▌    | 32/58 [00:01<00:00, 40.37it/s]Capturing num tokens (num_tokens=256 avail_mem=132.30 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.36it/s]Capturing num tokens (num_tokens=240 avail_mem=132.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.36it/s]Capturing num tokens (num_tokens=224 avail_mem=132.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.36it/s]Capturing num tokens (num_tokens=208 avail_mem=132.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.36it/s]Capturing num tokens (num_tokens=192 avail_mem=132.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.36it/s]Capturing num tokens (num_tokens=176 avail_mem=132.28 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.36it/s]

    Capturing num tokens (num_tokens=176 avail_mem=132.28 GB):  72%|███████▏  | 42/58 [00:01<00:00, 42.37it/s]Capturing num tokens (num_tokens=160 avail_mem=132.28 GB):  72%|███████▏  | 42/58 [00:01<00:00, 42.37it/s]Capturing num tokens (num_tokens=144 avail_mem=132.28 GB):  72%|███████▏  | 42/58 [00:01<00:00, 42.37it/s]Capturing num tokens (num_tokens=128 avail_mem=132.27 GB):  72%|███████▏  | 42/58 [00:01<00:00, 42.37it/s]Capturing num tokens (num_tokens=112 avail_mem=132.27 GB):  72%|███████▏  | 42/58 [00:01<00:00, 42.37it/s]Capturing num tokens (num_tokens=96 avail_mem=132.27 GB):  72%|███████▏  | 42/58 [00:01<00:00, 42.37it/s] Capturing num tokens (num_tokens=96 avail_mem=132.27 GB):  81%|████████  | 47/58 [00:01<00:00, 43.13it/s]Capturing num tokens (num_tokens=80 avail_mem=132.26 GB):  81%|████████  | 47/58 [00:01<00:00, 43.13it/s]Capturing num tokens (num_tokens=64 avail_mem=132.26 GB):  81%|████████  | 47/58 [00:01<00:00, 43.13it/s]Capturing num tokens (num_tokens=48 avail_mem=132.26 GB):  81%|████████  | 47/58 [00:01<00:00, 43.13it/s]Capturing num tokens (num_tokens=32 avail_mem=132.25 GB):  81%|████████  | 47/58 [00:01<00:00, 43.13it/s]

    Capturing num tokens (num_tokens=28 avail_mem=132.25 GB):  81%|████████  | 47/58 [00:01<00:00, 43.13it/s]Capturing num tokens (num_tokens=28 avail_mem=132.25 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.78it/s]Capturing num tokens (num_tokens=24 avail_mem=132.25 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.78it/s]Capturing num tokens (num_tokens=20 avail_mem=132.24 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.78it/s]Capturing num tokens (num_tokens=16 avail_mem=132.24 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.78it/s]Capturing num tokens (num_tokens=12 avail_mem=132.24 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.78it/s]Capturing num tokens (num_tokens=8 avail_mem=132.23 GB):  90%|████████▉ | 52/58 [00:01<00:00, 43.78it/s] Capturing num tokens (num_tokens=8 avail_mem=132.23 GB):  98%|█████████▊| 57/58 [00:01<00:00, 44.62it/s]Capturing num tokens (num_tokens=4 avail_mem=132.23 GB):  98%|█████████▊| 57/58 [00:01<00:00, 44.62it/s]Capturing num tokens (num_tokens=4 avail_mem=132.23 GB): 100%|██████████| 58/58 [00:01<00:00, 34.42it/s]


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
    Generated text:  Yuliya, a 22 year old Romanian-American writer and graphic designer. I specialize in short fiction, poetry, and social commentary, especially addressing the issues of race, gender, and identity in the American landscape. I am a student at the University of Illinois at Chicago and an active member of the literary community.
    My debut novel, The Case of the Subtle Sandbar (forthcoming in 2023 from Baen Books), features a young protagonist who lives in a suburb of Chicago. The novel follows her as she learns about her family’s history, her struggles with discrimination, and the resilience of her community.
    
    ===============================
    Prompt: The president of the United States is
    Generated text:  a very important person in the country. But what exactly does he do? The president is the leader of the country. They have the power to decide all the important matters of the country. The president is in charge of making the decisions and the plans of the country. The president is the one who is responsible for the security of the country. The president is also responsible for making sure that the country is healthy and fit for people to live in it. The president is also responsible for the general well-being of the country. The president has the power to make important decisions about important things that need to be decided. The president is the head
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. The city is famous for its famous landmarks, such as Notre Dame Cathedral, the Eiffel Tower, and the Louvre Museum. Paris is also known for its unique cuisine and arts scene.
    
    Paris is a major city with a population of over 1 million people. It is located in the south-central region of France, and it is situated on the banks of the Seine River, which flows through the city.
    
    The city has a rich history and culture, and it has been inhabited since the time of the Romans. The city was renamed Paris in 1790, and the city of Paris is named after the
    ===============================
    Prompt: The future of AI is
    Generated text:  brighter and more promising than ever, but it’s not easy for many to change their minds, especially when it comes to major trends like autonomous vehicles. However, one thing is clear: autonomous cars are on the horizon, and we’re all in for a fantastic ride! That’s the good news. Let’s see what makes autonomous vehicles a great idea and why the car industry is pushing them ahead.
    Autonomous vehicles are the future of transportation. The future of AI is brighter and more promising than ever, but it’s not easy for many to change their minds, especially when it comes to major trends like autonomous vehicles. In this post,


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm excited to meet you and learn more about your career and interests. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about your career and interests. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about your career and interests. What can you tell me about yourself? [Name] is a [job title] at [company name]. I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also the birthplace of French literature, art, and cuisine. Paris is a cultural and economic hub, with a diverse population of over 10 million people. It is a popular tourist destination, with millions of visitors annually. The city is known for its romantic atmosphere, romantic cafes, and romantic nightclubs. Paris is also home to the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. It is a city that is steeped in history and culture
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends, including:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn and adapt to human behavior and preferences. This could lead to more natural and intuitive interactions between humans and machines.
    
    2. Greater emphasis on ethical considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical considerations. This could lead to more stringent regulations and guidelines for AI development and deployment.
    
    3. Increased use of AI for autonomous systems: AI is likely to become more widely used for autonomous systems, such as self-driving cars, robots,
    


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
    Generated text:  [Name] and I'm an [occupation] who is passionate about [specific activity or hobby]. I've been pursuing my passion for [x] for [x] years now, and I'm always looking for new ways to [describe your passion or hobby]. I'm confident in my ability to succeed in [x], and I'm excited to share my knowledge and experiences with you. How can I get to know you better? It's always good to start with a personal connection, so please ask me a few questions. Sure thing! So, what's your name? Where did you go to school? What's your first job
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, the city where the Eiffel Tower stands and the Louvre Museum is located. It is a UNESCO World Heritage site and a major financial and cultural center. Paris is also known for its iconic landmarks such as the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. It is a popular tourist destination, and the French capital is known for its rich history, cultural heritage, and entertainment scene. Paris is a bustling and dynamic city with a rich cultural scene, and it is a popular tourist destination that attracts millions of visitors annually. As of 2021, the city is home to 
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be a combination of both positive and negative trends. Some potential areas of development include:
    
    Positive trends:
    
    1. Increased automation: AI is expected to automate many tasks that currently require human intervention, leading to more efficient and productive work environments.
    
    2. Improved accessibility: AI-powered tools and services are likely to become more accessible to a wider range of people, reducing the barriers to accessing technology.
    
    3. Enhanced healthcare: AI is being used to improve patient outcomes by providing more accurate diagnoses and more personalized treatment plans.
    
    4. Increased privacy: AI systems are becoming more sophisticated and capable of handling complex data, making it possible to collect and analyze


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

    ]

     and

     I

    'm

     a

     [

    role

    ],

     [

    current

     role

    ].

     I

    'm

     [

    age

    ]

     years

     old

     and

     [

    gender

    ].

     I

     have

     [

    physical

     characteristics

     or

     abilities

    ].

     I

    've

     been

     [

    relationship

     status

    ],

     and

     my

     [

    occupation

     or

     profession

    ].

     I

    've

     lived

     in

     [

    city

    ,

     country

    ]

     for

     [

    number

     of

     years

    ].

     I

     enjoy

     [

    one

     or

     two

     hobbies

     or

     interests

    ],

     and

     I

    've

     been

     [

    one

     or

     two

     things

     I

    've

     accomplished

    ].

     I

    'm

     [

    status

    ],

     [

    confidence

     level

    ].

     I

     strive

     to

     [

    one

     or

     two

     goals

     or

     values

    ].

     I

    'm

     an

     [

    educ

    ational

     background

    ,

     professional

    ,

     or

     creative

    ]

     and

     have

     [

    number

     of

     years

    ]

     of

     experience

    .

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     the

     city

     is

     known

     for

     its

     rich

     history

    ,

     cultural

     attractions

    ,

     and

     stunning

     architecture

    ,

     including

     the

     E

    iff

    el

     Tower

     and

     Notre

    -D

    ame

     Cathedral

    .

     
    


    This

     answer

     is

     concise

     and

     factual

    .

     Please

     provide

     another

     option

     if

     needed

    .

     
    


    The

     capital

     of

     France

     is

     Paris

    ,

     the

     city

     is

     known

     for

     its

     rich

     history

    ,

     cultural

     attractions

    ,

     and

     stunning

     architecture

    ,

     including

     the

     E

    iff

    el

     Tower

     and

     Notre

    -D

    ame

     Cathedral

    .

     
    


    The

     capital

     of

     France

     is

     Paris

    ,

     the

     city

     is

     known

     for

     its

     rich

     history

    ,

     cultural

     attractions

    ,

     and

     stunning

     architecture

    ,

     including

     the

     E

    iff

    el

     Tower

     and

     Notre

    -D

    ame

     Cathedral

    .

     
    


    The

     capital

     of

     France

     is

     Paris

    ,

     the

     city

     is

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     shaped

     by

     several

     key

     trends

    ,

     including

    :
    


    1

    .

     Increased

     automation

    :

     As

     AI

     becomes

     more

     integrated

     into

     everyday

     life

    ,

     we

     can

     expect

     to

     see

     even

     greater

     automation

     and

     self

    -driving

     vehicles

     becoming

     more

     prevalent

    .
    


    2

    .

     AI

     ethics

    :

     There

     will

     likely

     be

     a

     growing

     debate

     about

     the

     ethical

     implications

     of

     AI

    ,

     with

     concerns

     about

     privacy

    ,

     bias

    ,

     and

     fairness

    .
    


    3

    .

     AI

    -powered

     healthcare

    :

     AI

     is

     being

     used

     in

     healthcare

     to

     improve

     diagnosis

     and

     treatment

    ,

     predict

     patient

     outcomes

    ,

     and

     even

     create

     personalized

     medicine

    .
    


    4

    .

     AI

     in

     finance

    :

     AI

     will

     likely

     play

     a

     larger

     role

     in

     finance

    ,

     with

     applications

     including

     fraud

     detection

    ,

     risk

     management

    ,

     and

     personalized

     investment

     advice

    .
    


    



```python
llm.shutdown()
```
