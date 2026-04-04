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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.21it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.20it/s]


    2026-04-04 03:59:48,971 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 03:59:48] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:32,  2.68s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:02<00:40,  1.36it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:02<00:40,  1.36it/s]Compiling num tokens (num_tokens=6144):   5%|▌         | 3/58 [00:02<00:40,  1.36it/s]Compiling num tokens (num_tokens=5632):   5%|▌         | 3/58 [00:02<00:40,  1.36it/s]Compiling num tokens (num_tokens=5120):   5%|▌         | 3/58 [00:02<00:40,  1.36it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:02<00:13,  3.89it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:02<00:13,  3.89it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:02<00:13,  3.89it/s]Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:02<00:13,  3.89it/s]Compiling num tokens (num_tokens=3584):  12%|█▏        | 7/58 [00:02<00:13,  3.89it/s]Compiling num tokens (num_tokens=3328):  12%|█▏        | 7/58 [00:02<00:13,  3.89it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:03<00:05,  7.84it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:03<00:05,  7.84it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:03<00:05,  7.84it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:03<00:05,  7.84it/s]Compiling num tokens (num_tokens=2304):  21%|██        | 12/58 [00:03<00:05,  7.84it/s]Compiling num tokens (num_tokens=2048):  21%|██        | 12/58 [00:03<00:05,  7.84it/s]Compiling num tokens (num_tokens=1792):  21%|██        | 12/58 [00:03<00:05,  7.84it/s]Compiling num tokens (num_tokens=1536):  21%|██        | 12/58 [00:03<00:05,  7.84it/s]

    Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:02, 14.48it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:02, 14.48it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:02, 14.48it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:02, 14.48it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:02, 14.48it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:02, 14.48it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:02, 14.48it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 20.17it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 20.17it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 20.17it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 20.17it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 20.17it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 20.17it/s]

    Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 20.17it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:03<00:01, 26.16it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:03<00:01, 26.16it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:03<00:01, 26.16it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:03<00:01, 26.16it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:03<00:01, 26.16it/s]Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:03<00:01, 26.16it/s]Compiling num tokens (num_tokens=256):  53%|█████▎    | 31/58 [00:03<00:01, 26.16it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:03<00:00, 31.63it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:03<00:00, 31.63it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:03<00:00, 31.63it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:03<00:00, 31.63it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:03<00:00, 31.63it/s]

    Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:03<00:00, 31.63it/s]Compiling num tokens (num_tokens=160):  64%|██████▍   | 37/58 [00:03<00:00, 31.63it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:03<00:00, 36.32it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:03<00:00, 36.32it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:03<00:00, 36.32it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:03<00:00, 36.32it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:03<00:00, 36.32it/s] Compiling num tokens (num_tokens=80):  74%|███████▍  | 43/58 [00:03<00:00, 36.32it/s]Compiling num tokens (num_tokens=64):  74%|███████▍  | 43/58 [00:03<00:00, 36.32it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 38.92it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 38.92it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 38.92it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 38.92it/s]

    Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 38.92it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 38.92it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 38.92it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 38.92it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:03<00:00, 45.54it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:03<00:00, 45.54it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:03<00:00, 45.54it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.14it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=131.67 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=131.64 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=131.64 GB):   3%|▎         | 2/58 [00:00<00:02, 19.03it/s]Capturing num tokens (num_tokens=7168 avail_mem=131.64 GB):   3%|▎         | 2/58 [00:00<00:02, 19.03it/s]Capturing num tokens (num_tokens=6656 avail_mem=131.64 GB):   3%|▎         | 2/58 [00:00<00:02, 19.03it/s]Capturing num tokens (num_tokens=6144 avail_mem=131.64 GB):   3%|▎         | 2/58 [00:00<00:02, 19.03it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=131.64 GB):   9%|▊         | 5/58 [00:00<00:02, 22.25it/s]Capturing num tokens (num_tokens=5632 avail_mem=131.63 GB):   9%|▊         | 5/58 [00:00<00:02, 22.25it/s]Capturing num tokens (num_tokens=5120 avail_mem=131.64 GB):   9%|▊         | 5/58 [00:00<00:02, 22.25it/s]Capturing num tokens (num_tokens=4608 avail_mem=131.63 GB):   9%|▊         | 5/58 [00:00<00:02, 22.25it/s]Capturing num tokens (num_tokens=4096 avail_mem=131.63 GB):   9%|▊         | 5/58 [00:00<00:02, 22.25it/s]Capturing num tokens (num_tokens=4096 avail_mem=131.63 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.05it/s]Capturing num tokens (num_tokens=3840 avail_mem=131.63 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.05it/s]Capturing num tokens (num_tokens=3584 avail_mem=131.62 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.05it/s]Capturing num tokens (num_tokens=3328 avail_mem=131.62 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.05it/s]Capturing num tokens (num_tokens=3072 avail_mem=131.62 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.05it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=131.62 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.05it/s]Capturing num tokens (num_tokens=2816 avail_mem=131.62 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.76it/s]Capturing num tokens (num_tokens=2560 avail_mem=131.61 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.76it/s]Capturing num tokens (num_tokens=2304 avail_mem=131.61 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.76it/s]Capturing num tokens (num_tokens=2048 avail_mem=131.61 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.76it/s]Capturing num tokens (num_tokens=1792 avail_mem=131.60 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.76it/s]Capturing num tokens (num_tokens=1536 avail_mem=131.60 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.76it/s]Capturing num tokens (num_tokens=1536 avail_mem=131.60 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.08it/s]Capturing num tokens (num_tokens=1280 avail_mem=131.59 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.08it/s]Capturing num tokens (num_tokens=1024 avail_mem=131.57 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.08it/s]Capturing num tokens (num_tokens=960 avail_mem=131.59 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.08it/s] Capturing num tokens (num_tokens=896 avail_mem=131.59 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.08it/s]

    Capturing num tokens (num_tokens=832 avail_mem=131.58 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.08it/s]Capturing num tokens (num_tokens=832 avail_mem=131.58 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.99it/s]Capturing num tokens (num_tokens=768 avail_mem=131.58 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.99it/s]Capturing num tokens (num_tokens=704 avail_mem=131.58 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.99it/s]Capturing num tokens (num_tokens=640 avail_mem=131.57 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.99it/s]Capturing num tokens (num_tokens=576 avail_mem=131.57 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.99it/s]Capturing num tokens (num_tokens=512 avail_mem=131.56 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.99it/s]Capturing num tokens (num_tokens=512 avail_mem=131.56 GB):  50%|█████     | 29/58 [00:00<00:00, 41.95it/s]Capturing num tokens (num_tokens=480 avail_mem=131.58 GB):  50%|█████     | 29/58 [00:00<00:00, 41.95it/s]Capturing num tokens (num_tokens=448 avail_mem=131.57 GB):  50%|█████     | 29/58 [00:00<00:00, 41.95it/s]Capturing num tokens (num_tokens=416 avail_mem=131.57 GB):  50%|█████     | 29/58 [00:00<00:00, 41.95it/s]

    Capturing num tokens (num_tokens=384 avail_mem=131.57 GB):  50%|█████     | 29/58 [00:00<00:00, 41.95it/s]Capturing num tokens (num_tokens=352 avail_mem=131.56 GB):  50%|█████     | 29/58 [00:01<00:00, 41.95it/s]Capturing num tokens (num_tokens=352 avail_mem=131.56 GB):  59%|█████▊    | 34/58 [00:01<00:00, 31.91it/s]Capturing num tokens (num_tokens=320 avail_mem=131.56 GB):  59%|█████▊    | 34/58 [00:01<00:00, 31.91it/s]Capturing num tokens (num_tokens=288 avail_mem=131.56 GB):  59%|█████▊    | 34/58 [00:01<00:00, 31.91it/s]

    Capturing num tokens (num_tokens=256 avail_mem=131.55 GB):  59%|█████▊    | 34/58 [00:01<00:00, 31.91it/s]Capturing num tokens (num_tokens=240 avail_mem=131.55 GB):  59%|█████▊    | 34/58 [00:01<00:00, 31.91it/s]Capturing num tokens (num_tokens=240 avail_mem=131.55 GB):  66%|██████▌   | 38/58 [00:01<00:00, 32.99it/s]Capturing num tokens (num_tokens=224 avail_mem=131.55 GB):  66%|██████▌   | 38/58 [00:01<00:00, 32.99it/s]Capturing num tokens (num_tokens=208 avail_mem=131.54 GB):  66%|██████▌   | 38/58 [00:01<00:00, 32.99it/s]Capturing num tokens (num_tokens=192 avail_mem=131.54 GB):  66%|██████▌   | 38/58 [00:01<00:00, 32.99it/s]Capturing num tokens (num_tokens=176 avail_mem=131.54 GB):  66%|██████▌   | 38/58 [00:01<00:00, 32.99it/s]Capturing num tokens (num_tokens=160 avail_mem=131.54 GB):  66%|██████▌   | 38/58 [00:01<00:00, 32.99it/s]Capturing num tokens (num_tokens=160 avail_mem=131.54 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.56it/s]Capturing num tokens (num_tokens=144 avail_mem=131.53 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.56it/s]Capturing num tokens (num_tokens=128 avail_mem=131.53 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.56it/s]Capturing num tokens (num_tokens=112 avail_mem=131.53 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.56it/s]

    Capturing num tokens (num_tokens=96 avail_mem=131.52 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.56it/s] Capturing num tokens (num_tokens=80 avail_mem=131.52 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.56it/s]Capturing num tokens (num_tokens=80 avail_mem=131.52 GB):  83%|████████▎ | 48/58 [00:01<00:00, 39.08it/s]Capturing num tokens (num_tokens=64 avail_mem=131.52 GB):  83%|████████▎ | 48/58 [00:01<00:00, 39.08it/s]Capturing num tokens (num_tokens=48 avail_mem=131.52 GB):  83%|████████▎ | 48/58 [00:01<00:00, 39.08it/s]Capturing num tokens (num_tokens=32 avail_mem=131.51 GB):  83%|████████▎ | 48/58 [00:01<00:00, 39.08it/s]Capturing num tokens (num_tokens=28 avail_mem=131.51 GB):  83%|████████▎ | 48/58 [00:01<00:00, 39.08it/s]Capturing num tokens (num_tokens=24 avail_mem=131.50 GB):  83%|████████▎ | 48/58 [00:01<00:00, 39.08it/s]Capturing num tokens (num_tokens=24 avail_mem=131.50 GB):  91%|█████████▏| 53/58 [00:01<00:00, 41.12it/s]Capturing num tokens (num_tokens=20 avail_mem=131.50 GB):  91%|█████████▏| 53/58 [00:01<00:00, 41.12it/s]Capturing num tokens (num_tokens=16 avail_mem=131.50 GB):  91%|█████████▏| 53/58 [00:01<00:00, 41.12it/s]Capturing num tokens (num_tokens=12 avail_mem=131.49 GB):  91%|█████████▏| 53/58 [00:01<00:00, 41.12it/s]

    Capturing num tokens (num_tokens=8 avail_mem=131.49 GB):  91%|█████████▏| 53/58 [00:01<00:00, 41.12it/s] Capturing num tokens (num_tokens=4 avail_mem=131.49 GB):  91%|█████████▏| 53/58 [00:01<00:00, 41.12it/s]Capturing num tokens (num_tokens=4 avail_mem=131.49 GB): 100%|██████████| 58/58 [00:01<00:00, 43.02it/s]Capturing num tokens (num_tokens=4 avail_mem=131.49 GB): 100%|██████████| 58/58 [00:01<00:00, 36.92it/s]


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
    Generated text:  Bob. I'm from the United States. I can speak English and Chinese. I like to play basketball. I'm very good at basketball. I have one basketball. Basketball is my favorite sport. I often play basketball with my friends. I like to read books, too. I have many books. My favorite book is 'The Wonderful Land of Gollum'. I read it every day. I also like to listen to music. I play the guitar. I can play the guitar very well. I like my music. I love my family very much. I have a happy family. I have a big house. There are five
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to decide whether to use a robot or a human president. He has been given the choice of 3 robots and 4 humans. He wants to maximize the satisfaction of the people. If a robot has a satisfaction score of 300 and a human has a satisfaction score of 200, what is the best choice for the president?
    
    To determine the best choice for the president, we need to calculate the total satisfaction score for each possible combination of robots and humans. The total satisfaction score is the sum of the satisfaction scores of the robot and the human.
    
    Given:
    - Satisfaction of a robot = 300
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. Its coordinates are (48.8646, 2.3522).
    I would like to know the coordinates of the capital of Italy.
    
    The capital of Italy is Rome. The coordinates of Rome are (41.9027, 13.4055). 
    
    What is the Python code to convert the coordinates of Paris to Rome? Please refer to the following table:
    | Country | Capital |
    |---------|---------|
    | France | Paris |
    | Italy   | Rome    |
    To convert the coordinates of Paris to Rome, you can use the following Python code:
    ```
    
    ===============================
    Prompt: The future of AI is
    Generated text:  rapidly evolving, but the real world is filled with complex and unpredictable challenges that require sophisticated models to make informed predictions. In this interactive session, participants will engage in a series of interactive activities and questions, which will test their understanding of AI principles and practical applications. The program concludes with a virtual Q&A session where attendees can ask questions and receive support from the team.
    This is a great opportunity for participants to learn about the latest developments in AI, understand the key principles and applications, and practice their problem-solving skills. The team at Microsoft is committed to providing high-quality and engaging content, and we are excited to share this program with our audience


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


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also the seat of the French government and the country's cultural and political capital. Paris is a bustling metropolis with a rich history and diverse culture, and it is a popular tourist destination. The city is known for its fashion, art, and cuisine, and it is a major hub for business and commerce. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly into one another. It is a city that is both old and new, and it is a city
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior and decision-making processes. This could lead to more sophisticated and adaptive AI systems that can learn from feedback and improve their performance over time.
    
    2. Enhanced privacy and security: As AI systems become more sophisticated, there will be an increased need for privacy and security measures to protect user data. This could lead to the development of new technologies and protocols for handling and protecting sensitive information.
    
    3. Greater automation and efficiency: AI is likely to become more automated and
    


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
    Generated text:  [First name], I'm an [Last name] who happens to be a [occupation]. I'm also an avid [thing you love to do] and I'm always eager to share my experiences and knowledge with others. I'm a [majority of character's] and I enjoy learning about new things and exploring different cultures. I'm passionate about [interest or hobby] and I'm always on the lookout for new opportunities to learn and grow. If you're interested, feel free to ask me anything, I'm here to answer. What can I tell you about myself? [Personal details]
    My character's name is [First
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as the "City of Love" and is known for its classical architecture and vibrant culture. Paris has a rich history dating back to the Roman era and is home to many iconic landmarks such as Notre Dame Cathedral, the Eiffel Tower, and Montmartre. The city is also famous for its fashion industry and has hosted many major fashion and sports events. Paris is a bustling and cosmopolitan city with a diverse population that is known for its art, music, and food. Its location in the heart of Europe and its beautiful parks have made it a popular tourist destination, attracting millions of visitors annually. The city's
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  rapidly changing, and there are several trends that are likely to shape the technology's future. Some of the most notable trends include:
    
    1. Increased automation: As machines become more advanced, they will become more and more capable of performing tasks that were once done by humans. This will lead to increased automation and the need for workers to be replaced by machines.
    
    2. AI ethics: The development of AI will continue to raise questions about the ethics of AI, such as who should have control over the AI, and what are the potential consequences of AI on society.
    
    3. Privacy concerns: As AI systems become more sophisticated, there will be increased


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

    field

     or

     profession

    ]

     with

     a

     passion

     for

     [

    career

     field

    ]

     that

     I

    'm

     dedicated

     to

     mastering

    .

     I

    'm

     [

    age

    ]

     years

     old

    ,

     and

     I

    'm

     an

     [

    h

    onest

    y

    ]

     person

     with

     a

     strong

     sense

     of

     [

    mot

    ivation

    ].

     I

     thrive

     on

     [

    task

     or

     challenge

    ]

     and

     am

     constantly

     seeking

     to

     [

    growth

     or

     improvement

    ].

     I

    'm

     always

     eager

     to

     learn

     and

     help

     others

     achieve

     their

     goals

    .

     I

    'm

     [

    character

     traits

    ]

     and

     I

    'm

     [

    type

     of

     character

    ].

     If

     you

    'd

     like

     to

     know

     more

     about

     me

     or

     where

     I

    'm

     from

    ,

     please

     feel

     free

     to

     ask

     me

    !

     [

    Include

     any

     other

     relevant

     information

     about

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     which

     is

     known

     as

     the

     "

    City

     of

     Love

    "

     due

     to

     its

     historical

     significance

     and

     romantic

     atmosphere

    .

     
    


    (Note

    :

     This

     statement

     is

     based

     on

     popular

     culture

     and

     media

    ,

     not

     factual

     history

    ,

     and

     may

     not

     reflect

     the

     actual

     historical

     facts

     about

     Paris

    .)

     
    


    Note

    :

     While

     the

     city

     itself

     has

     a

     rich

     cultural

     history

    ,

     the

     statement

     only

     focuses

     on

     its

     romantic

     reputation

     and

     may

     not

     accurately

     reflect

     the

     city

    's

     current

     physical

     and

     political

     conditions

    .

     Paris

     remains

     a

     vibrant

     and

     dynamic

     city

     with

     a

     diverse

     range

     of

     cultural

    ,

     artistic

    ,

     and

     social

     activities

    .

     Its

     iconic

     landmarks

     like

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

     attract

     millions

     of

     visitors

     each

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     uncertain

     and

     constantly

     evolving

    ,

     with

     many

     different

     trends

     and

     technologies

     emerging

     on

     a

     daily

     basis

    .

     Some

     potential

     trends

     that

     could

     shape

     the

     AI

     landscape

     include

    :
    


    1

    .

     Increased

     automation

     and

     the

     development

     of

     artificial

     general

     intelligence

    :

     AI

     has

     already

     achieved

     remarkable

     results

     in

     areas

     such

     as

     image

     recognition

     and

     natural

     language

     processing

    ,

     and

     it

     is

     expected

     that

     it

     will

     continue

     to

     grow

     in

     complexity

     and

     sophistication

    .

     With

     the

     development

     of

     more

     advanced

     algorithms

     and

     machine

     learning

     techniques

    ,

     it

     is

     possible

     that

     machines

     will

     be

     able

     to

     perform

     tasks

     that

     are

     currently

     done

     by

     humans

    ,

     including

     cognitive

     tasks

     such

     as

     decision

    -making

     and

     problem

    -solving

    .
    


    2

    .

     More

     integration

     of

     AI

     with

     other

     technologies

    :

     AI

     is

     already

     being

    



```python
llm.shutdown()
```
