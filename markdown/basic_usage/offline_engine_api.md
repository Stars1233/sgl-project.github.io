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


    2026-03-29 04:20:02.802 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 04:20:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 04:20:02.802 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 04:20:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 04:20:02.803 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 04:20:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 04:20:02.803 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 04:20:02] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 04:20:02.803 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 04:20:02] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.61it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.59it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:12,  2.32s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:12,  2.32s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:12,  2.32s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:12,  2.32s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.15it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.15it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.15it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.15it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:25,  2.15it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:25,  2.15it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:25,  2.15it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:02, 13.65it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:02, 13.65it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:02, 13.65it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:02, 13.65it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:02, 13.65it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:02, 13.65it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:02, 13.65it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:02, 13.65it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 20.28it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 20.28it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 20.28it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 20.28it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 20.28it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 20.28it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 20.28it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:02<00:01, 20.28it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 27.37it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 27.37it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 27.37it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:00, 27.37it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:00, 27.37it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:02<00:00, 27.37it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:02<00:00, 27.37it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:02<00:00, 32.34it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:02<00:00, 32.34it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:02<00:00, 32.34it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:02<00:00, 32.34it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 32.34it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 32.34it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 32.34it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 37.23it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 37.23it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 37.23it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 37.23it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 37.23it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 37.23it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 37.23it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 39.35it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 39.35it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 39.35it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 39.35it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 39.35it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 39.35it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 39.35it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 39.35it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 39.35it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 47.51it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.53it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=117.08 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=117.08 GB):   2%|▏         | 1/58 [00:00<00:05,  9.53it/s]Capturing num tokens (num_tokens=7680 avail_mem=117.04 GB):   2%|▏         | 1/58 [00:00<00:05,  9.53it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=117.04 GB):   3%|▎         | 2/58 [00:00<00:06,  9.06it/s]Capturing num tokens (num_tokens=7168 avail_mem=117.03 GB):   3%|▎         | 2/58 [00:00<00:06,  9.06it/s]Capturing num tokens (num_tokens=6656 avail_mem=116.54 GB):   3%|▎         | 2/58 [00:00<00:06,  9.06it/s]Capturing num tokens (num_tokens=6656 avail_mem=116.54 GB):   7%|▋         | 4/58 [00:00<00:04, 12.53it/s]Capturing num tokens (num_tokens=6144 avail_mem=116.45 GB):   7%|▋         | 4/58 [00:00<00:04, 12.53it/s]Capturing num tokens (num_tokens=5632 avail_mem=116.44 GB):   7%|▋         | 4/58 [00:00<00:04, 12.53it/s]Capturing num tokens (num_tokens=5120 avail_mem=116.44 GB):   7%|▋         | 4/58 [00:00<00:04, 12.53it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=116.44 GB):  12%|█▏        | 7/58 [00:00<00:02, 18.16it/s]Capturing num tokens (num_tokens=4608 avail_mem=116.44 GB):  12%|█▏        | 7/58 [00:00<00:02, 18.16it/s]Capturing num tokens (num_tokens=4096 avail_mem=116.44 GB):  12%|█▏        | 7/58 [00:00<00:02, 18.16it/s]Capturing num tokens (num_tokens=3840 avail_mem=116.44 GB):  12%|█▏        | 7/58 [00:00<00:02, 18.16it/s]Capturing num tokens (num_tokens=3584 avail_mem=116.43 GB):  12%|█▏        | 7/58 [00:00<00:02, 18.16it/s]Capturing num tokens (num_tokens=3584 avail_mem=116.43 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.58it/s]Capturing num tokens (num_tokens=3328 avail_mem=116.43 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.58it/s]Capturing num tokens (num_tokens=3072 avail_mem=116.42 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.58it/s]Capturing num tokens (num_tokens=2816 avail_mem=116.42 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.58it/s]Capturing num tokens (num_tokens=2560 avail_mem=116.42 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.58it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=116.41 GB):  19%|█▉        | 11/58 [00:00<00:01, 24.58it/s]Capturing num tokens (num_tokens=2304 avail_mem=116.41 GB):  28%|██▊       | 16/58 [00:00<00:01, 31.17it/s]Capturing num tokens (num_tokens=2048 avail_mem=116.41 GB):  28%|██▊       | 16/58 [00:00<00:01, 31.17it/s]Capturing num tokens (num_tokens=1792 avail_mem=116.41 GB):  28%|██▊       | 16/58 [00:00<00:01, 31.17it/s]Capturing num tokens (num_tokens=1536 avail_mem=116.40 GB):  28%|██▊       | 16/58 [00:00<00:01, 31.17it/s]Capturing num tokens (num_tokens=1280 avail_mem=116.40 GB):  28%|██▊       | 16/58 [00:00<00:01, 31.17it/s]Capturing num tokens (num_tokens=1024 avail_mem=116.38 GB):  28%|██▊       | 16/58 [00:00<00:01, 31.17it/s]Capturing num tokens (num_tokens=1024 avail_mem=116.38 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.77it/s]Capturing num tokens (num_tokens=960 avail_mem=116.39 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.77it/s] Capturing num tokens (num_tokens=896 avail_mem=116.39 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.77it/s]Capturing num tokens (num_tokens=832 avail_mem=116.39 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.77it/s]Capturing num tokens (num_tokens=768 avail_mem=116.38 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.77it/s]

    Capturing num tokens (num_tokens=704 avail_mem=116.38 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.77it/s]Capturing num tokens (num_tokens=704 avail_mem=116.38 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.91it/s]Capturing num tokens (num_tokens=640 avail_mem=116.38 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.91it/s]Capturing num tokens (num_tokens=576 avail_mem=116.37 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.91it/s]Capturing num tokens (num_tokens=512 avail_mem=116.36 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.91it/s]Capturing num tokens (num_tokens=480 avail_mem=116.37 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.91it/s]Capturing num tokens (num_tokens=448 avail_mem=116.35 GB):  45%|████▍     | 26/58 [00:00<00:00, 38.91it/s]Capturing num tokens (num_tokens=448 avail_mem=116.35 GB):  53%|█████▎    | 31/58 [00:01<00:00, 40.85it/s]Capturing num tokens (num_tokens=416 avail_mem=116.31 GB):  53%|█████▎    | 31/58 [00:01<00:00, 40.85it/s]Capturing num tokens (num_tokens=384 avail_mem=116.31 GB):  53%|█████▎    | 31/58 [00:01<00:00, 40.85it/s]Capturing num tokens (num_tokens=352 avail_mem=116.31 GB):  53%|█████▎    | 31/58 [00:01<00:00, 40.85it/s]Capturing num tokens (num_tokens=320 avail_mem=116.30 GB):  53%|█████▎    | 31/58 [00:01<00:00, 40.85it/s]

    Capturing num tokens (num_tokens=288 avail_mem=116.30 GB):  53%|█████▎    | 31/58 [00:01<00:00, 40.85it/s]Capturing num tokens (num_tokens=288 avail_mem=116.30 GB):  62%|██████▏   | 36/58 [00:01<00:00, 42.30it/s]Capturing num tokens (num_tokens=256 avail_mem=116.30 GB):  62%|██████▏   | 36/58 [00:01<00:00, 42.30it/s]Capturing num tokens (num_tokens=240 avail_mem=116.29 GB):  62%|██████▏   | 36/58 [00:01<00:00, 42.30it/s]Capturing num tokens (num_tokens=224 avail_mem=116.29 GB):  62%|██████▏   | 36/58 [00:01<00:00, 42.30it/s]Capturing num tokens (num_tokens=208 avail_mem=116.29 GB):  62%|██████▏   | 36/58 [00:01<00:00, 42.30it/s]Capturing num tokens (num_tokens=192 avail_mem=116.29 GB):  62%|██████▏   | 36/58 [00:01<00:00, 42.30it/s]Capturing num tokens (num_tokens=192 avail_mem=116.29 GB):  71%|███████   | 41/58 [00:01<00:00, 43.46it/s]Capturing num tokens (num_tokens=176 avail_mem=116.28 GB):  71%|███████   | 41/58 [00:01<00:00, 43.46it/s]Capturing num tokens (num_tokens=160 avail_mem=116.28 GB):  71%|███████   | 41/58 [00:01<00:00, 43.46it/s]Capturing num tokens (num_tokens=144 avail_mem=116.28 GB):  71%|███████   | 41/58 [00:01<00:00, 43.46it/s]Capturing num tokens (num_tokens=128 avail_mem=116.27 GB):  71%|███████   | 41/58 [00:01<00:00, 43.46it/s]

    Capturing num tokens (num_tokens=112 avail_mem=116.27 GB):  71%|███████   | 41/58 [00:01<00:00, 43.46it/s]Capturing num tokens (num_tokens=112 avail_mem=116.27 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.88it/s]Capturing num tokens (num_tokens=96 avail_mem=116.27 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.88it/s] Capturing num tokens (num_tokens=80 avail_mem=116.26 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.88it/s]Capturing num tokens (num_tokens=64 avail_mem=116.26 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.88it/s]Capturing num tokens (num_tokens=48 avail_mem=116.26 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.88it/s]Capturing num tokens (num_tokens=32 avail_mem=116.25 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.88it/s]Capturing num tokens (num_tokens=32 avail_mem=116.25 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.95it/s]Capturing num tokens (num_tokens=28 avail_mem=116.25 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.95it/s]Capturing num tokens (num_tokens=24 avail_mem=116.25 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.95it/s]Capturing num tokens (num_tokens=20 avail_mem=116.24 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.95it/s]

    Capturing num tokens (num_tokens=16 avail_mem=116.24 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.95it/s]Capturing num tokens (num_tokens=12 avail_mem=116.24 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.95it/s]Capturing num tokens (num_tokens=12 avail_mem=116.24 GB):  97%|█████████▋| 56/58 [00:01<00:00, 44.55it/s]Capturing num tokens (num_tokens=8 avail_mem=116.23 GB):  97%|█████████▋| 56/58 [00:01<00:00, 44.55it/s] Capturing num tokens (num_tokens=4 avail_mem=116.23 GB):  97%|█████████▋| 56/58 [00:01<00:00, 44.55it/s]Capturing num tokens (num_tokens=4 avail_mem=116.23 GB): 100%|██████████| 58/58 [00:01<00:00, 36.18it/s]


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
    Generated text:  Pasha and I am 19 years old. I was once a student and I went to college but I am now away from the campus. I just went out for the summer and I have no job and I am now at home without a job either.
    
    My question is this: What should I do to start a new job? I have no idea what to do or where to look.
    
    I am looking for something that will make me interested in the job so I am not looking for a job, but I just want to get this started. Any advice would be great!
    
    ---
    
    **UPDATE:**
    
    I have a job offer from
    ===============================
    Prompt: The president of the United States is
    Generated text:  a _____ of the country.
    A. President
    B. President-in-Office
    C. President-elect
    D. President in transition
    
    To determine the correct answer, let's analyze each option step by step:
    
    A. President: This is a specific title for the president of the United States. It refers to the head of state who is elected by the country's citizens.
    
    B. President-in-Office: This term is often used in campaign rhetoric but does not necessarily refer to the president in the formal sense. It is a shortened form of "President-elect," which is the person who has been elected but hasn't yet been inaugur
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, located in the southeast of the country and is situated on the River Seine in the French Department of the Pas-de-Calais. Paris is the capital city of the French Republic. It is the most populous city in Europe, and the 2nd most populous city in the world after Beijing. It is the seat of the French government, the head of state, and the capital of the Republic of France. Paris is a major cultural and economic center in Western Europe.
    Can we conclude that can we conclude that "Paris is the capital of the United States"? 
    Options are:
    1). yes.
    2). it is not possible
    ===============================
    Prompt: The future of AI is
    Generated text:  already here, and it’s already changing the way we live. But there are still some major challenges to overcome before we can fully realize the power of AI. In this blog post, we’ll discuss what the future of AI will be like and how to prepare for these challenges.
    Firstly, the development of artificial intelligence (AI) is already happening. The field is rapidly evolving and constantly improving, making it possible to make intelligent machines that can perform a wide range of tasks. These machines can learn from data, make decisions, and adapt to new situations, making them incredibly useful in a variety of fields.
    However, there are still some


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a brief description of your profession or role]. I enjoy [insert a brief description of your hobbies or interests]. What do you like to do in your free time? I enjoy [insert a brief description of your hobbies or interests]. What's your favorite hobby? I love [insert a brief description of your favorite hobby]. What's your favorite book or movie? I love [insert a brief description of your favorite book or movie]. What
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city that was founded in 787 AD and is the largest city in Europe by population. It is also the seat of the French government and the country’s cultural and political capital. Paris is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a popular tourist destination, with many visitors coming to see the city's museums, art galleries, and historical sites. Paris is a vibrant and diverse city with a rich history and culture that has influenced the development of modern France. The city is also home to many important institutions of higher education,
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that could be expected in the future:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes, reduce costs, and increase efficiency. As AI technology continues to improve, we can expect to see even more widespread use of AI in healthcare, particularly in areas such as diagnosis, treatment planning, and patient monitoring.
    
    2. Increased use of AI in finance: AI is already being used in finance to improve risk management, fraud detection, and portfolio
    


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
    Generated text:  [Your Name], and I'm an aspiring artist with a passion for painting and sculpture. I'm known for my unique style and my dedication to creating beautiful works of art that inspire and encourage others. I have a keen eye for detail and a love for experimenting with different materials and techniques to create something truly unique. I'm always looking for new challenges and projects to try and stay up-to-date with the latest trends in the art world. What's your favorite color to work with, and what's your favorite hobby that you enjoy pursuing? As for my personal life, I'm an active member of the local art community and enjoy spending time
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    Explanation for the answer: Paris is the largest city in France, located in the central region of the country. It is also the capital city of France, and has a population of over 2.3 million people. It is the administrative and political center of France and is home to the French government, the French parliament, and the major cultural institutions and landmarks of the country. 
    
    Additional Information:
    - France has a rich history and cultural heritage in Paris, including the Louvre Museum, Notre-Dame Cathedral, and the Eiffel Tower.
    - Paris is known for its modern and futuristic architecture, such as the E
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  unpredictable and depends on a number of factors, including advances in computing power, new technologies, and shifts in societal values and expectations. Here are some potential trends that could shape the future of AI:
    
    1. Increased privacy and security concerns: With the increasing use of AI in areas like healthcare, finance, and transportation, there will be a growing need for robust privacy and security measures to protect personal data. This could lead to increased regulation and development of new privacy-preserving algorithms and techniques.
    
    2. Expansion of AI for humanitarian applications: As the world continues to confront challenges such as natural disasters and pandemics, AI could play a more critical role


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

     freelance

     writer

     and

     editor

    .

     I'm

     passionate

     about

     helping

     people

     improve

     their

     writing

     skills

     and

     working

     with

     talented

     writers

     to

     create

     impactful

     content

    .

     I

    'm

     a

     natural

     storyt

    eller

     and

     enjoy

     crafting

     a

     variety

     of

     genres

    ,

     from

     romance

     to

     fantasy

     to

     science

     fiction

    .

     I

     love

     sharing

     my

     writing

     with

     the

     world

     and

     believe

     in

     the

     power

     of

     creativity

     to

     shape

     the

     world

     around

     us

    .

     As

     a

     lifelong

     learner

    ,

     I

    'm

     always

     striving

     to

     improve

     my

     skills

     and

     stay

     up

    -to

    -date

     with

     the

     latest

     trends

     in

     the

     industry

    .

     So

    ,

     if

     you

    're

     looking

     for

     a

     writer

     who

     can

     help

     you

     with

     your

     writing

    ,

     please

     let

     me

     know

     and

     I

    'll

     be

     happy

     to

     help

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    That

    's

     correct

    !

     Paris

    ,

     the

     largest

     city

     in

     France

     and

     the

     capital

    ,

     is

     renowned

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

    's

     also

     known

     for

     its

     rich

     history

    ,

     art

    ,

     and

     cuisine

    .

     If

     you

     want

     to

     know

     the

     most

     famous

     place

     in

     Paris

    ,

     you

     might

     say

     "

    Le

     Lou

    vre

    ,"

     the

     Museum

     of

     Modern

     and

     Modern

     Art

    .

     Enjoy

     exploring

    !

     Paris

     is

     a

     charming

     and

     vibrant

     city

     with

     a

     rich

     history

     and

     a

     vibrant

     cultural

     scene

    .

     Would

     you

     like

     to

     learn

     about

     some

     of

     Paris

    's

     most

     famous

     attractions

     or

     landmarks

    ?

     Paris

     is

     a

     city

     with

     a

     lot

     to

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     shaped

     by

     a

     range

     of

     factors

    ,

     including

     advances

     in

     computing

     power

    ,

     changes

     in

     how

     data

     is

     processed

     and

     analyzed

    ,

     and

     developments

     in

     areas

     like

     robotics

    ,

     speech

     recognition

    ,

     and

     natural

     language

     processing

    .

     Here

     are

     some

     possible

     future

     trends

     in

     AI

     that

     could

     impact

     how

     we

     interact

     with

     technology

     and

     the

     world

     around

     us

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

     With

     advances

     in

     AI

     technology

    ,

     there

     could

     be

     an

     increased

     use

     of

     AI

     in

     healthcare

    ,

     particularly

     in

     areas

     such

     as

     diagnosis

    ,

     drug

     discovery

    ,

     and

     patient

     care

    .

     This

     could

     lead

     to

     more

     accurate

     and

     effective

     medical

     treatments

     and

     a

     reduction

     in

     costs

     associated

     with

     traditional

     medical

     practices

    .
    


    2

    .

     Personal

    ized

     medicine

    



```python
llm.shutdown()
```
