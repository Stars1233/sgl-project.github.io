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


    2026-04-02 01:44:39.304 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 01:44:39] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 01:44:39.304 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 01:44:39] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 01:44:39.304 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 01:44:39] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 01:44:39.304 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 01:44:39] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 01:44:39.305 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 01:44:39] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.73it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.72it/s]


    2026-04-02 01:44:42,774 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 01:44:42] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:35,  2.73s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:35,  2.73s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:35,  2.73s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:35,  2.73s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:35,  2.73s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:22,  2.31it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.05it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.05it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.05it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.05it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:07,  6.05it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:07,  6.05it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:07,  6.05it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:07,  6.05it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  6.05it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.27it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.27it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.27it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.27it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.27it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.27it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.27it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.27it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.23it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.23it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.23it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.23it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.23it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.23it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.23it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.23it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:01, 24.50it/s]

    Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 28.98it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 28.98it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 28.98it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 28.98it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 28.98it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 28.98it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 28.98it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 33.80it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 33.80it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 33.80it/s] 

    Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 33.80it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 33.80it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 33.80it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 33.80it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 37.67it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 37.67it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 37.67it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 37.67it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 37.67it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 37.67it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 37.67it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 37.67it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.51it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.34 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.31 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.31 GB):   3%|▎         | 2/58 [00:00<00:03, 17.52it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.52it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.52it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.52it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.16it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.16it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.16it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.16it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.61it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.61it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.61it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.61it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.61it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  21%|██        | 12/58 [00:00<00:01, 28.53it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 28.53it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 28.53it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 28.53it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 28.53it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.18it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.26 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.18it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.18it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.18it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.18it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.18it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.68it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.68it/s] Capturing num tokens (num_tokens=896 avail_mem=120.24 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.68it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.68it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.68it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.79it/s]Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.79it/s]

    Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.79it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.79it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.79it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:00<00:00, 35.79it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:00<00:00, 37.42it/s]Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:00<00:00, 37.42it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:00<00:00, 37.42it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:00<00:00, 37.42it/s]Capturing num tokens (num_tokens=352 avail_mem=120.22 GB):  52%|█████▏    | 30/58 [00:00<00:00, 37.42it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  52%|█████▏    | 30/58 [00:01<00:00, 37.42it/s]

    Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  60%|██████    | 35/58 [00:01<00:00, 38.62it/s]Capturing num tokens (num_tokens=288 avail_mem=120.21 GB):  60%|██████    | 35/58 [00:01<00:00, 38.62it/s]Capturing num tokens (num_tokens=256 avail_mem=120.21 GB):  60%|██████    | 35/58 [00:01<00:00, 38.62it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  60%|██████    | 35/58 [00:01<00:00, 38.62it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  60%|██████    | 35/58 [00:01<00:00, 38.62it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  60%|██████    | 35/58 [00:01<00:00, 38.62it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 39.75it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 39.75it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 39.75it/s]Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 39.75it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 39.75it/s]

    Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 39.75it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.69it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.69it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.69it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.69it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.69it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.69it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  86%|████████▌ | 50/58 [00:01<00:00, 39.76it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  86%|████████▌ | 50/58 [00:01<00:00, 39.76it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 39.76it/s]

    Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 39.76it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 39.76it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 39.76it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  95%|█████████▍| 55/58 [00:01<00:00, 40.42it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  95%|█████████▍| 55/58 [00:01<00:00, 40.42it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  95%|█████████▍| 55/58 [00:01<00:00, 40.42it/s] Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  95%|█████████▍| 55/58 [00:01<00:00, 40.42it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 36.37it/s]


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
    Generated text:  Colin and I am the Director of Programmes and Development for the Centre for Innovation, Creativity and Learning. I have held the role since 2009 and have a strong track record of developing new partnerships and initiatives to address the needs of people with disabilities in the broader community.
    I have a background in both research and education and have previously worked at the University of Newcastle on topics related to digital learning, literacy and online learning.
    I have a passion for the principles and the challenges of accessible technology and have a particular interest in the ways that technology can support people with disabilities to develop their digital literacies. I would like to use
    ===============================
    Prompt: The president of the United States is
    Generated text:  5 feet tall. If the president is standing with his arms outstretched, what is the height of the president if his arms reach to a height of 10 feet from the ground? Express your answer in feet.
    
    To determine the height of the president's arms when they reach to a height of 10 feet from the ground, we need to use the relationship between the president's height and the height of his arms. Specifically, the height of the president's arms is the difference between the president's total height and the height of the top of his head.
    
    1. Identify the president's height: 5 feet.
    
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, the symbol of France. How can you break the link between the capital and the city’s landmarks and the traditional French way of life?
    The answer is the Eurogates: a mysterious underground railway system that connects the Paris metro stations to the Sainte-Chapelle and the Notre-Dame Cathedral.
    The station, which opened in 2011, is the only underground station to be opened by the government, the first one of its kind in the world, and is a tourist attraction in its own right, with visitors able to explore the complex, including the crypt for the missing Louis XVI.
    The system has the advantage
    ===============================
    Prompt: The future of AI is
    Generated text:  uncertain. The technology is rapidly evolving at a rate unseen in decades, and it is difficult to predict how it will evolve further. As such, it is necessary to understand the current state of AI and the steps to take to ensure a safe and secure future for all individuals. AI is a complex and constantly evolving technology, and it is important to have a comprehensive understanding of its potential impacts on individuals, society, and the environment.
    To understand the current state of AI, it is important to examine the technologies involved and the key applications of AI in different industries. This can include things like self-driving cars, predictive analytics, speech recognition, machine


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a brief description of your job or profession]. I enjoy [insert a brief description of your hobbies or interests]. What's your favorite hobby or activity? I love [insert a hobby or activity you enjoy]. What's your favorite book or movie? I love [insert a book or movie you enjoy]. What's your favorite place to go? I love [insert a place you love]. What's your favorite color? I love [insert
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, which is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and Louvre Museum. It is also famous for its rich history, including the French Revolution and the French Revolution Monument. Paris is a bustling city with a diverse population and is home to many famous artists, writers, and musicians. It is also a popular tourist destination, with many visitors coming to see its beautiful architecture, museums, and food. The city is known for its cuisine, including its famous croissants and its famous French fries. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly into
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs. As AI technology continues to improve, we can expect to see even more widespread use of AI in healthcare, particularly in areas such as diagnosis, treatment planning, and patient care.
    
    2. Increased use of AI in finance: AI is already being used in finance to improve risk management and fraud detection. As AI technology continues
    


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
    Generated text:  [Name] and I'm a [Career/Background] at [Company]. I'm passionate about [interests/activities], and I'm always looking for new challenges and opportunities to grow in my career. I'm a [Personality Traits] and I'm always eager to learn, grow, and create positive impact. I'm always ready to collaborate with others and take on new responsibilities. I'm a [Managerial Skills] and I'm very organized and have a good sense of time management. I'm a [Adaptable] and I'm open to new experiences and ideas. I'm a [Team Player] and I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as the City of Light, a historic and cosmopolitan city that is renowned for its rich history, beautiful architecture, and lively cultural scene. It has been a major hub of trade, industry, and culture for centuries and is considered one of the world’s most beautiful cities. Paris is home to the Eiffel Tower, the Louvre Museum, the Notre-Dame Cathedral, and many other iconic landmarks that make it a truly unforgettable destination. Its vibrant nightlife, delicious food, and friendly locals make it a popular tourist destination. Paris is a city of contrasts and beauty, with its iconic landmarks and charming streets stretching out
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to involve a range of exciting developments and changes, depending on the level of development that occurs. Here are some possible trends in AI that could occur in the coming years:
    
    1. Advancements in machine learning: As AI technologies continue to advance, we can expect to see more sophisticated algorithms and models that can recognize patterns and make predictions based on data. This could lead to breakthroughs in areas such as autonomous driving, intelligent healthcare, and personalized medicine.
    
    2. Integration of AI into other industries: AI is already being used in a wide range of industries, from finance and retail to healthcare and transportation. As AI becomes more integrated into existing


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

     an

     enthusiastic

     and

     well

    -rounded

     person

     who

     thr

    ives

     on

     learning

     new

     things

    .

     I

    'm

     passionate

     about

     technology

    ,

     education

    ,

     and

     sustainability

    ,

     and

     I

    'm

     always

     eager

     to

     share

     my

     knowledge

     and

     engage

     with

     others

     who

     share

     the

     same

     interests

    .

     Whether

     it

    's

     learning

     about

     coding

    ,

     digital

     marketing

    ,

     or

     environmental

     science

    ,

     I

    'm

     always

     on

     the

     lookout

     for

     new

     opportunities

     to

     expand

     my

     hor

    izons

    .

     I

    'm

     also

     a

     passionate

     environmental

     activist

     who

    's

     always

     committed

     to

     making

     a

     positive

     impact

     on

     the

     world

    .

     I

    'm

     not

     afraid

     to

     take

     risks

     and

     take

     chances

    ,

     and

     I

    'm

     always

     open

     to

     new

     experiences

    .

     I

     believe

     in

     the

     power

     of

     collaboration

     and

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     the

     city

     that

     hosts

     the

     E

    iff

    el

     Tower

     and

     the

     Lou

    vre

     Museum

    .

     It

     has

     a

     rich

     history

    ,

     notable

     landmarks

    ,

     and

     is

     a

     popular

     tourist

     destination

    .

     The

     city

     is

     known

     for

     its

     fine

     cuisine

    ,

     charming

     architecture

    ,

     and

     beautiful

     parks

    .

     It

     is

     also

     home

     to

     many

     international

     organizations

     and

     cultural

     institutions

    ,

     including

     the

     Paris

     Opera

    ,

     the

     Mus

    ée

     d

    ’

    Or

    say

    ,

     and

     the

     Muse

    e

     Rod

    in

    .

     The

     French

     language

     is

     also

     spoken

     in

     Paris

    ,

     and

     the

     city

     is

     known

     for

     its

     diverse

     population

     and

     cultural

     contributions

     to

     the

     world

    .

     Paris

     is

     a

     vibrant

     and

     dynamic

     city

     with

     a

     rich

     history

     and

     culture

    ,

     making

     it

     a

     must

    -

    visit

     destination

     for

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     promising

     and

     exciting

    ,

     with

     a

     number

     of

     trends

     that

     are

     shaping

     the

     technology

    's

     path

     forward

    .

     Here

     are

     some

     of

     the

     possible

     future

     trends

     in

     AI

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

     As

     AI

     becomes

     more

     advanced

    ,

     there

     is

     a

     growing

     trend

     towards

     developing

     more

     accurate

     and

     personalized

     healthcare

     solutions

    .

     AI

     can

     help

     doctors

     and

     other

     healthcare

     professionals

     analyze

     large

     amounts

     of

     data

     to

     diagnose

     diseases

    ,

     develop

     personalized

     treatment

     plans

    ,

     and

     monitor

     patient

     health

    .
    


    2

    .

     Enhanced

     cognitive

     capabilities

    :

     AI

     is

     expected

     to

     continue

     to

     improve

     its

     cognitive

     abilities

    ,

     with

     the

     potential

     to

     create

     more

     intelligent

     and

     adaptive

     machines

     that

     can

     learn

     and

     adapt

     on

     their

     own

    .

     This

     could

     lead

     to

     new

     forms

     of

    



```python
llm.shutdown()
```
