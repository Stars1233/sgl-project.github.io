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


    2026-04-01 15:56:23.681 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:56:23] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 15:56:23.681 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:56:23] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 15:56:23.681 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:56:23] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 15:56:23.682 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:56:23] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 15:56:23.682 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:56:23] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  3.57it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  3.56it/s]


    2026-04-01 15:56:26,752 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 15:56:26] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:35,  2.72s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:35,  2.72s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:35,  2.72s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:35,  2.72s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.65it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.65it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.65it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.65it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.65it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.65it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:03<00:08,  5.65it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:03<00:08,  5.65it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:08,  5.65it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 11.98it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 11.98it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 11.98it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 11.98it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 11.98it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 11.98it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 11.98it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 11.98it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.03it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.03it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.03it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.03it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.03it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.03it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.03it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.03it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.66it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.66it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.66it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.66it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.66it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.66it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.66it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.72it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.72it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.72it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.72it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.72it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.72it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.72it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.17it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.17it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.17it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.17it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.17it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.17it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.17it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.67it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.33 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.16it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.16it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.16it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.16it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.42it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.42it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.42it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.42it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.77it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.33 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.77it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.33 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.77it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.10 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.77it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.00 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.77it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=119.00 GB):  21%|██        | 12/58 [00:00<00:01, 28.80it/s]Capturing num tokens (num_tokens=3072 avail_mem=119.00 GB):  21%|██        | 12/58 [00:00<00:01, 28.80it/s]Capturing num tokens (num_tokens=2816 avail_mem=119.00 GB):  21%|██        | 12/58 [00:00<00:01, 28.80it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.99 GB):  21%|██        | 12/58 [00:00<00:01, 28.80it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.99 GB):  21%|██        | 12/58 [00:00<00:01, 28.80it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.98 GB):  21%|██        | 12/58 [00:00<00:01, 28.80it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.98 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.94it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.98 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.94it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.98 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.94it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.97 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.94it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.95 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.94it/s]

    Capturing num tokens (num_tokens=960 avail_mem=118.97 GB):  29%|██▉       | 17/58 [00:00<00:01, 33.94it/s] Capturing num tokens (num_tokens=960 avail_mem=118.97 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.79it/s]Capturing num tokens (num_tokens=896 avail_mem=118.96 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.79it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.79it/s]Capturing num tokens (num_tokens=768 avail_mem=118.96 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.79it/s]Capturing num tokens (num_tokens=704 avail_mem=118.95 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.79it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  38%|███▊      | 22/58 [00:00<00:00, 36.79it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.48it/s]Capturing num tokens (num_tokens=576 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.48it/s]Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.48it/s]Capturing num tokens (num_tokens=480 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.48it/s]

    Capturing num tokens (num_tokens=448 avail_mem=118.66 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.48it/s]Capturing num tokens (num_tokens=448 avail_mem=118.66 GB):  53%|█████▎    | 31/58 [00:00<00:00, 32.42it/s]Capturing num tokens (num_tokens=416 avail_mem=118.65 GB):  53%|█████▎    | 31/58 [00:00<00:00, 32.42it/s]Capturing num tokens (num_tokens=384 avail_mem=118.65 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.42it/s]Capturing num tokens (num_tokens=352 avail_mem=118.64 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.42it/s]

    Capturing num tokens (num_tokens=320 avail_mem=118.64 GB):  53%|█████▎    | 31/58 [00:01<00:00, 32.42it/s]Capturing num tokens (num_tokens=320 avail_mem=118.64 GB):  60%|██████    | 35/58 [00:01<00:00, 28.42it/s]Capturing num tokens (num_tokens=288 avail_mem=118.64 GB):  60%|██████    | 35/58 [00:01<00:00, 28.42it/s]Capturing num tokens (num_tokens=256 avail_mem=118.63 GB):  60%|██████    | 35/58 [00:01<00:00, 28.42it/s]Capturing num tokens (num_tokens=240 avail_mem=118.63 GB):  60%|██████    | 35/58 [00:01<00:00, 28.42it/s]Capturing num tokens (num_tokens=240 avail_mem=118.63 GB):  66%|██████▌   | 38/58 [00:01<00:00, 26.42it/s]Capturing num tokens (num_tokens=224 avail_mem=118.63 GB):  66%|██████▌   | 38/58 [00:01<00:00, 26.42it/s]

    Capturing num tokens (num_tokens=208 avail_mem=118.62 GB):  66%|██████▌   | 38/58 [00:01<00:00, 26.42it/s]Capturing num tokens (num_tokens=192 avail_mem=118.62 GB):  66%|██████▌   | 38/58 [00:01<00:00, 26.42it/s]Capturing num tokens (num_tokens=176 avail_mem=118.62 GB):  66%|██████▌   | 38/58 [00:01<00:00, 26.42it/s]Capturing num tokens (num_tokens=176 avail_mem=118.62 GB):  72%|███████▏  | 42/58 [00:01<00:00, 28.18it/s]Capturing num tokens (num_tokens=160 avail_mem=118.62 GB):  72%|███████▏  | 42/58 [00:01<00:00, 28.18it/s]Capturing num tokens (num_tokens=144 avail_mem=118.61 GB):  72%|███████▏  | 42/58 [00:01<00:00, 28.18it/s]Capturing num tokens (num_tokens=128 avail_mem=118.61 GB):  72%|███████▏  | 42/58 [00:01<00:00, 28.18it/s]Capturing num tokens (num_tokens=112 avail_mem=118.60 GB):  72%|███████▏  | 42/58 [00:01<00:00, 28.18it/s]Capturing num tokens (num_tokens=112 avail_mem=118.60 GB):  79%|███████▉  | 46/58 [00:01<00:00, 30.73it/s]Capturing num tokens (num_tokens=96 avail_mem=118.60 GB):  79%|███████▉  | 46/58 [00:01<00:00, 30.73it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=118.60 GB):  79%|███████▉  | 46/58 [00:01<00:00, 30.73it/s]Capturing num tokens (num_tokens=64 avail_mem=118.59 GB):  79%|███████▉  | 46/58 [00:01<00:00, 30.73it/s]Capturing num tokens (num_tokens=48 avail_mem=118.59 GB):  79%|███████▉  | 46/58 [00:01<00:00, 30.73it/s]Capturing num tokens (num_tokens=48 avail_mem=118.59 GB):  86%|████████▌ | 50/58 [00:01<00:00, 32.36it/s]Capturing num tokens (num_tokens=32 avail_mem=118.59 GB):  86%|████████▌ | 50/58 [00:01<00:00, 32.36it/s]Capturing num tokens (num_tokens=28 avail_mem=118.58 GB):  86%|████████▌ | 50/58 [00:01<00:00, 32.36it/s]Capturing num tokens (num_tokens=24 avail_mem=118.58 GB):  86%|████████▌ | 50/58 [00:01<00:00, 32.36it/s]Capturing num tokens (num_tokens=20 avail_mem=118.57 GB):  86%|████████▌ | 50/58 [00:01<00:00, 32.36it/s]Capturing num tokens (num_tokens=20 avail_mem=118.57 GB):  93%|█████████▎| 54/58 [00:01<00:00, 34.20it/s]Capturing num tokens (num_tokens=16 avail_mem=118.57 GB):  93%|█████████▎| 54/58 [00:01<00:00, 34.20it/s]

    Capturing num tokens (num_tokens=12 avail_mem=118.57 GB):  93%|█████████▎| 54/58 [00:01<00:00, 34.20it/s]Capturing num tokens (num_tokens=8 avail_mem=118.57 GB):  93%|█████████▎| 54/58 [00:01<00:00, 34.20it/s] Capturing num tokens (num_tokens=4 avail_mem=118.56 GB):  93%|█████████▎| 54/58 [00:01<00:00, 34.20it/s]Capturing num tokens (num_tokens=4 avail_mem=118.56 GB): 100%|██████████| 58/58 [00:02<00:00, 24.42it/s]Capturing num tokens (num_tokens=4 avail_mem=118.56 GB): 100%|██████████| 58/58 [00:02<00:00, 28.87it/s]


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
    Generated text:  Kevin. I'm currently in my second year of college. I have been going to college for five years now, and have always had a great time. One thing I have always enjoyed doing is playing video games. I have been playing them for about five years. I have always gotten up early in the morning and start my day with a game, and I have found that playing video games has made my brain happy and keep me going.
    The other thing I like to do is to go for long walks around the school and relax. I have always known that I had to go to school because of my father's job. I have always
    ===============================
    Prompt: The president of the United States is
    Generated text:  a presidential candidate that has been in office for a certain number of years. He was in office for 27 years. How many years was he in office?
    To determine how many years the president of the United States was in office, we need to use the information given:
    
    1. The president is a presidential candidate.
    2. He has been in office for a certain number of years.
    3. The president was in office for 27 years.
    
    Since the president was in office for 27 years, the answer is:
    
    \[
    \boxed{27}
    \]
    ===============================
    Prompt: The capital of France is
    Generated text:  ________. A. Paris B. Brussels C. Lyon D. Nice
    A: Paris
    B: Brussels
    C: Lyon
    D: Nice
    
    To determine the capital of France, let's consider each option given:
    
    A. Paris - This is the capital city of France. It is located in the northwestern region of the country, near the Mediterranean Sea.
    
    B. Brussels - This is a city in Belgium, not part of France.
    
    C. Lyon - This is a city in France, but it is not the capital.
    
    D. Nice - This is a city in France, but it is not the capital.
    
    Based on
    ===============================
    Prompt: The future of AI is
    Generated text:  imminently here. In 2020, a groundbreaking research conducted by Microsoft researchers and researchers from the University of Oxford resulted in a breakthrough that could revolutionize the way we think about and use AI. The research, known as Azure AI, is a fascinating experiment that combines the best of both worlds – the precision of AI and the promise of big data and cloud computing.
    As you’ll see in this article, Azure AI is a system that allows researchers to analyze big datasets and learn from them. The system can identify patterns and relationships between different elements of a dataset, and use this information to make predictions and recommendations. This can


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm passionate about [job title] and [job title] at [company name]. I'm always looking for ways to [job title] and [job title] at [company name], and I'm always eager to learn and grow. I'm a [job title] at [company name], and I'm always looking for ways to [job title] and [job title] at
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French Parliament building. Paris is a cultural and historical center with a rich history dating back to the Roman Empire and the Middle Ages. It is also known for its fashion, art, and cuisine. The city is known for its annual festivals such as the Eiffel Tower Festival and the Parisian Carnival. Paris is a popular tourist destination and is home to many world-renowned museums, art galleries, and landmarks.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased integration with human intelligence: As AI becomes more advanced, it is likely to become more integrated with human intelligence, allowing it to learn and adapt to new situations and tasks. This could lead to more efficient and effective use of AI in various fields, such as healthcare, finance, and transportation.
    
    2. Greater emphasis on ethical and responsible AI: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical and responsible AI. This could include developing
    


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
    Generated text:  [Name] and I'm a [Occupation]. I'm [Age] years old, and I started my career in [Industry/Field]. I have [Number] years of experience, and I'm [Job Title]. I enjoy [Favorite Hobby/Activity] and I strive to be [Positive Aspirations]. I'm [Desired Outcome], and I'm [Character Type]. What's your background, and what do you bring to the table that sets you apart from others? If you could only have one superpower, what would it be, and why? Can you share any personal experiences that have shaped your character and
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, the most populous city and the largest metropolitan area in the world. It is also the seat of government, religion, culture, arts and education. It is situated on the Seine River and is famous for its many historical landmarks, including the Eiffel Tower. Paris is also renowned for its fashion industry, art scene, and food culture. It has a long and illustrious history dating back to ancient times and has seen many notable figures, including the French Revolution, Napoleon Bonaparte, and Victor Hugo. Today, Paris continues to be a vibrant and diverse city with a strong cultural identity and a global reputation. As of
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to involve continued advancements in areas such as artificial general intelligence, developing ethical considerations for AI, and improving the efficiency and scalability of AI systems. Here are some possible future trends in AI:
    
    1. Advanced Machine Learning: As the demand for AI continues to grow, we can expect to see more advanced machine learning techniques and algorithms that are capable of handling more complex and varied data sets.
    
    2. Improved Explainable AI: As AI systems become more complex, we may see an increase in the need for explainable AI, where we can see the underlying logic and decision-making processes behind the system's actions.
    
    3. Self-Driving Cars:


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

     [

    Age

    ]

     years

     old

    .

     I

     have

     always

     been

     fascinated

     by

     the

     history

     of

     [

    Name

    ],

     and

     I

    'm

     always

     eager

     to

     learn

     more

     about

     the

     past

    .

     I

     enjoy

     storytelling

     and

     my

     writing

     often

     uses

     the

     past

     tense

    ,

     as

     it

     allows

     for

     a

     more

     immersive

     and

     engaging

     experience

     for

     the

     reader

    .

     In

     my

     free

     time

    ,

     I

     enjoy

     exploring

     new

     places

    ,

     trying

     new

     foods

    ,

     and

     spending

     time

     with

     my

     family

    .

     I

     am

     confident

     in

     my

     abilities

     and

     excited

     to

     be

     a

     part

     of

     this

     dynamic

     and

     evolving

     community

    .

     How

     would

     you

     describe

     your

     personality

     and

     what

     makes

     you

     unique

    ?
    


    [

    Name

    ]

     is

     a

     versatile

     and

     adaptable

     individual

     with

     a

     keen

     interest

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     It

     is

     the

     largest

     city

     in

     Europe

     and

     the

     most

     populous

     city

     in

     the

     European

     Union

    .

     
    


    The

     city

     is

     situated

     on

     the

     banks

     of

     the

     River

     Se

    ine

    ,

     which

     serves

     as

     the

     city

    's

     main

     transportation

     artery

    .

     The

     city

     is

     known

     for

     its

     historic

     architecture

    ,

     artistic

     and

     cultural

     institutions

    ,

     and

     world

    -ren

    owned

     museums

    .

     Paris

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

     Lou

    vre

     Museum

    ,

     and

     the

     Notre

     Dame

     Cathedral

    ,

     among

     others

    .

     Its

     landmarks

     and

     historical

     significance

     continue

     to

     attract

     tourists

     from

     around

     the

     world

    .

     
    


    Paris

     is

     also

     renowned

     for

     its

     vibrant

     nightlife

    ,

     which

     attracts

     millions

     of

     visitors

     from

     all

     over

     the

     world

    .

     The

     city

     is

     also

     known

     for

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     uncertain

     and

     challenging

    .

     While

     the

     technology

     is

     rapidly

     advancing

    ,

     there

     are

     a

     few

     trends

     that

     seem

     likely

     to

     shape

     the

     future

     of

     AI

    :
    


    1

    .

     Integration

     of

     AI

     into

     everyday

     life

    :

     As

     AI

     becomes

     more

     prevalent

     in

     various

     sectors

    ,

     we

     are

     likely

     to

     see

     more

     AI

    -driven

     technologies

     integrated

     into

     our

     daily

     lives

    .

     This

     could

     include

     smart

     home

     devices

    ,

     autonomous

     vehicles

    ,

     and

     more

    .
    


    2

    .

     Increased

     focus

     on

     ethical

     AI

    :

     As

     more

     AI

     systems

     are

     developed

    ,

     we

     will

     likely

     see

     a

     greater

     emphasis

     on

     ethical

     considerations

    .

     This

     could

     include

     considerations

     of

     bias

    ,

     privacy

    ,

     and

     security

    .
    


    3

    .

     Development

     of

     new

     AI

     languages

    :

     AI

     is

     becoming

     more

     diverse

     and

     sophisticated

    ,

     which

    



```python
llm.shutdown()
```
