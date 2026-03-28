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


    2026-03-28 01:59:54.738 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 01:59:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 01:59:54.738 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 01:59:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 01:59:54.738 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 01:59:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 01:59:54.739 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 01:59:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 01:59:54.739 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 01:59:54] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.86it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.85it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.39it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.39it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.39it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.39it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.39it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.39it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.39it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.39it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.39it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:02, 13.39it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:02, 13.39it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:02, 13.39it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:02, 13.39it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:02, 13.39it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:02, 13.39it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:02, 13.39it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:02, 13.39it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 19.93it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 19.93it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 19.93it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 19.93it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 19.93it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 19.93it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 19.93it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:02<00:01, 19.93it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 26.92it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 26.92it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 26.92it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:00, 26.92it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:00, 26.92it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:02<00:00, 26.92it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:02<00:00, 26.92it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 37.28it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 37.28it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 37.28it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 37.28it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 37.28it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 37.28it/s]

    Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 37.28it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 40.27it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 40.27it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 40.27it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 40.27it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 40.27it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 40.27it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 40.27it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:03<00:00, 42.09it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:03<00:00, 42.09it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:03<00:00, 42.09it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.01it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=131.88 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=131.85 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=131.85 GB):   3%|▎         | 2/58 [00:00<00:04, 11.72it/s]Capturing num tokens (num_tokens=7168 avail_mem=131.85 GB):   3%|▎         | 2/58 [00:00<00:04, 11.72it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=131.85 GB):   3%|▎         | 2/58 [00:00<00:04, 11.72it/s]Capturing num tokens (num_tokens=6656 avail_mem=131.85 GB):   7%|▋         | 4/58 [00:00<00:03, 14.12it/s]Capturing num tokens (num_tokens=6144 avail_mem=131.85 GB):   7%|▋         | 4/58 [00:00<00:03, 14.12it/s]Capturing num tokens (num_tokens=5632 avail_mem=131.84 GB):   7%|▋         | 4/58 [00:00<00:03, 14.12it/s]Capturing num tokens (num_tokens=5120 avail_mem=131.84 GB):   7%|▋         | 4/58 [00:00<00:03, 14.12it/s]Capturing num tokens (num_tokens=5120 avail_mem=131.84 GB):  12%|█▏        | 7/58 [00:00<00:02, 17.43it/s]Capturing num tokens (num_tokens=4608 avail_mem=131.84 GB):  12%|█▏        | 7/58 [00:00<00:02, 17.43it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=131.84 GB):  12%|█▏        | 7/58 [00:00<00:02, 17.43it/s]Capturing num tokens (num_tokens=3840 avail_mem=131.83 GB):  12%|█▏        | 7/58 [00:00<00:02, 17.43it/s]Capturing num tokens (num_tokens=3840 avail_mem=131.83 GB):  17%|█▋        | 10/58 [00:00<00:02, 21.25it/s]Capturing num tokens (num_tokens=3584 avail_mem=131.83 GB):  17%|█▋        | 10/58 [00:00<00:02, 21.25it/s]Capturing num tokens (num_tokens=3328 avail_mem=131.82 GB):  17%|█▋        | 10/58 [00:00<00:02, 21.25it/s]Capturing num tokens (num_tokens=3072 avail_mem=131.82 GB):  17%|█▋        | 10/58 [00:00<00:02, 21.25it/s]Capturing num tokens (num_tokens=2816 avail_mem=131.82 GB):  17%|█▋        | 10/58 [00:00<00:02, 21.25it/s]Capturing num tokens (num_tokens=2816 avail_mem=131.82 GB):  24%|██▍       | 14/58 [00:00<00:01, 25.50it/s]Capturing num tokens (num_tokens=2560 avail_mem=131.81 GB):  24%|██▍       | 14/58 [00:00<00:01, 25.50it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=131.81 GB):  24%|██▍       | 14/58 [00:00<00:01, 25.50it/s]Capturing num tokens (num_tokens=2048 avail_mem=131.81 GB):  24%|██▍       | 14/58 [00:00<00:01, 25.50it/s]Capturing num tokens (num_tokens=1792 avail_mem=131.80 GB):  24%|██▍       | 14/58 [00:00<00:01, 25.50it/s]Capturing num tokens (num_tokens=1792 avail_mem=131.80 GB):  31%|███       | 18/58 [00:00<00:01, 29.47it/s]Capturing num tokens (num_tokens=1536 avail_mem=131.80 GB):  31%|███       | 18/58 [00:00<00:01, 29.47it/s]Capturing num tokens (num_tokens=1280 avail_mem=131.79 GB):  31%|███       | 18/58 [00:00<00:01, 29.47it/s]Capturing num tokens (num_tokens=1024 avail_mem=131.78 GB):  31%|███       | 18/58 [00:00<00:01, 29.47it/s]Capturing num tokens (num_tokens=960 avail_mem=131.79 GB):  31%|███       | 18/58 [00:00<00:01, 29.47it/s] Capturing num tokens (num_tokens=960 avail_mem=131.79 GB):  38%|███▊      | 22/58 [00:00<00:01, 31.92it/s]Capturing num tokens (num_tokens=896 avail_mem=131.79 GB):  38%|███▊      | 22/58 [00:00<00:01, 31.92it/s]

    Capturing num tokens (num_tokens=832 avail_mem=131.78 GB):  38%|███▊      | 22/58 [00:00<00:01, 31.92it/s]Capturing num tokens (num_tokens=768 avail_mem=131.78 GB):  38%|███▊      | 22/58 [00:00<00:01, 31.92it/s]Capturing num tokens (num_tokens=704 avail_mem=131.78 GB):  38%|███▊      | 22/58 [00:00<00:01, 31.92it/s]Capturing num tokens (num_tokens=640 avail_mem=131.77 GB):  38%|███▊      | 22/58 [00:00<00:01, 31.92it/s]Capturing num tokens (num_tokens=640 avail_mem=131.77 GB):  47%|████▋     | 27/58 [00:00<00:00, 35.74it/s]Capturing num tokens (num_tokens=576 avail_mem=131.77 GB):  47%|████▋     | 27/58 [00:00<00:00, 35.74it/s]Capturing num tokens (num_tokens=512 avail_mem=131.76 GB):  47%|████▋     | 27/58 [00:01<00:00, 35.74it/s]Capturing num tokens (num_tokens=480 avail_mem=131.78 GB):  47%|████▋     | 27/58 [00:01<00:00, 35.74it/s]Capturing num tokens (num_tokens=448 avail_mem=131.78 GB):  47%|████▋     | 27/58 [00:01<00:00, 35.74it/s]Capturing num tokens (num_tokens=416 avail_mem=131.77 GB):  47%|████▋     | 27/58 [00:01<00:00, 35.74it/s]Capturing num tokens (num_tokens=416 avail_mem=131.77 GB):  55%|█████▌    | 32/58 [00:01<00:00, 38.64it/s]Capturing num tokens (num_tokens=384 avail_mem=131.77 GB):  55%|█████▌    | 32/58 [00:01<00:00, 38.64it/s]

    Capturing num tokens (num_tokens=352 avail_mem=131.77 GB):  55%|█████▌    | 32/58 [00:01<00:00, 38.64it/s]Capturing num tokens (num_tokens=320 avail_mem=131.76 GB):  55%|█████▌    | 32/58 [00:01<00:00, 38.64it/s]Capturing num tokens (num_tokens=288 avail_mem=131.76 GB):  55%|█████▌    | 32/58 [00:01<00:00, 38.64it/s]Capturing num tokens (num_tokens=256 avail_mem=131.76 GB):  55%|█████▌    | 32/58 [00:01<00:00, 38.64it/s]Capturing num tokens (num_tokens=256 avail_mem=131.76 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.50it/s]Capturing num tokens (num_tokens=240 avail_mem=131.75 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.50it/s]Capturing num tokens (num_tokens=224 avail_mem=131.75 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.50it/s]Capturing num tokens (num_tokens=208 avail_mem=131.18 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.50it/s]

    Capturing num tokens (num_tokens=192 avail_mem=131.72 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.50it/s]Capturing num tokens (num_tokens=176 avail_mem=131.25 GB):  64%|██████▍   | 37/58 [00:01<00:00, 40.50it/s]Capturing num tokens (num_tokens=176 avail_mem=131.25 GB):  72%|███████▏  | 42/58 [00:01<00:00, 29.27it/s]Capturing num tokens (num_tokens=160 avail_mem=131.70 GB):  72%|███████▏  | 42/58 [00:01<00:00, 29.27it/s]Capturing num tokens (num_tokens=144 avail_mem=131.51 GB):  72%|███████▏  | 42/58 [00:01<00:00, 29.27it/s]

    Capturing num tokens (num_tokens=128 avail_mem=131.30 GB):  72%|███████▏  | 42/58 [00:01<00:00, 29.27it/s]Capturing num tokens (num_tokens=112 avail_mem=131.70 GB):  72%|███████▏  | 42/58 [00:01<00:00, 29.27it/s]Capturing num tokens (num_tokens=112 avail_mem=131.70 GB):  79%|███████▉  | 46/58 [00:01<00:00, 24.98it/s]Capturing num tokens (num_tokens=96 avail_mem=131.32 GB):  79%|███████▉  | 46/58 [00:01<00:00, 24.98it/s] Capturing num tokens (num_tokens=80 avail_mem=131.69 GB):  79%|███████▉  | 46/58 [00:01<00:00, 24.98it/s]

    Capturing num tokens (num_tokens=64 avail_mem=131.34 GB):  79%|███████▉  | 46/58 [00:01<00:00, 24.98it/s]Capturing num tokens (num_tokens=64 avail_mem=131.34 GB):  84%|████████▍ | 49/58 [00:01<00:00, 23.74it/s]Capturing num tokens (num_tokens=48 avail_mem=131.68 GB):  84%|████████▍ | 49/58 [00:01<00:00, 23.74it/s]Capturing num tokens (num_tokens=32 avail_mem=131.68 GB):  84%|████████▍ | 49/58 [00:01<00:00, 23.74it/s]Capturing num tokens (num_tokens=28 avail_mem=131.39 GB):  84%|████████▍ | 49/58 [00:01<00:00, 23.74it/s]Capturing num tokens (num_tokens=28 avail_mem=131.39 GB):  90%|████████▉ | 52/58 [00:01<00:00, 23.26it/s]Capturing num tokens (num_tokens=24 avail_mem=131.67 GB):  90%|████████▉ | 52/58 [00:01<00:00, 23.26it/s]

    Capturing num tokens (num_tokens=20 avail_mem=131.67 GB):  90%|████████▉ | 52/58 [00:02<00:00, 23.26it/s]Capturing num tokens (num_tokens=16 avail_mem=131.68 GB):  90%|████████▉ | 52/58 [00:02<00:00, 23.26it/s]Capturing num tokens (num_tokens=16 avail_mem=131.68 GB):  95%|█████████▍| 55/58 [00:02<00:00, 23.62it/s]Capturing num tokens (num_tokens=12 avail_mem=131.44 GB):  95%|█████████▍| 55/58 [00:02<00:00, 23.62it/s]Capturing num tokens (num_tokens=8 avail_mem=131.65 GB):  95%|█████████▍| 55/58 [00:02<00:00, 23.62it/s] Capturing num tokens (num_tokens=4 avail_mem=131.65 GB):  95%|█████████▍| 55/58 [00:02<00:00, 23.62it/s]

    Capturing num tokens (num_tokens=4 avail_mem=131.65 GB): 100%|██████████| 58/58 [00:02<00:00, 23.83it/s]Capturing num tokens (num_tokens=4 avail_mem=131.65 GB): 100%|██████████| 58/58 [00:02<00:00, 26.10it/s]


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
    Generated text:  Megan. I have been working as a school librarian for 12 years now. I have a strong background in the library. I have been at the library since I was in elementary school. I have been very passionate about the library for many years. I have always wanted to be a librarian because the library is the largest employer in most libraries, and the most frequently used library. I have been involved with the library from when I was in elementary school, in kindergarten, and in first grade. In elementary school, I started helping my parents sort through the books. I then moved on to kindergarten and first grade. I was very happy
    ===============================
    Prompt: The president of the United States is
    Generated text:  a very important person. He or she is supposed to make important decisions. The president decides what to do with money. This money is given to the government. The president also decides what kind of jobs and jobs for people. When the president makes decisions, he or she usually works with other important people. The president can choose what kind of president he or she wants. He or she can work with the president of the other country. This is called the foreign policy. Some presidents are better than others in being good presidents. It is important to keep in mind that the president is the head of the government and he has a lot of important
    ===============================
    Prompt: The capital of France is
    Generated text:  [ ]
    
    A: Paris
    B: Versailles
    C: St. Germain
    D: Versailles Palace
    To determine the capital of France, we need to understand that the capital of France is typically the city that serves as the administrative center of the country. The capital of France is Paris.
    
    Here are the key points to consider:
    1. Paris is the largest city in France.
    2. It serves as the political, economic, and cultural capital of the country.
    3. Other cities in France, such as Lyon, Marseille, and Toulouse, also have significant populations but are not considered the capital in the same sense as
    ===============================
    Prompt: The future of AI is
    Generated text:  very exciting. In the last 10 years, we’ve seen the emergence of a multitude of new technologies and applications that are all poised to transform the way we live, work, and communicate. From the most basic forms of personal assistants to the most complex forms of AI, the possibilities are truly endless. One of the most promising areas of AI is the field of robotics, and it’s already seen significant growth and innovation in recent years.
    
    In this blog, we’ll explore some of the exciting new technologies and applications that are changing the face of robotics and AI in 2022. Whether you’re a robotics engineer, a


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a short description of your profession or role]. I enjoy [insert a short description of your hobbies or interests]. I'm always looking for new challenges and opportunities to grow and learn. What's your favorite hobby or activity? I'm always up for a good challenge and love to explore new experiences. What's your favorite book or movie? I love to read and watch movies, and I'm always looking for new and exciting content to enjoy
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is a bustling metropolis with a rich history and a vibrant cultural scene. The city is home to many famous landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. Paris is also known for its cuisine, fashion, and art scene, making it a popular tourist destination. The city is home to over 10 million people and is a major economic and cultural hub in Europe. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly into one another. The city is also known for its annual festivals and events,
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased focus on ethical considerations: As AI becomes more integrated into our daily lives, there will be a greater emphasis on ethical considerations. This will include issues such as bias, privacy, and transparency. AI developers will need to be more mindful of how their technology is used and how it affects society.
    
    2. Integration with other technologies: AI is likely to become more integrated with other technologies, such as machine learning, natural language processing, and computer vision. This will allow for more complex and sophisticated
    


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
    Generated text:  [Your Name], and I specialize in [Your Specialty]. I'm a [Your Age] year-old tech enthusiast who loves to experiment with different programming languages and software tools. I'm also a [Your Interests] person who enjoys reading, watching movies, and spending time with my cat, [Your Cat's Name]. I'm a [Your Personality] person who is always looking for new ways to stay entertained and learn new things. I'm a [Your Work Style] person who loves to work independently and take on challenges. I'm passionate about [Your Hobby/Interest] and I'm always looking for new opportunities to learn more
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. It is the largest city in the European Union and the second-largest city in the world by population. Paris is known for its historical and cultural landmarks, including the Louvre Museum and the Eiffel Tower. The city is also home to many famous French artists, such as Pablo Picasso and Vincent Van Gogh. Paris is a vibrant and cosmopolitan city with a rich cultural history that continues to inspire and inspire people around the world. The city is known for its delicious cuisine, including French cuisine and tapas. Paris is also a center of science, technology, and innovation, with many universities, research institutions, and conferences held
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be a rapidly evolving landscape with several key trends that are shaping how we perceive, use, and develop AI technologies.
    
    1. Increase in Accessibility and Personalization: AI will continue to become more accessible to users through advancements in cloud computing, machine learning, and natural language processing. As AI becomes more intuitive, it will be easier for users to interact with AI systems and make decisions based on data from the internet. Additionally, AI will be more personalized, delivering tailored experiences that address individual user preferences and needs.
    
    2. Integration with Other Technologies: AI will continue to integrate with other emerging technologies, such as IoT, blockchain, and quantum


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

    ].

     I

     am

     a

     [

    Occup

    ation

    ]

     from

     [

    Location

    ].

     I

    've

     been

     living

     in

     [

    City

    ]

     for

     [

    Number

     of

     Years

    ]

     and

     I

     love

     [

    Favorite

     Thing

     to

     Do

    ].

     I

     enjoy

     [

    I

     enjoy

     doing

     because

     it

     makes

     me

     happy

    ].

     I

    ’m

     always

     [

    Kind

     of

     Exc

    ited

    /

    Exc

    ited

     About

     Something

     That

     Happ

    ened

    ].

     I

     also

     [

    I

    'm

     a

     Life

     Coach

    ],

     helping

     people

     achieve

     their

     goals

     and

     improve

     their

     lives

    .

     I

    've

     been

     a

     coach

     for

     [

    Number

     of

     Years

    ]

     and

     I

    'm

     very

     [

    Positive

    /A

    ng

    ry

    /N

    erv

    ous

    /etc

    .

    ].

     I

     have

     a

     dream

     to

     [

    My

     Dream

     for

     the

     World

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     known

     for

     its

     iconic

     landmarks

     like

     the

     E

    iff

    el

     Tower

    ,

     Notre

    -D

    ame

     Cathedral

    ,

     and

     the

     Lou

    vre

     Museum

    .

     It

     is

     also

     famous

     for

     its

     vibrant

     and

     diverse

     cultural

     scene

    ,

     as

     well

     as

     its

     historical

     architecture

     and

     museums

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

     rich

     history

     of

     artistic

     and

     intellectual

     pursuits

     that

     has

     influenced

     the

     world

     for

     centuries

    .

     Its

     status

     as

     the

     capital

     of

     France

     has

     made

     it

     one

     of

     the

     most

     important

     and

     dynamic

     cities

     in

     Europe

    .

     Paris

     has

     a

     diverse

     population

    ,

     with

     many

     people

     of

     different

     backgrounds

     and

     national

    ities

     living

     in

     the

     city

    .

     It

     is

     a

     major

     center

     for

     business

    ,

     finance

    ,

     arts

    ,

     education

    ,

     and

     entertainment

    ,

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     several

     key

     trends

    :
    


    1

    .

     Increased

     automation

     and

     precision

    :

     AI

     is

     expected

     to

     continue

     becoming

     more

     precise

     and

     automated

    ,

     with

     more

     tasks

     performed

     by

     machines

     to

     increase

     efficiency

     and

     reduce

     errors

    .

     This

     could

     result

     in

     widespread

     automation

     of

     jobs

     and

     processes

     across

     industries

    .
    


    2

    .

     Integration

     of

     AI

     with

     human

     AI

    :

     AI

     is

     likely

     to

     continue

     to

     be

     integrated

     with

     human

     AI

    ,

     with

     more

     complex

     decision

    -making

     and

     collaboration

     systems

     becoming

     common

    .

     This

     could

     lead

     to

     more

     personalized

     and

     context

    -aware

     AI

     systems

     that

     can

     learn

     and

     adapt

     to

     new

     situations

    .
    


    3

    .

     AI

    -powered

     healthcare

    :

     AI

     is

     expected

     to

     play

     a

     significant

     role

     in

     healthcare

     by

     improving

     diagnostics

    ,

     treatment

     planning

    ,

    



```python
llm.shutdown()
```
