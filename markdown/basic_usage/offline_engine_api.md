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


    2026-03-31 00:58:40.316 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 00:58:40] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 00:58:40.316 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 00:58:40] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 00:58:40.316 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 00:58:40] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 00:58:40.316 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 00:58:40] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 00:58:40.316 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 00:58:40] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.54it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.54it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:25,  2.14it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.51it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.51it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.51it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.51it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.51it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.51it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.51it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.51it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.51it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:02, 13.62it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:02, 13.62it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:02, 13.62it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:02, 13.62it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:02, 13.62it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:02, 13.62it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:02, 13.62it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:02, 13.62it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 19.87it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 19.87it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 19.87it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 19.87it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 19.87it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 19.87it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 19.87it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:02<00:01, 23.64it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:02<00:01, 23.64it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:02<00:01, 23.64it/s]

    Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:02<00:01, 23.64it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:02<00:01, 23.64it/s]Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:03<00:01, 23.64it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:03<00:00, 27.22it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:03<00:00, 27.22it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:03<00:00, 27.22it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:03<00:00, 27.22it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:03<00:00, 27.22it/s]Compiling num tokens (num_tokens=192):  62%|██████▏   | 36/58 [00:03<00:00, 27.22it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:03<00:00, 30.74it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:03<00:00, 30.74it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:03<00:00, 30.74it/s]

    Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:03<00:00, 30.74it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:03<00:00, 30.74it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:03<00:00, 30.74it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:03<00:00, 33.92it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:03<00:00, 33.92it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:03<00:00, 33.92it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:03<00:00, 33.92it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:03<00:00, 33.92it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:03<00:00, 33.92it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 37.29it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 37.29it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 37.29it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 37.29it/s]

    Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 37.29it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 37.29it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 37.29it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 37.29it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.79it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.75 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.72 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.72 GB):   3%|▎         | 2/58 [00:00<00:02, 19.11it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.71 GB):   3%|▎         | 2/58 [00:00<00:02, 19.11it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.71 GB):   3%|▎         | 2/58 [00:00<00:02, 19.11it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.71 GB):   3%|▎         | 2/58 [00:00<00:02, 19.11it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=118.71 GB):   9%|▊         | 5/58 [00:00<00:02, 22.35it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.71 GB):   9%|▊         | 5/58 [00:00<00:02, 22.35it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.71 GB):   9%|▊         | 5/58 [00:00<00:02, 22.35it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.71 GB):   9%|▊         | 5/58 [00:00<00:02, 22.35it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.71 GB):   9%|▊         | 5/58 [00:00<00:02, 22.35it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.71 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.16it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.70 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.16it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.37 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.16it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=118.65 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.16it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.65 GB):  21%|██        | 12/58 [00:00<00:01, 24.22it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.66 GB):  21%|██        | 12/58 [00:00<00:01, 24.22it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.66 GB):  21%|██        | 12/58 [00:00<00:01, 24.22it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.65 GB):  21%|██        | 12/58 [00:00<00:01, 24.22it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.65 GB):  26%|██▌       | 15/58 [00:00<00:01, 22.91it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.44 GB):  26%|██▌       | 15/58 [00:00<00:01, 22.91it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=118.63 GB):  26%|██▌       | 15/58 [00:00<00:01, 22.91it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.64 GB):  26%|██▌       | 15/58 [00:00<00:01, 22.91it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.64 GB):  31%|███       | 18/58 [00:00<00:01, 23.10it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.63 GB):  31%|███       | 18/58 [00:00<00:01, 23.10it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.63 GB):  31%|███       | 18/58 [00:00<00:01, 23.10it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.60 GB):  31%|███       | 18/58 [00:00<00:01, 23.10it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.60 GB):  36%|███▌      | 21/58 [00:00<00:01, 24.21it/s]Capturing num tokens (num_tokens=960 avail_mem=118.61 GB):  36%|███▌      | 21/58 [00:00<00:01, 24.21it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=118.61 GB):  36%|███▌      | 21/58 [00:00<00:01, 24.21it/s]Capturing num tokens (num_tokens=832 avail_mem=118.60 GB):  36%|███▌      | 21/58 [00:00<00:01, 24.21it/s]Capturing num tokens (num_tokens=832 avail_mem=118.60 GB):  41%|████▏     | 24/58 [00:00<00:01, 25.59it/s]Capturing num tokens (num_tokens=768 avail_mem=118.60 GB):  41%|████▏     | 24/58 [00:00<00:01, 25.59it/s]Capturing num tokens (num_tokens=704 avail_mem=118.59 GB):  41%|████▏     | 24/58 [00:01<00:01, 25.59it/s]Capturing num tokens (num_tokens=640 avail_mem=118.58 GB):  41%|████▏     | 24/58 [00:01<00:01, 25.59it/s]Capturing num tokens (num_tokens=576 avail_mem=118.58 GB):  41%|████▏     | 24/58 [00:01<00:01, 25.59it/s]Capturing num tokens (num_tokens=576 avail_mem=118.58 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.95it/s]Capturing num tokens (num_tokens=512 avail_mem=118.56 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.95it/s]

    Capturing num tokens (num_tokens=480 avail_mem=118.58 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.95it/s]Capturing num tokens (num_tokens=448 avail_mem=118.57 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.95it/s]Capturing num tokens (num_tokens=416 avail_mem=118.55 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.95it/s]Capturing num tokens (num_tokens=416 avail_mem=118.55 GB):  55%|█████▌    | 32/58 [00:01<00:00, 30.66it/s]Capturing num tokens (num_tokens=384 avail_mem=118.56 GB):  55%|█████▌    | 32/58 [00:01<00:00, 30.66it/s]Capturing num tokens (num_tokens=352 avail_mem=118.55 GB):  55%|█████▌    | 32/58 [00:01<00:00, 30.66it/s]Capturing num tokens (num_tokens=320 avail_mem=118.54 GB):  55%|█████▌    | 32/58 [00:01<00:00, 30.66it/s]Capturing num tokens (num_tokens=288 avail_mem=118.53 GB):  55%|█████▌    | 32/58 [00:01<00:00, 30.66it/s]Capturing num tokens (num_tokens=288 avail_mem=118.53 GB):  62%|██████▏   | 36/58 [00:01<00:00, 32.79it/s]Capturing num tokens (num_tokens=256 avail_mem=118.53 GB):  62%|██████▏   | 36/58 [00:01<00:00, 32.79it/s]

    Capturing num tokens (num_tokens=240 avail_mem=118.52 GB):  62%|██████▏   | 36/58 [00:01<00:00, 32.79it/s]Capturing num tokens (num_tokens=224 avail_mem=118.52 GB):  62%|██████▏   | 36/58 [00:01<00:00, 32.79it/s]Capturing num tokens (num_tokens=208 avail_mem=118.51 GB):  62%|██████▏   | 36/58 [00:01<00:00, 32.79it/s]Capturing num tokens (num_tokens=208 avail_mem=118.51 GB):  69%|██████▉   | 40/58 [00:01<00:00, 34.79it/s]Capturing num tokens (num_tokens=192 avail_mem=118.52 GB):  69%|██████▉   | 40/58 [00:01<00:00, 34.79it/s]Capturing num tokens (num_tokens=176 avail_mem=118.52 GB):  69%|██████▉   | 40/58 [00:01<00:00, 34.79it/s]Capturing num tokens (num_tokens=160 avail_mem=118.51 GB):  69%|██████▉   | 40/58 [00:01<00:00, 34.79it/s]Capturing num tokens (num_tokens=144 avail_mem=118.50 GB):  69%|██████▉   | 40/58 [00:01<00:00, 34.79it/s]Capturing num tokens (num_tokens=144 avail_mem=118.50 GB):  76%|███████▌  | 44/58 [00:01<00:00, 36.10it/s]Capturing num tokens (num_tokens=128 avail_mem=118.50 GB):  76%|███████▌  | 44/58 [00:01<00:00, 36.10it/s]

    Capturing num tokens (num_tokens=112 avail_mem=118.49 GB):  76%|███████▌  | 44/58 [00:01<00:00, 36.10it/s]Capturing num tokens (num_tokens=96 avail_mem=118.48 GB):  76%|███████▌  | 44/58 [00:01<00:00, 36.10it/s] Capturing num tokens (num_tokens=80 avail_mem=118.48 GB):  76%|███████▌  | 44/58 [00:01<00:00, 36.10it/s]Capturing num tokens (num_tokens=80 avail_mem=118.48 GB):  83%|████████▎ | 48/58 [00:01<00:00, 36.96it/s]Capturing num tokens (num_tokens=64 avail_mem=118.47 GB):  83%|████████▎ | 48/58 [00:01<00:00, 36.96it/s]Capturing num tokens (num_tokens=48 avail_mem=118.46 GB):  83%|████████▎ | 48/58 [00:01<00:00, 36.96it/s]Capturing num tokens (num_tokens=32 avail_mem=118.45 GB):  83%|████████▎ | 48/58 [00:01<00:00, 36.96it/s]Capturing num tokens (num_tokens=28 avail_mem=118.45 GB):  83%|████████▎ | 48/58 [00:01<00:00, 36.96it/s]Capturing num tokens (num_tokens=28 avail_mem=118.45 GB):  90%|████████▉ | 52/58 [00:01<00:00, 37.46it/s]Capturing num tokens (num_tokens=24 avail_mem=118.45 GB):  90%|████████▉ | 52/58 [00:01<00:00, 37.46it/s]

    Capturing num tokens (num_tokens=20 avail_mem=118.44 GB):  90%|████████▉ | 52/58 [00:01<00:00, 37.46it/s]Capturing num tokens (num_tokens=16 avail_mem=118.42 GB):  90%|████████▉ | 52/58 [00:01<00:00, 37.46it/s]Capturing num tokens (num_tokens=12 avail_mem=118.43 GB):  90%|████████▉ | 52/58 [00:01<00:00, 37.46it/s]Capturing num tokens (num_tokens=12 avail_mem=118.43 GB):  97%|█████████▋| 56/58 [00:01<00:00, 29.84it/s]Capturing num tokens (num_tokens=8 avail_mem=118.42 GB):  97%|█████████▋| 56/58 [00:01<00:00, 29.84it/s] Capturing num tokens (num_tokens=4 avail_mem=118.42 GB):  97%|█████████▋| 56/58 [00:01<00:00, 29.84it/s]

    Capturing num tokens (num_tokens=4 avail_mem=118.42 GB): 100%|██████████| 58/58 [00:01<00:00, 29.22it/s]


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
    Generated text:  Zora Neale Hurston and I'm an anthropologist working on two topics in the anthropology field: the construction of community and the language of the African American community. I have a B.A. in anthropology from Miami University of Ohio, an M.A. in African American Studies from Georgia Southern University, and an M.S. in anthropology from the University of Minnesota. I'm also a facilitator for the Hazelden Betty Ford Center's Family and Friends Training. Zora is also a professor of anthropology at the University of North Carolina at Greensboro.
    I've worked at the Family and Friends Training Institute since 2002 and
    ===============================
    Prompt: The president of the United States is
    Generated text:  representing a country with a population of 300 million. The president is planning to travel around the country to give speeches. If he travels by plane, it takes 4 hours to complete one round trip. If he travels by train, it takes 2 hours to complete one round trip. If he travels by boat, it takes him 1 hour to complete one round trip. He plans to make 5 round trips by plane, 3 by train, and 2 by boat. How many hours will the president spend traveling?
    To determine the total time the president will spend traveling, we need to calculate the time spent by
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. But Paris is a city in France. The capital of France is Paris. If the capital of France is not Paris, then it cannot be Paris. This can be represented by the statement: If $p$ then $q$. I. The capital of France is Paris. II. The capital of France is not Paris. III. $p$ then $q$. 
    Options:
    A) Only I
    B) Only II
    C) Only III
    D) I, II, and III
    E) Only III To solve this problem, we need to analyze the logical statement and determine which of the given options are correct
    ===============================
    Prompt: The future of AI is
    Generated text:  here, and it’s here to stay. As our reliance on machines and artificial intelligence continues to grow, it’s essential to know how to make the most of them. One of the key ways to do this is by understanding how to code. Whether you’re a beginner or an experienced coder, there are many resources available to help you learn and improve your coding skills. In this article, we’ll explore some of the best resources available to learn how to code.
    The first step in learning how to code is to choose a programming language. There are many different programming languages available, including Python, Java, C++, and more. Each language


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


    Generated text:  [Name] and I'm a [occupation] with [number] years of experience in [field]. I'm a [type of person] who is always [positive trait]. I'm [age] years old, and I'm [gender]. I'm [occupation] and I'm [number] years old. I'm [type of person] and I'm always [positive trait]. I'm [age] years old, and I'm [gender]. I'm [occupation] and I'm [number] years old. I'm [type of person] and I'm always [positive trait]. I'm [age] years
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a bustling city with a rich cultural heritage and is a major tourist destination. It is also known for its cuisine, including its famous croissants and its famous pastries. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly together. It is a city that has been a center of power and culture for over 2000 years. The city is also
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs. As AI technology continues to improve, we can expect to see even more widespread adoption in healthcare.
    
    2. AI in finance: AI is already being used in finance to improve fraud detection and risk management. As AI technology continues to improve, we can expect to see even more widespread adoption in finance.
    
    3. AI in manufacturing: AI is already being used in manufacturing to improve efficiency and
    


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
    Generated text:  [Your Name], and I'm an AI language model. I don't have emotions or physical presence, but I can assist with any questions you have. How can I help you today? Let me know if you have any questions about my capabilities or if there's anything specific you would like to know about me. Your kind support is greatly appreciated. 🌟✨ #AI #LanguageModeler #SelfIntroduction 🌏✨ #TechGuru #TechSavvy
    
    Hey there, I'm [Your Name], an AI language model. I'm here to assist you with any questions you have, so don't hesitate to
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as "la París." It is located on the River Seine and is the largest city in Europe by population, with a population of over 7 million. It is home to many iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. Paris is known for its rich cultural heritage, including the Gare de Lyon train station and the Opéra Garnier opera house. It is also known for its fashion, arts, and entertainment scenes, as well as its cuisine and music. Paris is a bustling city with a diverse population, and it is a popular tourist destination
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  filled with both opportunities and challenges. Some possible trends include:
    
      1. Advancements in AI algorithms and models: AI is continuously evolving and improving, with new algorithms and models being developed and refined every day. This means that AI is likely to become more sophisticated and capable of handling increasingly complex problems.
      2. Increased use of AI in healthcare: AI is already being used in the healthcare industry to assist with diagnosis, treatment, and patient care. As AI technology continues to improve, we may see even more widespread adoption in this area.
      3. Increased use of AI in transportation: AI is already being used in the


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

    name

    ]

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

    'm

     really

     passionate

     about

     [

    why

     you

     love

     what

     you

     do

    ],

     and

     I

     always

     strive

     to

     do

     my

     best

     work

     and

     make

     a

     positive

     impact

    .

     I

    'm

     a

     [

    name

    ],

     [

    job

     title

    ],

     at

     [

    company

     name

    ],

     and

     I

    'm

     always

     eager

     to

     learn

     and

     grow

    .

     I

    'm

     a

     team

     player

     and

     enjoy

     working

     collabor

    atively

     with

     others

     to

     achieve

     our

     goals

    .

     I

     thrive

     on

     creativity

     and

     always

     strive

     to

     push

     boundaries

    .

     I

    'm

     a

     [

    name

    ],

     [

    job

     title

    ],

     at

     [

    company

     name

    ],

     and

     I

    'm

     always

     excited

     to

     help

     my

     colleagues

     and

     team

     succeed

    .

     Thank

     you

     for

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     which

     is

     the

     largest

     city

     in

     the

     country

     and

     home

     to

     the

     country

    ’s

     political

    ,

     cultural

    ,

     and

     economic

     center

    .

     Paris

     is

     also

     the

     country

    's

     most

     populous

     city

     and

     is

     known

     for

     its

     iconic

     landmarks

    ,

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

     is

     a

     major

     center

     for

     arts

    ,

     music

    ,

     fashion

    ,

     and

     international

     trade

    .

     The

     city

     is

     also

     known

     for

     its

     cuisine

     and

     museums

    ,

     such

     as

     the

     Mus

    ée

     d

    '

    Or

    say

     and

     the

     Mus

    ée

     d

    '

    Or

    say

    .

     With

     its

     historical

     and

     cultural

     heritage

     and

     modern

     life

    ,

     Paris

     is

     a

     city

     that

     has

     captured

     the

     imag

    inations

     of

     people

     from

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     exciting

     and

     constantly

     evolving

    .

     Here

     are

     some

     possible

     trends

     in

     AI

     that

     we

     can

     expect

    :
    


    1

    .

     Increased

     sophistication

    :

     AI

     is

     becoming

     more

     advanced

     and

     capable

     of

     performing

     more

     complex

     tasks

    .

     We

     can

     expect

     to

     see

     more

     sophisticated

     AI

     systems

     that

     can

     learn

     from

     and

     adapt

     to

     new

     data

    ,

     as

     well

     as

     better

     understand

     and

     anticipate

     human

     behavior

    .
    


    2

    .

     Integration

     with

     other

     technologies

    :

     AI

     is

     becoming

     more

     integrated

     with

     other

     technologies

    ,

     such

     as

     healthcare

    ,

     finance

    ,

     and

     transportation

    .

     We

     can

     expect

     to

     see

     more

     seamless

     integration

     of

     AI

     into

     these

     sectors

    ,

     as

     well

     as

     the

     integration

     of

     AI

     into

     other

     areas

     of

     society

    .
    


    3

    .

     Personal

    ization

    :

     AI

     is

     becoming

     more

     personalized

     and

     able

    



```python
llm.shutdown()
```
