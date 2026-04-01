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


    2026-04-01 16:55:41.577 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 16:55:41] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 16:55:41.577 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 16:55:41] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 16:55:41.577 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 16:55:41] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 16:55:41.577 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 16:55:41] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 16:55:41.578 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 16:55:41] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.65it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.64it/s]


    2026-04-01 16:55:43,964 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 16:55:43] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.61it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.61it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.61it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.61it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:03<00:08,  5.61it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:03<00:08,  5.61it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:03<00:08,  5.61it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:03<00:08,  5.61it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:08,  5.61it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 11.84it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 11.84it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 11.84it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 11.84it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 11.84it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 11.84it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 11.84it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 11.84it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 17.88it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 17.88it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 17.88it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 17.88it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 17.88it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 17.88it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 17.88it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:03<00:01, 23.38it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:03<00:01, 23.38it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:03<00:01, 23.38it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:03<00:01, 23.38it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:03<00:01, 23.38it/s]Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:03<00:01, 23.38it/s]

    Compiling num tokens (num_tokens=256):  53%|█████▎    | 31/58 [00:03<00:01, 23.38it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:03<00:00, 28.75it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:03<00:00, 28.75it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:03<00:00, 28.75it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:03<00:00, 28.75it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:03<00:00, 28.75it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:03<00:00, 28.75it/s]Compiling num tokens (num_tokens=160):  64%|██████▍   | 37/58 [00:03<00:00, 28.75it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:03<00:00, 34.09it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:03<00:00, 34.09it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:03<00:00, 34.09it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:03<00:00, 34.09it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:03<00:00, 34.09it/s] 

    Compiling num tokens (num_tokens=80):  74%|███████▍  | 43/58 [00:03<00:00, 34.09it/s]Compiling num tokens (num_tokens=64):  74%|███████▍  | 43/58 [00:03<00:00, 34.09it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 37.56it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 37.56it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 37.56it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 37.56it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 37.56it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 37.56it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 37.56it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 37.56it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:03<00:00, 37.56it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:03<00:00, 46.17it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:03<00:00, 46.17it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.49it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.02 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.99 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.99 GB):   3%|▎         | 2/58 [00:00<00:03, 18.58it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.99 GB):   3%|▎         | 2/58 [00:00<00:03, 18.58it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.99 GB):   3%|▎         | 2/58 [00:00<00:03, 18.58it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.99 GB):   3%|▎         | 2/58 [00:00<00:03, 18.58it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=118.99 GB):   9%|▊         | 5/58 [00:00<00:02, 21.35it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.98 GB):   9%|▊         | 5/58 [00:00<00:02, 21.35it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.98 GB):   9%|▊         | 5/58 [00:00<00:02, 21.35it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.98 GB):   9%|▊         | 5/58 [00:00<00:02, 21.35it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.98 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.89it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.98 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.89it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.98 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.89it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.97 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.89it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.97 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.89it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=118.97 GB):  21%|██        | 12/58 [00:00<00:01, 29.80it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.97 GB):  21%|██        | 12/58 [00:00<00:01, 29.80it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.97 GB):  21%|██        | 12/58 [00:00<00:01, 29.80it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.96 GB):  21%|██        | 12/58 [00:00<00:01, 29.80it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.96 GB):  21%|██        | 12/58 [00:00<00:01, 29.80it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.95 GB):  21%|██        | 12/58 [00:00<00:01, 29.80it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.95 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.01it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.95 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.01it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.95 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.01it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.94 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.01it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.92 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.01it/s]

    Capturing num tokens (num_tokens=960 avail_mem=118.94 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.01it/s] Capturing num tokens (num_tokens=960 avail_mem=118.94 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.72it/s]Capturing num tokens (num_tokens=896 avail_mem=118.93 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.72it/s]Capturing num tokens (num_tokens=832 avail_mem=118.93 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.72it/s]Capturing num tokens (num_tokens=768 avail_mem=118.93 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.72it/s]Capturing num tokens (num_tokens=704 avail_mem=118.92 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.72it/s]Capturing num tokens (num_tokens=640 avail_mem=118.92 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.72it/s]Capturing num tokens (num_tokens=640 avail_mem=118.92 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.19it/s]Capturing num tokens (num_tokens=576 avail_mem=118.92 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.19it/s]Capturing num tokens (num_tokens=512 avail_mem=118.91 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.19it/s]Capturing num tokens (num_tokens=480 avail_mem=118.92 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.19it/s]Capturing num tokens (num_tokens=448 avail_mem=118.92 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.19it/s]

    Capturing num tokens (num_tokens=416 avail_mem=118.92 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.19it/s]Capturing num tokens (num_tokens=416 avail_mem=118.92 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.83it/s]Capturing num tokens (num_tokens=384 avail_mem=118.92 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.83it/s]Capturing num tokens (num_tokens=352 avail_mem=118.88 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.83it/s]Capturing num tokens (num_tokens=320 avail_mem=118.88 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.83it/s]Capturing num tokens (num_tokens=288 avail_mem=118.87 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.83it/s]Capturing num tokens (num_tokens=256 avail_mem=118.87 GB):  55%|█████▌    | 32/58 [00:01<00:00, 41.83it/s]Capturing num tokens (num_tokens=256 avail_mem=118.87 GB):  64%|██████▍   | 37/58 [00:01<00:00, 39.30it/s]Capturing num tokens (num_tokens=240 avail_mem=118.87 GB):  64%|██████▍   | 37/58 [00:01<00:00, 39.30it/s]Capturing num tokens (num_tokens=224 avail_mem=118.86 GB):  64%|██████▍   | 37/58 [00:01<00:00, 39.30it/s]

    Capturing num tokens (num_tokens=208 avail_mem=118.86 GB):  64%|██████▍   | 37/58 [00:01<00:00, 39.30it/s]Capturing num tokens (num_tokens=192 avail_mem=118.86 GB):  64%|██████▍   | 37/58 [00:01<00:00, 39.30it/s]Capturing num tokens (num_tokens=176 avail_mem=118.86 GB):  64%|██████▍   | 37/58 [00:01<00:00, 39.30it/s]Capturing num tokens (num_tokens=176 avail_mem=118.86 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.16it/s]Capturing num tokens (num_tokens=160 avail_mem=118.85 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.16it/s]Capturing num tokens (num_tokens=144 avail_mem=118.85 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.16it/s]Capturing num tokens (num_tokens=128 avail_mem=118.85 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.16it/s]Capturing num tokens (num_tokens=112 avail_mem=118.84 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.16it/s]Capturing num tokens (num_tokens=96 avail_mem=118.84 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.16it/s] Capturing num tokens (num_tokens=96 avail_mem=118.84 GB):  81%|████████  | 47/58 [00:01<00:00, 41.85it/s]Capturing num tokens (num_tokens=80 avail_mem=118.84 GB):  81%|████████  | 47/58 [00:01<00:00, 41.85it/s]

    Capturing num tokens (num_tokens=64 avail_mem=118.83 GB):  81%|████████  | 47/58 [00:01<00:00, 41.85it/s]Capturing num tokens (num_tokens=48 avail_mem=118.83 GB):  81%|████████  | 47/58 [00:01<00:00, 41.85it/s]Capturing num tokens (num_tokens=32 avail_mem=118.82 GB):  81%|████████  | 47/58 [00:01<00:00, 41.85it/s]Capturing num tokens (num_tokens=28 avail_mem=118.82 GB):  81%|████████  | 47/58 [00:01<00:00, 41.85it/s]Capturing num tokens (num_tokens=28 avail_mem=118.82 GB):  90%|████████▉ | 52/58 [00:01<00:00, 41.29it/s]Capturing num tokens (num_tokens=24 avail_mem=118.82 GB):  90%|████████▉ | 52/58 [00:01<00:00, 41.29it/s]Capturing num tokens (num_tokens=20 avail_mem=118.81 GB):  90%|████████▉ | 52/58 [00:01<00:00, 41.29it/s]Capturing num tokens (num_tokens=16 avail_mem=118.81 GB):  90%|████████▉ | 52/58 [00:01<00:00, 41.29it/s]Capturing num tokens (num_tokens=12 avail_mem=118.81 GB):  90%|████████▉ | 52/58 [00:01<00:00, 41.29it/s]Capturing num tokens (num_tokens=8 avail_mem=118.80 GB):  90%|████████▉ | 52/58 [00:01<00:00, 41.29it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=118.80 GB):  98%|█████████▊| 57/58 [00:01<00:00, 42.08it/s]Capturing num tokens (num_tokens=4 avail_mem=118.80 GB):  98%|█████████▊| 57/58 [00:01<00:00, 42.08it/s]Capturing num tokens (num_tokens=4 avail_mem=118.80 GB): 100%|██████████| 58/58 [00:01<00:00, 37.90it/s]


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
    Generated text:  Jiahui Zhang, and I am an international student at the University of Michigan. I have been working at the University of Michigan for the past three years. My home country is Taiwan. I was born in 1999 in Taipei, Taiwan. I am a member of the Student Government Association (SGA) of the Chinese University of Hong Kong. Before coming to the United States, I have worked as a research assistant in the Department of Computational Mathematics and Data Science at the University of California, Los Angeles (UCLA). During the years I have been working in a research lab at UCLA, my work has been focused on
    ===============================
    Prompt: The president of the United States is
    Generated text:  a political leader of the country. The United States is a federal republic, a country where the powers of the president are divided between executive and legislative branches. The executive branch is run by the president, who may appoint federal judges. He also has the power to veto legislation. The legislative branch is the House of Representatives and the Senate. The president has the power to declare war and to raise and maintain an army. He also has the power to appoint federal judges and to sign bills into law. He may veto the bills of the other branch of government and the executive branch.
    
    Based on this passage, who is the president? Let's do
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, famous for its many fine museums and libraries. But the city has much to offer beyond its museums, too. With its cobblestone streets and quaint streets, the city has a heart. The city’s history is the history of the country, and Paris is the heart of the nation. It has a rich past and a unique history.
    As a result, Paris has many fascinating landmarks that you can enjoy while visiting Paris. The most famous landmarks are the Eiffel Tower, the Louvre Museum, and the Notre Dame Cathedral. They are some of the most famous landmarks of Paris and they are a must-see for any
    ===============================
    Prompt: The future of AI is
    Generated text:  on the horizon, and the technology is already changing the world in interesting ways. Here are some of the top trends you should keep an eye on as it evolves.
    AI, or artificial intelligence, refers to the simulation of human intelligence in machines that are designed to perform tasks that would normally require human intelligence. Machine learning, which is a subset of AI, is a type of AI that involves the creation of algorithms that can learn from data and improve their performance over time.
    AI has the potential to revolutionize many industries, from healthcare to finance to transportation. Here are some of the top trends you should keep an eye on as it evolves:
    


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [age] year old, [gender] and [occupation]. I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [age] year old, [gender] and [occupation]. I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a bustling metropolis with a rich cultural heritage and is a major tourist destination. The city is also known for its cuisine, including its famous croissants and its famous French fries. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly into one another. It is a city that is constantly evolving and changing, with new developments and attractions being added to the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn and adapt to human behavior and preferences. This could lead to more personalized and adaptive AI systems.
    
    2. Greater reliance on machine learning: Machine learning is likely to become more prevalent in AI, allowing machines to learn from data and improve their performance over time. This could lead to more efficient and effective AI systems.
    
    3. Increased focus on ethical considerations: As AI becomes more integrated with human intelligence, there will be a greater focus on ethical considerations. This could lead to more responsible and
    


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
    Generated text:  [Your Name], and I'm a [job title] with [number] years of experience in [industry]. I'm passionate about [why you're passionate about the industry]. I enjoy [what you do well], and I love [goals for the future]. How about you?
    Sure, I'd be happy to write a short, neutral self-introduction for a fictional character. Here's a sample:
    
    "Hello, my name is [Your Name], and I'm a [job title] with [number] years of experience in [industry]. I'm passionate about [why you're passionate about the industry]. I enjoy [what
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, the historical and cultural center of the country. Paris is known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also famous for its cuisine, music, and art. The city is known for its rich history, diverse culture, and modernity. Paris is a major tourist destination and one of the most important cities in the world. With its vibrant nightlife, art, and fashion scene, it is a popular destination for tourists and locals alike. The city is home to over 10 million people, making it one of the largest cities in the world. The
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to see continued growth and development, driven by a combination of advances in machine learning, deep learning, and hardware advancements. Here are some possible future trends:
    
    1. Increased focus on ethical AI: There is growing recognition of the need to address ethical issues surrounding AI, such as bias, privacy, and transparency. This will lead to more rigorous testing and regulation of AI systems.
    
    2. Integration with other technologies: AI will continue to be integrated into other technologies, such as autonomous vehicles, smart homes, and virtual assistants. This will lead to more seamless and intuitive interactions between humans and machines.
    
    3. Enhanced capabilities: AI is likely to


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

     am

     a

     [

    profession

     or

     type

     of

     work

    ]

     who

     has

     been

     working

     in

     [

    industry

    ]

     for

     [

    number

    ]

     years

    .

     I

     am

     passionate

     about

     [

    what

     I

     love

     to

     do

    ]

     and

     I

     have

     always

     been

     inspired

     by

     [

    person

     or

     thing

     that

     inspired

     me

    ].

     I

     am

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

     I

     am

     very

     open

    -minded

     and

     willing

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

     people

     from

     all

     over

     the

     world

     and

     I

     am

     always

     looking

     for

     ways

     to

     share

     my

     knowledge

     and

     experience

     with

     others

    .

     So

    ,

     if

     you

     have

     a

     question

     or

     a

     problem

     that

     you

     would

     like

     to

     discuss

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     Its

     history

     and

     culture

     can

     be

     traced

     back

     to

     ancient

     times

    ,

     and

     it

     is

     now

     a

     cosm

    opolitan

     city

     known

     for

     its

     rich

     history

    ,

     beautiful

     architecture

    ,

     and

     delicious

     cuisine

    .

     Paris

     is

     a

     popular

     tourist

     destination

    ,

     known

     for

     its

     many

     museums

    ,

     landmarks

    ,

     and

     lively

     nightlife

    .

     It

     is

     also

     known

     for

     its

     cultural

     events

    ,

     including

     the

     Op

    éra

    ,

     the

     Mou

    lin

     Rouge

    ,

     and

     the

     Notre

    -D

    ame

     Cathedral

    .

     With

     its

     beautiful

     views

     of

     the

     city

     and

     its

     rich

     history

    ,

     Paris

     is

     a

     must

    -

    visit

     destination

     for

     anyone

     visiting

     France

    .

     It

     is

     also

     home

     to

     the

     headquarters

     of

     many

     global

     companies

    ,

     including

     Google

     and

     Apple

    .

     Paris

     has

     been

     a

     part

     of

     French

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     continue

     to

     be

     shaped

     by

     rapid

     advancements

     in

     technology

    ,

     and

     a

     number

     of

     potential

     trends

     are

     likely

     to

     emerge

    .

     Here

     are

     some

     potential

     areas

     of

     future

     AI

     development

    :
    


    1

    .

     Aug

    mented

     intelligence

    :

     AI

     systems

     will

     become

     more

     integrated

     with

     the

     physical

     world

    ,

     enabling

     them

     to

     interact

     with

     people

     and

     devices

     in

     a

     more

     natural

     way

    .

     This

     could

     include

     things

     like

     smart

     home

     devices

    ,

     virtual

     assistants

    ,

     and

     even

     human

    -like

     AI

     agents

    .
    


    2

    .

     Autonomous

     vehicles

    :

     Self

    -driving

     cars

     will

     become

     more

     common

     and

     widely

     available

    ,

     and

     AI

     systems

     will

     play

     a

     key

     role

     in

     their

     design

     and

     operation

    .

     Autonomous

     vehicles

     could

     potentially

     reduce

     accidents

     and

     improve

     traffic

     flow

    ,

     while

     also

     making

     transportation

    



```python
llm.shutdown()
```
