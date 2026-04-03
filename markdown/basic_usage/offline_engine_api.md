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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.33it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.32it/s]


    2026-04-03 21:06:03,512 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 21:06:03] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:24,  2.53s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:24,  2.53s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:24,  2.53s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:24,  2.53s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:27,  1.97it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:27,  1.97it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:27,  1.97it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:27,  1.97it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:27,  1.97it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:27,  1.97it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:27,  1.97it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.03it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.03it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.03it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.03it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.03it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.03it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.03it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.03it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.03it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:07,  6.03it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.52it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.52it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.52it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.52it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.52it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.52it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.52it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 13.52it/s]Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:02<00:02, 13.52it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:02<00:01, 20.89it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:02<00:01, 20.89it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:02<00:01, 20.89it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 20.89it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 20.89it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 20.89it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 20.89it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:03<00:01, 20.89it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:03<00:00, 27.30it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:03<00:00, 27.30it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:03<00:00, 27.30it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:00, 27.30it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:00, 27.30it/s]

    Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:03<00:00, 27.30it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:03<00:00, 27.30it/s]Compiling num tokens (num_tokens=192):  59%|█████▊    | 34/58 [00:03<00:00, 27.30it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:03<00:00, 33.35it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:03<00:00, 33.35it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:03<00:00, 33.35it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:03<00:00, 33.35it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:03<00:00, 33.35it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:03<00:00, 33.35it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:03<00:00, 33.35it/s] Compiling num tokens (num_tokens=80):  71%|███████   | 41/58 [00:03<00:00, 33.35it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:03<00:00, 39.81it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:03<00:00, 39.81it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:03<00:00, 39.81it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:03<00:00, 39.81it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:03<00:00, 39.81it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:03<00:00, 39.81it/s]

    Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:03<00:00, 39.81it/s]Compiling num tokens (num_tokens=16):  83%|████████▎ | 48/58 [00:03<00:00, 39.81it/s]Compiling num tokens (num_tokens=12):  83%|████████▎ | 48/58 [00:03<00:00, 39.81it/s]Compiling num tokens (num_tokens=8):  83%|████████▎ | 48/58 [00:03<00:00, 39.81it/s] Compiling num tokens (num_tokens=4):  83%|████████▎ | 48/58 [00:03<00:00, 39.81it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 52.02it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.04it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=40.76 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=40.76 GB):   2%|▏         | 1/58 [00:00<00:08,  6.99it/s]Capturing num tokens (num_tokens=7680 avail_mem=40.63 GB):   2%|▏         | 1/58 [00:00<00:08,  6.99it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=40.63 GB):   3%|▎         | 2/58 [00:00<00:07,  7.04it/s]Capturing num tokens (num_tokens=7168 avail_mem=40.63 GB):   3%|▎         | 2/58 [00:00<00:07,  7.04it/s]Capturing num tokens (num_tokens=7168 avail_mem=40.63 GB):   5%|▌         | 3/58 [00:00<00:07,  7.33it/s]Capturing num tokens (num_tokens=6656 avail_mem=40.63 GB):   5%|▌         | 3/58 [00:00<00:07,  7.33it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=40.63 GB):   5%|▌         | 3/58 [00:00<00:07,  7.33it/s]Capturing num tokens (num_tokens=6144 avail_mem=40.63 GB):   9%|▊         | 5/58 [00:00<00:04, 10.80it/s]Capturing num tokens (num_tokens=5632 avail_mem=40.62 GB):   9%|▊         | 5/58 [00:00<00:04, 10.80it/s]Capturing num tokens (num_tokens=5120 avail_mem=40.62 GB):   9%|▊         | 5/58 [00:00<00:04, 10.80it/s]Capturing num tokens (num_tokens=5120 avail_mem=40.62 GB):  12%|█▏        | 7/58 [00:00<00:04, 11.79it/s]Capturing num tokens (num_tokens=4608 avail_mem=40.62 GB):  12%|█▏        | 7/58 [00:00<00:04, 11.79it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=40.62 GB):  12%|█▏        | 7/58 [00:00<00:04, 11.79it/s]Capturing num tokens (num_tokens=4096 avail_mem=40.62 GB):  16%|█▌        | 9/58 [00:00<00:04, 10.79it/s]Capturing num tokens (num_tokens=3840 avail_mem=40.62 GB):  16%|█▌        | 9/58 [00:00<00:04, 10.79it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=40.61 GB):  16%|█▌        | 9/58 [00:00<00:04, 10.79it/s]Capturing num tokens (num_tokens=3584 avail_mem=40.61 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.66it/s]Capturing num tokens (num_tokens=3328 avail_mem=40.61 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.66it/s]Capturing num tokens (num_tokens=3072 avail_mem=40.60 GB):  19%|█▉        | 11/58 [00:01<00:04, 10.66it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=40.60 GB):  22%|██▏       | 13/58 [00:01<00:03, 11.92it/s]Capturing num tokens (num_tokens=2816 avail_mem=40.60 GB):  22%|██▏       | 13/58 [00:01<00:03, 11.92it/s]Capturing num tokens (num_tokens=2560 avail_mem=40.60 GB):  22%|██▏       | 13/58 [00:01<00:03, 11.92it/s]Capturing num tokens (num_tokens=2304 avail_mem=40.60 GB):  22%|██▏       | 13/58 [00:01<00:03, 11.92it/s]Capturing num tokens (num_tokens=2048 avail_mem=40.59 GB):  22%|██▏       | 13/58 [00:01<00:03, 11.92it/s]Capturing num tokens (num_tokens=2048 avail_mem=40.59 GB):  29%|██▉       | 17/58 [00:01<00:02, 17.89it/s]Capturing num tokens (num_tokens=1792 avail_mem=40.59 GB):  29%|██▉       | 17/58 [00:01<00:02, 17.89it/s]Capturing num tokens (num_tokens=1536 avail_mem=40.59 GB):  29%|██▉       | 17/58 [00:01<00:02, 17.89it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=40.58 GB):  29%|██▉       | 17/58 [00:01<00:02, 17.89it/s]Capturing num tokens (num_tokens=1280 avail_mem=40.58 GB):  34%|███▍      | 20/58 [00:01<00:02, 16.85it/s]Capturing num tokens (num_tokens=1024 avail_mem=40.56 GB):  34%|███▍      | 20/58 [00:01<00:02, 16.85it/s]Capturing num tokens (num_tokens=960 avail_mem=40.57 GB):  34%|███▍      | 20/58 [00:01<00:02, 16.85it/s] Capturing num tokens (num_tokens=960 avail_mem=40.57 GB):  38%|███▊      | 22/58 [00:01<00:02, 16.83it/s]Capturing num tokens (num_tokens=896 avail_mem=40.57 GB):  38%|███▊      | 22/58 [00:01<00:02, 16.83it/s]

    Capturing num tokens (num_tokens=832 avail_mem=40.57 GB):  38%|███▊      | 22/58 [00:01<00:02, 16.83it/s]Capturing num tokens (num_tokens=832 avail_mem=40.57 GB):  41%|████▏     | 24/58 [00:01<00:01, 17.25it/s]Capturing num tokens (num_tokens=768 avail_mem=40.56 GB):  41%|████▏     | 24/58 [00:01<00:01, 17.25it/s]Capturing num tokens (num_tokens=704 avail_mem=40.56 GB):  41%|████▏     | 24/58 [00:01<00:01, 17.25it/s]Capturing num tokens (num_tokens=704 avail_mem=40.56 GB):  45%|████▍     | 26/58 [00:01<00:01, 17.27it/s]Capturing num tokens (num_tokens=640 avail_mem=40.56 GB):  45%|████▍     | 26/58 [00:01<00:01, 17.27it/s]

    Capturing num tokens (num_tokens=576 avail_mem=40.56 GB):  45%|████▍     | 26/58 [00:01<00:01, 17.27it/s]Capturing num tokens (num_tokens=512 avail_mem=40.55 GB):  45%|████▍     | 26/58 [00:01<00:01, 17.27it/s]Capturing num tokens (num_tokens=480 avail_mem=40.56 GB):  45%|████▍     | 26/58 [00:01<00:01, 17.27it/s]Capturing num tokens (num_tokens=448 avail_mem=40.56 GB):  45%|████▍     | 26/58 [00:01<00:01, 17.27it/s]Capturing num tokens (num_tokens=448 avail_mem=40.56 GB):  53%|█████▎    | 31/58 [00:01<00:01, 24.66it/s]Capturing num tokens (num_tokens=416 avail_mem=40.56 GB):  53%|█████▎    | 31/58 [00:01<00:01, 24.66it/s]Capturing num tokens (num_tokens=384 avail_mem=40.56 GB):  53%|█████▎    | 31/58 [00:01<00:01, 24.66it/s]Capturing num tokens (num_tokens=352 avail_mem=40.55 GB):  53%|█████▎    | 31/58 [00:02<00:01, 24.66it/s]Capturing num tokens (num_tokens=320 avail_mem=40.55 GB):  53%|█████▎    | 31/58 [00:02<00:01, 24.66it/s]Capturing num tokens (num_tokens=288 avail_mem=40.54 GB):  53%|█████▎    | 31/58 [00:02<00:01, 24.66it/s]Capturing num tokens (num_tokens=256 avail_mem=40.54 GB):  53%|█████▎    | 31/58 [00:02<00:01, 24.66it/s]Capturing num tokens (num_tokens=256 avail_mem=40.54 GB):  64%|██████▍   | 37/58 [00:02<00:00, 32.74it/s]Capturing num tokens (num_tokens=240 avail_mem=40.54 GB):  64%|██████▍   | 37/58 [00:02<00:00, 32.74it/s]

    Capturing num tokens (num_tokens=224 avail_mem=40.54 GB):  64%|██████▍   | 37/58 [00:02<00:00, 32.74it/s]Capturing num tokens (num_tokens=208 avail_mem=40.53 GB):  64%|██████▍   | 37/58 [00:02<00:00, 32.74it/s]Capturing num tokens (num_tokens=192 avail_mem=40.53 GB):  64%|██████▍   | 37/58 [00:02<00:00, 32.74it/s]Capturing num tokens (num_tokens=176 avail_mem=40.53 GB):  64%|██████▍   | 37/58 [00:02<00:00, 32.74it/s]Capturing num tokens (num_tokens=160 avail_mem=40.52 GB):  64%|██████▍   | 37/58 [00:02<00:00, 32.74it/s]Capturing num tokens (num_tokens=160 avail_mem=40.52 GB):  74%|███████▍  | 43/58 [00:02<00:00, 38.73it/s]Capturing num tokens (num_tokens=144 avail_mem=40.52 GB):  74%|███████▍  | 43/58 [00:02<00:00, 38.73it/s]Capturing num tokens (num_tokens=128 avail_mem=40.52 GB):  74%|███████▍  | 43/58 [00:02<00:00, 38.73it/s]Capturing num tokens (num_tokens=112 avail_mem=40.52 GB):  74%|███████▍  | 43/58 [00:02<00:00, 38.73it/s]Capturing num tokens (num_tokens=96 avail_mem=40.51 GB):  74%|███████▍  | 43/58 [00:02<00:00, 38.73it/s] Capturing num tokens (num_tokens=80 avail_mem=40.51 GB):  74%|███████▍  | 43/58 [00:02<00:00, 38.73it/s]Capturing num tokens (num_tokens=64 avail_mem=40.50 GB):  74%|███████▍  | 43/58 [00:02<00:00, 38.73it/s]

    Capturing num tokens (num_tokens=64 avail_mem=40.50 GB):  84%|████████▍ | 49/58 [00:02<00:00, 42.72it/s]Capturing num tokens (num_tokens=48 avail_mem=40.50 GB):  84%|████████▍ | 49/58 [00:02<00:00, 42.72it/s]Capturing num tokens (num_tokens=32 avail_mem=40.50 GB):  84%|████████▍ | 49/58 [00:02<00:00, 42.72it/s]Capturing num tokens (num_tokens=28 avail_mem=40.49 GB):  84%|████████▍ | 49/58 [00:02<00:00, 42.72it/s]Capturing num tokens (num_tokens=24 avail_mem=40.49 GB):  84%|████████▍ | 49/58 [00:02<00:00, 42.72it/s]Capturing num tokens (num_tokens=20 avail_mem=40.49 GB):  84%|████████▍ | 49/58 [00:02<00:00, 42.72it/s]Capturing num tokens (num_tokens=20 avail_mem=40.49 GB):  93%|█████████▎| 54/58 [00:02<00:00, 36.50it/s]Capturing num tokens (num_tokens=16 avail_mem=40.49 GB):  93%|█████████▎| 54/58 [00:02<00:00, 36.50it/s]

    Capturing num tokens (num_tokens=12 avail_mem=40.48 GB):  93%|█████████▎| 54/58 [00:02<00:00, 36.50it/s]Capturing num tokens (num_tokens=8 avail_mem=40.48 GB):  93%|█████████▎| 54/58 [00:02<00:00, 36.50it/s] Capturing num tokens (num_tokens=4 avail_mem=40.48 GB):  93%|█████████▎| 54/58 [00:02<00:00, 36.50it/s]Capturing num tokens (num_tokens=4 avail_mem=40.48 GB): 100%|██████████| 58/58 [00:02<00:00, 30.54it/s]Capturing num tokens (num_tokens=4 avail_mem=40.48 GB): 100%|██████████| 58/58 [00:02<00:00, 21.56it/s]


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
    Generated text:  Adam and I'm a PhD student at the University of Hong Kong. I'm currently working on a project to develop an efficient algorithm to solve the traveling salesman problem. Can you tell me about it? Sure, my project focuses on developing an algorithm to find the shortest possible path between all the cities in a given network. The traveling salesman problem (TSP) is one of the most well-known optimization problems in operations research and has important applications in logistics, scheduling, and machine learning. To solve this problem, we need a better algorithm that can find the optimal solution to the problem. I'm currently working on developing an algorithm to find the
    ===============================
    Prompt: The president of the United States is
    Generated text:  a three (3) year term and each term begins on the first (1) of January (1). The president of the United States has the authority to choose the first (1) state (3) senator. (3) Senator is a position in the United States Congress. (3) Senator is the first (1) political office that each President of the United States holds. (3) Senator is the first (1) political office in which an individual may vote (3) for the President of the United States. (3) Senator is the first (1) political office in which an individual may campaign (3)
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, a major city and capital of the country. It is located in the south of the country, on the banks of the river Seine. It is surrounded by the country's second largest city, Lyon, to the west; and by the Loire River, to the north. The city is located between the 45th parallel south and 47th parallel north. It is the largest French city, with an area of 948 km² and a population of 2.6 million people.
    What is the population of Paris? (If the question is unanswerable, say "unanswerable)
    ===============================
    Prompt: The future of AI is
    Generated text:  not just in the software but also in the hardware. The development of next-generation AI models, from small quantum computers to large-scale neural networks, is reshaping the landscape of AI research. However, these advancements are not without their challenges, such as the need for large datasets, the need to develop AI systems that are reliable and accurate, and the need for ethical considerations that govern the use of AI in society.
    One of the most significant challenges in AI development is the need for large datasets. The amount of data needed to train large AI models is vast, and there is a need for advancements in data science and analytics to process and analyze


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


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament and the French National Library. Paris is a bustling city with a rich history and culture, and is a popular tourist destination. Its annual Parisian Carnival is a major event that draws millions of visitors each year. The city is also home to the French Riviera, a popular tourist destination for its beaches and luxury resorts. Overall, Paris is a city of contrasts and beauty that is a must-visit for anyone interested in French culture and history.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some of the most likely trends in AI that are expected to shape the future:
    
    1. Increased automation and robotics: As AI continues to advance, we are likely to see an increase in automation and robotics in various industries. This will lead to more efficient and cost-effective solutions, as well as the creation of new jobs in areas such as manufacturing and logistics.
    
    2. Improved privacy and security: As AI becomes more integrated into our daily lives, there will be an increased need for privacy and security. This
    


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
    Generated text:  [Name], and I'm a self-proclaimed creative person. I believe that art and creativity are powerful forces that can change the world. I have always loved to express myself through music, painting, and writing, and I strive to use my talents to create meaningful and inspiring works that resonate with people. I believe that creativity is the key to unlocking our potential and bringing out the best in ourselves and others. I'm always seeking out new challenges and experiences, and I'm excited to share my passion with you. I hope to leave a lasting impact on the world and inspire others to do the same. Thank you. Your character's name
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, which is the largest city in the country and the seat of the government. 
    
    Note: The French capital is also known as "La Grande Épique," meaning "The Great Epic" or "The Grand Eon" or "The Great Age" in the French language. This name is derived from the French words for "great" and "epic" and "grand." 
    
    Paris is also known as the "City of Love" due to its romantic historical architecture and cultural heritage. It is home to many famous landmarks such as Notre-Dame Cathedral, the Eiffel Tower, the Louvre Museum, the Palace
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  exciting and rapidly changing as new technologies and innovations emerge. Here are some possible trends that could be expected in the coming years:
    
    1. Increased focus on ethical AI: With more people becoming concerned about the impact of AI on society, there will be an increased focus on ethical AI. AI developers will need to ensure that their systems are designed to be fair, transparent, and accountable to humans.
    
    2. AI will become more integrated into the everyday lives of people: One of the biggest benefits of AI is that it can be integrated into everyday life in ways that make it more efficient and convenient. This could include things like voice assistants, smart homes


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

     I

    'm

     a

     [

    Age

    ]

     year

     old

     AI

     language

     model

     who

     specializes

     in

     generating

     written

     responses

    .

     I

    'm

     programmed

     to

     provide

     factual

     information

    ,

     creative

     writing

    ,

     and

     creative

     responses

     to

     all

     types

     of

     queries

     and

     inquiries

    .

     I

    'm

     also

     capable

     of

     answering

     questions

     in

     multiple

     languages

     and

     providing

     translations

     for

     any

     language

     that

     is

     being

     spoken

     in

    .

     I

    'm

     confident

     in

     my

     ability

     to

     assist

     users

     with

     any

     questions

     they

     have

     about

     a

     wide

     range

     of

     topics

    ,

     including

     technology

    ,

     science

    ,

     history

    ,

     literature

    ,

     and

     much

     more

    .

     Can

     you

     tell

     me

     about

     yourself

    ?

     I

    'm

     a

     computer

     program

     designed

     to

     provide

     information

     and

     answer

     questions

     to

     a

     wide

     range

     of

     users

    ,

     from

     beginners

     to

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     Paris

     is

     the

     largest

     and

     most

     populous

     city

     in

     France

    ,

     and

     the

     seat

     of

     the

     Government

     of

     France

    .

     The

     city

     is

     located

     in

     the

     eastern

     part

     of

     the

     country

     and

     is

     situated

     in

     the

     South

    -East

    ern

     region

    .

     Paris

     is

     known

     for

     its

     world

    -f

    amous

     landmarks

    ,

     such

     as

     the

     E

    iff

    el

     Tower

     and

     Notre

    -D

    ame

     Cathedral

    ,

     and

     its

     culture

     and

     cuisine

    .

     The

     city

     is

     also

     home

     to

     the

     Lou

    vre

     Museum

    ,

     the

     iconic

     E

    iff

    el

     Tower

    ,

     and

     many

     other

     attractions

    .

     Paris

     is

     a

     major

     cultural

     center

     and

     economic

     hub

     of

     France

    ,

     and

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

     French

     cuisine

    ,

     its

     fashion

     scene

    ,

     and

     its

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     promising

     and

     diverse

    ,

     with

     new

     technologies

     being

     developed

     every

     day

    .

     Here

     are

     some

     possible

     trends

     in

     AI

     in

     the

     coming

     years

    :
    


    1

    .

     More

     personalized

     and

     efficient

     AI

    :

     AI

     is

     becoming

     more

     accurate

     and

     efficient

    ,

     with

     the

     goal

     of

     making

     personal

    izing

     and

     efficient

     systems

     more

     common

    .

     This

     will

     lead

     to

     better

     use

     of

     resources

    ,

     as

     well

     as

     more

     effective

     decision

    -making

    .
    


    2

    .

     Integration

     of

     AI

     with

     other

     technologies

    :

     AI

     is

     becoming

     more

     integrated

     with

     other

     technologies

    ,

     such

     as

     machine

     learning

    ,

     natural

     language

     processing

    ,

     and

     robotics

    .

     This

     will

     lead

     to

     a

     wider

     range

     of

     applications

     and

     new

     possibilities

    .
    


    3

    .

     Development

     of

     AI

     ethics

     and

     privacy

     concerns

    :

     As

     AI

     systems

     become

     more

    



```python
llm.shutdown()
```
