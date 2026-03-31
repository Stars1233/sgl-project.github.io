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


    2026-03-31 18:39:25.662 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 18:39:25] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 18:39:25.663 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 18:39:25] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 18:39:25.663 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 18:39:25] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 18:39:25.663 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 18:39:25] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 18:39:25.663 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 18:39:25] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.49it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.48it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:47,  2.94s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:47,  2.94s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:47,  2.94s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:03<02:47,  2.94s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:31,  1.71it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:31,  1.71it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:31,  1.71it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:03<00:31,  1.71it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:03<00:31,  1.71it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:03<00:31,  1.71it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:03<00:31,  1.71it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:03<00:09,  5.29it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:03<00:09,  5.29it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:03<00:09,  5.29it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:03<00:09,  5.29it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:03<00:09,  5.29it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:03<00:09,  5.29it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:03<00:09,  5.29it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:03<00:09,  5.29it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:09,  5.29it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 11.25it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 11.25it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 11.25it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 11.25it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 11.25it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 11.25it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 11.25it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 11.25it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 17.02it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 17.02it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 17.02it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 17.02it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 17.02it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 17.02it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 17.02it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 17.02it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 23.42it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 23.42it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 23.42it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 23.42it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 23.42it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 23.42it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 23.42it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 28.28it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 28.28it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 28.28it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 28.28it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 28.28it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 28.28it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 28.28it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 33.58it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 33.58it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 33.58it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 33.58it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 33.58it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 33.58it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 33.58it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 36.92it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 36.92it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 36.92it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 36.92it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 36.92it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 36.92it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 36.92it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 36.92it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 36.92it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 45.84it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 14.74it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.43 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.31 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.31 GB):   3%|▎         | 2/58 [00:00<00:03, 17.61it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.61it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.61it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   7%|▋         | 4/58 [00:00<00:03, 16.67it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   7%|▋         | 4/58 [00:00<00:03, 16.67it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.29 GB):   7%|▋         | 4/58 [00:00<00:03, 16.67it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=120.29 GB):  10%|█         | 6/58 [00:00<00:04, 12.20it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.29 GB):  10%|█         | 6/58 [00:00<00:04, 12.20it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):  10%|█         | 6/58 [00:00<00:04, 12.20it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:04, 11.25it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:04, 11.25it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:04, 11.25it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  14%|█▍        | 8/58 [00:00<00:04, 11.25it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  19%|█▉        | 11/58 [00:00<00:03, 15.46it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  19%|█▉        | 11/58 [00:00<00:03, 15.46it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.27 GB):  19%|█▉        | 11/58 [00:00<00:03, 15.46it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.31 GB):  19%|█▉        | 11/58 [00:00<00:03, 15.46it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.31 GB):  19%|█▉        | 11/58 [00:00<00:03, 15.46it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=119.08 GB):  19%|█▉        | 11/58 [00:00<00:03, 15.46it/s]Capturing num tokens (num_tokens=2304 avail_mem=119.08 GB):  28%|██▊       | 16/58 [00:00<00:01, 22.71it/s]Capturing num tokens (num_tokens=2048 avail_mem=119.08 GB):  28%|██▊       | 16/58 [00:00<00:01, 22.71it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.98 GB):  28%|██▊       | 16/58 [00:00<00:01, 22.71it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.98 GB):  28%|██▊       | 16/58 [00:00<00:01, 22.71it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.97 GB):  28%|██▊       | 16/58 [00:00<00:01, 22.71it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.97 GB):  34%|███▍      | 20/58 [00:00<00:01, 27.03it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.95 GB):  34%|███▍      | 20/58 [00:00<00:01, 27.03it/s]Capturing num tokens (num_tokens=960 avail_mem=118.97 GB):  34%|███▍      | 20/58 [00:01<00:01, 27.03it/s] Capturing num tokens (num_tokens=896 avail_mem=118.96 GB):  34%|███▍      | 20/58 [00:01<00:01, 27.03it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  34%|███▍      | 20/58 [00:01<00:01, 27.03it/s]

    Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  41%|████▏     | 24/58 [00:01<00:01, 30.31it/s]Capturing num tokens (num_tokens=768 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:01<00:01, 30.31it/s]Capturing num tokens (num_tokens=704 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:01<00:01, 30.31it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:01<00:01, 30.31it/s]Capturing num tokens (num_tokens=576 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:01<00:01, 30.31it/s]Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  41%|████▏     | 24/58 [00:01<00:01, 30.31it/s]Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  50%|█████     | 29/58 [00:01<00:00, 34.16it/s]Capturing num tokens (num_tokens=480 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:01<00:00, 34.16it/s]Capturing num tokens (num_tokens=448 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:01<00:00, 34.16it/s]Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:01<00:00, 34.16it/s]Capturing num tokens (num_tokens=384 avail_mem=118.94 GB):  50%|█████     | 29/58 [00:01<00:00, 34.16it/s]

    Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  50%|█████     | 29/58 [00:01<00:00, 34.16it/s]Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  59%|█████▊    | 34/58 [00:01<00:00, 37.10it/s]Capturing num tokens (num_tokens=320 avail_mem=118.94 GB):  59%|█████▊    | 34/58 [00:01<00:00, 37.10it/s]Capturing num tokens (num_tokens=288 avail_mem=118.93 GB):  59%|█████▊    | 34/58 [00:01<00:00, 37.10it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  59%|█████▊    | 34/58 [00:01<00:00, 37.10it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  59%|█████▊    | 34/58 [00:01<00:00, 37.10it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  66%|██████▌   | 38/58 [00:01<00:00, 36.87it/s]Capturing num tokens (num_tokens=224 avail_mem=118.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 36.87it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 36.87it/s]

    Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 36.87it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 36.87it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  72%|███████▏  | 42/58 [00:01<00:00, 27.87it/s]Capturing num tokens (num_tokens=160 avail_mem=118.91 GB):  72%|███████▏  | 42/58 [00:01<00:00, 27.87it/s]Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  72%|███████▏  | 42/58 [00:01<00:00, 27.87it/s]

    Capturing num tokens (num_tokens=128 avail_mem=118.91 GB):  72%|███████▏  | 42/58 [00:01<00:00, 27.87it/s]Capturing num tokens (num_tokens=112 avail_mem=118.90 GB):  72%|███████▏  | 42/58 [00:01<00:00, 27.87it/s]Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  72%|███████▏  | 42/58 [00:01<00:00, 27.87it/s] Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  81%|████████  | 47/58 [00:01<00:00, 31.75it/s]Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  81%|████████  | 47/58 [00:01<00:00, 31.75it/s]Capturing num tokens (num_tokens=64 avail_mem=118.89 GB):  81%|████████  | 47/58 [00:01<00:00, 31.75it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  81%|████████  | 47/58 [00:01<00:00, 31.75it/s]Capturing num tokens (num_tokens=32 avail_mem=118.88 GB):  81%|████████  | 47/58 [00:01<00:00, 31.75it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  81%|████████  | 47/58 [00:01<00:00, 31.75it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  90%|████████▉ | 52/58 [00:01<00:00, 34.91it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  90%|████████▉ | 52/58 [00:01<00:00, 34.91it/s]

    Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  90%|████████▉ | 52/58 [00:01<00:00, 34.91it/s]Capturing num tokens (num_tokens=16 avail_mem=118.87 GB):  90%|████████▉ | 52/58 [00:01<00:00, 34.91it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  90%|████████▉ | 52/58 [00:01<00:00, 34.91it/s]Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  90%|████████▉ | 52/58 [00:01<00:00, 34.91it/s] Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  98%|█████████▊| 57/58 [00:02<00:00, 37.26it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB):  98%|█████████▊| 57/58 [00:02<00:00, 37.26it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:02<00:00, 28.63it/s]


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
    Generated text:  Yu, and I have been a member of the society of which group?
    A. European Students
    B. American Students
    C. Students from Japan
    D. Students from Asia
    Answer:
    
    B
    
    According to the passage, which of the following options most accurately summarizes the main idea of the text?
    A. It discusses the importance of obtaining proper parental supervision.
    B. It describes the different aspects of life for children.
    C. It explains the process of preparing for the future.
    D. It highlights the difficulty of providing for the children.
    Answer:
    
    B
    
    In a company's marketing strategy, what does the term 'master product'
    ===============================
    Prompt: The president of the United States is
    Generated text:  a unique individual whose personality and actions have a direct influence on the country. Just like how a president is a central figure in a nation's governance, he has a profound impact on the nation's development and progress. His leadership style, decision-making, and commitment to serving the country are integral to its growth. The president's power is also a central aspect of the country's identity, and it is a significant factor in the country's reputation and influence. The presidency of the United States has played a crucial role in the nation's history and has influenced its future. As such, the leadership style and decision-making of the president are crucial to
    ===============================
    Prompt: The capital of France is
    Generated text: :
    
    A) Paris
    B) Nice
    C) Lille
    D) Nantes
    The capital of France is Paris. Paris is the largest city in France and is located in the central part of the country, on the River Seine. It is the most populous city in Europe and is known for its historical architecture, museums, and vibrant cultural scene.
    
    Therefore, the correct answer is \boxed{A}.
    ===============================
    Prompt: The future of AI is
    Generated text:  uncertain, but in the near future it will be a strong contender for success. It is known as the quantum computer. It is a quantum computer that uses the principles of quantum physics, which is the study of the principles of matter and energy in the quantum world. The quantum computer is expected to be much faster than any other computer, which is why it is often called the future of AI.
    The quantum computer works by using quantum bits, or qubits. These qubits can be in multiple states at once, which allows them to perform calculations more efficiently than traditional computers. This makes the quantum computer much more powerful than any other computer,


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you. What can you tell me about yourself? I'm a [job title] at
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. It is the largest city in France and the third largest in the world. The city is known for its rich history, beautiful architecture, and vibrant culture. It is home to many famous landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. Paris is also a major center for business, finance, and tourism, and is a popular destination for tourists and locals alike. The city is home to many cultural institutions and events throughout the year, including the annual Eiffel Tower Festival and the annual Parisian Film Festival. Paris is a city of contrasts, with its modern architecture and historical landmarks
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: As AI becomes more advanced, it is likely to become more integrated with human intelligence, allowing it to learn from and adapt to human behavior and decision-making processes.
    
    2. Greater emphasis on ethical considerations: As AI becomes more advanced, there will be a greater emphasis on ethical considerations, including issues such as bias, transparency, and accountability.
    
    3. Greater use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs. As AI becomes more advanced, it is likely to be used in even more areas, including diagnosis,
    


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
    Generated text:  [Your Name]. I'm a [insert your occupation here], and I'm [insert your profession here]. I'm here to [insert your profession here], and I'm here to [insert your profession here]. Thank you for having me. 
    I'm a full-time professional in the field of [insert your field of interest here], and I've always been passionate about [insert your passion here]. I've always been passionate about [insert your passion here] and I've been working in the field of [insert your field of interest here] for [insert your duration of experience here] years now. I have a passion for [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, which is known as the "City of Light" for its striking architecture, vibrant culture, and annual Paris Festival. It is the world's 10th largest city by population and is the cultural and economic center of France. Paris has been a major cultural center since the 12th century, attracting artists, intellectuals, and politicians. The city has a rich history and is home to many famous landmarks such as Notre Dame Cathedral and the Eiffel Tower. The capital is also a major trading center, and Parisians are known for their love of the French language and the French cuisine. Overall, Paris is a unique
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  incredibly exciting, and there are many potential trends we can expect to see in the coming years. Some of the most common trends include:
    
    1. Increased transparency and explainability: As AI becomes more complex and sophisticated, we will see a greater emphasis on making AI systems more transparent and explainable. This means that we will need to develop better ways to explain AI decisions and train machines to do so.
    
    2. More personalized experiences: As AI becomes more integrated into our lives, we will see a growing emphasis on designing AI systems that are more personalized and context-aware. This means that machines will be able to learn from our behavior and adapt to


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

    role

    ]

     at

     [

    company

    ].

     I

    'm

     excited

     to

     meet

     you

     and

     discuss

     the

     opportunities

     and

     challenges

     we

     can

     address

     together

    .

     Let

    's

     get

     started

    !

     

    🌟

    ✨

    
    


    Remember

    ,

     the

     goal

     is

     to

     create

     an

     environment

     where

     people

     feel

     safe

     to

     share

     their

     thoughts

    ,

     ideas

    ,

     and

     problems

     without

     fear

     of

     judgment

     or

     re

    tribution

    .

     Let

    's

     do

     this

     together

    !

     

    🎉

     #

    Friendly

    Space

    
    


    #

    Tell

    It

    Now

     -

     What

     is

     your

     role

     at

     [

    Company

    ]?

     #

    Talk

    Talk

     -

     Tell

     a

     joke

    .

     #

    Travel

    G

    ems

     -

     What

    's

     the

     name

     of

     your

     favorite

     tourist

     destination

    ?

     #

    M

    edit

    ation

    M

    agnet

     -

     What

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    Paris

    ,

     the

     city

     of

     love

    ,

     is

     a

     bustling

     met

    ropolis

     of

     

    2

    .

    1

     million

     people

    ,

     known

     for

     its

     world

    -f

    amous

     landmarks

     such

     as

     the

     E

    iff

    el

     Tower

    ,

     Lou

    vre

     Museum

    ,

     Notre

    -D

    ame

     Cathedral

    ,

     and

     Paris

     Opera

     House

    ,

     as

     well

     as

     its

     renowned

     cuisine

    ,

     fashion

    ,

     and

     art

     scene

    .

     Its

     historical

     and

     cultural

     importance

     is

     reflected

     in

     its

     iconic

     architecture

     and

     its

     role

     as

     a

     major

     financial

     and

     cultural

     center

     of

     the

     European

     Union

    .

     Paris

     is

     also

     home

     to

     the

     French

     Parliament

    ,

     the

     Supreme

     Court

    ,

     and

     the

     E

    iff

    el

     Tower

    ,

     making

     it

     a

     culturally

     and

     politically

     significant

     city

     in

     France

    .

     The

     city

    's

     name

     translates

     to

     "

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     an

     explosion

     of

     new

     capabilities

    ,

     continuous

     innovation

    ,

     and

     the

     emergence

     of

     new

     parad

    ig

    ms

     that

     challenge

     our

     existing

     knowledge

     and

     assumptions

    .

     Here

     are

     some

     potential

     trends

     that

     could

     shape

     the

     future

     of

     AI

    :
    


    1

    .

     Increased

     focus

     on

     personal

    ization

    :

     With

     AI

    ,

     businesses

     are

     able

     to

     analyze

     vast

     amounts

     of

     data

     and

     learn

     from

     it

     to

     provide

     highly

     personalized

     experiences

     to

     their

     customers

    .

     This

     could

     lead

     to

     a

     new

     era

     of

     "

    personal

    ization

     by

     algorithms

    ,"

     where

     AI

    -driven

     systems

     take

     the

     unique

     characteristics

     of

     individual

     customers

     and

     use

     them

     to

     improve

     their

     overall

     experience

    .
    


    2

    .

     Improved

     ethical

     standards

    :

     As

     AI

     continues

     to

     evolve

    ,

     we

    'll

     need

     to

     develop

     new

    



```python
llm.shutdown()
```
