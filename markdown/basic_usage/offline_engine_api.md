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


    2026-04-01 08:51:27.363 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 08:51:27] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 08:51:27.363 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 08:51:27] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 08:51:27.363 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 08:51:27] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 08:51:27.363 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 08:51:27] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 08:51:27.363 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 08:51:27] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.52it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.50it/s]


    2026-04-01 08:51:30,278 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 08:51:30] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:38,  2.78s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:38,  2.78s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:38,  2.78s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:38,  2.78s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.80it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.80it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.80it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.80it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.80it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.80it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:29,  1.80it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.55it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.55it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:03<00:08,  5.55it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:03<00:08,  5.55it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:03<00:08,  5.55it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:03<00:08,  5.55it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:03<00:08,  5.55it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:03<00:08,  5.55it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:08,  5.55it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 11.73it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 11.73it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 11.73it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 11.73it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 11.73it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 11.73it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 11.73it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 11.73it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 17.68it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 17.68it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 17.68it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 17.68it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 17.68it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 17.68it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 17.68it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 17.68it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.26it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.26it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.26it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.26it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.26it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.26it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.26it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 34.65it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 34.65it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 34.65it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 34.65it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 34.65it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 34.65it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 34.65it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.00it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.00it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.00it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.00it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.00it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.00it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.00it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.00it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.00it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.40it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.33 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.51it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.51it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.51it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.51it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 20.10it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 20.10it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 20.10it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 20.10it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.34it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.34it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.34it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.34it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.34it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  21%|██        | 12/58 [00:00<00:01, 27.21it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 27.21it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 27.21it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 27.21it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 27.21it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.79it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.26 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.79it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.79it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.79it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.79it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.79it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.27it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.27it/s] Capturing num tokens (num_tokens=896 avail_mem=120.24 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.27it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.27it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.27it/s]Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.27it/s]Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.39it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.39it/s]

    Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.39it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.39it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.39it/s]Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.39it/s]Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:00<00:00, 40.44it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:00<00:00, 40.44it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:00<00:00, 40.44it/s]Capturing num tokens (num_tokens=352 avail_mem=120.26 GB):  53%|█████▎    | 31/58 [00:00<00:00, 40.44it/s]Capturing num tokens (num_tokens=320 avail_mem=120.26 GB):  53%|█████▎    | 31/58 [00:00<00:00, 40.44it/s]Capturing num tokens (num_tokens=288 avail_mem=119.03 GB):  53%|█████▎    | 31/58 [00:01<00:00, 40.44it/s]

    Capturing num tokens (num_tokens=288 avail_mem=119.03 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.77it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.77it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.77it/s]Capturing num tokens (num_tokens=224 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.77it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.77it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.88it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.88it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.88it/s]Capturing num tokens (num_tokens=160 avail_mem=118.92 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.88it/s]

    Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  69%|██████▉   | 40/58 [00:01<00:00, 38.88it/s]Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.68it/s]Capturing num tokens (num_tokens=128 avail_mem=118.91 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.68it/s]Capturing num tokens (num_tokens=112 avail_mem=118.91 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.68it/s]Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.68it/s] Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.68it/s]Capturing num tokens (num_tokens=64 avail_mem=118.90 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.68it/s]Capturing num tokens (num_tokens=64 avail_mem=118.90 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.68it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.68it/s]Capturing num tokens (num_tokens=32 avail_mem=118.89 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.68it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.68it/s]

    Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.68it/s]Capturing num tokens (num_tokens=20 avail_mem=118.88 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.68it/s]Capturing num tokens (num_tokens=20 avail_mem=118.88 GB):  93%|█████████▎| 54/58 [00:01<00:00, 41.06it/s]Capturing num tokens (num_tokens=16 avail_mem=118.88 GB):  93%|█████████▎| 54/58 [00:01<00:00, 41.06it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  93%|█████████▎| 54/58 [00:01<00:00, 41.06it/s]Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  93%|█████████▎| 54/58 [00:01<00:00, 41.06it/s] Capturing num tokens (num_tokens=4 avail_mem=118.87 GB):  93%|█████████▎| 54/58 [00:01<00:00, 41.06it/s]Capturing num tokens (num_tokens=4 avail_mem=118.87 GB): 100%|██████████| 58/58 [00:01<00:00, 36.34it/s]


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
    Generated text:  James White. I'm a free spirit who grew up on the West Coast and I currently live in Los Angeles, California, where I work at a consulting firm and teach my own company, The White Company, which produces custom wedding invitations. I'm also a part-time writer for the Los Angeles Times and a blogger for a nonprofit blog called DIY Chic.
    I'm here to share some of the many differences between "real" and "miniature" weddings. I do this by speaking with real people about weddings they've attended. I take notes on them, write about them, and even give them a love letter. The result is a
    ===============================
    Prompt: The president of the United States is
    Generated text:  elected for a term of 4 years. If Barack Obama won 78% of the 4 year terms of elected officials, how many terms did he serve? To determine how many terms Barack Obama served as president, we need to follow these steps:
    
    1. **Understand the total term length and the winning percentage:**
       - The term length for the presidency is 4 years.
       - Barack Obama won 78% of the 4-year terms.
    
    2. **Convert the percentage to a decimal:**
       \[
       78\% = \frac{78}{100} =
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. The main port of entry for French citizens, and also for visitors, is the terminal building of the International Organization of Marine Agencies. It is one of the largest facilities of its type in Europe. The terminal building houses the Marine Agency's support structures and offices and is located in the 12th arrondissement of Paris.
    
    Based on that paragraph can we conclude that this sentence is true?
    The Marine Agency's support structures and offices are located in the 12th arrondissement of Paris.
    
    OPTIONS:
    [a]. yes
    [b]. it is not possible to tell
    [c]. no
    [a]. yes
    
    The paragraph
    ===============================
    Prompt: The future of AI is
    Generated text:  uncertain. It will be essential for the continued success and success of society, and it will be the basis for our future. So what should we do?
    In this talk, Professor Emeritus of the Department of Computer Science, Feng Yang, will address the topic of Artificial Intelligence, which is an important issue for the future of mankind. He will describe his ideas and contribute to making the future of AI brighter and more bright.
    As a professor, Feng Yang's research area is on the design and development of machine learning and artificial intelligence. He is also a professor of the Department of Computer Science and Technology, Zhongnan University of Technology,


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm excited to meet you and learn more about your career and interests. How can I assist you today? [Name] [Company Name] is a [brief description of the company]. We are looking for someone like you to join our team and make a positive impact on the world. What can I do for you today? [Name] [Company Name] is a [brief description of the company]. We are looking for someone like you to join our team and make a positive impact on the world. What can I do for you today? [Name
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a cultural and historical center with a rich history dating back to the Middle Ages. Paris is a major transportation hub and a major tourist destination, with its famous landmarks and attractions attracting millions of visitors each year. It is also home to many renowned museums, including the Musée d'Orsay and the Musée Rodin. The city is known for its cuisine, including its famous croissants and its traditional French cuisine. Paris is a city of contrasts, with its modern architecture and high-tech
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior and decision-making processes. This could lead to more sophisticated and adaptive AI systems that can better understand and respond to human emotions and motivations.
    
    2. Greater emphasis on ethical considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical considerations and responsible development. This could lead to more stringent regulations and standards for
    


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
    Generated text:  [Name] and I'm a [Current Occupation/Experience]. I'm an [Age/Experience] who has been [Your Experience]. I'm passionate about [My Passion/Interest/Job], and I am always looking for [My Objective/Goal/Challenge]. I'm a [Your Hobby] with [Your Hobby] in my free time. I love [My Hobby/Interest/Activity] because [My Hobby/Interest/Activity] is [My Hobby/Interest/Activity] to me. I'm [Your Characteristic/Feature/Adjective]. I'm [Your Characteristic/Feature/Adjective] and I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, the world-famous city located in the northwestern part of the country. The capital city is home to over 10 million people, and is known for its iconic Eiffel Tower, the Louvre Museum, the Notre-Dame Cathedral, and the Notre Dame du Haut Basilica. It is also home to the French Parliament, the European Parliament, the European Commission, and the French Broadcasting Corporation. France's capital is considered to be one of the most important and recognizable cities in the world, with a rich history and culture that has been shaped by its many historical and cultural landmarks. France's capital city of Paris is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be dominated by several key trends, including:
    
    1. Autonomous agents: As AI becomes more capable, more machines will be able to make decisions without human intervention. This will lead to autonomous agents, which can perform tasks on their own, such as driving a car, working at a factory, or even taking care of patients.
    
    2. Improved predictive analytics: AI will become more accurate in predicting outcomes, which will allow businesses to make better decisions. This will lead to the use of predictive analytics in a wider range of industries, such as finance, healthcare, and retail.
    
    3. Quantum computing: Quantum computing will be a significant driver


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

     Emily

     and

     I

    'm

     a

     diverse

    ,

     adventurous

    ,

     and

     nurturing

     person

    .

     I

     love

     to

     travel

     and

     explore

     new

     things

    ,

     and

     I

    'm

     always

     up

     for

     a

     new

     challenge

    .

     I

     have

     a

     passion

     for

     teaching

     and

     learning

    ,

     and

     I

     believe

     that

     knowledge

     is

     the

     key

     to

     unlocking

     potential

    .

     I

    'm

     excited

     to

     start

     a

     new

     chapter

     in

     my

     life

     and

     am

     looking

     forward

     to

     meeting

     all

     new

     people

    .

     I

    'm

     available

     for

     interviews

     and

     can

    't

     wait

     to

     share

     my

     experiences

    .

     I

    'm

     confident

     in

     my

     abilities

     and

     I

    'm

     always

     eager

     to

     learn

     more

    .

     I

    'm

     excited

     to

     meet

     you

    !

     What

     can

     I

     expect

     from

     our

     conversation

    ?

     (

    optional

    )

     I

    'm

     ready

     to

     start

    !

     How

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     located

     in

     the

     south

     of

     the

     country

    .

     It

     is

     the

     largest

     city

     in

     France

     and

     is

     the

     seat

     of

     government

     and

     the

     country

    ’s

     cultural

    ,

     political

    ,

     and

     economic

     center

    .

     Paris

     has

     been

     a

     major

     cultural

     and

     artistic

     center

     for

     centuries

     and

     is

     known

     for

     its

     iconic

     landmarks

     such

     as

     the

     E

    iff

    el

     Tower

    ,

     the

     Lou

    vre

     Museum

    ,

     and

     Notre

    -D

    ame

     Cathedral

    .

     It

     is

     also

     home

     to

     the

     E

    iff

    el

     Tower

    ,

     a

     beloved

     landmark

     and

     a

     symbol

     of

     French

     culture

     and

     history

    .

     In

     addition

     to

     its

     cultural

     attractions

    ,

     Paris

     is

     a

     popular

     destination

     for

     tourists

     and

     business

     travelers

     seeking

     a

     taste

     of

     French

     life

     and

     cuisine

    .

     
    


    Therefore

    ,

     the

     answer

     to

     the

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     anticipated

    ,

     and

     it

    's

     difficult

     to

     predict

     exactly

     what

     the

     next

     

    1

    0

     years

     will

     bring

    .

     However

    ,

     some

     possible

     future

     trends

     that

     are

     commonly

     observed

     include

    :
    


    1

    .

     Increased

     transparency

    :

     As

     AI

     continues

     to

     become

     more

     sophisticated

    ,

     it

    's

     becoming

     clear

     that

     it

    's

     important

     to

     maintain

     transparency

     in

     its

     use

    .

     This

     means

     that

     developers

     will

     need

     to

     be

     more

     careful

     about

     how

     their

     systems

     are

     designed

     and

     how

     they

    're

     used

    ,

     and

     consumers

     will

     need

     to

     be

     more

     aware

     of

     the

     potential

     risks

     associated

     with

     AI

    .
    


    2

    .

     Personal

    ization

    :

     As

     AI

     continues

     to

     learn

     and

     improve

    ,

     it

    's

     becoming

     possible

     to

     tailor

     AI

     systems

     to

     individual

     users

     in

     ways

     that

     are

     not

     yet

    



```python
llm.shutdown()
```
