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


    2026-03-31 22:54:09.960 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 22:54:09] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 22:54:09.960 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 22:54:09] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 22:54:09.960 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 22:54:09] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 22:54:09.960 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 22:54:09] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 22:54:09.960 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 22:54:09] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.51it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.49it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:32,  2.67s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:32,  2.67s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:32,  2.67s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:32,  2.67s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:32,  2.67s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:22,  2.36it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:07,  6.20it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.56it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.56it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.56it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.56it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.56it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.56it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.56it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.56it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.66it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.66it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.66it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.66it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.66it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.66it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.66it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 23.44it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 23.44it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 23.44it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 23.44it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 23.44it/s]

    Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 23.44it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 23.44it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 28.69it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 28.69it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 28.69it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 28.69it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 28.69it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 28.69it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 28.69it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 32.99it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 32.99it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 32.99it/s]

    Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 32.99it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 32.99it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 32.99it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 32.99it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 36.79it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 36.79it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 36.79it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 36.79it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 36.79it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 36.79it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 36.79it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 36.79it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 36.79it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.71it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=136.07 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=136.04 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:02, 18.85it/s]Capturing num tokens (num_tokens=7168 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:02, 18.85it/s]Capturing num tokens (num_tokens=6656 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:02, 18.85it/s]Capturing num tokens (num_tokens=6144 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:02, 18.85it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=136.04 GB):   9%|▊         | 5/58 [00:00<00:02, 22.01it/s]Capturing num tokens (num_tokens=5632 avail_mem=134.96 GB):   9%|▊         | 5/58 [00:00<00:02, 22.01it/s]Capturing num tokens (num_tokens=5120 avail_mem=134.96 GB):   9%|▊         | 5/58 [00:00<00:02, 22.01it/s]Capturing num tokens (num_tokens=4608 avail_mem=127.47 GB):   9%|▊         | 5/58 [00:00<00:02, 22.01it/s]Capturing num tokens (num_tokens=4608 avail_mem=127.47 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.93it/s]Capturing num tokens (num_tokens=4096 avail_mem=121.70 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.93it/s]Capturing num tokens (num_tokens=3840 avail_mem=121.69 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.93it/s]Capturing num tokens (num_tokens=3584 avail_mem=121.69 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.93it/s]Capturing num tokens (num_tokens=3328 avail_mem=121.69 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.93it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=121.69 GB):  21%|██        | 12/58 [00:00<00:01, 30.16it/s]Capturing num tokens (num_tokens=3072 avail_mem=121.68 GB):  21%|██        | 12/58 [00:00<00:01, 30.16it/s]Capturing num tokens (num_tokens=2816 avail_mem=121.68 GB):  21%|██        | 12/58 [00:00<00:01, 30.16it/s]Capturing num tokens (num_tokens=2560 avail_mem=121.68 GB):  21%|██        | 12/58 [00:00<00:01, 30.16it/s]Capturing num tokens (num_tokens=2304 avail_mem=121.67 GB):  21%|██        | 12/58 [00:00<00:01, 30.16it/s]Capturing num tokens (num_tokens=2048 avail_mem=121.40 GB):  21%|██        | 12/58 [00:00<00:01, 30.16it/s]Capturing num tokens (num_tokens=2048 avail_mem=121.40 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.45it/s]Capturing num tokens (num_tokens=1792 avail_mem=121.39 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.45it/s]Capturing num tokens (num_tokens=1536 avail_mem=121.39 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.45it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.70 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.45it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.66 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.45it/s]

    Capturing num tokens (num_tokens=960 avail_mem=120.68 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.45it/s] Capturing num tokens (num_tokens=960 avail_mem=120.68 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.26it/s]Capturing num tokens (num_tokens=896 avail_mem=120.67 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.26it/s]Capturing num tokens (num_tokens=832 avail_mem=120.67 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.26it/s]Capturing num tokens (num_tokens=768 avail_mem=120.67 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.26it/s]Capturing num tokens (num_tokens=704 avail_mem=120.66 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.26it/s]Capturing num tokens (num_tokens=640 avail_mem=120.66 GB):  38%|███▊      | 22/58 [00:00<00:00, 38.26it/s]Capturing num tokens (num_tokens=640 avail_mem=120.66 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.51it/s]Capturing num tokens (num_tokens=576 avail_mem=120.66 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.51it/s]Capturing num tokens (num_tokens=512 avail_mem=120.65 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.51it/s]Capturing num tokens (num_tokens=480 avail_mem=120.66 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.51it/s]

    Capturing num tokens (num_tokens=448 avail_mem=120.66 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.51it/s]Capturing num tokens (num_tokens=416 avail_mem=120.66 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.51it/s]Capturing num tokens (num_tokens=416 avail_mem=120.66 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.81it/s]Capturing num tokens (num_tokens=384 avail_mem=120.66 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.81it/s]Capturing num tokens (num_tokens=352 avail_mem=120.65 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.81it/s]Capturing num tokens (num_tokens=320 avail_mem=120.65 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.81it/s]Capturing num tokens (num_tokens=288 avail_mem=120.64 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.81it/s]Capturing num tokens (num_tokens=256 avail_mem=120.64 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.81it/s]Capturing num tokens (num_tokens=256 avail_mem=120.64 GB):  64%|██████▍   | 37/58 [00:00<00:00, 42.91it/s]Capturing num tokens (num_tokens=240 avail_mem=120.64 GB):  64%|██████▍   | 37/58 [00:00<00:00, 42.91it/s]Capturing num tokens (num_tokens=224 avail_mem=120.64 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.91it/s]Capturing num tokens (num_tokens=208 avail_mem=120.63 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.91it/s]

    Capturing num tokens (num_tokens=192 avail_mem=120.30 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.91it/s]Capturing num tokens (num_tokens=176 avail_mem=120.30 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.91it/s]Capturing num tokens (num_tokens=176 avail_mem=120.30 GB):  72%|███████▏  | 42/58 [00:01<00:00, 40.48it/s]Capturing num tokens (num_tokens=160 avail_mem=120.20 GB):  72%|███████▏  | 42/58 [00:01<00:00, 40.48it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 40.48it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 40.48it/s]

    Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  72%|███████▏  | 42/58 [00:01<00:00, 40.48it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  72%|███████▏  | 42/58 [00:01<00:00, 40.48it/s] Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  81%|████████  | 47/58 [00:01<00:00, 27.74it/s]Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  81%|████████  | 47/58 [00:01<00:00, 27.74it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  81%|████████  | 47/58 [00:01<00:00, 27.74it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  81%|████████  | 47/58 [00:01<00:00, 27.74it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  81%|████████  | 47/58 [00:01<00:00, 27.74it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  81%|████████  | 47/58 [00:01<00:00, 27.74it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 31.46it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 31.46it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 31.46it/s]

    Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  90%|████████▉ | 52/58 [00:01<00:00, 31.46it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  90%|████████▉ | 52/58 [00:01<00:00, 31.46it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  90%|████████▉ | 52/58 [00:01<00:00, 31.46it/s] Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  98%|█████████▊| 57/58 [00:01<00:00, 34.98it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  98%|█████████▊| 57/58 [00:01<00:00, 34.98it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 34.48it/s]


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
    Generated text:  David and I am the owner of a small business called "The Galley." I have been working in the food business for over ten years and have a background in marketing and public relations. I have worked for a few different businesses before starting my own business, and I have had the opportunity to see the growth and change that these businesses have gone through.
    My experience includes working with the grocery stores, convenience stores, and restaurants. I have spent a lot of time listening to what the customers are looking for and how they feel and I have been learning that what we offer to our customers is as important as what we sell to them.
    I
    ===============================
    Prompt: The president of the United States is
    Generated text:  to be chosen by the Republican Party. If the president of the United States is a democrat, what is the probability that a Democrat will be chosen? To determine the probability that a Democrat will be chosen as the president of the United States, we need to understand the composition of the U. S. presidentship and the relationship between the party and the president.
    
    1. **Identify the total number of party members:**
       The Republican Party has 50 members, and the Democratic Party has 48 members. Together, they form the U. S. president's party.
    
    2. **Determine the number of Democrats in
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. What is the capital of Spain?
    The answer to the above question is Paris.
    To determine the capital of Spain, we need to understand that Spain is a country, not a city or a continent, and its capital is the city of Madrid. Madrid is the capital of Spain, which is located in the northwestern part of the country. Madrid is the largest city in Spain and serves as the capital for the country's government, as well as being the seat of the European Union. 
    
    Therefore, the answer to the question "What is the capital of Spain? " is Madrid. 
    
    The capital of Spain is Madrid. 
    
    
    ===============================
    Prompt: The future of AI is
    Generated text:  both possible and uncertain, but it offers some promising prospects for improving the quality of life and efficiency for individuals, businesses and organizations around the world. AI has the potential to revolutionize various fields such as healthcare, finance, transportation, and more.
    As AI continues to evolve, it will likely have a significant impact on our daily lives, but it also presents new challenges and opportunities. Here are some of the key areas where AI is expected to make a difference:
    AI will be essential in improving healthcare by providing faster and more accurate diagnoses, better patient care and personalized treatment plans. AI algorithms can analyze vast amounts of medical data to identify patterns and


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [age], [gender], [nationality], [occupation], and I have [number] years of experience in [field of work]. I'm a [type of person], and I'm always looking for ways to [add value to the company]. I'm [positive or negative] about [job title], and I'm always eager to learn and grow. I'm [positive or negative] about [company name], and I'm always
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, which is known for its iconic Eiffel Tower, Notre-Dame Cathedral, and vibrant French culture. It is also the birthplace of the French Revolution and the home of the French language. Paris is a bustling metropolis with a rich history and a diverse population. It is a popular tourist destination and a major economic center in Europe. The city is known for its fashion, art, and cuisine, and is a major hub for international business and diplomacy. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly. The city is also home to many museums, theaters, and other cultural institutions. Paris
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some of the most likely trends that could be expected in the future:
    
    1. Increased integration of AI into everyday life: As AI becomes more integrated into our daily lives, we are likely to see more widespread adoption of AI technologies. This could include things like smart homes, self-driving cars, and virtual assistants that can assist with tasks like scheduling, managing finances, and even making decisions.
    
    2. Greater emphasis on ethical AI: As AI becomes more advanced, there will be a greater emphasis on ethical considerations
    


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
    Generated text:  [insert character's name]. I'm a computer scientist with a master's degree in artificial intelligence. I've been working in the field of artificial intelligence for over a decade, and I'm always on the lookout for new challenges and developments in the field. I'm passionate about learning and exploring new ideas, and I'm always looking for ways to enhance my skills and knowledge in the field. I'm also a good communicator and enjoy helping others to understand complex concepts. So if you ever need any advice or help with anything in the AI field, please don't hesitate to reach out to me. You can always find me on [insert website
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    Here's a concise factual statement about Paris:
    
    Paris, the capital city of France, is renowned for its historic landmarks, rich cultural scene, and iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral.
    
    This statement provides a basic overview of Paris' importance and highlights some of its most famous attractions, but it's a very general statement that could be expanded with more specific details. Let me know if you would like me to make any changes or add more specific information!
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  full of potential, but there are certainly some trends that are likely to shape its trajectory in significant ways. Here are some possible future trends in AI:
    
    1. Increased AI autonomy: One of the biggest trends in AI is the potential for AI to become more autonomous and self-motivated. This could lead to more efficient and effective use of AI in various applications, but it also raises concerns about job displacement and ethical considerations.
    
    2. AI in healthcare: AI is already being used to improve patient care and treatment outcomes, but there are potential benefits and risks associated with its use in healthcare. For example, AI can help identify patterns in medical


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

    type

     of

     character

    ,

     such

     as

     a

     detective

    ,

     historian

    ,

     or

     scientist

    ].

     I

     am

     in

     my

     [

    age

    ]

     years

     old

    ,

     and

     I

     have

     been

     [

    occupation

    ,

     such

     as

     a

     writer

    ,

     artist

    ,

     or

     teacher

    ].

     My

     favorite

     hobby

     is

     [

    mention

     your

     hobbies

     or

     interests

    ,

     such

     as

     reading

    ,

     watching

     movies

    ,

     or

     playing

     chess

    ].

     I

     am

     [

    any

     profession

     or

     position

    ,

     such

     as

     a

     police

     officer

    ,

     teacher

    ,

     or

     artist

    ].

     I

     love

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

     am

     always

     on

     the

     lookout

     for

     [

    opport

    unities

     to

     learn

     or

     grow

    ].

     I

     am

     [

    any

     self

    -ref

    lection

     or

     qualities

     that

     make

     you

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     It

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

     Notre

    -D

    ame

     Cathedral

    ,

     and

     Lou

    vre

     Museum

    .

     The

     city

     is

     also

     home

     to

     many

     renowned

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

    ,

     and

     is

     a

     major

     center

     for

     art

     and

     culture

    .

     Paris

     is

     a

     popular

     tourist

     destination

    ,

     with

     its

     wide

     bou

    lev

    ards

    ,

     charming

     neighborhoods

    ,

     and

     historical

     sites

    .

     The

     city

     has

     a

     strong

     French

     language

     identity

    ,

     with

     many

     French

    -language

     media

     outlets

    ,

     restaurants

    ,

     and

     cultural

     institutions

    .

     It

     is

     a

     popular

     destination

     for

     both

     locals

     and

     visitors

    ,

     with

     its

     cultural

    ,

     historical

    ,

     and

     entertainment

     offerings

    .

     Paris

     is

     known

     for

     its

     rich

     history

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     several

     trends

    :
    


    1

    .

     Increased

     integration

     with

     other

     technologies

    :

     With

     the

     rise

     of

     the

     Internet

     of

     Things

     (

    Io

    T

    ),

     AI

     will

     increasingly

     integrate

     with

     other

     technologies

     such

     as

     smart

     homes

    ,

     wearable

     devices

    ,

     and

     autonomous

     vehicles

    .

     This

     integration

     will

     enable

     AI

     systems

     to

     learn

     and

     adapt

     to

     new

     environments

     and

     interactions

    .
    


    2

    .

     Enhanced

     privacy

     concerns

    :

     As

     AI

     systems

     become

     more

     integrated

     with

     other

     technologies

    ,

     there

     will

     be

     greater

     concerns

     about

     privacy

     and

     data

     protection

    .

     Governments

     and

     organizations

     will

     need

     to

     find

     ways

     to

     protect

     personal

     information

     and

     ensure

     that

     AI

     systems

     are

     not

     used

     in

     ways

     that

     violate

     privacy

    .
    


    3

    .

     Cognitive

     edge

    :

     AI

     systems

     will

     become

     more

     capable

     of

    



```python
llm.shutdown()
```
