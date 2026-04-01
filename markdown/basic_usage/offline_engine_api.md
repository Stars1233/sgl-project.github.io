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


    2026-04-01 20:18:43.454 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:18:43] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:18:43.454 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:18:43] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:18:43.454 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:18:43] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:18:43.454 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:18:43] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 20:18:43.455 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 20:18:43] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.48it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.46it/s]


    2026-04-01 20:18:46,348 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 20:18:46] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:44,  2.88s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:44,  2.88s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:44,  2.88s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:44,  2.88s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:44,  2.88s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:24,  2.20it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:24,  2.20it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:03<00:24,  2.20it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:03<00:24,  2.20it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:03<00:24,  2.20it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:03<00:24,  2.20it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:03<00:24,  2.20it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:08,  5.80it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:08,  5.80it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:08,  5.80it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:08,  5.80it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:08,  5.80it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:08,  5.80it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:08,  5.80it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:08,  5.80it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:08,  5.80it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 11.77it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 11.77it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 11.77it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 11.77it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 11.77it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 11.77it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 11.77it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 11.77it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 17.58it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 17.58it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 17.58it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 17.58it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 17.58it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 17.58it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 17.58it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 17.58it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:01, 23.85it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:01, 23.85it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:01, 23.85it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:01, 23.85it/s]

    Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:01, 23.85it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:01, 23.85it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:01, 23.85it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 27.84it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 27.84it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 27.84it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 27.84it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 27.84it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 27.84it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 27.84it/s]

    Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 32.09it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 32.09it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 32.09it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 32.09it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 32.09it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 32.09it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 32.09it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 36.30it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 36.30it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 36.30it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 36.30it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 36.30it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 36.30it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 36.30it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 36.30it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 14.86it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.33 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.49it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.49it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.49it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.49it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.15it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.15it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.15it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.15it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.15it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.01it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.01it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.01it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.01it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.01it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.28 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.36it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.36it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.36it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.36it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.26 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.36it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.26 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.42it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.42it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.42it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.42it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.42it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  29%|██▉       | 17/58 [00:00<00:01, 32.42it/s] Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.65it/s]Capturing num tokens (num_tokens=896 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.65it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.65it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.65it/s]Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.65it/s]Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 36.12it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 36.12it/s]

    Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 36.12it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  45%|████▍     | 26/58 [00:00<00:00, 36.12it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 36.12it/s]Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  45%|████▍     | 26/58 [00:00<00:00, 36.12it/s]Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:00<00:00, 37.65it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:00<00:00, 37.65it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  53%|█████▎    | 31/58 [00:00<00:00, 37.65it/s]Capturing num tokens (num_tokens=352 avail_mem=120.22 GB):  53%|█████▎    | 31/58 [00:00<00:00, 37.65it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.65it/s]Capturing num tokens (num_tokens=288 avail_mem=120.21 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.65it/s]

    Capturing num tokens (num_tokens=288 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 38.81it/s]Capturing num tokens (num_tokens=256 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 38.81it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 38.81it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  62%|██████▏   | 36/58 [00:01<00:00, 38.81it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  62%|██████▏   | 36/58 [00:01<00:00, 38.81it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 37.56it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 37.56it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 37.56it/s]Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 37.56it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 37.56it/s]

    Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  76%|███████▌  | 44/58 [00:01<00:00, 38.02it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  76%|███████▌  | 44/58 [00:01<00:00, 38.02it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  76%|███████▌  | 44/58 [00:01<00:00, 38.02it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  76%|███████▌  | 44/58 [00:01<00:00, 38.02it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  76%|███████▌  | 44/58 [00:01<00:00, 38.02it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  76%|███████▌  | 44/58 [00:01<00:00, 38.02it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.24it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.24it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.24it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.24it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.24it/s]

    Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.24it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.81it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.81it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.81it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.81it/s] Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.81it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 36.13it/s]


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
    Generated text:  Andy, I'm a bit of a sports enthusiast, and I'm keen on watching NFL football. Do you know what my favorite football player is and why I like them?
    
    Sure, you must be Andy, and you're right! One of my favorite football players is Tom Brady. He's a standout in NFL football, a legend, and a legend. Tom Brady's football skills are incredible, and he's a true leader on the field. He's not only a great athlete but also a great person who helps other players succeed. His work ethic and passion for the game are inspiring. And, he has a great smile and friendly
    ===============================
    Prompt: The president of the United States is
    Generated text:  seeking to understand the correlation between income inequality and healthcare costs. The income inequality index (I) is given by the function \( I(x) = 5000 + 150x \), where \( x \) represents the number of jobs in one's industry. The president wants to know the marginal rate of return on healthcare costs, which is defined as \( R(x) = \frac{dI}{dx} \). Additionally, he wants to determine the point where healthcare costs and income inequality intersect, assuming the healthcare cost function is given by \( C(x) = 10000 + 2
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris.
    
    Translate to French.
    
    English: The capital of France is Paris.
    
    French:
    
    Le capital de la France est Paris.
    
    Explication: 
    
    1. "Le" (le) est utilisé pour indiquer que le titre est le seul mot qui n'a pas de préposition.
    
    2. "Capitale" est le nom de la fonction qui est la traduction de "capital" en français.
    
    3. "de la France" est la preposition qui indique que le lieu est le pays.
    
    4. "Paris" est le nom du lieu et est la traduction de "France" en français.
    
    5. La
    ===============================
    Prompt: The future of AI is
    Generated text:  incredibly promising. AI is set to revolutionize virtually every industry, from healthcare to finance to transportation. The technology has already been used to improve safety, efficiency, and decision-making in various fields. This has led to an increase in demand for professionals with AI expertise.
    One of the most sought-after skills in the field of AI is that of a data scientist. A data scientist is a professional who uses data to inform and improve decision-making. They are responsible for gathering, analyzing, and interpreting data to provide valuable insights and make predictions.
    As AI technology continues to advance, the demand for data scientists will only increase. According to a report by


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a few details about your character, such as your personality, interests, or skills]. I'm always looking for new opportunities to grow and learn, and I'm always eager to contribute to the team. What do you think makes you a good fit for this role? I believe that I have the skills and experience to bring value to the team and contribute to the success of the company. What are some of your favorite things to do outside
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a bustling metropolis with a rich cultural heritage and is a major tourist destination. It is also a major economic center and a major player in international politics. The city is known for its fashion industry, art scene, and its role in French culture and identity. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly into one another. The city is also known
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased focus on ethical considerations: As AI becomes more integrated into our daily lives, there will be a growing emphasis on ethical considerations. This includes issues such as bias, transparency, accountability, and privacy. AI developers will need to take steps to ensure that their systems are fair and unbiased, and that they are transparent and accountable to their users.
    
    2. Greater use of AI in healthcare: AI is already being used in healthcare to help diagnose and treat diseases, and to improve patient outcomes. As
    


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
    Generated text:  [Name], and I'm [Age]. I'm a [Job or Profession] who has been working in the [Industry] for [Years] years. I enjoy [What I Do Best]. I'm always looking to learn new things and be the best version of myself. I'm excited to have the opportunity to talk to you today! [Insert a brief message about the conversation or event]. [Insert the end of the self-introduction] If you need anything, I'm always here to help! [Insert any additional information about the character, such as their favorite colors, pets, or hobbies]. Let's get to know each
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, which is the largest city in the country. 
    
    To address the problem, we need to identify the capital of France and then provide a concise statement that captures the key information. The capital of France is Paris, the largest city in the country. Therefore, the answer is:
    
    Paris is the capital of France. 
    
    To ensure accuracy and completeness, it's worth noting that while Paris is indeed the capital, it has other significant locations such as the Île-de-France region and the nearby suburbs, further highlighting its importance in French politics, economy, and culture. The answer should be clearly stated, particularly if it is a foundational
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be shaped by several trends and technologies that are emerging and evolving rapidly. Here are some of the most likely future trends in AI:
    
    1. Increased Automation: As AI continues to become more sophisticated, it is likely to automate many of the tasks that humans currently perform. This could include tasks such as data analysis, robotics, and natural language processing.
    
    2. Augmented Intelligence: AI is increasingly being used to enhance human capabilities, such as decision-making and problem-solving. This could lead to a "augmented intelligence" where AI is integrated with human intelligence to create a more complex and adaptive system.
    
    3. AI Ethics and Ethics AI


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

    Character

     Name

    ],

     and

     I

    'm

     a

     [

    职业

    /

    特长

    ]

     in

     the

     [

    行业

    /

    领域

    ]

     field

    .

     I

     have

     [

    amount

     of

     experience

    ]

     years

     of

     experience

     in

     this

     field

    ,

     and

     I

    'm

     passionate

     about

     [

    reason

     for

     passion

    ].

     I

    'm

     [

    role

     within

     the

     industry

    ,

     such

     as

     [

    director

    ,

     developer

    ,

     coach

    ,

     etc

    .

    ]],

     and

     I

    'm

     [

    character

     traits

     or

     values

     that

     make

     me

     unique

    ].

     I

     enjoy

     [

    part

     of

     the

     work

     process

    ,

     such

     as

     [

    getting

     feedback

    ,

     brainstorm

    ing

    ,

     etc

    .

    ]],

     and

     I

    'm

     [

    description

     of

     my leadership

     style

    ].

     I

    'm

     always

     looking

     for

     ways

     to

     improve

     and

     learn

     new

     things

    ,

     and

     I

    'm

     eager

     to

    
    
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

     landmarks

     such

     as

     Notre

    -D

    ame

     Cathedral

    ,

     Lou

    vre

     Museum

    ,

     and

     E

    iff

    el

     Tower

    .

     The

     city

     is

     also

     famous

     for

     its

     food

     and

     wine

    ,

     including

     its

     famous

     ch

    âte

    aux

     and

     wine

     cell

    ars

    .

     Paris

     is

     a

     cultural

     center

     with

     many

     museums

    ,

     theaters

    ,

     and

     parks

    ,

     and

     it is

     a

     major

     transportation

     hub

    ,

     with

     its

     main

     train

     station

     serving

     as

     a

     landmark

     in

     the

     city

    .

     The

     city

     is

     home

     to

     many

     French

     restaurants

    ,

     cafes

    ,

     and

     food

     trucks

    ,

     and

     it

     is

     a

     popular

     tourist

     destination

     for

     its

     museums

    ,

     art

     galleries

    ,

     and

     shopping

    .

     The

     French

     people

     love to

     celebrate

     the

     festivals

     and

     traditions

     of

     their

     country

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     uncertain

    ,

     but

     here

     are

     some

     possible

     trends

     we

     can

     expect

     to

     see

     in

     the

     coming

     years

    :
    


    1

    .

     Increased

     automation

    :

     The

     use

     of

     AI

     will

     continue

     to

     grow

    ,

     with

     more

     tasks

     being

     automated

     to

     increase

     efficiency

     and

     reduce

     errors

    .

     This

     will

     lead

     to

     a

     more

     automated

     workforce

     and

     a

     more

     efficient

     use

     of

     resources

    .
    


    2

    .

     AI

    -powered

     healthcare

    :

     AI

     will

     be

     used

     to

     improve

     the

     accuracy

     and

     speed

     of

     medical

     diagnoses

    ,

     improve

     treatment

     plans

    ,

     and

     personalize

     patient

     care

    .

     This will

     lead

     to

     better

     outcomes

     for

     patients

     and

     more

     efficient

     use

     of

     medical

     resources

    .
    


    3

    .

     AI

     in

     education

    :

     AI

     will

     be

     used

     to

     personalize

     learning

     experiences

     for

     students

    ,

     by

     tail

    oring

     education

     to

     individual

    



```python
llm.shutdown()
```
