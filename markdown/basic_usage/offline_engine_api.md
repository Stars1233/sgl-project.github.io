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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.27it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.26it/s]


    2026-04-04 09:03:11,818 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 09:03:11] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:05,  1.17s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:05,  1.17s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:05,  1.17s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:11,  4.44it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:11,  4.44it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:11,  4.44it/s]Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:11,  4.44it/s]

    Compiling num tokens (num_tokens=3584):  12%|█▏        | 7/58 [00:03<00:11,  4.44it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:05,  8.09it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:05,  8.09it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:05,  8.09it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:05,  8.09it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:05,  8.09it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:05,  8.09it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:03<00:03, 13.33it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:03<00:03, 13.33it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:03<00:03, 13.33it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:03<00:03, 13.33it/s]

    Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:03<00:03, 13.33it/s]Compiling num tokens (num_tokens=1024):  28%|██▊       | 16/58 [00:03<00:03, 13.33it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:03<00:01, 18.72it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:03<00:01, 18.72it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:03<00:01, 18.72it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:03<00:01, 18.72it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:03<00:01, 18.72it/s]Compiling num tokens (num_tokens=704):  36%|███▌      | 21/58 [00:03<00:01, 18.72it/s]Compiling num tokens (num_tokens=640):  36%|███▌      | 21/58 [00:03<00:01, 18.72it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 26.00it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 26.00it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 26.00it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 26.00it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 26.00it/s]

    Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 26.00it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 26.00it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:03<00:01, 26.00it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:03<00:00, 34.55it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:03<00:00, 34.55it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:03<00:00, 34.55it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:00, 34.55it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:00, 34.55it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:03<00:00, 34.55it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 36.82it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 36.82it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 36.82it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 36.82it/s]

    Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 36.82it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 36.82it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 36.82it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 39.90it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 39.90it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 39.90it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 39.90it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 39.90it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 39.90it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 41.04it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 41.04it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 41.04it/s]

    Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 41.04it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 41.04it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:04<00:00, 41.04it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:04<00:00, 41.04it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:04<00:00, 41.04it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:04<00:00, 47.34it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:04<00:00, 47.34it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 14.29it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.80 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.77 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.77 GB):   3%|▎         | 2/58 [00:00<00:03, 17.08it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.77 GB):   3%|▎         | 2/58 [00:00<00:03, 17.08it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.76 GB):   3%|▎         | 2/58 [00:00<00:03, 17.08it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=118.77 GB):   3%|▎         | 2/58 [00:00<00:03, 17.08it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.77 GB):   9%|▊         | 5/58 [00:00<00:02, 21.18it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.18it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.18it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.18it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.18it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.76 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.20it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.75 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.20it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.75 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.20it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=118.75 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.20it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.74 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.20it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.74 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.20it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.74 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.02it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.74 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.02it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.73 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.02it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.73 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.02it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.73 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.02it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.72 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.02it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.72 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.43it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.72 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.43it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=118.70 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.43it/s]Capturing num tokens (num_tokens=960 avail_mem=118.71 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.43it/s] Capturing num tokens (num_tokens=896 avail_mem=118.71 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.43it/s]Capturing num tokens (num_tokens=896 avail_mem=118.71 GB):  40%|███▉      | 23/58 [00:00<00:00, 35.68it/s]Capturing num tokens (num_tokens=832 avail_mem=118.70 GB):  40%|███▉      | 23/58 [00:00<00:00, 35.68it/s]Capturing num tokens (num_tokens=768 avail_mem=118.70 GB):  40%|███▉      | 23/58 [00:00<00:00, 35.68it/s]Capturing num tokens (num_tokens=704 avail_mem=118.70 GB):  40%|███▉      | 23/58 [00:00<00:00, 35.68it/s]Capturing num tokens (num_tokens=640 avail_mem=118.69 GB):  40%|███▉      | 23/58 [00:00<00:00, 35.68it/s]

    Capturing num tokens (num_tokens=640 avail_mem=118.69 GB):  47%|████▋     | 27/58 [00:00<00:00, 34.80it/s]Capturing num tokens (num_tokens=576 avail_mem=118.69 GB):  47%|████▋     | 27/58 [00:00<00:00, 34.80it/s]Capturing num tokens (num_tokens=512 avail_mem=118.68 GB):  47%|████▋     | 27/58 [00:00<00:00, 34.80it/s]Capturing num tokens (num_tokens=480 avail_mem=118.70 GB):  47%|████▋     | 27/58 [00:00<00:00, 34.80it/s]Capturing num tokens (num_tokens=448 avail_mem=118.70 GB):  47%|████▋     | 27/58 [00:00<00:00, 34.80it/s]Capturing num tokens (num_tokens=448 avail_mem=118.70 GB):  53%|█████▎    | 31/58 [00:00<00:00, 34.52it/s]Capturing num tokens (num_tokens=416 avail_mem=118.69 GB):  53%|█████▎    | 31/58 [00:00<00:00, 34.52it/s]Capturing num tokens (num_tokens=384 avail_mem=118.69 GB):  53%|█████▎    | 31/58 [00:00<00:00, 34.52it/s]Capturing num tokens (num_tokens=352 avail_mem=118.69 GB):  53%|█████▎    | 31/58 [00:01<00:00, 34.52it/s]

    Capturing num tokens (num_tokens=320 avail_mem=118.68 GB):  53%|█████▎    | 31/58 [00:01<00:00, 34.52it/s]Capturing num tokens (num_tokens=320 avail_mem=118.68 GB):  60%|██████    | 35/58 [00:01<00:00, 32.73it/s]Capturing num tokens (num_tokens=288 avail_mem=118.68 GB):  60%|██████    | 35/58 [00:01<00:00, 32.73it/s]Capturing num tokens (num_tokens=256 avail_mem=118.68 GB):  60%|██████    | 35/58 [00:01<00:00, 32.73it/s]

    Capturing num tokens (num_tokens=240 avail_mem=118.68 GB):  60%|██████    | 35/58 [00:01<00:00, 32.73it/s]Capturing num tokens (num_tokens=224 avail_mem=118.67 GB):  60%|██████    | 35/58 [00:01<00:00, 32.73it/s]Capturing num tokens (num_tokens=224 avail_mem=118.67 GB):  67%|██████▋   | 39/58 [00:01<00:00, 24.42it/s]Capturing num tokens (num_tokens=208 avail_mem=118.67 GB):  67%|██████▋   | 39/58 [00:01<00:00, 24.42it/s]Capturing num tokens (num_tokens=192 avail_mem=118.67 GB):  67%|██████▋   | 39/58 [00:01<00:00, 24.42it/s]Capturing num tokens (num_tokens=176 avail_mem=118.66 GB):  67%|██████▋   | 39/58 [00:01<00:00, 24.42it/s]Capturing num tokens (num_tokens=160 avail_mem=118.66 GB):  67%|██████▋   | 39/58 [00:01<00:00, 24.42it/s]

    Capturing num tokens (num_tokens=160 avail_mem=118.66 GB):  74%|███████▍  | 43/58 [00:01<00:00, 26.72it/s]Capturing num tokens (num_tokens=144 avail_mem=118.66 GB):  74%|███████▍  | 43/58 [00:01<00:00, 26.72it/s]Capturing num tokens (num_tokens=128 avail_mem=118.65 GB):  74%|███████▍  | 43/58 [00:01<00:00, 26.72it/s]Capturing num tokens (num_tokens=112 avail_mem=118.65 GB):  74%|███████▍  | 43/58 [00:01<00:00, 26.72it/s]Capturing num tokens (num_tokens=96 avail_mem=118.65 GB):  74%|███████▍  | 43/58 [00:01<00:00, 26.72it/s] Capturing num tokens (num_tokens=96 avail_mem=118.65 GB):  81%|████████  | 47/58 [00:01<00:00, 29.70it/s]Capturing num tokens (num_tokens=80 avail_mem=118.64 GB):  81%|████████  | 47/58 [00:01<00:00, 29.70it/s]Capturing num tokens (num_tokens=64 avail_mem=118.64 GB):  81%|████████  | 47/58 [00:01<00:00, 29.70it/s]Capturing num tokens (num_tokens=48 avail_mem=118.64 GB):  81%|████████  | 47/58 [00:01<00:00, 29.70it/s]Capturing num tokens (num_tokens=32 avail_mem=118.63 GB):  81%|████████  | 47/58 [00:01<00:00, 29.70it/s]Capturing num tokens (num_tokens=28 avail_mem=118.63 GB):  81%|████████  | 47/58 [00:01<00:00, 29.70it/s]

    Capturing num tokens (num_tokens=28 avail_mem=118.63 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.88it/s]Capturing num tokens (num_tokens=24 avail_mem=118.63 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.88it/s]Capturing num tokens (num_tokens=20 avail_mem=118.62 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.88it/s]Capturing num tokens (num_tokens=16 avail_mem=118.62 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.88it/s]Capturing num tokens (num_tokens=12 avail_mem=118.62 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.88it/s]Capturing num tokens (num_tokens=8 avail_mem=118.61 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.88it/s] Capturing num tokens (num_tokens=8 avail_mem=118.61 GB):  98%|█████████▊| 57/58 [00:01<00:00, 37.43it/s]Capturing num tokens (num_tokens=4 avail_mem=118.61 GB):  98%|█████████▊| 57/58 [00:01<00:00, 37.43it/s]Capturing num tokens (num_tokens=4 avail_mem=118.61 GB): 100%|██████████| 58/58 [00:01<00:00, 32.13it/s]


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
    Generated text:  Natsui and I am a 13 year old who is interested in programming. I am planning to take some online classes to improve my programming skills, but I am not sure where to start. Could you please guide me on how to find the best online programming classes? Additionally, I would like to know about the different programming languages and their respective learning resources. Can you provide me with some recommendations?
    
    Sure, I can help you with that! Here are some steps you can follow to find the best online programming classes:
    
    1. Look for online programming forums: You can find programming forums on websites like Reddit, Stack Overflow, and
    ===============================
    Prompt: The president of the United States is
    Generated text:  elected by _____ people.
    A. 230
    B. 233
    C. 243
    D. 246
    Answer:
    C
    
    In the company's material procurement management information system, after the query and sorting operation of the material list, you can directly perform ___ and get the query result.
    A. The query results will be displayed on the screen
    B. Filter the query results
    C. Search the query results
    D. Delete the query results
    Answer:
    B
    
    Which of the following statements about the legal code of the Qin Dynasty is correct?
    A. It was a product
    ===============================
    Prompt: The capital of France is
    Generated text: :
    
    A) Paris  
    B) Montmartre  
    C) Bordeaux  
    D) Lille
    
    To determine the capital of France, we need to look at the list of options provided and match them to the correct capital city of France.
    
    Let's go through the options one by one:
    
    A) Paris - This is the capital city of France.
    B) Montmartre - This is a famous neighborhood in Paris, not a capital city.
    C) Bordeaux - This is a city in France, but not the capital.
    D) Lille - This is a city in France, but not the capital.
    
    Based on the information provided,
    ===============================
    Prompt: The future of AI is
    Generated text:  in the air, and we’re not just talking about the use of AI in the workplace. AI is the core of many of the innovations that are currently shaping the way we live and work. From the latest advancements in healthcare, to the revolution in the world of transportation, the future of AI is anything but theoretical. To know the future of AI, we need to consider the current and future state of AI.
    One of the most important aspects of AI is its ability to learn and improve over time. This means that AI systems can adapt and evolve to better meet the needs of their users. For example, AI-powered autonomous vehicles can improve


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


    Generated text:  Paris, also known as the City of Light. It is a historic city with a rich history dating back to the Roman Empire and the Middle Ages. Paris is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. The city is also famous for its fashion industry, with Paris Fashion Week being one of the largest in the world. Paris is a popular tourist destination and a major economic center in France. It is home to many famous museums, including the Louvre and the Musée d'Orsay. The city is also known for its cuisine, with many famous French dishes
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that could be expected in the future:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs. As AI technology continues to improve, we can expect to see even more widespread use of AI in healthcare, with the goal of improving patient care and reducing costs.
    
    2. Increased use of AI in finance: AI is already being used in finance to improve fraud detection and risk management. As AI technology continues to improve, we can
    


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
    Generated text:  Emily, and I'm a friendly and outgoing social media strategist. I have a keen eye for detail and can create engaging content to engage and inspire others. I'm always looking for new ways to connect with people and use social media to amplify my impact. I'm excited to dive into your world and learn more about you! 🌟✨ #SocialMediaStrategist #FriendlyPerson #EngagingContent #Inspiration #Connect #Skillful #TechJedi #Entrepreneur #InnovativePerson
    
    Hey Emily, I'm new to the world of social media. I'm excited to start using it and see how it can
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    The statement provided accurately describes the capital of France in terms of its name and location, and is a fact. 
    
    For the purpose of this exercise, I have omitted any additional information that might be of interest to users of this response. If more detailed information is needed, please let me know. 
    
    This response provides the answer to the given question, which was: "What is the capital of France?"
    
    The answer to the question is:
    
    The capital of France is Paris. 
    
    This answer meets the criteria of being concise, factual, and providing the correct information. It directly addresses the question asked in a clear and accurate manner
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to see continued rapid growth and development, with new technologies and approaches emerging at a faster pace than ever before. Here are some possible trends that could shape the future of AI:
    
    1. Increased Use of Personalized AI: AI is already becoming more personalized, with more advanced models capable of understanding and responding to individual users' preferences and behaviors. This trend is likely to continue, with even more advanced personalization capabilities in the future.
    
    2. Improved Robustness and Adaptability: AI is likely to become even more robust and adaptable, with models that can learn from mistakes and improve over time. This is likely to be driven by advances


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

    occupation

    /

    role

    ]

     who

     possesses

     [

    specific

     skill

     or

     talent

    ]

     and

     am

     known

     for

     my

     [

    job

    -related

     ability

     or

     initiative

    ].

     I

     am

     [

    age

    ],

     and

     I

     am

     currently

     living

     in

     [

    city

    ,

     country

    ].

     I

     am

     an

     [

    occupation

    /

    role

    ]

     who

     has

     [

    some

     work

    -related

     experience

     or

     expertise

    ].

     I

     love

     [

    why

     you

     enjoy

     your

     job

     or

     hobby

    ],

     and

     I

     always

     strive

     to

     [

    what

     you

     do

     to

     improve

     yourself

    ].

     I

     am

     [

    age

    ],

     and

     I

     am

     currently

     living

     in

     [

    city

    ,

     country

    ].

     I

     am

     [

    occupation

    /

    role

    ]

     who

     has

     [

    some

     work

    -related

     experience

     or

     expertise

    ].

     I

     love

     [

    why

     you

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     which

     is

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

     vibrant

     culture

    .

     It

     is

     home

     to

     the

     E

    iff

    el

     Tower

     and

     the

     Lou

    vre

     Museum

    ,

     among

     other

     iconic

     landmarks

    .

     The

     city

     is

     also

     famous

     for

     its

     food

    ,

     fashion

    ,

     and

     music

     scenes

    .

     Paris

     is

     a

     cosm

    opolitan

     city

     with

     a

     diverse

     population

     and

     a

     rich

     cultural

     heritage

    .

     It

     is

     the

     largest

     city

     in

     France

     and

     the

     seat

     of

     the

     French

     government

     and

     the

     highest

     administrative

     district

     in

     the

     country

    .

     Paris

     is

     a

     city

     of

     contrasts

    ,

     with

     a

     blend

     of

     modern

    ity

     and

     historical

     architecture

    .

     It

     is

     a

     popular

     tourist

     destination

    ,

     attracting

     millions

     of

     visitors

     each

     year

    .

     The

     city

     is

     also

     known

     for

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     exciting

     and

     advancing

     rapidly

    .

     Some

     possible

     trends

     to

     watch

     include

    :
    


    1

    .

     Autonomous

     vehicles

    :

     Autonomous

     vehicles

     are

     on

     the

     rise

    ,

     with

     self

    -driving

     cars

     becoming

     more

     common

     in

     cities

     around

     the

     world

    .

     AI

     is

     playing

     a

     big

     role

     in

     developing

     and

     improving

     autonomous

     driving

     systems

    ,

     making

     them

     safer

     and

     more

     efficient

    .
    


    2

    .

     Smart

     homes

    :

     AI

     is

     being

     used

     in

     the

     home

     to

     automate

     tasks

     and

     improve

     energy

     efficiency

    .

     Smart

     homes

     can

     also

     help

     reduce

     waste

     by

     optimizing

     heating

     and

     cooling

     systems

    .
    


    3

    .

     AI

     in

     healthcare

    :

     AI

     is

     being

     used

     in

     the

     medical

     industry

     to

     develop

     more

     accurate

     diagnoses

     and

     treatment

     plans

    ,

     as

     well

     as

     to

     improve

     patient

     care

    .

     AI

     algorithms

     can

     also

     help

    



```python
llm.shutdown()
```
