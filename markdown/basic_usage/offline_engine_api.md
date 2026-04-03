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


    2026-04-03 04:02:38.244 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 04:02:38] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 04:02:38.245 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 04:02:38] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 04:02:38.245 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 04:02:38] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 04:02:38.245 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 04:02:38] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 04:02:38.245 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 04:02:38] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.30it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.29it/s]


    2026-04-03 04:02:43,686 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 04:02:43] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:36,  2.74s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:36,  2.74s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:36,  2.74s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:36,  2.74s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:36,  2.74s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.05it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.05it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.05it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.05it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:07,  6.05it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:07,  6.05it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:07,  6.05it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:07,  6.05it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  6.05it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.28it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.28it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.28it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.28it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.28it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.28it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.28it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.28it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.28it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.28it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.28it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.28it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.28it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.28it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.28it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.28it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:01, 24.84it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:01, 24.84it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:01, 24.84it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:01, 24.84it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:01, 24.84it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:01, 24.84it/s]

    Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:01, 24.84it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 29.84it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 29.84it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 29.84it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 29.84it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 29.84it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 29.84it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 29.84it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 34.77it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 34.77it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 34.77it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 34.77it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 34.77it/s]

    Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 34.77it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 34.77it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 38.89it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 38.89it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 38.89it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 38.89it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 38.89it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 38.89it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 38.89it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 38.89it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.63it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=137.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=137.42 GB):   2%|▏         | 1/58 [00:00<00:33,  1.72it/s]Capturing num tokens (num_tokens=7680 avail_mem=137.39 GB):   2%|▏         | 1/58 [00:00<00:33,  1.72it/s]Capturing num tokens (num_tokens=7168 avail_mem=137.38 GB):   2%|▏         | 1/58 [00:00<00:33,  1.72it/s]Capturing num tokens (num_tokens=7168 avail_mem=137.38 GB):   5%|▌         | 3/58 [00:00<00:10,  5.12it/s]Capturing num tokens (num_tokens=6656 avail_mem=137.38 GB):   5%|▌         | 3/58 [00:00<00:10,  5.12it/s]Capturing num tokens (num_tokens=6144 avail_mem=137.38 GB):   5%|▌         | 3/58 [00:00<00:10,  5.12it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=137.38 GB):   5%|▌         | 3/58 [00:00<00:10,  5.12it/s]Capturing num tokens (num_tokens=5632 avail_mem=137.38 GB):  10%|█         | 6/58 [00:00<00:05, 10.10it/s]Capturing num tokens (num_tokens=5120 avail_mem=137.38 GB):  10%|█         | 6/58 [00:00<00:05, 10.10it/s]Capturing num tokens (num_tokens=4608 avail_mem=137.38 GB):  10%|█         | 6/58 [00:00<00:05, 10.10it/s]Capturing num tokens (num_tokens=4096 avail_mem=137.38 GB):  10%|█         | 6/58 [00:00<00:05, 10.10it/s]Capturing num tokens (num_tokens=3840 avail_mem=137.37 GB):  10%|█         | 6/58 [00:00<00:05, 10.10it/s]Capturing num tokens (num_tokens=3840 avail_mem=137.37 GB):  17%|█▋        | 10/58 [00:00<00:02, 16.31it/s]Capturing num tokens (num_tokens=3584 avail_mem=137.37 GB):  17%|█▋        | 10/58 [00:00<00:02, 16.31it/s]Capturing num tokens (num_tokens=3328 avail_mem=137.37 GB):  17%|█▋        | 10/58 [00:00<00:02, 16.31it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=137.34 GB):  17%|█▋        | 10/58 [00:01<00:02, 16.31it/s]Capturing num tokens (num_tokens=3072 avail_mem=137.34 GB):  22%|██▏       | 13/58 [00:01<00:02, 17.71it/s]Capturing num tokens (num_tokens=2816 avail_mem=137.34 GB):  22%|██▏       | 13/58 [00:01<00:02, 17.71it/s]Capturing num tokens (num_tokens=2560 avail_mem=137.34 GB):  22%|██▏       | 13/58 [00:01<00:02, 17.71it/s]Capturing num tokens (num_tokens=2304 avail_mem=137.33 GB):  22%|██▏       | 13/58 [00:01<00:02, 17.71it/s]Capturing num tokens (num_tokens=2304 avail_mem=137.33 GB):  28%|██▊       | 16/58 [00:01<00:02, 20.10it/s]Capturing num tokens (num_tokens=2048 avail_mem=137.31 GB):  28%|██▊       | 16/58 [00:01<00:02, 20.10it/s]Capturing num tokens (num_tokens=1792 avail_mem=136.83 GB):  28%|██▊       | 16/58 [00:01<00:02, 20.10it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=136.83 GB):  28%|██▊       | 16/58 [00:01<00:02, 20.10it/s]Capturing num tokens (num_tokens=1280 avail_mem=136.67 GB):  28%|██▊       | 16/58 [00:01<00:02, 20.10it/s]Capturing num tokens (num_tokens=1024 avail_mem=136.65 GB):  28%|██▊       | 16/58 [00:01<00:02, 20.10it/s]Capturing num tokens (num_tokens=1024 avail_mem=136.65 GB):  36%|███▌      | 21/58 [00:01<00:01, 26.53it/s]Capturing num tokens (num_tokens=960 avail_mem=136.66 GB):  36%|███▌      | 21/58 [00:01<00:01, 26.53it/s] Capturing num tokens (num_tokens=896 avail_mem=136.66 GB):  36%|███▌      | 21/58 [00:01<00:01, 26.53it/s]Capturing num tokens (num_tokens=832 avail_mem=136.65 GB):  36%|███▌      | 21/58 [00:01<00:01, 26.53it/s]Capturing num tokens (num_tokens=768 avail_mem=136.65 GB):  36%|███▌      | 21/58 [00:01<00:01, 26.53it/s]Capturing num tokens (num_tokens=704 avail_mem=136.65 GB):  36%|███▌      | 21/58 [00:01<00:01, 26.53it/s]Capturing num tokens (num_tokens=704 avail_mem=136.65 GB):  45%|████▍     | 26/58 [00:01<00:01, 31.64it/s]Capturing num tokens (num_tokens=640 avail_mem=136.64 GB):  45%|████▍     | 26/58 [00:01<00:01, 31.64it/s]Capturing num tokens (num_tokens=576 avail_mem=136.64 GB):  45%|████▍     | 26/58 [00:01<00:01, 31.64it/s]

    Capturing num tokens (num_tokens=512 avail_mem=136.63 GB):  45%|████▍     | 26/58 [00:01<00:01, 31.64it/s]Capturing num tokens (num_tokens=480 avail_mem=136.65 GB):  45%|████▍     | 26/58 [00:01<00:01, 31.64it/s]Capturing num tokens (num_tokens=448 avail_mem=136.65 GB):  45%|████▍     | 26/58 [00:01<00:01, 31.64it/s]Capturing num tokens (num_tokens=448 avail_mem=136.65 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.48it/s]Capturing num tokens (num_tokens=416 avail_mem=136.64 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.48it/s]Capturing num tokens (num_tokens=384 avail_mem=136.64 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.48it/s]Capturing num tokens (num_tokens=352 avail_mem=136.64 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.48it/s]Capturing num tokens (num_tokens=320 avail_mem=136.63 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.48it/s]Capturing num tokens (num_tokens=288 avail_mem=136.63 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.48it/s]Capturing num tokens (num_tokens=288 avail_mem=136.63 GB):  62%|██████▏   | 36/58 [00:01<00:00, 38.18it/s]Capturing num tokens (num_tokens=256 avail_mem=136.63 GB):  62%|██████▏   | 36/58 [00:01<00:00, 38.18it/s]Capturing num tokens (num_tokens=240 avail_mem=136.62 GB):  62%|██████▏   | 36/58 [00:01<00:00, 38.18it/s]

    Capturing num tokens (num_tokens=224 avail_mem=136.62 GB):  62%|██████▏   | 36/58 [00:01<00:00, 38.18it/s]Capturing num tokens (num_tokens=208 avail_mem=136.62 GB):  62%|██████▏   | 36/58 [00:01<00:00, 38.18it/s]Capturing num tokens (num_tokens=192 avail_mem=136.62 GB):  62%|██████▏   | 36/58 [00:01<00:00, 38.18it/s]Capturing num tokens (num_tokens=192 avail_mem=136.62 GB):  71%|███████   | 41/58 [00:01<00:00, 40.33it/s]Capturing num tokens (num_tokens=176 avail_mem=136.61 GB):  71%|███████   | 41/58 [00:01<00:00, 40.33it/s]Capturing num tokens (num_tokens=160 avail_mem=136.61 GB):  71%|███████   | 41/58 [00:01<00:00, 40.33it/s]Capturing num tokens (num_tokens=144 avail_mem=136.61 GB):  71%|███████   | 41/58 [00:01<00:00, 40.33it/s]Capturing num tokens (num_tokens=128 avail_mem=136.60 GB):  71%|███████   | 41/58 [00:01<00:00, 40.33it/s]Capturing num tokens (num_tokens=112 avail_mem=136.60 GB):  71%|███████   | 41/58 [00:01<00:00, 40.33it/s]Capturing num tokens (num_tokens=112 avail_mem=136.60 GB):  79%|███████▉  | 46/58 [00:01<00:00, 41.72it/s]Capturing num tokens (num_tokens=96 avail_mem=136.60 GB):  79%|███████▉  | 46/58 [00:01<00:00, 41.72it/s] Capturing num tokens (num_tokens=80 avail_mem=136.59 GB):  79%|███████▉  | 46/58 [00:01<00:00, 41.72it/s]

    Capturing num tokens (num_tokens=64 avail_mem=136.59 GB):  79%|███████▉  | 46/58 [00:01<00:00, 41.72it/s]Capturing num tokens (num_tokens=48 avail_mem=136.59 GB):  79%|███████▉  | 46/58 [00:01<00:00, 41.72it/s]Capturing num tokens (num_tokens=32 avail_mem=136.58 GB):  79%|███████▉  | 46/58 [00:01<00:00, 41.72it/s]Capturing num tokens (num_tokens=32 avail_mem=136.58 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.21it/s]Capturing num tokens (num_tokens=28 avail_mem=136.58 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.21it/s]Capturing num tokens (num_tokens=24 avail_mem=136.58 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.21it/s]Capturing num tokens (num_tokens=20 avail_mem=136.57 GB):  88%|████████▊ | 51/58 [00:02<00:00, 42.21it/s]Capturing num tokens (num_tokens=16 avail_mem=136.57 GB):  88%|████████▊ | 51/58 [00:02<00:00, 42.21it/s]Capturing num tokens (num_tokens=12 avail_mem=136.57 GB):  88%|████████▊ | 51/58 [00:02<00:00, 42.21it/s]Capturing num tokens (num_tokens=12 avail_mem=136.57 GB):  97%|█████████▋| 56/58 [00:02<00:00, 43.44it/s]Capturing num tokens (num_tokens=8 avail_mem=136.56 GB):  97%|█████████▋| 56/58 [00:02<00:00, 43.44it/s] Capturing num tokens (num_tokens=4 avail_mem=136.56 GB):  97%|█████████▋| 56/58 [00:02<00:00, 43.44it/s]

    Capturing num tokens (num_tokens=4 avail_mem=136.56 GB): 100%|██████████| 58/58 [00:02<00:00, 27.34it/s]


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
    Generated text:  Haim Schindler. I am the first Member of Parliament (MP) in the 18th parliament in New Zealand. I was elected on 14 November 2018. I am the first New Zealand MP to run on a portfolio-based system for elections and have been a Member of Parliament since 2014, serving as a Minister of Transport, Infrastructure and Sustainability, Cabinet Secretary for Climate Change, Cabinet Secretary for Infrastructure, Cabinet Secretary for Infrastructure, Environment, Transport and Environment (CETE), Cabinet Secretary for Health and Primary Care, Cabinet Secretary for Energy and Climate Change, Cabinet Secretary for Food
    ===============================
    Prompt: The president of the United States is
    Generated text:  a man. The president of the United States is a leader. The president of the United States is a person. The president of the United States was elected. The president of the United States is a political leader. The president of the United States is an important person. The president of the United States is a nation leader. The president of the United States is not an office holder. The president of the United States is the head of government. The president of the United States is the head of state. The president of the United States is the head of the executive branch. The president of the United States is the head of the executive branch
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, and it has a population of 2.5 million. If the population of Paris increases by 15% each year, how many people will live in Paris in 5 years?
    
    To determine the population of Paris in 5 years, we need to calculate the population increase year by year. The population increases by 15% each year. This means that after one year, the population will be 115% of the current population, after two years it will be 115% of 115%, and so on.
    
    Let's denote the current population of Paris as \( P_0
    ===============================
    Prompt: The future of AI is
    Generated text:  heavily dependent on how you design and implement the algorithms that are used to determine the training data, the features, and the metrics that are used to evaluate the results. The responsibility of AI developers is to ensure that these algorithms are robust, accurate, and fair. This involves a combination of statistical analysis, machine learning, and domain expertise. It is also important to ensure that the development process is transparent and open to stakeholder feedback. Additionally, it is critical to continue to update and refine these algorithms to keep up with changing trends and demands in the industry. This is a complex and challenging task, but it is essential for the future of AI


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major cultural and economic center, hosting numerous museums, theaters, and other attractions. Paris is known for its rich history, art, and cuisine, and is a popular tourist destination. It is the largest city in France and the second-largest city in the world by population. Paris is also home to the French Parliament and the French National Library. The city is known for its fashion industry, with many famous fashion designers and boutiques. It is also home to the Eiffel Tower
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased automation and robotics: As AI continues to advance, we can expect to see more automation and robotics in various industries, including manufacturing, healthcare, and transportation. This will lead to increased efficiency, cost savings, and job displacement, but it will also create new opportunities for innovation and creativity.
    
    2. AI-powered healthcare: AI will play a crucial role in healthcare, with the ability to analyze medical data, predict disease outbreaks, and develop personalized treatment plans. This will lead
    


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
    Generated text:  _____. I'm a/an (insert occupation) who has (insert a characteristic or personal trait that makes you unique). My professional goals are to (insert one or two goals), and I'm passionate about (insert something you enjoy doing), and I'm dedicated to (insert something that represents your values or principles). Overall, I aim to (insert a sentiment or goal that I want to achieve).
    
    What's your favorite book, movie, or song? What would you like to learn more about? What's your next big project idea? What would you like to work on? What kind of conversation would you like to have with my audience
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    When asked what its capital city is, people often say "Paris". Paris is the capital city of France. It is the largest city in Europe and the most visited city in the world. It is also home to numerous cultural, historical, and natural attractions. Paris is known for its art and architecture, fine dining, and lively nightlife. Many famous people, including actor Edward “Madman” Murphy, have also been born and raised in Paris. Paris is a popular destination for tourists, visitors, and business travelers alike, offering a unique and diverse experience to visitors. Overall, Paris is a fascinating and culturally rich city with
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  highly likely to include the development of more advanced AI systems that can learn and adapt to new and changing situations. Here are some possible trends that could come in the future:
    
    1. Increased AI transparency: As AI systems become more complex and powerful, it may become easier for researchers to understand and control their behavior. This could lead to greater transparency in AI development and deployment.
    
    2. AI ethics and governance: With the rise of AI, there may be increased concerns about its ethical implications and the potential for misuse. As a result, there may be greater efforts to develop more ethical and responsible AI systems.
    
    3. AI with emotional intelligence: As


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

    ],

     and

     I

    'm

     a

     [

    job

     title

    ]

     at

     [

    company

     name

    ].

     I

    've

     always

     had

     a

     passion

     for

     [

    insert

     something

     personal

     or

     specific

     to

     you

    ].

     I

    'm

     always

     looking

     for

     new

     experiences

     and

     learning

     new

     things

    ,

     so

     I

    'm

     always

     open

     to

     new

     opportunities

     and

     challenges

    .

     I

    'm

     here

     to

     help

     those

     who

     need

     guidance

     or

     advice

    ,

     and

     I

    'm

     here

     to

     make

     a

     positive

     impact

     in

     the

     world

    .

     Thanks

     for

     taking

     the

     time

     to

     meet

     me

    ,

     [

    Person

    ].

     Let

    's

     talk

     about

     [

    insert

     something

     specific

     about

     your

     character

    ].

     Welcome

     to

     my

     world

    !

     Let

     me

     know

     if

     you

     need

     any

     information

     or

     guidance

    .

     [

    Person

    ].

     Hey

     [

    Name

    ],

     I

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     located

     in

     the

     Î

    le

     de

     France

     region

     on

     the

     French

     Riv

    iera

    ,

     boasting

     a

     population

     of

     around

     

    2

    .

    2

     million

     people

    .

     Paris

     is

     known

     for

     its

     iconic

     landmarks

     such

     as

     Notre

     Dame

     Cathedral

    ,

     E

    iff

    el

     Tower

    ,

     and

     Lou

    vre

     Museum

    ,

     as

     well

     as

     its

     rich

     history

     and

     diverse

     cultural

     scene

    .

     The

     city

     also

     plays

     host

     to

     many

     international

     events

     and

     festivals

    ,

     making

     it

     an

     important

     center

     for

     the

     French

    -speaking

     world

    .

     Paris

     is

     a

     major

     economic

     hub

     and

     the

     financial

     capital

     of

     France

    ,

     with

     many

     companies

     and

     institutions

     headquartered

     in

     the

     city

    .

     However

    ,

     due

     to

     its

     status

     as

     a

     major

     tourist

     destination

    ,

     Paris

     has

     become

     quite

     crowded

     and

     its

     population

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     continue

     to

     be

     dominated

     by

     a

     number

     of

     disruptive

     technologies

    ,

     including

    :
    


    1

    .

     Machine

     Learning

     and

     Deep

     Learning

    :

     These

     are

     the

     two

     main

     areas

     of

     AI

     research

     and

     development

    .

     Machine

     Learning

     involves

     training

     machines

     to

     learn

     from

     data

    ,

     while

     Deep

     Learning

     is

     a

     type

     of

     Machine

     Learning

     that

     uses

     multiple

     layers

     of

     neural

     networks

     to

     process

     and

     interpret

     complex

     data

    .
    


    2

    .

     Computer

     Vision

    :

     This

     area

     of

     AI

     research

     is

     focused

     on

     creating

     machines

     that

     can

     understand

     and

     interpret

     visual

     information

    ,

     such

     as

     images

     and

     videos

    .

     As

     autonomous

     vehicles

    ,

     facial

     recognition

    ,

     and

     augmented

     reality

     become

     more

     prevalent

    ,

     the

     importance

     of

     computer

     vision

     is

     set

     to

     grow

    .
    


    3

    .

     Robotics

    :

     AI

    -powered

     robots

    



```python
llm.shutdown()
```
