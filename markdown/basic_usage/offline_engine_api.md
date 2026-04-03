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


    2026-04-03 14:40:22.150 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 14:40:22] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 14:40:22.151 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 14:40:22] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 14:40:22.151 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 14:40:22] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 14:40:22.151 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 14:40:22] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 14:40:22.151 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 14:40:22] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.07it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.06it/s]


    2026-04-03 14:40:25,027 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 14:40:25] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:35,  2.72s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:35,  2.72s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:35,  2.72s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:35,  2.72s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:35,  2.72s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:22,  2.32it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:22,  2.32it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:22,  2.32it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:22,  2.32it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:22,  2.32it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:22,  2.32it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:22,  2.32it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.11it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.11it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.11it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.11it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:07,  6.11it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:07,  6.11it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:07,  6.11it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:07,  6.11it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  6.11it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.40it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.40it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.40it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.40it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.40it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.40it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.40it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.40it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 25.07it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 25.07it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 25.07it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 25.07it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 25.07it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 25.07it/s]

    Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 25.07it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 30.10it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 30.10it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 30.10it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 30.10it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 30.10it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 30.10it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 30.10it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 35.03it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 35.03it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 35.03it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 35.03it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 35.03it/s]

    Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 35.03it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 35.03it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 39.09it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 39.09it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 39.09it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 39.09it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 39.09it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 39.09it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 39.09it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 39.09it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.75it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=134.67 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=134.64 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=134.64 GB):   3%|▎         | 2/58 [00:00<00:03, 18.47it/s]Capturing num tokens (num_tokens=7168 avail_mem=134.64 GB):   3%|▎         | 2/58 [00:00<00:03, 18.47it/s]Capturing num tokens (num_tokens=6656 avail_mem=134.64 GB):   3%|▎         | 2/58 [00:00<00:03, 18.47it/s]Capturing num tokens (num_tokens=6144 avail_mem=134.64 GB):   3%|▎         | 2/58 [00:00<00:03, 18.47it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=134.64 GB):   9%|▊         | 5/58 [00:00<00:02, 21.82it/s]Capturing num tokens (num_tokens=5632 avail_mem=134.63 GB):   9%|▊         | 5/58 [00:00<00:02, 21.82it/s]Capturing num tokens (num_tokens=5120 avail_mem=134.63 GB):   9%|▊         | 5/58 [00:00<00:02, 21.82it/s]Capturing num tokens (num_tokens=4608 avail_mem=134.63 GB):   9%|▊         | 5/58 [00:00<00:02, 21.82it/s]Capturing num tokens (num_tokens=4096 avail_mem=134.63 GB):   9%|▊         | 5/58 [00:00<00:02, 21.82it/s]Capturing num tokens (num_tokens=4096 avail_mem=134.63 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.82it/s]Capturing num tokens (num_tokens=3840 avail_mem=134.63 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.82it/s]Capturing num tokens (num_tokens=3584 avail_mem=134.62 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.82it/s]Capturing num tokens (num_tokens=3328 avail_mem=134.62 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.82it/s]Capturing num tokens (num_tokens=3072 avail_mem=134.61 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.82it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=134.61 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.82it/s]Capturing num tokens (num_tokens=2816 avail_mem=134.61 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.57it/s]Capturing num tokens (num_tokens=2560 avail_mem=134.61 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.57it/s]Capturing num tokens (num_tokens=2304 avail_mem=133.59 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.57it/s]Capturing num tokens (num_tokens=2048 avail_mem=133.53 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.57it/s]Capturing num tokens (num_tokens=1792 avail_mem=133.52 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.57it/s]Capturing num tokens (num_tokens=1536 avail_mem=133.47 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.57it/s]Capturing num tokens (num_tokens=1536 avail_mem=133.47 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.34it/s]Capturing num tokens (num_tokens=1280 avail_mem=124.74 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.34it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.34it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.34it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=120.25 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.34it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.34it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.21it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.21it/s]Capturing num tokens (num_tokens=704 avail_mem=120.24 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.21it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.21it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.21it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.21it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  50%|█████     | 29/58 [00:00<00:00, 40.88it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  50%|█████     | 29/58 [00:00<00:00, 40.88it/s]Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  50%|█████     | 29/58 [00:00<00:00, 40.88it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  50%|█████     | 29/58 [00:00<00:00, 40.88it/s]

    Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  50%|█████     | 29/58 [00:00<00:00, 40.88it/s]Capturing num tokens (num_tokens=352 avail_mem=120.22 GB):  50%|█████     | 29/58 [00:00<00:00, 40.88it/s]Capturing num tokens (num_tokens=352 avail_mem=120.22 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.45it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.45it/s]Capturing num tokens (num_tokens=288 avail_mem=120.21 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.45it/s]Capturing num tokens (num_tokens=256 avail_mem=120.21 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.45it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.45it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.45it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.40it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.40it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.40it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.40it/s]

    Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.40it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.40it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.15it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.15it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.15it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.15it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.15it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.15it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.47it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.47it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.47it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.47it/s]

    Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.47it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.47it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  93%|█████████▎| 54/58 [00:01<00:00, 45.00it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  93%|█████████▎| 54/58 [00:01<00:00, 45.00it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  93%|█████████▎| 54/58 [00:01<00:00, 45.00it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  93%|█████████▎| 54/58 [00:01<00:00, 45.00it/s] Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  93%|█████████▎| 54/58 [00:01<00:00, 45.00it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 40.04it/s]


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
    Generated text:  Niki and I'm a graduate student in the Department of Electrical Engineering and Computer Science at the University of Illinois at Urbana-Champaign. I am interested in computational neuroscience and the role that brain activity plays in the processing and communication of information. Specifically, I am working on an array-based algorithm for the computation of attractors in the context of the Brownian point process.
    
    I joined UIC in January 2019 and began working in the Department of Electrical Engineering and Computer Science in the Fall of 2018. I started out as a Research Assistant in the Department of Electrical Engineering and Computer Science, and then in
    ===============================
    Prompt: The president of the United States is
    Generated text:  a person. This statement is about ___.
    A. a general
    B. a particular
    C. a category
    D. a characteristic
    
    To determine the correct answer, let's analyze the statement step by step:
    
    1. **Identify the type of logical reasoning involved**: This is a classification question, specifically a type of logical inference that classifies a person into a category.
    
    2. **Understand the statement**: The statement "the president of the United States is a person" is asking about a particular person.
    
    3. **Evaluate the options**:
       - **A. a general**: A general is a broader category or
    ===============================
    Prompt: The capital of France is
    Generated text:  [ ].
    A: Paris
    B: London
    C: New York
    D: Moscow
    To determine the capital of France, let's consider the options provided:
    
    A: Paris
    B: London
    C: New York
    D: Moscow
    
    Paris is the capital of France, but it is located in the north of the country. London is the capital of England, located in the south of the country. New York is the capital of the United States, located in the northeast of the country. Moscow is the capital of Russia, located in the north of the country.
    
    Given this information, the correct capital of France must be
    ===============================
    Prompt: The future of AI is
    Generated text:  digital, it is a technology that will transform every sector of the world in the years to come. The digital revolution will profoundly change the way we live, work, and interact with each other. But, at the same time, it can also be a source of damage, as digital technology has its own risks and risks can be serious. How can we ensure that we are not harming the digital world? Below, we explore the main issues and challenges of the digital revolution and how they can be mitigated to ensure the future of AI is digital and sustainable.
    Digital risk and impact
    Digital technology offers a range of opportunities and challenges, including


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


    Generated text:  Paris, also known as the City of Light. It is the largest city in France and the second-largest city in the European Union. Paris is home to many famous landmarks such as the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Palace of Versailles. The city is known for its rich history, art, and culture, and is a major tourist destination. Paris is also home to many international organizations and institutions, including the French Academy of Sciences and the European Parliament. The city is a major economic and cultural center, with a diverse population and a rich cultural heritage. Paris is a popular destination for tourists
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased integration with other technologies: AI is likely to become more integrated with other technologies, such as machine learning, natural language processing, and computer vision. This integration will enable AI to perform tasks that are currently only possible with human intelligence, such as image recognition, speech recognition, and decision-making.
    
    2. Greater emphasis on ethical considerations: As AI becomes more integrated with other technologies, there will be a greater emphasis on ethical considerations
    


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
    Generated text:  John, I'm 30 years old, and I have a passion for photography. I'm always excited to capture amazing images of people and places around me. I've been taking photos since I was 12 and I've traveled the world at least 3 times. I'm a big fan of photography, and I think it's amazing how much we can capture with just a camera. I'm always looking for new experiences and learning new things about photography, and I'm always up for a challenge. What's your favorite photo from the past year? I've been editing my own work and trying to improve my skills, and
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, commonly known as the "City of Light." It is located in the Loire Valley region of the France, in the north central part of the country, and is the largest city by population in France. The city has a rich history, which dates back to the Roman Empire, and is famous for its distinctive architecture, such as the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. Paris is also a global cultural and political center, with a vast range of attractions and events to offer visitors. It is considered to be one of the most vibrant and diverse cities in the world. 
    
    Paris is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  bright and likely to evolve rapidly. Here are some potential trends that could shape the way AI is used:
    
    1. Personalization: As AI becomes more sophisticated, it will become increasingly adept at understanding and personalizing user experiences. This could lead to a more personalized and efficient service to users.
    
    2. Integration with natural language processing: As AI becomes more sophisticated, it will become increasingly adept at understanding and processing natural language. This could lead to more intuitive and efficient interfaces for users.
    
    3. Increased focus on ethical and ethical issues: As AI continues to advance, there will likely be increased focus on ethical and ethical issues. This could lead to


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

    _.

     I

    'm

     a

     computer

     programmer

    .

     My

     primary

     focus

     is

     on

     developing

     software

     and

     designing

     user

     interfaces

    .

     I

    've

     built

     websites

     and

     apps

     for

     clients

     and

     have

     a

     keen

     interest

     in

     technical

     problem

    -solving

    .

     If

     you

     have

     any

     questions

     or

     need

     help

     with

     anything

    ,

     don

    't

     hesitate

     to

     ask

    .

     I

    'm

     always

     eager

     to

     learn

     and

     grow

    .

     So

    ,

     what

     can

     I

     do

     for

     you

     today

    ?

     Let

    's

     chat

    !

     I

     look

     forward

     to

     hearing

     from

     you

    .

     What

    's

     your

     name

    ?

     Do

     you

     have

     a

     unique

     name

     or

     a

     nickname

    ?

     I

     enjoy

     using

     my

     initials

    .

     This

     is

     my

     nickname

    :

     ____

    _.

     How

     did

     you

     get

     started

     in

     programming

    ?

     I

     started

     programming

     when

     I

     was

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .


    Paris

     is

     the

     largest

     city

     in

     France

     and

     its

     political

    ,

     cultural

    ,

     and

     economic

     center

    .

     It

     is

     also

     the

     most

     visited

     city

     in

     the

     world

    ,

     attracting

     millions

     of

     tourists

     each

     year

    .

     It

     has

     a

     rich

     history

    ,

     including

     a

     major

     re

    naissance

     period

     in

     the

     

    1

    3

    th

     and

     

    1

    4

    th

     centuries

    .

     France

    's

     flag

     and

     coat

     of

     arms

     are

     also

     inspired

     by

     Paris

    .

     The

     city

     is

     known

     for

     its

     art

    ,

     architecture

    ,

     and

     cuisine

    ,

     and

     is

     home

     to

     many

     famous

     landmarks

     and

     museums

    .

     Paris

     is

     a

     hub

     of

     culture

    ,

     industry

    ,

     and

     commerce

    ,

     and

     plays

     a

     significant

     role

     in

     French

     politics

     and

     the

     French

     people

    .

     The

     city

     is

     also

     known

    
    
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

     which

     are

     likely

     to

     transform

     the

     way

     we

     use

     and

     benefit

     from

     AI

     technologies

    .

     Some

     of

     these

     trends

     include

    :
    


    1

    .

     Increased

     use

     of

     AI

     in

     healthcare

    :

     AI

     is

     already

     being

     used

     in

     medical

     diagnosis

     and

     treatment

    ,

     but

     it

     has

     the

     potential

     to

     transform

     the

     way

     we

     deliver

     healthcare

    .

     The

     use

     of

     AI

     in

     healthcare

     may

     include

     developing

     more

     accurate

     medical

     diagnoses

    ,

     personalized

     treatment

     plans

    ,

     and

     even

     virtual

     reality

     for

     medical

     simulations

    .
    


    2

    .

     Enhanced

     personal

    ization

     of

     AI

    :

     As

     AI

     technology

     advances

    ,

     it

     will

     become

     increasingly

     powerful

     and

     able

     to

     learn

     from

     data

    ,

     making

     it

     possible

     to

     provide

     more

     personalized

     experiences

     to

     users

    .

     This

     could

     lead

    



```python
llm.shutdown()
```
