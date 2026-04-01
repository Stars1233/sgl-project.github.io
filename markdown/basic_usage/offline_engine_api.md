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


    2026-04-01 01:48:11.068 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:48:11] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:48:11.068 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:48:11] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:48:11.068 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:48:11] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:48:11.068 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:48:11] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 01:48:11.068 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 01:48:11] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.76it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.76it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:29,  1.86it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.71it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:03<00:08,  5.71it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.88it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.88it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.88it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.88it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.88it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.88it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.88it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.88it/s]

    Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:03<00:03, 12.88it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 19.92it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 19.92it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 19.92it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 19.92it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 19.92it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 19.92it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 19.92it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:03<00:01, 19.92it/s]Compiling num tokens (num_tokens=320):  47%|████▋     | 27/58 [00:03<00:01, 19.92it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 27.49it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 27.49it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:03<00:00, 27.49it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:03<00:00, 27.49it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:03<00:00, 27.49it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:03<00:00, 27.49it/s]

    Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:03<00:00, 27.49it/s]Compiling num tokens (num_tokens=176):  60%|██████    | 35/58 [00:03<00:00, 27.49it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:03<00:00, 33.87it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:03<00:00, 33.87it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:03<00:00, 33.87it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:03<00:00, 33.87it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:03<00:00, 33.87it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:03<00:00, 33.87it/s] Compiling num tokens (num_tokens=80):  72%|███████▏  | 42/58 [00:03<00:00, 33.87it/s]Compiling num tokens (num_tokens=64):  72%|███████▏  | 42/58 [00:03<00:00, 33.87it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 39.83it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 39.83it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 39.83it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 39.83it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 39.83it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 39.83it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 39.83it/s]

    Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 39.83it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:03<00:00, 39.83it/s] Compiling num tokens (num_tokens=4):  84%|████████▍ | 49/58 [00:03<00:00, 39.83it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.31it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.12 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=68.89 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=68.89 GB):   3%|▎         | 2/58 [00:00<00:04, 12.93it/s]Capturing num tokens (num_tokens=7168 avail_mem=60.40 GB):   3%|▎         | 2/58 [00:00<00:04, 12.93it/s]Capturing num tokens (num_tokens=6656 avail_mem=60.39 GB):   3%|▎         | 2/58 [00:00<00:04, 12.93it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=60.40 GB):   3%|▎         | 2/58 [00:00<00:04, 12.93it/s]Capturing num tokens (num_tokens=6144 avail_mem=60.40 GB):   9%|▊         | 5/58 [00:00<00:02, 19.00it/s]Capturing num tokens (num_tokens=5632 avail_mem=60.39 GB):   9%|▊         | 5/58 [00:00<00:02, 19.00it/s]Capturing num tokens (num_tokens=5120 avail_mem=60.39 GB):   9%|▊         | 5/58 [00:00<00:02, 19.00it/s]Capturing num tokens (num_tokens=4608 avail_mem=60.39 GB):   9%|▊         | 5/58 [00:00<00:02, 19.00it/s]Capturing num tokens (num_tokens=4096 avail_mem=60.39 GB):   9%|▊         | 5/58 [00:00<00:02, 19.00it/s]Capturing num tokens (num_tokens=4096 avail_mem=60.39 GB):  16%|█▌        | 9/58 [00:00<00:01, 24.98it/s]Capturing num tokens (num_tokens=3840 avail_mem=60.38 GB):  16%|█▌        | 9/58 [00:00<00:01, 24.98it/s]Capturing num tokens (num_tokens=3584 avail_mem=60.38 GB):  16%|█▌        | 9/58 [00:00<00:01, 24.98it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=60.38 GB):  16%|█▌        | 9/58 [00:00<00:01, 24.98it/s]Capturing num tokens (num_tokens=3328 avail_mem=60.38 GB):  21%|██        | 12/58 [00:00<00:01, 25.72it/s]Capturing num tokens (num_tokens=3072 avail_mem=60.37 GB):  21%|██        | 12/58 [00:00<00:01, 25.72it/s]Capturing num tokens (num_tokens=2816 avail_mem=60.37 GB):  21%|██        | 12/58 [00:00<00:01, 25.72it/s]Capturing num tokens (num_tokens=2560 avail_mem=60.37 GB):  21%|██        | 12/58 [00:00<00:01, 25.72it/s]Capturing num tokens (num_tokens=2304 avail_mem=60.36 GB):  21%|██        | 12/58 [00:00<00:01, 25.72it/s]Capturing num tokens (num_tokens=2048 avail_mem=60.36 GB):  21%|██        | 12/58 [00:00<00:01, 25.72it/s]Capturing num tokens (num_tokens=2048 avail_mem=60.36 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.44it/s]Capturing num tokens (num_tokens=1792 avail_mem=60.36 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.44it/s]Capturing num tokens (num_tokens=1536 avail_mem=60.35 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.44it/s]Capturing num tokens (num_tokens=1280 avail_mem=60.35 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.44it/s]Capturing num tokens (num_tokens=1024 avail_mem=60.33 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.44it/s]

    Capturing num tokens (num_tokens=960 avail_mem=60.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.44it/s] Capturing num tokens (num_tokens=896 avail_mem=60.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.44it/s]Capturing num tokens (num_tokens=896 avail_mem=60.34 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.63it/s]Capturing num tokens (num_tokens=832 avail_mem=60.34 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.63it/s]Capturing num tokens (num_tokens=768 avail_mem=60.33 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.63it/s]Capturing num tokens (num_tokens=704 avail_mem=60.33 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.63it/s]Capturing num tokens (num_tokens=640 avail_mem=60.33 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.63it/s]Capturing num tokens (num_tokens=576 avail_mem=60.33 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.63it/s]Capturing num tokens (num_tokens=512 avail_mem=60.32 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.63it/s]Capturing num tokens (num_tokens=512 avail_mem=60.32 GB):  50%|█████     | 29/58 [00:00<00:00, 42.35it/s]Capturing num tokens (num_tokens=480 avail_mem=60.33 GB):  50%|█████     | 29/58 [00:00<00:00, 42.35it/s]Capturing num tokens (num_tokens=448 avail_mem=60.33 GB):  50%|█████     | 29/58 [00:00<00:00, 42.35it/s]

    Capturing num tokens (num_tokens=416 avail_mem=60.33 GB):  50%|█████     | 29/58 [00:00<00:00, 42.35it/s]Capturing num tokens (num_tokens=384 avail_mem=60.33 GB):  50%|█████     | 29/58 [00:00<00:00, 42.35it/s]Capturing num tokens (num_tokens=352 avail_mem=60.32 GB):  50%|█████     | 29/58 [00:00<00:00, 42.35it/s]Capturing num tokens (num_tokens=352 avail_mem=60.32 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.99it/s]Capturing num tokens (num_tokens=320 avail_mem=60.32 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.99it/s]Capturing num tokens (num_tokens=288 avail_mem=60.31 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.99it/s]Capturing num tokens (num_tokens=256 avail_mem=60.31 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.99it/s]Capturing num tokens (num_tokens=240 avail_mem=60.31 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.99it/s]Capturing num tokens (num_tokens=224 avail_mem=60.30 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.99it/s]

    Capturing num tokens (num_tokens=224 avail_mem=60.30 GB):  67%|██████▋   | 39/58 [00:01<00:00, 39.15it/s]Capturing num tokens (num_tokens=208 avail_mem=60.30 GB):  67%|██████▋   | 39/58 [00:01<00:00, 39.15it/s]Capturing num tokens (num_tokens=192 avail_mem=60.30 GB):  67%|██████▋   | 39/58 [00:01<00:00, 39.15it/s]Capturing num tokens (num_tokens=176 avail_mem=60.30 GB):  67%|██████▋   | 39/58 [00:01<00:00, 39.15it/s]Capturing num tokens (num_tokens=160 avail_mem=60.29 GB):  67%|██████▋   | 39/58 [00:01<00:00, 39.15it/s]Capturing num tokens (num_tokens=144 avail_mem=60.29 GB):  67%|██████▋   | 39/58 [00:01<00:00, 39.15it/s]Capturing num tokens (num_tokens=144 avail_mem=60.29 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.27it/s]Capturing num tokens (num_tokens=128 avail_mem=60.29 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.27it/s]Capturing num tokens (num_tokens=112 avail_mem=60.28 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.27it/s]Capturing num tokens (num_tokens=96 avail_mem=60.28 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.27it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=60.28 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.27it/s]Capturing num tokens (num_tokens=80 avail_mem=60.28 GB):  83%|████████▎ | 48/58 [00:01<00:00, 33.10it/s]Capturing num tokens (num_tokens=64 avail_mem=60.27 GB):  83%|████████▎ | 48/58 [00:01<00:00, 33.10it/s]Capturing num tokens (num_tokens=48 avail_mem=60.27 GB):  83%|████████▎ | 48/58 [00:01<00:00, 33.10it/s]Capturing num tokens (num_tokens=32 avail_mem=60.27 GB):  83%|████████▎ | 48/58 [00:01<00:00, 33.10it/s]Capturing num tokens (num_tokens=28 avail_mem=60.26 GB):  83%|████████▎ | 48/58 [00:01<00:00, 33.10it/s]Capturing num tokens (num_tokens=28 avail_mem=60.26 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.81it/s]Capturing num tokens (num_tokens=24 avail_mem=60.26 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.81it/s]

    Capturing num tokens (num_tokens=20 avail_mem=60.25 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.81it/s]Capturing num tokens (num_tokens=16 avail_mem=60.25 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.81it/s]Capturing num tokens (num_tokens=12 avail_mem=60.25 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.81it/s]Capturing num tokens (num_tokens=8 avail_mem=60.25 GB):  90%|████████▉ | 52/58 [00:01<00:00, 33.81it/s] Capturing num tokens (num_tokens=8 avail_mem=60.25 GB):  98%|█████████▊| 57/58 [00:01<00:00, 36.58it/s]Capturing num tokens (num_tokens=4 avail_mem=60.24 GB):  98%|█████████▊| 57/58 [00:01<00:00, 36.58it/s]Capturing num tokens (num_tokens=4 avail_mem=60.24 GB): 100%|██████████| 58/58 [00:01<00:00, 34.60it/s]


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
    Generated text:  Amy and I am a female. I was raised in a military family. What is your name? Amy Hahn is a female, raised in a military family.
    How can I best summarize Amy Hahn's life experiences? Amy Hahn has a military background, raising children in a military family. She is a contemporary writer, living in a military family and raising children with a military family. She is currently a writer with a focus on literature and storytelling.
    Amy Hahn's military background has given her a unique perspective and experiences that she has brought into her writing. She is also a poet and continues to write and share her work with
    ===============================
    Prompt: The president of the United States is
    Generated text:  a high-ranking government official in the executive branch of the United States government. They serve as the head of the executive branch, and are responsible for implementing the policies and laws of the federal government. The President of the United States is elected every four years for a term of two years. Who is the President of the United States currently? There was no president of the United States when Donald Trump became president in 2017. The current president of the United States is Joe Biden, who succeeded Donald Trump as the 46th President of the United States. However, it's important to note that there have been other presidents in
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, the capital of Germany is Berlin, and the capital of the United States is Washington. What is the capital of Italy?
    The answer is Washington. Washington is the capital of the United States. The other capitals are:
    
    - Paris, France: the capital of France
    - Berlin, Germany: the capital of Germany
    - Rome, Italy: the capital of Italy
    - Naples, Italy: the capital of Italy (not the largest city, but the capital of the capital of Italy)
    ===============================
    Prompt: The future of AI is
    Generated text:  fast approaching – and that's true for AI-in-All fields, including healthcare, manufacturing, education and more. The future of AI is bright, as we've seen in the use of AI in the healthcare field in the wake of the recent outbreak of the novel coronavirus (COVID-19). AI has the potential to provide a new way of looking at and diagnosing diseases, as well as a new way of predicting the spread of the virus and identifying how to control the spread. But like any other technology, the use of AI in these fields can have both benefits and drawbacks. The future of AI in healthcare is bright, but


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


    Generated text:  [Name], and I'm a [Age] year old [Occupation]. I'm a [Type of Character] who has always been [What motivates you?]. I'm passionate about [What is your favorite hobby or activity?]. I'm always looking for new experiences and learning new things. I'm a [What is your favorite thing about yourself?]. I'm always eager to learn and grow. I'm [What is your greatest strength?]. I'm always ready to help others and make a positive impact. I'm [What is your greatest weakness?]. I'm always willing to adapt and change. I'm [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris. 
    
    A. True
    B. False
    A. True
    
    Paris is the capital city of France, and it is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. The city is also famous for its rich history, including the French Revolution and the French Revolution Monument. Paris is a bustling metropolis with a diverse population and a rich cultural heritage. The city is home to many famous artists, writers, and musicians, and it is a popular tourist destination. Despite its size, Paris is a relatively small city with a population of around 1.3 million
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in several key areas, including:
    
    1. Increased integration with human intelligence: AI systems will become more integrated with human intelligence, allowing them to learn from and adapt to human behavior and decision-making processes.
    
    2. Enhanced machine learning capabilities: AI will continue to improve its ability to learn from data and make more accurate predictions and decisions.
    
    3. Increased use of AI in healthcare: AI will be used to improve the accuracy and efficiency of medical diagnosis and treatment, as well as to develop new treatments and therapies.
    
    4. Greater use of AI in automation: AI will be used to automate repetitive and mundane tasks,
    


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
    Generated text:  [Your Name], and I am [Your occupation] with over [Number of years] years of experience in [Your profession], specializing in [Your field of expertise]. My journey into this industry began when I was just [Number of years] years old and decided to pursue a career that would allow me to make a positive impact on the world. Since then, I have worked tirelessly to advance my knowledge and skills in [Your field of expertise] and have achieved notable success. I am always looking for new opportunities to learn and grow within this industry and beyond. I am committed to sharing my experiences and knowledge with others and helping them achieve
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    A) It is a historical and cultural center, known for its art and architecture. 
    B) It is the largest city in the country, serving as the capital of France. 
    C) It is the largest city in the world, serving as the capital of France. 
    D) It is the most populous city in the country, serving as the capital of France. 
    C) It is the largest city in the world, serving as the capital of France. 
    
    I believe that option C is the correct answer, as it accurately describes Paris as the largest city in the world, serving as the capital of France.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to see continued advancements in technology and an increase in the use of AI in various areas of life. Some possible future trends include:
    
    1. Advancements in AI algorithms and machine learning: As more data is collected and analyzed, AI algorithms and machine learning will become more sophisticated, allowing for more accurate and personalized outcomes.
    
    2. Integration of AI into everyday life: AI will become more integrated into daily life, from self-driving cars and robots assisting with household tasks to virtual assistants that can understand and respond to human language.
    
    3. Increased focus on ethical and social implications: AI will become more integrated into society, with ethical concerns and social implications


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

    occupation

    ]

     at

     [

    company

    ].

     I

    'm

     here

     to

     share

     my

     insights

     on

     [

    topic

     of

     interest

    ]

     with

     you

    .

     Let

    's

     get

     started

    !

     

    📊

    💼

    ✨

    
    


    ---
    


    Hey

     there

    !

     I

    ’m

     [

    Name

    ],

     a

     [

    occupation

    ]

     at

     [

    company

    ],

     and

     I

     wanted

     to

     take

     a

     moment

     to

     introduce

     myself

    .

     

    🌟

     Let

    ’s

     chat

     about

     [

    topic

     of

     interest

    ]

    !

     

    📜

    ✨

    
    


    ---
    


    If

     you

     have

     any

     questions

     or

     need

     further

     information

    ,

     feel

     free

     to

     reach

     out

    .

     I

     look

     forward

     to

     learning

     more

    !

     

    👋

    ✨

    
    


    ---
    


    —

     [

    Name

    ]
    


    ---
    


    #

     Self

    -

    Introduction

     Format

     

    📘

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     which

     is

     known

     for

     its

     iconic

     Notre

    -D

    ame

     Cathedral

    ,

     vibrant

     arts

     and

     culture

     scene

    ,

     and

     rich

     culinary

     heritage

    .

     It

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

     Lou

    vre

     Museum

    ,

     and

     the

     most

     famous

     landmark

    ,

     the

     E

    iff

    el

     Tower

    ,

     which

     has

     been

     in

     use

     since

     

    1

    8

    8

    9

    .

     The

     city

     is

     known

     for

     its

     beautiful

     architecture

    ,

     including

     the

     Arc

     de

     Tri

    omp

    he

    ,

     the

     Arc

     du

     Sole

    il

    ,

     and

     the

     Arc

     de

     Tri

    omp

    he

    .

     Additionally

    ,

     Paris

     is

     home

     to

     the

     Lou

    vre

     Museum

    ,

     where

     many

     famous

     artworks

     and

     sculptures

     can

     be

     seen

    .

     The

     city

     also

     has

     a

     long

     and

     rich

     history

    ,

     with

     important

     dates

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     very

     promising

     and

     there

     are

     several

     trends

     that

     are

     likely

     to

     shape

     the

     industry

     in

     the

     coming

     years

    .

     Here

     are

     some

     potential

     trends

     that

     could

     potentially

     shape

     the

     future

     of

     AI

    :
    


    1

    .

     Advanced

     Machine

     Learning

    :

     Machine

     learning

     is

     likely

     to

     become

     even

     more

     sophisticated

     in

     the

     coming

     years

    .

     This

     could

     involve

     deep

     learning

    ,

     reinforcement

     learning

    ,

     and

     other

     advanced

     algorithms

     that

     are

     capable

     of

     understanding

     complex

     patterns

     and

     making

     predictions

    .
    


    2

    .

     AI

     Ethics

    :

     As

     AI

     becomes

     more

     prevalent

    ,

     there

     will

     be

     a

     growing

     focus

     on

     the

     ethical

     implications

     of

     AI

     development

     and

     deployment

    .

     This

     could

     include

     questions

     about

     the

     impact

     of

     AI

     on

     human

     jobs

    ,

     privacy

    ,

     and

     fairness

    .
    


    3

    .

     Predict

    ive

     Analytics

    :

    



```python
llm.shutdown()
```
