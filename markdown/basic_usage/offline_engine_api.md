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


    2026-04-02 16:13:21.168 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:13:21] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:13:21.168 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:13:21] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:13:21.168 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:13:21] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:13:21.168 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:13:21] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:13:21.168 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:13:21] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.53it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.53it/s]


    2026-04-02 16:13:27,100 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 16:13:27] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:25,  2.56s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:25,  2.56s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:25,  2.56s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:25,  2.56s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:27,  1.95it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:27,  1.95it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:27,  1.95it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:27,  1.95it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:27,  1.95it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:27,  1.95it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:27,  1.95it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.96it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.96it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.96it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.96it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.96it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.96it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.96it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.96it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.96it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:08,  5.96it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.39it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.39it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.39it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.39it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.39it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.39it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.39it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 13.39it/s]Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:02<00:02, 13.39it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 20.65it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 20.65it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 20.65it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 20.65it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 20.65it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 20.65it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 20.65it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:03<00:01, 20.65it/s]Compiling num tokens (num_tokens=320):  47%|████▋     | 27/58 [00:03<00:01, 20.65it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 28.49it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 28.49it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:03<00:00, 28.49it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:03<00:00, 28.49it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:03<00:00, 28.49it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:03<00:00, 28.49it/s]

    Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:03<00:00, 28.49it/s]Compiling num tokens (num_tokens=176):  60%|██████    | 35/58 [00:03<00:00, 28.49it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:03<00:00, 35.05it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:03<00:00, 35.05it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:03<00:00, 35.05it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:03<00:00, 35.05it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:03<00:00, 35.05it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:03<00:00, 35.05it/s] Compiling num tokens (num_tokens=80):  72%|███████▏  | 42/58 [00:03<00:00, 35.05it/s]Compiling num tokens (num_tokens=64):  72%|███████▏  | 42/58 [00:03<00:00, 35.05it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 41.10it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 41.10it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 41.10it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 41.10it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 41.10it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 41.10it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 41.10it/s]

    Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 41.10it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:03<00:00, 41.10it/s] Compiling num tokens (num_tokens=4):  84%|████████▍ | 49/58 [00:03<00:00, 41.10it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.02it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=71.12 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.09 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.09 GB):   3%|▎         | 2/58 [00:00<00:02, 19.68it/s]Capturing num tokens (num_tokens=7168 avail_mem=71.09 GB):   3%|▎         | 2/58 [00:00<00:02, 19.68it/s]Capturing num tokens (num_tokens=6656 avail_mem=71.09 GB):   3%|▎         | 2/58 [00:00<00:02, 19.68it/s]Capturing num tokens (num_tokens=6144 avail_mem=71.09 GB):   3%|▎         | 2/58 [00:00<00:02, 19.68it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=71.09 GB):   9%|▊         | 5/58 [00:00<00:02, 22.97it/s]Capturing num tokens (num_tokens=5632 avail_mem=71.08 GB):   9%|▊         | 5/58 [00:00<00:02, 22.97it/s]Capturing num tokens (num_tokens=5120 avail_mem=71.08 GB):   9%|▊         | 5/58 [00:00<00:02, 22.97it/s]Capturing num tokens (num_tokens=4608 avail_mem=71.08 GB):   9%|▊         | 5/58 [00:00<00:02, 22.97it/s]Capturing num tokens (num_tokens=4096 avail_mem=71.08 GB):   9%|▊         | 5/58 [00:00<00:02, 22.97it/s]Capturing num tokens (num_tokens=4096 avail_mem=71.08 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.99it/s]Capturing num tokens (num_tokens=3840 avail_mem=71.08 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.99it/s]Capturing num tokens (num_tokens=3584 avail_mem=71.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.99it/s]Capturing num tokens (num_tokens=3328 avail_mem=71.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.99it/s]Capturing num tokens (num_tokens=3072 avail_mem=71.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.99it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=71.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.99it/s]Capturing num tokens (num_tokens=2816 avail_mem=71.07 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.21it/s]Capturing num tokens (num_tokens=2560 avail_mem=71.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.21it/s]Capturing num tokens (num_tokens=2304 avail_mem=71.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.21it/s]Capturing num tokens (num_tokens=2048 avail_mem=71.05 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.21it/s]Capturing num tokens (num_tokens=1792 avail_mem=71.05 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.21it/s]Capturing num tokens (num_tokens=1536 avail_mem=71.05 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.21it/s]Capturing num tokens (num_tokens=1280 avail_mem=71.04 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.21it/s]Capturing num tokens (num_tokens=1280 avail_mem=71.04 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.49it/s]Capturing num tokens (num_tokens=1024 avail_mem=71.02 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.49it/s]Capturing num tokens (num_tokens=960 avail_mem=71.04 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.49it/s] Capturing num tokens (num_tokens=896 avail_mem=71.03 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.49it/s]

    Capturing num tokens (num_tokens=832 avail_mem=71.03 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.49it/s]Capturing num tokens (num_tokens=768 avail_mem=71.03 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.49it/s]Capturing num tokens (num_tokens=768 avail_mem=71.03 GB):  43%|████▎     | 25/58 [00:00<00:00, 41.80it/s]Capturing num tokens (num_tokens=704 avail_mem=71.02 GB):  43%|████▎     | 25/58 [00:00<00:00, 41.80it/s]Capturing num tokens (num_tokens=640 avail_mem=71.02 GB):  43%|████▎     | 25/58 [00:00<00:00, 41.80it/s]Capturing num tokens (num_tokens=576 avail_mem=71.02 GB):  43%|████▎     | 25/58 [00:00<00:00, 41.80it/s]Capturing num tokens (num_tokens=512 avail_mem=71.01 GB):  43%|████▎     | 25/58 [00:00<00:00, 41.80it/s]Capturing num tokens (num_tokens=480 avail_mem=71.02 GB):  43%|████▎     | 25/58 [00:00<00:00, 41.80it/s]Capturing num tokens (num_tokens=448 avail_mem=71.02 GB):  43%|████▎     | 25/58 [00:00<00:00, 41.80it/s]Capturing num tokens (num_tokens=448 avail_mem=71.02 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.20it/s]Capturing num tokens (num_tokens=416 avail_mem=71.02 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.20it/s]Capturing num tokens (num_tokens=384 avail_mem=71.02 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.20it/s]Capturing num tokens (num_tokens=352 avail_mem=71.01 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.20it/s]

    Capturing num tokens (num_tokens=320 avail_mem=71.01 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.20it/s]Capturing num tokens (num_tokens=288 avail_mem=71.00 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.20it/s]Capturing num tokens (num_tokens=256 avail_mem=71.00 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.20it/s]Capturing num tokens (num_tokens=256 avail_mem=71.00 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.92it/s]Capturing num tokens (num_tokens=240 avail_mem=71.00 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.92it/s]Capturing num tokens (num_tokens=224 avail_mem=71.00 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.92it/s]Capturing num tokens (num_tokens=208 avail_mem=70.99 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.92it/s]Capturing num tokens (num_tokens=192 avail_mem=70.99 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.92it/s]Capturing num tokens (num_tokens=176 avail_mem=70.99 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.92it/s]Capturing num tokens (num_tokens=176 avail_mem=70.99 GB):  72%|███████▏  | 42/58 [00:01<00:00, 46.20it/s]Capturing num tokens (num_tokens=160 avail_mem=70.98 GB):  72%|███████▏  | 42/58 [00:01<00:00, 46.20it/s]Capturing num tokens (num_tokens=144 avail_mem=70.98 GB):  72%|███████▏  | 42/58 [00:01<00:00, 46.20it/s]

    Capturing num tokens (num_tokens=128 avail_mem=70.98 GB):  72%|███████▏  | 42/58 [00:01<00:00, 46.20it/s]Capturing num tokens (num_tokens=112 avail_mem=70.98 GB):  72%|███████▏  | 42/58 [00:01<00:00, 46.20it/s]Capturing num tokens (num_tokens=96 avail_mem=70.97 GB):  72%|███████▏  | 42/58 [00:01<00:00, 46.20it/s] Capturing num tokens (num_tokens=96 avail_mem=70.97 GB):  81%|████████  | 47/58 [00:01<00:00, 46.64it/s]Capturing num tokens (num_tokens=80 avail_mem=70.97 GB):  81%|████████  | 47/58 [00:01<00:00, 46.64it/s]Capturing num tokens (num_tokens=64 avail_mem=70.97 GB):  81%|████████  | 47/58 [00:01<00:00, 46.64it/s]Capturing num tokens (num_tokens=48 avail_mem=70.96 GB):  81%|████████  | 47/58 [00:01<00:00, 46.64it/s]Capturing num tokens (num_tokens=32 avail_mem=70.96 GB):  81%|████████  | 47/58 [00:01<00:00, 46.64it/s]Capturing num tokens (num_tokens=28 avail_mem=70.95 GB):  81%|████████  | 47/58 [00:01<00:00, 46.64it/s]Capturing num tokens (num_tokens=24 avail_mem=70.95 GB):  81%|████████  | 47/58 [00:01<00:00, 46.64it/s]Capturing num tokens (num_tokens=24 avail_mem=70.95 GB):  91%|█████████▏| 53/58 [00:01<00:00, 48.79it/s]Capturing num tokens (num_tokens=20 avail_mem=70.95 GB):  91%|█████████▏| 53/58 [00:01<00:00, 48.79it/s]Capturing num tokens (num_tokens=16 avail_mem=70.95 GB):  91%|█████████▏| 53/58 [00:01<00:00, 48.79it/s]

    Capturing num tokens (num_tokens=12 avail_mem=70.94 GB):  91%|█████████▏| 53/58 [00:01<00:00, 48.79it/s]Capturing num tokens (num_tokens=8 avail_mem=70.94 GB):  91%|█████████▏| 53/58 [00:01<00:00, 48.79it/s] Capturing num tokens (num_tokens=4 avail_mem=70.94 GB):  91%|█████████▏| 53/58 [00:01<00:00, 48.79it/s]Capturing num tokens (num_tokens=4 avail_mem=70.94 GB): 100%|██████████| 58/58 [00:01<00:00, 43.40it/s]


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
    Generated text:  Isabelle, a computer programmer with a background in data science. I have a passion for teaching programming languages to students and helping them develop their coding skills. Currently, I'm working on a project that involves creating a game that combines concepts from AI, machine learning, and computer vision to create a sense of immersion in the game world. Can you provide me with some examples of AI and machine learning algorithms that I could incorporate into this game? Additionally, could you provide me with some code examples that demonstrate how to use these algorithms in my game? As for the computer vision component, do you have any examples or resources that you can recommend for
    ===============================
    Prompt: The president of the United States is
    Generated text:  200 years old. The vice president is 1/5 times younger than the president. If the president's age increases by 10% every year and the vice president's age increases by 5% every year, how old will the vice president be in 3 years? To determine the age of the vice president in 3 years, we need to follow these steps:
    
    1. Determine the president's current age in 3 years.
    2. Use the vice president's current age to find out how old he will be in 3 years.
    
    First, we know that the president is currently 200
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. Its history goes back to the Latin times. The Roman Forum was there already. It was an important place to have meetings. The earliest building was the Temple of Mars, but it was later destroyed by the Normans. The city was founded by the French king Louis IX in 1209. But the most important thing is that in 1358, the French king Charles VIII returned to Paris and declared the city the capital of France. Today, Paris is the largest city in the world. It has 60,000 km^2, which is twice the size of the United Kingdom.
    ===============================
    Prompt: The future of AI is
    Generated text:  here and it’s changing the way we live our lives. The possibilities are endless and the impact is tremendous.
    Artificial Intelligence (AI) is a technology that uses computers and algorithms to perform tasks that typically require human intelligence, such as learning, reasoning, and problem-solving. It is becoming increasingly important in many industries, including healthcare, finance, and transportation.
    The future of AI is bright, but it will require significant investment and development. As AI continues to evolve, it will likely become a key driver of innovation and change in many areas of society.
    AI is a rapidly growing field with the potential to transform industries and improve the quality of


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. It is the largest city in France and the third-largest city in the world by population. Paris is known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Arc de Triomphe. It is also home to many world-renowned museums, theaters, and restaurants. Paris is a cultural and economic hub of France and a major tourist destination. The city is known for its rich history, diverse culture, and beautiful architecture. It is a popular destination for tourists and locals alike. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly together.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased focus on ethical AI: As more people become aware of the potential risks of AI, there will be an increased focus on ethical AI. This will likely lead to the development of more transparent and accountable AI systems that are designed to minimize harm and maximize benefits.
    
    2. Greater use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs. As AI technology continues to advance, we can expect to see even more widespread use of AI in healthcare, particularly
    


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
    Generated text:  [Name] and I'm a professional writer and editor with over [number] years of experience in the field. I specialize in crafting compelling narratives and enhancing the impact of written content. My work has been published in several prestigious journals and featured in several online publications. My attention to detail and ability to craft stories that resonate with readers make me stand out among my peers. Please feel free to ask me any questions you may have! [Name] [Phone number] [Email address]
    
    P.S. I'm always here to help with your writing needs! [Name] [Phone number] [Email address] [Website URL] [LinkedIn
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, the Louvre Museum, and the Arc de Triomphe. Paris is home to millions of people and is considered a cultural and political center of the world. It is known for its lively nightlife, Paris Opera, and the annual Eiffel Tower Festival. The French language is also spoken in Paris. Paris is the most visited city in the world and hosts numerous world-class events and festivals throughout the year. Other notable landmarks in Paris include Notre Dame Cathedral, the Basilica of Saint-Denis, and the Seine River. The city is also home
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be characterized by rapid progress and significant developments. Here are some potential trends that could shape the development of AI in the next decade:
    
    1. Advancements in machine learning: Advances in machine learning, including more powerful models, faster computing power, and improved algorithms, are expected to drive even greater progress in AI. This will lead to more accurate and sophisticated predictions, better self-driving cars, and more efficient processes.
    
    2. Increased focus on ethical AI: As AI becomes more advanced, there will be increased scrutiny and regulation of its use. This will likely lead to a greater emphasis on ethical considerations, such as privacy, fairness, and


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

     am

     a

     [

    major

     or

     genre

    ]

     author

    .

     I

     am

     passionate

     about

     [

    writing

     about

     a

     specific

     subject

    ,

     such

     as

     horror

    ,

     fantasy

    ,

     science

     fiction

    ,

     or

     romance

    ],

     and

     I

     believe

     in

     delivering

     a

     compelling

     story

     that

     engages

     readers

     with

     vivid

     and

     immersive

     worlds

    .

     My

     novels

     have

     won

     numerous

     awards

    ,

     and

     I

     am

     known

     for

     my

     ability

     to

     create

     compelling

     characters

     and

     complex

     plots

     that

     leave

     readers

     on

     the

     edge

     of

     their

     seats

    .

     I

     believe

     that

     every

     story

     has

     a

     story

    ,

     and

     I

     strive

     to

     provide

     a

     rich

     and

     nuanced

     narrative

     that

     engages

     the

     reader

     on

     a

     deep

     level

    .

     So

    ,

     if

     you

    're

     looking

     for

     a

     writer

     who

     can

     create

     incredible

     stories

    ,

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     also

     known

     as

     "

    La

     Par

    que

    ,"

     the

     oldest

     city

     in

     the

     world

    .
    


    Paris

     is

     known

     as

     the

     "

    City

     of

     Love

    "

     and

     is

     a

     historic

     and

     culturally

     rich

     city

     with

     many

     iconic

     landmarks

     and

     attractions

    .

     Its

     name

     is

     derived

     from

     the

     Latin

     "

    par

    vis

    ,"

     meaning

     "

    green

    "

     and

     "

    par

    c

    ,"

     which

     refers

     to

     the

     area

     around

     the

     Notre

    -D

    ame

     Cathedral

    .

     Paris

     has

     a

     population

     of

     over

     

    1

    1

     million

     people

     and

     is

     a

     major

     cultural

     and

     economic

     hub

     of

     Europe

    .

     The

     city

     is

     also

     home

     to

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

     the

     Arc

     de

     Tri

    omp

    he

    ,

     and

     other

     attractions

    .

     The

     French

     capital

     is

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     anticipated

     and

     fascinating

     due

     to

     its

     numerous

     potential

     applications

     and

     the

     increasing

     complexity

     and

     sophistication

     of

     AI

     technologies

    .

     Here

     are

     some

     possible

     future

     trends

     in

     AI

    :
    


    1

    .

     Increased

     automation

     and

     deep

     learning

    :

     As

     AI

     continues

     to

     evolve

    ,

     we

     can

     expect

     automation

     in

     various

     industries

    ,

     including

     manufacturing

    ,

     healthcare

    ,

     and

     transportation

    .

     Deep

     learning

     will

     play

     an

     even

     greater

     role

     in

     these

     fields

    ,

     enabling

     more

     complex

     and

     sophisticated

     AI

     systems

    .
    


    2

    .

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

     integrated

     into

     our

     daily

     lives

    ,

     there

     will

     likely

     be

     greater

     scrutiny

     of

     their

     ethical

     implications

     and

     privacy

     concerns

    .

     This

     may

     lead

     to

     a

     shift

     towards

     more

     transparent

     and

     ethical

     AI

     practices

    .
    


    3

    .

    



```python
llm.shutdown()
```
