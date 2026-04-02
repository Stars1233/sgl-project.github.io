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


    2026-04-02 07:41:54.734 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 07:41:54] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 07:41:54.734 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 07:41:54] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 07:41:54.734 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 07:41:54] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 07:41:54.734 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 07:41:54] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 07:41:54.734 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 07:41:54] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.34it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.33it/s]


    2026-04-02 07:41:59,036 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 07:41:59] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:37,  2.76s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:37,  2.76s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:37,  2.76s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:37,  2.76s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:29,  1.82it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.60it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.60it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.60it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:03<00:08,  5.60it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:03<00:08,  5.60it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:03<00:08,  5.60it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:03<00:08,  5.60it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:03<00:08,  5.60it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:08,  5.60it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 11.87it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 11.87it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 11.87it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 11.87it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 11.87it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 11.87it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 11.87it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 11.87it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 17.89it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 17.89it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 17.89it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 17.89it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 17.89it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 17.89it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 17.89it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 17.89it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.50it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.50it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.47it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.47it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.47it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.47it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.47it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.47it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.47it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 34.82it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 34.82it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 34.82it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 34.82it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 34.82it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 34.82it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 34.82it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.09it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.09it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.09it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.09it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.09it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.09it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.09it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.09it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.09it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 47.06it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.50it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.44 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.42 GB):   3%|▎         | 2/58 [00:00<00:03, 18.24it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.41 GB):   3%|▎         | 2/58 [00:00<00:03, 18.24it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.41 GB):   3%|▎         | 2/58 [00:00<00:03, 18.24it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.41 GB):   3%|▎         | 2/58 [00:00<00:03, 18.24it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=118.41 GB):   9%|▊         | 5/58 [00:00<00:02, 21.24it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.41 GB):   9%|▊         | 5/58 [00:00<00:02, 21.24it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.41 GB):   9%|▊         | 5/58 [00:00<00:02, 21.24it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.40 GB):   9%|▊         | 5/58 [00:00<00:02, 21.24it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.40 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.81it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.40 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.81it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.40 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.81it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.39 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.81it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.39 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.81it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=118.39 GB):  21%|██        | 12/58 [00:00<00:01, 29.60it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.39 GB):  21%|██        | 12/58 [00:00<00:01, 29.60it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.39 GB):  21%|██        | 12/58 [00:00<00:01, 29.60it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.38 GB):  21%|██        | 12/58 [00:00<00:01, 29.60it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.38 GB):  21%|██        | 12/58 [00:00<00:01, 29.60it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.38 GB):  28%|██▊       | 16/58 [00:00<00:01, 33.10it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.38 GB):  28%|██▊       | 16/58 [00:00<00:01, 33.10it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.37 GB):  28%|██▊       | 16/58 [00:00<00:01, 33.10it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.37 GB):  28%|██▊       | 16/58 [00:00<00:01, 33.10it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.36 GB):  28%|██▊       | 16/58 [00:00<00:01, 33.10it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.35 GB):  28%|██▊       | 16/58 [00:00<00:01, 33.10it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=118.35 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.56it/s]Capturing num tokens (num_tokens=960 avail_mem=118.36 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.56it/s] Capturing num tokens (num_tokens=896 avail_mem=118.36 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.56it/s]Capturing num tokens (num_tokens=832 avail_mem=118.35 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.56it/s]Capturing num tokens (num_tokens=768 avail_mem=118.35 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.56it/s]Capturing num tokens (num_tokens=768 avail_mem=118.35 GB):  43%|████▎     | 25/58 [00:00<00:01, 30.52it/s]Capturing num tokens (num_tokens=704 avail_mem=118.35 GB):  43%|████▎     | 25/58 [00:00<00:01, 30.52it/s]Capturing num tokens (num_tokens=640 avail_mem=118.34 GB):  43%|████▎     | 25/58 [00:00<00:01, 30.52it/s]

    Capturing num tokens (num_tokens=576 avail_mem=118.34 GB):  43%|████▎     | 25/58 [00:00<00:01, 30.52it/s]Capturing num tokens (num_tokens=512 avail_mem=118.33 GB):  43%|████▎     | 25/58 [00:00<00:01, 30.52it/s]Capturing num tokens (num_tokens=512 avail_mem=118.33 GB):  50%|█████     | 29/58 [00:00<00:00, 31.17it/s]Capturing num tokens (num_tokens=480 avail_mem=118.35 GB):  50%|█████     | 29/58 [00:00<00:00, 31.17it/s]Capturing num tokens (num_tokens=448 avail_mem=118.35 GB):  50%|█████     | 29/58 [00:00<00:00, 31.17it/s]Capturing num tokens (num_tokens=416 avail_mem=118.34 GB):  50%|█████     | 29/58 [00:01<00:00, 31.17it/s]Capturing num tokens (num_tokens=384 avail_mem=118.34 GB):  50%|█████     | 29/58 [00:01<00:00, 31.17it/s]Capturing num tokens (num_tokens=384 avail_mem=118.34 GB):  57%|█████▋    | 33/58 [00:01<00:00, 32.34it/s]Capturing num tokens (num_tokens=352 avail_mem=118.34 GB):  57%|█████▋    | 33/58 [00:01<00:00, 32.34it/s]

    Capturing num tokens (num_tokens=320 avail_mem=118.33 GB):  57%|█████▋    | 33/58 [00:01<00:00, 32.34it/s]Capturing num tokens (num_tokens=288 avail_mem=118.33 GB):  57%|█████▋    | 33/58 [00:01<00:00, 32.34it/s]Capturing num tokens (num_tokens=256 avail_mem=118.33 GB):  57%|█████▋    | 33/58 [00:01<00:00, 32.34it/s]Capturing num tokens (num_tokens=256 avail_mem=118.33 GB):  64%|██████▍   | 37/58 [00:01<00:00, 33.07it/s]Capturing num tokens (num_tokens=240 avail_mem=118.32 GB):  64%|██████▍   | 37/58 [00:01<00:00, 33.07it/s]Capturing num tokens (num_tokens=224 avail_mem=118.32 GB):  64%|██████▍   | 37/58 [00:01<00:00, 33.07it/s]Capturing num tokens (num_tokens=208 avail_mem=118.32 GB):  64%|██████▍   | 37/58 [00:01<00:00, 33.07it/s]Capturing num tokens (num_tokens=192 avail_mem=118.32 GB):  64%|██████▍   | 37/58 [00:01<00:00, 33.07it/s]Capturing num tokens (num_tokens=192 avail_mem=118.32 GB):  71%|███████   | 41/58 [00:01<00:00, 33.95it/s]Capturing num tokens (num_tokens=176 avail_mem=118.31 GB):  71%|███████   | 41/58 [00:01<00:00, 33.95it/s]

    Capturing num tokens (num_tokens=160 avail_mem=118.31 GB):  71%|███████   | 41/58 [00:01<00:00, 33.95it/s]Capturing num tokens (num_tokens=144 avail_mem=118.30 GB):  71%|███████   | 41/58 [00:01<00:00, 33.95it/s]Capturing num tokens (num_tokens=128 avail_mem=118.30 GB):  71%|███████   | 41/58 [00:01<00:00, 33.95it/s]Capturing num tokens (num_tokens=128 avail_mem=118.30 GB):  78%|███████▊  | 45/58 [00:01<00:00, 34.35it/s]Capturing num tokens (num_tokens=112 avail_mem=118.30 GB):  78%|███████▊  | 45/58 [00:01<00:00, 34.35it/s]Capturing num tokens (num_tokens=96 avail_mem=118.29 GB):  78%|███████▊  | 45/58 [00:01<00:00, 34.35it/s] Capturing num tokens (num_tokens=80 avail_mem=118.29 GB):  78%|███████▊  | 45/58 [00:01<00:00, 34.35it/s]Capturing num tokens (num_tokens=64 avail_mem=118.29 GB):  78%|███████▊  | 45/58 [00:01<00:00, 34.35it/s]

    Capturing num tokens (num_tokens=64 avail_mem=118.29 GB):  84%|████████▍ | 49/58 [00:01<00:00, 34.42it/s]Capturing num tokens (num_tokens=48 avail_mem=118.29 GB):  84%|████████▍ | 49/58 [00:01<00:00, 34.42it/s]Capturing num tokens (num_tokens=32 avail_mem=118.28 GB):  84%|████████▍ | 49/58 [00:01<00:00, 34.42it/s]Capturing num tokens (num_tokens=28 avail_mem=118.28 GB):  84%|████████▍ | 49/58 [00:01<00:00, 34.42it/s]Capturing num tokens (num_tokens=24 avail_mem=118.27 GB):  84%|████████▍ | 49/58 [00:01<00:00, 34.42it/s]Capturing num tokens (num_tokens=24 avail_mem=118.27 GB):  91%|█████████▏| 53/58 [00:01<00:00, 35.21it/s]Capturing num tokens (num_tokens=20 avail_mem=118.27 GB):  91%|█████████▏| 53/58 [00:01<00:00, 35.21it/s]Capturing num tokens (num_tokens=16 avail_mem=118.27 GB):  91%|█████████▏| 53/58 [00:01<00:00, 35.21it/s]Capturing num tokens (num_tokens=12 avail_mem=118.27 GB):  91%|█████████▏| 53/58 [00:01<00:00, 35.21it/s]Capturing num tokens (num_tokens=8 avail_mem=118.26 GB):  91%|█████████▏| 53/58 [00:01<00:00, 35.21it/s] Capturing num tokens (num_tokens=4 avail_mem=118.26 GB):  91%|█████████▏| 53/58 [00:01<00:00, 35.21it/s]

    Capturing num tokens (num_tokens=4 avail_mem=118.26 GB): 100%|██████████| 58/58 [00:01<00:00, 37.92it/s]Capturing num tokens (num_tokens=4 avail_mem=118.26 GB): 100%|██████████| 58/58 [00:01<00:00, 33.05it/s]


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
    Generated text:  Jenna and I am currently a fourth year Ph.D. student at the University of Toronto, specializing in life sciences and neuroscience. I am passionate about neurodiversity and how science can help people with neurodivergent conditions understand and interact with the world around them.
    
    As a participant in a community where diversity and inclusion are fundamental values, I am continually learning to be more inclusive and to practice social justice. My goal is to encourage a more inclusive and accepting environment for neurodivergent individuals in the neuroscience community.
    
    I am also deeply committed to teaching and learning new skills and knowledge. I have a strong interest in using my research to
    ===============================
    Prompt: The president of the United States is
    Generated text:  now trying to secure a rare female nominee for the post of Secretary of State. Which of the following statements is the most likely to be true according to the passage?
    
    A: The candidate is highly intelligent, clever, and possesses a strong work ethic.
    
    B: The candidate is highly intelligent, but is not a consummate politician or a good public speaker.
    
    C: The candidate has a strong work ethic but is not a consummate politician or a good public speaker.
    
    D: The candidate possesses a strong work ethic and is a consummate politician or a good public speaker. To determine the most likely statement about the candidate based on the passage,
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. France is a country in Europe. So, Paris is a place. It is a city. But, Paris is also a country. Paris is a capital of France. Paris is the largest city in France. It is the biggest city in Europe. Paris is famous for many things. It is famous for the Eiffel Tower. It is famous for the Louvre Museum. It is famous for the Notre Dame Cathedral. It is famous for the Champs-Élysées. It is famous for the Louvre Museum. It is famous for the Notre Dame Cathedral. It is famous for the Eiffel Tower. It
    ===============================
    Prompt: The future of AI is
    Generated text:  an exciting area of research and development, with the potential to revolutionize many fields including healthcare, transportation, and education. AI is a complex field with many subfields, including machine learning, deep learning, natural language processing, and computer vision. Each of these fields has its own unique challenges and opportunities, and finding the right balance between them is crucial for successful AI development.
    Machine learning is a core subfield of AI that focuses on creating algorithms that can learn from data and make predictions or decisions. The goal of machine learning is to build models that can generalize from the training data to make accurate predictions or decisions without being explicitly programmed. Machine


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
    
    The statement is concise and accurately describes the capital city of France. It provides the name of the city and its capital, which are both key facts about the city. The statement is clear and easy to understand, making it suitable for use in various contexts. Additionally, it avoids any potentially sensitive or controversial topics, such as immigration or political issues, which could be considered sensitive in some contexts. Overall, the statement is a straightforward and informative representation of the capital city of France. 
    
    The answer is: Paris is the capital of France. 
    
    This answer is concise, factual, and accurately describes the capital city of France. It
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: As AI becomes more advanced, it is likely to become more integrated with human intelligence, allowing for more complex and nuanced interactions between humans and machines.
    
    2. Greater emphasis on ethical considerations: As AI becomes more advanced, there will be a greater emphasis on ethical considerations, including issues such as bias, privacy, and accountability.
    
    3. Increased use of AI in healthcare: AI is already being used in healthcare to improve diagnosis, treatment, and patient care. As AI becomes more advanced, it is likely to be used in even more advanced ways, including personalized medicine
    


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
    Generated text:  [Name] and I am a [job title] at [Company/Agency]. My [specific role or specialty] with [Company/Agency name] is responsible for [specific task or accomplishment]. I am [age], [gender], and have [number of years] years of experience in [field of expertise]. I am [general attitude] and always strive to [specific goal or objective]. I am [charisma] and am known for [unique selling point or ability]. I am [personality type] and I am [future aspirations]. Thank you for considering my application.
    Yourself. I'm [Your Name] and
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    Paris is the largest city in France and the 15th largest city in the world. It is located in the center of the country, on the Seine River, and is home to the headquarters of the French government, many of the country's wealthiest individuals and prominent companies, and many other notable landmarks and attractions. It is also the cultural and commercial center of France and is known for its French cuisine, couture, and film industry. Paris has a rich and diverse history dating back to the Roman Empire and remains a major cultural and economic center today. The city is also known for its beautiful architecture, including the E
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be shaped by a wide range of trends and advancements that will shape the way AI is developed, deployed, and used in the future. Some possible future trends in AI include:
    
    1. Increased depth and breadth of knowledge: AI systems are likely to become more capable of understanding and processing information in a deeper and broader way, which could lead to improved accuracy in areas such as natural language processing, medical diagnosis, and security.
    
    2. Improved accuracy and reliability: As AI systems become more sophisticated, they are likely to become more accurate and reliable, with better performance in a wider range of tasks.
    
    3. Enhanced collaboration and communication: AI


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

    'm

     

    2

    6

     years

     old

     and

     I

     currently

     work

     as

     a

     [

    Your

     profession

    /

    role

    ]

     with

     over

     [

    number

    ]

     years

     of

     experience

     in

     the

     industry

    .

     I

    'm

     a

     team

     player

     who

     thr

    ives

     in

     a

     collaborative

     environment

     and

     am

     always

     looking

     for

     new

     opportunities

     to

     grow

     and

     develop

    .

     I

    'm

     also

     a

     loyal

     and

     respectful

     colleague

     who

     is

     always

     willing

     to

     help

     others

     and

     take

     initiative

     to

     get

     the

     job

     done

    .

     I

     am

     passionate

     about

     learning

     and

     always

     aim

     to

     stay

     up

    -to

    -date

     with

     the

     latest

     trends

     and

     technologies

     in

     the

     field

    .

     I

    'm

     confident

     in

     my

     ability

     to

     accomplish

     any

     task

     I

     set

     my

     mind

     to

    ,

     and

     I

    'm

     always

     looking

     to

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     which

     is

     known

     for

     its

     rich

     history

    ,

     beautiful

     architecture

    ,

     and

     annual

     cultural

     events

     such

     as

     the

     world

    -ren

    owned

     E

    iff

    el

     Tower

    .

     It

     was

     founded

     by

     the

     Romans

     and

     is

     home

     to

     the

     ancient

     E

    iff

    el

     Tower

    ,

     which

     is

     considered

     one

     of

     the

     most

     recognizable

     symbols

     of

     Paris

    .

     The

     city

     has

     a

     diverse

     population

     of

     around

     

    2

    .

    3

     million

     residents

    ,

     and

     it

     is

     a

     popular

     tourist

     destination

    .

     Paris

     is

     also

     a

     hub

     for

     music

    ,

     fashion

    ,

     and

     film

    ,

     attracting

     artists

    ,

     musicians

    ,

     and

     filmmakers

     from

     around

     the

     world

    .

     Its

     long

     history

     and

     cultural

     richness

     have

     made

     it

     a

     popular

     destination

     for

     many

     years

    .

     

     #

    Phil

    osoph

    y

     #

    Paris

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     unpredictable

     and

     can

     be

     influenced

     by

     a

     multitude

     of

     factors

    .

     However

    ,

     here

     are

     some

     potential

     trends

     that

     are

     likely

     to

     be

     important

    :
    


    1

    .

     Automation

     and

     AI

     will

     become

     more

     pervasive

    .

     As

     automation

     and

     AI

     become

     more

     common

    ,

     they

     will

     become

     more

     integrated

     into

     our

     daily

     lives

    .

     This

     could

     lead

     to

     a

     significant

     increase

     in

     efficiency

     and

     productivity

    ,

     but

     it

     could

     also

     lead

     to

     job

     displacement

     if

     not

     managed

     properly

    .
    


    2

    .

     AI

     will

     become

     more

     ethical

    .

     As

     AI

     is

     increasingly

     used

     in

     a

     wide

     range

     of

     applications

    ,

     including

     healthcare

    ,

     finance

    ,

     and

     transportation

    ,

     ethical

     concerns

     will

     become

     more

     prominent

    .

     AI

     developers

     will

     need

     to

     consider

     the

     potential

     consequences

     of

     their

     work

     and

     develop

     ethical

    



```python
llm.shutdown()
```
