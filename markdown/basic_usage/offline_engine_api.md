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


    2026-03-30 13:26:08.350 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 13:26:08] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 13:26:08.350 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 13:26:08] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 13:26:08.350 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 13:26:08] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 13:26:08.350 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 13:26:08] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 13:26:08.350 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 13:26:08] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.54it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.53it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:22,  2.33it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:07,  6.14it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  6.14it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.44it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.44it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.44it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.44it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.44it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.44it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.44it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.44it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.52it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.52it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.52it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.52it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.52it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.52it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.52it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.52it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 25.12it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 25.12it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 25.12it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 25.12it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 25.12it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 25.12it/s]

    Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 25.12it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 30.17it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 30.17it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 30.17it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 30.17it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 30.17it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 30.17it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 30.17it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 35.11it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 35.11it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 35.11it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 35.11it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 35.11it/s]

    Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 35.11it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 35.11it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 39.27it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 39.27it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 39.27it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 39.27it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 39.27it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 39.27it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 39.27it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 39.27it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.81it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.83 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.80 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.80 GB):   3%|▎         | 2/58 [00:00<00:02, 19.04it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.79 GB):   3%|▎         | 2/58 [00:00<00:02, 19.04it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.79 GB):   3%|▎         | 2/58 [00:00<00:02, 19.04it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.78 GB):   3%|▎         | 2/58 [00:00<00:02, 19.04it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=118.78 GB):   9%|▊         | 5/58 [00:00<00:02, 20.92it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.77 GB):   9%|▊         | 5/58 [00:00<00:02, 20.92it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.75 GB):   9%|▊         | 5/58 [00:00<00:02, 20.92it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.75 GB):   9%|▊         | 5/58 [00:00<00:02, 20.92it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.75 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.77it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.75 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.77it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.74 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.77it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=118.74 GB):  14%|█▍        | 8/58 [00:00<00:02, 22.77it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.74 GB):  19%|█▉        | 11/58 [00:00<00:02, 21.36it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.74 GB):  19%|█▉        | 11/58 [00:00<00:02, 21.36it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.73 GB):  19%|█▉        | 11/58 [00:00<00:02, 21.36it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.73 GB):  19%|█▉        | 11/58 [00:00<00:02, 21.36it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.73 GB):  24%|██▍       | 14/58 [00:00<00:02, 21.88it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.73 GB):  24%|██▍       | 14/58 [00:00<00:02, 21.88it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=118.72 GB):  24%|██▍       | 14/58 [00:00<00:02, 21.88it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.72 GB):  24%|██▍       | 14/58 [00:00<00:02, 21.88it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.72 GB):  29%|██▉       | 17/58 [00:00<00:01, 23.13it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.72 GB):  29%|██▉       | 17/58 [00:00<00:01, 23.13it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.71 GB):  29%|██▉       | 17/58 [00:00<00:01, 23.13it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.71 GB):  29%|██▉       | 17/58 [00:00<00:01, 23.13it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.71 GB):  34%|███▍      | 20/58 [00:00<00:01, 24.71it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.69 GB):  34%|███▍      | 20/58 [00:00<00:01, 24.71it/s]

    Capturing num tokens (num_tokens=960 avail_mem=118.70 GB):  34%|███▍      | 20/58 [00:00<00:01, 24.71it/s] Capturing num tokens (num_tokens=896 avail_mem=118.70 GB):  34%|███▍      | 20/58 [00:00<00:01, 24.71it/s]Capturing num tokens (num_tokens=832 avail_mem=118.70 GB):  34%|███▍      | 20/58 [00:00<00:01, 24.71it/s]Capturing num tokens (num_tokens=832 avail_mem=118.70 GB):  41%|████▏     | 24/58 [00:00<00:01, 27.26it/s]Capturing num tokens (num_tokens=768 avail_mem=118.69 GB):  41%|████▏     | 24/58 [00:00<00:01, 27.26it/s]Capturing num tokens (num_tokens=704 avail_mem=118.69 GB):  41%|████▏     | 24/58 [00:01<00:01, 27.26it/s]Capturing num tokens (num_tokens=640 avail_mem=118.69 GB):  41%|████▏     | 24/58 [00:01<00:01, 27.26it/s]Capturing num tokens (num_tokens=576 avail_mem=118.69 GB):  41%|████▏     | 24/58 [00:01<00:01, 27.26it/s]Capturing num tokens (num_tokens=512 avail_mem=118.68 GB):  41%|████▏     | 24/58 [00:01<00:01, 27.26it/s]

    Capturing num tokens (num_tokens=512 avail_mem=118.68 GB):  50%|█████     | 29/58 [00:01<00:00, 32.60it/s]Capturing num tokens (num_tokens=480 avail_mem=118.69 GB):  50%|█████     | 29/58 [00:01<00:00, 32.60it/s]Capturing num tokens (num_tokens=448 avail_mem=118.69 GB):  50%|█████     | 29/58 [00:01<00:00, 32.60it/s]Capturing num tokens (num_tokens=416 avail_mem=118.69 GB):  50%|█████     | 29/58 [00:01<00:00, 32.60it/s]Capturing num tokens (num_tokens=384 avail_mem=118.69 GB):  50%|█████     | 29/58 [00:01<00:00, 32.60it/s]Capturing num tokens (num_tokens=384 avail_mem=118.69 GB):  57%|█████▋    | 33/58 [00:01<00:00, 33.62it/s]Capturing num tokens (num_tokens=352 avail_mem=118.68 GB):  57%|█████▋    | 33/58 [00:01<00:00, 33.62it/s]Capturing num tokens (num_tokens=320 avail_mem=118.68 GB):  57%|█████▋    | 33/58 [00:01<00:00, 33.62it/s]Capturing num tokens (num_tokens=288 avail_mem=118.67 GB):  57%|█████▋    | 33/58 [00:01<00:00, 33.62it/s]Capturing num tokens (num_tokens=256 avail_mem=118.67 GB):  57%|█████▋    | 33/58 [00:01<00:00, 33.62it/s]

    Capturing num tokens (num_tokens=256 avail_mem=118.67 GB):  64%|██████▍   | 37/58 [00:01<00:00, 33.62it/s]Capturing num tokens (num_tokens=240 avail_mem=118.67 GB):  64%|██████▍   | 37/58 [00:01<00:00, 33.62it/s]Capturing num tokens (num_tokens=224 avail_mem=118.66 GB):  64%|██████▍   | 37/58 [00:01<00:00, 33.62it/s]Capturing num tokens (num_tokens=208 avail_mem=118.66 GB):  64%|██████▍   | 37/58 [00:01<00:00, 33.62it/s]Capturing num tokens (num_tokens=192 avail_mem=118.66 GB):  64%|██████▍   | 37/58 [00:01<00:00, 33.62it/s]Capturing num tokens (num_tokens=192 avail_mem=118.66 GB):  71%|███████   | 41/58 [00:01<00:00, 33.96it/s]Capturing num tokens (num_tokens=176 avail_mem=118.66 GB):  71%|███████   | 41/58 [00:01<00:00, 33.96it/s]Capturing num tokens (num_tokens=160 avail_mem=118.65 GB):  71%|███████   | 41/58 [00:01<00:00, 33.96it/s]Capturing num tokens (num_tokens=144 avail_mem=118.65 GB):  71%|███████   | 41/58 [00:01<00:00, 33.96it/s]

    Capturing num tokens (num_tokens=128 avail_mem=118.65 GB):  71%|███████   | 41/58 [00:01<00:00, 33.96it/s]Capturing num tokens (num_tokens=128 avail_mem=118.65 GB):  78%|███████▊  | 45/58 [00:01<00:00, 32.04it/s]Capturing num tokens (num_tokens=112 avail_mem=118.64 GB):  78%|███████▊  | 45/58 [00:01<00:00, 32.04it/s]Capturing num tokens (num_tokens=96 avail_mem=118.64 GB):  78%|███████▊  | 45/58 [00:01<00:00, 32.04it/s] Capturing num tokens (num_tokens=80 avail_mem=118.64 GB):  78%|███████▊  | 45/58 [00:01<00:00, 32.04it/s]

    Capturing num tokens (num_tokens=64 avail_mem=118.63 GB):  78%|███████▊  | 45/58 [00:01<00:00, 32.04it/s]Capturing num tokens (num_tokens=64 avail_mem=118.63 GB):  84%|████████▍ | 49/58 [00:01<00:00, 21.43it/s]Capturing num tokens (num_tokens=48 avail_mem=118.63 GB):  84%|████████▍ | 49/58 [00:01<00:00, 21.43it/s]Capturing num tokens (num_tokens=32 avail_mem=118.62 GB):  84%|████████▍ | 49/58 [00:01<00:00, 21.43it/s]Capturing num tokens (num_tokens=28 avail_mem=118.62 GB):  84%|████████▍ | 49/58 [00:02<00:00, 21.43it/s]Capturing num tokens (num_tokens=28 avail_mem=118.62 GB):  90%|████████▉ | 52/58 [00:02<00:00, 22.16it/s]Capturing num tokens (num_tokens=24 avail_mem=118.62 GB):  90%|████████▉ | 52/58 [00:02<00:00, 22.16it/s]Capturing num tokens (num_tokens=20 avail_mem=118.61 GB):  90%|████████▉ | 52/58 [00:02<00:00, 22.16it/s]

    Capturing num tokens (num_tokens=16 avail_mem=118.61 GB):  90%|████████▉ | 52/58 [00:02<00:00, 22.16it/s]Capturing num tokens (num_tokens=12 avail_mem=118.61 GB):  90%|████████▉ | 52/58 [00:02<00:00, 22.16it/s]Capturing num tokens (num_tokens=8 avail_mem=118.61 GB):  90%|████████▉ | 52/58 [00:02<00:00, 22.16it/s] Capturing num tokens (num_tokens=8 avail_mem=118.61 GB):  98%|█████████▊| 57/58 [00:02<00:00, 27.23it/s]Capturing num tokens (num_tokens=4 avail_mem=118.60 GB):  98%|█████████▊| 57/58 [00:02<00:00, 27.23it/s]Capturing num tokens (num_tokens=4 avail_mem=118.60 GB): 100%|██████████| 58/58 [00:02<00:00, 26.68it/s]


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
    Generated text:  Ava and I'm a Marketing and Business Development professional. I've always had a love for helping people create success, so I create content that helps people succeed. I work with a variety of industries, including health and wellness, tech, and fitness. I'm dedicated to providing quality content that resonates with my audience and helps them achieve their goals. I like to stay updated on the latest trends and strategies in marketing and business development to stay ahead of the curve.
    Could you please share your experience and expertise in content creation and digital marketing, including any specific projects or experiences you've had with your clients or organizations? Ava has been a marketing
    ===============================
    Prompt: The president of the United States is
    Generated text:  20 years older than the president of Brazil, and the president of Brazil is 30 years younger than the president of France. If France is 3 times older than Germany, and Germany is 10 years older than Spain, and Spain is 2 years younger than Italy, how old is the president of France? Let's denote the age of the president of France as \( F \).
    
    Given the relationships between the ages of the presidents, we can express the ages of the other presidents in terms of \( F \):
    
    1. The president of Brazil is 30 years younger than the president of France, so the
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, and it is located on which of the following?
    A) The River Seine
    B) The Rhine
    C) The Atlantic
    D) The Mediterranean
    
    A) The River Seine
    
    The capital of France is indeed Paris, and it is located on the River Seine. The River Seine is one of the longest rivers in the world and runs through Paris, France. The other options listed are not actually rivers that flow through the capital of France. The Atlantic is the largest body of water in the world and not a part of the capital of France. The Mediterranean, on the other hand, is located to
    ===============================
    Prompt: The future of AI is
    Generated text:  where humans are the most critical.
    The future of AI is where humans are the most critical.
    Artificial intelligence (AI) is a growing field, and it has the potential to dramatically change the way we work and live. As the technology advances, AI is becoming increasingly integrated into our daily lives, and it is essential that we understand how it works and how we can use it to our advantage.
    One of the most important aspects of AI is its ability to learn and adapt. Unlike human beings, AI cannot memorize facts or formulas. It must be able to learn new information and adapt to new situations. This means that AI systems must


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


    Generated text:  [Name] and I'm a [Age] year old [Gender] [Occupation]. I'm a [Skill or Hobby] enthusiast who loves to [Describe a hobby or interest]. I'm always looking for new experiences and learning new things, and I'm always eager to share my knowledge and skills with others. I'm a [Favorite Subject] lover who enjoys [Describe a favorite subject or hobby]. I'm a [Favorite Book] lover who has read many books and have a deep appreciation for literature. I'm a [Favorite Movie] lover who has watched many movies and have a love for [Describe a favorite movie or TV
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and Louvre Museum. It is also a major cultural and economic center, hosting numerous world-renowned museums, theaters, and festivals. Paris is a popular tourist destination and a major hub for international business and diplomacy. The city is known for its rich history, art, and cuisine, and is a major cultural and economic center of France. It is also home to the French Parliament and the French Senate. Paris is a major hub for international business and diplomacy, and is a popular tourist destination. The city is known for its rich history,
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased automation and robotics: As AI technology continues to advance, we are likely to see an increase in automation and robotics in various industries. This will lead to the development of new jobs and the displacement of existing ones, but it will also create new opportunities for people to work in areas such as data analysis, software development, and robotics.
    
    2. AI ethics and privacy: As AI technology becomes more advanced, there
    


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
    Generated text:  [Your Name], and I'm a [Your Major/Position/Field]. If you haven't noticed, I'm a [Your Major/Position/Field], and I've always been passionate about [Your Passion/Affinity]. I love [Your Interests/Affinity] and I'm a natural leader. I've always wanted to be in this field and I've worked hard to achieve my goals. What's your favorite hobby or activity to do? How do you stay motivated? As a [Your Major/Position/Field], I work on [Your Area of Expertise/Profession]. I'm always looking for new
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known as "La Palme de la Connaissance," where many famous landmarks, including the Eiffel Tower, the Louvre, and the Notre-Dame Cathedral, are located. Paris is the cultural, economic, and political center of France and has a long and rich history dating back to the Roman Empire. It is also one of the most famous cities in the world, attracting visitors from all over the world. Its rich history and stunning architecture make it a popular tourist destination. Paris is a unique and fascinating city with a rich cultural heritage, diverse cuisine, and a unique perspective on French society and history. Paris is also
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  highly uncertain and subject to change. Here are some potential trends that could potentially shape the future of AI:
    
    1. Increased Integration with Human Work: As AI becomes more advanced and complex, it could become more integrated with human work. This could lead to the development of AI that can perform tasks that are now done by humans, but is also more efficient and accurate.
    
    2. Autonomous Systems: One of the biggest trends in AI is the development of autonomous systems. These systems could be used in a wide range of applications, from self-driving cars to drones to automated healthcare systems.
    
    3. Greater Use of AI in Healthcare: With the increasing use


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

    insert

     your

     name

    ]

     and

     I

    'm

     a

     [

    insert

     profession

     or

     background

    ]

    !

     I

    'm

     here

     to

     share

     my

     experiences

    ,

     knowledge

    ,

     and

     passion

     for

     [

    insert

     something

     interesting

     or

     meaningful

     about

     your

     occupation

     or

     hobby

    ].

     Let

    's

     connect

     to

     explore

     new

     ideas

    ,

     learn

     new

     skills

    ,

     and

     share

     our

     mutual

     interests

    .

     I

     look

     forward

     to

     meeting

     you

     and

     learning

     more

     about

     you

    !

     

    ✌

    ️

    🌟

    
    


    This

     self

    -int

    roduction

     is

     quite

     long

    .

     Can

     you

     suggest

     a

     shorter

     version

    ?

     Absolutely

    !

     Here

    's

     a

     more

     concise

     version

    :
    


    Hello

    ,

     my

     name

     is

     [

    insert

     your

     name

    ]

     and

     I

    'm

     a

     [

    insert

     profession

     or

     background

    ]

    !

     I

    'm

     here

     to

     share

     my

     experiences

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

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

    .

     It

     is

     renowned

     for

     its

     rich

     history

    ,

     landmarks

    ,

     and

     world

    -class

     museums

    ,

     as

     well

     as

     its

     culinary

     scene

     and

     fashion

     scene

    .

     The

     city

     is

     known

     for

     its

     fashion

    ,

     theater

    ,

     and

     music

     scenes

    ,

     and

     has

     been

     the

     center

     of

     French

     culture

     since

     ancient

     times

    .

     Paris

     is

     often

     described

     as

     the

     "

    city

     of

     a

     thousand

     windows

    "

     and

     is

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

     Sac

    ré

    -C

    œur

     Basil

    ica

    ,

     and

     many

     other

     iconic

     landmarks

    .

     The

     city

     is

     also

     a

     global

     center

     of

     art

     and

     culture

    ,

     with

     numerous

     museums

    ,

     galleries

    ,

     and

     theaters

    ,

     and

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     be

     shaped

     by

     several

     key

     trends

    ,

     including

    :
    


    1

    .

     Increased

     Integration

     with

     Human

    -A

    ware

    ness

    :

     AI

     systems

     are

     expected

     to

     become

     more

     human

    -like

    ,

     integrating

     seamlessly

     with

     humans

     in

     decision

    -making

     processes

    .

     This

     will

     require

     AI

     systems

     to

     be

     more

     responsive

     to

     human

     emotions

     and

     behaviors

    ,

     as

     well

     as

     to

     be

     able

     to

     adapt

     to

     changing

     human

     needs

    .
    


    2

    .

     AI

     Ethics

     and

     Responsibility

    :

     As

     AI

     becomes

     more

     integrated

     with

     human

     life

    ,

     there

     will

     be

     increasing

     pressure

     on

     AI

     systems

     to

     be

     responsible

     for

     their

     actions

    .

     This

     will

     require

     AI

     systems

     to

     be

     transparent

    ,

     accountable

    ,

     and

     responsible

     for

     their

     outputs

    ,

     while

     also

     providing

     a

     means

     for

     decision

    -makers

     to

     hold

     them

     accountable

    



```python
llm.shutdown()
```
