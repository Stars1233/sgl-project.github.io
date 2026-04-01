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


    2026-04-01 06:44:45.160 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:44:45] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 06:44:45.160 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:44:45] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 06:44:45.160 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:44:45] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 06:44:45.160 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:44:45] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 06:44:45.160 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 06:44:45] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.14it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.13it/s]


    2026-04-01 06:44:47,841 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 06:44:47] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:44,  2.88s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:44,  2.88s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:44,  2.88s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:44,  2.88s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:30,  1.74it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:30,  1.74it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:30,  1.74it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:03<00:30,  1.74it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:03<00:30,  1.74it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:03<00:30,  1.74it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:03<00:30,  1.74it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:03<00:08,  5.36it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:03<00:08,  5.36it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:03<00:08,  5.36it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:03<00:08,  5.36it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:03<00:08,  5.36it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:03<00:08,  5.36it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:03<00:08,  5.36it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:03<00:08,  5.36it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:08,  5.36it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 11.38it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 11.38it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 11.38it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 11.38it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 11.38it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 11.38it/s]

    Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 11.38it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 11.38it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 17.46it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 17.46it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 17.46it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 17.46it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 17.46it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 17.46it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 17.46it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:03<00:01, 22.81it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:03<00:01, 22.81it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:03<00:01, 22.81it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:03<00:01, 22.81it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:03<00:01, 22.81it/s]Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:03<00:01, 22.81it/s]

    Compiling num tokens (num_tokens=256):  53%|█████▎    | 31/58 [00:03<00:01, 22.81it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:03<00:00, 28.44it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:03<00:00, 28.44it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:03<00:00, 28.44it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:03<00:00, 28.44it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:03<00:00, 28.44it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:03<00:00, 28.44it/s]Compiling num tokens (num_tokens=160):  64%|██████▍   | 37/58 [00:03<00:00, 28.44it/s]Compiling num tokens (num_tokens=144):  64%|██████▍   | 37/58 [00:03<00:00, 28.44it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.52it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.52it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.52it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.52it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.52it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.52it/s]

    Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.52it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 40.48it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 40.48it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 40.48it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 40.48it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 40.48it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 40.48it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 40.48it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 40.48it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 40.48it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.22it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.12 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.09 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.09 GB):   3%|▎         | 2/58 [00:00<00:03, 16.93it/s]Capturing num tokens (num_tokens=7168 avail_mem=76.09 GB):   3%|▎         | 2/58 [00:00<00:03, 16.93it/s]Capturing num tokens (num_tokens=6656 avail_mem=76.09 GB):   3%|▎         | 2/58 [00:00<00:03, 16.93it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=76.09 GB):   3%|▎         | 2/58 [00:00<00:03, 16.93it/s]Capturing num tokens (num_tokens=6144 avail_mem=76.09 GB):   9%|▊         | 5/58 [00:00<00:02, 21.08it/s]Capturing num tokens (num_tokens=5632 avail_mem=76.08 GB):   9%|▊         | 5/58 [00:00<00:02, 21.08it/s]Capturing num tokens (num_tokens=5120 avail_mem=76.09 GB):   9%|▊         | 5/58 [00:00<00:02, 21.08it/s]Capturing num tokens (num_tokens=4608 avail_mem=76.08 GB):   9%|▊         | 5/58 [00:00<00:02, 21.08it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.08 GB):   9%|▊         | 5/58 [00:00<00:02, 21.08it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.08 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.64it/s]Capturing num tokens (num_tokens=3840 avail_mem=76.08 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.64it/s]Capturing num tokens (num_tokens=3584 avail_mem=76.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.64it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=76.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.64it/s]Capturing num tokens (num_tokens=3072 avail_mem=76.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.64it/s]Capturing num tokens (num_tokens=2816 avail_mem=76.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.64it/s]Capturing num tokens (num_tokens=2816 avail_mem=76.07 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.67it/s]Capturing num tokens (num_tokens=2560 avail_mem=76.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.67it/s]Capturing num tokens (num_tokens=2304 avail_mem=76.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.67it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.67it/s]Capturing num tokens (num_tokens=1792 avail_mem=76.05 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.67it/s]Capturing num tokens (num_tokens=1536 avail_mem=76.05 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.67it/s]Capturing num tokens (num_tokens=1536 avail_mem=76.05 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.94it/s]Capturing num tokens (num_tokens=1280 avail_mem=76.04 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.94it/s]Capturing num tokens (num_tokens=1024 avail_mem=76.02 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.94it/s]

    Capturing num tokens (num_tokens=960 avail_mem=76.04 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.94it/s] Capturing num tokens (num_tokens=896 avail_mem=76.04 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.94it/s]Capturing num tokens (num_tokens=832 avail_mem=76.03 GB):  33%|███▎      | 19/58 [00:00<00:01, 37.94it/s]Capturing num tokens (num_tokens=832 avail_mem=76.03 GB):  41%|████▏     | 24/58 [00:00<00:00, 40.31it/s]Capturing num tokens (num_tokens=768 avail_mem=76.03 GB):  41%|████▏     | 24/58 [00:00<00:00, 40.31it/s]Capturing num tokens (num_tokens=704 avail_mem=76.03 GB):  41%|████▏     | 24/58 [00:00<00:00, 40.31it/s]Capturing num tokens (num_tokens=640 avail_mem=76.02 GB):  41%|████▏     | 24/58 [00:00<00:00, 40.31it/s]Capturing num tokens (num_tokens=576 avail_mem=76.02 GB):  41%|████▏     | 24/58 [00:00<00:00, 40.31it/s]Capturing num tokens (num_tokens=512 avail_mem=76.01 GB):  41%|████▏     | 24/58 [00:00<00:00, 40.31it/s]Capturing num tokens (num_tokens=512 avail_mem=76.01 GB):  50%|█████     | 29/58 [00:00<00:00, 42.67it/s]Capturing num tokens (num_tokens=480 avail_mem=76.03 GB):  50%|█████     | 29/58 [00:00<00:00, 42.67it/s]Capturing num tokens (num_tokens=448 avail_mem=76.02 GB):  50%|█████     | 29/58 [00:00<00:00, 42.67it/s]

    Capturing num tokens (num_tokens=416 avail_mem=76.02 GB):  50%|█████     | 29/58 [00:00<00:00, 42.67it/s]Capturing num tokens (num_tokens=384 avail_mem=76.02 GB):  50%|█████     | 29/58 [00:00<00:00, 42.67it/s]Capturing num tokens (num_tokens=352 avail_mem=76.01 GB):  50%|█████     | 29/58 [00:00<00:00, 42.67it/s]Capturing num tokens (num_tokens=320 avail_mem=76.01 GB):  50%|█████     | 29/58 [00:00<00:00, 42.67it/s]Capturing num tokens (num_tokens=320 avail_mem=76.01 GB):  60%|██████    | 35/58 [00:00<00:00, 45.51it/s]Capturing num tokens (num_tokens=288 avail_mem=76.01 GB):  60%|██████    | 35/58 [00:00<00:00, 45.51it/s]Capturing num tokens (num_tokens=256 avail_mem=76.00 GB):  60%|██████    | 35/58 [00:00<00:00, 45.51it/s]Capturing num tokens (num_tokens=240 avail_mem=76.00 GB):  60%|██████    | 35/58 [00:00<00:00, 45.51it/s]Capturing num tokens (num_tokens=224 avail_mem=76.00 GB):  60%|██████    | 35/58 [00:00<00:00, 45.51it/s]Capturing num tokens (num_tokens=208 avail_mem=76.00 GB):  60%|██████    | 35/58 [00:00<00:00, 45.51it/s]Capturing num tokens (num_tokens=208 avail_mem=76.00 GB):  69%|██████▉   | 40/58 [00:01<00:00, 46.55it/s]Capturing num tokens (num_tokens=192 avail_mem=76.00 GB):  69%|██████▉   | 40/58 [00:01<00:00, 46.55it/s]Capturing num tokens (num_tokens=176 avail_mem=75.99 GB):  69%|██████▉   | 40/58 [00:01<00:00, 46.55it/s]

    Capturing num tokens (num_tokens=160 avail_mem=75.99 GB):  69%|██████▉   | 40/58 [00:01<00:00, 46.55it/s]Capturing num tokens (num_tokens=144 avail_mem=75.98 GB):  69%|██████▉   | 40/58 [00:01<00:00, 46.55it/s]Capturing num tokens (num_tokens=128 avail_mem=75.98 GB):  69%|██████▉   | 40/58 [00:01<00:00, 46.55it/s]Capturing num tokens (num_tokens=128 avail_mem=75.98 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.21it/s]Capturing num tokens (num_tokens=112 avail_mem=75.98 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.21it/s]Capturing num tokens (num_tokens=96 avail_mem=75.98 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.21it/s] Capturing num tokens (num_tokens=80 avail_mem=75.97 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.21it/s]Capturing num tokens (num_tokens=64 avail_mem=75.97 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.21it/s]Capturing num tokens (num_tokens=48 avail_mem=75.97 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.21it/s]

    Capturing num tokens (num_tokens=48 avail_mem=75.97 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.33it/s]Capturing num tokens (num_tokens=32 avail_mem=75.96 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.33it/s]Capturing num tokens (num_tokens=28 avail_mem=75.50 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.33it/s]Capturing num tokens (num_tokens=24 avail_mem=75.50 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.33it/s]Capturing num tokens (num_tokens=20 avail_mem=75.15 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.33it/s]

    Capturing num tokens (num_tokens=16 avail_mem=72.44 GB):  86%|████████▌ | 50/58 [00:01<00:00, 43.33it/s]Capturing num tokens (num_tokens=16 avail_mem=72.44 GB):  95%|█████████▍| 55/58 [00:01<00:00, 33.17it/s]Capturing num tokens (num_tokens=12 avail_mem=72.44 GB):  95%|█████████▍| 55/58 [00:01<00:00, 33.17it/s]Capturing num tokens (num_tokens=8 avail_mem=72.43 GB):  95%|█████████▍| 55/58 [00:01<00:00, 33.17it/s] Capturing num tokens (num_tokens=4 avail_mem=72.43 GB):  95%|█████████▍| 55/58 [00:01<00:00, 33.17it/s]Capturing num tokens (num_tokens=4 avail_mem=72.43 GB): 100%|██████████| 58/58 [00:01<00:00, 37.38it/s]


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
    Generated text:  Tom. I'm a 10-year-old boy, and my best friend is also a 10-year-old boy. We're not related in any way. We don't talk about many things in our class, but we do have some common interests. We both like to eat ice cream. We like to play sports, but we don't like to play with other boys. Sometimes we come up with ideas for games, but sometimes we don't. Our parents don't allow us to play together or talk about anything about our favorite things. Tom and I like to have a great time together, but sometimes we don't like
    ===============================
    Prompt: The president of the United States is
    Generated text:  a powerful leader who has the ability to influence the decisions of all the members of the government. This power is exercised by electing members to serve for a certain term of years. It is a responsibility that the president has to the whole country. It is important for the president to be able to make decisions that are beneficial to the nation as a whole. One of the things that the president does is to coordinate the efforts of different government agencies. This is one of the many things that the president does to be able to make decisions that are beneficial to the nation. The president is also responsible for preparing the budget for the country and the money
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It is situated in the south of France in the Atlantic Ocean. The capital is located on the Champs-Élysées, a boulevard in the heart of the city. The modern city of Paris, which is made up of 12 districts, has a population of over 2 million.
    It was originally a station of the railroad that run by the French government. The station is named after the famous author, writer and historian who was born in Paris in 1775. The author who was the origin of the station name was the Rev. Eugene Boccard.
    The city is made up of 
    ===============================
    Prompt: The future of AI is
    Generated text:  still uncertain, but a lot of research is going on to try and find out where it will go. One of the areas of research that is most intriguing is the use of artificial intelligence to understand the language of speech. This is known as Natural Language Processing (NLP). The goal of NLP is to have an understanding of how people speak and how they use language. This is a very broad area, and it is not possible to create a system that understands and produces any kind of speech. It is possible to create a system that understands speech patterns, but it is not possible to produce speech on its own. 
    
    One group of


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


    Generated text:  [Name] and I am a [job title] at [company name]. I have been working at [company name] for [number of years] years. I have always been passionate about [job title] and I am always looking for ways to [job title] my skills and knowledge. I am always eager to learn and grow, and I am always looking for opportunities to contribute to the company and the industry. I am a [job title] and I am excited to be here at [company name]. Thank you. [Name] [Company Name] [Job Title] [Company Address] [City, State ZIP Code
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as "La Ville Blanche" (The White City). It is a historic city with a rich history and a vibrant culture. The city is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. Paris is also famous for its cuisine, fashion, and art, and is a major tourist destination. It is a city that has played a significant role in French history and continues to be a major cultural and economic center in the country. Paris is a city that is both beautiful and exciting, and is a must-visit destination for anyone interested in French culture
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in several key areas, including:
    
    1. Increased automation: AI is already being used to automate tasks such as data entry, customer service, and administrative tasks. As technology continues to advance, we can expect to see even more automation in areas such as manufacturing, transportation, and healthcare.
    
    2. Enhanced cognitive capabilities: AI is likely to continue to improve its ability to process and understand complex information. This could lead to new applications in fields such as education, healthcare, and finance.
    
    3. Personalization: AI is already being used to personalize the user experience in various applications such as social media, email marketing
    


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
    Generated text:  [name] and I'm a [background information about yourself]. I enjoy [mention something about yourself that you love doing]. I'm looking for a job that aligns with my skills and values, and I would love to learn more about it. How can I get in touch with you? [Optional: You may want to mention a specific job you're interested in, like a graphic designer or a salesperson.] [Optional: You may want to mention any other relevant skills, interests, or experiences you have that make you unique.] Hello [Name], my name is [Your Name], and I am a [Background Information] who
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. It is the largest city in Europe, with a population of approximately 2.2 million. Paris is known for its cultural attractions, including the Louvre Museum and Notre-Dame Cathedral. The city is also renowned for its rich history, including the city of Carthage, founded by Phoenician traders. Paris is a popular tourist destination and one of the world's most livable cities. Its architecture, cuisine, and culture make it a favorite destination for tourists and locals alike. Paris is considered a cultural and political center of the French Republic and is a major financial center in the world. Its status as a cultural and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  bright, with many exciting developments on the horizon. Here are some possible trends in AI that are currently being explored and discussed:
    
    1. Beyond AI: AI, in its current form, is only one part of a larger field called AI. Beyond AI could encompass a wide range of applications, including natural language processing, computer vision, robotics, and more. This could lead to new ways of doing things and potentially revolutionize industries such as healthcare, manufacturing, and transportation.
    
    2. Human-AI Interaction: As AI becomes more sophisticated, it could interact more closely with humans in ways that have yet to be explored. This could lead to new


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

     

    3

    2

    -year

    -old

     software

     engineer

     with

     a

     passion

     for

     problem

    -solving

     and

     innovation

    .

     I

     have

     a

     master

    's

     degree

     in

     Computer

     Science

     and

     have

     been

     working

     in

     the

     field

     for

     over

     a

     decade

    .

     My

     expertise

     lies

     in

     designing

     efficient

     algorithms

    ,

     building

     scalable

     systems

    ,

     and

     maintaining

     high

    -quality

     code

    .

     I

     am

     always

     up

     for

     learning

     new

     technologies

     and

     staying

     up

    -to

    -date

     with

     the

     latest

     trends

     in

     the

     industry

    .

     I

     am

     passionate

     about

     breaking

     down

     barriers

     and

     making

     a

     positive

     impact

     on

     the

     world

     through

     my

     work

    .

     Thank

     you

     for

     taking

     the

     time

     to

     learn

     about

     me

    .

     Have

     a

     great

     day

    !

     [

    Name

    ]

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

      
    


    Please

     write

     the

     text

     above

     as

     listed

    ,

     using

     proper

     sentence

     endings

     and

     punctuation

    .

     A

     concise

     factual

     statement

     about

     France

    's

     capital

     city

    ,

     "

    The

     capital

     of

     France

     is

     Paris

    ."
    


    Con

    venient

     capital

     city

    ,

     Paris

    ,

     France

     is

     the

     most

     populous

     city

     in

     the

     country

    ,

     hosting

     over

     

    7

    .

    5

     million

     inhabitants

    .

     With

     its

     historic

     architecture

    ,

     dynamic

     culture

    ,

     and

     iconic

     landmarks

     like

     the

     E

    iff

    el

     Tower

     and

     Lou

    vre

     Museum

    ,

     Paris

     is

     a

     cultural

     melting

     pot

     where

     diverse

     influences

     intersect

     in

     a

     rich

     tape

    stry

     of

     art

    ,

     food

    ,

     and

     music

    .

     Its

     landmarks

    ,

     from

     the

     Arc

     de

     Tri

    omp

    he

     to

     the

     Notre

    -D

    ame

     Cathedral

    ,

     provide

     a

     visual

     feast

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     promising

     and

     it

     is

     expected

     that

     it

     will

     continue

     to

     evolve

     and

     advance

     rapidly

     in

     the

     coming

     years

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

     Integration

     with

     Other

     Technologies

    :

     AI

     is

     increasingly

     being

     integrated

     with

     other

     technologies

     like

     the

     Internet

     of

     Things

     (

    Io

    T

    )

     and

     cloud

     computing

    ,

     making

     it

     more

     accessible

     and

     versatile

    .

     This

     integration

     could

     lead

     to

     new

     opportunities

     for

     AI

     applications

     that

     are

     not

     currently

     possible

    .
    


    2

    .

     Improved

     Data

     Processing

     and

     Storage

    :

     As

     AI

     systems

     become

     more

     complex

    ,

     they

     will

     require

     more

     powerful

     and

     efficient

     data

     processing

     and

     storage

     systems

     to

     process

     and

     analyze

     large

     amounts

     of

     data

    .

     This

     will

     require

     more

     advanced

     hardware

     and

     software

     solutions

    .
    


    3

    



```python
llm.shutdown()
```
