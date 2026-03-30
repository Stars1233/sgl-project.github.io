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


    2026-03-30 08:51:07.698 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:51:07] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:51:07.698 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:51:07] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:51:07.698 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:51:07] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:51:07.698 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:51:07] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:51:07.698 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:51:07] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.31it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.31it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:13,  2.34s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:13,  2.34s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:13,  2.34s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:13,  2.34s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.43it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.43it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.43it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.43it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.43it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.43it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.43it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.43it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.43it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:02, 13.47it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:02, 13.47it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:02, 13.47it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:02, 13.47it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:02, 13.47it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:02, 13.47it/s]

    Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:02, 13.47it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:02, 13.47it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 20.16it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:02<00:01, 24.43it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:02<00:01, 24.43it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:02<00:01, 24.43it/s]

    Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:02<00:01, 24.43it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:02<00:01, 24.43it/s]Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:02<00:01, 24.43it/s]Compiling num tokens (num_tokens=256):  53%|█████▎    | 31/58 [00:03<00:01, 24.43it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:03<00:00, 29.69it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:03<00:00, 29.69it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:03<00:00, 29.69it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:03<00:00, 29.69it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:03<00:00, 29.69it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:03<00:00, 29.69it/s]Compiling num tokens (num_tokens=160):  64%|██████▍   | 37/58 [00:03<00:00, 29.69it/s]Compiling num tokens (num_tokens=144):  64%|██████▍   | 37/58 [00:03<00:00, 29.69it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 36.84it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 36.84it/s]

    Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 36.84it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 36.84it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 36.84it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 36.84it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 36.84it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 41.47it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 41.47it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 41.47it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 41.47it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 41.47it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 41.47it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 41.47it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 41.47it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 41.47it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.49it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.79 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=76.79 GB):   2%|▏         | 1/58 [00:00<00:17,  3.21it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.76 GB):   2%|▏         | 1/58 [00:00<00:17,  3.21it/s]Capturing num tokens (num_tokens=7168 avail_mem=76.76 GB):   2%|▏         | 1/58 [00:00<00:17,  3.21it/s]Capturing num tokens (num_tokens=7168 avail_mem=76.76 GB):   5%|▌         | 3/58 [00:00<00:07,  7.32it/s]Capturing num tokens (num_tokens=6656 avail_mem=76.76 GB):   5%|▌         | 3/58 [00:00<00:07,  7.32it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   5%|▌         | 3/58 [00:00<00:07,  7.32it/s]Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:04, 10.62it/s]Capturing num tokens (num_tokens=5632 avail_mem=76.75 GB):   9%|▊         | 5/58 [00:00<00:04, 10.62it/s]Capturing num tokens (num_tokens=5120 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:04, 10.62it/s]Capturing num tokens (num_tokens=5120 avail_mem=76.76 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.08it/s]Capturing num tokens (num_tokens=4608 avail_mem=76.75 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.08it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.75 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.08it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=76.75 GB):  16%|█▌        | 9/58 [00:00<00:03, 14.85it/s]Capturing num tokens (num_tokens=3840 avail_mem=76.75 GB):  16%|█▌        | 9/58 [00:00<00:03, 14.85it/s]Capturing num tokens (num_tokens=3584 avail_mem=76.74 GB):  16%|█▌        | 9/58 [00:00<00:03, 14.85it/s]Capturing num tokens (num_tokens=3328 avail_mem=76.74 GB):  16%|█▌        | 9/58 [00:00<00:03, 14.85it/s]Capturing num tokens (num_tokens=3328 avail_mem=76.74 GB):  21%|██        | 12/58 [00:00<00:02, 18.15it/s]Capturing num tokens (num_tokens=3072 avail_mem=76.74 GB):  21%|██        | 12/58 [00:00<00:02, 18.15it/s]Capturing num tokens (num_tokens=2816 avail_mem=76.74 GB):  21%|██        | 12/58 [00:00<00:02, 18.15it/s]Capturing num tokens (num_tokens=2560 avail_mem=76.73 GB):  21%|██        | 12/58 [00:00<00:02, 18.15it/s]Capturing num tokens (num_tokens=2304 avail_mem=76.73 GB):  21%|██        | 12/58 [00:00<00:02, 18.15it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=76.72 GB):  21%|██        | 12/58 [00:00<00:02, 18.15it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.72 GB):  29%|██▉       | 17/58 [00:01<00:01, 26.43it/s]Capturing num tokens (num_tokens=1792 avail_mem=76.72 GB):  29%|██▉       | 17/58 [00:01<00:01, 26.43it/s]Capturing num tokens (num_tokens=1536 avail_mem=76.72 GB):  29%|██▉       | 17/58 [00:01<00:01, 26.43it/s]Capturing num tokens (num_tokens=1280 avail_mem=76.71 GB):  29%|██▉       | 17/58 [00:01<00:01, 26.43it/s]Capturing num tokens (num_tokens=1024 avail_mem=76.69 GB):  29%|██▉       | 17/58 [00:01<00:01, 26.43it/s]Capturing num tokens (num_tokens=960 avail_mem=76.71 GB):  29%|██▉       | 17/58 [00:01<00:01, 26.43it/s] Capturing num tokens (num_tokens=896 avail_mem=76.70 GB):  29%|██▉       | 17/58 [00:01<00:01, 26.43it/s]Capturing num tokens (num_tokens=896 avail_mem=76.70 GB):  40%|███▉      | 23/58 [00:01<00:01, 34.16it/s]Capturing num tokens (num_tokens=832 avail_mem=76.70 GB):  40%|███▉      | 23/58 [00:01<00:01, 34.16it/s]Capturing num tokens (num_tokens=768 avail_mem=76.70 GB):  40%|███▉      | 23/58 [00:01<00:01, 34.16it/s]Capturing num tokens (num_tokens=704 avail_mem=76.69 GB):  40%|███▉      | 23/58 [00:01<00:01, 34.16it/s]Capturing num tokens (num_tokens=640 avail_mem=76.69 GB):  40%|███▉      | 23/58 [00:01<00:01, 34.16it/s]

    Capturing num tokens (num_tokens=576 avail_mem=76.69 GB):  40%|███▉      | 23/58 [00:01<00:01, 34.16it/s]Capturing num tokens (num_tokens=512 avail_mem=76.68 GB):  40%|███▉      | 23/58 [00:01<00:01, 34.16it/s]Capturing num tokens (num_tokens=512 avail_mem=76.68 GB):  50%|█████     | 29/58 [00:01<00:00, 39.46it/s]Capturing num tokens (num_tokens=480 avail_mem=76.69 GB):  50%|█████     | 29/58 [00:01<00:00, 39.46it/s]Capturing num tokens (num_tokens=448 avail_mem=76.69 GB):  50%|█████     | 29/58 [00:01<00:00, 39.46it/s]Capturing num tokens (num_tokens=416 avail_mem=76.69 GB):  50%|█████     | 29/58 [00:01<00:00, 39.46it/s]Capturing num tokens (num_tokens=384 avail_mem=76.69 GB):  50%|█████     | 29/58 [00:01<00:00, 39.46it/s]Capturing num tokens (num_tokens=352 avail_mem=76.68 GB):  50%|█████     | 29/58 [00:01<00:00, 39.46it/s]Capturing num tokens (num_tokens=320 avail_mem=76.68 GB):  50%|█████     | 29/58 [00:01<00:00, 39.46it/s]Capturing num tokens (num_tokens=320 avail_mem=76.68 GB):  60%|██████    | 35/58 [00:01<00:00, 43.18it/s]Capturing num tokens (num_tokens=288 avail_mem=76.68 GB):  60%|██████    | 35/58 [00:01<00:00, 43.18it/s]Capturing num tokens (num_tokens=256 avail_mem=76.67 GB):  60%|██████    | 35/58 [00:01<00:00, 43.18it/s]Capturing num tokens (num_tokens=240 avail_mem=76.67 GB):  60%|██████    | 35/58 [00:01<00:00, 43.18it/s]

    Capturing num tokens (num_tokens=224 avail_mem=76.67 GB):  60%|██████    | 35/58 [00:01<00:00, 43.18it/s]Capturing num tokens (num_tokens=208 avail_mem=76.66 GB):  60%|██████    | 35/58 [00:01<00:00, 43.18it/s]Capturing num tokens (num_tokens=192 avail_mem=76.66 GB):  60%|██████    | 35/58 [00:01<00:00, 43.18it/s]Capturing num tokens (num_tokens=192 avail_mem=76.66 GB):  71%|███████   | 41/58 [00:01<00:00, 45.85it/s]Capturing num tokens (num_tokens=176 avail_mem=76.66 GB):  71%|███████   | 41/58 [00:01<00:00, 45.85it/s]Capturing num tokens (num_tokens=160 avail_mem=76.66 GB):  71%|███████   | 41/58 [00:01<00:00, 45.85it/s]Capturing num tokens (num_tokens=144 avail_mem=76.65 GB):  71%|███████   | 41/58 [00:01<00:00, 45.85it/s]Capturing num tokens (num_tokens=128 avail_mem=76.65 GB):  71%|███████   | 41/58 [00:01<00:00, 45.85it/s]Capturing num tokens (num_tokens=112 avail_mem=76.65 GB):  71%|███████   | 41/58 [00:01<00:00, 45.85it/s]Capturing num tokens (num_tokens=96 avail_mem=76.64 GB):  71%|███████   | 41/58 [00:01<00:00, 45.85it/s] Capturing num tokens (num_tokens=96 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:01<00:00, 47.33it/s]Capturing num tokens (num_tokens=80 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:01<00:00, 47.33it/s]Capturing num tokens (num_tokens=64 avail_mem=76.64 GB):  81%|████████  | 47/58 [00:01<00:00, 47.33it/s]

    Capturing num tokens (num_tokens=48 avail_mem=76.63 GB):  81%|████████  | 47/58 [00:01<00:00, 47.33it/s]Capturing num tokens (num_tokens=32 avail_mem=76.63 GB):  81%|████████  | 47/58 [00:01<00:00, 47.33it/s]Capturing num tokens (num_tokens=28 avail_mem=76.62 GB):  81%|████████  | 47/58 [00:01<00:00, 47.33it/s]Capturing num tokens (num_tokens=24 avail_mem=76.62 GB):  81%|████████  | 47/58 [00:01<00:00, 47.33it/s]Capturing num tokens (num_tokens=24 avail_mem=76.62 GB):  91%|█████████▏| 53/58 [00:01<00:00, 48.23it/s]Capturing num tokens (num_tokens=20 avail_mem=76.62 GB):  91%|█████████▏| 53/58 [00:01<00:00, 48.23it/s]Capturing num tokens (num_tokens=16 avail_mem=76.62 GB):  91%|█████████▏| 53/58 [00:01<00:00, 48.23it/s]Capturing num tokens (num_tokens=12 avail_mem=76.61 GB):  91%|█████████▏| 53/58 [00:01<00:00, 48.23it/s]Capturing num tokens (num_tokens=8 avail_mem=76.61 GB):  91%|█████████▏| 53/58 [00:01<00:00, 48.23it/s] Capturing num tokens (num_tokens=4 avail_mem=76.61 GB):  91%|█████████▏| 53/58 [00:01<00:00, 48.23it/s]Capturing num tokens (num_tokens=4 avail_mem=76.61 GB): 100%|██████████| 58/58 [00:01<00:00, 32.03it/s]


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
    Generated text:  Sheryl. I am a mental health advocate, certified professional counselor, licensed clinical assistant professor of psychology at Oakland University, and a former clinical mental health counselor. My work is centered on building safe, nurturing environments and communities that facilitate mental health wellness. I am dedicated to helping people recover from trauma and the challenges of life's stresses. I have had the privilege of working in the field of psychology for over 30 years and have tutored individuals and children across the United States, Europe, and the Middle East. My passion is to foster meaningful relationships, support the development of strong, independent adults, and empower them to become the best
    ===============================
    Prompt: The president of the United States is
    Generated text:  a position that can be held by anyone. But how many times can the president serve? Let's explore how a person can become the president of the United States. The president is a unique and influential position, and it's important to understand how it works. In this article, we'll delve into the process of becoming president, including the qualifications required, the election process, and the duties of the office.
    One of the key qualifications for a person to become president of the United States is that they must be a natural-born citizen of the United States. This means that the person must be born in the United States, even if they were
    ===============================
    Prompt: The capital of France is
    Generated text:  located in:
    A. Île-de-France
    B. Île-de-Port
    C. Île-de-France
    D. Île-de-Port
    D. Île-de-Port
    
    The capital of France is located on the island of Corsica, not Île-de-France or Île-de-Port. Île-de-France is the easternmost part of France, while Île-de-Port is a small island located in the north of France. Both are not the capital of France. Île-de-France is the capital of France, and Île-de-Port is the administrative capital
    ===============================
    Prompt: The future of AI is
    Generated text:  going to be complex and unpredictable. The technology is currently limited to performing certain jobs, but the rise of neural networks and machine learning means that new kinds of tasks can be performed by AI.
    One of the most interesting areas of research is the development of new forms of AI that can interact with humans in a meaningful way. One of the most promising areas of research is the development of natural language processing, or NLP. NLP is the ability of machines to understand human language and respond to human questions, as well as the ability of humans to understand machine language.
    NLP is a rapidly growing field, and it has the potential to transform


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


    Generated text:  Paris. It is the largest city in the country and is home to many of France's most famous landmarks and attractions. Paris is known for its rich history, beautiful architecture, and vibrant culture, and is a popular tourist destination for people from all over the world. The city is also home to the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral, among other notable structures. Paris is a cultural and intellectual hub, and is a major economic and financial center for France. Its status as the capital of France has made it a major player in the world of politics, business, and culture. Paris is also
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some potential future trends in AI:
    
    1. Increased automation and robotics: As AI continues to improve, we are likely to see more automation and robotics in various industries, from manufacturing to healthcare. This will lead to increased efficiency, cost savings, and job displacement, but it will also create new opportunities for AI developers and entrepreneurs.
    
    2. AI in healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs. As AI continues to evolve, we may see even more sophisticated applications in
    


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
    Generated text:  [Your Name], and I am a [Your Major or Profession]. I hold a Bachelor's degree in [Your Field of Study] and have been a [Your Role] for [Your Current or Previous Organization]. I am a strong communicator and can effectively manage both personal and professional relationships. I am organized, detail-oriented, and have a positive attitude. I am passionate about [Your Passion/Interest], and I am constantly seeking to learn and grow in my field. I enjoy [Your Interests], and I look forward to [Your Future Goals] in my career. Thank you for having me! Let me know if you have any
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    Paris is known for its rich history, beautiful architecture, and diverse cultural scene. It is also a major financial center and a popular tourist destination. The city is home to iconic landmarks such as Notre-Dame Cathedral and the Eiffel Tower. Paris has a strong economy and is an important center for international business and diplomacy. The city's location in the French countryside is also a notable feature, known as the "Eiffil-la-Montagne" (Montmartre) and the "Champs-Élysées" (Le Comte). The city is often called the "City of Light" and is considered one
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  very promising and many possibilities are on the horizon. Some of the most exciting trends include:
    
    1. Increased AI ethics: As AI becomes more integrated into our daily lives, there is a growing need for ethical guidelines and regulations to ensure that AI systems are used safely and responsibly. This includes guidelines for the use of AI in healthcare, education, transportation, and more.
    
    2. Autonomous vehicles: With the development of AI, autonomous vehicles are becoming more feasible and are set to become an increasingly important part of our daily lives. These vehicles could potentially save lives by reducing accidents caused by human error and could also reduce traffic congestion and emissions.
    
    3.


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

     am

     an

     AI

     model

     created

     by

     [

    Company

     Name

    ].

     I

     am

     here

     to

     assist

     with

     any

     questions

     or

     tasks

     you

     may

     have

    ,

     and

     I

     will

     do

     my

     best

     to

     provide

     you

     with

     the

     best

     possible

     results

    .

     How

     can

     I

     assist

     you

     today

    ?

     [

    Name

    ]

     (

    your

     name

    )

     is

     here

     to

     help

    .

     [

    Name

    ]

     (

    your

     name

    )

     is

     here

     to

     help

    .

     [

    Name

    ]

     (

    your

     name

    )

     is

     here

     to

     help

    .

     [

    Name

    ]

     (

    your

     name

    )

     is

     here

     to

     help

    .

     [

    Name

    ]

     (

    your

     name

    )

     is

     here

     to

     help

    .

     [

    Name

    ]

     (

    your

     name

    )

     is

     here

     to

     help

    .

     [

    Name

    ]

     (

    your

     name

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     It

     is

     also

     the

     largest

     city

     in

     France

     and

     the

     third

     largest

     city

     in

     Europe

    ,

     with

     a

     population

     of

     over

     

    2

    .

    7

     million

     people

    .

     Paris

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

     and

     numerous

     other

     landmarks

    .

     It

     is

     also

     a

     cultural

    ,

     economic

    ,

     and

     political

     center

     of

     France

    .

     Paris

     is

     known

     for

     its

     rich

     history

    ,

     vibrant

     arts

     scene

    ,

     and

     rich

     cuisine

    .

     The

     city

     is

     a

     popular

     tourist

     destination

     and

     is

     home

     to

     numerous

     museums

    ,

     galleries

    ,

     and

     theaters

    .

     It

     is

     a

     major

     transportation

     hub

     and

     is

     easily

     accessible

     from

     all

     over

     Europe

     and

     the

     United

     States

    .

     Paris

     is

     a

     UNESCO

     World

     Heritage

     site

     and

     a

     UNESCO

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     exciting

    ,

     and

     there

     are

     many

     possible

     trends

     that

     could

     shape

     the

     development

     of

     this

     technology

    .

     Here

     are

     some

     of

     the

     most

     promising

     areas

     of

     focus

    :
    


    1

    .

     Increased

     Integration

     with

     Existing

     Technologies

    :

     As

     AI

     becomes

     more

     integrated

     with

     other

     technologies

    ,

     it

     will

     become

     easier

     and

     more

     efficient

     to

     create

     complex

     systems

     that

     utilize

     AI

     in

     ways

     that

     are

     currently

     impossible

     with

     traditional

     programming

    .
    


    2

    .

     Personal

    ization

     and

     Custom

    ization

    :

     AI

     will

     allow

     for

     more

     personalized

     and

     customized

     user

     experiences

    ,

     making

     it

     easier

     to

     create

     products

     or

     services

     that

     are

     tailored

     to

     individual

     needs

     and

     preferences

    .
    


    3

    .

     Natural

     Language

     Processing

     and

     Chat

    bots

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

     more

     natural

     language

     processing

     and

    



```python
llm.shutdown()
```
