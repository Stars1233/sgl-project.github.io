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


    2026-04-02 09:26:48.391 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:26:48] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:26:48.391 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:26:48] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:26:48.391 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:26:48] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:26:48.391 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:26:48] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 09:26:48.391 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 09:26:48] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.08it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.07it/s]


    2026-04-02 09:26:52,785 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 09:26:52] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:33,  2.70s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:33,  2.70s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:33,  2.70s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:33,  2.70s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:33,  2.70s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:22,  2.34it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:22,  2.34it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:22,  2.34it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:22,  2.34it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:22,  2.34it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:22,  2.34it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:22,  2.34it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.16it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.16it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.16it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.16it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:07,  6.16it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:07,  6.16it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:07,  6.16it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:07,  6.16it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  6.16it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.48it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.48it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.48it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.48it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.48it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.48it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.48it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.48it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.57it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.57it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.57it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.57it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.57it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.57it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.57it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.57it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:01, 24.96it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:01, 24.96it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:01, 24.96it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:01, 24.96it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:01, 24.96it/s]

    Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:01, 24.96it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:01, 24.96it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 34.99it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 34.99it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 34.99it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 34.99it/s]

    Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 34.99it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 34.99it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 34.99it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 39.18it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 39.18it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 39.18it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 39.18it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 39.18it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 39.18it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 39.18it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 39.18it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.83it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=133.24 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=133.21 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=133.21 GB):   3%|▎         | 2/58 [00:00<00:02, 19.03it/s]Capturing num tokens (num_tokens=7168 avail_mem=133.20 GB):   3%|▎         | 2/58 [00:00<00:02, 19.03it/s]Capturing num tokens (num_tokens=6656 avail_mem=133.20 GB):   3%|▎         | 2/58 [00:00<00:02, 19.03it/s]Capturing num tokens (num_tokens=6144 avail_mem=133.20 GB):   3%|▎         | 2/58 [00:00<00:02, 19.03it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=133.20 GB):   9%|▊         | 5/58 [00:00<00:02, 22.26it/s]Capturing num tokens (num_tokens=5632 avail_mem=133.20 GB):   9%|▊         | 5/58 [00:00<00:02, 22.26it/s]Capturing num tokens (num_tokens=5120 avail_mem=133.20 GB):   9%|▊         | 5/58 [00:00<00:02, 22.26it/s]Capturing num tokens (num_tokens=4608 avail_mem=133.20 GB):   9%|▊         | 5/58 [00:00<00:02, 22.26it/s]Capturing num tokens (num_tokens=4096 avail_mem=133.20 GB):   9%|▊         | 5/58 [00:00<00:02, 22.26it/s]Capturing num tokens (num_tokens=4096 avail_mem=133.20 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.96it/s]Capturing num tokens (num_tokens=3840 avail_mem=133.19 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.96it/s]Capturing num tokens (num_tokens=3584 avail_mem=133.19 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.96it/s]Capturing num tokens (num_tokens=3328 avail_mem=133.19 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.96it/s]Capturing num tokens (num_tokens=3072 avail_mem=133.18 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.96it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=133.18 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.96it/s]Capturing num tokens (num_tokens=2816 avail_mem=133.18 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.61it/s]Capturing num tokens (num_tokens=2560 avail_mem=133.18 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.61it/s]Capturing num tokens (num_tokens=2304 avail_mem=133.17 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.61it/s]Capturing num tokens (num_tokens=2048 avail_mem=133.17 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.61it/s]Capturing num tokens (num_tokens=1792 avail_mem=133.17 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.61it/s]Capturing num tokens (num_tokens=1536 avail_mem=133.16 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.61it/s]Capturing num tokens (num_tokens=1536 avail_mem=133.16 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.79it/s]Capturing num tokens (num_tokens=1280 avail_mem=133.16 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.79it/s]Capturing num tokens (num_tokens=1024 avail_mem=133.14 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.79it/s]Capturing num tokens (num_tokens=960 avail_mem=133.15 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.79it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=133.15 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.79it/s]Capturing num tokens (num_tokens=832 avail_mem=133.15 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.79it/s]Capturing num tokens (num_tokens=832 avail_mem=133.15 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.53it/s]Capturing num tokens (num_tokens=768 avail_mem=133.14 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.53it/s]Capturing num tokens (num_tokens=704 avail_mem=133.14 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.53it/s]Capturing num tokens (num_tokens=640 avail_mem=133.14 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.53it/s]Capturing num tokens (num_tokens=576 avail_mem=133.14 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.53it/s]Capturing num tokens (num_tokens=512 avail_mem=133.12 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.53it/s]Capturing num tokens (num_tokens=512 avail_mem=133.12 GB):  50%|█████     | 29/58 [00:00<00:00, 41.36it/s]Capturing num tokens (num_tokens=480 avail_mem=133.14 GB):  50%|█████     | 29/58 [00:00<00:00, 41.36it/s]Capturing num tokens (num_tokens=448 avail_mem=133.14 GB):  50%|█████     | 29/58 [00:00<00:00, 41.36it/s]Capturing num tokens (num_tokens=416 avail_mem=133.14 GB):  50%|█████     | 29/58 [00:00<00:00, 41.36it/s]

    Capturing num tokens (num_tokens=384 avail_mem=133.13 GB):  50%|█████     | 29/58 [00:00<00:00, 41.36it/s]Capturing num tokens (num_tokens=352 avail_mem=133.13 GB):  50%|█████     | 29/58 [00:00<00:00, 41.36it/s]Capturing num tokens (num_tokens=352 avail_mem=133.13 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.65it/s]Capturing num tokens (num_tokens=320 avail_mem=133.12 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.65it/s]Capturing num tokens (num_tokens=288 avail_mem=133.12 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.65it/s]Capturing num tokens (num_tokens=256 avail_mem=133.12 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.65it/s]Capturing num tokens (num_tokens=240 avail_mem=133.12 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.65it/s]Capturing num tokens (num_tokens=224 avail_mem=133.11 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.65it/s]Capturing num tokens (num_tokens=224 avail_mem=133.11 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.46it/s]Capturing num tokens (num_tokens=208 avail_mem=133.11 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.46it/s]Capturing num tokens (num_tokens=192 avail_mem=133.11 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.46it/s]Capturing num tokens (num_tokens=176 avail_mem=133.11 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.46it/s]

    Capturing num tokens (num_tokens=160 avail_mem=133.10 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.46it/s]Capturing num tokens (num_tokens=144 avail_mem=133.10 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.46it/s]Capturing num tokens (num_tokens=144 avail_mem=133.10 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.21it/s]Capturing num tokens (num_tokens=128 avail_mem=133.10 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.21it/s]Capturing num tokens (num_tokens=112 avail_mem=133.09 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.21it/s]Capturing num tokens (num_tokens=96 avail_mem=133.09 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.21it/s] Capturing num tokens (num_tokens=80 avail_mem=133.09 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.21it/s]Capturing num tokens (num_tokens=64 avail_mem=133.08 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.21it/s]Capturing num tokens (num_tokens=64 avail_mem=133.08 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.37it/s]Capturing num tokens (num_tokens=48 avail_mem=133.08 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.37it/s]Capturing num tokens (num_tokens=32 avail_mem=133.07 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.37it/s]Capturing num tokens (num_tokens=28 avail_mem=133.07 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.37it/s]

    Capturing num tokens (num_tokens=24 avail_mem=133.07 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.37it/s]Capturing num tokens (num_tokens=20 avail_mem=133.06 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.37it/s]Capturing num tokens (num_tokens=20 avail_mem=133.06 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.79it/s]Capturing num tokens (num_tokens=16 avail_mem=133.06 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.79it/s]Capturing num tokens (num_tokens=12 avail_mem=133.06 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.79it/s]Capturing num tokens (num_tokens=8 avail_mem=133.06 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.79it/s] Capturing num tokens (num_tokens=4 avail_mem=133.05 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.79it/s]Capturing num tokens (num_tokens=4 avail_mem=133.05 GB): 100%|██████████| 58/58 [00:01<00:00, 40.17it/s]


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
    Generated text:  Irfan and I am a guitarist, composer, sound designer and sound engineer. My aim is to learn how to enhance the experience of others with my music.
    I like to work on projects that are based around a concept and make it real and tangible in a compelling and engaging way. I aim to bring different aspects of sound and music to life, and am interested in exploring different genres and creating sounds that are truly unique and different. I bring a creative mindset to my music and am always looking for new ways to express myself and create new sounds and sounds that resonate with others. If you’re looking for something new and exciting to work
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to decide what kind of lunch to have with his entourage at work. The president is a huge meat lover who enjoys a steak dinner every night. He has gathered some information and decides to have a lunch date with his entourage.
    
    The president is a vegetarian and wants a vegetarian lunch. The closest vegetarian restaurant is a chain called "Vegetable Heaven". This chain is located in the city, but it is very far from the president's work. The distance to the nearest restaurant is about 120 miles.
    
    The president wants to have a vegetarian lunch date with his entourage to ensure that everyone gets a chance to eat
    ===============================
    Prompt: The capital of France is
    Generated text: :
    
    A. Paris
    
    B. Lille
    
    C. Marseille
    
    D. Nice
    
    To determine the capital of France, let's review the list of countries and their capitals provided in the options:
    
    A. Paris
    B. Lille
    C. Marseille
    D. Nice
    
    The capital of France is Paris. Therefore, the correct answer is:
    
    A. Paris
    
    To confirm, let's recall the capital of France:
    - Paris is the capital and largest city of France.
    - It is located in the north-central region of France.
    - Paris is the second-largest city in France by population, after Paris.
    
    Thus, the capital
    ===============================
    Prompt: The future of AI is
    Generated text:  uncertain, but it is certain that one day AI will be able to do things that we are not able to do. In the following paragraphs, we will look at some of the many exciting potential uses of AI.
    
      1. Chatbots: The first and most obvious example of an AI application that we will see in the future is the chatbot. A chatbot is a machine or software application that can respond to natural language queries by conversing with a user on the phone, a computer, or in writing. Chatbots are designed to answer questions, provide information, and facilitate communication with users. They can be used to assist


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title]
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. 
    
    This statement is factually correct and provides a clear and concise overview of the capital city's name and location. It is appropriate for use in various contexts, such as news articles, travel guides, or educational materials about France. 
    
    However, it could be improved for clarity and conciseness. For example, the statement could be rephrased as "Paris is the capital of France." This version is more concise and easier to understand, but it still includes the key information about the capital city's name. 
    
    Overall, the statement is a good representation of the capital city's name and location, and it is appropriate
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased focus on ethical AI: As more people become aware of the potential risks of AI, there will be a greater emphasis on developing AI that is ethical and responsible. This will likely involve developing AI that is designed to minimize harm to individuals and society as a whole.
    
    2. Integration of AI with other technologies: AI is likely to become more integrated with other technologies, such as the Internet of Things (IoT), blockchain, and quantum computing. This integration will likely lead to new applications and
    


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
    Generated text:  [Name] and I'm a/an [occupation] with [number of years] years of experience in the industry. I am a creative problem solver with a passion for innovation and continuously improving my skills in [industry]. My strong work ethic, attention to detail, and ability to collaborate with others to achieve common goals have led me to [career] success. I am always looking for ways to [share a personal achievement or accomplishment]. In my free time, I enjoy [pleasure or activity]. I am always looking for new challenges and learning opportunities to grow as a professional. I am always looking for ways to improve my skills and knowledge
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, a city with a rich history, noted for its art, cuisine, and fashion. 
    
    Here is a table of the official languages spoken in France:
    
    | Official Languages | Number |
    |--------------------|--------|
    | French            | 77,300 |
    | French           | 77,300 |
    | French            | 77,300 |
    
    Can you provide some information on the cultural significance of Paris and its attractions in the city? Certainly! Paris, the largest and most visited city in France, is home to an incredible array of cultural and architectural landmarks. Here are some key points:
    
    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  full of exciting possibilities, and it will continue to evolve and advance rapidly. Here are some possible trends that we can expect to see in AI:
    
    1. Increased Precision and Accuracy: AI is getting better at processing and analyzing large amounts of data, which will result in more accurate and precise predictions and recommendations.
    
    2. Autonomous and Self-Driving Cars: AI is already being used in self-driving cars, but it has the potential to become even more advanced in the future. Autonomous cars could improve safety, reduce traffic accidents, and make transportation more efficient.
    
    3. Personalized Medicine: AI is being used to help doctors make more accurate diagnoses and


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

     a

     [

    Role

    ]

     who

     has

     been

     working

     for

     [

    Company

     Name

    ]

     for

     [

    X

     years

    ]

     years

    ,

     and

     I

     have

     a

     passion

     for

     [

    N

    iche

     or

     Industry

    ].

     I

     bring

     a

     unique

     blend

     of

     skills

     and

     experiences

     to

     [

    Company

     Name

    ]

     that

     have

     allowed

     me

     to

     excel

     in

     my

     role

    .

     I

     am

     a

     reliable

    ,

     dependable

    ,

     and

     hard

    working

     individual

     who

     is

     always

     ready

     to

     go

     the

     extra

     mile

     to

     meet

     the

     highest

     standards

     of

     the

     job

    .

     I

    'm

     confident

     in

     my

     abilities

     and

     I

    'm

     always

     up

     for

     the

     challenge

     of

     taking

     on

     new

     tasks

     and

     responsibilities

    .

     I

     look

     forward

     to

     continuing

     our

     relationship

     and

     being

     a

     valuable

     asset

     to

     [

    Company

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     Its

     historical

     significance

     and

     cultural

     diversity

     make

     it

     a

     major

     city

     with

     a

     rich

     history

     and

     vibrant

     culture

    .

     The

     city

     is

     known

     for

     its

     stunning

     architecture

    ,

     world

    -ren

    owned

     museums

    ,

     and

     diverse

     food

     and

     drink

     scene

    ,

     as

     well

     as

     its

     numerous

     festivals

     and

     events

     throughout

     the

     year

    .

     Paris

     is

     a

     popular

     tourist

     destination

    ,

     attracting

     millions

     of

     visitors

     each

     year

    ,

     making

     it

     one

     of

     the

     most

     important

     cities

     in

     the

     world

    .

     The

     city

     also

     boasts

     a

     thriving

     economy

     and

     vibrant

     economy

    ,

     and

     is

     home

     to

     many

     world

    -ren

    owned

     art

     museums

    ,

     including

     the

     Lou

    vre

     and

     Mus

    ée

     d

    '

    Or

    say

    .

     With

     its

     charming

     neighborhoods

    ,

     vibrant

     nightlife

    ,

     and

     annual

     cultural

     events

    ,

     Paris

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     heavily

     influenced

     by

     a

     variety

     of

     factors

    ,

     including

     advances

     in

     computing

     power

    ,

     data

     science

    ,

     and

     machine

     learning

    ,

     as

     well

     as

     a

     growing

     interest

     in

     ethical

     considerations

     and

     regulation

    .

     Here

     are

     some

     possible

     future

     trends

     in

     artificial

     intelligence

    :
    


    1

    .

     Autonomous

     vehicles

    :

     With

     advances

     in

     AI

    ,

     autonomous

     vehicles

     are

     becoming

     more

     common

     in

     urban

     areas

    .

     These

     vehicles

     can

     navigate

     roads

    ,

     park

     themselves

    ,

     and

     even

     navigate

     traffic

    ,

     making

     it

     much

     safer

     than

     a

     human

     driver

    .
    


    2

    .

     Personal

    ized

     healthcare

    :

     AI

     can

     be

     used

     to

     analyze

     medical

     data

     and

     provide

     personalized

     treatments

     for

     individual

     patients

    .

     This

     can

     help

     improve

     outcomes

     and

     reduce

     costs

     in

     healthcare

     systems

    .
    


    3

    .

     Sent

    iment

     analysis

    



```python
llm.shutdown()
```
