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


    2026-03-30 21:44:41.893 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 21:44:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 21:44:41.893 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 21:44:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 21:44:41.893 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 21:44:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 21:44:41.893 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 21:44:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 21:44:41.893 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 21:44:41] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.97it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.96it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:12,  2.33s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:19,  2.68it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:19,  2.68it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:19,  2.68it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:19,  2.68it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:19,  2.68it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:19,  2.68it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:19,  2.68it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:06,  6.98it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.96it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.96it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.96it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.96it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.96it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.96it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.96it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 13.96it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 20.45it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 20.45it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 20.45it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 20.45it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 20.45it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 20.45it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 20.45it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:01, 25.82it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:01, 25.82it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:01, 25.82it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:01, 25.82it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:01, 25.82it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:02<00:01, 25.82it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:02<00:01, 25.82it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:02<00:00, 30.99it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:02<00:00, 30.99it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 30.99it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 30.99it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 30.99it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 30.99it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 30.99it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.87it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.87it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.87it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.87it/s] 

    Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.87it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.87it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.87it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.95it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.95it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.95it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.95it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.95it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.95it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.95it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.95it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.95it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.41it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.91 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.88 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.88 GB):   3%|▎         | 2/58 [00:00<00:03, 15.88it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.81 GB):   3%|▎         | 2/58 [00:00<00:03, 15.88it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.79 GB):   3%|▎         | 2/58 [00:00<00:03, 15.88it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=118.79 GB):   3%|▎         | 2/58 [00:00<00:03, 15.88it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.79 GB):   9%|▊         | 5/58 [00:00<00:02, 20.21it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.78 GB):   9%|▊         | 5/58 [00:00<00:02, 20.21it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.79 GB):   9%|▊         | 5/58 [00:00<00:02, 20.21it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.78 GB):   9%|▊         | 5/58 [00:00<00:02, 20.21it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.78 GB):   9%|▊         | 5/58 [00:00<00:02, 20.21it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.78 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.55it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.78 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.55it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.77 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.55it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=118.76 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.55it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.75 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.55it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.75 GB):  22%|██▏       | 13/58 [00:00<00:01, 27.44it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.73 GB):  22%|██▏       | 13/58 [00:00<00:01, 27.44it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=118.72 GB):  22%|██▏       | 13/58 [00:00<00:01, 27.44it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.72 GB):  22%|██▏       | 13/58 [00:00<00:01, 27.44it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.72 GB):  28%|██▊       | 16/58 [00:00<00:02, 19.06it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.72 GB):  28%|██▊       | 16/58 [00:00<00:02, 19.06it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.71 GB):  28%|██▊       | 16/58 [00:00<00:02, 19.06it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=118.71 GB):  28%|██▊       | 16/58 [00:00<00:02, 19.06it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.71 GB):  33%|███▎      | 19/58 [00:01<00:02, 16.00it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.71 GB):  33%|███▎      | 19/58 [00:01<00:02, 16.00it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.69 GB):  33%|███▎      | 19/58 [00:01<00:02, 16.00it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.69 GB):  36%|███▌      | 21/58 [00:01<00:02, 16.25it/s]Capturing num tokens (num_tokens=960 avail_mem=118.70 GB):  36%|███▌      | 21/58 [00:01<00:02, 16.25it/s] Capturing num tokens (num_tokens=896 avail_mem=118.70 GB):  36%|███▌      | 21/58 [00:01<00:02, 16.25it/s]

    Capturing num tokens (num_tokens=832 avail_mem=118.69 GB):  36%|███▌      | 21/58 [00:01<00:02, 16.25it/s]Capturing num tokens (num_tokens=768 avail_mem=118.69 GB):  36%|███▌      | 21/58 [00:01<00:02, 16.25it/s]Capturing num tokens (num_tokens=768 avail_mem=118.69 GB):  43%|████▎     | 25/58 [00:01<00:01, 20.14it/s]Capturing num tokens (num_tokens=704 avail_mem=118.69 GB):  43%|████▎     | 25/58 [00:01<00:01, 20.14it/s]Capturing num tokens (num_tokens=640 avail_mem=118.69 GB):  43%|████▎     | 25/58 [00:01<00:01, 20.14it/s]Capturing num tokens (num_tokens=576 avail_mem=118.69 GB):  43%|████▎     | 25/58 [00:01<00:01, 20.14it/s]Capturing num tokens (num_tokens=512 avail_mem=118.67 GB):  43%|████▎     | 25/58 [00:01<00:01, 20.14it/s]Capturing num tokens (num_tokens=512 avail_mem=118.67 GB):  50%|█████     | 29/58 [00:01<00:01, 23.94it/s]Capturing num tokens (num_tokens=480 avail_mem=118.69 GB):  50%|█████     | 29/58 [00:01<00:01, 23.94it/s]Capturing num tokens (num_tokens=448 avail_mem=118.69 GB):  50%|█████     | 29/58 [00:01<00:01, 23.94it/s]

    Capturing num tokens (num_tokens=416 avail_mem=118.69 GB):  50%|█████     | 29/58 [00:01<00:01, 23.94it/s]Capturing num tokens (num_tokens=384 avail_mem=118.68 GB):  50%|█████     | 29/58 [00:01<00:01, 23.94it/s]Capturing num tokens (num_tokens=352 avail_mem=118.68 GB):  50%|█████     | 29/58 [00:01<00:01, 23.94it/s]Capturing num tokens (num_tokens=352 avail_mem=118.68 GB):  59%|█████▊    | 34/58 [00:01<00:00, 29.05it/s]Capturing num tokens (num_tokens=320 avail_mem=118.67 GB):  59%|█████▊    | 34/58 [00:01<00:00, 29.05it/s]Capturing num tokens (num_tokens=288 avail_mem=118.67 GB):  59%|█████▊    | 34/58 [00:01<00:00, 29.05it/s]Capturing num tokens (num_tokens=256 avail_mem=118.67 GB):  59%|█████▊    | 34/58 [00:01<00:00, 29.05it/s]Capturing num tokens (num_tokens=240 avail_mem=118.67 GB):  59%|█████▊    | 34/58 [00:01<00:00, 29.05it/s]Capturing num tokens (num_tokens=240 avail_mem=118.67 GB):  66%|██████▌   | 38/58 [00:01<00:00, 30.35it/s]Capturing num tokens (num_tokens=224 avail_mem=118.66 GB):  66%|██████▌   | 38/58 [00:01<00:00, 30.35it/s]

    Capturing num tokens (num_tokens=208 avail_mem=118.66 GB):  66%|██████▌   | 38/58 [00:01<00:00, 30.35it/s]Capturing num tokens (num_tokens=192 avail_mem=118.66 GB):  66%|██████▌   | 38/58 [00:01<00:00, 30.35it/s]Capturing num tokens (num_tokens=176 avail_mem=118.65 GB):  66%|██████▌   | 38/58 [00:01<00:00, 30.35it/s]Capturing num tokens (num_tokens=176 avail_mem=118.65 GB):  72%|███████▏  | 42/58 [00:01<00:00, 31.55it/s]Capturing num tokens (num_tokens=160 avail_mem=118.65 GB):  72%|███████▏  | 42/58 [00:01<00:00, 31.55it/s]Capturing num tokens (num_tokens=144 avail_mem=118.65 GB):  72%|███████▏  | 42/58 [00:01<00:00, 31.55it/s]Capturing num tokens (num_tokens=128 avail_mem=118.64 GB):  72%|███████▏  | 42/58 [00:01<00:00, 31.55it/s]Capturing num tokens (num_tokens=112 avail_mem=118.64 GB):  72%|███████▏  | 42/58 [00:01<00:00, 31.55it/s]

    Capturing num tokens (num_tokens=112 avail_mem=118.64 GB):  79%|███████▉  | 46/58 [00:01<00:00, 32.34it/s]Capturing num tokens (num_tokens=96 avail_mem=118.64 GB):  79%|███████▉  | 46/58 [00:01<00:00, 32.34it/s] Capturing num tokens (num_tokens=80 avail_mem=118.63 GB):  79%|███████▉  | 46/58 [00:01<00:00, 32.34it/s]Capturing num tokens (num_tokens=64 avail_mem=118.63 GB):  79%|███████▉  | 46/58 [00:01<00:00, 32.34it/s]Capturing num tokens (num_tokens=48 avail_mem=118.63 GB):  79%|███████▉  | 46/58 [00:01<00:00, 32.34it/s]Capturing num tokens (num_tokens=48 avail_mem=118.63 GB):  86%|████████▌ | 50/58 [00:01<00:00, 32.48it/s]Capturing num tokens (num_tokens=32 avail_mem=118.62 GB):  86%|████████▌ | 50/58 [00:01<00:00, 32.48it/s]Capturing num tokens (num_tokens=28 avail_mem=118.62 GB):  86%|████████▌ | 50/58 [00:01<00:00, 32.48it/s]Capturing num tokens (num_tokens=24 avail_mem=118.62 GB):  86%|████████▌ | 50/58 [00:02<00:00, 32.48it/s]

    Capturing num tokens (num_tokens=20 avail_mem=118.61 GB):  86%|████████▌ | 50/58 [00:02<00:00, 32.48it/s]Capturing num tokens (num_tokens=20 avail_mem=118.61 GB):  93%|█████████▎| 54/58 [00:02<00:00, 32.28it/s]Capturing num tokens (num_tokens=16 avail_mem=118.61 GB):  93%|█████████▎| 54/58 [00:02<00:00, 32.28it/s]Capturing num tokens (num_tokens=12 avail_mem=118.61 GB):  93%|█████████▎| 54/58 [00:02<00:00, 32.28it/s]Capturing num tokens (num_tokens=8 avail_mem=118.61 GB):  93%|█████████▎| 54/58 [00:02<00:00, 32.28it/s] Capturing num tokens (num_tokens=4 avail_mem=118.60 GB):  93%|█████████▎| 54/58 [00:02<00:00, 32.28it/s]Capturing num tokens (num_tokens=4 avail_mem=118.60 GB): 100%|██████████| 58/58 [00:02<00:00, 32.75it/s]Capturing num tokens (num_tokens=4 avail_mem=118.60 GB): 100%|██████████| 58/58 [00:02<00:00, 26.24it/s]


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
    Generated text:  M2839. I'm a passionate and experienced coach and trainer with more than 30 years of experience in teaching and leading teams. I specialize in creating, implementing, and guiding teams toward success and improvement. My goal is to help my clients reach their full potential and achieve their goals. I believe that a coaching and training approach is the most effective way to help my clients grow and become the best they can be. I have been in the coaching and training industry for over a decade and have worked with a wide range of clients, including individuals, teams, and organizations. I am a personable and approachable individual who
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to decide how many military bases to build. He prefers a nation with fewer bases, as bases are expensive. He has $100$ possible bases to choose from, and $20$ candidates for each base. He will select $10$ bases. How many different ways can he select his bases? 
    
    (A) $3 \cdot 2^{10}$  
    (B) $10^{3}$  
    (C) $\binom{10}{3}$  
    (D) $\binom{20}{10}$  
    (E) $20^{3}$
    
    To determine the number of different
    ===============================
    Prompt: The capital of France is
    Generated text:  ____
    A. Paris
    B. Lyon
    C. Marseille
    D. Nice
    Answer:
    
    A
    
    The purpose of the government is to provide public services and improve people's lives through the following means: ____.
    A. Economic means
    B. Administrative means
    C. Legal means
    D. Educational means
    Answer:
    
    ABCD
    
    A and B are two friends. A has a deep understanding of the future, while B is more concerned with the present. A's future is uncertain, and B prefers a stable environment. One day, A is in a situation where he must make a decision, and B, seeing that A
    ===============================
    Prompt: The future of AI is
    Generated text:  shifting towards Machine Learning and Deep Learning. Machine Learning is a subset of Artificial Intelligence that is about the development of algorithms that can learn from data. Machine Learning is becoming more popular in industry. It has the potential to solve complex problems and automate the process of decision-making. Deep Learning is a type of machine learning that can analyze large amounts of data and learn from it in a way that is much more complex and powerful than the previous algorithms.
    Machine Learning is used in a wide range of industries, from healthcare to finance to manufacturing. It is used to analyze data, identify patterns and trends, and make predictions and recommendations. Deep Learning is used


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is the largest city in France and the second-largest city in the European Union. Paris is known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. The city is also famous for its rich history, including the French Revolution and the French Revolution Museum. Paris is a cultural and economic center of France and a major tourist destination. It is home to many world-renowned museums, art galleries, and theaters. The city is also known for its cuisine, including French cuisine and international cuisine. Paris is a vibrant and dynamic city with a
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way that AI is used and developed. Here are some of the most likely trends that could be expected in the future:
    
    1. Increased focus on ethical considerations: As AI becomes more integrated into our daily lives, there will be a growing emphasis on ethical considerations. This could include issues such as bias, privacy, and transparency. As AI systems become more complex and sophisticated, it will be important to ensure that they are designed and used in a way that is fair and equitable.
    
    2. Greater use of AI in healthcare: AI is already being used in a number of
    


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
    Generated text:  [Name]. I'm a [job title] at [company]. How can I help you today?
    
    Write a short, neutral self-introduction for a fictional character. Hello, my name is [Name]. I'm a [job title] at [company]. How can I help you today? 
    Sure, here's a short, neutral self-introduction for a fictional character:
    
    Hello, my name is Alex. I'm a [job title] at [company]. How can I help you today? I'm a talented professional with over [number of years] years of experience in [industry]. I have a passion for [career
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, a historical and cultural city with a rich history and a cosmopolitan environment. It is known for its iconic landmarks such as the Eiffel Tower, the Louvre Museum, and Notre-Dame Cathedral, as well as for its food, wine, and fashion. Paris is a major economic center and a hub of culture and arts, and has a unique blend of traditional and modern influences in its architecture and cuisine. It is also home to a diverse population of citizens, with Parisians often known for their passion for opera, jazz, and art. Overall, Paris is a city of vibrant culture, history, and cuisine that is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  poised for significant growth and transformation. Here are some possible trends that could shape this area:
    
    1. Increased focus on ethical considerations: As AI systems become more sophisticated, there will be greater focus on addressing ethical concerns. This could include issues like bias, accountability, transparency, and privacy.
    
    2. More natural language processing: With the rise of chatbots and voice assistants, there will be an increased emphasis on developing natural language processing capabilities. This could lead to new forms of AI that can understand and interact with humans in a more natural way.
    
    3. Increased integration with other technologies: AI is already being integrated into a wide range of technologies,


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

    ...

     (

    insert

     your

     name

     here

    )

     and

     I

     am

     a

    /an

    ...

     (

    insert

     your

     profession

     or

     occupation

     here

    ).

     I

     enjoy

    ...

     (

    insert

     something

     you

     do

     in

     your

     spare

     time

     here

    ),

     and

     I

    'm

     also

    ...

     (

    insert

     something

     that

     reflects

     my

     personality

     or

     interests

     here

    ).

     I

    'm

     currently

     working

     as

     a

    ...

     (

    insert

     your

     job

     here

    ),

     and

     my

     hobbies

     include

    ...

     (

    insert

     something

     you

     enjoy

     doing

     here

    ).

     I

    'm

     an

     AI

     language

     model

    ,

     and

     I

     have

     a

     passion

     for

    ...

     (

    insert

     something

     you

     care

     about

     here

    ).

     I

     strive

     to

     help

     people

     by

    ...

     (

    insert

     something

     specific

     that

     you

     do

     here

    ).

     I

     can

    't

     say

     my

     name

     because

     I

    'm

     not

     real

    ,

     but

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     Paris

     is

     known

     for

     its

     iconic

     E

    iff

    el

     Tower

    ,

     famous

     landmarks

     like

     Notre

    -D

    ame

     Cathedral

    ,

     and

     its

     bustling

     nightlife

    .

     It

    's

     a

     vibrant

     city

     with

     a

     rich

     history

     dating

     back

     to

     ancient

     times

     and

     becoming

     one

     of

     the

     world

    's

     most

     culturally

     and

     tourist

    y

     cities

    .

     Paris

     has

     a

     diverse

     population

    ,

     including

     people

     from

     various

     national

    ities

     and

     cultures

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

     also

     home

     to

     important

     museums

    ,

     such

     as

     the

     Lou

    vre

    ,

     and

     has

     a

     thriving

     arts

     scene

    ,

     with

     numerous

     theaters

    ,

     galleries

    ,

     and

     museums

    .

     Paris

     is

     also

     known

     for

     its

     culinary

     and

     gastr

    onomic

     culture

    ,

     with

     its

     famous

     dishes

     and

     culinary

     traditions

    .

     It

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     uncertain

     and

     can

     be

     influenced

     by

     many

     variables

     such

     as

     economic

     and

     political

     factors

    ,

     technological

     advancements

    ,

     and

     the

     evolution

     of

     society

    .

     Here

     are

     some

     potential

     future

     trends

     in

     AI

    :
    


    1

    .

     Increased

     emphasis

     on

     ethical

     AI

    :

     With

     more

     and

     more

     people

     becoming

     concerned

     about

     AI

    's

     impact

     on

     society

    ,

     there

     is

     a

     growing

     emphasis

     on

     ethical

     AI

    .

     This

     includes

     developing

     AI

     that

     is

     transparent

    ,

     accountable

    ,

     and

     unbiased

    .

     It

     is

     also

     expected

     that

     governments

     and

     industry

     leaders

     will

     start

     to

     implement

     regulations

     and

     standards

     to

     ensure

     that

     AI

     is

     developed

     and

     used

     responsibly

    .
    


    2

    .

     Greater

     focus

     on

     AI

     for

     personal

     and

     commercial

     use

    :

     As

     AI

     becomes

     more

     accessible

     and

     affordable

    ,

     its

     use

     is

     likely

     to

    



```python
llm.shutdown()
```
