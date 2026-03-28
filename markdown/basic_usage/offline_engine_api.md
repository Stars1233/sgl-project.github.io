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


    2026-03-28 09:35:19.505 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 09:35:19] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 09:35:19.505 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 09:35:19] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 09:35:19.505 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 09:35:19] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 09:35:19.505 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 09:35:19] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 09:35:19.505 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 09:35:19] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.54it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.53it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<00:57,  1.02s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<00:57,  1.02s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<00:57,  1.02s/it]Compiling num tokens (num_tokens=6144):   3%|▎         | 2/58 [00:02<00:57,  1.02s/it]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:16,  3.15it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:16,  3.15it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:16,  3.15it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:16,  3.15it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:16,  3.15it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:16,  3.15it/s]Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:16,  3.15it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:05,  8.50it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:05,  8.50it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:05,  8.50it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:05,  8.50it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:05,  8.50it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:02<00:03, 12.18it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:02<00:03, 12.18it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:02<00:03, 12.18it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:02<00:03, 12.18it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:02<00:03, 12.18it/s]Compiling num tokens (num_tokens=1280):  26%|██▌       | 15/58 [00:02<00:03, 12.18it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:02<00:02, 17.21it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:02<00:02, 17.21it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:02<00:02, 17.21it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:02<00:02, 17.21it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:02<00:02, 17.21it/s]Compiling num tokens (num_tokens=768):  34%|███▍      | 20/58 [00:02<00:02, 17.21it/s]

    Compiling num tokens (num_tokens=704):  34%|███▍      | 20/58 [00:02<00:02, 17.21it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 23.71it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 23.71it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 23.71it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 23.71it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 23.71it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 23.71it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 23.71it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:00, 29.96it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:00, 29.96it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:00, 29.96it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:00, 29.96it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:00, 29.96it/s]

    Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:00, 29.96it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:03<00:00, 33.52it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:03<00:00, 33.52it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:03<00:00, 33.52it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:03<00:00, 33.52it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:03<00:00, 33.52it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:03<00:00, 33.52it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:03<00:00, 37.11it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:03<00:00, 37.11it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:03<00:00, 37.11it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:03<00:00, 37.11it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:03<00:00, 37.11it/s]

    Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:03<00:00, 37.11it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:03<00:00, 40.21it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:03<00:00, 40.21it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:03<00:00, 40.21it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:03<00:00, 40.21it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:03<00:00, 40.21it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:03<00:00, 40.21it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:03<00:00, 40.21it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:03<00:00, 43.83it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:03<00:00, 43.83it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:03<00:00, 43.83it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:03<00:00, 43.83it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:03<00:00, 43.83it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:03<00:00, 43.83it/s]

    Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.04it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.87 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.87 GB):   2%|▏         | 1/58 [00:00<00:06,  9.02it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.84 GB):   2%|▏         | 1/58 [00:00<00:06,  9.02it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.84 GB):   2%|▏         | 1/58 [00:00<00:06,  9.02it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=118.84 GB):   5%|▌         | 3/58 [00:00<00:04, 13.13it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.84 GB):   5%|▌         | 3/58 [00:00<00:04, 13.13it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.84 GB):   5%|▌         | 3/58 [00:00<00:04, 13.13it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.83 GB):   5%|▌         | 3/58 [00:00<00:04, 13.13it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.83 GB):  10%|█         | 6/58 [00:00<00:02, 18.89it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.83 GB):  10%|█         | 6/58 [00:00<00:02, 18.89it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.83 GB):  10%|█         | 6/58 [00:00<00:02, 18.89it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.83 GB):  10%|█         | 6/58 [00:00<00:02, 18.89it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=118.83 GB):  10%|█         | 6/58 [00:00<00:02, 18.89it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.83 GB):  17%|█▋        | 10/58 [00:00<00:01, 24.99it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.82 GB):  17%|█▋        | 10/58 [00:00<00:01, 24.99it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.82 GB):  17%|█▋        | 10/58 [00:00<00:01, 24.99it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.81 GB):  17%|█▋        | 10/58 [00:00<00:01, 24.99it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.81 GB):  17%|█▋        | 10/58 [00:00<00:01, 24.99it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.81 GB):  17%|█▋        | 10/58 [00:00<00:01, 24.99it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.81 GB):  26%|██▌       | 15/58 [00:00<00:01, 31.39it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.81 GB):  26%|██▌       | 15/58 [00:00<00:01, 31.39it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.80 GB):  26%|██▌       | 15/58 [00:00<00:01, 31.39it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.80 GB):  26%|██▌       | 15/58 [00:00<00:01, 31.39it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=118.80 GB):  26%|██▌       | 15/58 [00:00<00:01, 31.39it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.80 GB):  33%|███▎      | 19/58 [00:00<00:01, 33.22it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.79 GB):  33%|███▎      | 19/58 [00:00<00:01, 33.22it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.77 GB):  33%|███▎      | 19/58 [00:00<00:01, 33.22it/s]Capturing num tokens (num_tokens=960 avail_mem=118.79 GB):  33%|███▎      | 19/58 [00:00<00:01, 33.22it/s] Capturing num tokens (num_tokens=896 avail_mem=118.78 GB):  33%|███▎      | 19/58 [00:00<00:01, 33.22it/s]Capturing num tokens (num_tokens=896 avail_mem=118.78 GB):  40%|███▉      | 23/58 [00:00<00:01, 32.43it/s]Capturing num tokens (num_tokens=832 avail_mem=118.78 GB):  40%|███▉      | 23/58 [00:00<00:01, 32.43it/s]Capturing num tokens (num_tokens=768 avail_mem=118.78 GB):  40%|███▉      | 23/58 [00:00<00:01, 32.43it/s]

    Capturing num tokens (num_tokens=704 avail_mem=118.77 GB):  40%|███▉      | 23/58 [00:00<00:01, 32.43it/s]Capturing num tokens (num_tokens=640 avail_mem=118.77 GB):  40%|███▉      | 23/58 [00:00<00:01, 32.43it/s]Capturing num tokens (num_tokens=640 avail_mem=118.77 GB):  47%|████▋     | 27/58 [00:00<00:00, 32.33it/s]Capturing num tokens (num_tokens=576 avail_mem=118.77 GB):  47%|████▋     | 27/58 [00:00<00:00, 32.33it/s]Capturing num tokens (num_tokens=512 avail_mem=118.76 GB):  47%|████▋     | 27/58 [00:00<00:00, 32.33it/s]Capturing num tokens (num_tokens=480 avail_mem=118.77 GB):  47%|████▋     | 27/58 [00:01<00:00, 32.33it/s]Capturing num tokens (num_tokens=448 avail_mem=118.77 GB):  47%|████▋     | 27/58 [00:01<00:00, 32.33it/s]Capturing num tokens (num_tokens=448 avail_mem=118.77 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.54it/s]Capturing num tokens (num_tokens=416 avail_mem=118.77 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.54it/s]

    Capturing num tokens (num_tokens=384 avail_mem=118.77 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.54it/s]Capturing num tokens (num_tokens=352 avail_mem=118.76 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.54it/s]Capturing num tokens (num_tokens=320 avail_mem=118.76 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.54it/s]Capturing num tokens (num_tokens=320 avail_mem=118.76 GB):  60%|██████    | 35/58 [00:01<00:00, 32.53it/s]Capturing num tokens (num_tokens=288 avail_mem=118.75 GB):  60%|██████    | 35/58 [00:01<00:00, 32.53it/s]Capturing num tokens (num_tokens=256 avail_mem=118.75 GB):  60%|██████    | 35/58 [00:01<00:00, 32.53it/s]Capturing num tokens (num_tokens=240 avail_mem=118.75 GB):  60%|██████    | 35/58 [00:01<00:00, 32.53it/s]Capturing num tokens (num_tokens=224 avail_mem=118.75 GB):  60%|██████    | 35/58 [00:01<00:00, 32.53it/s]

    Capturing num tokens (num_tokens=224 avail_mem=118.75 GB):  67%|██████▋   | 39/58 [00:01<00:00, 32.76it/s]Capturing num tokens (num_tokens=208 avail_mem=118.74 GB):  67%|██████▋   | 39/58 [00:01<00:00, 32.76it/s]Capturing num tokens (num_tokens=192 avail_mem=118.74 GB):  67%|██████▋   | 39/58 [00:01<00:00, 32.76it/s]Capturing num tokens (num_tokens=176 avail_mem=118.74 GB):  67%|██████▋   | 39/58 [00:01<00:00, 32.76it/s]Capturing num tokens (num_tokens=160 avail_mem=118.73 GB):  67%|██████▋   | 39/58 [00:01<00:00, 32.76it/s]Capturing num tokens (num_tokens=144 avail_mem=118.73 GB):  67%|██████▋   | 39/58 [00:01<00:00, 32.76it/s]Capturing num tokens (num_tokens=144 avail_mem=118.73 GB):  76%|███████▌  | 44/58 [00:01<00:00, 35.95it/s]Capturing num tokens (num_tokens=128 avail_mem=118.73 GB):  76%|███████▌  | 44/58 [00:01<00:00, 35.95it/s]Capturing num tokens (num_tokens=112 avail_mem=118.73 GB):  76%|███████▌  | 44/58 [00:01<00:00, 35.95it/s]Capturing num tokens (num_tokens=96 avail_mem=118.72 GB):  76%|███████▌  | 44/58 [00:01<00:00, 35.95it/s] Capturing num tokens (num_tokens=80 avail_mem=118.72 GB):  76%|███████▌  | 44/58 [00:01<00:00, 35.95it/s]

    Capturing num tokens (num_tokens=64 avail_mem=118.72 GB):  76%|███████▌  | 44/58 [00:01<00:00, 35.95it/s]Capturing num tokens (num_tokens=64 avail_mem=118.72 GB):  84%|████████▍ | 49/58 [00:01<00:00, 38.55it/s]Capturing num tokens (num_tokens=48 avail_mem=118.71 GB):  84%|████████▍ | 49/58 [00:01<00:00, 38.55it/s]Capturing num tokens (num_tokens=32 avail_mem=118.71 GB):  84%|████████▍ | 49/58 [00:01<00:00, 38.55it/s]Capturing num tokens (num_tokens=28 avail_mem=118.70 GB):  84%|████████▍ | 49/58 [00:01<00:00, 38.55it/s]Capturing num tokens (num_tokens=24 avail_mem=118.70 GB):  84%|████████▍ | 49/58 [00:01<00:00, 38.55it/s]Capturing num tokens (num_tokens=20 avail_mem=118.70 GB):  84%|████████▍ | 49/58 [00:01<00:00, 38.55it/s]Capturing num tokens (num_tokens=20 avail_mem=118.70 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.70it/s]Capturing num tokens (num_tokens=16 avail_mem=118.54 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.70it/s]Capturing num tokens (num_tokens=12 avail_mem=118.53 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.70it/s]Capturing num tokens (num_tokens=8 avail_mem=118.53 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.70it/s] Capturing num tokens (num_tokens=4 avail_mem=118.52 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.70it/s]

    Capturing num tokens (num_tokens=4 avail_mem=118.52 GB): 100%|██████████| 58/58 [00:01<00:00, 33.48it/s]


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
    Generated text:  Carter and I'm a dedicated health enthusiast who prefers to stay active through jogging, meditation and reading. I am a rock star at the gym and have never found a workout session dull. How can I improve my heart health by increasing my heart rate and exercising in a variety of ways? 
    One of my goals is to get to a heart rate of 170 beats per minute. I can't walk very fast and I can't do a lot of cardio. I need some guidance on how to increase my heart rate. Do you have any recommendations? Also, I'm looking to build up the flexibility of my chest so that I
    ===============================
    Prompt: The president of the United States is
    Generated text:  holding a press conference to announce a new policy. The policy involves a series of outcomes, with each outcome having a probability of 0.1. The president decides to implement the policy by making a decision on the outcome of each potential outcome based on a scoring system where:
    
    - An outcome with probability 0.1 is worth 1 point.
    - An outcome with probability 0.5 is worth 2 points.
    - An outcome with probability 0.4 is worth 3 points.
    - An outcome with probability 0.1 is worth 4 points.
    
    If the president decides to implement the policy, what is the expected
    ===============================
    Prompt: The capital of France is
    Generated text: :
    A) Paris
    B) Rome
    C) Moscow
    D) London
    The capital of France is:
    
    A) Paris
    
    Paris is the capital city of France, located in the central region of the country. It is the most populous city and the second largest metropolitan area in France. Paris has a rich history, culture, and architecture, and it is known for its时尚（时尚）and its famous landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. The city is also known for its romantic history, including the romantic legends of François-René de Chateaubriand and
    ===============================
    Prompt: The future of AI is
    Generated text:  uncertain. However, several key areas of research are likely to continue their growth and development in the coming years. Here are the top areas of research that are expected to have the most significant impact on AI in the coming years:
    
    1. Machine Learning: The field of machine learning is expected to continue to grow and evolve. This includes the development of new algorithms and techniques, as well as the exploration of new areas of research such as reinforcement learning and transfer learning.
    
    2. Natural Language Processing: Natural language processing (NLP) is expected to be a key area of research in the coming years. This includes the development of new technologies such as


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


    Generated text:  [Name] and I'm a [Age] year old [Occupation]. I'm a [Skill] with [Number] years of experience in [Field]. I'm a [Skill] with [Number] years of experience in [Field]. I'm a [Skill] with [Number] years of experience in [Field]. I'm a [Skill] with [Number] years of experience in [Field]. I'm a [Skill] with [Number] years of experience in [Field]. I'm a [Skill] with [Number] years of experience in [Field]. I'm a [Skill] with [Number]
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also home to the French Parliament and the French National Museum of Modern Art. Paris is a cultural and historical center with a rich history dating back to the Roman Empire and the French Revolution. It is a popular tourist destination and a major economic and financial center in Europe. The city is also known for its cuisine, including French cuisine and international cuisine. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly. It is a city of art, culture, and history that is a must
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn and adapt to human behavior and preferences. This could lead to more sophisticated and personalized AI systems that can better understand and respond to human needs.
    
    2. Greater emphasis on ethical and social implications: As AI becomes more integrated with human intelligence, there will be increased scrutiny of its ethical and social implications. This could lead to more stringent regulations and guidelines for AI development and deployment.
    
    3. Greater focus on AI ethics and safety: As AI becomes more integrated with human intelligence, there will be
    


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
    Generated text:  [name], and I am a [occupation]. I have always been an enthusiastic learner, always seeking to grow and understand the world around me. I have a keen curiosity that drives me to explore new ideas, cultures, and experiences. I am a perfectionist, always striving to improve myself and become the best version of myself possible. I am always looking for ways to make the world a better place, and I am constantly learning from others and adapting to new experiences. I am a friendly and outgoing person, and I have a great sense of humor. I am someone who values kindness, respect, and empathy, and I am always looking
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, located in the Île-de-France region of the country. Its status as the largest city in the country and its role as the cultural, financial, political, and commercial center of the country is one of the most famous and iconic features of the capital. The city is known for its towering Eiffel Tower, the Arc de Triomphe, and the Louvre Museum, among other landmarks. Paris is also home to numerous historic sites and museums, such as the Louvre, the Musée d'Orsay, and the Musée Rodin. The city is also renowned for its gastronomy, with many iconic
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  exciting and seems likely to continue to evolve rapidly. Here are some possible trends that could shape AI in the coming years:
    
    1. Increased accuracy and precision: As AI continues to improve, we're likely to see more accurate and precise predictions, diagnoses, and decisions. This could lead to applications like autonomous vehicles, medical diagnosis, and personalized medicine.
    
    2. Integration with human intelligence: As AI becomes more integrated with our existing human intelligence, we can expect to see more advanced applications like language translation, virtual assistants, and chatbots. However, this integration is likely to come with challenges like privacy and ethical concerns.
    
    3. Personalization: AI


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

     am

     an

     experienced

     [

    Field

     of

     Work

    /

    Interest

    ].

     I

     have

     been

     working

     in

     the

     [

    Field

     of

     Work

    /

    Interest

    ]

     field

     for

     over

     [

    Number

     of

     Years

    ]

     years

     now

    .

     I

     have

     always

     been

     passionate

     about

     [

    What

     I

     love

     about

     the

     field

    ]

     and

     have

     dedicated

     my

     entire

     life

     to

     making

     a

     difference

    .

     My

     goal

     is

     to

     continue

     to

     push

     the

     boundaries

     of

     my

     field

     and

     inspire

     others

     to

     do

     the

     same

    .

     What

     is

     your

     name

     and

     what

     field

     of

     work

     or

     interest

     do

     you

     have

    ?


    [

    Name

    ]:

     Welcome

     to

     [

    Name

    ],

     my

     self

    -int

    roduction

    !

     I

     am

     [

    Name

    ],

     an

     experienced

     [

    Field

     of

     Work

    /

    Interest

    ].

     I

     have

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     often

     referred

     to

     as

     the

     "

    City

     of

     Light

    "

     and

     "

    The

     Queen

     of

     Cities

    ."

     It

     is

     a

     historic

     and

     vibrant

     city

     located

     in

     the

     south

    -central

     region

     of

     France

    .

     Paris

     is

     known

     for

     its

     elegant

     architecture

    ,

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

     and

     its

     importance

     as

     the

     headquarters

     of

     major

     French

     companies

    ,

     including

     IBM

     and

     France

     Telecom

    .

     The

     city

     is

     also

     home

     to

     iconic

     attractions

     such

     as

     the

     Lou

    vre

     Museum

    ,

     the

     Arc

     de

     Tri

    omp

    he

    ,

     and

     the

     Ch

    amps

    -

    É

    lys

    ées

    ,

     as

     well

     as

     its

     annual

     E

    iff

    el

     Tower

     celebration

    ,

     which

     attracts

     millions

     of

     visitors

     each

     year

    .

     Paris

     is

     a

     UNESCO

     World

     Heritage

     Site

     and

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     shaped

     by

     a

     variety

     of

     factors

    ,

     including

     advances

     in

     machine

     learning

    ,

     deep

     learning

    ,

     and

     natural

     language

     processing

    .

     Here

     are

     some

     potential

     trends

     that

     could

     be

     expected

     in

     the

     near

     future

    :
    


    1

    .

     Increased

     autonomy

    :

     As

     AI

     systems

     become

     more

     sophisticated

    ,

     we

     may

     see

     an

     increase

     in

     their

     ability

     to

     make

     decisions

     that

     are

     more

     similar

     to

     human

     decision

    -making

    .

     This

     could

     lead

     to

     more

     autonomous

     systems

     being

     used

     in

     fields

     such

     as

     transportation

    ,

     healthcare

    ,

     and

     manufacturing

    .
    


    2

    .

     Improved

     transparency

    :

     AI

     systems

     are

     becoming

     more

     transparent

     as

     they

     are

     developed

     to

     incorporate

     more

     human

    -like

     features

     and

     to

     explain

     their

     decision

    -making

     processes

    .

     This

     could

     lead

     to

     greater

     trust

     between

     humans

     and

    



```python
llm.shutdown()
```
