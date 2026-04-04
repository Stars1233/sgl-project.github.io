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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.66it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.65it/s]


    2026-04-04 06:06:46,142 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 06:06:46] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:28,  2.61s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:28,  2.61s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:28,  2.61s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:28,  2.61s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:28,  1.92it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:28,  1.92it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:28,  1.92it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:28,  1.92it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:28,  1.92it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:28,  1.92it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:28,  1.92it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.81it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.81it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.81it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.81it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.81it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:02<00:05,  8.56it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:02<00:05,  8.56it/s]

    Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:03<00:05,  8.56it/s]Compiling num tokens (num_tokens=2048):  24%|██▍       | 14/58 [00:03<00:05,  8.56it/s]Compiling num tokens (num_tokens=1792):  24%|██▍       | 14/58 [00:03<00:05,  8.56it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 11.86it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 11.86it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 11.86it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 11.86it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 11.86it/s] 

    Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:02, 14.83it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:02, 14.83it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:02, 14.83it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:02, 14.83it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:02, 14.83it/s]Compiling num tokens (num_tokens=640):  38%|███▊      | 22/58 [00:03<00:02, 14.83it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 19.45it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 19.45it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 19.45it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 19.45it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 19.45it/s]

    Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 19.45it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.42it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.42it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.42it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.42it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.42it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.42it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.42it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 30.55it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 30.55it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 30.55it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 30.55it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 30.55it/s]

    Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 30.55it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 30.55it/s]Compiling num tokens (num_tokens=128):  66%|██████▌   | 38/58 [00:03<00:00, 30.55it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 37.63it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 37.63it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 37.63it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 37.63it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 37.63it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 37.63it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 37.63it/s]Compiling num tokens (num_tokens=28):  78%|███████▊  | 45/58 [00:03<00:00, 37.63it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:03<00:00, 44.09it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:03<00:00, 44.09it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:03<00:00, 44.09it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:03<00:00, 44.09it/s]

    Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:03<00:00, 44.09it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:03<00:00, 44.09it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:03<00:00, 44.09it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.00it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=72.37 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.76 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.76 GB):   3%|▎         | 2/58 [00:00<00:05, 11.12it/s]Capturing num tokens (num_tokens=7168 avail_mem=71.85 GB):   3%|▎         | 2/58 [00:00<00:05, 11.12it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=71.88 GB):   3%|▎         | 2/58 [00:00<00:05, 11.12it/s]Capturing num tokens (num_tokens=6656 avail_mem=71.88 GB):   7%|▋         | 4/58 [00:00<00:05, 10.55it/s]Capturing num tokens (num_tokens=6144 avail_mem=71.91 GB):   7%|▋         | 4/58 [00:00<00:05, 10.55it/s]Capturing num tokens (num_tokens=5632 avail_mem=71.94 GB):   7%|▋         | 4/58 [00:00<00:05, 10.55it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=71.94 GB):  10%|█         | 6/58 [00:00<00:05,  8.83it/s]Capturing num tokens (num_tokens=5120 avail_mem=71.97 GB):  10%|█         | 6/58 [00:00<00:05,  8.83it/s]Capturing num tokens (num_tokens=4608 avail_mem=72.26 GB):  10%|█         | 6/58 [00:00<00:05,  8.83it/s]Capturing num tokens (num_tokens=4608 avail_mem=72.26 GB):  14%|█▍        | 8/58 [00:00<00:05,  9.42it/s]Capturing num tokens (num_tokens=4096 avail_mem=72.29 GB):  14%|█▍        | 8/58 [00:00<00:05,  9.42it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=72.26 GB):  14%|█▍        | 8/58 [00:00<00:05,  9.42it/s]Capturing num tokens (num_tokens=3840 avail_mem=72.26 GB):  17%|█▋        | 10/58 [00:01<00:04, 10.18it/s]Capturing num tokens (num_tokens=3584 avail_mem=72.19 GB):  17%|█▋        | 10/58 [00:01<00:04, 10.18it/s]Capturing num tokens (num_tokens=3328 avail_mem=71.78 GB):  17%|█▋        | 10/58 [00:01<00:04, 10.18it/s]Capturing num tokens (num_tokens=3072 avail_mem=71.62 GB):  17%|█▋        | 10/58 [00:01<00:04, 10.18it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=71.62 GB):  22%|██▏       | 13/58 [00:01<00:03, 13.25it/s]Capturing num tokens (num_tokens=2816 avail_mem=71.62 GB):  22%|██▏       | 13/58 [00:01<00:03, 13.25it/s]Capturing num tokens (num_tokens=2560 avail_mem=71.62 GB):  22%|██▏       | 13/58 [00:01<00:03, 13.25it/s]Capturing num tokens (num_tokens=2304 avail_mem=71.61 GB):  22%|██▏       | 13/58 [00:01<00:03, 13.25it/s]Capturing num tokens (num_tokens=2304 avail_mem=71.61 GB):  28%|██▊       | 16/58 [00:01<00:02, 16.52it/s]Capturing num tokens (num_tokens=2048 avail_mem=71.61 GB):  28%|██▊       | 16/58 [00:01<00:02, 16.52it/s]Capturing num tokens (num_tokens=1792 avail_mem=71.60 GB):  28%|██▊       | 16/58 [00:01<00:02, 16.52it/s]Capturing num tokens (num_tokens=1536 avail_mem=71.49 GB):  28%|██▊       | 16/58 [00:01<00:02, 16.52it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=71.59 GB):  28%|██▊       | 16/58 [00:01<00:02, 16.52it/s]Capturing num tokens (num_tokens=1280 avail_mem=71.59 GB):  34%|███▍      | 20/58 [00:01<00:01, 20.61it/s]Capturing num tokens (num_tokens=1024 avail_mem=71.57 GB):  34%|███▍      | 20/58 [00:01<00:01, 20.61it/s]Capturing num tokens (num_tokens=960 avail_mem=71.58 GB):  34%|███▍      | 20/58 [00:01<00:01, 20.61it/s] Capturing num tokens (num_tokens=896 avail_mem=71.55 GB):  34%|███▍      | 20/58 [00:01<00:01, 20.61it/s]Capturing num tokens (num_tokens=832 avail_mem=71.57 GB):  34%|███▍      | 20/58 [00:01<00:01, 20.61it/s]Capturing num tokens (num_tokens=832 avail_mem=71.57 GB):  41%|████▏     | 24/58 [00:01<00:01, 24.54it/s]Capturing num tokens (num_tokens=768 avail_mem=71.56 GB):  41%|████▏     | 24/58 [00:01<00:01, 24.54it/s]Capturing num tokens (num_tokens=704 avail_mem=71.56 GB):  41%|████▏     | 24/58 [00:01<00:01, 24.54it/s]Capturing num tokens (num_tokens=640 avail_mem=71.53 GB):  41%|████▏     | 24/58 [00:01<00:01, 24.54it/s]

    Capturing num tokens (num_tokens=576 avail_mem=71.54 GB):  41%|████▏     | 24/58 [00:01<00:01, 24.54it/s]Capturing num tokens (num_tokens=576 avail_mem=71.54 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.95it/s]Capturing num tokens (num_tokens=512 avail_mem=71.53 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.95it/s]Capturing num tokens (num_tokens=480 avail_mem=71.56 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.95it/s]Capturing num tokens (num_tokens=448 avail_mem=71.54 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.95it/s]Capturing num tokens (num_tokens=416 avail_mem=71.53 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.95it/s]Capturing num tokens (num_tokens=384 avail_mem=71.54 GB):  48%|████▊     | 28/58 [00:01<00:01, 27.95it/s]Capturing num tokens (num_tokens=384 avail_mem=71.54 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.87it/s]Capturing num tokens (num_tokens=352 avail_mem=71.53 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.87it/s]Capturing num tokens (num_tokens=320 avail_mem=71.53 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.87it/s]Capturing num tokens (num_tokens=288 avail_mem=71.50 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.87it/s]

    Capturing num tokens (num_tokens=256 avail_mem=71.51 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.87it/s]Capturing num tokens (num_tokens=240 avail_mem=71.51 GB):  57%|█████▋    | 33/58 [00:01<00:00, 31.87it/s]Capturing num tokens (num_tokens=240 avail_mem=71.51 GB):  66%|██████▌   | 38/58 [00:01<00:00, 34.69it/s]Capturing num tokens (num_tokens=224 avail_mem=71.50 GB):  66%|██████▌   | 38/58 [00:01<00:00, 34.69it/s]Capturing num tokens (num_tokens=208 avail_mem=71.50 GB):  66%|██████▌   | 38/58 [00:01<00:00, 34.69it/s]Capturing num tokens (num_tokens=192 avail_mem=71.47 GB):  66%|██████▌   | 38/58 [00:01<00:00, 34.69it/s]Capturing num tokens (num_tokens=176 avail_mem=71.48 GB):  66%|██████▌   | 38/58 [00:01<00:00, 34.69it/s]Capturing num tokens (num_tokens=160 avail_mem=71.48 GB):  66%|██████▌   | 38/58 [00:01<00:00, 34.69it/s]Capturing num tokens (num_tokens=160 avail_mem=71.48 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.95it/s]Capturing num tokens (num_tokens=144 avail_mem=71.47 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.95it/s]Capturing num tokens (num_tokens=128 avail_mem=71.47 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.95it/s]

    Capturing num tokens (num_tokens=112 avail_mem=71.46 GB):  74%|███████▍  | 43/58 [00:02<00:00, 36.95it/s]Capturing num tokens (num_tokens=96 avail_mem=71.45 GB):  74%|███████▍  | 43/58 [00:02<00:00, 36.95it/s] Capturing num tokens (num_tokens=96 avail_mem=71.45 GB):  81%|████████  | 47/58 [00:02<00:00, 35.47it/s]Capturing num tokens (num_tokens=80 avail_mem=71.45 GB):  81%|████████  | 47/58 [00:02<00:00, 35.47it/s]Capturing num tokens (num_tokens=64 avail_mem=71.44 GB):  81%|████████  | 47/58 [00:02<00:00, 35.47it/s]Capturing num tokens (num_tokens=48 avail_mem=71.44 GB):  81%|████████  | 47/58 [00:02<00:00, 35.47it/s]Capturing num tokens (num_tokens=32 avail_mem=71.44 GB):  81%|████████  | 47/58 [00:02<00:00, 35.47it/s]

    Capturing num tokens (num_tokens=32 avail_mem=71.44 GB):  88%|████████▊ | 51/58 [00:02<00:00, 34.12it/s]Capturing num tokens (num_tokens=28 avail_mem=71.43 GB):  88%|████████▊ | 51/58 [00:02<00:00, 34.12it/s]Capturing num tokens (num_tokens=24 avail_mem=71.43 GB):  88%|████████▊ | 51/58 [00:02<00:00, 34.12it/s]Capturing num tokens (num_tokens=20 avail_mem=71.42 GB):  88%|████████▊ | 51/58 [00:02<00:00, 34.12it/s]Capturing num tokens (num_tokens=16 avail_mem=71.42 GB):  88%|████████▊ | 51/58 [00:02<00:00, 34.12it/s]Capturing num tokens (num_tokens=16 avail_mem=71.42 GB):  95%|█████████▍| 55/58 [00:02<00:00, 29.80it/s]Capturing num tokens (num_tokens=12 avail_mem=71.42 GB):  95%|█████████▍| 55/58 [00:02<00:00, 29.80it/s]Capturing num tokens (num_tokens=8 avail_mem=71.42 GB):  95%|█████████▍| 55/58 [00:02<00:00, 29.80it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=71.41 GB):  95%|█████████▍| 55/58 [00:02<00:00, 29.80it/s]Capturing num tokens (num_tokens=4 avail_mem=71.41 GB): 100%|██████████| 58/58 [00:02<00:00, 23.57it/s]


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
    Generated text:  Han
    How can I improve my public speaking skills?
    
    Improving your public speaking skills is a fantastic way to enhance your confidence and enhance your ability to communicate effectively in front of an audience. Here are some tips to help you improve your public speaking:
    
    1. **Research and Preparation**: Before you even get to the stage of delivering your speech, make sure you have a solid understanding of the topic you're speaking about. Prepare a detailed outline or a short presentation slide. This will help you cover your points in a clear and concise manner.
    
    2. **Practice**: Practice your speech multiple times. Even short speeches can benefit from practicing. If
    ===============================
    Prompt: The president of the United States is
    Generated text:  a powerful person, but there are ways to elect the right president. There are two major ways to elect a president: the Republican Party is the main party in the U.S., so the president is chosen from that party. The second major way is the Electoral College, which is a method used by the U.S. to elect the president and vice president. The Electoral College is the main method to elect a president. This method is more complex because it is based on how many states have voted for a particular party. You can think of the voting as a race, where the candidate with the most votes for each state wins. The party
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. The city is very busy and there are a lot of people living there. People often make mistakes, so in Paris, it is very important to be polite. The first person to greet you when you come to Paris is the Reception Desk. After you have arrived at your hotel, you can ask for your room number in the Reception Desk. It is also important to know the directions to all the most important places. To help you find the most important places, there are maps of Paris. They are placed in many places, such as the entrance of many hotels, the main street, and the subway. What is the main idea
    ===============================
    Prompt: The future of AI is
    Generated text:  in the hands of the individuals. As the world’s first AI governance initiative, AIAI is working with governments, other industry leaders, and individuals to work towards making AI a tool for a better, safer, more prosperous world. In this series, we will highlight one of the AIAI’s leadership in the development of the AI Governance Code.
    The AI Governance Code (AGC) is a set of principles, rules, and guidance that enables the governance of AI systems to be transparent, fair, and accountable. The AGC is developed as a tool for the private sector to support the use of AI in a responsible manner. It aims


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm excited to meet you and learn more about your career. What can you tell me about yourself? I'm a [insert a short description of your personality or skills]. I enjoy [insert a short description of your hobbies or interests]. What do you like to do in your free time? I like to [insert a short description of your hobbies or interests]. What's your favorite hobby? I like to [insert a short description of your hobbies or interests]. What's your favorite book or movie? I like to [insert a short description of your hobbies
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a cultural and economic center with a rich history dating back to the Roman Empire and the French Revolution. It is a popular tourist destination and a major hub for international business and diplomacy. The city is known for its fashion, art, and cuisine, and is a major center for the arts and sciences. Paris is a vibrant and dynamic city with a rich cultural heritage and a strong sense of identity. The
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs. As AI technology continues to improve, we can expect to see even more widespread use of AI in healthcare, with more sophisticated algorithms and machine learning techniques being developed to improve diagnosis, treatment, and patient care.
    
    2. Increased use of AI in finance: AI is already being used in finance to improve risk management, fraud
    


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
    Generated text:  [Name] and I am a [Occupation] in my [Role] field. I have always been interested in [Interests/Challenges/Goals]. I am a [Strength/Weakness] in [Skill/Ability] and I strive to [Action/Goal] with [Effort/Support]. I am always [Positive/Emotional], and I believe in [Purpose/Quest] and [Vision]. I am [Reliable/Resilient] and [Selfless/Generous]. I believe in [Quality/Value], and I am [Emotional Resonance]. I am [Curiosity/
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    Available options: 1. Yes. 2. No.
    
    1. Yes. 
    
    Paris, often referred to as "the city of love" and the "city of lights," is the capital city of France. It is located on the Left Bank of the Seine, in the Île de la Cité, and is the largest city in the European Union. Paris is known for its iconic landmarks like the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and many museums and attractions. The city is also famous for its rich cultural heritage, including the French Riviera, and its cosmopolitan
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  an exciting and rapidly evolving field, with many potential directions for development. Some of the most promising trends include:
    
    1. Advancements in machine learning: With the development of deep learning and other advanced algorithms, AI will become even more capable of learning and making predictions. This will lead to more accurate predictions and even autonomous decision-making.
    
    2. Integration of AI into various industries: AI will be integrated into more and more industries, from healthcare to transportation to financial services. This will lead to new opportunities for businesses to improve efficiency, reduce costs, and improve customer experiences.
    
    3. Increased emphasis on ethical AI: As AI becomes more sophisticated, there


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

     am

     a

    /an

     [

    Job

     Title

    ]

     at

     [

    Company

     Name

    ].

     I

     am

     passionate

     about

     [

    What

     you

     do

     at

     work

    ],

     and

     I

     am

     a

     hard

     worker

    .

     I

     enjoy

     [

    Why

     you

     love

     your

     job

    ]

     and

     am

     always

     eager

     to

     learn

     new

     things

    .

     I

     have

     a

     solid

     work

     ethic

     and

     pride

     in

     my

     work

    .

     I

     am

     a

     [

    General

     Statement

    ]

     of

     character

    .

     I

     am

     someone

     who

     is

     [

    Tell

     one

     of

     your

     three

     favorite

     qualities

     about

     yourself

    ].

     I

     believe

     that

     I

     can

     make

     a

     positive

     impact

     on

     the

     world

     and

     that

     I

     am

     committed

     to

     doing

     so

    .

     I

     am

     [

    Tell

     one

     of

     your

     three

     most

     important

     life

     goals

    ].

     What

     does

     that

     mean

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    Paris

    ,

     the

     city

     of

     love

    ,

     culture

    ,

     and

     history

    ,

     is

     the

     most

     populous

     city

     in

     France

     and

     one

     of

     the

     world

    's

     most

     famous

     cities

    .

     It

     is

     located

     on

     the

     left

     bank

     of

     the

     Se

    ine

     river

    ,

     near

     the

     historic

     center

     of

     the

     city

    ,

     where

     the

     Se

    ine

     flows

     through

     the

     city

     and

     meets

     the

     ocean

    .

     
    


    Paris

     is

     known

     for

     its

     unique

     architecture

    ,

     including

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

     Notre

    -D

    ame

     Cathedral

    ,

     and

     the

     Arc

     de

     Tri

    omp

    he

    .

     It

     is

     also

     famous

     for

     its

     fashion

     industry

    ,

     with

     bout

    iques

    ,

     high

    -end

     restaurants

    ,

     and

     luxury

     hotels

    .

     
    


    Paris

     is

     known

     for

     its

     rich

     history

    ,

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     involve

     several

     trends

     that

     will

     shape

     the

     landscape

     of

     this

     field

    .

     Here

     are

     some

     of

     the

     most

     likely

     ones

    :
    


    1

    .

     Increased

     automation

     and

     productivity

    :

     AI

     will

     continue

     to

     improve

     at

     its

     core

     functions

    ,

     which

     will

     lead

     to

     greater

     automation

     in

     various

     areas

     of

     work

    .

     This

     will

     result

     in

     increased

     productivity

    ,

     lower

     costs

    ,

     and

     more

     efficiency

    .

     Automation

     will

     likely

     become

     more

     prevalent

     in

     areas

     such

     as

     manufacturing

    ,

     healthcare

    ,

     and

     transportation

    .
    


    2

    .

     AI

    -powered

     healthcare

    :

     AI

     will

     continue

     to

     play

     a

     critical

     role

     in

     healthcare

     in

     the

     coming

     years

    .

     AI

    -powered

     diagnostic

     tools

     will

     become

     more

     sophisticated

    ,

     and

     AI

    -ass

    isted

     treatments

     will

     become

     more

     common

    .

     AI

     will

     also

     be

     used

    



```python
llm.shutdown()
```
