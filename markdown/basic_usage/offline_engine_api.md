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


    2026-04-01 21:29:35.285 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 21:29:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 21:29:35.285 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 21:29:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 21:29:35.285 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 21:29:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 21:29:35.285 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 21:29:35] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 21:29:35.285 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 21:29:35] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.21it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.20it/s]


    2026-04-01 21:29:38,376 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 21:29:38] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.84it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:02<00:09,  5.04it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:02<00:09,  5.04it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:02<00:09,  5.04it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:02<00:09,  5.04it/s]Compiling num tokens (num_tokens=3072):  16%|█▌        | 9/58 [00:02<00:09,  5.04it/s]Compiling num tokens (num_tokens=2816):  16%|█▌        | 9/58 [00:02<00:09,  5.04it/s]Compiling num tokens (num_tokens=2560):  16%|█▌        | 9/58 [00:02<00:09,  5.04it/s]Compiling num tokens (num_tokens=2304):  16%|█▌        | 9/58 [00:03<00:09,  5.04it/s]Compiling num tokens (num_tokens=2048):  16%|█▌        | 9/58 [00:03<00:09,  5.04it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:03<00:03, 11.50it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:03<00:03, 11.50it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:03<00:03, 11.50it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:03<00:03, 11.50it/s]Compiling num tokens (num_tokens=1024):  29%|██▉       | 17/58 [00:03<00:03, 11.50it/s]Compiling num tokens (num_tokens=960):  29%|██▉       | 17/58 [00:03<00:03, 11.50it/s] Compiling num tokens (num_tokens=896):  29%|██▉       | 17/58 [00:03<00:03, 11.50it/s]Compiling num tokens (num_tokens=832):  29%|██▉       | 17/58 [00:03<00:03, 11.50it/s]

    Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:03<00:01, 17.89it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:03<00:01, 17.89it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:03<00:01, 17.89it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:03<00:01, 17.89it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:03<00:01, 17.89it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:03<00:01, 17.89it/s]Compiling num tokens (num_tokens=480):  41%|████▏     | 24/58 [00:03<00:01, 17.89it/s]Compiling num tokens (num_tokens=448):  41%|████▏     | 24/58 [00:03<00:01, 17.89it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:03<00:01, 24.75it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:03<00:01, 24.75it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:03<00:01, 24.75it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:03<00:01, 24.75it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:03<00:01, 24.75it/s]Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:03<00:01, 24.75it/s]Compiling num tokens (num_tokens=256):  53%|█████▎    | 31/58 [00:03<00:01, 24.75it/s]Compiling num tokens (num_tokens=240):  53%|█████▎    | 31/58 [00:03<00:01, 24.75it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 31.68it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 31.68it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 31.68it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 31.68it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 31.68it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 31.68it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 31.68it/s]Compiling num tokens (num_tokens=128):  66%|██████▌   | 38/58 [00:03<00:00, 31.68it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 38.39it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 38.39it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 38.39it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 38.39it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 38.39it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 38.39it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 38.39it/s]Compiling num tokens (num_tokens=28):  78%|███████▊  | 45/58 [00:03<00:00, 38.39it/s]

    Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:03<00:00, 44.72it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:03<00:00, 44.72it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:03<00:00, 44.72it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:03<00:00, 44.72it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:03<00:00, 44.72it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:03<00:00, 44.72it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:03<00:00, 44.72it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.08it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=71.94 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.91 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.91 GB):   3%|▎         | 2/58 [00:00<00:03, 15.91it/s]Capturing num tokens (num_tokens=7168 avail_mem=71.19 GB):   3%|▎         | 2/58 [00:00<00:03, 15.91it/s]Capturing num tokens (num_tokens=6656 avail_mem=71.09 GB):   3%|▎         | 2/58 [00:00<00:03, 15.91it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=71.09 GB):   7%|▋         | 4/58 [00:00<00:03, 17.67it/s]Capturing num tokens (num_tokens=6144 avail_mem=71.09 GB):   7%|▋         | 4/58 [00:00<00:03, 17.67it/s]Capturing num tokens (num_tokens=5632 avail_mem=71.08 GB):   7%|▋         | 4/58 [00:00<00:03, 17.67it/s]Capturing num tokens (num_tokens=5120 avail_mem=71.09 GB):   7%|▋         | 4/58 [00:00<00:03, 17.67it/s]Capturing num tokens (num_tokens=5120 avail_mem=71.09 GB):  12%|█▏        | 7/58 [00:00<00:02, 20.13it/s]Capturing num tokens (num_tokens=4608 avail_mem=71.08 GB):  12%|█▏        | 7/58 [00:00<00:02, 20.13it/s]Capturing num tokens (num_tokens=4096 avail_mem=71.08 GB):  12%|█▏        | 7/58 [00:00<00:02, 20.13it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=71.08 GB):  12%|█▏        | 7/58 [00:00<00:02, 20.13it/s]Capturing num tokens (num_tokens=3840 avail_mem=71.08 GB):  17%|█▋        | 10/58 [00:00<00:02, 22.38it/s]Capturing num tokens (num_tokens=3584 avail_mem=71.07 GB):  17%|█▋        | 10/58 [00:00<00:02, 22.38it/s]Capturing num tokens (num_tokens=3328 avail_mem=71.07 GB):  17%|█▋        | 10/58 [00:00<00:02, 22.38it/s]Capturing num tokens (num_tokens=3072 avail_mem=71.07 GB):  17%|█▋        | 10/58 [00:00<00:02, 22.38it/s]Capturing num tokens (num_tokens=2816 avail_mem=71.07 GB):  17%|█▋        | 10/58 [00:00<00:02, 22.38it/s]Capturing num tokens (num_tokens=2816 avail_mem=71.07 GB):  24%|██▍       | 14/58 [00:00<00:01, 27.23it/s]Capturing num tokens (num_tokens=2560 avail_mem=71.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 27.23it/s]Capturing num tokens (num_tokens=2304 avail_mem=71.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 27.23it/s]Capturing num tokens (num_tokens=2048 avail_mem=71.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 27.23it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=71.05 GB):  24%|██▍       | 14/58 [00:00<00:01, 27.23it/s]Capturing num tokens (num_tokens=1536 avail_mem=70.78 GB):  24%|██▍       | 14/58 [00:00<00:01, 27.23it/s]Capturing num tokens (num_tokens=1536 avail_mem=70.78 GB):  33%|███▎      | 19/58 [00:00<00:01, 32.38it/s]Capturing num tokens (num_tokens=1280 avail_mem=70.77 GB):  33%|███▎      | 19/58 [00:00<00:01, 32.38it/s]Capturing num tokens (num_tokens=1024 avail_mem=70.75 GB):  33%|███▎      | 19/58 [00:00<00:01, 32.38it/s]Capturing num tokens (num_tokens=960 avail_mem=70.21 GB):  33%|███▎      | 19/58 [00:00<00:01, 32.38it/s] Capturing num tokens (num_tokens=896 avail_mem=70.06 GB):  33%|███▎      | 19/58 [00:00<00:01, 32.38it/s]Capturing num tokens (num_tokens=896 avail_mem=70.06 GB):  40%|███▉      | 23/58 [00:00<00:01, 33.42it/s]Capturing num tokens (num_tokens=832 avail_mem=70.06 GB):  40%|███▉      | 23/58 [00:00<00:01, 33.42it/s]Capturing num tokens (num_tokens=768 avail_mem=70.05 GB):  40%|███▉      | 23/58 [00:00<00:01, 33.42it/s]

    Capturing num tokens (num_tokens=704 avail_mem=70.05 GB):  40%|███▉      | 23/58 [00:00<00:01, 33.42it/s]Capturing num tokens (num_tokens=640 avail_mem=70.05 GB):  40%|███▉      | 23/58 [00:00<00:01, 33.42it/s]Capturing num tokens (num_tokens=640 avail_mem=70.05 GB):  47%|████▋     | 27/58 [00:00<00:00, 33.66it/s]Capturing num tokens (num_tokens=576 avail_mem=70.05 GB):  47%|████▋     | 27/58 [00:00<00:00, 33.66it/s]Capturing num tokens (num_tokens=512 avail_mem=70.03 GB):  47%|████▋     | 27/58 [00:00<00:00, 33.66it/s]Capturing num tokens (num_tokens=480 avail_mem=70.05 GB):  47%|████▋     | 27/58 [00:00<00:00, 33.66it/s]Capturing num tokens (num_tokens=448 avail_mem=70.05 GB):  47%|████▋     | 27/58 [00:01<00:00, 33.66it/s]Capturing num tokens (num_tokens=448 avail_mem=70.05 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.85it/s]Capturing num tokens (num_tokens=416 avail_mem=70.04 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.85it/s]

    Capturing num tokens (num_tokens=384 avail_mem=70.04 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.85it/s]Capturing num tokens (num_tokens=352 avail_mem=70.04 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.85it/s]Capturing num tokens (num_tokens=320 avail_mem=70.03 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.85it/s]Capturing num tokens (num_tokens=320 avail_mem=70.03 GB):  60%|██████    | 35/58 [00:01<00:00, 33.76it/s]Capturing num tokens (num_tokens=288 avail_mem=70.03 GB):  60%|██████    | 35/58 [00:01<00:00, 33.76it/s]Capturing num tokens (num_tokens=256 avail_mem=70.03 GB):  60%|██████    | 35/58 [00:01<00:00, 33.76it/s]Capturing num tokens (num_tokens=240 avail_mem=70.02 GB):  60%|██████    | 35/58 [00:01<00:00, 33.76it/s]Capturing num tokens (num_tokens=224 avail_mem=70.02 GB):  60%|██████    | 35/58 [00:01<00:00, 33.76it/s]

    Capturing num tokens (num_tokens=224 avail_mem=70.02 GB):  67%|██████▋   | 39/58 [00:01<00:00, 33.94it/s]Capturing num tokens (num_tokens=208 avail_mem=69.69 GB):  67%|██████▋   | 39/58 [00:01<00:00, 33.94it/s]Capturing num tokens (num_tokens=192 avail_mem=69.59 GB):  67%|██████▋   | 39/58 [00:01<00:00, 33.94it/s]Capturing num tokens (num_tokens=176 avail_mem=69.59 GB):  67%|██████▋   | 39/58 [00:01<00:00, 33.94it/s]Capturing num tokens (num_tokens=160 avail_mem=69.58 GB):  67%|██████▋   | 39/58 [00:01<00:00, 33.94it/s]Capturing num tokens (num_tokens=160 avail_mem=69.58 GB):  74%|███████▍  | 43/58 [00:01<00:00, 33.91it/s]Capturing num tokens (num_tokens=144 avail_mem=69.58 GB):  74%|███████▍  | 43/58 [00:01<00:00, 33.91it/s]Capturing num tokens (num_tokens=128 avail_mem=69.58 GB):  74%|███████▍  | 43/58 [00:01<00:00, 33.91it/s]Capturing num tokens (num_tokens=112 avail_mem=69.57 GB):  74%|███████▍  | 43/58 [00:01<00:00, 33.91it/s]

    Capturing num tokens (num_tokens=96 avail_mem=69.57 GB):  74%|███████▍  | 43/58 [00:01<00:00, 33.91it/s] Capturing num tokens (num_tokens=96 avail_mem=69.57 GB):  81%|████████  | 47/58 [00:01<00:00, 33.15it/s]Capturing num tokens (num_tokens=80 avail_mem=69.57 GB):  81%|████████  | 47/58 [00:01<00:00, 33.15it/s]Capturing num tokens (num_tokens=64 avail_mem=69.56 GB):  81%|████████  | 47/58 [00:01<00:00, 33.15it/s]Capturing num tokens (num_tokens=48 avail_mem=69.56 GB):  81%|████████  | 47/58 [00:01<00:00, 33.15it/s]Capturing num tokens (num_tokens=32 avail_mem=69.56 GB):  81%|████████  | 47/58 [00:01<00:00, 33.15it/s]Capturing num tokens (num_tokens=32 avail_mem=69.56 GB):  88%|████████▊ | 51/58 [00:01<00:00, 34.82it/s]Capturing num tokens (num_tokens=28 avail_mem=69.55 GB):  88%|████████▊ | 51/58 [00:01<00:00, 34.82it/s]Capturing num tokens (num_tokens=24 avail_mem=69.55 GB):  88%|████████▊ | 51/58 [00:01<00:00, 34.82it/s]Capturing num tokens (num_tokens=20 avail_mem=69.54 GB):  88%|████████▊ | 51/58 [00:01<00:00, 34.82it/s]Capturing num tokens (num_tokens=16 avail_mem=69.54 GB):  88%|████████▊ | 51/58 [00:01<00:00, 34.82it/s]

    Capturing num tokens (num_tokens=12 avail_mem=69.54 GB):  88%|████████▊ | 51/58 [00:01<00:00, 34.82it/s]Capturing num tokens (num_tokens=8 avail_mem=69.54 GB):  88%|████████▊ | 51/58 [00:01<00:00, 34.82it/s] Capturing num tokens (num_tokens=8 avail_mem=69.54 GB):  98%|█████████▊| 57/58 [00:01<00:00, 40.20it/s]Capturing num tokens (num_tokens=4 avail_mem=69.53 GB):  98%|█████████▊| 57/58 [00:01<00:00, 40.20it/s]Capturing num tokens (num_tokens=4 avail_mem=69.53 GB): 100%|██████████| 58/58 [00:01<00:00, 32.82it/s]


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
    Generated text:  Kana and I am a 4th-year medical student at the University of California, Davis. I am from a small town in California where I have always been interested in medicine, and I am always eager to learn new things. My background in medical school is about to start, and I am excited about the opportunity to learn about different diseases and conditions that are prevalent in our country. If you have any questions or concerns about my medical school experience, I would be more than happy to answer them. If you would like to find out more about me, I can tell you more about my background and medical studies.
    Sure, I would
    ===============================
    Prompt: The president of the United States is
    Generated text:  a very important person. He is the leader of the country. The president usually takes care of all the important decisions in the country. The president has the power to help his country. The president also has to take care of the country. He is the leader of the country. He is like a king. The president has the power to make up all the important decisions in the country. The president has to take care of the country, he has to make decisions for the country. He is like a king. The president has the power to help the country. The president also has to take care of the country. He is the leader
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. The name of Paris is derived from the French word for “city.” Originally, the word “Paris” meant the “city of the five hundred.” It was in 1589 that the first building was built in the French Quarter area of Paris that is now the center of the city. Originally, the name of the city was Vieux Parthenay.
    The French, who occupied the area in the 17th and 18th centuries, called the area the “city of the four hundred.” In 1778, the name of the city was officially changed to Paris. The 1
    ===============================
    Prompt: The future of AI is
    Generated text:  not in the language, but in the people. The most important people who will enable the new AI revolution.
    The Turing Test is the benchmark test for artificial intelligence. It was established by Alan Turing in 1950. It was used to determine if a computer could understand human language.
    In 1966, the Turing Test was used in a famous experiment in which two people were able to converse, with the participant more fluent in the second language than the first. This is the most fundamental test of artificial intelligence.
    The Turing Test is used to determine if AI can understand human language. AI is more advanced than human language


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


    Generated text:  Paris, also known as "La Ville de Paris". It is the largest city in France and the second-largest city in the European Union. Paris is a cultural and historical center, known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Arc de Triomphe. It is also a major financial center and home to many of France's major museums, including the Louvre and the Musée d'Orsay. Paris is a popular tourist destination and a major center for business and commerce. It is also home to many of France's major political and cultural institutions, including the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: As AI becomes more advanced, it is likely to become more integrated with human intelligence, allowing it to learn and adapt to new situations. This could lead to more sophisticated and adaptive AI systems that can perform tasks that are currently beyond the capabilities of humans.
    
    2. Greater emphasis on ethical considerations: As AI becomes more advanced, there will be a greater emphasis on ethical considerations, including issues such as bias, transparency, and accountability. This will require developers to take a more responsible approach to AI development, and to ensure that their systems are designed to be fair and
    


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
    Generated text:  [Name] and I am an aspiring [occupation]! 🌟✨✨ I am [Number] years old, and I'm a [X] male, [Y] female, or [Z] other gender. I'm passionate about [X] and have a strong work ethic. I am always looking to grow and learn, and I'm eager to share my experiences with others. Whether it's [X] or [Y] or [Z], I'm excited to contribute to any cause I care about! 🌟✨✨✨✨✨✨✨✨✨✨✨✨✨✨✨✨✨✨✨
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. It is the largest city in Europe by population and the seat of the government of France. Paris is known for its rich history, beautiful architecture, and lively cultural scene. It is also a cosmopolitan city, with a diverse population of people from various cultures and backgrounds. Paris is a popular tourist destination, with millions of visitors annually. The city is home to many famous landmarks, including the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. Paris is a UNESCO World Heritage site and a major tourist destination, attracting millions of visitors each year. It is an important cultural, economic, and political center
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by rapid advancement and integration of new technologies and algorithms. Some possible trends in AI include:
    
    1. Increased AI capabilities: The ability to create more complex and sophisticated AI models will likely continue to improve in terms of accuracy, efficiency and scalability. AI systems will become even more capable of understanding natural language, visual perception, and other forms of human intelligence.
    
    2. Natural language processing: AI systems will be able to understand and respond to natural language input from humans, leading to the development of more conversational and self-learning AI agents.
    
    3. Autonomous vehicles: Autonomous vehicles will become more prevalent in the future, with AI systems


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

     name

    ]

     and

     I

     am

     a

     friendly

     and

     enthusiastic

     character

     who

     enjoys

     reading

    ,

     writing

    ,

     and

     traveling

    .

     I

     have

     been

     traveling

     and

     learning

     new

     things

     for

     years

    ,

     and

     I

     am

     always

     up

     for

     a

     new

     adventure

    .

     I

     love

     playing

     games

     and

     keeping

     up

     with

     the

     latest

     tech

     trends

    .

     My

     favorite

     hobbies

     are

     creating

     art

     and

     reading

     books

    .

     I

     am

     always

     on

     the

     go

    ,

     ready

     to

     meet

     new

     people

     and

     make

     new

     friends

    .

     I

     love

     to

     eat

     pizza

     and

     have

     a

     good

     time

     with

     my

     family

     and

     friends

    .

     I

     am

     confident

     and

     proud

     of

     who

     I

     am

     and

     what

     I

     believe

     in

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

     I

     look

     forward

     to

    
    
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

     the

     European

     Union

     and

     one

     of

     the

     most

     important

     cities

     in

     the

     world

    .

     The

     city

     is

     also

     a

     major

     economic

     and

     cultural

     hub

    ,

     with

     many

     famous

     landmarks

     such

     as

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

     Notre

    -D

    ame

     Cathedral

    ,

     and

     the

     Palace

     of

     Vers

    ailles

    .

     It

     is

     also

     a

     major

     tourist

     destination

     and

     has

     a

     rich

     history

     dating

     back

     over

     

    2

    ,

    0

    0

    0

     years

    .

     
    


    Paris

     is

     the

     official

     residence

     of

     the

     President

     of

     the

     French

     Republic

    ,

     the

     French

     Prime

     Minister

    ,

     and

     the

     Mayor

     of

     Paris

    .

     It

     has

     a

     population

     of

     around

     

    2

    .

    7

     million

     people

    ,

     making

     it

     the

     most

     populous

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     a

     mix

     of

     incredible

     and

     transformative

     innovations

    ,

     along

     with

     a

     range

     of

     challenges

     and

     risks

    .

     Here

     are

     some

     possible

     future

     trends

     in

     artificial

     intelligence

    :
    


    1

    .

     Increased

     human

     involvement

     in

     AI

    :

     As

     AI

     becomes

     more

     sophisticated

     and

     complex

    ,

     we

     may

     see

     a

     more

     significant

     role

     for

     humans

     in

     the

     development

     and

     deployment

     of

     AI

     systems

    .

     This

     could

     include

     more

     direct

     involvement

     in

     the

     design

     and

     implementation

     of

     AI

     systems

    ,

     as

     well

     as

     in

     the

     monitoring

     and

     evaluation

     of

     AI

     systems

    '

     performance

    .
    


    2

    .

     AI

    -driven

     healthcare

    :

     As

     AI

     becomes

     more

     advanced

    ,

     we

     may

     see

     a

     wide

     range

     of

     applications

     in

     the

     healthcare

     industry

    ,

     from

     personalized

     medicine

     to

     the

     diagnosis

     and

     treatment

     of

     chronic

    



```python
llm.shutdown()
```
