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


    2026-04-01 18:03:23.134 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 18:03:23] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 18:03:23.134 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 18:03:23] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 18:03:23.134 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 18:03:23] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 18:03:23.134 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 18:03:23] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 18:03:23.134 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 18:03:23] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.86it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.85it/s]


    2026-04-01 18:03:25,543 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 18:03:25] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:37,  2.77s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:37,  2.77s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:37,  2.77s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:37,  2.77s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:30,  1.79it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:30,  1.79it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:30,  1.79it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:30,  1.79it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:30,  1.79it/s]

    Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:30,  1.79it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:10,  4.87it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:10,  4.87it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:10,  4.87it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:10,  4.87it/s]Compiling num tokens (num_tokens=3072):  16%|█▌        | 9/58 [00:03<00:10,  4.87it/s]Compiling num tokens (num_tokens=2816):  16%|█▌        | 9/58 [00:03<00:10,  4.87it/s]Compiling num tokens (num_tokens=2560):  16%|█▌        | 9/58 [00:03<00:10,  4.87it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:03<00:04,  9.48it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:03<00:04,  9.48it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:03<00:04,  9.48it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:03<00:04,  9.48it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:03<00:04,  9.48it/s]Compiling num tokens (num_tokens=1280):  26%|██▌       | 15/58 [00:03<00:04,  9.48it/s]

    Compiling num tokens (num_tokens=1024):  26%|██▌       | 15/58 [00:03<00:04,  9.48it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:03<00:02, 14.86it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:03<00:02, 14.86it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:03<00:02, 14.86it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:03<00:02, 14.86it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:03<00:02, 14.86it/s]Compiling num tokens (num_tokens=704):  36%|███▌      | 21/58 [00:03<00:02, 14.86it/s]Compiling num tokens (num_tokens=640):  36%|███▌      | 21/58 [00:03<00:02, 14.86it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 20.77it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 20.77it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 20.77it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 20.77it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 20.77it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 20.77it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 20.77it/s]

    Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:03<00:01, 20.77it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:03<00:00, 28.23it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:03<00:00, 28.23it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:03<00:00, 28.23it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:00, 28.23it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:00, 28.23it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:03<00:00, 28.23it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:03<00:00, 28.23it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:03<00:00, 30.34it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:03<00:00, 30.34it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:03<00:00, 30.34it/s]

    Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:03<00:00, 30.34it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:03<00:00, 30.34it/s]Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:03<00:00, 30.34it/s]Compiling num tokens (num_tokens=112):  69%|██████▉   | 40/58 [00:03<00:00, 30.34it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:03<00:00, 35.12it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:03<00:00, 35.12it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:03<00:00, 35.12it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:03<00:00, 35.12it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:03<00:00, 35.12it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:03<00:00, 35.12it/s]Compiling num tokens (num_tokens=28):  79%|███████▉  | 46/58 [00:03<00:00, 35.12it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:03<00:00, 39.69it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:03<00:00, 39.69it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:03<00:00, 39.69it/s]

    Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:03<00:00, 39.69it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:03<00:00, 39.69it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:03<00:00, 39.69it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:03<00:00, 39.69it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 14.96it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=131.67 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=131.64 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=131.64 GB):   3%|▎         | 2/58 [00:00<00:03, 18.59it/s]Capturing num tokens (num_tokens=7168 avail_mem=131.64 GB):   3%|▎         | 2/58 [00:00<00:03, 18.59it/s]Capturing num tokens (num_tokens=6656 avail_mem=131.64 GB):   3%|▎         | 2/58 [00:00<00:03, 18.59it/s]Capturing num tokens (num_tokens=6144 avail_mem=131.64 GB):   3%|▎         | 2/58 [00:00<00:03, 18.59it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=131.64 GB):   9%|▊         | 5/58 [00:00<00:02, 21.86it/s]Capturing num tokens (num_tokens=5632 avail_mem=131.63 GB):   9%|▊         | 5/58 [00:00<00:02, 21.86it/s]Capturing num tokens (num_tokens=5120 avail_mem=131.64 GB):   9%|▊         | 5/58 [00:00<00:02, 21.86it/s]Capturing num tokens (num_tokens=4608 avail_mem=131.63 GB):   9%|▊         | 5/58 [00:00<00:02, 21.86it/s]Capturing num tokens (num_tokens=4608 avail_mem=131.63 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.82it/s]Capturing num tokens (num_tokens=4096 avail_mem=131.63 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.82it/s]Capturing num tokens (num_tokens=3840 avail_mem=131.63 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.82it/s]Capturing num tokens (num_tokens=3584 avail_mem=131.62 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.82it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=131.62 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.82it/s]Capturing num tokens (num_tokens=3328 avail_mem=131.62 GB):  21%|██        | 12/58 [00:00<00:01, 27.86it/s]Capturing num tokens (num_tokens=3072 avail_mem=131.62 GB):  21%|██        | 12/58 [00:00<00:01, 27.86it/s]Capturing num tokens (num_tokens=2816 avail_mem=131.62 GB):  21%|██        | 12/58 [00:00<00:01, 27.86it/s]Capturing num tokens (num_tokens=2560 avail_mem=131.61 GB):  21%|██        | 12/58 [00:00<00:01, 27.86it/s]Capturing num tokens (num_tokens=2304 avail_mem=131.61 GB):  21%|██        | 12/58 [00:00<00:01, 27.86it/s]Capturing num tokens (num_tokens=2048 avail_mem=131.61 GB):  21%|██        | 12/58 [00:00<00:01, 27.86it/s]Capturing num tokens (num_tokens=2048 avail_mem=131.61 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.48it/s]Capturing num tokens (num_tokens=1792 avail_mem=131.60 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.48it/s]Capturing num tokens (num_tokens=1536 avail_mem=131.60 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.48it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=131.59 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.48it/s]Capturing num tokens (num_tokens=1024 avail_mem=131.57 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.48it/s]Capturing num tokens (num_tokens=1024 avail_mem=131.57 GB):  36%|███▌      | 21/58 [00:00<00:01, 30.23it/s]Capturing num tokens (num_tokens=960 avail_mem=131.59 GB):  36%|███▌      | 21/58 [00:00<00:01, 30.23it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=131.59 GB):  36%|███▌      | 21/58 [00:00<00:01, 30.23it/s]Capturing num tokens (num_tokens=832 avail_mem=131.58 GB):  36%|███▌      | 21/58 [00:01<00:01, 30.23it/s]Capturing num tokens (num_tokens=768 avail_mem=131.58 GB):  36%|███▌      | 21/58 [00:01<00:01, 30.23it/s]Capturing num tokens (num_tokens=768 avail_mem=131.58 GB):  43%|████▎     | 25/58 [00:01<00:01, 20.00it/s]Capturing num tokens (num_tokens=704 avail_mem=131.58 GB):  43%|████▎     | 25/58 [00:01<00:01, 20.00it/s]

    Capturing num tokens (num_tokens=640 avail_mem=131.57 GB):  43%|████▎     | 25/58 [00:01<00:01, 20.00it/s]Capturing num tokens (num_tokens=576 avail_mem=131.57 GB):  43%|████▎     | 25/58 [00:01<00:01, 20.00it/s]Capturing num tokens (num_tokens=576 avail_mem=131.57 GB):  48%|████▊     | 28/58 [00:01<00:01, 20.94it/s]Capturing num tokens (num_tokens=512 avail_mem=131.56 GB):  48%|████▊     | 28/58 [00:01<00:01, 20.94it/s]Capturing num tokens (num_tokens=480 avail_mem=131.58 GB):  48%|████▊     | 28/58 [00:01<00:01, 20.94it/s]Capturing num tokens (num_tokens=448 avail_mem=131.57 GB):  48%|████▊     | 28/58 [00:01<00:01, 20.94it/s]Capturing num tokens (num_tokens=416 avail_mem=131.57 GB):  48%|████▊     | 28/58 [00:01<00:01, 20.94it/s]Capturing num tokens (num_tokens=384 avail_mem=131.57 GB):  48%|████▊     | 28/58 [00:01<00:01, 20.94it/s]Capturing num tokens (num_tokens=384 avail_mem=131.57 GB):  57%|█████▋    | 33/58 [00:01<00:00, 26.02it/s]Capturing num tokens (num_tokens=352 avail_mem=131.56 GB):  57%|█████▋    | 33/58 [00:01<00:00, 26.02it/s]Capturing num tokens (num_tokens=320 avail_mem=131.56 GB):  57%|█████▋    | 33/58 [00:01<00:00, 26.02it/s]

    Capturing num tokens (num_tokens=288 avail_mem=131.56 GB):  57%|█████▋    | 33/58 [00:01<00:00, 26.02it/s]Capturing num tokens (num_tokens=256 avail_mem=131.55 GB):  57%|█████▋    | 33/58 [00:01<00:00, 26.02it/s]Capturing num tokens (num_tokens=240 avail_mem=131.55 GB):  57%|█████▋    | 33/58 [00:01<00:00, 26.02it/s]Capturing num tokens (num_tokens=240 avail_mem=131.55 GB):  66%|██████▌   | 38/58 [00:01<00:00, 30.56it/s]Capturing num tokens (num_tokens=224 avail_mem=131.55 GB):  66%|██████▌   | 38/58 [00:01<00:00, 30.56it/s]Capturing num tokens (num_tokens=208 avail_mem=131.54 GB):  66%|██████▌   | 38/58 [00:01<00:00, 30.56it/s]Capturing num tokens (num_tokens=192 avail_mem=131.54 GB):  66%|██████▌   | 38/58 [00:01<00:00, 30.56it/s]Capturing num tokens (num_tokens=176 avail_mem=131.54 GB):  66%|██████▌   | 38/58 [00:01<00:00, 30.56it/s]Capturing num tokens (num_tokens=160 avail_mem=131.53 GB):  66%|██████▌   | 38/58 [00:01<00:00, 30.56it/s]Capturing num tokens (num_tokens=160 avail_mem=131.53 GB):  74%|███████▍  | 43/58 [00:01<00:00, 34.19it/s]Capturing num tokens (num_tokens=144 avail_mem=131.53 GB):  74%|███████▍  | 43/58 [00:01<00:00, 34.19it/s]Capturing num tokens (num_tokens=128 avail_mem=131.53 GB):  74%|███████▍  | 43/58 [00:01<00:00, 34.19it/s]

    Capturing num tokens (num_tokens=112 avail_mem=131.53 GB):  74%|███████▍  | 43/58 [00:01<00:00, 34.19it/s]Capturing num tokens (num_tokens=96 avail_mem=131.52 GB):  74%|███████▍  | 43/58 [00:01<00:00, 34.19it/s] Capturing num tokens (num_tokens=80 avail_mem=131.52 GB):  74%|███████▍  | 43/58 [00:01<00:00, 34.19it/s]Capturing num tokens (num_tokens=80 avail_mem=131.52 GB):  83%|████████▎ | 48/58 [00:01<00:00, 36.96it/s]Capturing num tokens (num_tokens=64 avail_mem=131.52 GB):  83%|████████▎ | 48/58 [00:01<00:00, 36.96it/s]Capturing num tokens (num_tokens=48 avail_mem=131.51 GB):  83%|████████▎ | 48/58 [00:01<00:00, 36.96it/s]Capturing num tokens (num_tokens=32 avail_mem=131.51 GB):  83%|████████▎ | 48/58 [00:01<00:00, 36.96it/s]Capturing num tokens (num_tokens=28 avail_mem=131.50 GB):  83%|████████▎ | 48/58 [00:01<00:00, 36.96it/s]Capturing num tokens (num_tokens=24 avail_mem=131.50 GB):  83%|████████▎ | 48/58 [00:01<00:00, 36.96it/s]Capturing num tokens (num_tokens=24 avail_mem=131.50 GB):  91%|█████████▏| 53/58 [00:01<00:00, 39.18it/s]Capturing num tokens (num_tokens=20 avail_mem=131.50 GB):  91%|█████████▏| 53/58 [00:01<00:00, 39.18it/s]

    Capturing num tokens (num_tokens=16 avail_mem=131.50 GB):  91%|█████████▏| 53/58 [00:01<00:00, 39.18it/s]Capturing num tokens (num_tokens=12 avail_mem=131.49 GB):  91%|█████████▏| 53/58 [00:01<00:00, 39.18it/s]Capturing num tokens (num_tokens=8 avail_mem=131.49 GB):  91%|█████████▏| 53/58 [00:01<00:00, 39.18it/s] Capturing num tokens (num_tokens=4 avail_mem=131.48 GB):  91%|█████████▏| 53/58 [00:01<00:00, 39.18it/s]Capturing num tokens (num_tokens=4 avail_mem=131.48 GB): 100%|██████████| 58/58 [00:01<00:00, 41.15it/s]Capturing num tokens (num_tokens=4 avail_mem=131.48 GB): 100%|██████████| 58/58 [00:01<00:00, 30.91it/s]


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
    Generated text:  Claudio and I am a master of the C# programming language. I specialize in teaching software development and am passionate about helping people become more efficient with their code. I believe that writing clean, readable, and maintainable code is one of the most important skills a programmer can learn. I have worked with a variety of languages, including Java, Python, and C#. My experience has been in areas such as web development, mobile development, and desktop applications. I am also a skilled SQL database expert. With a background in artificial intelligence, I have been exploring new possibilities for machine learning and natural language processing. What are your skills and expertise
    ===============================
    Prompt: The president of the United States is
    Generated text:  a nobleman of royal birth. He is the head of the government in the country. He is usually the leader of the country. Many people like him. He is often called the "Father of the Nation" because he is the most important person in the country. He is always ready to help others. His name is Abraham Lincoln. He was born on the 6th of November, 1809. He was a very poor child. At the age of 18, he became a soldier. He fought in a war against the Indians in the south. He never told anyone about his job. He did not
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It is a city famous for its many landmarks, including the Eiffel Tower and Notre-Dame Cathedral. France is also known for its cuisine and wine, and Paris is a hub for fashion and luxury, with landmarks such as the Louvre Museum and the Eiffel Tower. The city is also famous for its chocolate and wine production.
    
    Make this true or false. Paris is the capital of France.
    False. Paris is not the capital of France. The capital of France is Paris, which is the largest city and the seat of government of France. The other locations mentioned (Eiffel Tower, Notre-Dame Cathedral
    ===============================
    Prompt: The future of AI is
    Generated text:  bright. But as we look to the future of AI, one big step is for AI to not only be used for solving complex problems, but also for creating new, interesting experiences. One interesting example of this is the new genre of interactive storytelling that is being created today.
    One of the ways in which interactive storytelling is creating new stories and experiences is through the use of computer games. In a lot of cases, these games are designed to be fun, engaging, and create a sense of community. The story may be a story about solving a difficult problem, but the game may be about making new friends.
    In some cases, the story


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a unique trait or skill] who is passionate about [insert a hobby or interest]. I'm always looking for new experiences and learning opportunities, and I'm always eager to share my knowledge and insights with others. I'm a [insert a personality trait or quality] who is always ready to help and support others. I'm a [insert a personal characteristic or trait] who is always looking for ways to improve myself and make the world
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, which is known for its rich history, beautiful architecture, and vibrant culture. It is the largest city in France and the seat of the French government and the nation's capital. Paris is also famous for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. The city is home to many famous museums, including the Louvre, the Musée d'Orsay, and the Musée d'Art Moderne. Paris is a popular tourist destination and a major cultural hub in Europe. It is also known for its cuisine, fashion, and music scene. The city is home to
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased automation: AI is expected to become more and more integrated into our daily lives, from manufacturing to customer service. As AI becomes more capable of performing tasks that were previously done by humans, we may see an increase in automation in many industries.
    
    2. AI-powered healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs. As AI becomes more advanced, we may see even more sophisticated
    


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
    Generated text:  [insert name], and I am a/an [insert age] year old [insert occupation]. I enjoy [insert hobby or interest]. I am an AI language model, trained by [insert company name], to assist users in generating text. I am here to help you, guide your thought process, and provide you with relevant information and insights. How can I assist you today? Keep in mind that my responses are generated by a large language model, and while I try to provide helpful and informative content, I am not a live human. So, if you have any questions or concerns, please feel free to ask, and I will do
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known for its iconic architecture, vibrant culture, and rich history. 
    
    (Note: I have not seen a map of Paris, but I have been able to infer its location from its historical significance and cultural prominence.) 
    
    Given the political and social importance of Paris, it is a major city in France, known for its iconic architecture and cultural landmarks. Paris is also a major hub for tourism, with its Parisian neighborhoods and charming cobblestone streets serving as important shopping and cultural centers. It is home to several world-famous landmarks, including the Eiffel Tower and Notre-Dame Cathedral. Despite its status as a major global
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be influenced by a variety of factors, including advances in hardware, software, and algorithms, as well as changes in societal and economic conditions. Here are some possible future trends in AI:
    
    1. More autonomous robots and drones: As automation and robotics continue to advance, it's likely that we'll see more autonomous robots and drones, which are designed to perform tasks without human intervention. These systems could be used in fields like agriculture, healthcare, and transportation.
    
    2. Enhanced cognitive capabilities: AI research will likely continue to advance in areas like language, perception, and reasoning, leading to increasingly advanced cognitive capabilities. This could enable AI systems


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

    ],

     and

     I

    'm

     a

     [

    Your

     Profession

    ].

     I

    'm

     excited

     to

     be

     here

     today

     to

     share

     my

     thoughts

     and

     experiences

     with

     you

    ,

     and

     to

     answer

     any

     questions

     you

     might

     have

    .

     Please

     let

     me

     know

     if

     you

     would

     like

     to

     chat

    ,

     and

     if

     you

     have

     any

     questions

    ,

     I

    'll

     do

     my

     best

     to

     provide

     you

     with

     the

     information

     you

    're

     looking

     for

    .

     Let

    's

     get

     started

    !

     [

    Your

     Name

    ]

     [

    Your

     Profession

    ]

     -

     Welcome

     to

     [

    Your

     Location

    ]

    !

     How

     can

     I

     assist

     you

     today

    ?

     [

    Your

     Name

    ]

     [

    Your

     Profession

    ]

     -

     [

    Your

     Name

    ]

     [

    Your

     Profession

    ]

     -

     [

    Your

     Name

    ]

     [

    Your

     Profession

    ]

     -

     [

    Your

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    I

     apologize

    ,

     but

     I

     cannot

     provide

     the

     requested

     information

     as

     there

     is

     no

     specific

     information

     about

     the

     capital

     city

     of

     France

     that

     I

    'm

     familiar

     with

    .

     To

     give

     a

     concise

     factual

     statement

     about

     Paris

    ,

     I

     would

     need

     more

     context

     about

     the

     topic

    .

     If

     you

     could

     provide

     more

     details

     or

     ask

     about

     a

     specific

     aspect

     of

     Paris

    ,

     I

    'd

     be

     happy

     to

     help

     you

     with

     the

     necessary

     information

    .

     Alternatively

    ,

     if

     you

     have

     a

     different

     question

     related

     to

     France

     or

     Paris

     that

     I

     can

     assist

     with

    ,

     please

     let

     me

     know

    !

     Let

     me

     know

     if

     you

    'd

     like

     to

     explore

     other

     topics

    .

     Let

     me

     know

    !

     [

    If

     you

     meant

     the

     "

    capital

    "

     of

     Switzerland

     instead

     of

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     rapidly

     evolving

    ,

     and

     there

     are

     many

     trends

     that

     are

     likely

     to

     shape

     the

     way

     we

     live

    ,

     work

    ,

     and

     interact

     with

     technology

     in

     the

     coming

     years

    .

     Here

     are

     some

     of

     the

     potential

     future

     trends

     in

     AI

    :
    


    1

    .

     AI

     will

     become

     more

     human

    -like

    :

     As

     AI

     becomes

     more

     advanced

    ,

     we

     are

     likely

     to

     see

     an

     increase

     in

     human

    -like

     behavior

     and

     emotions

     in

     AI

     systems

    .

     For

     example

    ,

     AI

    -powered

     chat

    bots

     could

     interact

     more

     human

    ely

     and

     empath

    etically

     with

     customers

    ,

     providing

     more

     personalized

     and

     helpful

     responses

    .
    


    2

    .

     AI

     will

     be

     more

     ubiquitous

    :

     As

     AI

     systems

     become

     more

     prevalent

     in

     our

     daily

     lives

    ,

     we

     are

     likely

     to

     see

     an

     increase

     in

     their

     ubiqu

    ity

    .

    



```python
llm.shutdown()
```
