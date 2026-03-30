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


    2026-03-30 19:37:23.285 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 19:37:23] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 19:37:23.285 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 19:37:23] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 19:37:23.285 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 19:37:23] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 19:37:23.285 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 19:37:23] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 19:37:23.286 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 19:37:23] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.33it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.32it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:03,  2.17s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:03,  2.17s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<00:53,  1.04it/s]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<00:53,  1.04it/s]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<00:53,  1.04it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:21,  2.52it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:21,  2.52it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:21,  2.52it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:02<00:12,  4.28it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:02<00:12,  4.28it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:02<00:12,  4.28it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:02<00:12,  4.28it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:02<00:06,  7.28it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:02<00:06,  7.28it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:02<00:06,  7.28it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:02<00:06,  7.28it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:02<00:04, 10.59it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:02<00:04, 10.59it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:02<00:04, 10.59it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:02<00:04, 10.59it/s]Compiling num tokens (num_tokens=2304):  21%|██        | 12/58 [00:02<00:04, 10.59it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:02<00:02, 15.19it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:02<00:02, 15.19it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:02<00:02, 15.19it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:02<00:02, 15.19it/s]Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:02<00:02, 15.19it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:02<00:02, 18.87it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:02<00:02, 18.87it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:03<00:02, 18.87it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:03<00:02, 18.87it/s]

    Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:03<00:02, 18.87it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:03<00:01, 23.20it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:03<00:01, 23.20it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:03<00:01, 23.20it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:03<00:01, 23.20it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:03<00:01, 23.20it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:03<00:01, 26.41it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:03<00:01, 26.41it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:03<00:01, 26.41it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:01, 26.41it/s]

    Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:03<00:01, 26.41it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:00, 29.28it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:03<00:00, 31.63it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:03<00:00, 31.63it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:03<00:00, 31.63it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:03<00:00, 31.63it/s]

    Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:03<00:00, 31.63it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:03<00:00, 32.59it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:03<00:00, 32.59it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:03<00:00, 32.59it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:03<00:00, 32.59it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:03<00:00, 32.59it/s]Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:03<00:00, 32.59it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 34.59it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 34.59it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 34.59it/s] 

    Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 34.59it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 34.59it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 35.32it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 35.32it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 35.32it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 35.32it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 35.32it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 35.32it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:03<00:00, 38.12it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:03<00:00, 38.12it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:03<00:00, 38.12it/s]

    Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:03<00:00, 38.12it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:03<00:00, 38.12it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 14.71it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=52.70 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=52.70 GB):   2%|▏         | 1/58 [00:00<00:08,  6.51it/s]Capturing num tokens (num_tokens=7680 avail_mem=52.67 GB):   2%|▏         | 1/58 [00:00<00:08,  6.51it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=52.67 GB):   3%|▎         | 2/58 [00:00<00:08,  6.40it/s]Capturing num tokens (num_tokens=7168 avail_mem=52.67 GB):   3%|▎         | 2/58 [00:00<00:08,  6.40it/s]Capturing num tokens (num_tokens=7168 avail_mem=52.67 GB):   5%|▌         | 3/58 [00:00<00:07,  6.96it/s]Capturing num tokens (num_tokens=6656 avail_mem=52.67 GB):   5%|▌         | 3/58 [00:00<00:07,  6.96it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=52.67 GB):   7%|▋         | 4/58 [00:00<00:07,  7.34it/s]Capturing num tokens (num_tokens=6144 avail_mem=52.67 GB):   7%|▋         | 4/58 [00:00<00:07,  7.34it/s]Capturing num tokens (num_tokens=6144 avail_mem=52.67 GB):   9%|▊         | 5/58 [00:00<00:06,  7.65it/s]Capturing num tokens (num_tokens=5632 avail_mem=52.66 GB):   9%|▊         | 5/58 [00:00<00:06,  7.65it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=52.66 GB):  10%|█         | 6/58 [00:00<00:06,  8.03it/s]Capturing num tokens (num_tokens=5120 avail_mem=52.66 GB):  10%|█         | 6/58 [00:00<00:06,  8.03it/s]Capturing num tokens (num_tokens=5120 avail_mem=52.66 GB):  12%|█▏        | 7/58 [00:00<00:06,  8.34it/s]Capturing num tokens (num_tokens=4608 avail_mem=52.66 GB):  12%|█▏        | 7/58 [00:00<00:06,  8.34it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=52.66 GB):  14%|█▍        | 8/58 [00:01<00:05,  8.77it/s]Capturing num tokens (num_tokens=4096 avail_mem=52.66 GB):  14%|█▍        | 8/58 [00:01<00:05,  8.77it/s]Capturing num tokens (num_tokens=4096 avail_mem=52.66 GB):  16%|█▌        | 9/58 [00:01<00:05,  9.03it/s]Capturing num tokens (num_tokens=3840 avail_mem=52.66 GB):  16%|█▌        | 9/58 [00:01<00:05,  9.03it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=52.65 GB):  16%|█▌        | 9/58 [00:01<00:05,  9.03it/s]Capturing num tokens (num_tokens=3584 avail_mem=52.65 GB):  19%|█▉        | 11/58 [00:01<00:04,  9.61it/s]Capturing num tokens (num_tokens=3328 avail_mem=52.65 GB):  19%|█▉        | 11/58 [00:01<00:04,  9.61it/s]Capturing num tokens (num_tokens=3072 avail_mem=52.64 GB):  19%|█▉        | 11/58 [00:01<00:04,  9.61it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=52.64 GB):  22%|██▏       | 13/58 [00:01<00:04, 10.04it/s]Capturing num tokens (num_tokens=2816 avail_mem=52.64 GB):  22%|██▏       | 13/58 [00:01<00:04, 10.04it/s]Capturing num tokens (num_tokens=2560 avail_mem=52.64 GB):  22%|██▏       | 13/58 [00:01<00:04, 10.04it/s]Capturing num tokens (num_tokens=2560 avail_mem=52.64 GB):  26%|██▌       | 15/58 [00:01<00:04, 10.36it/s]Capturing num tokens (num_tokens=2304 avail_mem=52.63 GB):  26%|██▌       | 15/58 [00:01<00:04, 10.36it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=52.63 GB):  26%|██▌       | 15/58 [00:01<00:04, 10.36it/s]Capturing num tokens (num_tokens=2048 avail_mem=52.63 GB):  29%|██▉       | 17/58 [00:01<00:03, 10.70it/s]Capturing num tokens (num_tokens=1792 avail_mem=52.63 GB):  29%|██▉       | 17/58 [00:01<00:03, 10.70it/s]Capturing num tokens (num_tokens=1536 avail_mem=52.62 GB):  29%|██▉       | 17/58 [00:01<00:03, 10.70it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=52.62 GB):  33%|███▎      | 19/58 [00:02<00:03, 10.96it/s]Capturing num tokens (num_tokens=1280 avail_mem=52.62 GB):  33%|███▎      | 19/58 [00:02<00:03, 10.96it/s]Capturing num tokens (num_tokens=1024 avail_mem=52.60 GB):  33%|███▎      | 19/58 [00:02<00:03, 10.96it/s]Capturing num tokens (num_tokens=1024 avail_mem=52.60 GB):  36%|███▌      | 21/58 [00:02<00:03, 11.37it/s]Capturing num tokens (num_tokens=960 avail_mem=52.61 GB):  36%|███▌      | 21/58 [00:02<00:03, 11.37it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=52.61 GB):  36%|███▌      | 21/58 [00:02<00:03, 11.37it/s]Capturing num tokens (num_tokens=896 avail_mem=52.61 GB):  40%|███▉      | 23/58 [00:02<00:02, 11.71it/s]Capturing num tokens (num_tokens=832 avail_mem=52.61 GB):  40%|███▉      | 23/58 [00:02<00:02, 11.71it/s]Capturing num tokens (num_tokens=768 avail_mem=52.60 GB):  40%|███▉      | 23/58 [00:02<00:02, 11.71it/s]

    Capturing num tokens (num_tokens=768 avail_mem=52.60 GB):  43%|████▎     | 25/58 [00:02<00:02, 12.00it/s]Capturing num tokens (num_tokens=704 avail_mem=52.60 GB):  43%|████▎     | 25/58 [00:02<00:02, 12.00it/s]Capturing num tokens (num_tokens=640 avail_mem=52.60 GB):  43%|████▎     | 25/58 [00:02<00:02, 12.00it/s]Capturing num tokens (num_tokens=640 avail_mem=52.60 GB):  47%|████▋     | 27/58 [00:02<00:02, 12.13it/s]Capturing num tokens (num_tokens=576 avail_mem=52.60 GB):  47%|████▋     | 27/58 [00:02<00:02, 12.13it/s]

    Capturing num tokens (num_tokens=512 avail_mem=52.58 GB):  47%|████▋     | 27/58 [00:02<00:02, 12.13it/s]Capturing num tokens (num_tokens=512 avail_mem=52.58 GB):  50%|█████     | 29/58 [00:02<00:02, 12.23it/s]Capturing num tokens (num_tokens=480 avail_mem=52.60 GB):  50%|█████     | 29/58 [00:02<00:02, 12.23it/s]Capturing num tokens (num_tokens=448 avail_mem=52.60 GB):  50%|█████     | 29/58 [00:02<00:02, 12.23it/s]

    Capturing num tokens (num_tokens=448 avail_mem=52.60 GB):  53%|█████▎    | 31/58 [00:02<00:02, 12.34it/s]Capturing num tokens (num_tokens=416 avail_mem=52.60 GB):  53%|█████▎    | 31/58 [00:02<00:02, 12.34it/s]Capturing num tokens (num_tokens=384 avail_mem=52.59 GB):  53%|█████▎    | 31/58 [00:03<00:02, 12.34it/s]Capturing num tokens (num_tokens=384 avail_mem=52.59 GB):  57%|█████▋    | 33/58 [00:03<00:02, 12.41it/s]Capturing num tokens (num_tokens=352 avail_mem=52.59 GB):  57%|█████▋    | 33/58 [00:03<00:02, 12.41it/s]

    Capturing num tokens (num_tokens=320 avail_mem=52.58 GB):  57%|█████▋    | 33/58 [00:03<00:02, 12.41it/s]Capturing num tokens (num_tokens=320 avail_mem=52.58 GB):  60%|██████    | 35/58 [00:03<00:01, 12.54it/s]Capturing num tokens (num_tokens=288 avail_mem=52.58 GB):  60%|██████    | 35/58 [00:03<00:01, 12.54it/s]Capturing num tokens (num_tokens=256 avail_mem=52.58 GB):  60%|██████    | 35/58 [00:03<00:01, 12.54it/s]

    Capturing num tokens (num_tokens=256 avail_mem=52.58 GB):  64%|██████▍   | 37/58 [00:03<00:01, 12.58it/s]Capturing num tokens (num_tokens=240 avail_mem=52.58 GB):  64%|██████▍   | 37/58 [00:03<00:01, 12.58it/s]Capturing num tokens (num_tokens=224 avail_mem=52.57 GB):  64%|██████▍   | 37/58 [00:03<00:01, 12.58it/s]Capturing num tokens (num_tokens=224 avail_mem=52.57 GB):  67%|██████▋   | 39/58 [00:03<00:01, 12.50it/s]Capturing num tokens (num_tokens=208 avail_mem=52.57 GB):  67%|██████▋   | 39/58 [00:03<00:01, 12.50it/s]

    Capturing num tokens (num_tokens=192 avail_mem=52.57 GB):  67%|██████▋   | 39/58 [00:03<00:01, 12.50it/s]Capturing num tokens (num_tokens=192 avail_mem=52.57 GB):  71%|███████   | 41/58 [00:03<00:01, 12.53it/s]Capturing num tokens (num_tokens=176 avail_mem=52.57 GB):  71%|███████   | 41/58 [00:03<00:01, 12.53it/s]Capturing num tokens (num_tokens=160 avail_mem=52.56 GB):  71%|███████   | 41/58 [00:03<00:01, 12.53it/s]

    Capturing num tokens (num_tokens=160 avail_mem=52.56 GB):  74%|███████▍  | 43/58 [00:03<00:01, 12.62it/s]Capturing num tokens (num_tokens=144 avail_mem=52.56 GB):  74%|███████▍  | 43/58 [00:03<00:01, 12.62it/s]Capturing num tokens (num_tokens=128 avail_mem=52.56 GB):  74%|███████▍  | 43/58 [00:04<00:01, 12.62it/s]Capturing num tokens (num_tokens=128 avail_mem=52.56 GB):  78%|███████▊  | 45/58 [00:04<00:01, 12.69it/s]Capturing num tokens (num_tokens=112 avail_mem=52.55 GB):  78%|███████▊  | 45/58 [00:04<00:01, 12.69it/s]

    Capturing num tokens (num_tokens=96 avail_mem=52.55 GB):  78%|███████▊  | 45/58 [00:04<00:01, 12.69it/s] Capturing num tokens (num_tokens=96 avail_mem=52.55 GB):  81%|████████  | 47/58 [00:04<00:00, 12.65it/s]Capturing num tokens (num_tokens=80 avail_mem=52.55 GB):  81%|████████  | 47/58 [00:04<00:00, 12.65it/s]Capturing num tokens (num_tokens=64 avail_mem=52.54 GB):  81%|████████  | 47/58 [00:04<00:00, 12.65it/s]

    Capturing num tokens (num_tokens=64 avail_mem=52.54 GB):  84%|████████▍ | 49/58 [00:04<00:00, 12.72it/s]Capturing num tokens (num_tokens=48 avail_mem=52.54 GB):  84%|████████▍ | 49/58 [00:04<00:00, 12.72it/s]Capturing num tokens (num_tokens=32 avail_mem=52.54 GB):  84%|████████▍ | 49/58 [00:04<00:00, 12.72it/s]Capturing num tokens (num_tokens=32 avail_mem=52.54 GB):  88%|████████▊ | 51/58 [00:04<00:00, 12.69it/s]Capturing num tokens (num_tokens=28 avail_mem=52.53 GB):  88%|████████▊ | 51/58 [00:04<00:00, 12.69it/s]

    Capturing num tokens (num_tokens=24 avail_mem=52.53 GB):  88%|████████▊ | 51/58 [00:04<00:00, 12.69it/s]Capturing num tokens (num_tokens=24 avail_mem=52.53 GB):  91%|█████████▏| 53/58 [00:04<00:00, 12.73it/s]Capturing num tokens (num_tokens=20 avail_mem=52.52 GB):  91%|█████████▏| 53/58 [00:04<00:00, 12.73it/s]Capturing num tokens (num_tokens=16 avail_mem=52.52 GB):  91%|█████████▏| 53/58 [00:04<00:00, 12.73it/s]

    Capturing num tokens (num_tokens=16 avail_mem=52.52 GB):  95%|█████████▍| 55/58 [00:04<00:00, 12.73it/s]Capturing num tokens (num_tokens=12 avail_mem=52.52 GB):  95%|█████████▍| 55/58 [00:04<00:00, 12.73it/s]Capturing num tokens (num_tokens=8 avail_mem=52.52 GB):  95%|█████████▍| 55/58 [00:04<00:00, 12.73it/s] Capturing num tokens (num_tokens=8 avail_mem=52.52 GB):  98%|█████████▊| 57/58 [00:05<00:00, 12.75it/s]Capturing num tokens (num_tokens=4 avail_mem=52.51 GB):  98%|█████████▊| 57/58 [00:05<00:00, 12.75it/s]

    Capturing num tokens (num_tokens=4 avail_mem=52.51 GB): 100%|██████████| 58/58 [00:05<00:00, 11.35it/s]


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
    Generated text:  Jade. I'm a high school student at the University of Pennsylvania. I've been in the United States since 2012. When I was in my junior year, I was offered a scholarship to go to college in Canada and I was really happy about it. It was the first time I've ever had a scholarship. In June, I had a meeting with the Canadian government to apply for the scholarship. As I started to prepare, I realized that I was very poor in mathematics. My mother suggested that I take a math course because I had been studying English, so I decided to take one. I have been using my
    ===============================
    Prompt: The president of the United States is
    Generated text:  a public office, and there is no reason to suspect that any one person will be able to create a governing totalitarian system. But the existence of such a system in our country is a fact. This is evident from the fact that the American people are aware of this, and they are determined to resist it. For example, the mass movement of the American people against the draft, as well as the protests that were held in the city of New York against the arrest of the three Communists, not only reflect the people's sense of indignation against the rule of the dictatorship of the American ruling class, but also show a united and
    ===============================
    Prompt: The capital of France is
    Generated text:  ____.
    A. Paris
    B. Nice
    C. Lyon
    D. Brussels
    Answer:
    A
    
    On the line segment AB with endpoints A(5, 4) and B(1, 8), point M is the midpoint of the segment. What is the value of M?
    A. 3
    B. 6
    C. 12
    D. 18
    Answer:
    B
    
    The goal of this chapter is to help students understand the fundamental principles of important concepts, provide them with a comprehensive understanding of the content, and cultivate their ability to apply and use them. In order to achieve this
    ===============================
    Prompt: The future of AI is
    Generated text:  digital transformation. Here are some of the key reasons why businesses should embrace a digital-first approach to AI.
    
    1. Increased Efficiency and Accuracy: By automating tasks and processes using AI, businesses can increase efficiency and accuracy. This is particularly important for tasks that require repetitive or time-consuming work, such as data entry or customer support.
    
    2. Improved Customer Experience: AI can be used to personalize the customer experience, by analyzing customer data and preferences to offer tailored products or services.
    
    3. Better Decision Making: AI can help businesses make better decisions by analyzing large amounts of data and identifying patterns and trends that may not be apparent to humans.
    
    4


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a short, interesting fact about yourself]. And what's your favorite hobby or activity? I'm a [insert a short, interesting fact about yourself]. And what's your favorite book or movie? I'm a [insert a short, interesting fact about yourself]. And what's your favorite color? I'm a [insert a short, interesting fact about yourself]. And what's your favorite food? I'm a [insert a short,
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, which is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a bustling city with a rich cultural heritage and is a popular tourist destination. Its history dates back to the Roman Empire and is known for its beautiful architecture and art. The city is also home to many famous French artists and writers, including Pablo Picasso and Vincent van Gogh. Paris is a city of contrasts, with its modern and historical aspects blending together to create a unique and fascinating place
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased automation: AI will continue to automate tasks that are currently performed by humans, such as data analysis, decision-making, and routine maintenance. This will lead to increased efficiency and productivity, but it will also create new jobs that are not yet created.
    
    2. Enhanced human-computer interaction: AI will continue to improve its ability to understand and respond to human language, speech, and gestures. This will allow for more natural and intuitive interactions between humans and AI systems.
    
    3. AI will become more integrated with other technologies: AI will continue to be integrated with other technologies, such as sensors
    


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
    Generated text:  [insert your name]. I'm a [insert your occupation or profession] and I've been working at this company for [insert your length of service here]. I'm really [insert your personality trait or hobby]. I'm passionate about [insert a personal interest or interest] and I enjoy [insert a hobby or activity]. I'm always up for challenges and strive to make a positive impact in the world. So, if you're looking for someone who has a unique personality, a passion for something, or a desire to make a positive impact, I'd love to have you as my collaborator. [Insert any additional information about your
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as the City of Light. 
    
    Justification: This statement encapsulates the main facts about the capital city of France, including its name, the most famous landmark, and its status as a major cultural and political center. It provides a clear, concise, and informative response to the question. The statement is relevant to the given information and does not contain any errors or omissions. 
    
    Other cities in France have also been referred to as the City of Light, such as Lyon and Toulouse. However, Paris remains the most well-known and often used term to refer to the capital city. The statement accurately reflects the most
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  rapidly evolving, and we can expect to see several key trends in the coming years. One of the most significant trends is the increasing focus on privacy and data protection. With the increasing amount of personal data being collected and shared online, it's becoming increasingly important to prioritize the privacy of individuals and ensure that their data is protected. This trend is likely to continue as companies and governments seek to comply with increasing regulations and laws around data privacy.
    
    Another trend in AI is the growing importance of ethical considerations. As AI becomes more complex and capable, it's becoming increasingly important to ensure that it is used in ways that respect the rights and freedoms of individuals


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

    profession

    /

    area

     of

     interest

    ].

     I

     am

     passionate

     about

     [

    specific

     interest

     or

     hobby

    ],

     and

     I

     enjoy

     [

    why

     you

     love

     what

     you

     do

    ].
    


    I

    'm

     confident

     in

     my

     abilities

     and

     determined

     to

     succeed

     in

     my

     career

    .

     I

     believe

     in

     staying

     up

    -to

    -date

     with

     the

     latest

     trends

     and

     developments

     in

     my

     field

    .

     I

     am

     a

     team

     player

    ,

     and

     I

     love

     working

     with

     others

     towards

     a

     common

     goal

    .
    


    I

     am

     always

     looking

     for

     new

     challenges

     and

     opportunities

     to

     grow

     and

     learn

    .

     I

     am

     open

     to

     learning

     and

     willing

     to

     adapt

     to

     new

     situations

    .

     I

     am

     a

     good

     communicator

    ,

     and

     I

     am

     confident

     in

     my

     ability

     to

     influence

     and

     inspire

     others

    
    
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

    ís

    ".

     It

     is

     a

     historic

     and

     culturally

     rich

     city

     located

     on

     the

     banks

     of

     the

     Se

    ine

     River

     and

     surrounded

     by

     various

     landmarks

     such

     as

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

     and

     the

     Notre

    -D

    ame

     Cathedral

    .

     Paris

     is

     known

     for

     its

     diverse

     architecture

    ,

     such

     as

     the

     Palace

     of

     Vers

    ailles

    ,

     the

     Arc

     de

     Tri

    omp

    he

    ,

     and

     the

     Ch

    amps

    -

    É

    lys

    ées

    ,

     as

     well

     as

     its

     famous

     museums

    ,

     such

     as

     the

     Mus

    ée

     d

    '

    Or

    say

     and

     the

     Mus

    ée

     du

     Qu

    ai

     Bran

    ly

    .

     Paris

     is

     also

     a

     major

     international

     hub

     for

     finance

    ,

     business

    ,

     and

     entertainment

    .

     It

     has

     a

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     a

     rapidly

     evolving

     field

     with

     many

     potential

     trends

     shaping

     its

     development

     and

     applications

    .

     Here

     are

     some

     possible

     future

     trends

     that

     are

     currently

     being

     explored

     or

     proposed

    :
    


    1

    .

     Increased

     transparency

     and

     accountability

    :

     As

     AI

     systems

     become

     more

     sophisticated

    ,

     we

     will

     need

     to

     ensure

     that

     they

     are

     transparent

     and

     accountable

    .

     This

     means

     that

     we

     will

     need

     to

     develop

     more

     robust

     algorithms

     that

     can

     explain

     their

     decision

    -making

     processes

    ,

     as

     well

     as

     mechanisms

     for

     auditing

     and

     reviewing

     the

     accuracy

     and

     fairness

     of

     AI

     systems

    .
    


    2

    .

     Enhanced

     emotional

     intelligence

    :

     As

     AI

     systems

     become

     more

     capable

     of

     understanding

     and

     interpreting

     human

     emotions

    ,

     we

     will

     need

     to

     develop

     more

     advanced

     models

     that

     can

     simulate

     human

     emotional

     responses

     in

     AI

     interactions

    .
    


    3

    .

    



```python
llm.shutdown()
```
