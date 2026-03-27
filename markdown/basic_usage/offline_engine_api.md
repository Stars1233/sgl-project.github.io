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


    2026-03-27 02:07:09.921 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 02:07:09] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 02:07:09.921 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 02:07:09] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 02:07:09.921 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 02:07:09] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 02:07:09.921 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 02:07:09] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 02:07:09.921 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 02:07:09] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.54it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.53it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:24,  2.53s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:24,  2.53s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:24,  2.53s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:24,  2.53s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:27,  1.96it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.01it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.01it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.01it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.01it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.01it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.01it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.01it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.01it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.01it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 12.59it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 12.59it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:03, 12.59it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:03, 12.59it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:03, 12.59it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:03, 12.59it/s]

    Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:03, 12.59it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:03, 12.59it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 18.82it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 18.82it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 18.82it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.82it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.82it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.82it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.82it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.82it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 25.48it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 25.48it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 25.48it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 25.48it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 25.48it/s]

    Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 25.48it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 25.48it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 30.44it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 30.44it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 30.44it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 30.44it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 30.44it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 30.44it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 30.44it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.81it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.81it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.81it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.81it/s] 

    Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.81it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.81it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.81it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.97it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.97it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.97it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.97it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.97it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.97it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.97it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.97it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.97it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.44it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=121.74 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.71 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.71 GB):   3%|▎         | 2/58 [00:00<00:03, 17.91it/s]Capturing num tokens (num_tokens=7168 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:03, 17.91it/s]Capturing num tokens (num_tokens=6656 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:03, 17.91it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:03, 17.91it/s]Capturing num tokens (num_tokens=6144 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 21.04it/s]Capturing num tokens (num_tokens=5632 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 21.04it/s]Capturing num tokens (num_tokens=5120 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 21.04it/s]Capturing num tokens (num_tokens=4608 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 21.04it/s]Capturing num tokens (num_tokens=4608 avail_mem=121.70 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.22it/s]Capturing num tokens (num_tokens=4096 avail_mem=121.70 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.22it/s]Capturing num tokens (num_tokens=3840 avail_mem=121.69 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.22it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=121.69 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.22it/s]Capturing num tokens (num_tokens=3328 avail_mem=121.69 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.22it/s]Capturing num tokens (num_tokens=3328 avail_mem=121.69 GB):  21%|██        | 12/58 [00:00<00:01, 28.71it/s]Capturing num tokens (num_tokens=3072 avail_mem=121.68 GB):  21%|██        | 12/58 [00:00<00:01, 28.71it/s]Capturing num tokens (num_tokens=2816 avail_mem=121.68 GB):  21%|██        | 12/58 [00:00<00:01, 28.71it/s]Capturing num tokens (num_tokens=2560 avail_mem=121.68 GB):  21%|██        | 12/58 [00:00<00:01, 28.71it/s]Capturing num tokens (num_tokens=2304 avail_mem=121.67 GB):  21%|██        | 12/58 [00:00<00:01, 28.71it/s]Capturing num tokens (num_tokens=2304 avail_mem=121.67 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.20it/s]Capturing num tokens (num_tokens=2048 avail_mem=121.67 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.20it/s]Capturing num tokens (num_tokens=1792 avail_mem=121.67 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.20it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=121.66 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.20it/s]Capturing num tokens (num_tokens=1280 avail_mem=121.38 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.20it/s]Capturing num tokens (num_tokens=1024 avail_mem=121.36 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.20it/s]Capturing num tokens (num_tokens=1024 avail_mem=121.36 GB):  36%|███▌      | 21/58 [00:00<00:01, 36.03it/s]Capturing num tokens (num_tokens=960 avail_mem=121.38 GB):  36%|███▌      | 21/58 [00:00<00:01, 36.03it/s] Capturing num tokens (num_tokens=896 avail_mem=121.36 GB):  36%|███▌      | 21/58 [00:00<00:01, 36.03it/s]Capturing num tokens (num_tokens=832 avail_mem=120.67 GB):  36%|███▌      | 21/58 [00:00<00:01, 36.03it/s]Capturing num tokens (num_tokens=768 avail_mem=120.67 GB):  36%|███▌      | 21/58 [00:00<00:01, 36.03it/s]Capturing num tokens (num_tokens=768 avail_mem=120.67 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.65it/s]Capturing num tokens (num_tokens=704 avail_mem=120.66 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.65it/s]

    Capturing num tokens (num_tokens=640 avail_mem=120.66 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.65it/s]Capturing num tokens (num_tokens=576 avail_mem=120.66 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.65it/s]Capturing num tokens (num_tokens=512 avail_mem=120.65 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.65it/s]Capturing num tokens (num_tokens=480 avail_mem=120.66 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.65it/s]Capturing num tokens (num_tokens=480 avail_mem=120.66 GB):  52%|█████▏    | 30/58 [00:00<00:00, 38.48it/s]Capturing num tokens (num_tokens=448 avail_mem=120.66 GB):  52%|█████▏    | 30/58 [00:00<00:00, 38.48it/s]Capturing num tokens (num_tokens=416 avail_mem=120.66 GB):  52%|█████▏    | 30/58 [00:00<00:00, 38.48it/s]Capturing num tokens (num_tokens=384 avail_mem=120.66 GB):  52%|█████▏    | 30/58 [00:00<00:00, 38.48it/s]Capturing num tokens (num_tokens=352 avail_mem=120.65 GB):  52%|█████▏    | 30/58 [00:00<00:00, 38.48it/s]Capturing num tokens (num_tokens=320 avail_mem=120.65 GB):  52%|█████▏    | 30/58 [00:00<00:00, 38.48it/s]

    Capturing num tokens (num_tokens=320 avail_mem=120.65 GB):  60%|██████    | 35/58 [00:01<00:00, 40.11it/s]Capturing num tokens (num_tokens=288 avail_mem=120.64 GB):  60%|██████    | 35/58 [00:01<00:00, 40.11it/s]Capturing num tokens (num_tokens=256 avail_mem=120.64 GB):  60%|██████    | 35/58 [00:01<00:00, 40.11it/s]Capturing num tokens (num_tokens=240 avail_mem=120.64 GB):  60%|██████    | 35/58 [00:01<00:00, 40.11it/s]Capturing num tokens (num_tokens=224 avail_mem=120.64 GB):  60%|██████    | 35/58 [00:01<00:00, 40.11it/s]Capturing num tokens (num_tokens=208 avail_mem=120.63 GB):  60%|██████    | 35/58 [00:01<00:00, 40.11it/s]Capturing num tokens (num_tokens=208 avail_mem=120.63 GB):  69%|██████▉   | 40/58 [00:01<00:00, 41.66it/s]Capturing num tokens (num_tokens=192 avail_mem=120.63 GB):  69%|██████▉   | 40/58 [00:01<00:00, 41.66it/s]Capturing num tokens (num_tokens=176 avail_mem=120.63 GB):  69%|██████▉   | 40/58 [00:01<00:00, 41.66it/s]Capturing num tokens (num_tokens=160 avail_mem=120.62 GB):  69%|██████▉   | 40/58 [00:01<00:00, 41.66it/s]Capturing num tokens (num_tokens=144 avail_mem=120.29 GB):  69%|██████▉   | 40/58 [00:01<00:00, 41.66it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 41.66it/s]

    Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=96 avail_mem=120.19 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.57it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=48 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=48 avail_mem=120.18 GB):  86%|████████▌ | 50/58 [00:01<00:00, 42.89it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  86%|████████▌ | 50/58 [00:01<00:00, 42.89it/s]Capturing num tokens (num_tokens=28 avail_mem=120.17 GB):  86%|████████▌ | 50/58 [00:01<00:00, 42.89it/s]Capturing num tokens (num_tokens=24 avail_mem=120.17 GB):  86%|████████▌ | 50/58 [00:01<00:00, 42.89it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 42.89it/s]

    Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 42.89it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  95%|█████████▍| 55/58 [00:01<00:00, 43.68it/s]Capturing num tokens (num_tokens=12 avail_mem=120.16 GB):  95%|█████████▍| 55/58 [00:01<00:00, 43.68it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  95%|█████████▍| 55/58 [00:01<00:00, 43.68it/s] Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  95%|█████████▍| 55/58 [00:01<00:00, 43.68it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 37.88it/s]


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
    Generated text:  Tanya Jankovic from the United States. I have completed my Bachelor's degree in Education at the University of Nevada, Reno, and my Master's degree in Higher Education Management from the University of Nevada, Reno. I am a certified teacher in Nevada, United States, and hold a teaching credential in English, mathematics, and Spanish from the University of Nevada, Reno.
    My primary focus is to assist teachers in identifying students who may be struggling with learning, and to support them in overcoming these challenges. I work with teachers to increase their knowledge of teaching, and to provide them with tools and strategies that will help them to improve their teaching
    ===============================
    Prompt: The president of the United States is
    Generated text:  a hereditary office, elected from among the American people through a democratic process, but not by a conscripted army. The person in charge of the United States government is elected by the people, and the president is allowed to be re-elected. What do you think is the basis of the difference between the United States and other countries?
    A. The political system is different
    B. The government is different
    C. The country's system of government is different
    D. The country's characteristics are different
    Answer:
    C
    
    Which of the following statements about literary knowledge is correct?
    A. The "Kong Yiji" and
    ===============================
    Prompt: The capital of France is
    Generated text:  ______. (1 point) 
    A. Paris 
    B. London 
    C. New York 
    D. Rome
    A. Paris
    
    The capital of France is Paris. This is a standard fact in geography and historical knowledge. Option A is the correct answer. 
    
    Some additional context: 
    - Paris is often referred to as the "City of Love" due to its historical association with the romantic drama "The Façade" by Puccini.
    - The city is famous for its architecture, history, and culture, including the Eiffel Tower and the Louvre Museum. 
    - It has a rich history, having been
    ===============================
    Prompt: The future of AI is
    Generated text:  rapidly evolving, and there are many exciting developments in this field. In particular, there are several new models that are promising, such as Generative Adversarial Networks (GANs), Generative Models, and Mixture Models. In this essay, I will present a brief overview of these models and their potential applications. I will also discuss the challenges and limitations of these models, as well as the future of AI in general.
    First, let's talk about Generative Adversarial Networks (GANs). GANs are a type of deep learning model that uses two neural networks to generate data that is indistinguishable from real data


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


    Generated text:  [Name] and I'm a [Age] year old [Occupation]. I'm a [Skill] with a passion for [Interest]. I'm [Job Title] and I'm always looking for new challenges and opportunities to grow and learn. I'm [Personality] and I'm always ready to learn from others and make a positive impact in the world. I'm [Hobby] and I enjoy [Favorite Activity]. I'm [Favorite Quote] and I'm always looking for ways to inspire others and make a difference in the world. I'm [Favorite Movie] and I'm always eager to explore new ideas and experiences
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city that serves as the political, cultural, and economic center of the country. It is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum, as well as its rich history and diverse culture. Paris is also home to many famous museums, including the Louvre, the Musée d'Orsay, and the Musée Rodin. The city is also known for its vibrant nightlife and its role as a major transportation hub for Europe. Paris is a city that has a rich history and a unique blend of modern and traditional elements, making it a popular destination
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes and reduce costs. As AI technology continues to improve, we can expect to see even more widespread use of AI in healthcare, particularly in areas such as diagnosis, treatment planning, and patient monitoring.
    
    2. Increased use of AI in finance: AI is already being used in finance to improve risk management, fraud detection, and trading algorithms.
    


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
    Generated text:  [Name], and I'm a [job title] at [company name]. I'm passionate about [specific area of interest] and enjoy [reason for interest], which is why I've been working on this project for [number] years. I'm a [character trait] person, and I love [reason for this trait]. I'm always ready to learn and take on new challenges, and I'm constantly looking for ways to improve my skills and knowledge. I'm [any other relevant traits or qualities] and I look forward to continuing this journey with [company name]. I'm excited to get started on this project and see what
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    That's correct! Paris is the capital city of France, located in the northwestern part of the country. It is France's largest and most populous city, and serves as the seat of government, economy, and culture for the country. It is also one of the most famous cities in the world for its unique architecture, arts, music, and gastronomy. Paris has a rich history, including many significant events like the French Revolution, Napoleon's conquest of Paris, and the opening of the 19th-century Opéra Garnier. Today, it is one of the most popular cities in the world, with a population
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  bright, and there are many possibilities ahead. Here are some possible trends that could shape the future of AI:
    
    1. Increased Use of AI in Healthcare: AI is already being used to improve patient care, diagnose diseases, and develop new treatments. In the future, we could see even more AI being integrated into healthcare, from diagnosing diseases to creating personalized treatment plans.
    
    2. Autonomous Vehicles: As technology advances, autonomous vehicles could become more prevalent, and many cities will be able to develop rules and regulations around their use. This could lead to a decrease in traffic congestion and improve transportation efficiency.
    
    3. AI in Manufacturing: With the help


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

     Jane

    .

     I

     am

     a

     

    3

    2

    -year

    -old

     software

     developer

     who

     has

     been

     working

     in

     the

     field

     for

     over

     a

     decade

    .

     I

    'm

     a

     detail

    -oriented

     individual

     who

     thr

    ives

     on

     creating

     innovative

     solutions

     to

     complex

     problems

    .

     I

    'm

     also

     a

     strong

     communicator

     and

     enjoy

     helping

     others

     learn

     new

     skills

    .

     I

    'm

     always

     looking

     for

     new

     opportunities

     to

     grow

     and

     improve

     my

     skills

    .

     I

    'm

     excited

     to

     learn

     more

     about

     the

     world

     and

     share

     my

     knowledge

     with

     others

    .

     How

     can

     I

     help

     someone

     start

     their

     journey

     towards

     learning

     and

     growth

    ?

     As

     an

     AI

     language

     model

    ,

     I

     don

    't

     have

     personal

     experiences

    ,

     but

     I

     can

     provide

     you

     with

     some

     tips

     on

     how

     to

     help

     someone

     start

     their

     journey

     towards

    
    
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

     and

     the

     world

    ,

     and

     is

     considered

     to

     be

     one

     of

     the

     world

    ’s

     major

     cultural

     and

     historical

     centers

    .

     Paris

     is

     home

     to

     some

     of

     the

     world

    ’s

     most

     famous

     art

     museums

    ,

     cultural

     institutions

    ,

     and

     landmarks

    ,

     and

     is

     known

     for

     its

     vibrant

     nightlife

    ,

     fashion

    ,

     and

     shopping

    .

     The

     city

     is

     also

     famous

     for

     its

     cuisine

    ,

     which

     includes

     dishes

     such

     as

     cro

    iss

    ants

    ,

     bag

    uet

    tes

    ,

     and

     past

    ries

    .

     Paris

     is

     a

     UNESCO

     World

     Heritage

     site

     and

     hosts

     many

     cultural

     festivals

     and

     events

     throughout

     the

     year

    .

     It

     is

     a

     major

     international

     airport

     and

     a

     major

     transportation

     hub

    ,

     with

     many

     train

     and

     bus

     routes

     connecting

     it

     to

     other

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     involve

     a

     number

     of

     different

     trends

    ,

     each

     of

     which

     will

     have

     a

     significant

     impact

     on

     the

     way

     we

     live

    ,

     work

     and

     interact

     with

     technology

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

     Improved

     Natural

     Language

     Processing

    :

     As

     AI

     continues

     to

     improve

    ,

     the

     ability

     to

     understand

     and

     generate

     human

    -like

     language

     will

     become

     increasingly

     powerful

    .

     This

     will

     allow

     for

     more

     accurate

     and

     effective

     communication

    ,

     better

     understanding

     of

     human

     emotions

     and

     behavior

    ,

     and

     improved

     decision

    -making

     processes

    .
    


    2

    .

     Greater

     Integration

     with

     Existing

     Technology

    :

     As

     AI

     technology

     becomes

     more

     advanced

    ,

     it

     will

     become

     increasingly

     integrated

     with

     other

     technologies

    ,

     such

     as

     sensors

    ,

     data

     storage

     and

     processing

    ,

     and

     the

     Internet

     of

     Things

     (

    



```python
llm.shutdown()
```
