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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.94it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.93it/s]


    2026-04-03 21:21:48,096 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 21:21:48] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:31,  2.65s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:31,  2.65s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:31,  2.65s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:31,  2.65s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.79it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 12.22it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 12.22it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:03, 12.22it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 12.22it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 12.22it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 12.22it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 12.22it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 12.22it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.32it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.32it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.32it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.32it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.32it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.32it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.32it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.32it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.95it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.95it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.95it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.95it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.95it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.95it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.95it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.87it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.87it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.87it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.87it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.87it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.87it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.87it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.12it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.12it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.12it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.12it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.12it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.12it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.12it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.15it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.15it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.15it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.15it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.15it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.15it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.15it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.15it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.15it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 47.12it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.90it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.34 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.31 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.31 GB):   3%|▎         | 2/58 [00:00<00:03, 18.03it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.03it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.03it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.03it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.57it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.57it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.57it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.57it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.57it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.18it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.18it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.18it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.18it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.18it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.28 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.55it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.28 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.55it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.55it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.55it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.55it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.55it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  31%|███       | 18/58 [00:00<00:01, 35.11it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  31%|███       | 18/58 [00:00<00:01, 35.11it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  31%|███       | 18/58 [00:00<00:01, 35.11it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  31%|███       | 18/58 [00:00<00:01, 35.11it/s]

    Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  31%|███       | 18/58 [00:00<00:01, 35.11it/s] Capturing num tokens (num_tokens=896 avail_mem=120.25 GB):  31%|███       | 18/58 [00:00<00:01, 35.11it/s]Capturing num tokens (num_tokens=896 avail_mem=120.25 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.98it/s]Capturing num tokens (num_tokens=832 avail_mem=120.28 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.98it/s]Capturing num tokens (num_tokens=768 avail_mem=120.27 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.98it/s]Capturing num tokens (num_tokens=704 avail_mem=120.27 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.98it/s]Capturing num tokens (num_tokens=640 avail_mem=119.05 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.98it/s]Capturing num tokens (num_tokens=576 avail_mem=118.95 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.98it/s]Capturing num tokens (num_tokens=576 avail_mem=118.95 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.57it/s]Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.57it/s]Capturing num tokens (num_tokens=480 avail_mem=118.95 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.57it/s]

    Capturing num tokens (num_tokens=448 avail_mem=118.95 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.57it/s]Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.57it/s]Capturing num tokens (num_tokens=384 avail_mem=118.95 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.57it/s]Capturing num tokens (num_tokens=384 avail_mem=118.95 GB):  57%|█████▋    | 33/58 [00:00<00:00, 40.77it/s]Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  57%|█████▋    | 33/58 [00:00<00:00, 40.77it/s]Capturing num tokens (num_tokens=320 avail_mem=118.94 GB):  57%|█████▋    | 33/58 [00:00<00:00, 40.77it/s]Capturing num tokens (num_tokens=288 avail_mem=118.94 GB):  57%|█████▋    | 33/58 [00:00<00:00, 40.77it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  57%|█████▋    | 33/58 [00:01<00:00, 40.77it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  57%|█████▋    | 33/58 [00:01<00:00, 40.77it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.74it/s]Capturing num tokens (num_tokens=224 avail_mem=118.93 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.74it/s]

    Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.74it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.74it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.74it/s]Capturing num tokens (num_tokens=160 avail_mem=118.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.74it/s]Capturing num tokens (num_tokens=160 avail_mem=118.92 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.58it/s]Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.58it/s]Capturing num tokens (num_tokens=128 avail_mem=118.91 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.58it/s]Capturing num tokens (num_tokens=112 avail_mem=118.91 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.58it/s]Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.58it/s] Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  74%|███████▍  | 43/58 [00:01<00:00, 42.58it/s]

    Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  83%|████████▎ | 48/58 [00:01<00:00, 42.76it/s]Capturing num tokens (num_tokens=64 avail_mem=118.89 GB):  83%|████████▎ | 48/58 [00:01<00:00, 42.76it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  83%|████████▎ | 48/58 [00:01<00:00, 42.76it/s]Capturing num tokens (num_tokens=32 avail_mem=118.89 GB):  83%|████████▎ | 48/58 [00:01<00:00, 42.76it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  83%|████████▎ | 48/58 [00:01<00:00, 42.76it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  83%|████████▎ | 48/58 [00:01<00:00, 42.76it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.02it/s]Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.02it/s]Capturing num tokens (num_tokens=16 avail_mem=118.87 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.02it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.02it/s]Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.02it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=118.86 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.02it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:01<00:00, 43.68it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:01<00:00, 38.70it/s]


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
    Generated text:  Thomas, and I am a proud and grateful person. It's my privilege to be a part of this community, and I am excited to share a little bit of my life here with you.
    
    I was born in 1979 in Manitoba, Canada, where I was raised. I grew up with a strong sense of identity and a deep appreciation for the diverse experiences of my people. I attended public schools and have been a member of a church for many years, both in my home city and beyond.
    
    My life has been marked by hard work and a commitment to improving my community. I have worked in various capacities, including as
    ===============================
    Prompt: The president of the United States is
    Generated text:  seeking donations for a fundraising campaign to improve public transportation in his state. The campaign consists of raising $800 in 30 days. Each day, he aims to make donations in multiples of 10. To achieve his goal, he decides to donate a base amount of $10 per day and an additional $2 per day. Calculate the total amount of donations he needs to make in the first 7 days to reach his goal.
    To determine the total amount of donations the president needs to make in the first 7 days to reach his goal, we start by defining the base amount of donations per day and then calculate
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, the capital of the United Kingdom is London and the capital of the United States is Washington D.C. The capital of Israel is Jerusalem. But how did they get there? In 1917, the Jewish people fought against the Ottoman Turks in the First World War. They were finally victorious and won control of Palestine and had their capital in Jerusalem. For the next 30 years, they fought against the British Empire. They were finally defeated and the city was bombed. They were finally victorious in 1948 and won the city back to them. But the city was not the only thing that was changed.
    ===============================
    Prompt: The future of AI is
    Generated text:  in human hands. Some people view AI as a positive force, seeing it as a tool for betterment of our lives, while others see it as a threat. In this article, we will explore the potential impact of AI on society and the individuals using it.
    Potential impact of AI on society:
    1. Advancing technology: AI has the potential to transform the way we live and work. It can automate tasks, improve efficiency, and reduce costs. This can lead to increased productivity and better outcomes for individuals and businesses.
    2. Social equity: AI can also exacerbate social inequality if not managed properly. For example, if AI is


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


    Generated text:  Paris, also known as the City of Light, a historic city with a rich history and culture. It is located in the south of France and is the largest city in the country. Paris is known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Arc de Triomphe. The city is also famous for its cuisine, fashion, and art scene. Paris is a major tourist destination and is home to many world-renowned museums, theaters, and landmarks. It is a city that has played a significant role in French history and continues to be a cultural and economic center of
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased integration with human intelligence: As AI becomes more sophisticated, it is likely to become more integrated with human intelligence, allowing it to learn and adapt in ways that are difficult for humans to do. This could lead to more efficient and effective decision-making, as well as more personalized and context-aware interactions with humans.
    
    2. Greater emphasis on ethical considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical considerations. This could lead to more
    


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
    Generated text:  [Name] and I am a [type of character]! I am [Age] years old and I have always been [specific characteristic] in my life. I am a [job role] at [company name] and I have worked in [specific position] for [number of years] years. I have a passion for [occupation], and I am always ready to learn new things and expand my horizons. I am [good at] [specific skill or ability], and I am committed to [reason for being]. I enjoy [career development goals] and I am excited to continue exploring new opportunities. Thank you! Congratulations
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    This answer is only 33 characters long and directly states the capital city of France. It's a concise factual statement about the capital city. 
    
    Please let me know if you need anything else. I'm here to assist you. 😊✨✨
    
    Have a great day! 📅✨✨
    
    Here's a French translation:
    
    "La capitale de la France est Paris." 
    *Note: The statement is only 33 characters long. It's a concise factual statement about the capital city, which is Paris in French.*
    
    This French translation preserves the meaning of the original statement. Let me know if
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to involve a number of trends and developments in the coming years. Here are some possible trends that are likely to shape the future of AI:
    
    1. Increased use of AI for healthcare: AI can be used to improve the accuracy and efficiency of medical diagnosis and treatment, as well as to assist with patient care and medication management.
    
    2. Development of AI for autonomous vehicles: With the increasing number of road accidents caused by human error, AI is likely to play a key role in the development of autonomous vehicles that can safely navigate highways and other public spaces.
    
    3. AI in the education sector: AI is likely to play a more significant role


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

    name

    ],

     and

     I

    'm

     a

     [

    职业

    ]

     at

     [

    company

    ].

     I

    've

     been

     working

     at

     [

    company

    ]

     since

     [

    start

     year

    ]

     and

     I

    'm

     currently

     [

    job

     title

    ].

     I

     enjoy

     [

    your

     passion

     or

     hobby

    ],

     and

     I

     have

     a

     strong

     work

     ethic

     and

     a

     great

     attitude

    .

     I

    'm

     always

     looking

     for

     ways

     to

     improve

     my

     skills

     and

     I

    'm

     always

     eager

     to

     learn

     new

     things

    .

     I

    'm

     always

     open

     to

     new

     experiences

     and

     am

     always

     looking

     for

     opportunities

     to

     grow

     and

     develop

     my

     skills

    .

     I

     believe

     in

     [

    reason

     why

     you

     believe

     in

     yourself

    ],

     and

     I

    'm

     always

     willing

     to

     help

     others

     and

     share

     my

     knowledge

     and

     experience

    .

     Thank

     you

     for

     asking

    !

     Let

     me

    
    
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

    ,

     located

     in

     the

     Lo

    ire

     Valley

     on

     the

     French

     Riv

    iera

    .

     It

     is

     home

     to

     many

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

     Notre

    -D

    ame

     Cathedral

    ,

     and

     Lou

    vre

     Museum

    .

     The

     city

     is

     also

     known

     for

     its

     rich

     history

    ,

     including

     the

     birth

    place

     of

     Napoleon

     Bon

    ap

    arte

    ,

     and

     its

     numerous

     museums

     and

     art

     galleries

    .

     Paris

     has

     a

     diverse

     population

     of

     over

     

    1

     million

     people

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

     The

     French

     capital

     is

     a

     must

    -

    visit

     destination

     for

     anyone

     looking

     to

     experience

     the

     city

    's

     cultural

     and

     historical

     attractions

    .

     Is

     there

     anything

     else

     you

     would

     like

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     involve

     a

     growing

     focus

     on

     developing

     more

     advanced

     and

     intelligent

     systems

     that

     can

     perform

     a

     wide

     range

     of

     tasks

    ,

     from

     tasks

     that

     are

     currently

     performed

     by

     humans

    ,

     such

     as

     playing

     video

     games

     or

     driving

     cars

    ,

     to

     tasks

     that

     involve

     complex

     decision

    -making

     and

     problem

    -solving

    ,

     such

     as

     diagn

    osing

     medical

     conditions

     or

     predicting

     economic

     trends

    .

     Additionally

    ,

     there

     is

     a

     growing

     emphasis

     on

     ethical

     considerations

     and

     the

     responsible

     use

     of

     AI

    ,

     including

     issues

     related

     to

     data

     privacy

    ,

     bias

    ,

     and

     transparency

    .

     Finally

    ,

     the

     integration

     of

     AI

     with

     other

     technologies

    ,

     such

     as

     robotics

     and

     sensors

    ,

     is

     expected

     to

     drive

     further

     advances

     in

     the

     field

     and

     open

     up

     new

     possibilities

     for

     applications

     that

     were

     previously

     impossible

    .

    



```python
llm.shutdown()
```
