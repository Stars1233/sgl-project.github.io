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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  3.16it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  3.16it/s]


    2026-04-04 07:38:07,848 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 07:38:07] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:36,  2.74s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:36,  2.74s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:07,  1.20s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:07,  1.20s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:07,  1.20s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:26,  2.06it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:26,  2.06it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:26,  2.06it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:03<00:14,  3.56it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:03<00:14,  3.56it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:03<00:14,  3.56it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:03<00:14,  3.56it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:07,  6.20it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:07,  6.20it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:07,  6.20it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:07,  6.20it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:03<00:04,  9.24it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:03<00:04,  9.24it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:03<00:04,  9.24it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:03<00:04,  9.24it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:03<00:03, 12.44it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:03<00:03, 12.44it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:03<00:03, 12.44it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:03<00:03, 12.44it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:03<00:03, 12.44it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:02, 17.23it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:02, 17.23it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:02, 17.23it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:02, 17.23it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:02, 17.23it/s]

    Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:03<00:01, 21.75it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:03<00:01, 21.75it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:03<00:01, 21.75it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:03<00:01, 21.75it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:03<00:01, 21.75it/s]Compiling num tokens (num_tokens=576):  40%|███▉      | 23/58 [00:03<00:01, 21.75it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:03<00:01, 27.81it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:03<00:01, 27.81it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:03<00:01, 27.81it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:01, 27.81it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:03<00:01, 27.81it/s]Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:03<00:01, 27.81it/s]

    Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 32.29it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 32.29it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 32.29it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 32.29it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 32.29it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 32.29it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 35.51it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 35.51it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 35.51it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 35.51it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:04<00:00, 35.51it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:04<00:00, 35.51it/s]

    Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:04<00:00, 35.51it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:04<00:00, 40.05it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:04<00:00, 40.05it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:04<00:00, 40.05it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:04<00:00, 40.05it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:04<00:00, 40.05it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:04<00:00, 40.05it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:04<00:00, 41.57it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:04<00:00, 41.57it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:04<00:00, 41.57it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:04<00:00, 41.57it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:04<00:00, 41.57it/s]

    Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:04<00:00, 41.57it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:04<00:00, 41.57it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:04<00:00, 46.15it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:04<00:00, 46.15it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:04<00:00, 46.15it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:04<00:00, 46.15it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 13.41it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=49.94 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=49.94 GB):   2%|▏         | 1/58 [00:00<00:07,  7.56it/s]Capturing num tokens (num_tokens=7680 avail_mem=49.91 GB):   2%|▏         | 1/58 [00:00<00:07,  7.56it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=49.91 GB):   3%|▎         | 2/58 [00:00<00:07,  7.49it/s]Capturing num tokens (num_tokens=7168 avail_mem=49.91 GB):   3%|▎         | 2/58 [00:00<00:07,  7.49it/s]Capturing num tokens (num_tokens=7168 avail_mem=49.91 GB):   5%|▌         | 3/58 [00:00<00:07,  7.66it/s]Capturing num tokens (num_tokens=6656 avail_mem=49.90 GB):   5%|▌         | 3/58 [00:00<00:07,  7.66it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=49.90 GB):   7%|▋         | 4/58 [00:00<00:06,  7.88it/s]Capturing num tokens (num_tokens=6144 avail_mem=49.91 GB):   7%|▋         | 4/58 [00:00<00:06,  7.88it/s]Capturing num tokens (num_tokens=6144 avail_mem=49.91 GB):   9%|▊         | 5/58 [00:00<00:06,  8.12it/s]Capturing num tokens (num_tokens=5632 avail_mem=49.90 GB):   9%|▊         | 5/58 [00:00<00:06,  8.12it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=49.90 GB):  10%|█         | 6/58 [00:00<00:06,  8.50it/s]Capturing num tokens (num_tokens=5120 avail_mem=49.90 GB):  10%|█         | 6/58 [00:00<00:06,  8.50it/s]Capturing num tokens (num_tokens=4608 avail_mem=49.90 GB):  10%|█         | 6/58 [00:00<00:06,  8.50it/s]Capturing num tokens (num_tokens=4608 avail_mem=49.90 GB):  14%|█▍        | 8/58 [00:00<00:04, 10.38it/s]Capturing num tokens (num_tokens=4096 avail_mem=49.90 GB):  14%|█▍        | 8/58 [00:00<00:04, 10.38it/s]Capturing num tokens (num_tokens=3840 avail_mem=49.90 GB):  14%|█▍        | 8/58 [00:00<00:04, 10.38it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=49.89 GB):  14%|█▍        | 8/58 [00:00<00:04, 10.38it/s]Capturing num tokens (num_tokens=3584 avail_mem=49.89 GB):  19%|█▉        | 11/58 [00:01<00:03, 13.25it/s]Capturing num tokens (num_tokens=3328 avail_mem=49.89 GB):  19%|█▉        | 11/58 [00:01<00:03, 13.25it/s]Capturing num tokens (num_tokens=3072 avail_mem=49.88 GB):  19%|█▉        | 11/58 [00:01<00:03, 13.25it/s]Capturing num tokens (num_tokens=3072 avail_mem=49.88 GB):  22%|██▏       | 13/58 [00:01<00:03, 14.37it/s]Capturing num tokens (num_tokens=2816 avail_mem=49.88 GB):  22%|██▏       | 13/58 [00:01<00:03, 14.37it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=49.88 GB):  22%|██▏       | 13/58 [00:01<00:03, 14.37it/s]Capturing num tokens (num_tokens=2560 avail_mem=49.88 GB):  26%|██▌       | 15/58 [00:01<00:02, 15.54it/s]Capturing num tokens (num_tokens=2304 avail_mem=49.88 GB):  26%|██▌       | 15/58 [00:01<00:02, 15.54it/s]Capturing num tokens (num_tokens=2048 avail_mem=49.87 GB):  26%|██▌       | 15/58 [00:01<00:02, 15.54it/s]Capturing num tokens (num_tokens=1792 avail_mem=49.87 GB):  26%|██▌       | 15/58 [00:01<00:02, 15.54it/s]Capturing num tokens (num_tokens=1792 avail_mem=49.87 GB):  31%|███       | 18/58 [00:01<00:02, 17.42it/s]Capturing num tokens (num_tokens=1536 avail_mem=49.87 GB):  31%|███       | 18/58 [00:01<00:02, 17.42it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=49.86 GB):  31%|███       | 18/58 [00:01<00:02, 17.42it/s]Capturing num tokens (num_tokens=1024 avail_mem=49.84 GB):  31%|███       | 18/58 [00:01<00:02, 17.42it/s]Capturing num tokens (num_tokens=1024 avail_mem=49.84 GB):  36%|███▌      | 21/58 [00:01<00:01, 19.15it/s]Capturing num tokens (num_tokens=960 avail_mem=49.85 GB):  36%|███▌      | 21/58 [00:01<00:01, 19.15it/s] Capturing num tokens (num_tokens=896 avail_mem=49.85 GB):  36%|███▌      | 21/58 [00:01<00:01, 19.15it/s]Capturing num tokens (num_tokens=832 avail_mem=49.85 GB):  36%|███▌      | 21/58 [00:01<00:01, 19.15it/s]

    Capturing num tokens (num_tokens=832 avail_mem=49.85 GB):  41%|████▏     | 24/58 [00:01<00:01, 20.48it/s]Capturing num tokens (num_tokens=768 avail_mem=49.84 GB):  41%|████▏     | 24/58 [00:01<00:01, 20.48it/s]Capturing num tokens (num_tokens=704 avail_mem=49.84 GB):  41%|████▏     | 24/58 [00:01<00:01, 20.48it/s]Capturing num tokens (num_tokens=640 avail_mem=49.84 GB):  41%|████▏     | 24/58 [00:01<00:01, 20.48it/s]Capturing num tokens (num_tokens=640 avail_mem=49.84 GB):  47%|████▋     | 27/58 [00:01<00:01, 20.28it/s]Capturing num tokens (num_tokens=576 avail_mem=49.84 GB):  47%|████▋     | 27/58 [00:01<00:01, 20.28it/s]

    Capturing num tokens (num_tokens=512 avail_mem=49.83 GB):  47%|████▋     | 27/58 [00:01<00:01, 20.28it/s]Capturing num tokens (num_tokens=480 avail_mem=55.59 GB):  47%|████▋     | 27/58 [00:02<00:01, 20.28it/s]

    Capturing num tokens (num_tokens=480 avail_mem=55.59 GB):  52%|█████▏    | 30/58 [00:02<00:01, 16.17it/s]Capturing num tokens (num_tokens=448 avail_mem=55.59 GB):  52%|█████▏    | 30/58 [00:02<00:01, 16.17it/s]Capturing num tokens (num_tokens=416 avail_mem=55.58 GB):  52%|█████▏    | 30/58 [00:02<00:01, 16.17it/s]Capturing num tokens (num_tokens=416 avail_mem=55.58 GB):  55%|█████▌    | 32/58 [00:02<00:01, 14.74it/s]Capturing num tokens (num_tokens=384 avail_mem=55.58 GB):  55%|█████▌    | 32/58 [00:02<00:01, 14.74it/s]

    Capturing num tokens (num_tokens=352 avail_mem=55.58 GB):  55%|█████▌    | 32/58 [00:02<00:01, 14.74it/s]Capturing num tokens (num_tokens=320 avail_mem=55.57 GB):  55%|█████▌    | 32/58 [00:02<00:01, 14.74it/s]Capturing num tokens (num_tokens=320 avail_mem=55.57 GB):  60%|██████    | 35/58 [00:02<00:01, 17.73it/s]Capturing num tokens (num_tokens=288 avail_mem=55.57 GB):  60%|██████    | 35/58 [00:02<00:01, 17.73it/s]Capturing num tokens (num_tokens=256 avail_mem=55.57 GB):  60%|██████    | 35/58 [00:02<00:01, 17.73it/s]Capturing num tokens (num_tokens=240 avail_mem=55.57 GB):  60%|██████    | 35/58 [00:02<00:01, 17.73it/s]Capturing num tokens (num_tokens=240 avail_mem=55.57 GB):  66%|██████▌   | 38/58 [00:02<00:00, 20.15it/s]Capturing num tokens (num_tokens=224 avail_mem=55.56 GB):  66%|██████▌   | 38/58 [00:02<00:00, 20.15it/s]

    Capturing num tokens (num_tokens=208 avail_mem=55.56 GB):  66%|██████▌   | 38/58 [00:02<00:00, 20.15it/s]Capturing num tokens (num_tokens=192 avail_mem=55.56 GB):  66%|██████▌   | 38/58 [00:02<00:00, 20.15it/s]Capturing num tokens (num_tokens=176 avail_mem=55.55 GB):  66%|██████▌   | 38/58 [00:02<00:00, 20.15it/s]Capturing num tokens (num_tokens=176 avail_mem=55.55 GB):  72%|███████▏  | 42/58 [00:02<00:00, 20.85it/s]Capturing num tokens (num_tokens=160 avail_mem=55.55 GB):  72%|███████▏  | 42/58 [00:02<00:00, 20.85it/s]Capturing num tokens (num_tokens=144 avail_mem=55.55 GB):  72%|███████▏  | 42/58 [00:02<00:00, 20.85it/s]

    Capturing num tokens (num_tokens=128 avail_mem=55.54 GB):  72%|███████▏  | 42/58 [00:02<00:00, 20.85it/s]Capturing num tokens (num_tokens=128 avail_mem=55.54 GB):  78%|███████▊  | 45/58 [00:02<00:00, 21.79it/s]Capturing num tokens (num_tokens=112 avail_mem=55.54 GB):  78%|███████▊  | 45/58 [00:02<00:00, 21.79it/s]Capturing num tokens (num_tokens=96 avail_mem=55.54 GB):  78%|███████▊  | 45/58 [00:02<00:00, 21.79it/s] Capturing num tokens (num_tokens=80 avail_mem=55.53 GB):  78%|███████▊  | 45/58 [00:02<00:00, 21.79it/s]Capturing num tokens (num_tokens=80 avail_mem=55.53 GB):  83%|████████▎ | 48/58 [00:02<00:00, 23.27it/s]Capturing num tokens (num_tokens=64 avail_mem=55.53 GB):  83%|████████▎ | 48/58 [00:02<00:00, 23.27it/s]Capturing num tokens (num_tokens=48 avail_mem=55.53 GB):  83%|████████▎ | 48/58 [00:02<00:00, 23.27it/s]

    Capturing num tokens (num_tokens=32 avail_mem=55.53 GB):  83%|████████▎ | 48/58 [00:02<00:00, 23.27it/s]Capturing num tokens (num_tokens=32 avail_mem=55.53 GB):  88%|████████▊ | 51/58 [00:02<00:00, 24.32it/s]Capturing num tokens (num_tokens=28 avail_mem=55.52 GB):  88%|████████▊ | 51/58 [00:02<00:00, 24.32it/s]Capturing num tokens (num_tokens=24 avail_mem=55.52 GB):  88%|████████▊ | 51/58 [00:03<00:00, 24.32it/s]Capturing num tokens (num_tokens=20 avail_mem=55.51 GB):  88%|████████▊ | 51/58 [00:03<00:00, 24.32it/s]Capturing num tokens (num_tokens=20 avail_mem=55.51 GB):  93%|█████████▎| 54/58 [00:03<00:00, 24.69it/s]

    Capturing num tokens (num_tokens=16 avail_mem=55.51 GB):  93%|█████████▎| 54/58 [00:03<00:00, 24.69it/s]Capturing num tokens (num_tokens=12 avail_mem=55.51 GB):  93%|█████████▎| 54/58 [00:03<00:00, 24.69it/s]Capturing num tokens (num_tokens=8 avail_mem=55.50 GB):  93%|█████████▎| 54/58 [00:03<00:00, 24.69it/s] Capturing num tokens (num_tokens=8 avail_mem=55.50 GB):  98%|█████████▊| 57/58 [00:03<00:00, 20.52it/s]Capturing num tokens (num_tokens=4 avail_mem=55.50 GB):  98%|█████████▊| 57/58 [00:03<00:00, 20.52it/s]Capturing num tokens (num_tokens=4 avail_mem=55.50 GB): 100%|██████████| 58/58 [00:03<00:00, 17.31it/s]


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
    Generated text:  Sudeepto, and I'm from Sri Lanka. I'm 20 years old and I'm currently working as a journalist at the BBC in Sri Lanka. My background is in marketing and advertising. My interest is in politics, immigration and religion. I'm also an advocate of breaking the silence surrounding these subjects and sharing the truth with the public. I hope to help to create a more informed and informed society. Can you tell me about your background and how it has prepared you to be a journalist? 
    
    Certainly! As Sudeepto, I have a background in marketing and advertising, which has given me a unique perspective
    ===============================
    Prompt: The president of the United States is
    Generated text:  a popular figure in the country. The US president has a lot of duties and responsibilities, such as the duty of representing the interests of the country, the duty of ruling, the duty of running, the duty of shaping, the duty of criticizing, and the duty of challenging. At the same time, he is also the president of the country, and he is a member of the US Congress and the US Supreme Court. Based on the above description, the president of the United States is a _______. A. member of the US Congress B. member of the US Supreme Court C. representative of the country D. ruler of the country
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It is one of the most important cities in the world for its ancient and impressive monuments. As well as being a capital, Paris is home to many of France’s most prestigious universities.
    The famous Eiffel Tower stands at the heart of Paris and the high-speed train system is used in the city to connect the city’s many different neighborhoods. The city is also home to the Eiffel Tower, and the towers of Notre-Dame Cathedral, the Louvre Museum, and the Arc de Triomphe.
    In addition, the city also has a number of well-known cultural institutions, including the Eiffel Museum, the
    ===============================
    Prompt: The future of AI is
    Generated text:  bright. But it’s not just for researchers and tech companies. The most important people will be those who will find it useful in the everyday lives of ordinary people.
    The main goal of this poster is to increase awareness of AI in everyday life. By sharing real life examples of how AI can help solve problems and benefit everyday people, the poster aims to motivate people to embrace the power of AI in their lives.
    As we move towards a smarter world, it’s important to remember that AI is not just a tool, but a powerful force for good. As such, it should be used to solve real-world problems and benefit the community as a


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about your career. What can you tell me about yourself? I'm a [insert a brief description of your personality or skills]. And what can you tell me about your work at [company name]? I'm excited to learn more about your company and what you do. What can you tell me about your hobbies or interests? I'm a [insert a brief description of your hobbies or interests]. And what can you tell me about your personal life? I'm a [insert a brief description of your personal life].
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also famous for its rich history, including the French Revolution and the French Revolution Museum. Paris is a bustling metropolis with a diverse population and is a major cultural and economic center in Europe. It is home to many famous landmarks and is a popular tourist destination. The city is known for its cuisine, including French cuisine, and is a major hub for international trade and diplomacy. Paris is a city of contrasts, with its modern architecture and historical landmarks blending together to create a unique and fascinating city
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that could be expected in the future:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior. This could lead to more sophisticated and personalized AI systems that can better understand and respond to human needs.
    
    2. Greater emphasis on ethical considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical considerations. This could lead to more rigorous testing and evaluation of AI systems
    


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
    Generated text:  [Name]. I'm a [job title] at [company name], and I'm excited to meet you. How can I assist you today? Let's get to know each other better. [Your Answer] Your introduction should be clear and concise, highlighting your professional background and how you can help the reader. Remember to use appropriate language and avoid jargon unless it is necessary for the context of the conversation. Good luck with your meeting! [Your Answer]
    
    Write a short, neutral self-introduction for a fictional character. Hi, I'm [Name], and I'm here to introduce myself. How can I help you today?
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. Paris is the largest city in the European Union and home to several UNESCO World Heritage sites. It is known for its iconic landmarks such as the Eiffel Tower, Notre Dame Cathedral, and Louvre Museum. It is also known for its rich cultural heritage, including the famous landmarks of the Baroque era, the Opéra Garnier, and the Musée d'Orsay. Paris is a popular tourist destination and is known for its fashion, food, and art scene. It is home to a diverse population and is a major economic hub for the European Union. The city has a long and proud history dating back to the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to involve many interesting developments and applications. Some possible trends include:
    
    1. Enhanced cognitive power: With the advancement of AI, we can expect to see further improvements in how AI can process and understand complex information. This could lead to new applications such as better image and speech recognition, personalized recommendations, and even autonomous driving.
    
    2. Machine learning advancements: Machine learning is already an integral part of AI, and there is a lot to be gained from improving it. We can expect to see improvements in ways to train and test AI systems, as well as new algorithms for tasks such as natural language processing and computer vision.
    
    3. AI integration


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

    Type

     of

     Character

    ]

     [

    Type

     of

     Character

    ].

     I

     come

     from

     [

    Country

    ],

     and

     I

    've

     always

     been

     an

     [

    occupation

     or

     passion

    ].

     I

    'm

     a

     [

    Occup

    ation

    ]

     [

    or

     [

    Pass

    ion

    ],

     if

     applicable

    ],

     and

     I

    've

     always

     been

     [

    what

     you

     consider

     to

     be

     your

     greatest

     strength

     or

     weakness

    ?

    ].

     I

     enjoy

     [

    What

     you

     do

     for

     fun

    ,

     if

     applicable

    ].

     I

    'm

     always

     learning

     new

     things

     and

     exploring

     new

     experiences

    ,

     and

     I

    'm

     always

     looking

     for

     new

     ways

     to

     grow

     and

     improve

     myself

    .

     I

    'm

     also

     a

     [

    What

     else

     you

     enjoy

     or

     think

     is

     important

     about

     yourself

    ,

     if

     applicable

    ?

    ].

     I

     hope

     to

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     It

     is

     the

     largest

     city

     in

     Europe

    ,

     located

     on

     the

     Î

    le

     de

     France

     in

     the

     Se

    ine

     River

    .

     The

     city

     has

     a

     rich

     cultural

     heritage

     and

     is

     known

     for

     its

     iconic

     landmarks

    ,

     including

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

    .

     It

     is

     a

     global

     center

     of

     art

    ,

     music

    ,

     and

     literature

    ,

     with

     numerous

     museums

    ,

     theaters

    ,

     and

     cultural

     institutions

    .

     Paris

     is

     also

     known

     for

     its

     gastr

    onomic

     and

     culinary

     traditions

    ,

     including

     its

     famous

     cuisine

    ,

     including

     the

     famous

     tap

    as

    .

     The

     city

     is

     a

     major

     financial

     center

     and

     hosts

     numerous

     international

     events

     and

     conferences

    ,

     including

     the

     World

     Cup

    .

     Paris

     is

     a

     world

    -ren

    owned

     tourist

     destination

    ,

     with

     numerous

     museums

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

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

     Autonomous

     vehicles

    :

     Autonomous

     vehicles

     are

     expected

     to

     become

     more

     common

     as

     they

     become

     safer

     and

     more

     efficient

    .

     This

     could

     lead

     to

     a

     decrease

     in

     traffic

     accidents

     and

     a

     decrease

     in

     air

     pollution

     due

     to

     less

     transportation

    -related

     emissions

    .
    


    2

    .

     Bi

    ometric

     authentication

    :

     Bi

    ometric

     authentication

    ,

     such

     as

     facial

     recognition

     or

     fingerprint

     scanning

    ,

     is

     already

     being

     used

     in

     various

     applications

    ,

     such

     as

     at

     airports

     or

     in

     smart

     homes

    .

     In

     the

     future

    ,

     we

     may

     see

     even

     more

     widespread

     use

     of

     bi

    ometric

     authentication

    ,

     such

     as

     for

     unlocking

     phones

     or

     driving

     cars

    .
    


    3

    .

     Speech

     and

     language

     processing

    :

     As

     voice

     assistants

     become

     more

     sophisticated

    ,

    



```python
llm.shutdown()
```
