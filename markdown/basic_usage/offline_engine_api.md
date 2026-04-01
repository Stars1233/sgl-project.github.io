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


    2026-04-01 02:43:48.166 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 02:43:48] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 02:43:48.166 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 02:43:48] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 02:43:48.166 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 02:43:48] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 02:43:48.166 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 02:43:48] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 02:43:48.167 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 02:43:48] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.23it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.22it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:28,  1.87it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.73it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 12.09it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 12.09it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 12.09it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 12.09it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 12.09it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 12.09it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 12.09it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 12.09it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.19it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.19it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.19it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.19it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.19it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.19it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.19it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.19it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.88it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.88it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.88it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.88it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.88it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.88it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.88it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.88it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.88it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.88it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.88it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.88it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.88it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.88it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.29it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.29it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.29it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.29it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.29it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.29it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.29it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.63it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.63it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.63it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.63it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.63it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.63it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.63it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.63it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.63it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.84it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.33 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.33 GB):   2%|▏         | 1/58 [00:00<00:10,  5.64it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   2%|▏         | 1/58 [00:00<00:10,  5.64it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=120.30 GB):   2%|▏         | 1/58 [00:00<00:10,  5.64it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.30 GB):   5%|▌         | 3/58 [00:00<00:04, 11.68it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   5%|▌         | 3/58 [00:00<00:04, 11.68it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   5%|▌         | 3/58 [00:00<00:04, 11.68it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.29 GB):   5%|▌         | 3/58 [00:00<00:04, 11.68it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=120.29 GB):  10%|█         | 6/58 [00:00<00:03, 14.45it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.29 GB):  10%|█         | 6/58 [00:00<00:03, 14.45it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):  10%|█         | 6/58 [00:00<00:03, 14.45it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  10%|█         | 6/58 [00:00<00:03, 14.45it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.34it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.34it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.34it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.34it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.27 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.34it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 24.30it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 24.30it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 24.30it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 24.30it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.26 GB):  22%|██▏       | 13/58 [00:00<00:01, 24.30it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  22%|██▏       | 13/58 [00:00<00:01, 24.30it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  31%|███       | 18/58 [00:00<00:01, 30.23it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.28 GB):  31%|███       | 18/58 [00:00<00:01, 30.23it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.29 GB):  31%|███       | 18/58 [00:00<00:01, 30.23it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.27 GB):  31%|███       | 18/58 [00:00<00:01, 30.23it/s]Capturing num tokens (num_tokens=960 avail_mem=119.06 GB):  31%|███       | 18/58 [00:00<00:01, 30.23it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=118.96 GB):  31%|███       | 18/58 [00:00<00:01, 30.23it/s]Capturing num tokens (num_tokens=896 avail_mem=118.96 GB):  40%|███▉      | 23/58 [00:00<00:01, 34.06it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  40%|███▉      | 23/58 [00:00<00:01, 34.06it/s]Capturing num tokens (num_tokens=768 avail_mem=118.96 GB):  40%|███▉      | 23/58 [00:00<00:01, 34.06it/s]Capturing num tokens (num_tokens=704 avail_mem=118.95 GB):  40%|███▉      | 23/58 [00:00<00:01, 34.06it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  40%|███▉      | 23/58 [00:00<00:01, 34.06it/s]Capturing num tokens (num_tokens=576 avail_mem=118.95 GB):  40%|███▉      | 23/58 [00:01<00:01, 34.06it/s]Capturing num tokens (num_tokens=576 avail_mem=118.95 GB):  48%|████▊     | 28/58 [00:01<00:00, 36.85it/s]Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  48%|████▊     | 28/58 [00:01<00:00, 36.85it/s]Capturing num tokens (num_tokens=480 avail_mem=118.95 GB):  48%|████▊     | 28/58 [00:01<00:00, 36.85it/s]Capturing num tokens (num_tokens=448 avail_mem=118.95 GB):  48%|████▊     | 28/58 [00:01<00:00, 36.85it/s]

    Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  48%|████▊     | 28/58 [00:01<00:00, 36.85it/s]Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.31it/s]Capturing num tokens (num_tokens=384 avail_mem=118.94 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.31it/s]Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.31it/s]Capturing num tokens (num_tokens=320 avail_mem=118.94 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.31it/s]Capturing num tokens (num_tokens=288 avail_mem=118.93 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.31it/s]Capturing num tokens (num_tokens=288 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 37.17it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 37.17it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  62%|██████▏   | 36/58 [00:01<00:00, 37.17it/s]Capturing num tokens (num_tokens=224 avail_mem=118.92 GB):  62%|██████▏   | 36/58 [00:01<00:00, 37.17it/s]

    Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  62%|██████▏   | 36/58 [00:01<00:00, 37.17it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  69%|██████▉   | 40/58 [00:01<00:00, 37.40it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  69%|██████▉   | 40/58 [00:01<00:00, 37.40it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  69%|██████▉   | 40/58 [00:01<00:00, 37.40it/s]Capturing num tokens (num_tokens=160 avail_mem=118.91 GB):  69%|██████▉   | 40/58 [00:01<00:00, 37.40it/s]Capturing num tokens (num_tokens=144 avail_mem=118.61 GB):  69%|██████▉   | 40/58 [00:01<00:00, 37.40it/s]Capturing num tokens (num_tokens=144 avail_mem=118.61 GB):  76%|███████▌  | 44/58 [00:01<00:00, 34.60it/s]Capturing num tokens (num_tokens=128 avail_mem=118.61 GB):  76%|███████▌  | 44/58 [00:01<00:00, 34.60it/s]

    Capturing num tokens (num_tokens=112 avail_mem=118.61 GB):  76%|███████▌  | 44/58 [00:01<00:00, 34.60it/s]Capturing num tokens (num_tokens=96 avail_mem=118.60 GB):  76%|███████▌  | 44/58 [00:01<00:00, 34.60it/s] Capturing num tokens (num_tokens=80 avail_mem=118.60 GB):  76%|███████▌  | 44/58 [00:01<00:00, 34.60it/s]Capturing num tokens (num_tokens=80 avail_mem=118.60 GB):  83%|████████▎ | 48/58 [00:01<00:00, 28.01it/s]Capturing num tokens (num_tokens=64 avail_mem=118.60 GB):  83%|████████▎ | 48/58 [00:01<00:00, 28.01it/s]Capturing num tokens (num_tokens=48 avail_mem=118.60 GB):  83%|████████▎ | 48/58 [00:01<00:00, 28.01it/s]

    Capturing num tokens (num_tokens=32 avail_mem=118.59 GB):  83%|████████▎ | 48/58 [00:01<00:00, 28.01it/s]Capturing num tokens (num_tokens=28 avail_mem=118.59 GB):  83%|████████▎ | 48/58 [00:01<00:00, 28.01it/s]Capturing num tokens (num_tokens=28 avail_mem=118.59 GB):  90%|████████▉ | 52/58 [00:01<00:00, 28.45it/s]Capturing num tokens (num_tokens=24 avail_mem=118.58 GB):  90%|████████▉ | 52/58 [00:01<00:00, 28.45it/s]Capturing num tokens (num_tokens=20 avail_mem=118.58 GB):  90%|████████▉ | 52/58 [00:01<00:00, 28.45it/s]Capturing num tokens (num_tokens=16 avail_mem=118.58 GB):  90%|████████▉ | 52/58 [00:01<00:00, 28.45it/s]Capturing num tokens (num_tokens=12 avail_mem=118.57 GB):  90%|████████▉ | 52/58 [00:01<00:00, 28.45it/s]Capturing num tokens (num_tokens=12 avail_mem=118.57 GB):  97%|█████████▋| 56/58 [00:01<00:00, 30.15it/s]Capturing num tokens (num_tokens=8 avail_mem=118.57 GB):  97%|█████████▋| 56/58 [00:01<00:00, 30.15it/s] Capturing num tokens (num_tokens=4 avail_mem=118.57 GB):  97%|█████████▋| 56/58 [00:01<00:00, 30.15it/s]

    Capturing num tokens (num_tokens=4 avail_mem=118.57 GB): 100%|██████████| 58/58 [00:01<00:00, 29.10it/s]


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
    Generated text:  Meena, I am a 33 year old woman. My diagnosis was left sided atrial fibrillation. I was given an implantable cardioverter defibrillator (ICD) through my car door. The procedure was a minor 60 minute procedure. I went to the hospital the day after the procedure. I was discharged on Monday. I had a heart attack on the 13th and was transferred to the Emergency room. They gave me a heart rate of 131. My doctor says I will have a pacemaker placed in about 6 months.
    
    What can I do to get my heart rate back
    ===============================
    Prompt: The president of the United States is
    Generated text:  from which of the following countries?
    A) Japan
    B) China
    C) United Kingdom
    D) France
    
    To determine which country the president of the United States is from, we need to identify the president of the United States. The current president of the United States is President Joe Biden. Let's go through the options step by step:
    
    A) Japan - The president of Japan is the Prime Minister.
    B) China - The president of China is the President of the People's Republic of China.
    C) United Kingdom - The president of the United Kingdom is the Prime Minister.
    D) France - The president of France is the
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. The street from the Bourse (stock exchange) to the Sorbonne (Paris University) is called the Champs-Élysées. Paris is a city that is in the center of France and is famous for its architecture and culture.
    
    The staff at an airport is all located in the terminal of the flight gates. The terminal is part of the city’s infrastructure, but it is not part of the city. The airport is a place where passengers use planes to travel from one place to another.
    
    What do you think of the airport? Imagine you are a traveler who has just arrived at the airport. The airport staff are
    ===============================
    Prompt: The future of AI is
    Generated text:  bleak. Here’s how to keep it from becoming a dark future.
    The path to artificial intelligence (AI) is not smooth. For many years, AI has been plagued by a lack of transparency and accountability. For example, the AI model used to generate results in a 2012 US federal data set was used to create a list of 2,000 “underachieving” schools, which were later used to target funding for schools. The AI model used to generate results in a 2017 US federal data set was used to target funding for schools. The AI model used to generate results in a


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


    Generated text:  [Name] and I'm a [Age] year old [Gender] [Occupation]. I'm currently [Current Location] and I'm here to [Purpose of Visit]. I'm here to [Briefly Describe Your Purpose]. I'm excited to meet you and learn more about you. What's your name? What's your occupation? What's your purpose? What's your current location? What's your age? What's your gender? What's your occupation? What's your purpose? What's your current location? What's your age? What's your gender? What's your occupation? What's your purpose? What's
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament and the French National Museum. Paris is a cultural and historical center with a rich history dating back to the Roman Empire and the Middle Ages. It is a major transportation hub and a major tourist destination. The city is known for its cuisine, fashion, and art scene. Paris is a vibrant and dynamic city that continues to evolve and grow. It is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly. The city is also known for its
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some of the most likely future trends in AI:
    
    1. Increased automation and robotics: As AI technology continues to advance, we can expect to see more automation and robotics in various industries. This will likely lead to increased efficiency, cost savings, and job displacement, but it will also create new opportunities for innovation and creativity.
    
    2. Enhanced personalization: AI will continue to improve as it learns more about human behavior and preferences. This will lead to more personalized experiences, such as recommendations for products and services
    


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
    Generated text:  [insert your name here], and I am a [insert your profession here], [insert your field of expertise here].
    I'm passionate about [insert something you enjoy doing, like reading, playing music, or cooking] and I'm always looking for new challenges and opportunities to learn more about myself and the world. I'm a friendly, approachable person who loves to connect with people and share my knowledge and experiences with others. I enjoy helping people and making a difference in the world through my work.
    I'm always looking for new challenges and opportunities to learn more about myself and the world. I'm a friendly, approachable person who
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, a historic city in the south of the country known for its elegant architecture and rich cultural heritage. It is the largest city in France and the second-largest city in the world, with a population of approximately 1.2 million people. Paris is a bustling metropolis with a rich history dating back to the Roman Empire and featuring a vibrant culture, art, and food scene. It is a popular tourist destination, attracting millions of visitors each year for its beautiful architecture, cafes, and nightlife. Paris is also known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. The city
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be shaped by a number of trends and changes in the technology sector, including:
    
    1. Increasing focus on ethical considerations: As AI becomes more advanced and involved in human lives, there will be increased focus on ethical considerations, privacy, and transparency. This will likely lead to stricter regulations and increased scrutiny of AI development and deployment.
    
    2. Integration of more advanced technologies: AI is likely to become more integrated with other technologies, such as quantum computing, biotechnology, and materials science. This will create a new set of challenges and opportunities for researchers and developers.
    
    3. Expansion of the AI community: As AI technology continues to advance,


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

     __

    ________

    .

     I

    'm

     a

    /an

     __

    ________

    _

     (

    person

    ality

     trait

    ).

     I

     enjoy

     __

    ________

     (

    activity

    ),

     and

     I

    'm

     a

    /an

     __

    ________

    _

     (

    person

    ality

     trait

    ).

     What

     are

     your

     hobbies

     and

     interests

    ?


    Please

     do

     not

     include

     any

     names

     or

     personal

     details

    .

     Start

     with

     your

     name

     and

     personality

     trait

    .


    Hello

    ,

     my

     name

     is

     [

    insert

     name

    ].

     I

    'm

     a

     [

    insert

     personality

     trait

    ]

     [

    insert

     personality

     trait

    ].

     I

     enjoy

     [

    insert

     activity

    ],

     and

     I

    'm

     a

     [

    insert

     personality

     trait

    ]

     [

    insert

     personality

     trait

    ].

     What

     are

     your

     hobbies

     and

     interests

    ?

     [

    insert

     hobbies

     and

     interests

    ]

     [

    insert

     hobbies

     and

     interests

    ]

     [

    insert

     hobbies

     and

     interests

    ]


    (Note

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     which

     is

     known

     for

     its

     diverse

     architecture

    ,

     rich

     history

    ,

     and

     annual

     celebrations

     such

     as

     the

     E

    iff

    el

     Tower

     par

    ades

     and

     the

     Lou

    vre

     Museum

     celebrations

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

     where

     President

     Nicolas

     Sark

    ozy

     celebrated

     the

     

    1

    0

    0

    th

     anniversary

     of

     the

     E

    iff

    el

     Tower

     tower

     in

     

    2

    0

    1

    2

    .

     
    


    Additionally

    ,

     Paris

     hosts

     the

     World

     Chess

     Championship

     in

     the

     E

    iff

    el

     Tower

     each

     year

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

     including

     its

     famous

     cro

    iss

    ants

    ,

     its

     wine

    ,

     and

     its

     chocolate

    ,

     which

     are

     popular

     throughout

     the

     world

    .

     Paris

     is

     also

     known

     for

     its

     art

     and

     culture

    ,

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     uncertain

    ,

     but

     some

     possible

     trends

     that

     are

     likely

     to

     be

     explored

     include

    :
    


    1

    .

     Improved

     personal

    ization

     and

     ad

     personal

    ization

    :

     As

     AI

     algorithms

     learn

     more

     about

     individual

     users

    ,

     they

     will

     be

     able

     to

     create

     more

     accurate

     and

     personalized

     recommendations

     and

     advertisements

    .

     This

     will

     lead

     to

     increased

     engagement

    ,

     satisfaction

    ,

     and

     loyalty

     for

     businesses

    .
    


    2

    .

     Autonomous

     vehicles

    :

     AI

     will

     continue

     to

     advance

    ,

     leading

     to

     more

     efficient

    ,

     safer

    ,

     and

     cost

    -effective

     transportation

    .

     Autonomous

     vehicles

     will

     be

     able

     to

     navigate

     roads

     safely

    ,

     reduce

     accidents

    ,

     and

     increase

     traffic

     flow

    .
    


    3

    .

     Health

    care

     advancements

    :

     AI

     will

     play

     a

     significant

     role

     in

     improving

     medical

     care

    ,

     such

     as

     through

     precision

     medicine

     and

     personalized

     treatments

    .

    



```python
llm.shutdown()
```
