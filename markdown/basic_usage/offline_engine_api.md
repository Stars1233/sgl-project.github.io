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


    2026-03-28 22:59:33.899 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 22:59:33] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 22:59:33.899 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 22:59:33] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 22:59:33.900 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 22:59:33] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 22:59:33.900 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 22:59:33] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 22:59:33.900 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 22:59:33] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.87it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.86it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:11,  2.31s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:11,  2.31s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:11,  2.31s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:11,  2.31s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:11,  2.31s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:19,  2.70it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:19,  2.70it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:19,  2.70it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:19,  2.70it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:19,  2.70it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:19,  2.70it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:19,  2.70it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:06,  7.02it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:06,  7.02it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:06,  7.02it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:06,  7.02it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:06,  7.02it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:06,  7.02it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:06,  7.02it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:06,  7.02it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:06,  7.02it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 14.02it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 14.02it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 14.02it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 14.02it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 14.02it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 14.02it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 14.02it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 14.02it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 20.51it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 20.51it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 20.51it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 20.51it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 20.51it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 20.51it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 20.51it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:02<00:01, 20.51it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:02<00:00, 27.37it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:02<00:00, 27.37it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:02<00:00, 27.37it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:02<00:00, 27.37it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:02<00:00, 27.37it/s]

    Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:02<00:00, 27.37it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:02<00:00, 27.37it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:02<00:00, 32.26it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:02<00:00, 32.26it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:02<00:00, 32.26it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 32.26it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 32.26it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 32.26it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 32.26it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 36.94it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 36.94it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 36.94it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 36.94it/s]

    Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 36.94it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 36.94it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 36.94it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 40.60it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 40.60it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 40.60it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 40.60it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 40.60it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 40.60it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 40.60it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 40.60it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.63it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.34 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.31 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.31 GB):   3%|▎         | 2/58 [00:00<00:03, 18.14it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.14it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.14it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.14it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.01it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.01it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.01it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.01it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 21.15it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 21.15it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 21.15it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  14%|█▍        | 8/58 [00:00<00:02, 21.15it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  19%|█▉        | 11/58 [00:00<00:01, 23.66it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  19%|█▉        | 11/58 [00:00<00:01, 23.66it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.27 GB):  19%|█▉        | 11/58 [00:00<00:01, 23.66it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.27 GB):  19%|█▉        | 11/58 [00:00<00:01, 23.66it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  19%|█▉        | 11/58 [00:00<00:01, 23.66it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  19%|█▉        | 11/58 [00:00<00:01, 23.66it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.31it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.26 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.31it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.31it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.31it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.31it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.31it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  36%|███▌      | 21/58 [00:00<00:01, 34.87it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  36%|███▌      | 21/58 [00:00<00:01, 34.87it/s] Capturing num tokens (num_tokens=896 avail_mem=120.24 GB):  36%|███▌      | 21/58 [00:00<00:01, 34.87it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  36%|███▌      | 21/58 [00:00<00:01, 34.87it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  36%|███▌      | 21/58 [00:00<00:01, 34.87it/s]Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  36%|███▌      | 21/58 [00:00<00:01, 34.87it/s]

    Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 37.97it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 37.97it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 37.97it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  45%|████▍     | 26/58 [00:00<00:00, 37.97it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 37.97it/s]Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 37.97it/s]Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:00<00:00, 40.08it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:00<00:00, 40.08it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:00<00:00, 40.08it/s]Capturing num tokens (num_tokens=352 avail_mem=120.22 GB):  53%|█████▎    | 31/58 [00:00<00:00, 40.08it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  53%|█████▎    | 31/58 [00:01<00:00, 40.08it/s]

    Capturing num tokens (num_tokens=288 avail_mem=120.21 GB):  53%|█████▎    | 31/58 [00:01<00:00, 40.08it/s]Capturing num tokens (num_tokens=288 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.39it/s]Capturing num tokens (num_tokens=256 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.39it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.39it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.39it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.39it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  62%|██████▏   | 36/58 [00:01<00:00, 41.39it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  71%|███████   | 41/58 [00:01<00:00, 42.51it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  71%|███████   | 41/58 [00:01<00:00, 42.51it/s]Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  71%|███████   | 41/58 [00:01<00:00, 42.51it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  71%|███████   | 41/58 [00:01<00:00, 42.51it/s]

    Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  71%|███████   | 41/58 [00:01<00:00, 42.51it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  71%|███████   | 41/58 [00:01<00:00, 42.51it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.11it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.11it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.11it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.11it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.11it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  79%|███████▉  | 46/58 [00:01<00:00, 43.11it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.12it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.12it/s]Capturing num tokens (num_tokens=24 avail_mem=120.20 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.12it/s]

    Capturing num tokens (num_tokens=20 avail_mem=120.20 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.12it/s]Capturing num tokens (num_tokens=16 avail_mem=120.20 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.12it/s]Capturing num tokens (num_tokens=12 avail_mem=120.19 GB):  88%|████████▊ | 51/58 [00:01<00:00, 43.12it/s]Capturing num tokens (num_tokens=12 avail_mem=120.19 GB):  97%|█████████▋| 56/58 [00:01<00:00, 43.78it/s]Capturing num tokens (num_tokens=8 avail_mem=118.97 GB):  97%|█████████▋| 56/58 [00:01<00:00, 43.78it/s] Capturing num tokens (num_tokens=4 avail_mem=118.87 GB):  97%|█████████▋| 56/58 [00:01<00:00, 43.78it/s]Capturing num tokens (num_tokens=4 avail_mem=118.87 GB): 100%|██████████| 58/58 [00:01<00:00, 37.42it/s]


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
    Generated text:  Mary. I am a student of Senior 3 at Richard Cox High School. I'm in the science club and I'm going to the Science Olympiad. My favorite subject is chemistry, and I'm taking a class this semester. In chemistry, I am really excited to study acid-base reaction. I want to learn how to make salt and other chemicals. I also really like biology. I want to study the part of the body called heart because I want to understand it. My favorite sport is basketball. I think I can improve my skills in basketball very much. I am good at playing ping pong. I like to keep me busy
    ===============================
    Prompt: The president of the United States is
    Generated text:  25 years younger than the president of California. The president of California is 8 times older than the president of Hawaii. If the president of Hawaii is 30 years old, how old is the president of the United States?
    To determine the age of the president of the United States, we need to follow these steps:
    
    1. Identify the age of the president of California.
    2. Use the relationship between the president of California and the president of Hawaii to find the president of Hawaii.
    3. Use the relationship between the president of Hawaii and the president of the United States to find the president of the United States.
    
    Let's
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. Among the following cities, which one is the largest city in France? A. Lyon B. Lyon C. Lyon D. Lyon
    Answer:
    
    D
    
    Which of the following statements about a certain type of bacteria is true?
    A. It is an unicellular organism
    B. It can only survive in aquatic environments
    C. It has a cell wall
    D. It has a cell membrane
    Answer:
    
    A
    
    Which of the following statements about the internal environment of the human body is true?
    A. The primary components of plasma and interstitial fluid are both proteins.
    B. The internal environment of the human body is composed
    ===============================
    Prompt: The future of AI is
    Generated text:  still uncertain, and the world is in need of a new paradigm in the way we interact with the digital ecosystem. In this world, AI will be a key factor in creating a new way of life for humans. The future of AI will be a world where it will help us, empower us, and empower all. Let us see how we can utilize AI in the future.
    The future of AI is uncertain, and the world is in need of a new paradigm in the way we interact with the digital ecosystem. In this world, AI will be a key factor in creating a new way of life for humans. The future of AI will


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


    Generated text:  [Name] and I'm a [occupation] with [number] years of experience in [field]. I'm a [character trait] and I'm always [character trait]. I'm [character trait] and I'm always [character trait]. I'm [character trait] and I'm always [character trait]. I'm [character trait] and I'm always [character trait]. I'm [character trait] and I'm always [character trait]. I'm [character trait] and I'm always [character trait]. I'm [character trait] and I'm always [character trait]. I'm [character trait] and I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic Eiffel Tower and the annual Eiffel Tower Festival. It is the largest city in France and the second-largest city in the European Union. Paris is also the birthplace of many famous French artists, writers, and composers. The city is home to many museums, theaters, and landmarks, including the Louvre Museum, the Notre-Dame Cathedral, and the Champs-Élysées. Paris is a vibrant and diverse city with a rich history and culture, making it a popular tourist destination. The city is also known for its cuisine, with many famous French dishes and restaurants serving up
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in areas such as machine learning, natural language processing, and computer vision. Here are some possible future trends in AI:
    
    1. Increased focus on ethical considerations: As AI becomes more integrated into our daily lives, there will be a growing emphasis on ethical considerations. This will include issues such as bias, transparency, and accountability.
    
    2. Greater integration with human intelligence: AI will continue to become more integrated with human intelligence, allowing for more complex and nuanced decision-making.
    
    3. Increased use of AI in healthcare: AI will be used to improve the accuracy and efficiency of medical diagnosis and treatment, leading to better
    


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
    Generated text:  [Your Name]. I'm a computer science student at [University Name], majoring in [major]. I have a passion for [specific interest or hobby]. I enjoy [specific extracurricular activities or social groups]. I have a strong work ethic and am always seeking to learn new things. I'm passionate about [specific area of interest or contribution to society]. I'm always looking for new opportunities to grow and be more successful in my field. What are some ways in which you can expand your network and opportunities for growth in your field of study?
    I'm always looking for new opportunities to grow and be more successful in my field
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known for its iconic architecture, rich history, and vibrant culture.
    
    Paris is the capital city of France and is known for its stunning architecture, rich history, and vibrant culture. Its iconic landmarks include the Eiffel Tower, the Louvre Museum, and Notre-Dame Cathedral. Paris is a city that has a unique blend of traditional French culture and modernity, attracting millions of visitors each year. The city is also home to various international organizations and institutions, such as UNESCO, the European Commission, and the European Parliament. Overall, Paris is a beloved city that has made a significant contribution to French culture and history.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  promising and there are several key trends that suggest it will continue to evolve and transform our world in ways we can't even imagine yet. Here are some of the most likely trends that we can expect to see in the future:
    
    1. Increased AI Personalization: AI will become more personalized, providing increasingly accurate and relevant information to users. This will result in more effective decision-making and better customer experiences.
    
    2. Ethical and Responsible AI: As AI technology becomes more prevalent, there will be a growing emphasis on ethical and responsible AI development. This will involve creating AI systems that are transparent, accountable, and responsible for their actions.
    
    3.


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

    ]

     and

     I

    'm

     a

     software

     engineer

     with

     over

     

    1

    0

     years

     of

     experience

     in

     the

     field

    .

     I

    've

     spent

     my

     time

     working

     on

     cutting

    -edge

     projects

     that

     push

     the

     boundaries

     of

     what

    's

     possible

     in

     the

     technology

     industry

    .

     I

    'm

     passionate

     about

     pushing

     the

     limits

     of

     technology

     and

     creating

     innovative

     solutions

     that

     benefit

     society

    .

     I

    'm

     always

     eager

     to

     learn

     and

     grow

    ,

     and

     I

    'm

     a

     proactive

     problem

     solver

    .

     I

     believe

     in

     making

     a

     positive

     impact

     on

     the

     world

     through

     my

     work

    ,

     and

     I

    'm

     always

     eager

     to

     contribute

     to

     the

     tech

     industry

     in

     some

     way

    .

     Thank

     you

     for

     taking

     the

     time

     to

     meet

     me

    .

     It

    's

     nice

     to

     meet

     someone

     like

     you

    .

     Congratulations

     on

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    The

     statement

     is

    :

     "

    Paris

     is

     the

     capital

     of

     France

    ."

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     be

     a

     rapidly

     evolving

     landscape

     with

     numerous

     opportunities

     and

     challenges

    .

     Here

     are

     some

     possible

     future

     trends

     in

     AI

    :
    


    1

    .

     Adv

    ancements

     in

     machine

     learning

    :

     With

     the

     continued

     growth

     of

     AI

    ,

     there

     is

     likely

     to

     be

     a

     significant

     increase

     in

     the

     number

     of

     machine

     learning

     algorithms

     and

     techniques

     being

     developed

    .

     This

     will

     lead

     to

     more

     sophisticated

     models

     that

     can

     perform

     tasks

     more

     accurately

     and

     efficiently

    .
    


    2

    .

     Increased

     focus

     on

     ethics

     and

     safety

    :

     With

     the

     potential

     for

     AI

     to

     be

     used

     in

     dangerous

     or

     unethical

     ways

    ,

     there

     will

     be

     a

     greater

     focus

     on

     ethical

     considerations

     and

     the

     potential

     for

     AI

     to

     be

     used

     in

     a

     responsible

     and

     beneficial

     manner

    .
    


    3

    .

     Integration

     with

     natural

     language

     processing

    :

    



```python
llm.shutdown()
```
