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


    2026-03-27 23:13:23.417 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:13:23] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 23:13:23.417 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:13:23] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 23:13:23.417 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:13:23] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 23:13:23.417 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:13:23] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 23:13:23.417 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:13:23] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  4.71it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  4.71it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:08,  2.25s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:08,  2.25s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:08,  2.25s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:08,  2.25s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:24,  2.20it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:24,  2.20it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:24,  2.20it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:24,  2.20it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:24,  2.20it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:24,  2.20it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:24,  2.20it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:07,  6.65it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 14.74it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 14.74it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 14.74it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 14.74it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 14.74it/s]

    Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 14.74it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 14.74it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 20.26it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 20.26it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 20.26it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 20.26it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 20.26it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 20.26it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 20.26it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:02<00:01, 20.26it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 27.60it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 27.60it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 27.60it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:00, 27.60it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:00, 27.60it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:02<00:00, 27.60it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:02<00:00, 27.60it/s]Compiling num tokens (num_tokens=224):  55%|█████▌    | 32/58 [00:02<00:00, 27.60it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:02<00:00, 34.55it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:02<00:00, 34.55it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:02<00:00, 34.55it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:02<00:00, 34.55it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:02<00:00, 34.55it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:02<00:00, 34.55it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:02<00:00, 34.55it/s]Compiling num tokens (num_tokens=112):  67%|██████▋   | 39/58 [00:02<00:00, 34.55it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:03<00:00, 40.87it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:03<00:00, 40.87it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:03<00:00, 40.87it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:03<00:00, 40.87it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:03<00:00, 40.87it/s]

    Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:03<00:00, 40.87it/s]Compiling num tokens (num_tokens=28):  79%|███████▉  | 46/58 [00:03<00:00, 40.87it/s]Compiling num tokens (num_tokens=24):  79%|███████▉  | 46/58 [00:03<00:00, 40.87it/s]Compiling num tokens (num_tokens=20):  79%|███████▉  | 46/58 [00:03<00:00, 40.87it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:03<00:00, 48.53it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:03<00:00, 48.53it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:03<00:00, 48.53it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:03<00:00, 48.53it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:03<00:00, 48.53it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 18.41it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.80 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.77 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.77 GB):   3%|▎         | 2/58 [00:00<00:05, 10.75it/s]Capturing num tokens (num_tokens=7168 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:05, 10.75it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:05, 10.75it/s]Capturing num tokens (num_tokens=6656 avail_mem=76.76 GB):   7%|▋         | 4/58 [00:00<00:06,  8.72it/s]Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   7%|▋         | 4/58 [00:00<00:06,  8.72it/s]Capturing num tokens (num_tokens=5632 avail_mem=76.76 GB):   7%|▋         | 4/58 [00:00<00:06,  8.72it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=76.76 GB):   7%|▋         | 4/58 [00:00<00:06,  8.72it/s]Capturing num tokens (num_tokens=5120 avail_mem=76.76 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.18it/s]Capturing num tokens (num_tokens=4608 avail_mem=76.76 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.18it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.76 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.18it/s]Capturing num tokens (num_tokens=3840 avail_mem=76.75 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.18it/s]Capturing num tokens (num_tokens=3840 avail_mem=76.75 GB):  17%|█▋        | 10/58 [00:00<00:02, 16.89it/s]Capturing num tokens (num_tokens=3584 avail_mem=76.75 GB):  17%|█▋        | 10/58 [00:00<00:02, 16.89it/s]Capturing num tokens (num_tokens=3328 avail_mem=76.74 GB):  17%|█▋        | 10/58 [00:00<00:02, 16.89it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=76.74 GB):  17%|█▋        | 10/58 [00:00<00:02, 16.89it/s]Capturing num tokens (num_tokens=3072 avail_mem=76.74 GB):  22%|██▏       | 13/58 [00:00<00:02, 19.70it/s]Capturing num tokens (num_tokens=2816 avail_mem=76.74 GB):  22%|██▏       | 13/58 [00:00<00:02, 19.70it/s]Capturing num tokens (num_tokens=2560 avail_mem=76.73 GB):  22%|██▏       | 13/58 [00:00<00:02, 19.70it/s]Capturing num tokens (num_tokens=2304 avail_mem=76.73 GB):  22%|██▏       | 13/58 [00:00<00:02, 19.70it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.73 GB):  22%|██▏       | 13/58 [00:00<00:02, 19.70it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.73 GB):  29%|██▉       | 17/58 [00:00<00:01, 23.91it/s]Capturing num tokens (num_tokens=1792 avail_mem=76.72 GB):  29%|██▉       | 17/58 [00:00<00:01, 23.91it/s]Capturing num tokens (num_tokens=1536 avail_mem=76.72 GB):  29%|██▉       | 17/58 [00:00<00:01, 23.91it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=76.72 GB):  29%|██▉       | 17/58 [00:00<00:01, 23.91it/s]Capturing num tokens (num_tokens=1024 avail_mem=76.70 GB):  29%|██▉       | 17/58 [00:00<00:01, 23.91it/s]Capturing num tokens (num_tokens=1024 avail_mem=76.70 GB):  36%|███▌      | 21/58 [00:01<00:01, 27.37it/s]Capturing num tokens (num_tokens=960 avail_mem=76.71 GB):  36%|███▌      | 21/58 [00:01<00:01, 27.37it/s] Capturing num tokens (num_tokens=896 avail_mem=76.71 GB):  36%|███▌      | 21/58 [00:01<00:01, 27.37it/s]Capturing num tokens (num_tokens=832 avail_mem=76.70 GB):  36%|███▌      | 21/58 [00:01<00:01, 27.37it/s]Capturing num tokens (num_tokens=768 avail_mem=76.70 GB):  36%|███▌      | 21/58 [00:01<00:01, 27.37it/s]Capturing num tokens (num_tokens=704 avail_mem=76.70 GB):  36%|███▌      | 21/58 [00:01<00:01, 27.37it/s]Capturing num tokens (num_tokens=704 avail_mem=76.70 GB):  45%|████▍     | 26/58 [00:01<00:00, 33.04it/s]Capturing num tokens (num_tokens=640 avail_mem=76.69 GB):  45%|████▍     | 26/58 [00:01<00:00, 33.04it/s]Capturing num tokens (num_tokens=576 avail_mem=76.69 GB):  45%|████▍     | 26/58 [00:01<00:00, 33.04it/s]

    Capturing num tokens (num_tokens=512 avail_mem=76.68 GB):  45%|████▍     | 26/58 [00:01<00:00, 33.04it/s]Capturing num tokens (num_tokens=480 avail_mem=76.70 GB):  45%|████▍     | 26/58 [00:01<00:00, 33.04it/s]Capturing num tokens (num_tokens=448 avail_mem=76.70 GB):  45%|████▍     | 26/58 [00:01<00:00, 33.04it/s]Capturing num tokens (num_tokens=448 avail_mem=76.70 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.63it/s]Capturing num tokens (num_tokens=416 avail_mem=76.69 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.63it/s]Capturing num tokens (num_tokens=384 avail_mem=76.69 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.63it/s]Capturing num tokens (num_tokens=352 avail_mem=76.69 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.63it/s]Capturing num tokens (num_tokens=320 avail_mem=76.68 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.63it/s]Capturing num tokens (num_tokens=288 avail_mem=76.68 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.63it/s]Capturing num tokens (num_tokens=288 avail_mem=76.68 GB):  62%|██████▏   | 36/58 [00:01<00:00, 38.00it/s]Capturing num tokens (num_tokens=256 avail_mem=76.68 GB):  62%|██████▏   | 36/58 [00:01<00:00, 38.00it/s]

    Capturing num tokens (num_tokens=240 avail_mem=76.68 GB):  62%|██████▏   | 36/58 [00:01<00:00, 38.00it/s]Capturing num tokens (num_tokens=224 avail_mem=76.67 GB):  62%|██████▏   | 36/58 [00:01<00:00, 38.00it/s]Capturing num tokens (num_tokens=208 avail_mem=76.67 GB):  62%|██████▏   | 36/58 [00:01<00:00, 38.00it/s]Capturing num tokens (num_tokens=208 avail_mem=76.67 GB):  69%|██████▉   | 40/58 [00:01<00:00, 36.88it/s]Capturing num tokens (num_tokens=192 avail_mem=76.67 GB):  69%|██████▉   | 40/58 [00:01<00:00, 36.88it/s]Capturing num tokens (num_tokens=176 avail_mem=76.66 GB):  69%|██████▉   | 40/58 [00:01<00:00, 36.88it/s]Capturing num tokens (num_tokens=160 avail_mem=76.66 GB):  69%|██████▉   | 40/58 [00:01<00:00, 36.88it/s]Capturing num tokens (num_tokens=144 avail_mem=76.66 GB):  69%|██████▉   | 40/58 [00:01<00:00, 36.88it/s]

    Capturing num tokens (num_tokens=144 avail_mem=76.66 GB):  76%|███████▌  | 44/58 [00:01<00:00, 29.80it/s]Capturing num tokens (num_tokens=128 avail_mem=76.65 GB):  76%|███████▌  | 44/58 [00:01<00:00, 29.80it/s]Capturing num tokens (num_tokens=112 avail_mem=76.65 GB):  76%|███████▌  | 44/58 [00:01<00:00, 29.80it/s]Capturing num tokens (num_tokens=96 avail_mem=76.65 GB):  76%|███████▌  | 44/58 [00:01<00:00, 29.80it/s] Capturing num tokens (num_tokens=80 avail_mem=76.64 GB):  76%|███████▌  | 44/58 [00:01<00:00, 29.80it/s]

    Capturing num tokens (num_tokens=80 avail_mem=76.64 GB):  83%|████████▎ | 48/58 [00:01<00:00, 24.76it/s]Capturing num tokens (num_tokens=64 avail_mem=76.64 GB):  83%|████████▎ | 48/58 [00:01<00:00, 24.76it/s]Capturing num tokens (num_tokens=48 avail_mem=76.64 GB):  83%|████████▎ | 48/58 [00:01<00:00, 24.76it/s]Capturing num tokens (num_tokens=32 avail_mem=76.64 GB):  83%|████████▎ | 48/58 [00:01<00:00, 24.76it/s]Capturing num tokens (num_tokens=28 avail_mem=76.63 GB):  83%|████████▎ | 48/58 [00:01<00:00, 24.76it/s]Capturing num tokens (num_tokens=24 avail_mem=76.63 GB):  83%|████████▎ | 48/58 [00:02<00:00, 24.76it/s]Capturing num tokens (num_tokens=24 avail_mem=76.63 GB):  91%|█████████▏| 53/58 [00:02<00:00, 29.56it/s]Capturing num tokens (num_tokens=20 avail_mem=76.62 GB):  91%|█████████▏| 53/58 [00:02<00:00, 29.56it/s]Capturing num tokens (num_tokens=16 avail_mem=76.62 GB):  91%|█████████▏| 53/58 [00:02<00:00, 29.56it/s]Capturing num tokens (num_tokens=12 avail_mem=76.62 GB):  91%|█████████▏| 53/58 [00:02<00:00, 29.56it/s]Capturing num tokens (num_tokens=8 avail_mem=76.61 GB):  91%|█████████▏| 53/58 [00:02<00:00, 29.56it/s] Capturing num tokens (num_tokens=4 avail_mem=76.61 GB):  91%|█████████▏| 53/58 [00:02<00:00, 29.56it/s]

    Capturing num tokens (num_tokens=4 avail_mem=76.61 GB): 100%|██████████| 58/58 [00:02<00:00, 33.73it/s]Capturing num tokens (num_tokens=4 avail_mem=76.61 GB): 100%|██████████| 58/58 [00:02<00:00, 27.26it/s]


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
    Generated text:  Sam and I am a woman. How can I use this platform to help you? I am an active and enthusiastic young woman looking to learn and grow. I am passionate about learning and have a strong interest in technology and innovation. I love exploring new ideas and always want to improve myself. I am looking forward to learning more about the platform and how it can help me achieve my goals. How can I use this platform to help you?
    Hello! I'm excited to help you learn more about the platform. What can I do for you today? What topics are you interested in learning about? Can you provide me with more information about the
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to become more environmentally conscious. To do so, he has decided to purchase new technology. The president is considering purchasing a new energy-efficient refrigerator to replace the old one. He has been advised to take the following steps:
    
    1. He should choose the best technology for his needs.
    2. The technology should have the lowest initial cost.
    3. The technology should offer the highest expected return on investment.
    
    Assuming that the cost of replacing the old refrigerator is $12,000, the annual operating cost is $3,000, the new technology has a life expectancy of 5 years, and it will save
    ===============================
    Prompt: The capital of France is
    Generated text:  ______.
    A. Lille
    B. Paris
    C. Dijon
    D. Lyon
    Answer:
    B
    
    Regarding the structure of a standard ASCII table, which of the following statements is correct? A. The first 10 bits in the table correspond to the decimal numbers 0 to 9. B. The first 10 bits in the table correspond to the decimal numbers 0 to 31. C. The first 10 bits in the table correspond to the decimal numbers 32 to 61. D. The first 10 bits in the table correspond to the decimal numbers 
    ===============================
    Prompt: The future of AI is
    Generated text:  in the hands of the people
    
    The future of AI is in the hands of the people
    
    The future of AI is in the hands of the people.
    
    AI is a rapidly evolving field that is rapidly changing. We are now in the time of a revolution, as a number of new technologies are emerging and taking on new forms, with the technology being so advanced that it is impossible to predict the future. This is a time when the stakes are high as new technologies are being developed and there is a risk of one way of life being replaced by another. The tech industry is increasingly concerned with the need for individuals to be responsible for the impact


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, which is known for its iconic Eiffel Tower and the Louvre Museum. It is also home to the Louvre Museum, the most famous art museum in the world, and the Notre-Dame Cathedral. Paris is a bustling city with a rich history and culture, and it is a popular tourist destination. It is also home to the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. The city is known for its beautiful architecture, vibrant nightlife, and delicious cuisine. Paris is a city that is a true melting pot of cultures and is a must-visit destination for anyone interested in France.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased automation and robotics: As AI continues to advance, we are likely to see more automation and robotics in various industries, including manufacturing, transportation, and healthcare. This will lead to increased efficiency, cost savings, and job displacement, but it will also create new opportunities for AI developers and entrepreneurs.
    
    2. AI in healthcare: AI is already being used in healthcare to improve patient outcomes, reduce costs, and increase efficiency. As AI continues to advance, we are likely to
    


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
    Generated text:  [name], and I am a [position] at [organization]. I am a [occupation] who has always been passionate about [what you do for a living]. I have always been driven by my desire to [reason why you love your work], and I am always seeking to [what you do in your free time]. I am always trying to [reason why you are better than everyone else], and I am always seeking to [what you do in your free time]. So, if you have any questions or need any information about me, please don't hesitate to reach out. Your time with me is valuable to me, and
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, located on the banks of the Seine River in the region of Paris, and is the largest city in the country by population. It is also one of the most important cities in Europe and is known for its medieval architecture, romantic landscapes, and rich cultural heritage. The city is also famous for its iconic landmarks such as the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Paris Opera. 
    
    Paris is the capital of France and is also the capital of France. The city is known for its medieval architecture and romantic landscape. The city is also famous for its iconic landmarks such as the Eiffel
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  highly uncertain, but here are a few potential trends that could emerge in the coming years:
    
    1. Personalized AI: As we continue to collect and analyze large amounts of data, we may see a rise in personalized AI that can adapt to individual needs and preferences. This could lead to more efficient and effective use of resources, as well as more accurate and personalized treatments for a wide range of conditions.
    
    2. Ethical AI: As AI becomes more integrated into our daily lives, there will be an increasing need for ethical guidelines and standards to ensure that AI systems are used in a responsible and transparent manner. This could lead to the development of


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

    ].

     I

    'm

     a

     [

    title

    ]

     at

     [

    company

     name

    ].

     My

     interests

     and

     skills

     include

     [

    insert

     specific

     skills

     or

     hobbies

    ],

     and

     I

     enjoy

     [

    insert

     something

     related

     to

     my

     career

     or

     hobbies

    ].

     I

    'm

     always

     looking

     for

     opportunities

     to

     grow

     and

     learn

    ,

     and

     I

    'm

     passionate

     about

     [

    insert

     something

     related

     to

     my

     career

     or

     hobbies

    ].

     I

     believe

     in

     [

    insert

     something

     related

     to

     my

     career

     or

     hobbies

    ],

     and

     I

    'm

     excited

     to

     make

     a

     positive

     impact

     on

     the

     world

    .

     
    


    [

    Note

    :

     Feel

     free

     to

     add

     any

     personal

     anecdotes

     or

     experiences

     that

     you

     might

     want

     to

     share

     with

     the

     reader

    .]

     
    


    [

    Your

     Name

    ]

     [

    Your

     Title

    ]

     at

     [

    Your

     Company

     Name

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     commonly

     referred

     to

     as

     "

    Paris

    "

     in

     English

    .

     The

     city

     is

     a

     major

     political

    ,

     cultural

    ,

     and

     economic

     center

     of

     France

     and

     its

     largest

     metropolitan

     area

    .

     It

     is

     known

     for

     its

     historic

     landmarks

    ,

     such

     as

     the

     E

    iff

    el

     Tower

    ,

     Notre

     Dame

     Cathedral

    ,

     and

     Lou

    vre

     Museum

    ,

     as

     well

     as

     its

     charming

     neighborhoods

     and

     vibrant

     nightlife

    .

     Paris

     is

     also

     known

     for

     its

     fashion

     industry

    ,

     art

    ,

     and

     cuisine

    .

     The

     city

     has

     a

     rich

     history

     dating

     back

     to

     the

     Roman

     Empire

     and

     is

     home

     to

     many

     cultural

     and

     historical

     sites

    .

     It

     is

     a

     major

     tourist

     destination

     and

     is

     the

     most

     populous

     city

     in

     France

    .

     Paris

     is

     known

     for

     its

     beautiful

     architecture

    ,

     vibrant

     nightlife

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     be

     characterized

     by

     continuous

     innovation

    ,

     broad

     adoption

    ,

     and

     increased

     integration

     with

     other

     technologies

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

     already

     integrated

     into

     numerous

     systems

    ,

     but

     it

     is

     likely

     to

     become

     even

     more

     integrated

     in

     the

     coming

     years

    .

     This

     will

     likely

     include

     integration

     with

     sensors

    ,

     cameras

    ,

     drones

    ,

     and

     other

     hardware

    .

     AI

     will

     also

     likely

     be

     used

     in

     healthcare

    ,

     finance

    ,

     and

     transportation

    .
    


    2

    .

     More

     Personal

    ized

     AI

    :

     AI

     will

     become

     more

     personalized

    ,

     taking

     into

     account

     a

     wider

     range

     of

     data

     sources

     and

     users

    '

     preferences

    .

     This

     will

     likely

     result

     in

     more

     intelligent

     and

     tailored

     solutions

     for

     users

    .
    


    3

    



```python
llm.shutdown()
```
