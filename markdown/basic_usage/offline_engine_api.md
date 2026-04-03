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


    2026-04-03 19:47:24.507 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 19:47:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 19:47:24.507 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 19:47:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 19:47:24.507 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 19:47:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 19:47:24.507 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 19:47:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 19:47:24.507 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 19:47:24] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.81it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.80it/s]


    2026-04-03 19:47:26,958 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 19:47:26] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.62it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.62it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.62it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.62it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:03<00:08,  5.62it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:03<00:08,  5.62it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:03<00:08,  5.62it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:03<00:08,  5.62it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:08,  5.62it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 11.89it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 11.89it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 11.89it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 11.89it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 11.89it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 11.89it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 11.89it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 11.89it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 17.83it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 17.83it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 17.83it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 17.83it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 17.83it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 17.83it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 17.83it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:03<00:01, 23.35it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:03<00:01, 23.35it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:03<00:01, 23.35it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:03<00:01, 23.35it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:03<00:01, 23.35it/s]Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:03<00:01, 23.35it/s]

    Compiling num tokens (num_tokens=256):  53%|█████▎    | 31/58 [00:03<00:01, 23.35it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:03<00:00, 28.82it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:03<00:00, 28.82it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:03<00:00, 28.82it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:03<00:00, 28.82it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:03<00:00, 28.82it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:03<00:00, 28.82it/s]Compiling num tokens (num_tokens=160):  64%|██████▍   | 37/58 [00:03<00:00, 28.82it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:03<00:00, 33.76it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:03<00:00, 33.76it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:03<00:00, 33.76it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:03<00:00, 33.76it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:03<00:00, 33.76it/s] 

    Compiling num tokens (num_tokens=80):  74%|███████▍  | 43/58 [00:03<00:00, 33.76it/s]Compiling num tokens (num_tokens=64):  74%|███████▍  | 43/58 [00:03<00:00, 33.76it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 36.98it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 36.98it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 36.98it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 36.98it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 36.98it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 36.98it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 36.98it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 36.98it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:03<00:00, 43.85it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:03<00:00, 43.85it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:03<00:00, 43.85it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.42it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.05 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.02 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:03, 18.22it/s]Capturing num tokens (num_tokens=7168 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:03, 18.22it/s]Capturing num tokens (num_tokens=6656 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:03, 18.22it/s]Capturing num tokens (num_tokens=6144 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:03, 18.22it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=119.02 GB):   9%|▊         | 5/58 [00:00<00:02, 21.32it/s]Capturing num tokens (num_tokens=5632 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:00<00:02, 21.32it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.98 GB):   9%|▊         | 5/58 [00:00<00:02, 21.32it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.98 GB):   9%|▊         | 5/58 [00:00<00:02, 21.32it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.98 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.16it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.98 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.16it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.98 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.16it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=118.97 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.16it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.97 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.03it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.97 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.03it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.97 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.03it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.97 GB):  19%|█▉        | 11/58 [00:00<00:02, 18.03it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.97 GB):  24%|██▍       | 14/58 [00:00<00:02, 20.87it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.96 GB):  24%|██▍       | 14/58 [00:00<00:02, 20.87it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.96 GB):  24%|██▍       | 14/58 [00:00<00:02, 20.87it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.95 GB):  24%|██▍       | 14/58 [00:00<00:02, 20.87it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=118.95 GB):  24%|██▍       | 14/58 [00:00<00:02, 20.87it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.95 GB):  24%|██▍       | 14/58 [00:00<00:02, 20.87it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.95 GB):  33%|███▎      | 19/58 [00:00<00:01, 27.13it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.94 GB):  33%|███▎      | 19/58 [00:00<00:01, 27.13it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.92 GB):  33%|███▎      | 19/58 [00:00<00:01, 27.13it/s]Capturing num tokens (num_tokens=960 avail_mem=118.94 GB):  33%|███▎      | 19/58 [00:00<00:01, 27.13it/s] Capturing num tokens (num_tokens=896 avail_mem=118.93 GB):  33%|███▎      | 19/58 [00:00<00:01, 27.13it/s]Capturing num tokens (num_tokens=832 avail_mem=118.93 GB):  33%|███▎      | 19/58 [00:00<00:01, 27.13it/s]Capturing num tokens (num_tokens=832 avail_mem=118.93 GB):  41%|████▏     | 24/58 [00:00<00:01, 31.67it/s]Capturing num tokens (num_tokens=768 avail_mem=118.93 GB):  41%|████▏     | 24/58 [00:00<00:01, 31.67it/s]Capturing num tokens (num_tokens=704 avail_mem=118.92 GB):  41%|████▏     | 24/58 [00:00<00:01, 31.67it/s]

    Capturing num tokens (num_tokens=640 avail_mem=118.92 GB):  41%|████▏     | 24/58 [00:00<00:01, 31.67it/s]Capturing num tokens (num_tokens=576 avail_mem=118.92 GB):  41%|████▏     | 24/58 [00:00<00:01, 31.67it/s]Capturing num tokens (num_tokens=512 avail_mem=118.91 GB):  41%|████▏     | 24/58 [00:01<00:01, 31.67it/s]Capturing num tokens (num_tokens=512 avail_mem=118.91 GB):  50%|█████     | 29/58 [00:01<00:00, 34.63it/s]Capturing num tokens (num_tokens=480 avail_mem=118.92 GB):  50%|█████     | 29/58 [00:01<00:00, 34.63it/s]Capturing num tokens (num_tokens=448 avail_mem=118.92 GB):  50%|█████     | 29/58 [00:01<00:00, 34.63it/s]Capturing num tokens (num_tokens=416 avail_mem=118.92 GB):  50%|█████     | 29/58 [00:01<00:00, 34.63it/s]Capturing num tokens (num_tokens=384 avail_mem=118.92 GB):  50%|█████     | 29/58 [00:01<00:00, 34.63it/s]Capturing num tokens (num_tokens=352 avail_mem=118.91 GB):  50%|█████     | 29/58 [00:01<00:00, 34.63it/s]Capturing num tokens (num_tokens=352 avail_mem=118.91 GB):  59%|█████▊    | 34/58 [00:01<00:00, 36.82it/s]Capturing num tokens (num_tokens=320 avail_mem=118.91 GB):  59%|█████▊    | 34/58 [00:01<00:00, 36.82it/s]

    Capturing num tokens (num_tokens=288 avail_mem=118.90 GB):  59%|█████▊    | 34/58 [00:01<00:00, 36.82it/s]Capturing num tokens (num_tokens=256 avail_mem=118.90 GB):  59%|█████▊    | 34/58 [00:01<00:00, 36.82it/s]Capturing num tokens (num_tokens=240 avail_mem=118.90 GB):  59%|█████▊    | 34/58 [00:01<00:00, 36.82it/s]Capturing num tokens (num_tokens=224 avail_mem=118.90 GB):  59%|█████▊    | 34/58 [00:01<00:00, 36.82it/s]Capturing num tokens (num_tokens=224 avail_mem=118.90 GB):  67%|██████▋   | 39/58 [00:01<00:00, 38.53it/s]Capturing num tokens (num_tokens=208 avail_mem=118.89 GB):  67%|██████▋   | 39/58 [00:01<00:00, 38.53it/s]Capturing num tokens (num_tokens=192 avail_mem=118.89 GB):  67%|██████▋   | 39/58 [00:01<00:00, 38.53it/s]Capturing num tokens (num_tokens=176 avail_mem=118.89 GB):  67%|██████▋   | 39/58 [00:01<00:00, 38.53it/s]Capturing num tokens (num_tokens=160 avail_mem=118.85 GB):  67%|██████▋   | 39/58 [00:01<00:00, 38.53it/s]Capturing num tokens (num_tokens=144 avail_mem=118.85 GB):  67%|██████▋   | 39/58 [00:01<00:00, 38.53it/s]

    Capturing num tokens (num_tokens=144 avail_mem=118.85 GB):  76%|███████▌  | 44/58 [00:01<00:00, 39.72it/s]Capturing num tokens (num_tokens=128 avail_mem=118.85 GB):  76%|███████▌  | 44/58 [00:01<00:00, 39.72it/s]Capturing num tokens (num_tokens=112 avail_mem=118.84 GB):  76%|███████▌  | 44/58 [00:01<00:00, 39.72it/s]Capturing num tokens (num_tokens=96 avail_mem=118.84 GB):  76%|███████▌  | 44/58 [00:01<00:00, 39.72it/s] Capturing num tokens (num_tokens=80 avail_mem=118.84 GB):  76%|███████▌  | 44/58 [00:01<00:00, 39.72it/s]Capturing num tokens (num_tokens=64 avail_mem=118.83 GB):  76%|███████▌  | 44/58 [00:01<00:00, 39.72it/s]Capturing num tokens (num_tokens=64 avail_mem=118.83 GB):  84%|████████▍ | 49/58 [00:01<00:00, 40.23it/s]Capturing num tokens (num_tokens=48 avail_mem=118.83 GB):  84%|████████▍ | 49/58 [00:01<00:00, 40.23it/s]Capturing num tokens (num_tokens=32 avail_mem=118.82 GB):  84%|████████▍ | 49/58 [00:01<00:00, 40.23it/s]Capturing num tokens (num_tokens=28 avail_mem=118.82 GB):  84%|████████▍ | 49/58 [00:01<00:00, 40.23it/s]Capturing num tokens (num_tokens=24 avail_mem=118.82 GB):  84%|████████▍ | 49/58 [00:01<00:00, 40.23it/s]

    Capturing num tokens (num_tokens=20 avail_mem=118.81 GB):  84%|████████▍ | 49/58 [00:01<00:00, 40.23it/s]Capturing num tokens (num_tokens=20 avail_mem=118.81 GB):  93%|█████████▎| 54/58 [00:01<00:00, 39.59it/s]Capturing num tokens (num_tokens=16 avail_mem=118.81 GB):  93%|█████████▎| 54/58 [00:01<00:00, 39.59it/s]Capturing num tokens (num_tokens=12 avail_mem=118.81 GB):  93%|█████████▎| 54/58 [00:01<00:00, 39.59it/s]Capturing num tokens (num_tokens=8 avail_mem=118.80 GB):  93%|█████████▎| 54/58 [00:01<00:00, 39.59it/s] Capturing num tokens (num_tokens=4 avail_mem=118.80 GB):  93%|█████████▎| 54/58 [00:01<00:00, 39.59it/s]

    Capturing num tokens (num_tokens=4 avail_mem=118.80 GB): 100%|██████████| 58/58 [00:01<00:00, 31.64it/s]Capturing num tokens (num_tokens=4 avail_mem=118.80 GB): 100%|██████████| 58/58 [00:01<00:00, 31.46it/s]


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
    Generated text:  Sheryll, and I am a first-year undergraduate student at the University of Michigan. My major is psychology, and I am majoring in neuroscience. I am very interested in the role of social interaction in human behavior and mental health. I would love to start a PhD in psychology, and am currently applying for my first two graduate school programs.
    This week we are going to have a quick look at the concepts of correlation, regression, and hypothesis testing in Statistics. We will look at how we can use these concepts to answer questions about relationships between two variables. We will begin with the concept of correlation, then we will look at how
    ===============================
    Prompt: The president of the United States is
    Generated text:  a citizen of the United States, and therefore, a citizen of the United States is a citizen of the United States.
    
    To what category does this reasoning belong? This reasoning is an example of **instance-based reasoning**. The type of reasoning involved here is called **instance-based reasoning**. This is a form of reasoning where a conclusion is drawn based on a specific instance or example, using general rules or principles to determine the correctness of a conclusion. In this case, the conclusion drawn is that all citizens of the United States are citizens of the United States, which is a specific instance of the general principle that all citizens of the United States
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It is the largest city in Europe and is the most populous (as of 2014) and the oldest city in Europe. The population is 2.3 million. As of 2014, the population of Paris is estimated to be 2.3 million.
    Is there an answer to this question (If it cannot be answered, return "Unanswerable"). Unanswerable.
    ===============================
    Prompt: The future of AI is
    Generated text:  in the hands of those who understand it.
    
    At the Drones of Tomorrow Conference, Cisco Systems, the world’s leading provider of innovation and technology solutions for drones, will be the keynote speaker at the conference on July 11, 2018. As part of the conference, Cisco will host a networking forum to explore the future of AI.
    
    One of the hottest topics to be covered is the importance of data in the creation of a predictive analytics model. In this webinar, Cisco Systems will take a deep dive into this area to discover the key roles of data, and how AI can revolutionize the way we see how data


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


    Generated text:  [Name] and I'm a [occupation] who has been working in [industry] for [number] years. I'm passionate about [reason for interest] and I'm always looking for ways to [action or goal]. I'm always eager to learn and grow, and I'm always open to new experiences and challenges. I'm a [character trait] and I'm always ready to help others and make a positive impact. I'm [character trait] and I'm always looking for ways to [action or goal]. I'm a [character trait] and I'm always ready to help others and make a positive impact. I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a bustling city with a rich cultural heritage and is a major economic and political center in Europe. It is also known for its fashion industry, with Paris Fashion Week being one of the largest in the world. The city is also home to the French Riviera, a popular tourist destination known for its beaches, luxury resorts, and luxury hotels. Paris is a city that is a must-visit for
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: As AI becomes more advanced, it is likely to become more integrated with human intelligence, allowing it to learn and adapt to new situations and tasks. This could lead to more efficient and effective AI systems that can perform tasks that are currently beyond the capabilities of humans.
    
    2. Enhanced ethical considerations: As AI becomes more advanced, there will be increased scrutiny of its ethical implications. This could lead to more stringent regulations and guidelines for AI development and use, as well as increased public awareness of the potential risks and benefits of AI.
    
    3. Greater reliance on AI for
    


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
    Generated text:  [Your Name], and I am a [Type of Character] [Your Type of Character, such as hero, villain, etc.]. I am [Your Profession or Occupation], and I am known for my [Your Strength or Influence in the world]. I have always been [Your Character Trait, such as intelligent, brave, kind, etc.]. I have always been [Your Passion or Purpose, such as helping others, making things better, etc.]. I am [Your Age, if applicable] years old, and [Your Nationality, if applicable]. I am from [Your Place of Origin, if applicable]. I am
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known for its beautiful architecture, iconic landmarks such as the Eiffel Tower, and its role in the French Revolution and the French language. It is also a major center for finance, business, and media, and is an important cultural and historical site in the world. Paris's famous landmarks and architecture include the Louvre Museum, Notre-Dame Cathedral, and the Arc de Triomphe. It's also the birthplace of French President Emmanuel Macron, who served as the first President of France from 2017 to 2022. Paris has a long and rich history dating back to the Roman Empire and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  vast and unpredictable, and it is difficult to predict exactly where it will lead. However, here are some possible trends that are likely to continue:
    
    1. Increased automation: AI is expected to become more automated in the future, leading to more efficient and cost-effective systems. This could include things like self-driving cars, robotic arms, and other automation in industry.
    
    2. AI in healthcare: AI is already being used in healthcare to help diagnose and treat diseases, and it has the potential to become even more advanced in the future. AI could help with predicting and preventing disease, improving patient outcomes, and even developing new medical treatments.
    
    3.


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

     am

     an

     [

    job

     title

     or

     profession

    ]

     who

     enjoys

     [

    most

     recent

     hobby

     or

     passion

    ].

     I

     have

     a

     love

     for

     [

    more

     recent

     passion

     or

     hobby

    ]

     and

     I

     am

     always

     looking

     for

     new

     ways

     to

     [

    future

     goal

     or

     challenge

    ].

     What

     brings

     you

     to

     this

     profession

     and

     why

     do

     you

     enjoy

     it

     so

     much

    ?

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

     [

    Insert

     any

     additional

     information

     or

     details

     about

     the

     character

     that

     you

     feel

     would

     add

     to

     the

     introduction

    ]

     "

    Hello

    ,

     my

     name

     is

     [

    Name

    ]

     and

     I

     am

     an

     [

    job

     title

     or

     profession

    ]

     who

     enjoys

     [

    most

     recent

     hobby

     or

     passion

    ].

     I

     have

     a

     love

     for

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     It

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

     numerous

     landmarks

     that

     attract

     tourists

     from

     around

     the

     world

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

     Notre

    -D

    ame

     Cathedral

    ,

     the

     E

    iff

    el

     Tower

    ,

     and

     the

     Lou

    vre

     Museum

    .

     It

     also

     has

     a

     thriving

     cultural

     scene

    ,

     with

     many

     artists

    ,

     writers

    ,

     and

     musicians

     who

     contribute

     to

     the

     city

    's

     diverse

     arts

     community

    .

     Paris

     has

     a

     rich

     culinary

     heritage

     with

     many

     famous

     restaurants

    ,

     markets

    ,

     and

     shops

    ,

     as

     well

     as

     a

     thriving

     fashion

     industry

    .

     Its

     status

     as

     a

     cultural

     and

     political

     center

     has

     led

     to

     the

     city

    's

     growth

     and

     development

    ,

     making

     it

     one

     of

     the

     most

     important

     and

     dynamic

     cities

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     promising

    ,

     and

     it

     has

     the

     potential

     to

     revolution

    ize

     nearly

     every

     aspect

     of

     our

     lives

    .

     Here

     are

     some

     possible

     future

     trends

     in

     AI

    :
    


    1

    .

     Autonomous

     vehicles

    :

     AI

     is

     already

     playing

     a

     significant

     role

     in

     autonomous

     vehicles

    ,

     which

     are

     designed

     to

     drive

     themselves

    .

     By

     combining

     AI

     algorithms

     with

     real

    -world

     sensors

    ,

     autonomous

     vehicles

     can

     learn

     from

     real

    -world

     experiences

     and

     make

     informed

     decisions

    .

     This

     will

     make

     driving

     safer

     and

     more

     efficient

    .
    


    2

    .

     Predict

    ive

     analytics

    :

     AI

     is

     already

     being

     used

     to

     predict

     customer

     behavior

     and

     sales

    ,

     which

     can

     help

     companies

     make

     better

     decisions

    .

     By

     analyzing

     big

     data

    ,

     AI

     can

     provide

     insights

     that

     would

     be

     difficult

     or

     impossible

     for

     humans

     to

     obtain

    .
    


    3

    .

    



```python
llm.shutdown()
```
