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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.67it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.66it/s]


    2026-04-05 10:44:01,423 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-05 10:44:01] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 12.25it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 12.25it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:03, 12.25it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 12.25it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 12.25it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 12.25it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 12.25it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 12.25it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.47it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.47it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.47it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.47it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.47it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.47it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.47it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.47it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 25.24it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 25.24it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 25.24it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 25.24it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 25.24it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 25.24it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 25.24it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 30.35it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 30.35it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 30.35it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 30.35it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 30.35it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 30.35it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 30.35it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 32.25it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 32.25it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 32.25it/s]

    Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 32.25it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 32.25it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 32.25it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 32.25it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 35.82it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 35.82it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 35.82it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 35.82it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 35.82it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 35.82it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 35.82it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 35.82it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 35.82it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.75it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.05 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.99 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.99 GB):   3%|▎         | 2/58 [00:00<00:03, 18.49it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.99 GB):   3%|▎         | 2/58 [00:00<00:03, 18.49it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.99 GB):   3%|▎         | 2/58 [00:00<00:03, 18.49it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.99 GB):   3%|▎         | 2/58 [00:00<00:03, 18.49it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=118.99 GB):   9%|▊         | 5/58 [00:00<00:02, 21.78it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.98 GB):   9%|▊         | 5/58 [00:00<00:02, 21.78it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.98 GB):   9%|▊         | 5/58 [00:00<00:02, 21.78it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.98 GB):   9%|▊         | 5/58 [00:00<00:02, 21.78it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.98 GB):   9%|▊         | 5/58 [00:00<00:02, 21.78it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.98 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.98 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.97 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.97 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=118.97 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.97 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.58it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.97 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.33it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.96 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.33it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.96 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.33it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.95 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.33it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.95 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.33it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.95 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.33it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.95 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.76it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.94 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.76it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.92 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.76it/s]Capturing num tokens (num_tokens=960 avail_mem=118.94 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.76it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=118.93 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.76it/s]Capturing num tokens (num_tokens=832 avail_mem=118.93 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.76it/s]Capturing num tokens (num_tokens=832 avail_mem=118.93 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.89it/s]Capturing num tokens (num_tokens=768 avail_mem=118.93 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.89it/s]Capturing num tokens (num_tokens=704 avail_mem=118.92 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.89it/s]Capturing num tokens (num_tokens=640 avail_mem=118.92 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.89it/s]Capturing num tokens (num_tokens=576 avail_mem=118.92 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.89it/s]Capturing num tokens (num_tokens=512 avail_mem=118.91 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.89it/s]Capturing num tokens (num_tokens=512 avail_mem=118.91 GB):  50%|█████     | 29/58 [00:00<00:00, 41.33it/s]Capturing num tokens (num_tokens=480 avail_mem=118.92 GB):  50%|█████     | 29/58 [00:00<00:00, 41.33it/s]Capturing num tokens (num_tokens=448 avail_mem=118.92 GB):  50%|█████     | 29/58 [00:00<00:00, 41.33it/s]Capturing num tokens (num_tokens=416 avail_mem=118.92 GB):  50%|█████     | 29/58 [00:00<00:00, 41.33it/s]

    Capturing num tokens (num_tokens=384 avail_mem=118.92 GB):  50%|█████     | 29/58 [00:00<00:00, 41.33it/s]Capturing num tokens (num_tokens=352 avail_mem=118.91 GB):  50%|█████     | 29/58 [00:00<00:00, 41.33it/s]Capturing num tokens (num_tokens=352 avail_mem=118.91 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.93it/s]Capturing num tokens (num_tokens=320 avail_mem=118.91 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.93it/s]Capturing num tokens (num_tokens=288 avail_mem=118.90 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.93it/s]Capturing num tokens (num_tokens=256 avail_mem=118.90 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.93it/s]Capturing num tokens (num_tokens=240 avail_mem=118.90 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.93it/s]Capturing num tokens (num_tokens=224 avail_mem=118.90 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.93it/s]Capturing num tokens (num_tokens=224 avail_mem=118.90 GB):  67%|██████▋   | 39/58 [00:01<00:00, 44.05it/s]Capturing num tokens (num_tokens=208 avail_mem=118.89 GB):  67%|██████▋   | 39/58 [00:01<00:00, 44.05it/s]Capturing num tokens (num_tokens=192 avail_mem=118.89 GB):  67%|██████▋   | 39/58 [00:01<00:00, 44.05it/s]Capturing num tokens (num_tokens=176 avail_mem=118.89 GB):  67%|██████▋   | 39/58 [00:01<00:00, 44.05it/s]

    Capturing num tokens (num_tokens=160 avail_mem=118.89 GB):  67%|██████▋   | 39/58 [00:01<00:00, 44.05it/s]Capturing num tokens (num_tokens=144 avail_mem=118.85 GB):  67%|██████▋   | 39/58 [00:01<00:00, 44.05it/s]Capturing num tokens (num_tokens=144 avail_mem=118.85 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.96it/s]Capturing num tokens (num_tokens=128 avail_mem=118.85 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.96it/s]Capturing num tokens (num_tokens=112 avail_mem=118.84 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.96it/s]Capturing num tokens (num_tokens=96 avail_mem=118.84 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.96it/s] Capturing num tokens (num_tokens=80 avail_mem=118.84 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.96it/s]Capturing num tokens (num_tokens=64 avail_mem=118.83 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.96it/s]Capturing num tokens (num_tokens=64 avail_mem=118.83 GB):  84%|████████▍ | 49/58 [00:01<00:00, 45.30it/s]Capturing num tokens (num_tokens=48 avail_mem=118.83 GB):  84%|████████▍ | 49/58 [00:01<00:00, 45.30it/s]Capturing num tokens (num_tokens=32 avail_mem=118.82 GB):  84%|████████▍ | 49/58 [00:01<00:00, 45.30it/s]Capturing num tokens (num_tokens=28 avail_mem=118.82 GB):  84%|████████▍ | 49/58 [00:01<00:00, 45.30it/s]

    Capturing num tokens (num_tokens=24 avail_mem=118.82 GB):  84%|████████▍ | 49/58 [00:01<00:00, 45.30it/s]Capturing num tokens (num_tokens=20 avail_mem=118.81 GB):  84%|████████▍ | 49/58 [00:01<00:00, 45.30it/s]Capturing num tokens (num_tokens=20 avail_mem=118.81 GB):  93%|█████████▎| 54/58 [00:01<00:00, 45.86it/s]Capturing num tokens (num_tokens=16 avail_mem=118.81 GB):  93%|█████████▎| 54/58 [00:01<00:00, 45.86it/s]Capturing num tokens (num_tokens=12 avail_mem=118.81 GB):  93%|█████████▎| 54/58 [00:01<00:00, 45.86it/s]Capturing num tokens (num_tokens=8 avail_mem=118.81 GB):  93%|█████████▎| 54/58 [00:01<00:00, 45.86it/s] Capturing num tokens (num_tokens=4 avail_mem=118.80 GB):  93%|█████████▎| 54/58 [00:01<00:00, 45.86it/s]Capturing num tokens (num_tokens=4 avail_mem=118.80 GB): 100%|██████████| 58/58 [00:01<00:00, 40.44it/s]


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
    Generated text:  John, and I'm a friendly dog. I have a friendly name and a good personality. I was born in 2000 and have lived in different places around the world, but I can't remember exactly where I was born. I love to play and love to go outside. I love to run and play with my friends, and I love to eat lots of delicious treats. I also like to sleep, so I usually go to sleep early in the morning. Do you have a favorite pet or favorite animal? My favorite animal is a puppy. My favorite pet is a teddy bear. I love to cuddle with
    ===============================
    Prompt: The president of the United States is
    Generated text:  seeking to fill a vacancy in the position. The current president has a birth date of October 15, 1970. The new president is to be inaugurated on January 20, 2019. If the current president is 35 years old when inaugurated, what is the projected age of the new president when they take office?
    To determine the projected age of the new president when they take office, we need to follow these steps:
    
    1. Identify the current age of the president.
    2. Determine the age difference between the current president and the new president.
    3. Add the age difference
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, and the capital of the United Kingdom is London. Complete the following sentence:
    
    The capital of ________ is Paris, and the capital of ________ is London. The capital of France is Paris, and the capital of the United Kingdom is London.
    You are an AI assistant that helps you learn something new every day. Always provide answers summing up from the nine basic questions. Avoid creating making up the answers or the answers which are only based on coming up with answers. Try to avoid answering riddles in the sentence. Answer this question: what are the capital cities of the world? The capital cities of the world are Tokyo,
    ===============================
    Prompt: The future of AI is
    Generated text:  dark, but its use case is bright. At least that's what people say from the tech community.
    
    We have to acknowledge that AI technology is advancing at an incredible rate. But the potential benefits are huge, and it's not a mystery what it will do for the world.
    
    We've already seen how AI is making it easier to train artificial intelligence to perform tasks that were previously the domain of humans. That includes working in hospitals, manufacturing plants, and even in fields like law enforcement.
    
    As AI continues to grow, it's clear that it will play a major role in industries and industries across the globe.
    
    But as we move forward,


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


    Generated text:  Paris, which is known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also home to the French Parliament building and the French Quarter, a historic district known for its narrow streets and charming architecture. Paris is a bustling city with a rich cultural heritage and is a popular tourist destination. It is also known for its fashion industry and has a long history of artistic and literary contributions. The city is home to many famous museums, including the Louvre and the Musée d'Orsay. Paris is a vibrant and dynamic city that is a must-visit destination for anyone interested
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that could emerge in the coming years:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes, reduce costs, and increase efficiency. As AI technology continues to improve, we can expect to see even more widespread use of AI in healthcare, with more sophisticated algorithms and machine learning techniques being developed to improve diagnosis, treatment, and patient care.
    
    2. AI in finance: AI is already being used in finance to improve risk management, fraud detection
    


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
    Generated text:  [insert name], and I'm a [insert occupation or profession] with a passion for [insert something specific that you enjoy doing]. I love taking risks and experimenting with new things, and I'm always looking for ways to learn and grow. I believe in the power of creativity and innovation to make the world a better place, and I'm always striving to do my best work. I'm excited to meet you and learn more about you. Please let me know if you're ready to connect. [Insert any additional information or context that you would like to include in the self-introduction.]. 
    Example:
    Hello, my name is
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    Paris is the largest city in France and the seat of the government. It is a historical and cultural center of France, known for its architecture, art, and cuisine. Paris is also a major business hub and has a world-renowned film industry. The city is home to many famous landmarks such as the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Palace of Versailles. Paris has a population of around 2. 4 million people and is the fifth-most populous city in the world. The city is also home to many cultural institutions, such as the Louvre Museum, the Sorbon
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  full of exciting possibilities and challenges. Here are some possible trends that are likely to shape the AI landscape:
    
    1. Increased automation: With the rise of automation in industries such as manufacturing, finance, and healthcare, AI is likely to become more prevalent in these roles. This will result in increased efficiency and productivity.
    
    2. Human-like AI: As AI technology continues to advance, it is possible that we will see more AI systems that exhibit human-like behavior and cognitive functions. This could lead to new innovations in fields such as psychology and psychology.
    
    3. AI ethics and privacy concerns: As AI systems become more integrated into our daily lives, there


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

     software

     engineer

     with

     a

     passion

     for

     technology

     and

     innovation

    .

     I

    'm

     passionate

     about

     creating

     solutions

     that

     make

     people

    's

     lives

     easier

     and

     more

     efficient

    .

     I

     love

     spending

     my

     free

     time

     building

     cool

     things

     and

     pursuing

     my

     interests

     in

     design

     and

     photography

    .

     I

    'm

     excited

     to

     contribute

     to

     the

     growth

     of

     the

     tech

     industry

     and

     help

     people

     find

     solutions

     to

     their

     problems

    .

     
    


    Remember

     to

     use

     a

     neutral

    ,

     objective

    ,

     and

     respectful

     tone

     throughout

     your

     introduction

    ,

     and

     to

     include

     a

     brief

     bi

    ographical

     sketch

     of

     yourself

    ,

     if

     available

    .

     Remember

     to

     highlight

     any

     relevant

     skills

    ,

     experiences

    ,

     or

     achievements

     that

     set

     you

     apart

     from

     other

     potential

     candidates

    .
    


    Additionally

    ,

     ensure

     that

     your

     self

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .


    That

     is

     the

     official

     capital

     of

     France

     and

     one

     of

     its

     most

     important

     cities

    .

     It

     is

     located

     in

     the

     south

     of

     the

     country

    ,

     near

     the

     Mediterranean

     Sea

    .

     The

     city

     is

     home

     to

     the

     oldest

     continuously

     inhabited

     community

     in

     the

     world

    ,

     the

     Latin

     Quarter

    ,

     and

     has

     been

     a

     center

     of

     French

     culture

     since

     the

     

    1

    4

    th

     century

    .

     Paris

     is also

     known

     for

     its

     many

     museums

    ,

     theaters

    ,

     and

     landmarks

    ,

     including

     the

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

     Notre

    -D

    ame

     Cathedral

    .

     It

     has

     played

     an

     important

     role

     in

     the

     history

     and

     culture

     of

     France

     for

     centuries

     and

     continues

     to

     be

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

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     involve

     an

     ever

    -in

    creasing

     range

     of

     capabilities

     and

     applications

     as

     it

     continues

     to

     evolve

    .

     Some

     possible

     future

     trends

     in

     AI

     include

    :
    


    1

    .

     Increased

     accuracy

     and

     precision

    :

     AI

     is

     getting

     better

     at

     understanding

     and

     interpreting

     data

    ,

     which

     is

     leading

     to

     greater

     accuracy

     and

     precision

     in

     many

     applications

    .
    


    2

    .

     Greater

     flexibility

    :

     AI

     systems

     are

     becoming

     more

     adaptable

     and

     capable

     of

     learning

     and

     responding

     to

     new

     situations

     quickly

     and

     efficiently

    .
    


    3

    .

     Greater

     integration

     with

     human

     decision

    -making

    :

     AI

     is

     becoming

     more

     integrated

     with

     human

     decision

    -making

     processes

    ,

     allowing

     for

     more

     complex

     and

     nuanced

     solutions

    .
    


    4

    .

     Greater

     reliance

     on

     AI

     in

     healthcare

    :

     AI

     is

     being

     used

     in

     healthcare

     to

     improve

     patient

     outcomes

    ,

     reduce

    



```python
llm.shutdown()
```
