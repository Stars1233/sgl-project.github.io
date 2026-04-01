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


    2026-04-01 04:21:41.558 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 04:21:41] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 04:21:41.558 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 04:21:41] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 04:21:41.558 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 04:21:41] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 04:21:41.558 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 04:21:41] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 04:21:41.558 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 04:21:41] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.30it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.29it/s]


    2026-04-01 04:21:44,625 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 04:21:44] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:33,  2.70s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:33,  2.70s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:33,  2.70s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:33,  2.70s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:08,  5.69it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 12.05it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]

    Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.12it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.12it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.12it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.12it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.12it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.12it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.12it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.12it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.78it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.78it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.78it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.78it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.78it/s]

    Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.78it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.78it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.67it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.67it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.67it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.67it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.67it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.67it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.67it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.04it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.04it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.04it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.04it/s] 

    Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.04it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.04it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.04it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.30it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.30it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.30it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.30it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.30it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.30it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.30it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.30it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.30it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.72it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.34 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:04, 13.32it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:04, 13.32it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:04, 13.32it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   7%|▋         | 4/58 [00:00<00:03, 16.11it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   7%|▋         | 4/58 [00:00<00:03, 16.11it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.29 GB):   7%|▋         | 4/58 [00:00<00:03, 16.11it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.29 GB):  10%|█         | 6/58 [00:00<00:03, 16.91it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.29 GB):  10%|█         | 6/58 [00:00<00:03, 16.91it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):  10%|█         | 6/58 [00:00<00:03, 16.91it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  10%|█         | 6/58 [00:00<00:03, 16.91it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  10%|█         | 6/58 [00:00<00:03, 16.91it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  17%|█▋        | 10/58 [00:00<00:02, 23.57it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  17%|█▋        | 10/58 [00:00<00:02, 23.57it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  17%|█▋        | 10/58 [00:00<00:02, 23.57it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.27 GB):  17%|█▋        | 10/58 [00:00<00:02, 23.57it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.27 GB):  17%|█▋        | 10/58 [00:00<00:02, 23.57it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  17%|█▋        | 10/58 [00:00<00:02, 23.57it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  26%|██▌       | 15/58 [00:00<00:01, 30.40it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  26%|██▌       | 15/58 [00:00<00:01, 30.40it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.26 GB):  26%|██▌       | 15/58 [00:00<00:01, 30.40it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  26%|██▌       | 15/58 [00:00<00:01, 30.40it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  26%|██▌       | 15/58 [00:00<00:01, 30.40it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  26%|██▌       | 15/58 [00:00<00:01, 30.40it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  34%|███▍      | 20/58 [00:00<00:01, 35.02it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  34%|███▍      | 20/58 [00:00<00:01, 35.02it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  34%|███▍      | 20/58 [00:00<00:01, 35.02it/s] Capturing num tokens (num_tokens=896 avail_mem=120.24 GB):  34%|███▍      | 20/58 [00:00<00:01, 35.02it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  34%|███▍      | 20/58 [00:00<00:01, 35.02it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  34%|███▍      | 20/58 [00:00<00:01, 35.02it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  43%|████▎     | 25/58 [00:00<00:00, 38.19it/s]Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:00<00:00, 38.19it/s]

    Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:00<00:00, 38.19it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:00<00:00, 38.19it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  43%|████▎     | 25/58 [00:00<00:00, 38.19it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:00<00:00, 38.19it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:00<00:00, 40.27it/s]Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:00<00:00, 40.27it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:00<00:00, 40.27it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:00<00:00, 40.27it/s]Capturing num tokens (num_tokens=352 avail_mem=120.22 GB):  52%|█████▏    | 30/58 [00:01<00:00, 40.27it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  52%|█████▏    | 30/58 [00:01<00:00, 40.27it/s]

    Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  60%|██████    | 35/58 [00:01<00:00, 41.64it/s]Capturing num tokens (num_tokens=288 avail_mem=120.21 GB):  60%|██████    | 35/58 [00:01<00:00, 41.64it/s]Capturing num tokens (num_tokens=256 avail_mem=120.21 GB):  60%|██████    | 35/58 [00:01<00:00, 41.64it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  60%|██████    | 35/58 [00:01<00:00, 41.64it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  60%|██████    | 35/58 [00:01<00:00, 41.64it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  60%|██████    | 35/58 [00:01<00:00, 41.64it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 42.89it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 42.89it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 42.89it/s]Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 42.89it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 42.89it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 42.89it/s]

    Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  78%|███████▊  | 45/58 [00:01<00:00, 43.58it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  78%|███████▊  | 45/58 [00:01<00:00, 43.58it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 43.58it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 43.58it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 43.58it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  78%|███████▊  | 45/58 [00:01<00:00, 43.58it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.84it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.84it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.84it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.84it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.84it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.84it/s]

    Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  95%|█████████▍| 55/58 [00:01<00:00, 44.52it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  95%|█████████▍| 55/58 [00:01<00:00, 44.52it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  95%|█████████▍| 55/58 [00:01<00:00, 44.52it/s] Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  95%|█████████▍| 55/58 [00:01<00:00, 44.52it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 37.28it/s]


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
    Generated text:  Patricia and I am 24 years old. I'm from the Philippines and I live in the Philippines.
    I am starting a new job this month in Europe. I'm currently unemployed, but I am actively looking for a job. I have read the Global Jobs website and have gotten a job listing for a data scientist position in Europe. I am applying for this position and the company says that they are looking for a candidate with experience with Python. 
    Can you help me find out if I am qualified for the job and if so, what kind of experience do I need?
    I am very excited about the opportunity to work with a company
    ===============================
    Prompt: The president of the United States is
    Generated text:  a person. President Obama, for example, was elected to this position by the American people. If the United States were to separate from the Union, we would be a country with its own government, its own laws, and its own military. The president of that country would be a different person from the president of the United States. That is the main point of my message today. The United States is the world's greatest country. We have the most powerful government in the world. We have the most people living in the world. Our economy is the most valuable in the world. We produce the most food, the most oil, and
    ===============================
    Prompt: The capital of France is
    Generated text:  the city of Paris. It is also one of the largest cities in Europe. In Paris, you can find many of the famous landmarks. You can find the Eiffel Tower, the Louvre Museum, and other important places. In Paris, you can see many different kinds of buildings. There are tall buildings such as the Eiffel Tower and the Eiffelix. There are also very old buildings such as the Louvre Museum. The Louvre Museum is very important and it was built in 1150. In Paris, you can enjoy the food and drink in restaurants and cafes. Paris is famous for its food
    ===============================
    Prompt: The future of AI is
    Generated text:  multi-dimensional and it’s not just a matter of choosing the right tool for the job, but also of choosing the right people to use it. In this episode, we talk to Aiden, one of the most well-known developers of cognitive AI for the automotive industry. Aiden has been working on the neural network, which enables a car to drive itself in both autonomous and semi-autonomous modes. He explains how he uses a neural network to help the car understand the world around it and respond accordingly. We also take a look at the future of autonomous driving, including who is responsible for the development of these new technologies, who will be


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


    Generated text:  [Name] and I'm a [Age] year old [Occupation]. I'm a [Skill or Hobby] enthusiast and I love [Reason for Hobby]. I'm always looking for new experiences and adventures, and I'm always eager to learn and grow. I'm a [Favorite Activity] lover and I love [Reason for Love]. I'm always looking for new challenges and opportunities to grow and improve myself. I'm a [Favorite Book or Movie] fan and I love [Reason for Love]. I'm always looking for new ways to express myself and share my experiences with others. I'm a [Favorite Music] lover and
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major cultural and economic center, hosting numerous museums, theaters, and other attractions. Paris is a popular tourist destination and a major hub for international business and diplomacy. The city is known for its rich history, including the influence of the French Revolution and the influence of the French Revolution on modern French society. Paris is also home to many famous French artists, writers, and musicians. The city is a major hub for international trade and diplomacy, and is known for its vibrant nightlife and cultural
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that could be expected in the future:
    
    1. Increased focus on ethical considerations: As AI becomes more integrated into our daily lives, there will be a growing emphasis on ethical considerations. This could include issues such as bias, transparency, accountability, and privacy. AI developers will need to be more mindful of the potential consequences of their creations and work to ensure that they are used in a way that is fair and responsible.
    
    2. Greater integration with human intelligence: AI is likely to become more
    


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
    Generated text:  [Name], and I'm a [Type] [Occupation].
    Hello, my name is [Name], and I'm a [Type] [Occupation]. As a [Type] [Occupation], my primary focus is [Primary focus], and I'm known for my [Primary focus] abilities. As a [Type] [Occupation], I'm [Primary focus] and I'm known for my [Primary focus] abilities.
    I believe in [Positive attribute], and I strive to [Primary focus] in [Positive attribute] for [Primary focus]. As a [Type] [Occupation], I'm [Primary focus
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. It is the most populous city in Europe and is known for its rich history, art, and architecture. Paris is located in the center of France and is the administrative and cultural center of the country. It has a famous Eiffel Tower, the Louvre Museum, the Notre-Dame Cathedral, and many other landmarks that make it a popular tourist destination. The city is also home to many famous artists and musicians. Paris is a popular tourist destination for people from all over the world and is known for its fashion, food, and art scene. With its unique blend of history, culture, and modernity, Paris is a
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to involve a number of different trends and developments. Here are some possible future trends in AI:
    
    1. AI will become more pervasive and integrated into our everyday lives. This could be through the use of AI in everything from smart home devices to self-driving cars. AI will likely become more integrated into our daily routines, making it easier for us to interact with technology and solve problems around us.
    
    2. AI will become more powerful and capable. In recent years, there have been several breakthroughs in AI, such as the development of deep learning algorithms that can perform tasks that were previously thought to be intractable. As AI technology continues


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

    Your

     Name

    ].

     I

     am

     an

     [

    age

    ],

     [

    gender

    ],

     [

    occupation

    ],

     [

    special

    ity

    ],

     and

     I

     am

     currently

     [

    location

    ]

     [

    reason

     for

     being

     there

    ].

     I

     bring

     to

     this

     world

     a

     sense

     of

     [

    positive

     or

     negative

     traits

    ],

     and

     I

     am

     constantly

     learning

     and

     growing

    .

     My

     values

     are

     [

    list

     of

     values

    ],

     and

     I

     strive

     to

     be

     a

     [

    good

     or

     bad

    ]

     person

    .

     I

     am

     an

     [

    attribute

     or

     trait

    ]

     person

    ,

     and

     I

     am

     always

     [

    good

     or

     bad

    ]

     about

     [

    reason

     for

     being

    ].

     Thank

     you

     for

     having

     me

    .

     
    


    (Note

    :

     In

     the

     example

     above

    ,

     you

     can

     replace

     "

    Your

     Name

    "

     with

     your

     real

     name

    ,

     "

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     which

     is

     the

     most

     populous

     city

     in

     the

     country

     and

     is

     known

     for

     its

     historic

     architecture

    ,

     rich

     cultural

     heritage

    ,

     and

     vibrant

     nightlife

    .

     It

     is

     a

     UNESCO

     World

     Heritage

     site

     and

     is

     home

     to

     numerous

     museums

    ,

     including

     the

     Lou

    vre

    ,

     the

     National

     Museum

     of

     African

     History

    ,

     and

     the

     Mus

    ée

     de

     l

    '

    Or

    anger

    ie

    .

     Paris

     is

     also

     known

     for

     its

     fashion

     industry

    ,

     with

     many

     famous

     designers

     and

     fashion

     houses

     headquartered

     there

    .

     Its

     economy

     is

     primarily

     based

     on

     tourism

    ,

     with

     millions

     of

     visitors

     coming

     to

     the

     city

     each

     year

     to

     experience

     its

     vibrant

     culture

     and

     history

    .

     Paris

     is

     a

     popular

     destination

     for

     French

     people

    ,

     known

     for

     its

     rich

     culture

    ,

     delicious

     food

    ,

     and

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     rapidly

     evolving

    ,

     and

     there

     are

     several

     trends

     that

     are

     likely

     to

     shape

     the

     field

     in

     the

     coming

     years

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

     automation

     and

     AI

     integration

    :

     As

     AI

     becomes

     more

     sophisticated

    ,

     more

     and

     more

     jobs

     will

     be

     automated

    ,

     and

     AI

     will

     be

     integrated

     into

     many

     industries

    .

     This

     will

     create

     new

     opportunities

     for

     workers

     who

     can

     leverage

     AI

     to

     increase

     productivity

     and

     efficiency

    .
    


    2

    .

     More

     ethical

     and

     responsible

     AI

    :

     There

     is

     growing

     concern

     about

     the

     ethical

     implications

     of

     AI

    ,

     such

     as

     bias

    ,

     privacy

    ,

     and

     the

     potential

     for

     unintended

     consequences

    .

     As

     AI

     becomes

     more

     advanced

    ,

     it

     will

     be

     essential

     to

     ensure

     that

     it

     is

     developed

     and

     used

     in

     ways

     that

     are

    



```python
llm.shutdown()
```
