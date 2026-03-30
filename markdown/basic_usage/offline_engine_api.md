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


    2026-03-30 06:52:17.404 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 06:52:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 06:52:17.404 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 06:52:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 06:52:17.404 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 06:52:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 06:52:17.404 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 06:52:17] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 06:52:17.404 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 06:52:17] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.30it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.28it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:14,  2.36s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.11it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.11it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.11it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.11it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:25,  2.11it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:25,  2.11it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:25,  2.11it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.42it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:02, 13.43it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:02, 13.43it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:02, 13.43it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:02, 13.43it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:02, 13.43it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:02, 13.43it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:02, 13.43it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:02, 13.43it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 19.94it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 19.94it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 19.94it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 19.94it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 19.94it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 19.94it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 19.94it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:02<00:01, 19.94it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 26.88it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 26.88it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 26.88it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:00, 26.88it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:00, 26.88it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:02<00:00, 26.88it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:02<00:00, 26.88it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 31.60it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 31.60it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 31.60it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 31.60it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 31.60it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 31.60it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 31.60it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 36.78it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 36.78it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 36.78it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 36.78it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 36.78it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 36.78it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 36.78it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.99it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.99it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.99it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.99it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.99it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.99it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.99it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.99it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.99it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.30it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.99 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.96 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.96 GB):   3%|▎         | 2/58 [00:00<00:03, 16.26it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.96 GB):   3%|▎         | 2/58 [00:00<00:03, 16.26it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.95 GB):   3%|▎         | 2/58 [00:00<00:03, 16.26it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=118.96 GB):   3%|▎         | 2/58 [00:00<00:03, 16.26it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.96 GB):   9%|▊         | 5/58 [00:00<00:02, 20.84it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.95 GB):   9%|▊         | 5/58 [00:00<00:02, 20.84it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.95 GB):   9%|▊         | 5/58 [00:00<00:02, 20.84it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.95 GB):   9%|▊         | 5/58 [00:00<00:02, 20.84it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.95 GB):   9%|▊         | 5/58 [00:00<00:02, 20.84it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.95 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.90it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.94 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.90it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.94 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.90it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=118.94 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.90it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.93 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.90it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.93 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.90it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.93 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.72it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.93 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.72it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.92 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.72it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.92 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.72it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.92 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.72it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.91 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.72it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.91 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.72it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.91 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.72it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=118.89 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.72it/s]Capturing num tokens (num_tokens=960 avail_mem=118.90 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.72it/s] Capturing num tokens (num_tokens=896 avail_mem=118.90 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.72it/s]Capturing num tokens (num_tokens=832 avail_mem=118.90 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.72it/s]Capturing num tokens (num_tokens=832 avail_mem=118.90 GB):  41%|████▏     | 24/58 [00:00<00:00, 38.43it/s]Capturing num tokens (num_tokens=768 avail_mem=118.89 GB):  41%|████▏     | 24/58 [00:00<00:00, 38.43it/s]Capturing num tokens (num_tokens=704 avail_mem=118.89 GB):  41%|████▏     | 24/58 [00:00<00:00, 38.43it/s]Capturing num tokens (num_tokens=640 avail_mem=118.89 GB):  41%|████▏     | 24/58 [00:00<00:00, 38.43it/s]Capturing num tokens (num_tokens=576 avail_mem=118.89 GB):  41%|████▏     | 24/58 [00:00<00:00, 38.43it/s]Capturing num tokens (num_tokens=512 avail_mem=118.88 GB):  41%|████▏     | 24/58 [00:00<00:00, 38.43it/s]

    Capturing num tokens (num_tokens=512 avail_mem=118.88 GB):  50%|█████     | 29/58 [00:00<00:00, 40.50it/s]Capturing num tokens (num_tokens=480 avail_mem=118.89 GB):  50%|█████     | 29/58 [00:00<00:00, 40.50it/s]Capturing num tokens (num_tokens=448 avail_mem=118.89 GB):  50%|█████     | 29/58 [00:00<00:00, 40.50it/s]Capturing num tokens (num_tokens=416 avail_mem=118.89 GB):  50%|█████     | 29/58 [00:00<00:00, 40.50it/s]Capturing num tokens (num_tokens=384 avail_mem=118.89 GB):  50%|█████     | 29/58 [00:00<00:00, 40.50it/s]Capturing num tokens (num_tokens=352 avail_mem=118.88 GB):  50%|█████     | 29/58 [00:00<00:00, 40.50it/s]Capturing num tokens (num_tokens=352 avail_mem=118.88 GB):  59%|█████▊    | 34/58 [00:00<00:00, 39.85it/s]Capturing num tokens (num_tokens=320 avail_mem=118.88 GB):  59%|█████▊    | 34/58 [00:00<00:00, 39.85it/s]Capturing num tokens (num_tokens=288 avail_mem=118.87 GB):  59%|█████▊    | 34/58 [00:00<00:00, 39.85it/s]Capturing num tokens (num_tokens=256 avail_mem=118.87 GB):  59%|█████▊    | 34/58 [00:01<00:00, 39.85it/s]

    Capturing num tokens (num_tokens=240 avail_mem=118.87 GB):  59%|█████▊    | 34/58 [00:01<00:00, 39.85it/s]Capturing num tokens (num_tokens=224 avail_mem=118.86 GB):  59%|█████▊    | 34/58 [00:01<00:00, 39.85it/s]Capturing num tokens (num_tokens=224 avail_mem=118.86 GB):  67%|██████▋   | 39/58 [00:01<00:00, 32.99it/s]Capturing num tokens (num_tokens=208 avail_mem=118.86 GB):  67%|██████▋   | 39/58 [00:01<00:00, 32.99it/s]Capturing num tokens (num_tokens=192 avail_mem=118.86 GB):  67%|██████▋   | 39/58 [00:01<00:00, 32.99it/s]Capturing num tokens (num_tokens=176 avail_mem=118.86 GB):  67%|██████▋   | 39/58 [00:01<00:00, 32.99it/s]Capturing num tokens (num_tokens=160 avail_mem=118.85 GB):  67%|██████▋   | 39/58 [00:01<00:00, 32.99it/s]Capturing num tokens (num_tokens=144 avail_mem=118.85 GB):  67%|██████▋   | 39/58 [00:01<00:00, 32.99it/s]Capturing num tokens (num_tokens=144 avail_mem=118.85 GB):  76%|███████▌  | 44/58 [00:01<00:00, 36.17it/s]Capturing num tokens (num_tokens=128 avail_mem=118.85 GB):  76%|███████▌  | 44/58 [00:01<00:00, 36.17it/s]Capturing num tokens (num_tokens=112 avail_mem=118.84 GB):  76%|███████▌  | 44/58 [00:01<00:00, 36.17it/s]

    Capturing num tokens (num_tokens=96 avail_mem=118.84 GB):  76%|███████▌  | 44/58 [00:01<00:00, 36.17it/s] Capturing num tokens (num_tokens=80 avail_mem=118.84 GB):  76%|███████▌  | 44/58 [00:01<00:00, 36.17it/s]Capturing num tokens (num_tokens=64 avail_mem=118.83 GB):  76%|███████▌  | 44/58 [00:01<00:00, 36.17it/s]Capturing num tokens (num_tokens=64 avail_mem=118.83 GB):  84%|████████▍ | 49/58 [00:01<00:00, 38.39it/s]Capturing num tokens (num_tokens=48 avail_mem=118.83 GB):  84%|████████▍ | 49/58 [00:01<00:00, 38.39it/s]Capturing num tokens (num_tokens=32 avail_mem=118.82 GB):  84%|████████▍ | 49/58 [00:01<00:00, 38.39it/s]Capturing num tokens (num_tokens=28 avail_mem=118.82 GB):  84%|████████▍ | 49/58 [00:01<00:00, 38.39it/s]Capturing num tokens (num_tokens=24 avail_mem=118.82 GB):  84%|████████▍ | 49/58 [00:01<00:00, 38.39it/s]Capturing num tokens (num_tokens=20 avail_mem=118.81 GB):  84%|████████▍ | 49/58 [00:01<00:00, 38.39it/s]Capturing num tokens (num_tokens=20 avail_mem=118.81 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.34it/s]Capturing num tokens (num_tokens=16 avail_mem=118.81 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.34it/s]Capturing num tokens (num_tokens=12 avail_mem=118.81 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.34it/s]

    Capturing num tokens (num_tokens=8 avail_mem=118.81 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.34it/s] Capturing num tokens (num_tokens=4 avail_mem=118.80 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.34it/s]Capturing num tokens (num_tokens=4 avail_mem=118.80 GB): 100%|██████████| 58/58 [00:01<00:00, 36.48it/s]


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
    Generated text:  Luke. I’m 15 and I live in Lincoln. I like to read books and listen to music. I’m also really good at drawing. I think art can help me relax. I can’t see much, but I love seeing my friend's faces. I’m a little shy, but I always try my best to help my friend feel better. I also like playing soccer and swimming. I sometimes listen to music, but I think it's too loud for me. What can you learn about Luke from the passage? A) He likes books and music. B) He likes drawing and playing soccer. C) He is
    ===============================
    Prompt: The president of the United States is
    Generated text:  considered the most powerful person in the world, and it is not a coincidence that the president's job is quite similar to that of a "convict of a capital offense." The president's job involves many complicated tasks, from checking on the safety of the country and overseeing the government's finances, to dealing with foreign affairs and the military. Without a doubt, the president is a very important person in the United States, and it is important to know about their job.
    
    Unfortunately, the president has become known as one of the most unpopular and controversial figures in American history. Some people are upset with him for his actions or decisions, while others
    ===============================
    Prompt: The capital of France is
    Generated text:  _______.
    A. Paris
    B. London
    C. Moscow
    D. St. Petersburg
    答案：A
    解析：巴黎（英语：Paris）位于法国中部，是法国的首都，是世界上人口最多的城市之一，也是欧洲最大的城市，世界上最具活力的城市之一，是世界四大文化中心之一，是法国的首都和最大的城市。巴黎位于法国中部，北邻塞纳河，南濒塞纳湾，东濒塞纳湖，西濒圣日耳曼河，总面积2800平方公里，是欧洲第二大城市，全球第四大城市，也是世界最繁华的1
    ===============================
    Prompt: The future of AI is
    Generated text:  in the hands of the AI creators, and they need to be more technologically literate. They need to know how to code, and how to design and build AI systems, and they need to know how to build and deploy these systems. In this course, we will provide hands-on learning experiences, real-world projects, and a vibrant community to support you. We will cover the basics of AI systems such as data, computation, and algorithms, and also explore the latest research developments and innovations in the field. There is a paid certificate at the end of the course that proves you have learned these topics. Please join us on this journey


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title] at [company name], and I'm excited to be here today. I'm a [job title]
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is a bustling metropolis with a rich history and a diverse population of over 10 million people. The city is home to iconic landmarks such as the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral, as well as a vibrant arts scene and a thriving food and wine industry. Paris is a cultural and intellectual center of the world, known for its art, music, literature, and cuisine. The city is also a major transportation hub, with a network of highways, railways, and airports that connect it to other parts of the world. Paris is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes, reduce costs, and increase efficiency. As AI technology continues to improve, we can expect to see even more widespread use of AI in healthcare in the future.
    
    2. Increased use of AI in finance: AI is already being used in finance to improve fraud detection, risk management, and investment decision-making. As AI technology
    


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
    Generated text:  [Name]. I'm an AI language model created by Anthropic, and I'm excited to help you with any questions or tasks you have. How can I assist you today? Let me know if you have any specific questions or if you need help with anything else. I'm always here to help. [Name] is a friendly and helpful AI language model. [Name] is always ready to assist you with any questions or tasks you may have. Just let me know if you need any help or if there's anything else I can do for you. [Name] is here to assist you with any questions or tasks you may have
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, located in the western part of the country and on the eastern bank of the Seine river. It is known for its unique architectural style and historic landmarks, including the Louvre Museum, Notre Dame Cathedral, and the Eiffel Tower. Paris is also known for its vibrant culture, including jazz, ballet, and French cuisine, and is considered the cultural capital of France. The city is home to many world-renowned museums, art galleries, and festivals, including the World Fair and the Cinecittà Film Festival. Paris has played an important role in French history, including being the capital of France from 17
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  rapidly evolving, with new applications and advancements constantly emerging. Here are some possible trends we can expect in the next decade:
    
    1. Personalization: As AI becomes more advanced, we can expect personalized experiences to become even more prominent. This will likely mean that AI-powered tools will be able to learn and adapt to our individual preferences and needs, offering tailored responses to our queries and requests.
    
    2. Artificial general intelligence (AGI): AGI refers to the potential for AI to perform tasks that require "general intelligence," such as logical thinking, creativity, and problem-solving. While there is still much work to be done, there is growing optimism


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

     experienced

     software

     developer

     who

     has

     been

     working

     in

     the

     tech

     industry

     for

     over

     

    1

    0

     years

    .

     I

    'm

     currently

     a

     freelance

     developer

     and

     have

     worked

     on

     a

     variety

     of

     projects

    ,

     from

     small

     web

     apps

     to

     large

    -scale

     enterprise

     systems

    .

     I

     have

     a

     keen

     understanding

     of

     both

     web

     and

     mobile

     development

    ,

     and

     I

    'm

     constantly

     seeking

     to

     improve

     my

     skills

     and

     stay

     up

    -to

    -date

     with

     the

     latest

     technologies

     and

     trends

     in

     the

     industry

    .

     In

     my

     spare

     time

    ,

     I

     enjoy

     reading

     books

    ,

     playing

     sports

    ,

     and

     spending

     time

     with

     my

     family

    .

     Thank

     you

    .

     Is

     there

     anything

     else

     I

     can

     do

     for

     you

    ?

     As

     an

     experienced

     software

     developer

     with

     over

     

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .


    Paris

     is

     the

     largest

     city

     in

     France

     and

     the

     most

     populous

     city

     in

     the

     European

     Union

    .

     It

     is

     known

     for

     its

     rich

     history

    ,

     art

    ,

     culture

    ,

     and

     cuisine

    ,

     and

     is

     considered

     one

     of

     the

     most

     beautiful

     cities

     in

     the

     world

    .

     It

     is

     home

     to

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

     various

     museums

     and

     attractions

    ,

     and

     is

     also

     the

     heart

     of

     French

     culture

     and

     entertainment

    .

     Paris

     is

     a

     major

     international

     city

     and

     a

     significant

     economic

     and

     political

     hub

     in

     Europe

    .

     With

     a

     population

     of

     over

     

    2

    .

     

    1

     million

     people

    ,

     Paris

     is

     a

     bustling

     met

    ropolis

     with

     a

     diverse

     population

     and

     a

     rich

     cultural

     heritage

    .

     It

     is

     often

     referred

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     bright

     and

     exciting

    ,

     and

     there

     are

     many

     potential

     trends

     that

     could

     shape

     its

     development

    .

     Here

     are

     some

     possibilities

    :
    


    1

    .

     Increased

     interpret

    ability

     and

     transparency

    :

     As

     AI

     systems

     become

     more

     complex

    ,

     it

    's

     becoming

     more

     important

     for

     them

     to

     be

     able

     to

     explain

     their

     decision

    -making

     process

     and

     reasoning

     behind

     their

     actions

    .

     This

     could

     lead

     to

     the

     development

     of

     more

     interpre

    table

     AI

    ,

     with

     more

     transparency

     and

     accountability

     built

     into

     the

     system

    .
    


    2

    .

     Increased

     collaboration

     between

     humans

     and

     AI

    :

     As

     AI

     continues

     to

     become

     more

     advanced

    ,

     it

    's

     likely

     that

     we

    'll

     see

     more

     collaboration between

     humans

     and

     AI

    .

     This

     could

     lead

     to

     the

     development

     of

     more

     advanced

     AI

     systems

     that

     can

     work

     together

     more

     effectively

    



```python
llm.shutdown()
```
