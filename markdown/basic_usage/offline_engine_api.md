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


    2026-04-01 09:48:35.001 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:48:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 09:48:35.001 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:48:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 09:48:35.001 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:48:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 09:48:35.001 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:48:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 09:48:35.001 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 09:48:35] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.79it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.78it/s]


    2026-04-01 09:48:37,413 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 09:48:37] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.76it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.76it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.76it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.76it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.76it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.76it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.76it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.76it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.76it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 12.18it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 12.18it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 12.18it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 12.18it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 12.18it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 12.18it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 12.18it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 12.18it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.34it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.34it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.34it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.34it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.34it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.34it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.34it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.34it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 25.08it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 25.08it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 25.08it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 25.08it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 25.08it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 25.08it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 25.08it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 30.14it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 30.14it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 30.14it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 30.14it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 30.14it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 30.14it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 30.14it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.56it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.56it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.56it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.56it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.56it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.56it/s]

    Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.56it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.87it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.87it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.87it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.87it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.87it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.87it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.87it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.87it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.87it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.95it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=137.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=137.39 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=137.39 GB):   3%|▎         | 2/58 [00:00<00:03, 18.16it/s]Capturing num tokens (num_tokens=7168 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:03, 18.16it/s]Capturing num tokens (num_tokens=6656 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:03, 18.16it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:03, 18.16it/s]Capturing num tokens (num_tokens=6144 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.13it/s]Capturing num tokens (num_tokens=5632 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.13it/s]Capturing num tokens (num_tokens=5120 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.13it/s]Capturing num tokens (num_tokens=4608 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.13it/s]Capturing num tokens (num_tokens=4608 avail_mem=137.38 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.66it/s]Capturing num tokens (num_tokens=4096 avail_mem=137.38 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.66it/s]Capturing num tokens (num_tokens=3840 avail_mem=137.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.66it/s]Capturing num tokens (num_tokens=3584 avail_mem=137.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.66it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=137.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.66it/s]Capturing num tokens (num_tokens=3328 avail_mem=137.37 GB):  21%|██        | 12/58 [00:00<00:01, 29.54it/s]Capturing num tokens (num_tokens=3072 avail_mem=137.36 GB):  21%|██        | 12/58 [00:00<00:01, 29.54it/s]Capturing num tokens (num_tokens=2816 avail_mem=137.36 GB):  21%|██        | 12/58 [00:00<00:01, 29.54it/s]Capturing num tokens (num_tokens=2560 avail_mem=137.36 GB):  21%|██        | 12/58 [00:00<00:01, 29.54it/s]Capturing num tokens (num_tokens=2304 avail_mem=137.35 GB):  21%|██        | 12/58 [00:00<00:01, 29.54it/s]Capturing num tokens (num_tokens=2048 avail_mem=137.35 GB):  21%|██        | 12/58 [00:00<00:01, 29.54it/s]Capturing num tokens (num_tokens=2048 avail_mem=137.35 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.79it/s]Capturing num tokens (num_tokens=1792 avail_mem=137.35 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.79it/s]Capturing num tokens (num_tokens=1536 avail_mem=137.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.79it/s]Capturing num tokens (num_tokens=1280 avail_mem=137.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.79it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=137.32 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.79it/s]Capturing num tokens (num_tokens=960 avail_mem=137.33 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.79it/s] Capturing num tokens (num_tokens=960 avail_mem=137.33 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.20it/s]Capturing num tokens (num_tokens=896 avail_mem=137.33 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.20it/s]Capturing num tokens (num_tokens=832 avail_mem=137.33 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.20it/s]

    Capturing num tokens (num_tokens=768 avail_mem=137.30 GB):  38%|███▊      | 22/58 [00:08<00:00, 38.20it/s]Capturing num tokens (num_tokens=704 avail_mem=137.30 GB):  38%|███▊      | 22/58 [00:08<00:00, 38.20it/s]Capturing num tokens (num_tokens=704 avail_mem=137.30 GB):  45%|████▍     | 26/58 [00:08<00:21,  1.52it/s]Capturing num tokens (num_tokens=640 avail_mem=137.30 GB):  45%|████▍     | 26/58 [00:08<00:21,  1.52it/s]Capturing num tokens (num_tokens=576 avail_mem=137.30 GB):  45%|████▍     | 26/58 [00:08<00:21,  1.52it/s]Capturing num tokens (num_tokens=512 avail_mem=137.29 GB):  45%|████▍     | 26/58 [00:08<00:21,  1.52it/s]

    Capturing num tokens (num_tokens=512 avail_mem=137.29 GB):  50%|█████     | 29/58 [00:09<00:14,  1.98it/s]Capturing num tokens (num_tokens=480 avail_mem=137.30 GB):  50%|█████     | 29/58 [00:09<00:14,  1.98it/s]Capturing num tokens (num_tokens=448 avail_mem=137.30 GB):  50%|█████     | 29/58 [00:09<00:14,  1.98it/s]Capturing num tokens (num_tokens=416 avail_mem=137.28 GB):  50%|█████     | 29/58 [00:09<00:14,  1.98it/s]Capturing num tokens (num_tokens=416 avail_mem=137.28 GB):  55%|█████▌    | 32/58 [00:09<00:10,  2.58it/s]Capturing num tokens (num_tokens=384 avail_mem=136.80 GB):  55%|█████▌    | 32/58 [00:09<00:10,  2.58it/s]

    Capturing num tokens (num_tokens=352 avail_mem=136.70 GB):  55%|█████▌    | 32/58 [00:09<00:10,  2.58it/s]Capturing num tokens (num_tokens=320 avail_mem=136.66 GB):  55%|█████▌    | 32/58 [00:09<00:10,  2.58it/s]Capturing num tokens (num_tokens=320 avail_mem=136.66 GB):  60%|██████    | 35/58 [00:09<00:06,  3.44it/s]Capturing num tokens (num_tokens=288 avail_mem=136.63 GB):  60%|██████    | 35/58 [00:09<00:06,  3.44it/s]Capturing num tokens (num_tokens=256 avail_mem=136.63 GB):  60%|██████    | 35/58 [00:09<00:06,  3.44it/s]Capturing num tokens (num_tokens=240 avail_mem=136.62 GB):  60%|██████    | 35/58 [00:09<00:06,  3.44it/s]Capturing num tokens (num_tokens=224 avail_mem=136.62 GB):  60%|██████    | 35/58 [00:09<00:06,  3.44it/s]Capturing num tokens (num_tokens=208 avail_mem=136.62 GB):  60%|██████    | 35/58 [00:09<00:06,  3.44it/s]Capturing num tokens (num_tokens=208 avail_mem=136.62 GB):  69%|██████▉   | 40/58 [00:09<00:03,  5.41it/s]Capturing num tokens (num_tokens=192 avail_mem=136.62 GB):  69%|██████▉   | 40/58 [00:09<00:03,  5.41it/s]Capturing num tokens (num_tokens=176 avail_mem=136.61 GB):  69%|██████▉   | 40/58 [00:09<00:03,  5.41it/s]

    Capturing num tokens (num_tokens=160 avail_mem=136.61 GB):  69%|██████▉   | 40/58 [00:09<00:03,  5.41it/s]Capturing num tokens (num_tokens=144 avail_mem=136.61 GB):  69%|██████▉   | 40/58 [00:09<00:03,  5.41it/s]Capturing num tokens (num_tokens=128 avail_mem=136.60 GB):  69%|██████▉   | 40/58 [00:09<00:03,  5.41it/s]Capturing num tokens (num_tokens=128 avail_mem=136.60 GB):  78%|███████▊  | 45/58 [00:09<00:01,  7.93it/s]Capturing num tokens (num_tokens=112 avail_mem=136.60 GB):  78%|███████▊  | 45/58 [00:09<00:01,  7.93it/s]Capturing num tokens (num_tokens=96 avail_mem=136.60 GB):  78%|███████▊  | 45/58 [00:09<00:01,  7.93it/s] Capturing num tokens (num_tokens=80 avail_mem=136.59 GB):  78%|███████▊  | 45/58 [00:09<00:01,  7.93it/s]Capturing num tokens (num_tokens=64 avail_mem=136.59 GB):  78%|███████▊  | 45/58 [00:09<00:01,  7.93it/s]Capturing num tokens (num_tokens=48 avail_mem=136.59 GB):  78%|███████▊  | 45/58 [00:09<00:01,  7.93it/s]Capturing num tokens (num_tokens=48 avail_mem=136.59 GB):  86%|████████▌ | 50/58 [00:09<00:00, 11.00it/s]Capturing num tokens (num_tokens=32 avail_mem=136.58 GB):  86%|████████▌ | 50/58 [00:09<00:00, 11.00it/s]

    Capturing num tokens (num_tokens=28 avail_mem=136.58 GB):  86%|████████▌ | 50/58 [00:09<00:00, 11.00it/s]Capturing num tokens (num_tokens=24 avail_mem=136.58 GB):  86%|████████▌ | 50/58 [00:09<00:00, 11.00it/s]Capturing num tokens (num_tokens=20 avail_mem=136.57 GB):  86%|████████▌ | 50/58 [00:09<00:00, 11.00it/s]Capturing num tokens (num_tokens=16 avail_mem=136.57 GB):  86%|████████▌ | 50/58 [00:09<00:00, 11.00it/s]Capturing num tokens (num_tokens=16 avail_mem=136.57 GB):  95%|█████████▍| 55/58 [00:09<00:00, 14.65it/s]Capturing num tokens (num_tokens=12 avail_mem=136.57 GB):  95%|█████████▍| 55/58 [00:09<00:00, 14.65it/s]Capturing num tokens (num_tokens=8 avail_mem=136.56 GB):  95%|█████████▍| 55/58 [00:09<00:00, 14.65it/s] Capturing num tokens (num_tokens=4 avail_mem=136.56 GB):  95%|█████████▍| 55/58 [00:09<00:00, 14.65it/s]Capturing num tokens (num_tokens=4 avail_mem=136.56 GB): 100%|██████████| 58/58 [00:09<00:00,  5.92it/s]


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
    Generated text:  Emily and I'm currently a freshman at the University of Toronto. My name is Emily and I'm a first-year student at the University of Toronto. I'm studying Economics and Political Science. I'm looking for some advice on how to make myself more employable. I have a lot of free time to study on my own but I don't know how to find a job. How can I make myself more employable? I'm looking to major in Economics and Political Science. There is a big gap between my education in Economics and my requirement for Political Science. I feel stuck in my major and I have to get used to how to
    ===============================
    Prompt: The president of the United States is
    Generated text:  a very important person. He has many offices, such as the president of the United States, the President of the Senate and the President of the House of Representatives. He is also the leader of the government of the United States. He is the commander-in-chief of the armed forces and the highest-ranking official in the United States. The president of the United States is the head of the government and is often called the "boss" of the government. He is the leader of the United States and often has to make important decisions for the country. The president has to work with the other members of the government. He is responsible for making sure
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris.
    A. 正确
    B. 错误
    Answer:
    
    A
    
    Which of the following statements about the characteristics of the Chinese Dream is correct? ( ) ① The Chinese Dream is the dream of the Chinese nation. ② The Chinese Dream is the dream of the people of all ethnic groups in China. ③ The Chinese Dream is a vividly depicted vision of the future of the Chinese nation and the Chinese people. ④ The Chinese Dream has rich content, the main elements include economic prosperity, social harmony, national unity, and ethnic harmony.
    A. ①②③
    ===============================
    Prompt: The future of AI is
    Generated text:  about to get very exciting. If not for the recent fiasco of the Boston Dynamics robot, we'd be talking about how this tech could revolutionize the way we handle our pets. But as we move forward, the future of AI is changing. As the technology is becoming more sophisticated, there is a lot to think about.
    The world of AI is quite exciting, and the possibilities of using AI to improve our lives are endless. But there are also many potential risks and ethical concerns associated with AI.
    One of the main risks of AI is that it can be used to harm or exploit people. This is a particularly important concern when it


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a brief description of your profession or role]. I enjoy [insert a short description of your hobbies or interests]. What brings you to this company? I'm always looking for new challenges and opportunities to grow and learn. What's your favorite part of working at [company name]? I love the [insert a short description of a fun or memorable experience you've had at [company name]]. What's your biggest challenge at the moment? I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major cultural and economic center, hosting numerous museums, theaters, and other attractions. Paris is a popular tourist destination and a major hub for international business and diplomacy. The city is known for its rich history, including the influence of the French Revolution and the influence of the French Revolution on modern French culture and politics. Paris is also home to many famous French artists, writers, and musicians, including Pablo Picasso, Vincent van Gogh, and Claude Monet. The city is known for
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased integration with other technologies: AI is likely to become more integrated with other technologies, such as machine learning, natural language processing, and computer vision. This integration will enable AI to perform tasks that are currently difficult or impossible for humans to do.
    
    2. Greater emphasis on ethical considerations: As AI becomes more integrated with other technologies, there will be a greater emphasis on ethical considerations. This will include issues such as bias, privacy, and transparency.
    
    3. Increased use of AI in healthcare:
    


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
    Generated text:  [Your Name], and I am a [type of character] with [number of years of experience] years of experience in the industry. I am a [occupation] who has [mention any unique traits or skills] and have a passion for [mention any passions or hobbies]. I enjoy [mention any hobbies or interests], and I love [mention any hobbies or interests]. I am a [mention any hobbies or interests] who have a [mention any interests or hobbies] and am constantly learning and growing as a professional. I am always looking for new challenges and opportunities to grow and improve. I am excited to work with you and look
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as "La République" and "La Haute Ville" in French, which is the largest city in Europe and the seventh-largest city in the world. Paris is home to numerous iconic landmarks such as Notre-Dame Cathedral, Eiffel Tower, and the Louvre Museum, as well as a diverse array of cultural institutions and neighborhoods throughout the city. The city has a rich history dating back to the French Revolution and is known for its lively and eclectic culture, food, and fashion. Paris is a major financial hub and international center for art, music, and literature, and is the birthplace of many
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by an increase in the complexity and sophistication of algorithms, as well as the emergence of new technologies such as deep learning, natural language processing, and quantum computing. We may also see an increase in the use of AI in areas such as healthcare, finance, transportation, and entertainment. However, it is also possible that AI will continue to be a subject of controversy and debate, with concerns about job displacement, ethical considerations, and the potential for AI to exacerbate existing social inequalities. Overall, the future of AI is uncertain, and the risks and benefits will depend on how we approach its development and deployment. As AI continues


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

     ____

     and

     I

    'm

     currently

     working

     at

     XYZ

     Corporation

    .

     I

    'm

     here

     to

     share

     some

     thoughts

     on

     the

     company

    's

     mission

    ,

     strategies

    ,

     and

     overall

     outlook

     for

     the

     future

    .

     What

     can

     you

     tell

     me

     about

     XYZ

     Corporation

     and

     what

     interests

     or

     expertise

     you

     bring

     to

     the

     table

    ?

     Let

    's

     get

     started

    !


    Sure

    ,

     I

    'd

     love

     to

     hear

     your

     thoughts

     on

     XYZ

     Corporation

     and

     what

     interests

     you

     in

     the

     field

     of

     strategy

    .

     What

     kind

     of

     background

     do

     you

     have

     that

     would

     make

     you

     an ideal

     fit

     for

     this

     role

    ?

     Let

    's

     get

     started

    !

     (

    I

     will

     give

     you

     a

     brief

     overview

     of

     XYZ

     Corporation

     and

     let

     you

     know

     what

     you

    're

     looking

     for

     in

     a

     career

     for

     this

     role

    .)

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     the

     city

     where

     the

     E

    iff

    el

     Tower

     stands

     tall

    .
    


    Paris

     is

     the

     capital

     of

     France

     and

     its

     historic

     center

     is

     home

     to

     the

     E

    iff

    el

     Tower

    ,

     the

     world

    's

     tallest

     and

     most

     famous

     structure

    .

     The

     city

     also

     has

     numerous

     museums

    ,

     including

     the

     Lou

    vre

    ,

     the

     Mus

    ée

     d

    '

    Or

    say

    ,

     and

     the

     Mus

    ée

     d

    '

    Art

     Moder

    ne

    .

     Paris

     is

     known

     for

     its

     vibrant

     culture

    ,

     delicious

     cuisine

    ,

     and

     rich

     history

    .

     It

    's

     a

     cultural

     hub

     that

    's

     popular

     with

     tourists

     and

     locals

     alike

    .

     Visitors

     to

     Paris

     can

     explore

     the

     city

    's

     many

     landmarks

    ,

     try

     delicious

     food

    ,

     and

     immer

    se

     themselves

     in

     its

     rich

     history

    .

     Paris

     is

     the

     largest

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     shaped

     by

     a

     number

     of

     factors

    ,

     including

     advances

     in

     technology

     and

     research

    ,

     changing

     societal

     and

     political

     norms

    ,

     and

     new

     challenges

     and

     opportunities

     presented

     by

     emerging

     technologies

     and

     industries

    .

     Here

     are

     some

     possible

     trends

     in

     AI

     that

     could

     emerge

     in

     the

     coming

     years

    :
    


    1

    .

     Increased

     focus

     on

     ethics

     and

     fairness

    :

     As

     AI

     is

     used

     in

     many

     industries

    ,

     including

     healthcare

    ,

     finance

    ,

     and

     transportation

    ,

     there

     will be

     increased

     scrutiny

     of

     how

     AI

     is

     developed

    ,

     deployed

    ,

     and

     used

    .

     Eth

    ical

     concerns

     about

     bias

    ,

     discrimination

    ,

     and

     privacy

     will

     become

     more

     prominent

    ,

     and

     there

     will

     be

     a

     greater

     emphasis

     on

     fairness

     and

     fairness

     in

     AI

    .
    


    2

    .

     More

     collaboration

     between

     researchers

     and

     industry

    



```python
llm.shutdown()
```
