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


    2026-03-28 15:16:29.984 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 15:16:29] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 15:16:29.984 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 15:16:29] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 15:16:29.984 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 15:16:29] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 15:16:29.984 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 15:16:29] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 15:16:29.984 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 15:16:29] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.14it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.14it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:04,  2.19s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:04,  2.19s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:04,  2.19s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:02<00:33,  1.63it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:02<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   5%|▌         | 3/58 [00:02<00:33,  1.63it/s]Compiling num tokens (num_tokens=5632):   5%|▌         | 3/58 [00:02<00:33,  1.63it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:02<00:13,  3.89it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:02<00:13,  3.89it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:02<00:13,  3.89it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:02<00:13,  3.89it/s]Compiling num tokens (num_tokens=3840):  10%|█         | 6/58 [00:02<00:13,  3.89it/s]Compiling num tokens (num_tokens=3584):  10%|█         | 6/58 [00:02<00:13,  3.89it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:05,  8.46it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:05,  8.46it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:05,  8.46it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:05,  8.46it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:05,  8.46it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:05,  8.46it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:05,  8.46it/s]

    Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:05,  8.46it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:02, 15.85it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:02, 15.85it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:02, 15.85it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:02, 15.85it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:02, 15.85it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:02, 15.85it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:02, 15.85it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:02<00:01, 22.30it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:02<00:01, 22.30it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:02<00:01, 22.30it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:02<00:01, 22.30it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:02<00:01, 22.30it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:02<00:01, 22.30it/s]

    Compiling num tokens (num_tokens=480):  41%|████▏     | 24/58 [00:02<00:01, 22.30it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:02<00:00, 28.75it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:02<00:00, 28.75it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:02<00:00, 28.75it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:02<00:00, 28.75it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:02<00:00, 28.75it/s]Compiling num tokens (num_tokens=320):  52%|█████▏    | 30/58 [00:02<00:00, 28.75it/s]Compiling num tokens (num_tokens=288):  52%|█████▏    | 30/58 [00:02<00:00, 28.75it/s]Compiling num tokens (num_tokens=256):  52%|█████▏    | 30/58 [00:02<00:00, 28.75it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:02<00:00, 36.01it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:02<00:00, 36.01it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:02<00:00, 36.01it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:02<00:00, 36.01it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:03<00:00, 36.01it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:03<00:00, 36.01it/s]

    Compiling num tokens (num_tokens=160):  64%|██████▍   | 37/58 [00:03<00:00, 36.01it/s]Compiling num tokens (num_tokens=144):  64%|██████▍   | 37/58 [00:03<00:00, 36.01it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 42.63it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 42.63it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 42.63it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 42.63it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 42.63it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 42.63it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 42.63it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 46.59it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 46.59it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 46.59it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 46.59it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 46.59it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 46.59it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 46.59it/s]

    Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 46.59it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 46.59it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.79it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=71.05 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.02 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7168 avail_mem=71.02 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7168 avail_mem=71.02 GB):   5%|▌         | 3/58 [00:00<00:02, 21.26it/s]Capturing num tokens (num_tokens=6656 avail_mem=71.01 GB):   5%|▌         | 3/58 [00:00<00:02, 21.26it/s]Capturing num tokens (num_tokens=6144 avail_mem=71.02 GB):   5%|▌         | 3/58 [00:00<00:02, 21.26it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=71.01 GB):   5%|▌         | 3/58 [00:00<00:02, 21.26it/s]Capturing num tokens (num_tokens=5632 avail_mem=71.01 GB):  10%|█         | 6/58 [00:00<00:02, 24.77it/s]Capturing num tokens (num_tokens=5120 avail_mem=71.01 GB):  10%|█         | 6/58 [00:00<00:02, 24.77it/s]Capturing num tokens (num_tokens=4608 avail_mem=71.01 GB):  10%|█         | 6/58 [00:00<00:02, 24.77it/s]Capturing num tokens (num_tokens=4096 avail_mem=71.01 GB):  10%|█         | 6/58 [00:00<00:02, 24.77it/s]Capturing num tokens (num_tokens=4096 avail_mem=71.01 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.04it/s]Capturing num tokens (num_tokens=3840 avail_mem=71.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.04it/s]Capturing num tokens (num_tokens=3584 avail_mem=71.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.04it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=71.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.04it/s]Capturing num tokens (num_tokens=3072 avail_mem=70.99 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.04it/s]Capturing num tokens (num_tokens=3072 avail_mem=70.99 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.23it/s]Capturing num tokens (num_tokens=2816 avail_mem=70.99 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.23it/s]Capturing num tokens (num_tokens=2560 avail_mem=70.99 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.23it/s]Capturing num tokens (num_tokens=2304 avail_mem=70.99 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.23it/s]Capturing num tokens (num_tokens=2048 avail_mem=70.98 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.23it/s]Capturing num tokens (num_tokens=1792 avail_mem=70.98 GB):  22%|██▏       | 13/58 [00:00<00:01, 30.23it/s]Capturing num tokens (num_tokens=1792 avail_mem=70.98 GB):  31%|███       | 18/58 [00:00<00:01, 36.15it/s]Capturing num tokens (num_tokens=1536 avail_mem=70.98 GB):  31%|███       | 18/58 [00:00<00:01, 36.15it/s]Capturing num tokens (num_tokens=1280 avail_mem=70.97 GB):  31%|███       | 18/58 [00:00<00:01, 36.15it/s]Capturing num tokens (num_tokens=1024 avail_mem=70.95 GB):  31%|███       | 18/58 [00:00<00:01, 36.15it/s]

    Capturing num tokens (num_tokens=960 avail_mem=70.96 GB):  31%|███       | 18/58 [00:00<00:01, 36.15it/s] Capturing num tokens (num_tokens=896 avail_mem=70.96 GB):  31%|███       | 18/58 [00:00<00:01, 36.15it/s]Capturing num tokens (num_tokens=896 avail_mem=70.96 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.98it/s]Capturing num tokens (num_tokens=832 avail_mem=70.96 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.98it/s]Capturing num tokens (num_tokens=768 avail_mem=70.95 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.98it/s]Capturing num tokens (num_tokens=704 avail_mem=70.95 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.98it/s]Capturing num tokens (num_tokens=640 avail_mem=70.95 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.98it/s]Capturing num tokens (num_tokens=640 avail_mem=70.95 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.83it/s]Capturing num tokens (num_tokens=576 avail_mem=70.95 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.83it/s]

    Capturing num tokens (num_tokens=512 avail_mem=70.94 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.83it/s]Capturing num tokens (num_tokens=480 avail_mem=70.95 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.83it/s]Capturing num tokens (num_tokens=448 avail_mem=70.95 GB):  47%|████▋     | 27/58 [00:00<00:00, 37.83it/s]Capturing num tokens (num_tokens=448 avail_mem=70.95 GB):  53%|█████▎    | 31/58 [00:00<00:00, 35.73it/s]Capturing num tokens (num_tokens=416 avail_mem=70.95 GB):  53%|█████▎    | 31/58 [00:00<00:00, 35.73it/s]Capturing num tokens (num_tokens=384 avail_mem=70.95 GB):  53%|█████▎    | 31/58 [00:00<00:00, 35.73it/s]Capturing num tokens (num_tokens=352 avail_mem=70.94 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.73it/s]Capturing num tokens (num_tokens=320 avail_mem=70.94 GB):  53%|█████▎    | 31/58 [00:01<00:00, 35.73it/s]Capturing num tokens (num_tokens=320 avail_mem=70.94 GB):  60%|██████    | 35/58 [00:01<00:00, 34.49it/s]Capturing num tokens (num_tokens=288 avail_mem=70.93 GB):  60%|██████    | 35/58 [00:01<00:00, 34.49it/s]

    Capturing num tokens (num_tokens=256 avail_mem=70.93 GB):  60%|██████    | 35/58 [00:01<00:00, 34.49it/s]Capturing num tokens (num_tokens=240 avail_mem=70.93 GB):  60%|██████    | 35/58 [00:01<00:00, 34.49it/s]Capturing num tokens (num_tokens=224 avail_mem=70.92 GB):  60%|██████    | 35/58 [00:01<00:00, 34.49it/s]Capturing num tokens (num_tokens=224 avail_mem=70.92 GB):  67%|██████▋   | 39/58 [00:01<00:00, 35.32it/s]Capturing num tokens (num_tokens=208 avail_mem=70.92 GB):  67%|██████▋   | 39/58 [00:01<00:00, 35.32it/s]Capturing num tokens (num_tokens=192 avail_mem=70.92 GB):  67%|██████▋   | 39/58 [00:01<00:00, 35.32it/s]Capturing num tokens (num_tokens=176 avail_mem=70.92 GB):  67%|██████▋   | 39/58 [00:01<00:00, 35.32it/s]Capturing num tokens (num_tokens=160 avail_mem=70.91 GB):  67%|██████▋   | 39/58 [00:01<00:00, 35.32it/s]Capturing num tokens (num_tokens=160 avail_mem=70.91 GB):  74%|███████▍  | 43/58 [00:01<00:00, 34.73it/s]Capturing num tokens (num_tokens=144 avail_mem=70.91 GB):  74%|███████▍  | 43/58 [00:01<00:00, 34.73it/s]

    Capturing num tokens (num_tokens=128 avail_mem=70.91 GB):  74%|███████▍  | 43/58 [00:01<00:00, 34.73it/s]Capturing num tokens (num_tokens=112 avail_mem=70.91 GB):  74%|███████▍  | 43/58 [00:01<00:00, 34.73it/s]Capturing num tokens (num_tokens=96 avail_mem=70.90 GB):  74%|███████▍  | 43/58 [00:01<00:00, 34.73it/s] Capturing num tokens (num_tokens=96 avail_mem=70.90 GB):  81%|████████  | 47/58 [00:01<00:00, 33.85it/s]Capturing num tokens (num_tokens=80 avail_mem=70.90 GB):  81%|████████  | 47/58 [00:01<00:00, 33.85it/s]Capturing num tokens (num_tokens=64 avail_mem=70.89 GB):  81%|████████  | 47/58 [00:01<00:00, 33.85it/s]Capturing num tokens (num_tokens=48 avail_mem=70.89 GB):  81%|████████  | 47/58 [00:01<00:00, 33.85it/s]

    Capturing num tokens (num_tokens=32 avail_mem=70.89 GB):  81%|████████  | 47/58 [00:01<00:00, 33.85it/s]Capturing num tokens (num_tokens=32 avail_mem=70.89 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.48it/s]Capturing num tokens (num_tokens=28 avail_mem=70.88 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.48it/s]Capturing num tokens (num_tokens=24 avail_mem=70.88 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.48it/s]Capturing num tokens (num_tokens=20 avail_mem=70.88 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.48it/s]Capturing num tokens (num_tokens=16 avail_mem=70.88 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.48it/s]Capturing num tokens (num_tokens=12 avail_mem=70.87 GB):  88%|████████▊ | 51/58 [00:01<00:00, 32.48it/s]Capturing num tokens (num_tokens=12 avail_mem=70.87 GB):  97%|█████████▋| 56/58 [00:01<00:00, 35.42it/s]Capturing num tokens (num_tokens=8 avail_mem=70.87 GB):  97%|█████████▋| 56/58 [00:01<00:00, 35.42it/s] Capturing num tokens (num_tokens=4 avail_mem=70.86 GB):  97%|█████████▋| 56/58 [00:01<00:00, 35.42it/s]Capturing num tokens (num_tokens=4 avail_mem=70.86 GB): 100%|██████████| 58/58 [00:01<00:00, 34.22it/s]


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
    Generated text:  Asmuta and I'm a Science Student at the University of Michigan in Ann Arbor, Michigan.
    I am interested in learning more about the neurobiology of learning and memory. I have studied the effects of specific neurotransmitters on learning and memory and I hope to study the effects of new drugs on learning and memory. I'm also interested in animal welfare and I hope to study how stress can affect learning.
    I would like to learn more about the role of stress on the nervous system, how stress affects learning and memory, and the role of hormones in stress. I would also like to study how stress affects the brain of both animals and humans
    ===============================
    Prompt: The president of the United States is
    Generated text:  a very important person in the country. He is like the boss of the whole country. He has many important things to do. In this way, he always feels very busy. He has to work in the morning and in the evening. He has to be in Washington D. C. every day. He has to drive his car to work. He has to eat a lot of food. He also likes to spend time in bed with his family. What is the best title of this passage? A) Important person. B) Boss of the country. C) President of the United States. D) Secretary of the United States.
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, famous for its world-famous Eiffel Tower and 18 other landmarks. The city is rich in history, culture and gastronomy. Paris is also the capital of the French Republic, and the French language is the national language of France. The climate of Paris is hot and humid, and it has been the capital of France since 1804, when Napoleon Bonaparte reigned. The city of Paris is the home of the Louvre Museum and the Gare du Nord railway station.
    The French Revolution began in 1789, when there was widespread discontent with the monarchy. The people showed
    ===============================
    Prompt: The future of AI is
    Generated text:  not in the data
    
    The future of AI is not in the data
    
    Image source: Shutterstock
    
    The future of AI is not in the data
    
    If you’re looking for a compelling argument for AI, there are a few things you might consider. One of the most widely debated topics is whether the future of AI will be data-driven or not. To answer the question, we need to understand what “data” means. This is a topic worth discussing.
    
    In the field of AI, we’re constantly looking at how to gather and analyze data. The algorithms and software that powers these algorithms and software are constantly being updated. However, we


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


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a cultural and economic hub, known for its rich history, art, and cuisine. It is also home to many famous landmarks and attractions, including the Louvre, the Eiffel Tower, and the Champs-Élysées. Paris is a major transportation hub, with many major highways and rail lines connecting the city to other parts of France and the world. It is also
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes, reduce costs, and improve the quality of care. As AI technology continues to improve, we can expect to see even more widespread use of AI in healthcare, with the potential to revolutionize the way we treat and diagnose diseases.
    
    2. Increased use of AI in finance: AI is already being used in finance to improve risk management,
    


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
    Generated text:  [Your Name] and I am a [insert your profession or identity], and I am thrilled to be here today. I am a [insert your profession or identity], and I am [insert your profession or identity]. I am excited to meet all of you and to discuss [insert your interests or expertise]. Thank you. [Your Name]. [Your Profession/Identity]. Hello! I'm thrilled to have the opportunity to meet you all. Thank you for being here today! Let's get started. [Your Name]. [Your Profession/Identity]. Hello! I'm thrilled to have the opportunity to meet you all. Thank you for
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, the city where the Eiffel Tower stands as an iconic landmark. It serves as the political, economic, cultural, and diplomatic center of the country. Paris is known for its rich history, diverse architecture, and vibrant culture. The city's cuisine, art, fashion, and music industries have also made it a globally recognized city. Paris is often referred to as the "city of a thousand museums" due to its numerous cultural institutions, including the Louvre, Notre-Dame Cathedral, and the Musée d'Orsay. It's also famous for its fashion and music scenes. Overall, Paris is a thriving metropolis
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  full of possibilities and possibilities. Here are some possible trends we could expect in the AI field:
    
    1. Increased accuracy and reliability: AI systems will continue to get more accurate and reliable. This will be achieved by increasing the complexity of AI models, reducing their reliance on human supervision, and improving their ability to learn from data.
    
    2. Personalization and customization: AI will be able to learn more about individual users and provide personalized recommendations, products, and services. This will be achieved by using data analytics and machine learning to understand user behavior and preferences.
    
    3. Autonomous and self-driving vehicles: Self-driving vehicles will become more common, and AI


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

    First

     Name

    ]

     and

     I

     am

     a

     [

    Title

    ]

     at

     [

    Company

    ].

     My

     name

     is

     [

    First

     Name

    ]

     and

     I

     am

     a

     [

    Title

    ]

     at

     [

    Company

    ].

     I

     have

     been

     [

    number

     of

     years

    ]

     working

     at

     [

    Company

    ]

     and

     I

     am

     currently

     [

    current

     role

     or

     position

    ].

     I

     am

     excited

     to

     bring

     my

     [

    number

     of

     years

    ]

     years

     of

     experience

     and

     my

     [

    any

     relevant

     skills

     or

     qualifications

    ]

     to

     [

    Company

    ].

     I

     believe

     that

     my

     [

    number

     of

     years

    ]

     years

     of

     experience

     and

     my

     [

    any

     relevant

     skills

     or

     qualifications

    ]

     make

     me

     a

     great

     fit

     for

     [

    Company

    's

    ]

     leadership

     role

    .

     I

     am

     looking

     forward

     to

     [

    reason

     for

     my

     first

     job

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .


    Paris

     is

     the

     largest

     and

     most

     populous

     city

     in

     France

    ,

     known

     for

     its

     historic

     architecture

    ,

     world

    -ren

    owned

     museums

    ,

     and

     vibrant

     culture

    .

     It

     is

     also

     known

     as

     "

    La

     Pop

    "

     in

     French

    ,

     which

     refers

     to

     the

     Paris

    ian

     style

     of

     French

     art

     and

     design

    .

     Paris

     is

     a

     cultural

     and

     political

     center

    ,

     hosting

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

     and

     Notre

     Dame

     Cathedral

    ,

     as

     well

     as

     hosting

     the

     Winter

     Olympics

     and

     the

     Summer

     Olympics

    .

     The

     city

     is

     also

     the

     seat

     of

     the

     French

     government

     and

     is

     known

     for

     its

     elaborate

     public

     transportation

     system

     and

     various

     festivals

     throughout

     the

     year

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

     love

    "

     due

     to

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     a

     combination

     of

     several

     trends

    ,

     some

     of

     which

     include

    :
    


    1

    .

     Increased

     integration

     of

     AI

     into

     various

     industries

    :

     As

     AI

     technology

     becomes

     more

     advanced

    ,

     it

     is

     likely

     to

     be

     integrated

     into

     a

     wide

     range

     of

     industries

    ,

     including

     healthcare

    ,

     transportation

    ,

     manufacturing

    ,

     and

     finance

    .
    


    2

    .

     Greater

     adoption

     of

     AI

     for

     predictive

     analytics

    :

     AI

     will

     continue

     to

     be

     used

     for

     predictive

     analytics

    ,

     which

     can

     help

     businesses

     make

     more

     informed

     decisions

     and

     optimize

     operations

    .
    


    3

    .

     Growth

     of

     AI

    -powered

     chat

    bots

     and

     virtual

     assistants

    :

     As

     AI

     technology

     continues

     to

     improve

    ,

     it

     is

     likely

     that

     we

     will

     see

     the

     rise

     of

     AI

    -powered

     chat

    bots

     and

     virtual

     assistants

    ,

     which

     can

     provide

    



```python
llm.shutdown()
```
