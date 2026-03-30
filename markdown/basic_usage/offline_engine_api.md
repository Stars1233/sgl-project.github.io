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


    2026-03-30 12:25:45.223 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 12:25:45] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 12:25:45.223 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 12:25:45] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 12:25:45.223 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 12:25:45] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 12:25:45.223 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 12:25:45] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 12:25:45.224 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 12:25:45] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.56it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.54it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:20,  2.65it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:20,  2.65it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:20,  2.65it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:20,  2.65it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:20,  2.65it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:20,  2.65it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:20,  2.65it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:06,  6.90it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.83it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.83it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.83it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.83it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.83it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.83it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.83it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 19.34it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 19.34it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 19.34it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 19.34it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 19.34it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 19.34it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 19.34it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:02<00:01, 19.34it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 26.49it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 26.49it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 26.49it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:00, 26.49it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:00, 26.49it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:02<00:00, 26.49it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:02<00:00, 26.49it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 31.65it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 31.65it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 31.65it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 31.65it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 31.65it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 31.65it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 31.65it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 37.11it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 37.11it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 37.11it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 37.11it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 37.11it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 37.11it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 37.11it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 40.19it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 40.19it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 40.19it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 40.19it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 40.19it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 40.19it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 40.19it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 40.19it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 40.19it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.41it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=136.07 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=136.04 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:02, 18.90it/s]Capturing num tokens (num_tokens=7168 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:02, 18.90it/s]Capturing num tokens (num_tokens=6656 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:02, 18.90it/s]Capturing num tokens (num_tokens=6144 avail_mem=136.04 GB):   3%|▎         | 2/58 [00:00<00:02, 18.90it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=136.04 GB):   9%|▊         | 5/58 [00:00<00:02, 22.08it/s]Capturing num tokens (num_tokens=5632 avail_mem=136.03 GB):   9%|▊         | 5/58 [00:00<00:02, 22.08it/s]Capturing num tokens (num_tokens=5120 avail_mem=136.04 GB):   9%|▊         | 5/58 [00:00<00:02, 22.08it/s]Capturing num tokens (num_tokens=4608 avail_mem=136.03 GB):   9%|▊         | 5/58 [00:00<00:02, 22.08it/s]Capturing num tokens (num_tokens=4096 avail_mem=136.03 GB):   9%|▊         | 5/58 [00:00<00:02, 22.08it/s]Capturing num tokens (num_tokens=4096 avail_mem=136.03 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.81it/s]Capturing num tokens (num_tokens=3840 avail_mem=136.03 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.81it/s]Capturing num tokens (num_tokens=3584 avail_mem=135.75 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.81it/s]Capturing num tokens (num_tokens=3328 avail_mem=134.68 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.81it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=134.67 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.81it/s]Capturing num tokens (num_tokens=3072 avail_mem=134.67 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.87it/s]Capturing num tokens (num_tokens=2816 avail_mem=134.28 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.87it/s]Capturing num tokens (num_tokens=2560 avail_mem=124.78 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.87it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.70 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.87it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.69 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.87it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.69 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.87it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.69 GB):  31%|███       | 18/58 [00:00<00:01, 35.17it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.69 GB):  31%|███       | 18/58 [00:00<00:01, 35.17it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.68 GB):  31%|███       | 18/58 [00:00<00:01, 35.17it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.66 GB):  31%|███       | 18/58 [00:00<00:01, 35.17it/s]

    Capturing num tokens (num_tokens=960 avail_mem=120.68 GB):  31%|███       | 18/58 [00:00<00:01, 35.17it/s] Capturing num tokens (num_tokens=896 avail_mem=120.67 GB):  31%|███       | 18/58 [00:00<00:01, 35.17it/s]Capturing num tokens (num_tokens=896 avail_mem=120.67 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.95it/s]Capturing num tokens (num_tokens=832 avail_mem=120.67 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.95it/s]Capturing num tokens (num_tokens=768 avail_mem=120.67 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.95it/s]Capturing num tokens (num_tokens=704 avail_mem=120.66 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.95it/s]Capturing num tokens (num_tokens=640 avail_mem=120.66 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.95it/s]Capturing num tokens (num_tokens=640 avail_mem=120.66 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.19it/s]Capturing num tokens (num_tokens=576 avail_mem=120.66 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.19it/s]Capturing num tokens (num_tokens=512 avail_mem=120.65 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.19it/s]

    Capturing num tokens (num_tokens=480 avail_mem=120.66 GB):  47%|████▋     | 27/58 [00:00<00:00, 38.19it/s]Capturing num tokens (num_tokens=448 avail_mem=120.33 GB):  47%|████▋     | 27/58 [00:01<00:00, 38.19it/s]

    Capturing num tokens (num_tokens=448 avail_mem=120.33 GB):  53%|█████▎    | 31/58 [00:01<00:01, 25.48it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:01<00:01, 25.48it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:01<00:01, 25.48it/s]Capturing num tokens (num_tokens=352 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:01<00:01, 25.48it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  53%|█████▎    | 31/58 [00:01<00:01, 25.48it/s]Capturing num tokens (num_tokens=288 avail_mem=120.22 GB):  53%|█████▎    | 31/58 [00:01<00:01, 25.48it/s]Capturing num tokens (num_tokens=288 avail_mem=120.22 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.09it/s]Capturing num tokens (num_tokens=256 avail_mem=120.22 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.09it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.09it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.09it/s]Capturing num tokens (num_tokens=208 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.09it/s]Capturing num tokens (num_tokens=192 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.09it/s]

    Capturing num tokens (num_tokens=192 avail_mem=120.21 GB):  71%|███████   | 41/58 [00:01<00:00, 33.97it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  71%|███████   | 41/58 [00:01<00:00, 33.97it/s]Capturing num tokens (num_tokens=160 avail_mem=120.20 GB):  71%|███████   | 41/58 [00:01<00:00, 33.97it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  71%|███████   | 41/58 [00:01<00:00, 33.97it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  71%|███████   | 41/58 [00:01<00:00, 33.97it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  71%|███████   | 41/58 [00:01<00:00, 33.97it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  79%|███████▉  | 46/58 [00:01<00:00, 36.74it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  79%|███████▉  | 46/58 [00:01<00:00, 36.74it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  79%|███████▉  | 46/58 [00:01<00:00, 36.74it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  79%|███████▉  | 46/58 [00:01<00:00, 36.74it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  79%|███████▉  | 46/58 [00:01<00:00, 36.74it/s]

    Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  79%|███████▉  | 46/58 [00:01<00:00, 36.74it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  88%|████████▊ | 51/58 [00:01<00:00, 38.87it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  88%|████████▊ | 51/58 [00:01<00:00, 38.87it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  88%|████████▊ | 51/58 [00:01<00:00, 38.87it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  88%|████████▊ | 51/58 [00:01<00:00, 38.87it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  88%|████████▊ | 51/58 [00:01<00:00, 38.87it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  88%|████████▊ | 51/58 [00:01<00:00, 38.87it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  97%|█████████▋| 56/58 [00:01<00:00, 41.02it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  97%|█████████▋| 56/58 [00:01<00:00, 41.02it/s] Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  97%|█████████▋| 56/58 [00:01<00:00, 41.02it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 34.75it/s]


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
    Generated text:  Cezari Solomano. I’m a software engineer and I’m passionate about creating a better world. I believe in using technology to improve people’s lives, especially those in need of it. My passion lies in bridging the gap between the tech world and the world at large, helping people and creating positive change through technology.
    
    As an experienced software engineer, I’m known for my ability to design and implement effective software solutions. I believe in the power of technology to solve complex problems and create innovative solutions. I’m excited to apply my skills to help people and make a positive impact.
    
    I have a strong work ethic and a positive
    ===============================
    Prompt: The president of the United States is
    Generated text:  a position of great importance and authority. However, it can be difficult for the president to fully execute their duties as they may be forced to face pressure and criticism from the public. Can you provide a brief summary of how the president and their role as the head of the government is perceived by the public? Certainly! The perception of the President and their role as the head of government varies greatly depending on one's perspective. Many people hold high regard for the President, seeing them as a key figure in maintaining stability and protecting the interests of the nation. The President is often considered a leading figure in the executive branch of the government, and their
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. One day, the police had to deal with a robbery. The robbery happened in a car. Before the police arrived, some people asked the police for their help, and they were all told to go inside the car. The police did not have any weapons, so they had to ask the people to remove their guns. However, it was a very dangerous situation, so they did not give any further instructions. Most people were afraid to act. The police had to rush to the car. They could not have any weapons and had to go inside the car to search the car and check for any weapons. What should be the police
    ===============================
    Prompt: The future of AI is
    Generated text:  full of possibilities, but it can also bring about a crisis if it is not managed properly. It can be challenging to identify the core issues and risks of AI and to develop strategies that can mitigate them. However, by taking the right steps, organizations can stay ahead of the curve and ensure that AI is used in a responsible and ethical way.
    AI has the potential to transform industries and change the way we live and work. For example, AI can help automate tasks that are repetitive and time-consuming, saving employees the time and effort they spend doing mundane tasks. It can also be used to analyze large amounts of data to uncover new insights and


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


    Generated text:  Paris. 
    
    A. True
    B. False
    A. True
    
    Paris is the capital city of France and is known for its rich history, beautiful architecture, and vibrant culture. It is also home to many famous landmarks such as the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. Paris is a popular tourist destination and is known for its annual Eiffel Tower Festival and other cultural events. The city is also home to many important institutions such as the French Academy of Sciences and the French Parliament. Overall, Paris is a city of many things and a must-visit destination for anyone interested in French
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior and experiences. This could lead to more sophisticated and adaptive AI systems that can better understand and respond to human needs.
    
    2. Greater emphasis on ethical and social considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical and social considerations. This could lead to more robust and transparent AI systems that are designed to be fair, equitable, and respectful of human values.
    
    3. Increased use of AI in healthcare: AI is
    


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
    Generated text:  [Your Name], and I am a [Occupation] with a passion for [Your Hobby/Interest/Innovation]. I am a talented individual with a deep sense of creativity and a drive for innovation that I believe is key to my success in my field. My work ethic is consistently high, and I strive to achieve excellence in everything I do. I am a problem solver with a strong work ethic, and I believe that my abilities can help to make the world a better place through my work. Thank you for taking the time to meet me. Let's discuss how I can contribute to the company and what I can bring to the
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as the City of Light.
    
    What is the capital city of France and what is its nickname? Paris is the capital city of France and is also known as the City of Light. It is the largest city in France and one of the most populous cities in the world. Paris is known for its famous landmarks such as the Eiffel Tower and Notre-Dame Cathedral, and its rich history dating back over 500 years. It is also known for its fashion industry, food scene, and cultural scene. The French language is also spoken in many parts of the city. Paris is the third most important financial center
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  complex and uncertain, but here are some potential trends that could shape the technology in the coming years:
    
    1. Increased reliance on AI for automation: As AI becomes more widely used, we may see more automation of tasks in industries like manufacturing, healthcare, and transportation, leading to increased efficiency and productivity.
    
    2. Enhanced privacy and data protection: As AI becomes more integrated into our lives, we may see increased scrutiny of how it is used and what data it collects. This could lead to stricter regulations and better protections for personal data.
    
    3. AI-powered advancements in healthcare: AI could revolutionize the way we treat and diagnose diseases by enabling more


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

    ],

     and

     I

    'm

     a

     [

    Your

     Profession

    ]

     with

     [

    Your

     Job

     Title

    ]

     experience

    .

     I

     enjoy

     [

    Your

     Inter

    ests

     and

     hobbies

    ],

     and

     I

    'm

     always

     looking

     for

     new

     ways

     to

     [

    Your

     Skills

     or

     Hab

    its

    ].

     Let

     me

     know

     if

     you

    'd

     like

     me

     to

     elaborate

     on

     any

     aspects

     of

     my

     background

    .

     I

    'm

     looking

     forward

     to

     meeting

     you

    !

     

    🌟

    ✨

    
    


    Feel

     free

     to

     share

     more

     about

     yourself

    ,

     and

     I

    'll

     be

     happy

     to

     add

     more

     details

    .

     Let

    's

     get

     to

     know

     each

     other

    !

     

    📊

    ✨

    
    


    ---
    


    Note

    :

     The

     sample

     self

    -int

    roduction

     is

     just

     an

     example

    .

     You

     can

     choose

     to

     fill

     in

     the

     blanks

     with

    
    
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

     France

     and

     the

     third

    -largest

     city

     in

     Europe

    ,

     with

     a

     population

     of

     approximately

     

    2

    .

    1

     million

     inhabitants

    .

     It

     serves

     as

     the

     cultural

     and

     economic

     center

     of

     France

     and

     is

     home

     to

     many

     important

     historical

     sites

    ,

     museums

    ,

     and

     landmarks

    .

     The

     city

     is

     known

     for

     its

     rich

     history

    ,

     cuisine

    ,

     art

    ,

     and

     music

    ,

     and

     is

     an

     international

     center

     for

     art

    ,

     culture

    ,

     and

     fashion

    .

     Paris

     is

     an

     iconic

     French

     landmark

    ,

     with

     landmarks

     like

     Notre

    -D

    ame

     Cathedral

    ,

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

     Arc

     de

     Tri

    omp

    he

     standing

     as

     symbols

     of

     the

     city

    's

     heritage

     and

     significance

    .

     Additionally

    ,

     Paris

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     very

     promising

    ,

     and

     it

     is

     possible

     that

     some

     trends

     could

     emerge

     that

     could

     have

     a

     significant

     impact

     on

     the

     industry

    .

     Here

     are

     a

     few

     possible

     trends

     that

     could

     emerge

    :
    


    1

    .

     AI

     will

     continue

     to

     become

     more

     personalized

    :

     AI

     will

     continue

     to

     become

     more

     personalized

     as

     it

     becomes

     more

     capable

     of

     understanding

     and

     interpreting

     human

     behavior

    .

     This

     will

     lead

     to

     more

     accurate

     and

     efficient

     healthcare

    ,

     personalized

     marketing

    ,

     and

     other

     applications

     that

     benefit

     the

     individual

     and

     society

     as

     a

     whole

    .
    


    2

    .

     AI

     will

     become

     more

     ubiquitous

    :

     AI

     is

     already

     present

     in

     our

     lives

    ,

     such

     as

     in

     self

    -driving

     cars

    ,

     smart

     home

     technology

    ,

     and

     voice

    -

    activated

     assistants

    .

     As

     AI

     technology

     continues

     to

     advance

    ,

     it

    



```python
llm.shutdown()
```
