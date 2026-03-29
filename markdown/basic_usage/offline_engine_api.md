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


    2026-03-29 15:12:05.154 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 15:12:05] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 15:12:05.154 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 15:12:05] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 15:12:05.154 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 15:12:05] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 15:12:05.154 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 15:12:05] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 15:12:05.154 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 15:12:05] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.65it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.64it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:11,  2.31s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:11,  2.31s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:11,  2.31s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:11,  2.31s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:11,  2.31s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:19,  2.70it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:19,  2.70it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:19,  2.70it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:19,  2.70it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:19,  2.70it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:19,  2.70it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:19,  2.70it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:06,  7.03it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:06,  7.03it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:06,  7.03it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:06,  7.03it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:06,  7.03it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:06,  7.03it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:06,  7.03it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:06,  7.03it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:06,  7.03it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 14.05it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 14.05it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 14.05it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 14.05it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 14.05it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 14.05it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 14.05it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 14.05it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 20.56it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 20.56it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 20.56it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 20.56it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 20.56it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 20.56it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 20.56it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:02<00:01, 20.56it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:02<00:00, 27.43it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:02<00:00, 27.43it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:02<00:00, 27.43it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:02<00:00, 27.43it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:02<00:00, 27.43it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:02<00:00, 27.43it/s]

    Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:02<00:00, 27.43it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:02<00:00, 32.36it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:02<00:00, 32.36it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:02<00:00, 32.36it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 32.36it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 32.36it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 32.36it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 32.36it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 37.01it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 37.01it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 37.01it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 37.01it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 37.01it/s]

    Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 37.01it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 37.01it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 40.59it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 40.59it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 40.59it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 40.59it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 40.59it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 40.59it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 40.59it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 40.59it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.65it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=133.24 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=133.21 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=133.21 GB):   3%|▎         | 2/58 [00:00<00:02, 18.95it/s]Capturing num tokens (num_tokens=7168 avail_mem=133.20 GB):   3%|▎         | 2/58 [00:00<00:02, 18.95it/s]Capturing num tokens (num_tokens=6656 avail_mem=133.20 GB):   3%|▎         | 2/58 [00:00<00:02, 18.95it/s]Capturing num tokens (num_tokens=6144 avail_mem=133.20 GB):   3%|▎         | 2/58 [00:00<00:02, 18.95it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=133.20 GB):   9%|▊         | 5/58 [00:00<00:02, 22.11it/s]Capturing num tokens (num_tokens=5632 avail_mem=133.20 GB):   9%|▊         | 5/58 [00:00<00:02, 22.11it/s]Capturing num tokens (num_tokens=5120 avail_mem=133.20 GB):   9%|▊         | 5/58 [00:00<00:02, 22.11it/s]Capturing num tokens (num_tokens=4608 avail_mem=133.20 GB):   9%|▊         | 5/58 [00:00<00:02, 22.11it/s]Capturing num tokens (num_tokens=4096 avail_mem=133.20 GB):   9%|▊         | 5/58 [00:00<00:02, 22.11it/s]Capturing num tokens (num_tokens=4096 avail_mem=133.20 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.79it/s]Capturing num tokens (num_tokens=3840 avail_mem=133.19 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.79it/s]Capturing num tokens (num_tokens=3584 avail_mem=133.19 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.79it/s]Capturing num tokens (num_tokens=3328 avail_mem=133.19 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.79it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=133.18 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.79it/s]Capturing num tokens (num_tokens=2816 avail_mem=133.18 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.79it/s]Capturing num tokens (num_tokens=2816 avail_mem=133.18 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.40it/s]Capturing num tokens (num_tokens=2560 avail_mem=133.18 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.40it/s]Capturing num tokens (num_tokens=2304 avail_mem=133.17 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.40it/s]Capturing num tokens (num_tokens=2048 avail_mem=133.17 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.40it/s]Capturing num tokens (num_tokens=1792 avail_mem=133.17 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.40it/s]Capturing num tokens (num_tokens=1536 avail_mem=133.16 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.40it/s]Capturing num tokens (num_tokens=1536 avail_mem=133.16 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.57it/s]Capturing num tokens (num_tokens=1280 avail_mem=133.16 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.57it/s]Capturing num tokens (num_tokens=1024 avail_mem=133.14 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.57it/s]

    Capturing num tokens (num_tokens=960 avail_mem=133.15 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.57it/s] Capturing num tokens (num_tokens=896 avail_mem=133.15 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.57it/s]Capturing num tokens (num_tokens=832 avail_mem=133.15 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.57it/s]Capturing num tokens (num_tokens=832 avail_mem=133.15 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.32it/s]Capturing num tokens (num_tokens=768 avail_mem=133.14 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.32it/s]Capturing num tokens (num_tokens=704 avail_mem=133.14 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.32it/s]Capturing num tokens (num_tokens=640 avail_mem=133.14 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.32it/s]Capturing num tokens (num_tokens=576 avail_mem=133.14 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.32it/s]Capturing num tokens (num_tokens=512 avail_mem=133.12 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.32it/s]Capturing num tokens (num_tokens=512 avail_mem=133.12 GB):  50%|█████     | 29/58 [00:00<00:00, 41.26it/s]Capturing num tokens (num_tokens=480 avail_mem=133.14 GB):  50%|█████     | 29/58 [00:00<00:00, 41.26it/s]Capturing num tokens (num_tokens=448 avail_mem=133.14 GB):  50%|█████     | 29/58 [00:00<00:00, 41.26it/s]

    Capturing num tokens (num_tokens=416 avail_mem=133.14 GB):  50%|█████     | 29/58 [00:00<00:00, 41.26it/s]Capturing num tokens (num_tokens=384 avail_mem=133.13 GB):  50%|█████     | 29/58 [00:00<00:00, 41.26it/s]Capturing num tokens (num_tokens=352 avail_mem=133.13 GB):  50%|█████     | 29/58 [00:00<00:00, 41.26it/s]Capturing num tokens (num_tokens=352 avail_mem=133.13 GB):  59%|█████▊    | 34/58 [00:00<00:00, 41.57it/s]Capturing num tokens (num_tokens=320 avail_mem=133.12 GB):  59%|█████▊    | 34/58 [00:00<00:00, 41.57it/s]Capturing num tokens (num_tokens=288 avail_mem=133.12 GB):  59%|█████▊    | 34/58 [00:00<00:00, 41.57it/s]Capturing num tokens (num_tokens=256 avail_mem=133.12 GB):  59%|█████▊    | 34/58 [00:00<00:00, 41.57it/s]

    Capturing num tokens (num_tokens=240 avail_mem=133.12 GB):  59%|█████▊    | 34/58 [00:01<00:00, 41.57it/s]Capturing num tokens (num_tokens=224 avail_mem=133.11 GB):  59%|█████▊    | 34/58 [00:01<00:00, 41.57it/s]Capturing num tokens (num_tokens=224 avail_mem=133.11 GB):  67%|██████▋   | 39/58 [00:01<00:00, 31.05it/s]Capturing num tokens (num_tokens=208 avail_mem=133.11 GB):  67%|██████▋   | 39/58 [00:01<00:00, 31.05it/s]Capturing num tokens (num_tokens=192 avail_mem=133.11 GB):  67%|██████▋   | 39/58 [00:01<00:00, 31.05it/s]Capturing num tokens (num_tokens=176 avail_mem=133.11 GB):  67%|██████▋   | 39/58 [00:01<00:00, 31.05it/s]Capturing num tokens (num_tokens=160 avail_mem=133.10 GB):  67%|██████▋   | 39/58 [00:01<00:00, 31.05it/s]Capturing num tokens (num_tokens=160 avail_mem=133.10 GB):  74%|███████▍  | 43/58 [00:01<00:00, 32.25it/s]Capturing num tokens (num_tokens=144 avail_mem=133.10 GB):  74%|███████▍  | 43/58 [00:01<00:00, 32.25it/s]Capturing num tokens (num_tokens=128 avail_mem=133.10 GB):  74%|███████▍  | 43/58 [00:01<00:00, 32.25it/s]

    Capturing num tokens (num_tokens=112 avail_mem=133.09 GB):  74%|███████▍  | 43/58 [00:01<00:00, 32.25it/s]Capturing num tokens (num_tokens=96 avail_mem=133.09 GB):  74%|███████▍  | 43/58 [00:01<00:00, 32.25it/s] Capturing num tokens (num_tokens=80 avail_mem=133.09 GB):  74%|███████▍  | 43/58 [00:01<00:00, 32.25it/s]Capturing num tokens (num_tokens=80 avail_mem=133.09 GB):  83%|████████▎ | 48/58 [00:01<00:00, 35.46it/s]Capturing num tokens (num_tokens=64 avail_mem=133.08 GB):  83%|████████▎ | 48/58 [00:01<00:00, 35.46it/s]Capturing num tokens (num_tokens=48 avail_mem=133.08 GB):  83%|████████▎ | 48/58 [00:01<00:00, 35.46it/s]Capturing num tokens (num_tokens=32 avail_mem=133.07 GB):  83%|████████▎ | 48/58 [00:01<00:00, 35.46it/s]Capturing num tokens (num_tokens=28 avail_mem=133.07 GB):  83%|████████▎ | 48/58 [00:01<00:00, 35.46it/s]Capturing num tokens (num_tokens=24 avail_mem=133.07 GB):  83%|████████▎ | 48/58 [00:01<00:00, 35.46it/s]Capturing num tokens (num_tokens=24 avail_mem=133.07 GB):  91%|█████████▏| 53/58 [00:01<00:00, 38.08it/s]Capturing num tokens (num_tokens=20 avail_mem=133.06 GB):  91%|█████████▏| 53/58 [00:01<00:00, 38.08it/s]Capturing num tokens (num_tokens=16 avail_mem=133.06 GB):  91%|█████████▏| 53/58 [00:01<00:00, 38.08it/s]

    Capturing num tokens (num_tokens=12 avail_mem=133.06 GB):  91%|█████████▏| 53/58 [00:01<00:00, 38.08it/s]Capturing num tokens (num_tokens=8 avail_mem=133.06 GB):  91%|█████████▏| 53/58 [00:01<00:00, 38.08it/s] Capturing num tokens (num_tokens=4 avail_mem=133.05 GB):  91%|█████████▏| 53/58 [00:01<00:00, 38.08it/s]Capturing num tokens (num_tokens=4 avail_mem=133.05 GB): 100%|██████████| 58/58 [00:01<00:00, 40.40it/s]Capturing num tokens (num_tokens=4 avail_mem=133.05 GB): 100%|██████████| 58/58 [00:01<00:00, 35.87it/s]


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
    Generated text:  Wu and I am an Australian student studying in Japan. I have been teaching a big class of kids in a school. One of my students, Mr. Tanaka, has been a long time friend of mine. They are both very popular among the students and the kids are really enjoying studying together. I also think that Mr. Tanaka is a good teacher, but he is not really good at math. I would like to know if there is a good way to teach math to students. So, I would like to know how to teach math so that the students can understand it better. I also want to know if it is really
    ===============================
    Prompt: The president of the United States is
    Generated text:  a person who holds a specific political office. The most recent one, which was in office from January 2009 to February 2011, was Barack Obama. During his tenure, he held the position of President of the United States and served as the United States Secretary of State from 2009 to 2013. What can we conclude about the president of the United States based on the information given?
    A) The president of the United States was a person who has never held a position in the public service.
    B) The president of the United States is a person who has held the position
    ===============================
    Prompt: The capital of France is
    Generated text:  _______.____
    A. Paris
    B. Marseilles
    C. Lyon
    D. Toulouse
    答案:
     A
    
    患者男性，62岁。反复咳嗽、咳痰、气促30余年，加重伴发热1周入院。入院时体温39.4°C，呼吸急促，口唇发绀，双肺散在干、湿性啰音，心率120次/分，下肢水肿。该患者可能的诊断是
    A. 慢性阻塞性肺疾病
    B. 支气管扩张
    C. �
    ===============================
    Prompt: The future of AI is
    Generated text:  about to hit the cloud. By 2030, the world’s leading cloud service providers will have a firm grip on AI and deep learning. In fact, the whole of AI is the future. The only thing left to do is to implement the solutions.
    This is the future of AI. It is the future of the world and technology. From the time the first AI model was created, AI has been a force that changed the way we live and work. At first, it was in the fields of military and security. In the next 25 years, AI will transform the world in the following ways:
    1.


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a brief description of your job or profession]. I enjoy [insert a short description of your hobbies or interests]. I'm always looking for new challenges and opportunities to grow and learn. What do you like to do in your free time? I enjoy [insert a short description of your hobbies or interests]. I'm always looking for new challenges and opportunities to grow and learn. What's your favorite hobby or activity? I'm always looking for
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is a historic city with a rich history and a vibrant culture. The city is known for its beautiful architecture, world-renowned museums, and annual festivals such as the Eiffel Tower and the Louvre. Paris is also a popular tourist destination, with many visitors coming to see its famous landmarks and museums. The city is home to many important institutions, including the French Academy of Sciences and the French National Library. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly. The city is also known for its cuisine, with many famous French dishes such as cro
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased focus on ethical considerations: As AI becomes more integrated into our daily lives, there will be a greater emphasis on ethical considerations. This will include issues such as bias, transparency, accountability, and the potential for AI to be used for harmful purposes.
    
    2. Greater integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing for more complex and nuanced decision-making. This could lead to new forms of AI that are more capable of understanding and responding
    


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
    Generated text:  [Name] and I'm a [job title or profession] who's passionate about [specific interest or hobby]. I love traveling, trying new foods, and learning about different cultures. My journey has taken me across the globe, visiting new countries, trying new foods, and even meeting amazing people from different parts of the world. I'm excited to share my experiences and meet new people who are as curious and adventurous as I am. Thank you for having me! [Name] [Company or Group Name]
    
    Please remove the unnecessary details from the self-introduction and rewrite it to include specific details about the character's travel experiences and interests.
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    This statement is factually accurate as of the latest data available, according to official sources, and is widely recognized as the official name for the capital city of France. 
    
    To provide additional context, Paris is the largest city in France and the 13th largest city in the world. It is the seat of the French government and serves as the capital of the country and the largest metropolitan area in Europe. The city is also one of the most visited places in the world and is known for its rich history, culture, and cuisine. 
    
    Paris has a diverse population of over 2 million people, and it is known
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  highly dynamic and unpredictable, with a wide range of possible trends and developments that can shape the development of the field. Some of the most likely future trends in AI include:
    
    1. Increased integration with other technologies: AI will continue to be integrated with other technologies, including sensors, cameras, and drones, to create more efficient, accurate, and personalized solutions to a variety of problems. This integration will lead to the development of new AI systems that can be more powerful and efficient than those currently in use.
    
    2. Deep learning breakthroughs: Researchers are currently making significant progress in developing deep learning algorithms that are able to perform tasks that are currently difficult


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

    job

     title

     or

     field

     of

     expertise

    ]

     with

     over

     [

    x

    ]

     years

     of

     experience

     in

     [

    domain

     or

     field

    ].

     I

     have

     a

     [

    a

    ]

     knack

     for

     [

    a

    ]

     and

     a

     passion

     for

     [

    a

    ],

     and

     I

     enjoy

     [

    a

    ]

     and

     [

    a

    ].

     I

    'm

     always

     looking

     for

     new

     challenges

     and

     opportunities

     to

     grow

     and

     learn

    .

     I

    'm

     [

    a

    ]

     and

     I

    'm

     [

    a

    ].

     What

     kind

     of

     day

     do

     you

     usually

     have

    ?

     How

     do

     you

     stay

     motivated

     and

     productive

    ?

     What

    's

     your

     favorite

     hobby

     or

     activity

     to

     engage

     in

     outside

     of

     work

    ?

     What

     advice

     would

     you

     give

     to

     someone

     looking

     to

     transition

     into

     this

     field

     or

     industry

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     known

     for

     its

     rich

     history

    ,

     culture

    ,

     and

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

     Lou

    vre

     Museum

    ,

     and

     Notre

    -D

    ame

     Cathedral

    .

     The

     city

     is

     home

     to

     many

     of

     the

     world

    's

     major

     museums

    ,

     including

     the

     Lou

    vre

    ,

     the

     Mus

    ée

     d

    '

    Or

    say

    ,

     and

     the

     Museum

     of

     Modern

     Art

    .

     Paris

     is

     also

     known

     for

     its

     delicious

     cuisine

    ,

     particularly

     French

     cuisine

    ,

     and

     its

     diverse

     cultural

     scene

    ,

     with

     a

     large

     French

    -speaking

     population

    .

     The

     city

     is

     a

     popular

     tourist

     destination

    ,

     with

     millions

     of

     visitors

     each

     year

     and

     a

     thriving

     arts

     and

     entertainment

     industry

    .

     The

     capital

     of

     France

     has

     a

     rich

     and

     complex

     history

    ,

     and

     it

     continues

     to

     be

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     uncertain

     and

     rapidly

     changing

    ,

     but

     here

     are

     some

     potential

     trends

     that

     could

     shape

     the

     field

    :
    


    1

    .

     Increased

     AI

     Transparency

    :

     There

    's

     increasing

     concern

     about

     the

     ethical

     implications

     of

     AI

    ,

     and

     AI

     that

     is

     opaque

     or

     transparent

     about

     its

     decisions

     could

     be

     seen

     as

     more

     ethical

    .

     This

     could

     lead

     to

     a

     shift

     towards

     more

     transparent

     AI

    ,

     where

     AI

     is

     designed

     with

     its

     decision

    -making

     process

     in

     mind

    ,

     making

     it

     more

     difficult

     to

     blame

     or

     blame

    .
    


    2

    .

     Enhanced

     AI

     Eth

    ical

     Consider

    ations

    :

     As

     the

     field

     of

     AI

     evolves

    ,

     ethical

     considerations

     will

     likely

     become

     more

     important

    .

     This

     could

     lead

     to

     a

     greater

     emphasis

     on

     ethics

     in

     AI

     development

    ,

     with

     a

     focus

     on

     creating

     AI

     that

     is

    



```python
llm.shutdown()
```
