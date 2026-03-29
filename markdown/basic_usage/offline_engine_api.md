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


    2026-03-29 04:58:48.010 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 04:58:48] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 04:58:48.010 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 04:58:48] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 04:58:48.010 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 04:58:48] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 04:58:48.010 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 04:58:48] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 04:58:48.010 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 04:58:48] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.30it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.29it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:03,  2.17s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:03,  2.17s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:03,  2.17s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:03,  2.17s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:23,  2.28it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:23,  2.28it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:23,  2.28it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:23,  2.28it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:23,  2.28it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:23,  2.28it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:23,  2.28it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:06,  6.89it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:06,  6.89it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:06,  6.89it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:06,  6.89it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:06,  6.89it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:06,  6.89it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:06,  6.89it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:06,  6.89it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:06,  6.89it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:06,  6.89it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 15.23it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 15.23it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 15.23it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 15.23it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 15.23it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 15.23it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 15.23it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 15.23it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 22.07it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 22.07it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 22.07it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 22.07it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 22.07it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 22.07it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 22.07it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 26.50it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 26.50it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 26.50it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:00, 26.50it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:00, 26.50it/s]

    Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:02<00:00, 26.50it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:02<00:00, 26.50it/s]Compiling num tokens (num_tokens=224):  55%|█████▌    | 32/58 [00:02<00:00, 26.50it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:02<00:00, 33.74it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:02<00:00, 33.74it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:02<00:00, 33.74it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:02<00:00, 33.74it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:02<00:00, 33.74it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:02<00:00, 33.74it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:02<00:00, 33.74it/s]Compiling num tokens (num_tokens=112):  67%|██████▋   | 39/58 [00:02<00:00, 33.74it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:02<00:00, 40.60it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:02<00:00, 40.60it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:02<00:00, 40.60it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:02<00:00, 40.60it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:02<00:00, 40.60it/s]

    Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:03<00:00, 40.60it/s]Compiling num tokens (num_tokens=28):  79%|███████▉  | 46/58 [00:03<00:00, 40.60it/s]Compiling num tokens (num_tokens=24):  79%|███████▉  | 46/58 [00:03<00:00, 40.60it/s]Compiling num tokens (num_tokens=20):  79%|███████▉  | 46/58 [00:03<00:00, 40.60it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:03<00:00, 49.03it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:03<00:00, 49.03it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:03<00:00, 49.03it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:03<00:00, 49.03it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:03<00:00, 49.03it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 18.84it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=60.50 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=60.47 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=60.47 GB):   3%|▎         | 2/58 [00:00<00:03, 14.43it/s]Capturing num tokens (num_tokens=7168 avail_mem=60.47 GB):   3%|▎         | 2/58 [00:00<00:03, 14.43it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=60.47 GB):   3%|▎         | 2/58 [00:00<00:03, 14.43it/s]Capturing num tokens (num_tokens=6656 avail_mem=60.47 GB):   7%|▋         | 4/58 [00:00<00:03, 15.09it/s]Capturing num tokens (num_tokens=6144 avail_mem=60.47 GB):   7%|▋         | 4/58 [00:00<00:03, 15.09it/s]Capturing num tokens (num_tokens=5632 avail_mem=60.46 GB):   7%|▋         | 4/58 [00:00<00:03, 15.09it/s]Capturing num tokens (num_tokens=5632 avail_mem=60.46 GB):  10%|█         | 6/58 [00:00<00:03, 16.18it/s]Capturing num tokens (num_tokens=5120 avail_mem=60.47 GB):  10%|█         | 6/58 [00:00<00:03, 16.18it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=60.46 GB):  10%|█         | 6/58 [00:00<00:03, 16.18it/s]Capturing num tokens (num_tokens=4608 avail_mem=60.46 GB):  14%|█▍        | 8/58 [00:00<00:02, 16.85it/s]Capturing num tokens (num_tokens=4096 avail_mem=60.46 GB):  14%|█▍        | 8/58 [00:00<00:02, 16.85it/s]Capturing num tokens (num_tokens=3840 avail_mem=60.46 GB):  14%|█▍        | 8/58 [00:00<00:02, 16.85it/s]Capturing num tokens (num_tokens=3584 avail_mem=60.45 GB):  14%|█▍        | 8/58 [00:00<00:02, 16.85it/s]Capturing num tokens (num_tokens=3584 avail_mem=60.45 GB):  19%|█▉        | 11/58 [00:00<00:02, 19.75it/s]Capturing num tokens (num_tokens=3328 avail_mem=60.45 GB):  19%|█▉        | 11/58 [00:00<00:02, 19.75it/s]Capturing num tokens (num_tokens=3072 avail_mem=60.45 GB):  19%|█▉        | 11/58 [00:00<00:02, 19.75it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=60.45 GB):  19%|█▉        | 11/58 [00:00<00:02, 19.75it/s]Capturing num tokens (num_tokens=2816 avail_mem=60.45 GB):  24%|██▍       | 14/58 [00:00<00:01, 22.27it/s]Capturing num tokens (num_tokens=2560 avail_mem=60.44 GB):  24%|██▍       | 14/58 [00:00<00:01, 22.27it/s]Capturing num tokens (num_tokens=2304 avail_mem=60.44 GB):  24%|██▍       | 14/58 [00:00<00:01, 22.27it/s]Capturing num tokens (num_tokens=2048 avail_mem=60.43 GB):  24%|██▍       | 14/58 [00:00<00:01, 22.27it/s]Capturing num tokens (num_tokens=1792 avail_mem=60.43 GB):  24%|██▍       | 14/58 [00:00<00:01, 22.27it/s]Capturing num tokens (num_tokens=1792 avail_mem=60.43 GB):  31%|███       | 18/58 [00:00<00:01, 25.52it/s]Capturing num tokens (num_tokens=1536 avail_mem=60.43 GB):  31%|███       | 18/58 [00:00<00:01, 25.52it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=60.42 GB):  31%|███       | 18/58 [00:00<00:01, 25.52it/s]Capturing num tokens (num_tokens=1024 avail_mem=60.40 GB):  31%|███       | 18/58 [00:00<00:01, 25.52it/s]Capturing num tokens (num_tokens=960 avail_mem=60.42 GB):  31%|███       | 18/58 [00:00<00:01, 25.52it/s] Capturing num tokens (num_tokens=960 avail_mem=60.42 GB):  38%|███▊      | 22/58 [00:00<00:01, 27.73it/s]Capturing num tokens (num_tokens=896 avail_mem=60.41 GB):  38%|███▊      | 22/58 [00:00<00:01, 27.73it/s]Capturing num tokens (num_tokens=832 avail_mem=60.41 GB):  38%|███▊      | 22/58 [00:00<00:01, 27.73it/s]Capturing num tokens (num_tokens=768 avail_mem=59.25 GB):  38%|███▊      | 22/58 [00:01<00:01, 27.73it/s]

    Capturing num tokens (num_tokens=768 avail_mem=59.25 GB):  43%|████▎     | 25/58 [00:01<00:01, 23.18it/s]Capturing num tokens (num_tokens=704 avail_mem=59.25 GB):  43%|████▎     | 25/58 [00:01<00:01, 23.18it/s]Capturing num tokens (num_tokens=640 avail_mem=59.25 GB):  43%|████▎     | 25/58 [00:01<00:01, 23.18it/s]Capturing num tokens (num_tokens=576 avail_mem=59.25 GB):  43%|████▎     | 25/58 [00:01<00:01, 23.18it/s]

    Capturing num tokens (num_tokens=576 avail_mem=59.25 GB):  48%|████▊     | 28/58 [00:01<00:01, 18.25it/s]Capturing num tokens (num_tokens=512 avail_mem=59.23 GB):  48%|████▊     | 28/58 [00:01<00:01, 18.25it/s]Capturing num tokens (num_tokens=480 avail_mem=59.25 GB):  48%|████▊     | 28/58 [00:01<00:01, 18.25it/s]Capturing num tokens (num_tokens=448 avail_mem=59.25 GB):  48%|████▊     | 28/58 [00:01<00:01, 18.25it/s]

    Capturing num tokens (num_tokens=448 avail_mem=59.25 GB):  53%|█████▎    | 31/58 [00:01<00:01, 16.39it/s]Capturing num tokens (num_tokens=416 avail_mem=59.25 GB):  53%|█████▎    | 31/58 [00:01<00:01, 16.39it/s]Capturing num tokens (num_tokens=384 avail_mem=59.24 GB):  53%|█████▎    | 31/58 [00:01<00:01, 16.39it/s]Capturing num tokens (num_tokens=384 avail_mem=59.24 GB):  57%|█████▋    | 33/58 [00:01<00:01, 15.20it/s]Capturing num tokens (num_tokens=352 avail_mem=59.24 GB):  57%|█████▋    | 33/58 [00:01<00:01, 15.20it/s]

    Capturing num tokens (num_tokens=320 avail_mem=59.23 GB):  57%|█████▋    | 33/58 [00:01<00:01, 15.20it/s]Capturing num tokens (num_tokens=320 avail_mem=59.23 GB):  60%|██████    | 35/58 [00:01<00:01, 14.30it/s]Capturing num tokens (num_tokens=288 avail_mem=59.23 GB):  60%|██████    | 35/58 [00:01<00:01, 14.30it/s]Capturing num tokens (num_tokens=256 avail_mem=59.23 GB):  60%|██████    | 35/58 [00:02<00:01, 14.30it/s]

    Capturing num tokens (num_tokens=256 avail_mem=59.23 GB):  64%|██████▍   | 37/58 [00:02<00:01, 13.99it/s]Capturing num tokens (num_tokens=240 avail_mem=59.23 GB):  64%|██████▍   | 37/58 [00:02<00:01, 13.99it/s]Capturing num tokens (num_tokens=224 avail_mem=59.22 GB):  64%|██████▍   | 37/58 [00:02<00:01, 13.99it/s]Capturing num tokens (num_tokens=224 avail_mem=59.22 GB):  67%|██████▋   | 39/58 [00:02<00:01, 13.65it/s]Capturing num tokens (num_tokens=208 avail_mem=59.22 GB):  67%|██████▋   | 39/58 [00:02<00:01, 13.65it/s]

    Capturing num tokens (num_tokens=192 avail_mem=59.22 GB):  67%|██████▋   | 39/58 [00:02<00:01, 13.65it/s]Capturing num tokens (num_tokens=192 avail_mem=59.22 GB):  71%|███████   | 41/58 [00:02<00:01, 13.56it/s]Capturing num tokens (num_tokens=176 avail_mem=59.21 GB):  71%|███████   | 41/58 [00:02<00:01, 13.56it/s]Capturing num tokens (num_tokens=160 avail_mem=59.21 GB):  71%|███████   | 41/58 [00:02<00:01, 13.56it/s]

    Capturing num tokens (num_tokens=160 avail_mem=59.21 GB):  74%|███████▍  | 43/58 [00:02<00:01, 13.49it/s]Capturing num tokens (num_tokens=144 avail_mem=59.21 GB):  74%|███████▍  | 43/58 [00:02<00:01, 13.49it/s]Capturing num tokens (num_tokens=128 avail_mem=59.20 GB):  74%|███████▍  | 43/58 [00:02<00:01, 13.49it/s]Capturing num tokens (num_tokens=128 avail_mem=59.20 GB):  78%|███████▊  | 45/58 [00:02<00:00, 13.27it/s]Capturing num tokens (num_tokens=112 avail_mem=59.20 GB):  78%|███████▊  | 45/58 [00:02<00:00, 13.27it/s]

    Capturing num tokens (num_tokens=96 avail_mem=59.20 GB):  78%|███████▊  | 45/58 [00:02<00:00, 13.27it/s] Capturing num tokens (num_tokens=96 avail_mem=59.20 GB):  81%|████████  | 47/58 [00:02<00:00, 13.27it/s]Capturing num tokens (num_tokens=80 avail_mem=59.19 GB):  81%|████████  | 47/58 [00:02<00:00, 13.27it/s]Capturing num tokens (num_tokens=64 avail_mem=59.19 GB):  81%|████████  | 47/58 [00:02<00:00, 13.27it/s]

    Capturing num tokens (num_tokens=64 avail_mem=59.19 GB):  84%|████████▍ | 49/58 [00:03<00:00, 12.98it/s]Capturing num tokens (num_tokens=48 avail_mem=59.19 GB):  84%|████████▍ | 49/58 [00:03<00:00, 12.98it/s]Capturing num tokens (num_tokens=32 avail_mem=59.19 GB):  84%|████████▍ | 49/58 [00:03<00:00, 12.98it/s]

    Capturing num tokens (num_tokens=32 avail_mem=59.19 GB):  88%|████████▊ | 51/58 [00:03<00:00, 10.94it/s]Capturing num tokens (num_tokens=28 avail_mem=59.18 GB):  88%|████████▊ | 51/58 [00:03<00:00, 10.94it/s]Capturing num tokens (num_tokens=24 avail_mem=59.18 GB):  88%|████████▊ | 51/58 [00:03<00:00, 10.94it/s]Capturing num tokens (num_tokens=24 avail_mem=59.18 GB):  91%|█████████▏| 53/58 [00:03<00:00, 11.43it/s]Capturing num tokens (num_tokens=20 avail_mem=59.17 GB):  91%|█████████▏| 53/58 [00:03<00:00, 11.43it/s]

    Capturing num tokens (num_tokens=16 avail_mem=59.17 GB):  91%|█████████▏| 53/58 [00:03<00:00, 11.43it/s]Capturing num tokens (num_tokens=16 avail_mem=59.17 GB):  95%|█████████▍| 55/58 [00:03<00:00, 12.57it/s]Capturing num tokens (num_tokens=12 avail_mem=59.17 GB):  95%|█████████▍| 55/58 [00:03<00:00, 12.57it/s]Capturing num tokens (num_tokens=8 avail_mem=59.16 GB):  95%|█████████▍| 55/58 [00:03<00:00, 12.57it/s] Capturing num tokens (num_tokens=8 avail_mem=59.16 GB):  98%|█████████▊| 57/58 [00:03<00:00, 13.69it/s]Capturing num tokens (num_tokens=4 avail_mem=59.16 GB):  98%|█████████▊| 57/58 [00:03<00:00, 13.69it/s]

    Capturing num tokens (num_tokens=4 avail_mem=59.16 GB): 100%|██████████| 58/58 [00:03<00:00, 15.52it/s]


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
    Generated text:  Michael. I'm a student in Senior 1. I'm from the UK. I have two best friends in my class, Karen and Linda. I'm really happy to have two friends. I think they are very nice. They do the same things every day, so I think they are very kind and helpful. My friend, Karen, is a good student. She always stays in class, does her homework and plays sports. Linda is a good helper. She helps me with my homework. She always helps me clean my room. The other day, I had a new teacher at school. He is very nice. He tells me
    ===============================
    Prompt: The president of the United States is
    Generated text:  from ______.
    A. Russia
    B. Germany
    C. France
    D. United States
    Answer:
    D
    
    The primary responsibilities of a securities company include ____.
    A. Providing financial advisory services
    B. Acting as an intermediary for securities transactions
    C. Implementing capital verification
    D. Storing securities
    Answer:
    B
    
    In the accounting treatment of intangible assets, which of the following statements is true?
    A. When an asset reaches its intended use state, its book value should be transferred to the current period's income statement.
    B. The amortization of intangible assets should be recorded as an additional expense in the
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It is the largest city of Europe and is the cultural and economic center of the country. It is known for its rich history, beautiful architecture and landmarks. Paris is also a popular tourist destination with millions of visitors each year. Paris has a long history dating back to the Middle Ages and is one of the oldest cities in the world. The city was founded in the 8th century and it was the capital of France until 1969. Paris has been the capital of France since 1871. It has a long-standing tradition of art, architecture and culture.
    
    Paris is also a world-renowned city
    ===============================
    Prompt: The future of AI is
    Generated text:  a rapidly evolving landscape, with an increasing number of new technologies emerging. One of these emerging technologies is blockchain technology, which is a distributed ledger technology that allows for secure and transparent transactions and data sharing. This technology has the potential to revolutionize various industries, from finance and healthcare to supply chain management and more.
    Blockchain technology uses a secure and immutable ledger to record transactions, allowing for secure and transparent transactions between parties. This means that data is not stored in one location, but rather across multiple nodes, ensuring that there is no single point of failure. This makes blockchain technology highly resistant to fraud and hacking.
    In addition to its security features,


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


    Generated text:  [Name] and I am a [occupation] who has been [number of years] in the industry. I am passionate about [reason for passion], and I am always looking for ways to [action or goal]. I am [age] years old, and I have [number of years] of experience in [industry]. I am [gender] and I am [gender] and I am [gender]. I am [gender] and I am [gender]. I am [gender] and I am [gender]. I am [gender] and I am [gender]. I am [gender] and I am [gender]. I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and Louvre Museum. It is also a major cultural and economic center, hosting numerous museums, theaters, and other attractions. Paris is a popular tourist destination, attracting millions of visitors each year. The city is known for its rich history, including the influence of the French Revolution and the influence of the French Revolution on modern French culture. Paris is also home to many notable French artists, writers, and musicians. The city is a hub for business and commerce, with many international companies and institutions located there. Overall, Paris is a vibrant and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased focus on ethical AI: As more people become aware of the potential risks of AI, there will be a greater emphasis on developing ethical AI that is designed to minimize harm and maximize benefits.
    
    2. Greater integration of AI with other technologies: AI is becoming increasingly integrated with other technologies, such as machine learning, natural language processing, and computer vision. This integration will likely lead to even more sophisticated AI systems that can perform a wider range of tasks.
    
    3. Increased use of AI in healthcare
    


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
    Generated text:  [Name] and I am a [Career/Title] at [Company/Company Name], [Career/Title] at [Company/Company Name]. As an [Area of Expertise], I bring a wealth of [Technical Skills, Relevant Experience, or Education] to [Your Field/Industry]. I am driven, highly organized, and detail-oriented, and I am always looking for new opportunities to contribute to the success of [Your Field/Industry]. I am passionate about [Personal Interest/Interest in Industry, Passion, or Motivational Goal], and I am committed to [Mission/Goal, Personal Mission, or Leadership].
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    The statement is accurate as of 2023, and it is the largest city in Europe by population. Paris is also the world's most populous city by both land and sea. Despite being the capital, the city is divided into 12 regions, with each region having its own government. It's also home to some of France's most well-known landmarks, including Notre-Dame Cathedral and the Eiffel Tower. Paris is known for its rich cultural scene, including the annual Eiffel Tower Grand Opening and the annual Les Invalides Festival. Additionally, it's important to note that Paris is divided into
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  complex and depends on a number of factors, including the research and development of new technologies, the adoption of AI in various industries, and the integration of AI into everyday life. Here are some possible future trends in AI:
    
    1. Increased efficiency and accuracy: As AI becomes more sophisticated and capable of performing complex tasks, its efficiency and accuracy will continue to improve. This will result in more accurate predictions and better decision-making, which will lead to better outcomes and increased productivity.
    
    2. Increased personalization: AI will continue to play a more significant role in personalized marketing and recommendations. Personalized content will be more relevant and useful to users, and


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

    career

    ]

     with

     [

    number

    ]

     years

     of

     experience

     in

     [

    industry

    ].

     I

    'm

     always

     seeking

     to

     learn

     and

     grow

    ,

     and

     I

    'm

     excited

     to

     explore

     new

     opportunities

     and

     challenges

     with

     you

    .

     What

    's

     your

     name

     and

     what

     do

     you

     do

    ?

     [

    Your

     name

    ]

     [

    Your

     current

     role

    ]

     [

    Your

     skills

    ,

     knowledge

    ,

     and

     experience

    ]

     [

    Your

     enthusiasm

     and

     passion

     for

     your

     work

    ]

     [

    Your

     career

     goals

     and

     aspirations

    ]


    Hello

    ,

     my

     name

     is

     [

    name

    ],

     and

     I

    'm

     a

     [

    career

    ]

     with

     [

    number

    ]

     years

     of

     experience

     in

     [

    industry

    ].

     I

    'm

     always

     seeking

     to

     learn

     and

     grow

    ,

     and

     I

    'm

     excited

     to

     explore

    
    
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

     and

     the

     seat

     of

     the

     government

    ,

     culture

    ,

     and

     economy

     of

     the

     country

    .

     It

     is

     the

     most

     populous

     city

     in

     the

     European

     Union

    .

     Paris

     is

     also

     one

     of

     the

     most

     visited

     cities

     in

     the

     world

    ,

     with

     millions

     of

     visitors

     each

     year

    .

     The

     city

     is

     known

     for

     its

     historical

     and

     architectural

     landmarks

    ,

     such

     as

     Notre

    -D

    ame

     Cathedral

     and

     Lou

    vre

     Museum

    ,

     as

     well

     as

     its

     contemporary

     art

     scene

     and

     fashion

     industry

    .

     Paris

     is

     also

     a

     center

     for

     education

    ,

     research

    ,

     and

     innovation

    ,

     with

     many

     famous

     universities

     and

     institutions

    .

     As

     of

     

    2

    0

    2

    1

    ,

     Paris

     had

     a

     population

     of

     approximately

     

    2

    .

    1

     million

     people

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     rapidly

     evolving

    ,

     and

     we

     can

     expect

     to

     see

     several

     key

     trends

     in

     the

     coming

     years

    :
    


    1

    .

     Increased

     autonomy

    :

     As

     AI

     technology

     becomes

     more

     advanced

    ,

     we

     are

     likely

     to

     see

     more

     autonomous

     AI

     systems

     that

     can

     perform

     tasks

     without

     human

     intervention

    ,

     such

     as

     driving

     a

     car

    ,

     managing

     a

     home

    ,

     or

     even

     playing

     sports

    .

     These

     systems

     will

     be

     able

     to

     learn

     from

     experience

     and

     make

     decisions

     based

     on

     feedback

    ,

     making

     them

     more

     efficient

     and

     effective

    .
    


    2

    .

     Increased

     integration

     with

     human

     AI

    :

     As

     AI

     technology

     becomes

     more

     integrated

     with

     human

     AI

     systems

    ,

     we

     can

     expect

     to

     see

     more

     sophisticated

     AI

     that

     can

     adapt

     to

     the

     complex

     needs

     of

     human

     beings

    .

     For

     example

    ,

     AI

     systems

     that

    



```python
llm.shutdown()
```
