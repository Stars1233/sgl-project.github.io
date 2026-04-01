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


    2026-04-01 21:03:50.823 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 21:03:50] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 21:03:50.824 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 21:03:50] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 21:03:50.824 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 21:03:50] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 21:03:50.824 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 21:03:50] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 21:03:50.824 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 21:03:50] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.02it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.01it/s]


    2026-04-01 21:03:53,487 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 21:03:53] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:05,  1.18s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:05,  1.18s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:05,  1.18s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.08it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.08it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.08it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:03<00:14,  3.59it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:03<00:14,  3.59it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:03<00:14,  3.59it/s]

    Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:03<00:14,  3.59it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:07,  6.35it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:07,  6.35it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:07,  6.35it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:07,  6.35it/s]Compiling num tokens (num_tokens=3072):  16%|█▌        | 9/58 [00:03<00:07,  6.35it/s]Compiling num tokens (num_tokens=2816):  16%|█▌        | 9/58 [00:03<00:07,  6.35it/s]Compiling num tokens (num_tokens=2560):  16%|█▌        | 9/58 [00:03<00:07,  6.35it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:03<00:03, 13.34it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:03<00:03, 13.34it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:03<00:03, 13.34it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:03<00:03, 13.34it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:03<00:03, 13.34it/s]Compiling num tokens (num_tokens=1280):  26%|██▌       | 15/58 [00:03<00:03, 13.34it/s]Compiling num tokens (num_tokens=1024):  26%|██▌       | 15/58 [00:03<00:03, 13.34it/s]

    Compiling num tokens (num_tokens=960):  26%|██▌       | 15/58 [00:03<00:03, 13.34it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:01, 22.18it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:01, 22.18it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:01, 22.18it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:01, 22.18it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:01, 22.18it/s]Compiling num tokens (num_tokens=640):  38%|███▊      | 22/58 [00:03<00:01, 22.18it/s]Compiling num tokens (num_tokens=576):  38%|███▊      | 22/58 [00:03<00:01, 22.18it/s]Compiling num tokens (num_tokens=512):  38%|███▊      | 22/58 [00:03<00:01, 22.18it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:03<00:00, 30.23it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:03<00:00, 30.23it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:03<00:00, 30.23it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:03<00:00, 30.23it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:03<00:00, 30.23it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:03<00:00, 30.23it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:03<00:00, 30.23it/s]

    Compiling num tokens (num_tokens=288):  50%|█████     | 29/58 [00:03<00:00, 30.23it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:03<00:00, 29.55it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:03<00:00, 29.55it/s]

    Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:03<00:00, 29.55it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:03<00:00, 29.55it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:03<00:00, 29.55it/s]Compiling num tokens (num_tokens=192):  62%|██████▏   | 36/58 [00:03<00:00, 29.55it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:03<00:00, 32.56it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:03<00:00, 32.56it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:03<00:00, 32.56it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:03<00:00, 32.56it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:03<00:00, 32.56it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:03<00:00, 32.56it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:03<00:00, 32.56it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:03<00:00, 37.73it/s]

    Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:03<00:00, 37.73it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:04<00:00, 37.73it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:04<00:00, 37.73it/s]Compiling num tokens (num_tokens=20):  81%|████████  | 47/58 [00:04<00:00, 37.73it/s]Compiling num tokens (num_tokens=16):  81%|████████  | 47/58 [00:04<00:00, 37.73it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:04<00:00, 46.93it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:04<00:00, 46.93it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:04<00:00, 46.93it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:04<00:00, 46.93it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 14.25it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.80 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.77 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.77 GB):   3%|▎         | 2/58 [00:00<00:03, 15.87it/s]Capturing num tokens (num_tokens=7168 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:03, 15.87it/s]Capturing num tokens (num_tokens=6656 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:03, 15.87it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=76.76 GB):   7%|▋         | 4/58 [00:00<00:03, 17.77it/s]Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   7%|▋         | 4/58 [00:00<00:03, 17.77it/s]Capturing num tokens (num_tokens=5632 avail_mem=76.76 GB):   7%|▋         | 4/58 [00:00<00:03, 17.77it/s]Capturing num tokens (num_tokens=5120 avail_mem=76.76 GB):   7%|▋         | 4/58 [00:00<00:03, 17.77it/s]Capturing num tokens (num_tokens=5120 avail_mem=76.76 GB):  12%|█▏        | 7/58 [00:00<00:02, 22.36it/s]Capturing num tokens (num_tokens=4608 avail_mem=76.76 GB):  12%|█▏        | 7/58 [00:00<00:02, 22.36it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.76 GB):  12%|█▏        | 7/58 [00:00<00:02, 22.36it/s]Capturing num tokens (num_tokens=3840 avail_mem=76.75 GB):  12%|█▏        | 7/58 [00:00<00:02, 22.36it/s]Capturing num tokens (num_tokens=3584 avail_mem=76.75 GB):  12%|█▏        | 7/58 [00:00<00:02, 22.36it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=76.75 GB):  19%|█▉        | 11/58 [00:00<00:01, 28.20it/s]Capturing num tokens (num_tokens=3328 avail_mem=76.74 GB):  19%|█▉        | 11/58 [00:00<00:01, 28.20it/s]Capturing num tokens (num_tokens=3072 avail_mem=76.74 GB):  19%|█▉        | 11/58 [00:00<00:01, 28.20it/s]Capturing num tokens (num_tokens=2816 avail_mem=76.74 GB):  19%|█▉        | 11/58 [00:00<00:01, 28.20it/s]Capturing num tokens (num_tokens=2560 avail_mem=76.73 GB):  19%|█▉        | 11/58 [00:00<00:01, 28.20it/s]Capturing num tokens (num_tokens=2304 avail_mem=76.73 GB):  19%|█▉        | 11/58 [00:00<00:01, 28.20it/s]Capturing num tokens (num_tokens=2304 avail_mem=76.73 GB):  28%|██▊       | 16/58 [00:00<00:01, 34.57it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.73 GB):  28%|██▊       | 16/58 [00:00<00:01, 34.57it/s]Capturing num tokens (num_tokens=1792 avail_mem=76.72 GB):  28%|██▊       | 16/58 [00:00<00:01, 34.57it/s]Capturing num tokens (num_tokens=1536 avail_mem=76.72 GB):  28%|██▊       | 16/58 [00:00<00:01, 34.57it/s]Capturing num tokens (num_tokens=1280 avail_mem=76.72 GB):  28%|██▊       | 16/58 [00:00<00:01, 34.57it/s]Capturing num tokens (num_tokens=1024 avail_mem=76.70 GB):  28%|██▊       | 16/58 [00:00<00:01, 34.57it/s]

    Capturing num tokens (num_tokens=960 avail_mem=76.71 GB):  28%|██▊       | 16/58 [00:00<00:01, 34.57it/s] Capturing num tokens (num_tokens=960 avail_mem=76.71 GB):  38%|███▊      | 22/58 [00:00<00:00, 40.90it/s]Capturing num tokens (num_tokens=896 avail_mem=76.71 GB):  38%|███▊      | 22/58 [00:00<00:00, 40.90it/s]Capturing num tokens (num_tokens=832 avail_mem=76.70 GB):  38%|███▊      | 22/58 [00:00<00:00, 40.90it/s]Capturing num tokens (num_tokens=768 avail_mem=76.70 GB):  38%|███▊      | 22/58 [00:00<00:00, 40.90it/s]Capturing num tokens (num_tokens=704 avail_mem=76.70 GB):  38%|███▊      | 22/58 [00:00<00:00, 40.90it/s]Capturing num tokens (num_tokens=640 avail_mem=76.69 GB):  38%|███▊      | 22/58 [00:00<00:00, 40.90it/s]Capturing num tokens (num_tokens=640 avail_mem=76.69 GB):  47%|████▋     | 27/58 [00:00<00:00, 43.34it/s]Capturing num tokens (num_tokens=576 avail_mem=76.69 GB):  47%|████▋     | 27/58 [00:00<00:00, 43.34it/s]

    Capturing num tokens (num_tokens=512 avail_mem=76.68 GB):  47%|████▋     | 27/58 [00:00<00:00, 43.34it/s]Capturing num tokens (num_tokens=480 avail_mem=76.70 GB):  47%|████▋     | 27/58 [00:00<00:00, 43.34it/s]Capturing num tokens (num_tokens=448 avail_mem=76.70 GB):  47%|████▋     | 27/58 [00:00<00:00, 43.34it/s]Capturing num tokens (num_tokens=416 avail_mem=76.69 GB):  47%|████▋     | 27/58 [00:00<00:00, 43.34it/s]Capturing num tokens (num_tokens=416 avail_mem=76.69 GB):  55%|█████▌    | 32/58 [00:00<00:00, 33.88it/s]Capturing num tokens (num_tokens=384 avail_mem=76.69 GB):  55%|█████▌    | 32/58 [00:00<00:00, 33.88it/s]Capturing num tokens (num_tokens=352 avail_mem=76.69 GB):  55%|█████▌    | 32/58 [00:01<00:00, 33.88it/s]Capturing num tokens (num_tokens=320 avail_mem=76.68 GB):  55%|█████▌    | 32/58 [00:01<00:00, 33.88it/s]Capturing num tokens (num_tokens=288 avail_mem=76.68 GB):  55%|█████▌    | 32/58 [00:01<00:00, 33.88it/s]Capturing num tokens (num_tokens=256 avail_mem=76.68 GB):  55%|█████▌    | 32/58 [00:01<00:00, 33.88it/s]

    Capturing num tokens (num_tokens=256 avail_mem=76.68 GB):  64%|██████▍   | 37/58 [00:01<00:00, 37.77it/s]Capturing num tokens (num_tokens=240 avail_mem=76.68 GB):  64%|██████▍   | 37/58 [00:01<00:00, 37.77it/s]Capturing num tokens (num_tokens=224 avail_mem=76.67 GB):  64%|██████▍   | 37/58 [00:01<00:00, 37.77it/s]Capturing num tokens (num_tokens=208 avail_mem=76.67 GB):  64%|██████▍   | 37/58 [00:01<00:00, 37.77it/s]Capturing num tokens (num_tokens=192 avail_mem=76.67 GB):  64%|██████▍   | 37/58 [00:01<00:00, 37.77it/s]Capturing num tokens (num_tokens=176 avail_mem=76.66 GB):  64%|██████▍   | 37/58 [00:01<00:00, 37.77it/s]Capturing num tokens (num_tokens=160 avail_mem=76.66 GB):  64%|██████▍   | 37/58 [00:01<00:00, 37.77it/s]Capturing num tokens (num_tokens=160 avail_mem=76.66 GB):  74%|███████▍  | 43/58 [00:01<00:00, 41.58it/s]Capturing num tokens (num_tokens=144 avail_mem=76.66 GB):  74%|███████▍  | 43/58 [00:01<00:00, 41.58it/s]Capturing num tokens (num_tokens=128 avail_mem=76.65 GB):  74%|███████▍  | 43/58 [00:01<00:00, 41.58it/s]Capturing num tokens (num_tokens=112 avail_mem=76.65 GB):  74%|███████▍  | 43/58 [00:01<00:00, 41.58it/s]Capturing num tokens (num_tokens=96 avail_mem=76.65 GB):  74%|███████▍  | 43/58 [00:01<00:00, 41.58it/s] Capturing num tokens (num_tokens=80 avail_mem=76.64 GB):  74%|███████▍  | 43/58 [00:01<00:00, 41.58it/s]

    Capturing num tokens (num_tokens=64 avail_mem=76.64 GB):  74%|███████▍  | 43/58 [00:01<00:00, 41.58it/s]Capturing num tokens (num_tokens=64 avail_mem=76.64 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.23it/s]Capturing num tokens (num_tokens=48 avail_mem=76.64 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.23it/s]Capturing num tokens (num_tokens=32 avail_mem=76.64 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.23it/s]Capturing num tokens (num_tokens=28 avail_mem=76.63 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.23it/s]Capturing num tokens (num_tokens=24 avail_mem=76.63 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.23it/s]Capturing num tokens (num_tokens=20 avail_mem=76.62 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.23it/s]Capturing num tokens (num_tokens=16 avail_mem=76.62 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.23it/s]Capturing num tokens (num_tokens=16 avail_mem=76.62 GB):  95%|█████████▍| 55/58 [00:01<00:00, 46.19it/s]Capturing num tokens (num_tokens=12 avail_mem=76.62 GB):  95%|█████████▍| 55/58 [00:01<00:00, 46.19it/s]Capturing num tokens (num_tokens=8 avail_mem=76.61 GB):  95%|█████████▍| 55/58 [00:01<00:00, 46.19it/s] Capturing num tokens (num_tokens=4 avail_mem=76.61 GB):  95%|█████████▍| 55/58 [00:01<00:00, 46.19it/s]Capturing num tokens (num_tokens=4 avail_mem=76.61 GB): 100%|██████████| 58/58 [00:01<00:00, 38.85it/s]


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
    Generated text:  Jessica. I am a 19 year old girl. I have a piggy bank. I have an allowance. I like to play with my toys, and I love to eat ice cream. I like to read books, and I love to travel.
    What is your favorite thing to do? My favorite thing to do is play with my toys. I love to play with my toys because they are fun and I can play with them all day long. I also love to eat ice cream, but I do not like to eat ice cream a lot. I like to read books, but I do not like to read a lot.
    ===============================
    Prompt: The president of the United States is
    Generated text:  inaugurated on January 20, 2023. On January 20, 2022, the president's inauguration date was what day of the week? To determine the day of the week for the inauguration of the president on January 20, 2022, we need to calculate the number of days between January 1, 2022, and January 20, 2022, and then find the corresponding day of the week.
    
    1. Calculate the number of days from January 1, 2022, to January 20,
    ===============================
    Prompt: The capital of France is
    Generated text:  ________. A. Paris B. Rome C. Madrid D. London
    Paris
    
    Circling the globe in a single day is called _____. A. trip B. holiday C. sightseeing D. adventure
    trip
    
    Which of the following statements about China is NOT true? A. China is the largest country in the world. B. China is the largest country in Asia. C. China is the most populous country in the world. D. China is the only country in the world that is both a country and an island.
    China is the only country in the world that is both a country and an island.
    
    When the
    ===============================
    Prompt: The future of AI is
    Generated text:  not yet clear, but there are two broad paths that it is moving in. The first path is an artificial intelligence that is self-improving. The second path is an artificial intelligence that is interacting with humans. Which path will AI be moving towards?
    
    The future of AI is likely to be a hybrid path, combining elements of both self-improving AI and human-AI interaction. Here are some potential paths that AI might take:
    
    1. Self-improving AI: In the future, AI will continue to advance in sophistication and autonomy. Self-improving AI will be able to learn and adapt to new situations, making it


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a cultural and economic center with a rich history dating back to the Roman Empire and the French Revolution. It is a major transportation hub and a popular tourist destination. The city is known for its cuisine, fashion, and music, and is home to many famous artists and writers. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly. It is a city of art,
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends, including:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior and experiences. This could lead to more natural and intuitive interactions between humans and machines.
    
    2. Enhanced ethical considerations: As AI becomes more integrated with human intelligence, there will be increased scrutiny of its ethical implications. This could lead to more stringent regulations and guidelines for AI development and deployment.
    
    3. Increased reliance on AI for decision-making: AI is likely to become more integrated into decision-making processes, allowing machines to make more informed and accurate decisions
    


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
    Generated text:  [name] and I am a writer by day and a pizza lover by night. I have always been fascinated by storytelling and have had the opportunity to work with characters who have been as varied and diverse as my own experiences. My most notable achievement so far has been my book [book title], a coming-of-age story that I hope to turn into a film. I am not afraid to take risks and think outside of the box, and I am excited to continue to push the boundaries of storytelling. Thanks for asking! What's your favorite genre to write in? To be honest, I'm not a big fan of any one genre,
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known for its rich history, vibrant culture, and iconic landmarks such as the Eiffel Tower and the Louvre Museum. It is a major transportation hub, with airports and rail connections to various parts of the country. Paris is a popular tourist destination, with many visitors from around the world visiting the city during its annual tourism season. The city is home to numerous museums, museums, and theaters, and is known for its art, architecture, and food. It is also a leading center for science, education, and the arts. France's capital is a beautiful and fascinating city, with a rich history and a vibrant culture that
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be shaped by several key trends:
    
    1. Increased integration with human cognition: AI is becoming more closely integrated with human cognition, enabling machines to learn from and improve their performance. This could lead to more intelligent and adaptive systems that can learn from human behavior and experience.
    
    2. Greater reliance on large datasets: AI is increasingly being used to process and analyze large amounts of data, which could lead to more accurate and effective predictions and insights.
    
    3. Increased use of machine learning: Machine learning is becoming more prevalent, allowing AI to learn from data on its own and make more accurate predictions and decisions.
    
    4. Enhanced privacy and security:


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

    First

     Name

    ]

     and

     I

    'm

     a

     [

    Career

    /

    Job

     Title

    ]

     at

     [

    Company

     Name

    ].

     I

    'm

     currently

     [

    Job

     Title

    ]

     at

     [

    Company

     Name

    ]

     and

     I

    'm

     very

     excited

     to

     be

     here

    .

     I

    'm

     here

     to

     share

     my

     passion

     for

     [

    Your

     Passion

    /

    Interest

    ]

     with

     you

    .

     Feel

     free

     to

     ask

     me

     anything

     and

     I

    'll

     be

     happy

     to

     answer

    .

     Please

     let

     me

     know

     if

     you

     have

     any

     questions

    .

     
    


    My

     journey

     to

     this

     position

     has

     been

     challenging

    ,

     but

     I

    've

     learned

     a

     lot

     through

     my

     experiences

    .

     I

    'm

     always

     looking

     for

     ways

     to

     improve

     and

     learn

    .

     Please

     let

     me

     know

     if

     you

     have

     any

     questions

     or

     concerns

    .

     


    I

    'm

     happy

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     located

     in

     the

     northern

     region

     of

     the

     country

    ,

     on

     the

     Se

    ine

     River

    .

     It

     serves

     as

     the

     seat

     of

     government

     for

     the

     French

     Republic

     and

     is

     home

     to

     the

     French

     language

    ,

     the

     Lou

    vre

     Museum

    ,

     and

     other

     cultural

     and

     historical

     landmarks

    .

     Paris

     is

     known

     for

     its

     iconic

     landmarks

    ,

     such

     as

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

    ,

     and

     is

     a

     major

     tourist

     destination

     for

     visitors

     from

     around

     the

     world

    .

     The

     French

     language

     is

     spoken

     by

     millions

     of

     people

     and

     is

     an

     official

     language

     of

     France

    ,

     while

     French

     cuisine

     and

     wine

     are

     highly

     regarded

     worldwide

    .

     Paris

     is

     also

     a

     major

     center

     for

     business

    ,

     finance

    ,

     and

     art

    ,

     and

     has

     played

     a

     significant

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     a

     variety

     of

     new

     trends

     and

     developments

    ,

     including

    :
    


    1

    .

     Enhanced

     Natural

     Language

     Processing

    :

     AI

     will

     continue

     to

     improve

     its

     ability

     to

     understand

     and

     generate

     human

     language

    ,

     leading

     to

     more

     natural

     and

     efficient

     communication

    .

     This

     will

     likely

     lead

     to

     the

     development

     of

     more

     convers

    ational

     AI

     systems

    ,

     which

     will

     enable

     more

     effective

     communication

     with

     customers

     and

     other

     users

    .
    


    2

    .

     Self

    -

    Driving

     Cars

    :

     AI

     will

     continue

     to

     improve

     its

     ability

     to

     learn

     and

     adapt

    ,

     leading

     to

     the

     development

     of

     self

    -driving

     cars

     that

     are

     more

     capable

     of

     navigating

     complex

     road

     environments

     and

     other

     factors

     that

     make

     human

     driving

     difficult

    .
    


    3

    .

     Artificial

     Intelligence

     in

     Healthcare

    :

     AI

     will

     continue

     to

     be

     used

    



```python
llm.shutdown()
```
