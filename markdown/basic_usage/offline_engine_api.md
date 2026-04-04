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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.03it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.03it/s]


    2026-04-04 08:30:34,718 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 08:30:34] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.67it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.67it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.67it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.67it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.67it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.67it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.67it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.67it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:08,  5.67it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:03<00:08,  5.67it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.80it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.80it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.80it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.80it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.80it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.80it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.80it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.80it/s]Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:03<00:03, 12.80it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 19.50it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 19.50it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 19.50it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 19.50it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 19.50it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 19.50it/s]

    Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 19.50it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:01, 21.94it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:01, 21.94it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:01, 21.94it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:01, 21.94it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:01, 21.94it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:01, 21.94it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 24.16it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 24.16it/s]

    Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 24.16it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 24.16it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 24.16it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 24.16it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:03<00:00, 27.27it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:03<00:00, 27.27it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:03<00:00, 27.27it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:03<00:00, 27.27it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:03<00:00, 27.27it/s] 

    Compiling num tokens (num_tokens=80):  74%|███████▍  | 43/58 [00:03<00:00, 27.27it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:03<00:00, 29.62it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:03<00:00, 29.62it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:03<00:00, 29.62it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:03<00:00, 29.62it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:03<00:00, 29.62it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:03<00:00, 29.62it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:03<00:00, 31.47it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:03<00:00, 31.47it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:03<00:00, 31.47it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:03<00:00, 31.47it/s]

    Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:03<00:00, 31.47it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:03<00:00, 31.47it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 14.58it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=71.25 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.22 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.22 GB):   3%|▎         | 2/58 [00:00<00:04, 13.26it/s]Capturing num tokens (num_tokens=7168 avail_mem=71.22 GB):   3%|▎         | 2/58 [00:00<00:04, 13.26it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=71.22 GB):   3%|▎         | 2/58 [00:00<00:04, 13.26it/s]Capturing num tokens (num_tokens=6656 avail_mem=71.22 GB):   7%|▋         | 4/58 [00:00<00:04, 13.41it/s]Capturing num tokens (num_tokens=6144 avail_mem=71.22 GB):   7%|▋         | 4/58 [00:00<00:04, 13.41it/s]Capturing num tokens (num_tokens=5632 avail_mem=71.21 GB):   7%|▋         | 4/58 [00:00<00:04, 13.41it/s]Capturing num tokens (num_tokens=5632 avail_mem=71.21 GB):  10%|█         | 6/58 [00:00<00:03, 15.59it/s]Capturing num tokens (num_tokens=5120 avail_mem=71.22 GB):  10%|█         | 6/58 [00:00<00:03, 15.59it/s]Capturing num tokens (num_tokens=4608 avail_mem=71.21 GB):  10%|█         | 6/58 [00:00<00:03, 15.59it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=71.21 GB):  10%|█         | 6/58 [00:00<00:03, 15.59it/s]Capturing num tokens (num_tokens=4096 avail_mem=71.21 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.99it/s]Capturing num tokens (num_tokens=3840 avail_mem=71.21 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.99it/s]Capturing num tokens (num_tokens=3584 avail_mem=71.20 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.99it/s]Capturing num tokens (num_tokens=3328 avail_mem=71.20 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.99it/s]Capturing num tokens (num_tokens=3072 avail_mem=71.20 GB):  16%|█▌        | 9/58 [00:00<00:02, 18.99it/s]Capturing num tokens (num_tokens=3072 avail_mem=71.20 GB):  22%|██▏       | 13/58 [00:00<00:01, 24.88it/s]Capturing num tokens (num_tokens=2816 avail_mem=71.20 GB):  22%|██▏       | 13/58 [00:00<00:01, 24.88it/s]Capturing num tokens (num_tokens=2560 avail_mem=70.62 GB):  22%|██▏       | 13/58 [00:00<00:01, 24.88it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=71.20 GB):  22%|██▏       | 13/58 [00:00<00:01, 24.88it/s]Capturing num tokens (num_tokens=2304 avail_mem=71.20 GB):  28%|██▊       | 16/58 [00:00<00:02, 18.10it/s]Capturing num tokens (num_tokens=2048 avail_mem=71.16 GB):  28%|██▊       | 16/58 [00:00<00:02, 18.10it/s]Capturing num tokens (num_tokens=1792 avail_mem=70.71 GB):  28%|██▊       | 16/58 [00:00<00:02, 18.10it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=71.15 GB):  28%|██▊       | 16/58 [00:01<00:02, 18.10it/s]Capturing num tokens (num_tokens=1536 avail_mem=71.15 GB):  33%|███▎      | 19/58 [00:01<00:02, 15.04it/s]Capturing num tokens (num_tokens=1280 avail_mem=71.16 GB):  33%|███▎      | 19/58 [00:01<00:02, 15.04it/s]Capturing num tokens (num_tokens=1024 avail_mem=71.12 GB):  33%|███▎      | 19/58 [00:01<00:02, 15.04it/s]Capturing num tokens (num_tokens=1024 avail_mem=71.12 GB):  36%|███▌      | 21/58 [00:01<00:02, 15.77it/s]Capturing num tokens (num_tokens=960 avail_mem=71.14 GB):  36%|███▌      | 21/58 [00:01<00:02, 15.77it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=71.11 GB):  36%|███▌      | 21/58 [00:01<00:02, 15.77it/s]Capturing num tokens (num_tokens=896 avail_mem=71.11 GB):  40%|███▉      | 23/58 [00:01<00:02, 16.57it/s]Capturing num tokens (num_tokens=832 avail_mem=71.13 GB):  40%|███▉      | 23/58 [00:01<00:02, 16.57it/s]Capturing num tokens (num_tokens=768 avail_mem=70.78 GB):  40%|███▉      | 23/58 [00:01<00:02, 16.57it/s]Capturing num tokens (num_tokens=704 avail_mem=71.12 GB):  40%|███▉      | 23/58 [00:01<00:02, 16.57it/s]

    Capturing num tokens (num_tokens=704 avail_mem=71.12 GB):  45%|████▍     | 26/58 [00:01<00:01, 17.56it/s]Capturing num tokens (num_tokens=640 avail_mem=70.81 GB):  45%|████▍     | 26/58 [00:01<00:01, 17.56it/s]Capturing num tokens (num_tokens=576 avail_mem=71.12 GB):  45%|████▍     | 26/58 [00:01<00:01, 17.56it/s]Capturing num tokens (num_tokens=512 avail_mem=71.11 GB):  45%|████▍     | 26/58 [00:01<00:01, 17.56it/s]Capturing num tokens (num_tokens=512 avail_mem=71.11 GB):  50%|█████     | 29/58 [00:01<00:01, 19.03it/s]Capturing num tokens (num_tokens=480 avail_mem=70.88 GB):  50%|█████     | 29/58 [00:01<00:01, 19.03it/s]Capturing num tokens (num_tokens=448 avail_mem=71.12 GB):  50%|█████     | 29/58 [00:01<00:01, 19.03it/s]

    Capturing num tokens (num_tokens=416 avail_mem=71.11 GB):  50%|█████     | 29/58 [00:01<00:01, 19.03it/s]Capturing num tokens (num_tokens=416 avail_mem=71.11 GB):  55%|█████▌    | 32/58 [00:01<00:01, 20.43it/s]Capturing num tokens (num_tokens=384 avail_mem=71.11 GB):  55%|█████▌    | 32/58 [00:01<00:01, 20.43it/s]Capturing num tokens (num_tokens=352 avail_mem=70.96 GB):  55%|█████▌    | 32/58 [00:01<00:01, 20.43it/s]Capturing num tokens (num_tokens=320 avail_mem=71.10 GB):  55%|█████▌    | 32/58 [00:01<00:01, 20.43it/s]Capturing num tokens (num_tokens=320 avail_mem=71.10 GB):  60%|██████    | 35/58 [00:01<00:01, 22.20it/s]Capturing num tokens (num_tokens=288 avail_mem=71.09 GB):  60%|██████    | 35/58 [00:01<00:01, 22.20it/s]Capturing num tokens (num_tokens=256 avail_mem=71.09 GB):  60%|██████    | 35/58 [00:01<00:01, 22.20it/s]

    Capturing num tokens (num_tokens=240 avail_mem=70.92 GB):  60%|██████    | 35/58 [00:01<00:01, 22.20it/s]Capturing num tokens (num_tokens=240 avail_mem=70.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 23.89it/s]Capturing num tokens (num_tokens=224 avail_mem=71.08 GB):  66%|██████▌   | 38/58 [00:01<00:00, 23.89it/s]Capturing num tokens (num_tokens=208 avail_mem=71.07 GB):  66%|██████▌   | 38/58 [00:02<00:00, 23.89it/s]Capturing num tokens (num_tokens=192 avail_mem=71.06 GB):  66%|██████▌   | 38/58 [00:02<00:00, 23.89it/s]Capturing num tokens (num_tokens=192 avail_mem=71.06 GB):  71%|███████   | 41/58 [00:02<00:00, 25.19it/s]Capturing num tokens (num_tokens=176 avail_mem=71.06 GB):  71%|███████   | 41/58 [00:02<00:00, 25.19it/s]Capturing num tokens (num_tokens=160 avail_mem=70.95 GB):  71%|███████   | 41/58 [00:02<00:00, 25.19it/s]

    Capturing num tokens (num_tokens=144 avail_mem=71.00 GB):  71%|███████   | 41/58 [00:02<00:00, 25.19it/s]Capturing num tokens (num_tokens=144 avail_mem=71.00 GB):  76%|███████▌  | 44/58 [00:02<00:00, 21.65it/s]Capturing num tokens (num_tokens=128 avail_mem=71.04 GB):  76%|███████▌  | 44/58 [00:02<00:00, 21.65it/s]Capturing num tokens (num_tokens=112 avail_mem=71.00 GB):  76%|███████▌  | 44/58 [00:02<00:00, 21.65it/s]

    Capturing num tokens (num_tokens=96 avail_mem=71.00 GB):  76%|███████▌  | 44/58 [00:02<00:00, 21.65it/s] Capturing num tokens (num_tokens=96 avail_mem=71.00 GB):  81%|████████  | 47/58 [00:02<00:00, 13.17it/s]Capturing num tokens (num_tokens=80 avail_mem=71.00 GB):  81%|████████  | 47/58 [00:02<00:00, 13.17it/s]Capturing num tokens (num_tokens=64 avail_mem=71.00 GB):  81%|████████  | 47/58 [00:02<00:00, 13.17it/s]

    Capturing num tokens (num_tokens=64 avail_mem=71.00 GB):  84%|████████▍ | 49/58 [00:02<00:00, 13.23it/s]Capturing num tokens (num_tokens=48 avail_mem=71.01 GB):  84%|████████▍ | 49/58 [00:02<00:00, 13.23it/s]Capturing num tokens (num_tokens=32 avail_mem=70.99 GB):  84%|████████▍ | 49/58 [00:02<00:00, 13.23it/s]Capturing num tokens (num_tokens=28 avail_mem=70.99 GB):  84%|████████▍ | 49/58 [00:02<00:00, 13.23it/s]Capturing num tokens (num_tokens=24 avail_mem=70.99 GB):  84%|████████▍ | 49/58 [00:02<00:00, 13.23it/s]Capturing num tokens (num_tokens=24 avail_mem=70.99 GB):  91%|█████████▏| 53/58 [00:02<00:00, 17.27it/s]Capturing num tokens (num_tokens=20 avail_mem=70.98 GB):  91%|█████████▏| 53/58 [00:02<00:00, 17.27it/s]Capturing num tokens (num_tokens=16 avail_mem=70.98 GB):  91%|█████████▏| 53/58 [00:03<00:00, 17.27it/s]Capturing num tokens (num_tokens=12 avail_mem=70.97 GB):  91%|█████████▏| 53/58 [00:03<00:00, 17.27it/s]Capturing num tokens (num_tokens=8 avail_mem=70.96 GB):  91%|█████████▏| 53/58 [00:03<00:00, 17.27it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=70.96 GB):  98%|█████████▊| 57/58 [00:03<00:00, 21.39it/s]Capturing num tokens (num_tokens=4 avail_mem=70.96 GB):  98%|█████████▊| 57/58 [00:03<00:00, 21.39it/s]Capturing num tokens (num_tokens=4 avail_mem=70.96 GB): 100%|██████████| 58/58 [00:03<00:00, 18.66it/s]


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
    Generated text:  Alan Johnson. I am a student at St Mary's College in Birmingham, United Kingdom. I am a serious professional and I am determined to succeed in my studies and in the future. I have had a lot of experience in my life, but I do not know how to decide what to do with my life. I have not found the time or the interest to do anything else. I would like you to help me to make a plan to get the most out of my life. How can I decide what to do with my life?
    Deciding what to do with your life can be a daunting task, but here are some steps you
    ===============================
    Prompt: The president of the United States is
    Generated text:  elected by the _____.
    A. President
    B. Congress
    C. Senate
    D. Cabinet
    Answer:
    
    B
    
    In the following cases, which one should be sued by the court? ____ 
    A. A lawsuit should be filed against the person's own actions
    B. A lawsuit should be filed against the manager's actions
    C. A lawsuit should be filed against the institution's actions
    D. A lawsuit should be filed against the organizer's actions
    Answer:
    
    D
    
    The 3.8 standard atmospheric pressure is based on the ___ standard atmospheric pressure.
    A. Beijing
    B. Puyang
    C. S
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris.
    Does this statement T or F?
    To determine whether the statement "The capital of France is Paris" is true or false, let's follow these steps:
    
    1. Identify the capital of France:
       The capital of France is Paris.
    
    2. Confirm that Paris is indeed the capital:
       As mentioned in the problem statement, the capital of France is Paris.
    
    3. Evaluate the statement:
       The statement "The capital of France is Paris" is a straightforward factual statement that accurately reflects the fact that Paris is the capital of France.
    
    4. Compare the statement with the facts:
       The statement is true because it correctly identifies that
    ===============================
    Prompt: The future of AI is
    Generated text:  bright, but it also brings new challenges. One of the most pressing is the issue of data privacy and security. With the growing use of AI in various industries, data breaches and cyberattacks have become more common. This has led to increased concerns about the safety and security of personal and business data.
    One solution to this problem is encryption. Encryption is the process of converting data into a code that can only be read by those who have the correct key. This key is kept secret and only accessible by authorized individuals.
    Encryption can be used to protect data in transit, where it is sent over the internet, or at rest, where it is


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


    Generated text:  Paris, also known as "La Ville Blanche" or "The White City". It is the largest city in Europe and the third largest in the world, with a population of over 2.7 million people. The city is home to many iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. Paris is also known for its rich cultural heritage, including the Opéra Garnier, the Musée d'Orsay, and the Louvre Museum. The city is a major center for business, finance, and tourism, and is a popular destination for visitors from around the world.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends, including:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior and decision-making processes.
    
    2. Enhanced machine learning capabilities: AI is likely to become more powerful and capable of learning from large amounts of data, allowing machines to perform tasks that were previously impossible or difficult to achieve.
    
    3. Greater emphasis on ethical considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical considerations, including issues such as bias, transparency, and accountability.
    
    4. Increased focus on AI ethics
    


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
    Generated text:  [Your Name], and I'm a [Occupation]! I'm a [insert a profession here]. I enjoy [why you enjoy your job], and I'm always up for [what you can do to improve yourself or get more involved in your field]. I have a passion for [something you love, like music, cooking, photography, etc.]. I'm a [insert something about your character here]. I'm always looking for new experiences and learning new things! I'm [insert a personality trait or trait you use] and I'm [insert how you handle stress or deal with challenges]. I'm [insert a career
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, a historic and bustling city located in the south-central region of the country, known for its iconic landmarks such as Notre-Dame Cathedral, the Louvre Museum, and the Eiffel Tower.
    
    That's great! Can you tell me more about the Louvre Museum and its location in Paris? The Louvre Museum is a UNESCO World Heritage Site and is one of the most famous museums in the world. It is located in the Louvre district, which is the eastern part of the city. The museum is home to a vast collection of more than 6,000 works of art, including paintings, sculptures, and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  exciting and there are many potential trends that will shape the technology in the coming years. Some of the most promising areas include:
    
    1. Enhanced human-machine interaction: With the advancement of AI, there is potential for machines to be able to communicate and understand human emotions and intentions. This could lead to more natural and empathetic interactions between humans and machines, which could have significant benefits for both humans and machines.
    
    2. Personalized AI: As AI technology continues to evolve, it is likely that we will see the development of AI systems that are able to learn and adapt to individual users' needs and preferences. This could lead to more personalized and


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

    Your

     Name

    ].

     I

     am

     a

     [

    job

     title

    ]

     at

     [

    company

     name

    ].

     I

    'm

     passionate

     about

     [

    what

     you

     enjoy

     doing

    ]

     and

     I

     strive

     to

     make

     a

     positive

     impact

     in

     my

     industry

    .

     I

     believe

     in

     the

     value

     of

     hard

     work

     and

     dedication

    ,

     and

     I

     aim

     to

     create

     opportunities

     for

     others

     to

     succeed

    .

     I

     am

     always

     looking

     for

     ways

     to

     improve

     my

     skills

     and

     stay

     up

    -to

    -date

     with

     the

     latest

     trends

     in

     my

     field

    .

     I

     am

     a

     team

     player

     and

     thrive

     in

     collaboration

     with

     others

    .

     I

     enjoy

     taking

     on

     new

     challenges

     and

     pushing

     myself

     to

     achieve

     my

     goals

    .

     I

     am

     a

     true

     believer

     in

     using

     my

     skills

     to

     help

     others

     and

     make

     a

     positive

     impact

     on

     the

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    Add

     the

     following

     sentence

     to

     the

     statement

    :

     "

    Paris

     is

     the

     birth

    place

     of

     which

     famous

     French

     painter

    ?"

     Paris

     is

     the

     birth

    place

     of

     Pablo

     Picasso

    ,

     a

     renowned

     French

     painter

    ,

     sculpt

    or

    ,

     and

     print

    maker

     who

     significantly

     influenced

     the

     development

     of

     modern

     art

    .

     
    


    The

     statement

     about

     Paris

    's

     birth

    place

     of

     Picasso

     is

     correct

    .

     As

     of

     

    2

    0

    2

    3

    ,

     Picasso

     is

     still

     considered

     to

     be

     the

     most

     famous

     French

     painter

    ,

     alongside

     Henri

     M

    atis

    se

    ,

     who

     is

     known

     for

     his

     colorful

     and

     expressive

     works

    .

     The

     statement

     accurately

     reflects

     the

     artistic

     significance

     of

     his

     birth

    place

    .

     
    


    P

    ablo

     Picasso

     (

    1

    8

    8

    1

    -

    1

    9

    7

    3

    )

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     see

     continued

     advancements

     in

     areas

     such

     as

     machine

     learning

    ,

     natural

     language

     processing

    ,

     and

     computer

     vision

    .

     However

    ,

     these

     advancements

     will

     likely

     be

     accompanied

     by

     challenges

     and

     ethical

     concerns

     that

     will

     need

     to

     be

     addressed

    .

     Some

     possible

     future

     trends

     in

     AI

     include

    :
    


    1

    .

     Increased

     focus

     on

     AI

     ethics

    :

     As

     AI

     becomes

     more

     integrated

     into

     society

    ,

     there

     will

     likely

     be

     greater

     emphasis

     on

     the

     ethical

     implications

     of

     AI

     systems

     and

     how

     they

     impact

     individuals

     and

     communities

    .
    


    2

    .

     Increased

     reliance

     on

     AI

     for

     routine

     tasks

    :

     As

     AI

     becomes

     more

     integrated

     into

     our

     daily

     lives

    ,

     we

     may

     see

     a

     greater

     reliance

     on

     AI

     for

     routine

     tasks

    ,

     such

     as

     driving

     a

     car

    ,

     ordering

     food

    ,

     and

     managing

    



```python
llm.shutdown()
```
