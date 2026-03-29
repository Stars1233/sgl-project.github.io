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


    2026-03-29 22:20:23.407 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 22:20:23] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 22:20:23.407 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 22:20:23] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 22:20:23.407 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 22:20:23] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 22:20:23.407 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 22:20:23] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 22:20:23.407 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 22:20:23] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.58it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.57it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.41it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.41it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.41it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.41it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.41it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.41it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.41it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.41it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.41it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 13.32it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 13.32it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:03, 13.32it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:03, 13.32it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:03, 13.32it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:03, 13.32it/s]

    Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:03, 13.32it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:03, 13.32it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 19.83it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 19.83it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 19.83it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 19.83it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 19.83it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 19.83it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 19.83it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:02<00:01, 19.83it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 26.75it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 26.75it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 26.75it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:00, 26.75it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:00, 26.75it/s]

    Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:02<00:00, 26.75it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:02<00:00, 26.75it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 31.77it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 31.77it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 31.77it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 31.77it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 31.77it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 31.77it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 31.77it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.60it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.60it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.60it/s]

    Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.60it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.60it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.60it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.60it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.88it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.88it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.88it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.88it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.88it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.88it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.88it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.88it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.88it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.28it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=132.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=132.39 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=132.39 GB):   3%|▎         | 2/58 [00:00<00:02, 18.79it/s]Capturing num tokens (num_tokens=7168 avail_mem=132.38 GB):   3%|▎         | 2/58 [00:00<00:02, 18.79it/s]Capturing num tokens (num_tokens=6656 avail_mem=132.38 GB):   3%|▎         | 2/58 [00:00<00:02, 18.79it/s]Capturing num tokens (num_tokens=6144 avail_mem=132.38 GB):   3%|▎         | 2/58 [00:00<00:02, 18.79it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=132.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.87it/s]Capturing num tokens (num_tokens=5632 avail_mem=132.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.87it/s]Capturing num tokens (num_tokens=5120 avail_mem=132.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.87it/s]Capturing num tokens (num_tokens=4608 avail_mem=132.37 GB):   9%|▊         | 5/58 [00:00<00:02, 21.87it/s]Capturing num tokens (num_tokens=4096 avail_mem=132.37 GB):   9%|▊         | 5/58 [00:00<00:02, 21.87it/s]Capturing num tokens (num_tokens=4096 avail_mem=132.37 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=3840 avail_mem=132.37 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=3584 avail_mem=132.37 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=3328 avail_mem=132.36 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=132.36 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=2816 avail_mem=132.36 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=2816 avail_mem=132.36 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.25it/s]Capturing num tokens (num_tokens=2560 avail_mem=132.35 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.25it/s]Capturing num tokens (num_tokens=2304 avail_mem=132.35 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.25it/s]Capturing num tokens (num_tokens=2048 avail_mem=132.35 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.25it/s]Capturing num tokens (num_tokens=1792 avail_mem=132.34 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.25it/s]Capturing num tokens (num_tokens=1536 avail_mem=132.34 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.25it/s]Capturing num tokens (num_tokens=1536 avail_mem=132.34 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.54it/s]Capturing num tokens (num_tokens=1280 avail_mem=132.34 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.54it/s]Capturing num tokens (num_tokens=1024 avail_mem=132.32 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.54it/s]

    Capturing num tokens (num_tokens=960 avail_mem=132.33 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.54it/s] Capturing num tokens (num_tokens=896 avail_mem=132.33 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.54it/s]Capturing num tokens (num_tokens=832 avail_mem=132.32 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.54it/s]Capturing num tokens (num_tokens=832 avail_mem=132.32 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.36it/s]Capturing num tokens (num_tokens=768 avail_mem=132.32 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.36it/s]Capturing num tokens (num_tokens=704 avail_mem=132.32 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.36it/s]Capturing num tokens (num_tokens=640 avail_mem=132.31 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.36it/s]Capturing num tokens (num_tokens=576 avail_mem=132.31 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.36it/s]Capturing num tokens (num_tokens=512 avail_mem=132.30 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.36it/s]Capturing num tokens (num_tokens=512 avail_mem=132.30 GB):  50%|█████     | 29/58 [00:00<00:00, 41.31it/s]Capturing num tokens (num_tokens=480 avail_mem=132.32 GB):  50%|█████     | 29/58 [00:00<00:00, 41.31it/s]Capturing num tokens (num_tokens=448 avail_mem=132.32 GB):  50%|█████     | 29/58 [00:00<00:00, 41.31it/s]

    Capturing num tokens (num_tokens=416 avail_mem=132.31 GB):  50%|█████     | 29/58 [00:00<00:00, 41.31it/s]Capturing num tokens (num_tokens=384 avail_mem=132.31 GB):  50%|█████     | 29/58 [00:00<00:00, 41.31it/s]Capturing num tokens (num_tokens=352 avail_mem=132.31 GB):  50%|█████     | 29/58 [00:00<00:00, 41.31it/s]Capturing num tokens (num_tokens=352 avail_mem=132.31 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.77it/s]Capturing num tokens (num_tokens=320 avail_mem=132.30 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.77it/s]Capturing num tokens (num_tokens=288 avail_mem=132.30 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.77it/s]Capturing num tokens (num_tokens=256 avail_mem=132.30 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.77it/s]Capturing num tokens (num_tokens=240 avail_mem=132.29 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.77it/s]Capturing num tokens (num_tokens=224 avail_mem=132.29 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.77it/s]Capturing num tokens (num_tokens=224 avail_mem=132.29 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.55it/s]Capturing num tokens (num_tokens=208 avail_mem=132.29 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.55it/s]Capturing num tokens (num_tokens=192 avail_mem=132.29 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.55it/s]

    Capturing num tokens (num_tokens=176 avail_mem=132.28 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.55it/s]Capturing num tokens (num_tokens=160 avail_mem=132.28 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.55it/s]Capturing num tokens (num_tokens=144 avail_mem=132.28 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.55it/s]Capturing num tokens (num_tokens=144 avail_mem=132.28 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.31it/s]Capturing num tokens (num_tokens=128 avail_mem=132.27 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.31it/s]Capturing num tokens (num_tokens=112 avail_mem=132.27 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.31it/s]Capturing num tokens (num_tokens=96 avail_mem=132.27 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.31it/s] Capturing num tokens (num_tokens=80 avail_mem=132.26 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.31it/s]Capturing num tokens (num_tokens=64 avail_mem=132.26 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.31it/s]Capturing num tokens (num_tokens=64 avail_mem=132.26 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.48it/s]Capturing num tokens (num_tokens=48 avail_mem=132.26 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.48it/s]Capturing num tokens (num_tokens=32 avail_mem=132.25 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.48it/s]

    Capturing num tokens (num_tokens=28 avail_mem=132.25 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.48it/s]Capturing num tokens (num_tokens=24 avail_mem=132.25 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.48it/s]Capturing num tokens (num_tokens=20 avail_mem=132.24 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.48it/s]Capturing num tokens (num_tokens=20 avail_mem=132.24 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.87it/s]Capturing num tokens (num_tokens=16 avail_mem=132.24 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.87it/s]Capturing num tokens (num_tokens=12 avail_mem=132.24 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.87it/s]Capturing num tokens (num_tokens=8 avail_mem=132.23 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.87it/s] Capturing num tokens (num_tokens=4 avail_mem=132.23 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.87it/s]Capturing num tokens (num_tokens=4 avail_mem=132.23 GB): 100%|██████████| 58/58 [00:01<00:00, 40.07it/s]


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
    Generated text:  Remy, I'm 21 years old and I was born in the United States. I'm very fond of sports, music and food. I have a great passion for being with people and I feel like I'm really good at my job. I've always been interested in healthcare since I was 15. I have always been really interested in different kinds of restaurants and I've been a vegetarian for about 10 years. In addition, I've been an avid long-distance runner for over 10 years. I have a great deal of experience with health and fitness. I have been a member of the Long Distance
    ===============================
    Prompt: The president of the United States is
    Generated text:  seeking to assess the effectiveness of their policies and the influence of various political groups on the American political system. The president's assistant has suggested that the White House should have a separate office to monitor the influence of outside groups on American politics. The president would like to explore this idea further and is seeking input from experts in the field of political science. 
    
    The assistant suggests that the White House should set up a working group to study the influence of different political groups on American politics. This working group would need to collect data on how these groups influence political outcomes and then present their findings to the White House for consideration. 
    
    The assistant also proposes that
    ===============================
    Prompt: The capital of France is
    Generated text: :
    
    A) Paris  
    B) London  
    C) Madrid  
    D) Rome  
    E) Berlin
    
    1. **Identify the Capital Cities**: First, we need to identify the capital cities of all the provided options. The cities in question are Paris, London, Madrid, Rome, and Berlin.
    
    2. **Analyze the Options**:
       - **Paris**: The capital of France is indeed located in the city of Paris. It is the largest city in France, with significant cultural, economic, and political importance.
       - **London**: The capital of the United Kingdom is London.
       - **Madrid**: The capital
    ===============================
    Prompt: The future of AI is
    Generated text:  very bright. New areas of research and development will be launched, and new products will be made available to consumers. In the future, we will see a rise in the number of AI-driven applications, which will allow people to live more convenient and efficient lives. As a result, the internet of things (IoT) will become even more prevalent. In addition, we will see the development of AI as a communication tool. People can use AI to communicate with people in the world and share their thoughts and ideas. This will help people connect with each other and form a community. Finally, AI will be used in various industries, such as


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. It is the largest city in France and the third-largest city in the world by population. The city is known for its rich history, art, and culture, and is home to many famous landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. Paris is also a major transportation hub, with many major highways, railroads, and airports. The city is known for its fashion industry, with many famous designers and boutiques. Overall, Paris is a vibrant and exciting city that is a must-visit for anyone interested in French culture and history.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in several key areas, including:
    
    1. Increased integration with human intelligence: AI systems will become more integrated with human intelligence, allowing them to learn from and adapt to human behavior and decision-making processes.
    
    2. Enhanced machine learning capabilities: AI will continue to improve its ability to learn from data and make more accurate predictions and decisions.
    
    3. Increased focus on ethical considerations: As AI systems become more integrated with human intelligence, there will be increased focus on ethical considerations and responsible use of AI.
    
    4. Greater use of AI in healthcare: AI will be used to improve the accuracy and efficiency of medical diagnoses and
    


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
    Generated text:  [Your Name], and I am a freelance graphic designer with a passion for creating beautiful and professional design solutions for clients. I am a collaborative and empathetic designer who always strives to go beyond what the client needs, delivering results that not only meet their expectations but also exceed them. My design process is guided by a deep understanding of color theory, typography, and image composition, resulting in designs that are not only aesthetically pleasing but also effective in conveying the intended message. I believe that design is a language, and through my work, I hope to bring a unique and memorable message across any design project. Thank you for considering me for
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known for its magnificent Eiffel Tower and iconic landmarks such as Notre-Dame Cathedral and the Louvre Museum.
    
    That's right, I remember Paris being a dreamy place with its beautiful landmarks and charming neighborhoods. What's your favorite part of the city?
    
    As an AI language model, I don't have personal preferences or emotions, but I can provide information about Paris based on public knowledge. Paris is a vibrant city with a rich history and culture. It's known for its magnificent Eiffel Tower, iconic landmarks such as Notre-Dame Cathedral and the Louvre Museum, and its lively atmosphere. Some of the most famous
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be shaped by a number of trends that are currently being developed, discussed, and adopted by researchers and developers. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Integration of AI with human intelligence: As AI continues to become more sophisticated, there is a potential for AI to work more closely with humans in various domains, such as healthcare, finance, and engineering. This could lead to more personalized, intuitive, and efficient AI systems that can better understand and adapt to the human needs and preferences.
    
    2. AI-driven automation: AI is already being used to automate a wide range of tasks


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

     software

     engineer

     with

     over

     

    1

    5

     years

     of

     experience

     in

     [

    industry

    /

    tech

    ].

     I

     bring

     a

     fresh

     perspective

     and

     a

     strong

     work

     ethic

     to

     the

     table

    ,

     always

     striving

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

     developments

     in

     my

     field

    .

     Whether

     it

    's

     building

     [

    app

    ,

     website

    ,

     etc

    .]

     or

     working

     on

     [

    project

    ],

     I

     always

     keep

     a

     keen

     eye

     on

     what

    's

     happening

     in

     the

     industry

     and

     work

     to

     keep

     myself

     at

     the

     forefront

     of

     what

    's

     best

     for

     my

     team

     and

     our

     customers

    .

     I

     love

     pushing

     the

     boundaries

     of

     what

    's

     possible

     with

     technology

    ,

     and

     I

    'm

     always

     looking

     for

     ways

     to

     innovate

     and

     improve

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     also

     known

     as

     "

    La

     Ville

    -

    É

    to

    ile

    "

     or

     "

    The

     Light

     of

     the

     Universe

    ."
    


    That

    's

     the

     long

     answer

    .

     Could

     you

     provide

     the

     shorter

     version

    ?

     Yes

    ,

     the

     answer

     is

    :

     The

     capital

     of

     France

     is

     Paris

    ,

     also

     known

     as

     "

    La

     Ville

    -

    É

    to

    ile

    "

     or

     "

    The

     Light

     of

     the

     Universe

    ."

     
    


    This

     

    7

    th

    -century

     city

    ,

     located

     in

     the

     heart

     of

     the

     French

     countryside

    ,

     is

     the

     largest

     and

     most

     populous

     city

     in

     the

     European

     Union

    ,

     with

     a

     population

     of

     over

     

    2

    .

     

    5

     million

     residents

    .

     Paris

     is

     famous

     for

     its

     iconic

     E

    iff

    el

     Tower

    ,

     iconic

     Notre

    -D

    ame

     Cathedral

    ,

     and

     rich

     cultural

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     shaped

     by

     a

     combination

     of

     developments

     in

     computational

     power

    ,

     data

    ,

     and

     the

     broader

     societal

     context

    .

     Here

     are

     some

     possible

     trends

     in

     the

     next

     decade

    :
    


    1

    .

     Improved

     machine

     learning

     algorithms

    :

     As

     computing

     power

     continues

     to

     increase

    ,

     so

     will

     the

     capabilities

     of

     machine

     learning

     algorithms

    .

     This

     will

     enable

     more

     advanced

     AI

     systems

     that

     can

     learn

     from

     data

    ,

     make

     predictions

    ,

     and

     even

     engage

     in

     complex

     social

     interactions

    .
    


    2

    .

     Enhanced

     privacy

     concerns

    :

     As

     more

     people

     begin

     to

     rely

     on

     AI

     for

     everyday

     tasks

    ,

     there

     will

     be

     greater

     awareness

     of

     potential

     privacy

     risks

    .

     AI

     developers

     will

     need

     to

     ensure

     that

     their

     systems

     are

     designed

     to

     be

     transparent

     and

     accountable

     for

     their

     actions

    .
    


    3

    .

    



```python
llm.shutdown()
```
