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


    2026-03-30 09:05:19.234 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 09:05:19] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 09:05:19.235 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 09:05:19] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 09:05:19.235 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 09:05:19] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 09:05:19.235 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 09:05:19] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 09:05:19.235 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 09:05:19] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.89it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.87it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.74it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 12.14it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 12.14it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 12.14it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 12.14it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 12.14it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 12.14it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 12.14it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 12.14it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.27it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.27it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.27it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.27it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.27it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.27it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.27it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.27it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.97it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.97it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.97it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.97it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.97it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.97it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.97it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.91it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.91it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.91it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.91it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.91it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.91it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.91it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.28it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.28it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.28it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.28it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.28it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.28it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.28it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.52it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.52it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.52it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.52it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.52it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.52it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.52it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:03<00:00, 38.40it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:03<00:00, 38.40it/s] 

    Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:03<00:00, 38.40it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.43it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=131.88 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=131.88 GB):   2%|▏         | 1/58 [00:00<00:06,  8.99it/s]Capturing num tokens (num_tokens=7680 avail_mem=131.85 GB):   2%|▏         | 1/58 [00:00<00:06,  8.99it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=131.85 GB):   3%|▎         | 2/58 [00:00<00:05,  9.35it/s]Capturing num tokens (num_tokens=7168 avail_mem=131.85 GB):   3%|▎         | 2/58 [00:00<00:05,  9.35it/s]Capturing num tokens (num_tokens=6656 avail_mem=131.85 GB):   3%|▎         | 2/58 [00:00<00:05,  9.35it/s]Capturing num tokens (num_tokens=6656 avail_mem=131.85 GB):   7%|▋         | 4/58 [00:00<00:04, 12.01it/s]Capturing num tokens (num_tokens=6144 avail_mem=131.85 GB):   7%|▋         | 4/58 [00:00<00:04, 12.01it/s]Capturing num tokens (num_tokens=5632 avail_mem=131.84 GB):   7%|▋         | 4/58 [00:00<00:04, 12.01it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=131.84 GB):  10%|█         | 6/58 [00:00<00:03, 14.69it/s]Capturing num tokens (num_tokens=5120 avail_mem=131.84 GB):  10%|█         | 6/58 [00:00<00:03, 14.69it/s]Capturing num tokens (num_tokens=4608 avail_mem=131.84 GB):  10%|█         | 6/58 [00:00<00:03, 14.69it/s]Capturing num tokens (num_tokens=4096 avail_mem=131.84 GB):  10%|█         | 6/58 [00:00<00:03, 14.69it/s]Capturing num tokens (num_tokens=4096 avail_mem=131.84 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.72it/s]Capturing num tokens (num_tokens=3840 avail_mem=131.84 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.72it/s]Capturing num tokens (num_tokens=3584 avail_mem=131.83 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.72it/s]Capturing num tokens (num_tokens=3328 avail_mem=131.82 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.72it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=131.82 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.72it/s]Capturing num tokens (num_tokens=3072 avail_mem=131.82 GB):  22%|██▏       | 13/58 [00:00<00:01, 23.59it/s]Capturing num tokens (num_tokens=2816 avail_mem=131.82 GB):  22%|██▏       | 13/58 [00:00<00:01, 23.59it/s]Capturing num tokens (num_tokens=2560 avail_mem=131.81 GB):  22%|██▏       | 13/58 [00:00<00:01, 23.59it/s]Capturing num tokens (num_tokens=2304 avail_mem=131.81 GB):  22%|██▏       | 13/58 [00:00<00:01, 23.59it/s]Capturing num tokens (num_tokens=2048 avail_mem=131.81 GB):  22%|██▏       | 13/58 [00:00<00:01, 23.59it/s]Capturing num tokens (num_tokens=2048 avail_mem=131.81 GB):  29%|██▉       | 17/58 [00:00<00:01, 27.81it/s]Capturing num tokens (num_tokens=1792 avail_mem=131.80 GB):  29%|██▉       | 17/58 [00:00<00:01, 27.81it/s]Capturing num tokens (num_tokens=1536 avail_mem=131.80 GB):  29%|██▉       | 17/58 [00:00<00:01, 27.81it/s]Capturing num tokens (num_tokens=1280 avail_mem=131.79 GB):  29%|██▉       | 17/58 [00:00<00:01, 27.81it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=131.78 GB):  29%|██▉       | 17/58 [00:00<00:01, 27.81it/s]Capturing num tokens (num_tokens=1024 avail_mem=131.78 GB):  36%|███▌      | 21/58 [00:00<00:01, 31.20it/s]Capturing num tokens (num_tokens=960 avail_mem=131.79 GB):  36%|███▌      | 21/58 [00:00<00:01, 31.20it/s] Capturing num tokens (num_tokens=896 avail_mem=131.79 GB):  36%|███▌      | 21/58 [00:00<00:01, 31.20it/s]Capturing num tokens (num_tokens=832 avail_mem=131.78 GB):  36%|███▌      | 21/58 [00:00<00:01, 31.20it/s]Capturing num tokens (num_tokens=768 avail_mem=131.78 GB):  36%|███▌      | 21/58 [00:00<00:01, 31.20it/s]Capturing num tokens (num_tokens=768 avail_mem=131.78 GB):  43%|████▎     | 25/58 [00:01<00:00, 33.25it/s]Capturing num tokens (num_tokens=704 avail_mem=131.78 GB):  43%|████▎     | 25/58 [00:01<00:00, 33.25it/s]Capturing num tokens (num_tokens=640 avail_mem=131.77 GB):  43%|████▎     | 25/58 [00:01<00:00, 33.25it/s]Capturing num tokens (num_tokens=576 avail_mem=131.77 GB):  43%|████▎     | 25/58 [00:01<00:00, 33.25it/s]

    Capturing num tokens (num_tokens=512 avail_mem=131.76 GB):  43%|████▎     | 25/58 [00:01<00:00, 33.25it/s]Capturing num tokens (num_tokens=480 avail_mem=131.78 GB):  43%|████▎     | 25/58 [00:01<00:00, 33.25it/s]Capturing num tokens (num_tokens=480 avail_mem=131.78 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.19it/s]Capturing num tokens (num_tokens=448 avail_mem=131.78 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.19it/s]Capturing num tokens (num_tokens=416 avail_mem=131.77 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.19it/s]Capturing num tokens (num_tokens=384 avail_mem=131.77 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.19it/s]Capturing num tokens (num_tokens=352 avail_mem=131.77 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.19it/s]Capturing num tokens (num_tokens=320 avail_mem=131.76 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.19it/s]Capturing num tokens (num_tokens=320 avail_mem=131.76 GB):  60%|██████    | 35/58 [00:01<00:00, 38.95it/s]Capturing num tokens (num_tokens=288 avail_mem=131.76 GB):  60%|██████    | 35/58 [00:01<00:00, 38.95it/s]Capturing num tokens (num_tokens=256 avail_mem=131.76 GB):  60%|██████    | 35/58 [00:01<00:00, 38.95it/s]Capturing num tokens (num_tokens=240 avail_mem=131.75 GB):  60%|██████    | 35/58 [00:01<00:00, 38.95it/s]

    Capturing num tokens (num_tokens=224 avail_mem=131.75 GB):  60%|██████    | 35/58 [00:01<00:00, 38.95it/s]Capturing num tokens (num_tokens=208 avail_mem=131.75 GB):  60%|██████    | 35/58 [00:01<00:00, 38.95it/s]Capturing num tokens (num_tokens=208 avail_mem=131.75 GB):  69%|██████▉   | 40/58 [00:01<00:00, 40.98it/s]Capturing num tokens (num_tokens=192 avail_mem=131.75 GB):  69%|██████▉   | 40/58 [00:01<00:00, 40.98it/s]Capturing num tokens (num_tokens=176 avail_mem=131.74 GB):  69%|██████▉   | 40/58 [00:01<00:00, 40.98it/s]Capturing num tokens (num_tokens=160 avail_mem=131.74 GB):  69%|██████▉   | 40/58 [00:01<00:00, 40.98it/s]Capturing num tokens (num_tokens=144 avail_mem=131.17 GB):  69%|██████▉   | 40/58 [00:01<00:00, 40.98it/s]

    Capturing num tokens (num_tokens=128 avail_mem=131.70 GB):  69%|██████▉   | 40/58 [00:01<00:00, 40.98it/s]Capturing num tokens (num_tokens=128 avail_mem=131.70 GB):  78%|███████▊  | 45/58 [00:01<00:00, 31.04it/s]Capturing num tokens (num_tokens=112 avail_mem=131.24 GB):  78%|███████▊  | 45/58 [00:01<00:00, 31.04it/s]Capturing num tokens (num_tokens=96 avail_mem=131.69 GB):  78%|███████▊  | 45/58 [00:01<00:00, 31.04it/s] Capturing num tokens (num_tokens=80 avail_mem=131.69 GB):  78%|███████▊  | 45/58 [00:01<00:00, 31.04it/s]

    Capturing num tokens (num_tokens=64 avail_mem=131.62 GB):  78%|███████▊  | 45/58 [00:01<00:00, 31.04it/s]Capturing num tokens (num_tokens=64 avail_mem=131.62 GB):  84%|████████▍ | 49/58 [00:01<00:00, 26.23it/s]Capturing num tokens (num_tokens=48 avail_mem=131.69 GB):  84%|████████▍ | 49/58 [00:01<00:00, 26.23it/s]Capturing num tokens (num_tokens=32 avail_mem=131.31 GB):  84%|████████▍ | 49/58 [00:01<00:00, 26.23it/s]Capturing num tokens (num_tokens=28 avail_mem=131.68 GB):  84%|████████▍ | 49/58 [00:01<00:00, 26.23it/s]

    Capturing num tokens (num_tokens=28 avail_mem=131.68 GB):  90%|████████▉ | 52/58 [00:01<00:00, 25.10it/s]Capturing num tokens (num_tokens=24 avail_mem=131.63 GB):  90%|████████▉ | 52/58 [00:01<00:00, 25.10it/s]Capturing num tokens (num_tokens=20 avail_mem=131.70 GB):  90%|████████▉ | 52/58 [00:02<00:00, 25.10it/s]Capturing num tokens (num_tokens=16 avail_mem=131.66 GB):  90%|████████▉ | 52/58 [00:02<00:00, 25.10it/s]

    Capturing num tokens (num_tokens=16 avail_mem=131.66 GB):  95%|█████████▍| 55/58 [00:02<00:00, 21.45it/s]Capturing num tokens (num_tokens=12 avail_mem=131.37 GB):  95%|█████████▍| 55/58 [00:02<00:00, 21.45it/s]Capturing num tokens (num_tokens=8 avail_mem=131.66 GB):  95%|█████████▍| 55/58 [00:02<00:00, 21.45it/s] Capturing num tokens (num_tokens=4 avail_mem=131.66 GB):  95%|█████████▍| 55/58 [00:02<00:00, 21.45it/s]Capturing num tokens (num_tokens=4 avail_mem=131.66 GB): 100%|██████████| 58/58 [00:02<00:00, 21.80it/s]Capturing num tokens (num_tokens=4 avail_mem=131.66 GB): 100%|██████████| 58/58 [00:02<00:00, 25.45it/s]


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
    Generated text:  Bong. I'm now 12 years old and I am currently taking French classes at the local school. I love my class because it's not only fun but also it teaches me how to use vocabulary properly and also how to be good at learning a new language. I'm still very young, but I have already learned a lot. 
    
    How can I improve my French vocabulary? Can you suggest any tips or resources that could help me improve my vocabulary? 
    
    Thanks! Bong
    Sure, Bong! Improving French vocabulary is a great goal. Here are a few tips and resources that might help you:
    
    1. Focus
    ===============================
    Prompt: The president of the United States is
    Generated text:  running for a third term. He has a fixed salary of $800,000 per year. He also earns an additional $200,000 in his first term and an additional $300,000 in his second term. If the president has a total of 20 years of service, what is the highest salary he could still receive for his third term?
    To determine the highest salary the president could still receive for his third term, we need to calculate his total current salary and subtract it from the maximum salary he could receive for his third term, which is $800
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris.
    
    Translate to Czech The capital of France is Paris. (Czech) 
    
    Translation: Párova capitána Francie je Paris. (Czech) 
    
    This translation conveys the same meaning as the original statement, which is that the capital of France is Paris. The Czech translation maintains the same structure and meaning as the English version. 
    
    Note: I provided the Czech translation assuming that you are interested in learning or writing in the Czech language. If you have any specific Czech language questions or need a translation for a particular context, please let me know. I'm happy to assist further in Czech language-related matters.
    ===============================
    Prompt: The future of AI is
    Generated text:  a very complex and advanced world. Artificial Intelligence has been around since the 1950s. It started as a computer program that can perform tasks like copying text and simple tasks like translating language. We've seen the implementation of deep learning algorithms that can perform very complex tasks like making decisions and visual recognition. But there's still a lot of work to be done. Over the past 10 years, AI has experienced a boom. Many researchers have been working on new algorithms and programming languages that can enable AI to perform tasks that humans can't. But even with these advancements, there are still a lot of challenges ahead. To


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


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also home to the French Parliament, the French Academy of Sciences, and the French Riviera. Paris is a cultural and economic hub, known for its fashion, art, and cuisine. It is also home to the world's largest library, the Bibliothèque nationale de France. The city is known for its romantic ambiance, with its cafes, bistros, and nightclubs. Paris is a popular tourist destination, with millions of visitors annually. It is also home to the French Parliament, the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way that AI is used and developed. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased focus on ethical considerations: As AI becomes more integrated into our daily lives, there will be a greater emphasis on ethical considerations. This will include issues such as bias, transparency, accountability, and the impact of AI on society.
    
    2. Greater use of AI in healthcare: AI is already being used in healthcare to help diagnose and treat diseases, and there is a lot of potential for further development in this area. AI could
    


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
    Generated text:  [Your Name], and I'm a [Your Profession or Interest]. I'm a [Your Age] year old [Your Speciality or Expertise] with a [Your passion or hobby]. I'm known for my [Your talents or skills] and love to [Your hobbies or interests]. What is your favorite [Your hobby or activity]?
    
    Hello, my name is [Your Name], and I'm a [Your Profession or Interest]. I'm a [Your Age] year old [Your Speciality or Expertise] with a [Your passion or hobby]. I'm known for my [Your talents or skills] and love to
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    Paris is the capital city of France. Here are some details:
    
    1. Location: It is located in the northeast of France, on the Loire River in the south of the country.
    2. Population: As of the 2021 French census, Paris had a population of approximately 2.2 million people.
    3. Capital: It is the oldest capital of France, having been the seat of the French monarchy since the 12th century.
    4. Climate: Paris enjoys a temperate climate with mild winters and hot summers.
    5. Key landmarks: The Louvre Museum, the Eiffel Tower
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be shaped by a number of key trends, including the increasing complexity of AI, its integration with other technologies, and the development of new ethical and regulatory frameworks. Here are some possible future trends in AI:
    
    1. Increased transparency and accountability: As AI systems become more integrated into our daily lives, it is important that they are transparent and accountable to their users. This means that we need to be able to understand how AI systems work and who is responsible for their decisions.
    
    2. Personalization and customization: AI is already becoming more personalized and customized to individual users. As AI continues to advance, it is likely that we will see


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

     John

     Smith

    .

     I

    'm

     an

     entry

    -level

     software

     engineer

     working

     on

     a

     team

     at

     a

     tech

     startup

    .

     What

     do

     you

     think

     about

     working

     there

    ?

     Hi

     John

    ,

     glad

     to

     meet

     you

    .

     I

     think

     it

    's

     a

     great

     place

     for

     an

     entry

    -level

     software

     engineer

     to

     work

     because

     the

     team

     is

     very

     supportive

     and

     the

     culture

     is

     very

     dynamic

    .

     What

     do

     you

     think

     about

     working

     there

    ?

     John

    ,

     thank

     you

     for

     stopping

     by

    .

     I

    'm

     really

     impressed

     by

     your

     work

     ethic

     and

     the

     level

     of

     dedication

     and

     creativity

     that

     you

     bring

     to

     the

     job

    .

     What

    's

     your

     advice

     for

     someone

     like

     you

     seeking

     a

     similar

     opportunity

    ?

     John

    ,

     I

     would

     advise

     someone

     like

     you

     to

     focus

     on

     building

     your

     skills

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     known

     as

     the

     City

     of

     Love

    .
    


    This

     statement

     is

     concise

    ,

     factual

    ,

     and

     accurately

     describes

     the

     capital

     city

     of

     France

    .

     However

    ,

     it

     could

     be

     expanded

     to

     include

     additional

     information

     about

     the

     city

    's

     historical

     significance

    ,

     notable

     landmarks

    ,

     and

     cultural

     attractions

    .

     For

     example

    :
    


    The

     capital

     of

     France

     is

     Paris

    ,

     known

     as

     the

     City

     of

     Love

    .

     Paris

     is

     renowned

     for

     its

     historical

     significance

    ,

     including

     the

     birth

    place

     of

     French

     democracy

    ,

     the

     ancient

     Roman

     Forum

    ,

     the

     Lou

    vre

     Museum

    ,

     and

     the

     E

    iff

    el

     Tower

    .

     Its

     notable

     landmarks

     include

     the

     E

    iff

    el

     Tower

    ,

     the

     Palace

     of

     Vers

    ailles

    ,

     and

     the

     Notre

    -D

    ame

     Cathedral

    .

     Paris

     is

     also

     known

     for

     its

    
    
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

     different

     trends

     and

     developments

    .

     Here

     are

     some

     potential

     areas

     of

     focus

    :
    


    1

    .

     Increased

     machine

     learning

     and

     artificial

     intelligence

    :

     One

     of

     the

     most

     exciting

     areas

     of

     AI

     development

     is

     the

     continued

     advancement

     of

     machine

     learning

     and

     artificial

     intelligence

    .

     This

     includes

     the

     ability

     to

     train

     AI

     systems

     to

     learn

     from

     and

     adapt

     to

     new

     situations

    ,

     as

     well

     as

     the

     development

     of

     more

     sophisticated

     algorithms

     and

     techniques

    .
    


    2

    .

     Improved

     efficiency

     and

     cost

    -effect

    iveness

    :

     As

     AI

     becomes

     more

     integrated

     into

     various

     industries

    ,

     there

     will

     be

     an

     increasing

     emphasis

     on

     improving

     its

     efficiency

     and

     cost

    -effect

    iveness

    .

     This

     could

     involve

     developing

     new

     algorithms

     and

     techniques

     for

     managing

     data

    ,

     optimizing

     processes

    ,

     and

    



```python
llm.shutdown()
```
