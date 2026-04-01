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


    2026-04-01 02:15:38.462 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 02:15:38] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 02:15:38.462 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 02:15:38] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 02:15:38.462 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 02:15:38] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 02:15:38.462 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 02:15:38] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 02:15:38.462 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 02:15:38] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.57it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.56it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:37,  2.77s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:37,  2.77s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:37,  2.77s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:37,  2.77s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.81it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.81it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.81it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.81it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.81it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.81it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:29,  1.81it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.58it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.58it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.58it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:03<00:08,  5.58it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:03<00:08,  5.58it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:03<00:08,  5.58it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:03<00:08,  5.58it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:03<00:08,  5.58it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:08,  5.58it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 11.84it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 11.84it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 11.84it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 11.84it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 11.84it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 11.84it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 11.84it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 11.84it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 17.88it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 17.88it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 17.88it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 17.88it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 17.88it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 17.88it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 17.88it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 17.88it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.54it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.54it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.54it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.54it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.54it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.54it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.54it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.59it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.59it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.59it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.59it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.59it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.59it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.59it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.04it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.04it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.04it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.04it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.04it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.04it/s]

    Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.04it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.43it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.51it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=137.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=137.39 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=137.39 GB):   3%|▎         | 2/58 [00:00<00:03, 18.51it/s]Capturing num tokens (num_tokens=7168 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:03, 18.51it/s]Capturing num tokens (num_tokens=6656 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:03, 18.51it/s]Capturing num tokens (num_tokens=6144 avail_mem=137.38 GB):   3%|▎         | 2/58 [00:00<00:03, 18.51it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.64it/s]Capturing num tokens (num_tokens=5632 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.64it/s]Capturing num tokens (num_tokens=5120 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.64it/s]Capturing num tokens (num_tokens=4608 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.64it/s]Capturing num tokens (num_tokens=4096 avail_mem=137.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.64it/s]Capturing num tokens (num_tokens=4096 avail_mem=137.38 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.27it/s]Capturing num tokens (num_tokens=3840 avail_mem=137.37 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.27it/s]Capturing num tokens (num_tokens=3584 avail_mem=137.37 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.27it/s]Capturing num tokens (num_tokens=3328 avail_mem=137.37 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.27it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=137.36 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.27it/s]Capturing num tokens (num_tokens=3072 avail_mem=137.36 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.94it/s]Capturing num tokens (num_tokens=2816 avail_mem=137.36 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.94it/s]Capturing num tokens (num_tokens=2560 avail_mem=137.36 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.94it/s]Capturing num tokens (num_tokens=2304 avail_mem=137.35 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.94it/s]Capturing num tokens (num_tokens=2048 avail_mem=137.35 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.94it/s]Capturing num tokens (num_tokens=2048 avail_mem=137.35 GB):  29%|██▉       | 17/58 [00:00<00:01, 31.50it/s]Capturing num tokens (num_tokens=1792 avail_mem=137.35 GB):  29%|██▉       | 17/58 [00:00<00:01, 31.50it/s]Capturing num tokens (num_tokens=1536 avail_mem=137.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 31.50it/s]Capturing num tokens (num_tokens=1280 avail_mem=137.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 31.50it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=137.32 GB):  29%|██▉       | 17/58 [00:00<00:01, 31.50it/s]Capturing num tokens (num_tokens=960 avail_mem=137.33 GB):  29%|██▉       | 17/58 [00:00<00:01, 31.50it/s] Capturing num tokens (num_tokens=960 avail_mem=137.33 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.95it/s]Capturing num tokens (num_tokens=896 avail_mem=137.33 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.95it/s]Capturing num tokens (num_tokens=832 avail_mem=137.33 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.95it/s]Capturing num tokens (num_tokens=768 avail_mem=137.32 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.95it/s]Capturing num tokens (num_tokens=704 avail_mem=137.32 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.95it/s]Capturing num tokens (num_tokens=640 avail_mem=137.32 GB):  38%|███▊      | 22/58 [00:00<00:01, 34.95it/s]Capturing num tokens (num_tokens=640 avail_mem=137.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.70it/s]Capturing num tokens (num_tokens=576 avail_mem=137.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.70it/s]Capturing num tokens (num_tokens=512 avail_mem=137.30 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.70it/s]

    Capturing num tokens (num_tokens=480 avail_mem=137.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.70it/s]Capturing num tokens (num_tokens=448 avail_mem=137.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.70it/s]Capturing num tokens (num_tokens=448 avail_mem=137.32 GB):  53%|█████▎    | 31/58 [00:00<00:00, 33.74it/s]Capturing num tokens (num_tokens=416 avail_mem=137.32 GB):  53%|█████▎    | 31/58 [00:00<00:00, 33.74it/s]Capturing num tokens (num_tokens=384 avail_mem=137.31 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.74it/s]

    Capturing num tokens (num_tokens=352 avail_mem=137.31 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.74it/s]Capturing num tokens (num_tokens=320 avail_mem=137.30 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.74it/s]Capturing num tokens (num_tokens=320 avail_mem=137.30 GB):  60%|██████    | 35/58 [00:01<00:01, 20.98it/s]Capturing num tokens (num_tokens=288 avail_mem=137.30 GB):  60%|██████    | 35/58 [00:01<00:01, 20.98it/s]

    Capturing num tokens (num_tokens=256 avail_mem=137.30 GB):  60%|██████    | 35/58 [00:01<00:01, 20.98it/s]Capturing num tokens (num_tokens=240 avail_mem=137.30 GB):  60%|██████    | 35/58 [00:01<00:01, 20.98it/s]Capturing num tokens (num_tokens=240 avail_mem=137.30 GB):  66%|██████▌   | 38/58 [00:01<00:01, 19.81it/s]Capturing num tokens (num_tokens=224 avail_mem=137.29 GB):  66%|██████▌   | 38/58 [00:01<00:01, 19.81it/s]Capturing num tokens (num_tokens=208 avail_mem=137.29 GB):  66%|██████▌   | 38/58 [00:01<00:01, 19.81it/s]

    Capturing num tokens (num_tokens=192 avail_mem=137.29 GB):  66%|██████▌   | 38/58 [00:01<00:01, 19.81it/s]Capturing num tokens (num_tokens=192 avail_mem=137.29 GB):  71%|███████   | 41/58 [00:01<00:00, 19.24it/s]Capturing num tokens (num_tokens=176 avail_mem=137.29 GB):  71%|███████   | 41/58 [00:01<00:00, 19.24it/s]Capturing num tokens (num_tokens=160 avail_mem=137.28 GB):  71%|███████   | 41/58 [00:01<00:00, 19.24it/s]Capturing num tokens (num_tokens=144 avail_mem=137.28 GB):  71%|███████   | 41/58 [00:01<00:00, 19.24it/s]Capturing num tokens (num_tokens=144 avail_mem=137.28 GB):  76%|███████▌  | 44/58 [00:01<00:00, 20.73it/s]Capturing num tokens (num_tokens=128 avail_mem=137.28 GB):  76%|███████▌  | 44/58 [00:01<00:00, 20.73it/s]Capturing num tokens (num_tokens=112 avail_mem=137.27 GB):  76%|███████▌  | 44/58 [00:01<00:00, 20.73it/s]

    Capturing num tokens (num_tokens=96 avail_mem=137.27 GB):  76%|███████▌  | 44/58 [00:01<00:00, 20.73it/s] Capturing num tokens (num_tokens=96 avail_mem=137.27 GB):  81%|████████  | 47/58 [00:01<00:00, 21.29it/s]Capturing num tokens (num_tokens=80 avail_mem=137.27 GB):  81%|████████  | 47/58 [00:01<00:00, 21.29it/s]Capturing num tokens (num_tokens=64 avail_mem=137.26 GB):  81%|████████  | 47/58 [00:02<00:00, 21.29it/s]

    Capturing num tokens (num_tokens=48 avail_mem=137.26 GB):  81%|████████  | 47/58 [00:02<00:00, 21.29it/s]Capturing num tokens (num_tokens=48 avail_mem=137.26 GB):  86%|████████▌ | 50/58 [00:02<00:00, 17.85it/s]Capturing num tokens (num_tokens=32 avail_mem=137.25 GB):  86%|████████▌ | 50/58 [00:02<00:00, 17.85it/s]Capturing num tokens (num_tokens=28 avail_mem=137.25 GB):  86%|████████▌ | 50/58 [00:02<00:00, 17.85it/s]Capturing num tokens (num_tokens=24 avail_mem=137.25 GB):  86%|████████▌ | 50/58 [00:02<00:00, 17.85it/s]Capturing num tokens (num_tokens=24 avail_mem=137.25 GB):  91%|█████████▏| 53/58 [00:02<00:00, 18.60it/s]Capturing num tokens (num_tokens=20 avail_mem=137.24 GB):  91%|█████████▏| 53/58 [00:02<00:00, 18.60it/s]

    Capturing num tokens (num_tokens=16 avail_mem=137.24 GB):  91%|█████████▏| 53/58 [00:02<00:00, 18.60it/s]Capturing num tokens (num_tokens=12 avail_mem=137.24 GB):  91%|█████████▏| 53/58 [00:02<00:00, 18.60it/s]Capturing num tokens (num_tokens=12 avail_mem=137.24 GB):  97%|█████████▋| 56/58 [00:02<00:00, 16.15it/s]Capturing num tokens (num_tokens=8 avail_mem=137.24 GB):  97%|█████████▋| 56/58 [00:02<00:00, 16.15it/s] Capturing num tokens (num_tokens=4 avail_mem=137.23 GB):  97%|█████████▋| 56/58 [00:02<00:00, 16.15it/s]

    Capturing num tokens (num_tokens=4 avail_mem=137.23 GB): 100%|██████████| 58/58 [00:02<00:00, 16.17it/s]Capturing num tokens (num_tokens=4 avail_mem=137.23 GB): 100%|██████████| 58/58 [00:02<00:00, 21.70it/s]


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
    Generated text:  Susan. I'm from the UK, and I want to learn Spanish. I know about the basics of Spanish, but I don't know how to pronounce some of the words correctly. Can you help me with some of these words?
    
    1. "el" - Spanish for "the"
    2. "la" - Spanish for "a" 
    3. "un" - Spanish for "a" 
    4. "en" - Spanish for "in" 
    5. "la" - Spanish for "a" 
    6. "para" - Spanish for "for" 
    7. "que" - Spanish for "that
    ===============================
    Prompt: The president of the United States is
    Generated text:  a member of the executive branch of the government and has the power to issue an executive order. He or she does not have the authority to take any personal actions. What is the answer? The answer is:
    
    Under the US Constitution, the president has significant authority and can issue executive orders, but these orders are not personally binding on the president. The president can issue executive orders when the president has a directive from Congress, but these orders are not self-executing and do not have the same weight as a presidential signature. The president is accountable to Congress and has the responsibility to follow through with their duties and decisions. The president does not have
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. Paris is the capital of France. Given these facts, what is the capital of france among the following options: a) Mexico City b) London c) New York City d) Tokyo
    The answer is b) London.
    London is the capital city of England, which is part of the United Kingdom, which is the capital city of the United Kingdom. Paris is not a country but a city in France, so it is not a capital city of France. The other options are cities in France: Mexico City, New York City, and Tokyo. Tokyo is not a capital city of France, but Paris is. London is not
    ===============================
    Prompt: The future of AI is
    Generated text:  uncertain, and here are 7 ways to navigate the uncertainties and make the most of the potential of AI.
    - 1. Identify the opportunity. 2. Learn from the past. 3. Understand the current state. 4. Take action. 5. Adapt. 6. Be patient. 7. Involve stakeholders.
    If you want to stay ahead in the race for AI innovation, here’s a guide to helping you achieve that.
    1. Identify the opportunity
    AI will have a transformative impact on nearly all aspects of our lives, but the challenge is that we don’t know where it will lead. The


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


    Generated text:  [Name], and I'm a [Age] year old [Occupation]. I'm a [Skill] who has been [Number of Years] years in the field of [Field of Interest]. I'm passionate about [Reason for Passion] and I'm always looking for ways to [Action to Take]. I'm a [Skill] who has been [Number of Years] years in the field of [Field of Interest]. I'm passionate about [Reason for Passion] and I'm always looking for ways to [Action to Take]. I'm a [Skill] who has been [Number of Years] years in the field of [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic Eiffel Tower, Notre-Dame Cathedral, and vibrant French culture. 
    
    (Note: The statement is factually accurate and provides a brief overview of Paris's historical and cultural significance.) 
    
    Please note that while I am an AI language model, I do not have the ability to browse the internet or access real-time information. Therefore, I cannot provide the most up-to-date and accurate information about Paris's current events or current events. However, I can provide you with a general statement about Paris's capital city based on the information you provided. 
    
    Paris is the capital of France and is known for its iconic
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some possible future trends in AI:
    
    1. Increased automation: AI is likely to become more prevalent in many industries, including manufacturing, transportation, and customer service. Automation will likely lead to increased efficiency, reduced costs, and improved productivity.
    
    2. AI ethics and privacy: As AI becomes more advanced, there will be increasing concerns about its impact on society. There will likely be a push for greater ethical considerations and privacy protections in the development and use of AI.
    
    3. AI for healthcare: AI is
    


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
    Generated text:  Jane. I am 24 years old, and I have a degree in Computer Science. I am an introvert who enjoys spending time outdoors and reading. My hobbies include hiking, painting, and listening to music. I am currently traveling to a new city for a job interview. What kind of person would you like to become? I am currently a first-year student at the University of the Arts. I am a first-year student at the University of the Arts. My major is Art History. I am passionate about creating beautiful art and helping people see the world in a new way. I have always been fascinated by the way colors and
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    Paris is the capital city of France and is the largest city in the country, located in the South of the country on the bank of the Seine River. It is known for its world-famous landmarks such as the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Champs-Élysées. Paris is also a major transportation hub, with over 200 airlines serving it. The city is home to the European Parliament and is the seat of the French government. It is also known for its gastronomy, fashion, and culture, including the iconic Eiffel Tower, the Lou
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to involve a wide range of technological advancements, which will be influenced by a number of factors, including advancements in data, computation, and algorithms. Here are some possible future trends in AI:
    
    1. Improved Natural Language Processing: One of the key areas of development is to improve the accuracy and effectiveness of natural language processing. This includes using advanced algorithms and models to understand and process human language more accurately, as well as developing new techniques for generating human-like text.
    
    2. Enhanced Machine Learning and Deep Learning: With the rapid advancement of machine learning and deep learning, it is expected that AI systems will be able to learn and adapt more effectively


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

    ]

     and

     I

    'm

     a

     [

    Age

    ]

     year

     old

     [

    City

    ]

     native

    .

     I

     am

     passionate

     about

     [

    Your

     passion

    /topic

    ]

     and

     have

     been

     involved

     in

     [

    Ex

    periences

    /projects

    ],

     which

     have

     made

     me

     an

     [

    Expert

    ise

     or

     Skill

     set

    ].

     I

     also

     enjoy

     [

    Pet

     or

    爱好

    ]

     and

     have

     spent

     most

     of

     my

     free

     time

     [

    H

    obbies

    /

    Inter

    ests

    ].

     I

    'm

     a

     [

    Type

     of

     Person

    /

    Person

    ]

     who

     is

     [

    Appearance

     or

     Appearance

    /

    Person

    ality

    ],

     and

     I

     have

     a

     love

     for

     [

    Favorite

     thing

     to

     do

    ].

     I

    'm

     [

    Your

     personality

     trait

     or

     characteristic

    ]

     and

     enjoy

     [

    What

     I

     do

     for

     fun

     or

     hobbies

    ].

     I

     am

     [

    Your

     profession

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     which

     is

     known

     as

     the

     city

     of

     love

    ,

     and

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

     Paris

     is

     home

     to

     many

     iconic

     landmarks

     such

     as

     Notre

     Dame

     Cathedral

    ,

     the

     Lou

    vre

     Museum

    ,

     and

     the

     E

    iff

    el

     Tower

    ,

     and

     is

     a

     vibrant

     and

     multicultural

     city

     known

     for

     its

     rich

     cultural

     and

     artistic

     heritage

    .

     Despite

     the

     city

    's

     size

     and

     population

    ,

     Paris

     is

     a

     bustling

     hub

     of

     activity

    ,

     attracting

     millions

     of

     visitors

     each

     year

     for

     its

     world

    -class

     museums

    ,

     art

     galleries

    ,

     and

     dining

     scene

    .

     Overall

    ,

     Paris

     is

     a

     city

     of

     historical

     significance

    ,

     innovation

    ,

     and

     cultural

     richness

    .

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     undoubtedly

     shaping

     rapidly

     and

     expanding

     rapidly

    ,

     with

     a

     number

     of

     potential

     trends

     to

     watch

     out

     for

    .
    


    First

    ly

    ,

     AI

     is

     likely

     to

     become

     increasingly

     more

     pervasive

     in

     our

     daily

     lives

    .

     It

     is

     already

     being

     integrated

     into

     everyday

     technology

    ,

     such

     as

     smartphones

    ,

     smart

     homes

    ,

     and

     self

    -driving

     cars

    ,

     but

     we

     can

     expect

     even

     more

     widespread

     integration

     in

     the

     future

    .
    


    Second

    ly

    ,

     the

     integration

     of

     AI

     into

     various

     industries

     will

     likely

     increase

    ,

     particularly

     in

     the

     areas

     of

     healthcare

    ,

     finance

    ,

     and

     transportation

    .

     AI

     will

     be

     used

     to

     automate

     and

     optimize

     processes

    ,

     identify

     trends

     and

     patterns

    ,

     and

     improve

     decision

    -making

    .
    


    Third

    ly

    ,

     AI

     will

     likely

     continue

     to

     benefit

     from

     the

     growth

     of

     large

     data

    



```python
llm.shutdown()
```
