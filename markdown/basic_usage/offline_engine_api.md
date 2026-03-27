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


    2026-03-27 23:38:15.610 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:38:15] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 23:38:15.610 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:38:15] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 23:38:15.610 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:38:15] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 23:38:15.610 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:38:15] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 23:38:15.610 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 23:38:15] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  1.91it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  1.91it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:15,  2.39s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:15,  2.39s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:15,  2.39s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:15,  2.39s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:15,  2.39s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:20,  2.63it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:20,  2.63it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:20,  2.63it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:20,  2.63it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:20,  2.63it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:20,  2.63it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:20,  2.63it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:06,  6.85it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:06,  6.85it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:06,  6.85it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:06,  6.85it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:06,  6.85it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:06,  6.85it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:06,  6.85it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:06,  6.85it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:06,  6.85it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.74it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.74it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.74it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.74it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.74it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.74it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.74it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 19.20it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 19.20it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 19.20it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 19.20it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 19.20it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 19.20it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 19.20it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:02<00:01, 19.20it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 26.30it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 26.30it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 26.30it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:00, 26.30it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:00, 26.30it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:02<00:00, 26.30it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:00, 26.30it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 31.38it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 31.38it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 31.38it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 31.38it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 31.38it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 31.38it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 31.38it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 36.80it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 36.80it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 36.80it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 36.80it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 36.80it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 36.80it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 36.80it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 39.77it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 39.77it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 39.77it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 39.77it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 39.77it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 39.77it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 39.77it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 39.77it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 39.77it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.26it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.76 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 17.45it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.45it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.45it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 17.45it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 20.12it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 20.12it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 20.12it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 20.12it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 20.10it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 20.10it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 20.10it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  17%|█▋        | 10/58 [00:00<00:03, 15.37it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  17%|█▋        | 10/58 [00:00<00:03, 15.37it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  17%|█▋        | 10/58 [00:00<00:03, 15.37it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  21%|██        | 12/58 [00:00<00:03, 13.57it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:03, 13.57it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:03, 13.57it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:03, 13.57it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  26%|██▌       | 15/58 [00:00<00:02, 17.39it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  26%|██▌       | 15/58 [00:00<00:02, 17.39it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.26 GB):  26%|██▌       | 15/58 [00:00<00:02, 17.39it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  26%|██▌       | 15/58 [00:00<00:02, 17.39it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  26%|██▌       | 15/58 [00:00<00:02, 17.39it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  26%|██▌       | 15/58 [00:00<00:02, 17.39it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  34%|███▍      | 20/58 [00:01<00:01, 23.74it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  34%|███▍      | 20/58 [00:01<00:01, 23.74it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  34%|███▍      | 20/58 [00:01<00:01, 23.74it/s] Capturing num tokens (num_tokens=896 avail_mem=120.24 GB):  34%|███▍      | 20/58 [00:01<00:01, 23.74it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  34%|███▍      | 20/58 [00:01<00:01, 23.74it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  34%|███▍      | 20/58 [00:01<00:01, 23.74it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  43%|████▎     | 25/58 [00:01<00:01, 28.81it/s]Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:01<00:01, 28.81it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:01<00:01, 28.81it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:01<00:01, 28.81it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  43%|████▎     | 25/58 [00:01<00:01, 28.81it/s]

    Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:01<00:01, 28.81it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:01<00:00, 32.79it/s]Capturing num tokens (num_tokens=448 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:01<00:00, 32.79it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:01<00:00, 32.79it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:01<00:00, 32.79it/s]Capturing num tokens (num_tokens=352 avail_mem=120.22 GB):  52%|█████▏    | 30/58 [00:01<00:00, 32.79it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  52%|█████▏    | 30/58 [00:01<00:00, 32.79it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  60%|██████    | 35/58 [00:01<00:00, 35.92it/s]Capturing num tokens (num_tokens=288 avail_mem=120.21 GB):  60%|██████    | 35/58 [00:01<00:00, 35.92it/s]Capturing num tokens (num_tokens=256 avail_mem=120.21 GB):  60%|██████    | 35/58 [00:01<00:00, 35.92it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  60%|██████    | 35/58 [00:01<00:00, 35.92it/s]

    Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  60%|██████    | 35/58 [00:01<00:00, 35.92it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  60%|██████    | 35/58 [00:01<00:00, 35.92it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 37.68it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 37.68it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  69%|██████▉   | 40/58 [00:01<00:00, 37.68it/s]Capturing num tokens (num_tokens=160 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 37.68it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 37.68it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  69%|██████▉   | 40/58 [00:01<00:00, 37.68it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.84it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.84it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.84it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.84it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.84it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  78%|███████▊  | 45/58 [00:01<00:00, 39.84it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  86%|████████▌ | 50/58 [00:01<00:00, 40.59it/s]Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  86%|████████▌ | 50/58 [00:01<00:00, 40.59it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 40.59it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 40.59it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 40.59it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  86%|████████▌ | 50/58 [00:01<00:00, 40.59it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  95%|█████████▍| 55/58 [00:01<00:00, 40.63it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  95%|█████████▍| 55/58 [00:01<00:00, 40.63it/s]

    Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  95%|█████████▍| 55/58 [00:01<00:00, 40.63it/s] Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  95%|█████████▍| 55/58 [00:01<00:00, 40.63it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 30.21it/s]


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
    Generated text:  Vitor and I am a 20-year-old Brazilian who is very passionate about science. I am interested in the fields of astronomy and physics. I have a very strong interest in learning about the universe and how it has evolved. I also enjoy math and physics, as I believe that they are both valuable in understanding and predicting the world around us.
    I am a computer programmer and have always had a strong interest in technology. I am a skilled coder and enjoy working with the latest software and hardware. I love to learn new programming languages and technologies and I am always looking for ways to improve my skills. 
    I am also very good
    ===============================
    Prompt: The president of the United States is
    Generated text:  currently 42 years old. If the president was half as old as the president four years ago, how old would he be now? To determine the current age of the president, we need to work backwards from four years ago. Let's denote the current age of the president as \( x \).
    
    First, we know that the president was half as old as the president four years ago. So, four years ago, his age was \( x - 4 \) and four years ago, his age was \( (x - 4) - 4 = x - 8 \).
    
    According to the problem, the president's
    ===============================
    Prompt: The capital of France is
    Generated text:  ______.
    A. London
    B. Paris
    C. Shanghai
    D. New York
    Answer:
    
    B
    
    For a woman in her 40s, what type of contraceptive method is most suitable?
    A. Condoms
    B. Intrauterine device (IUD)
    C. Oral contraceptive pills
    D. Short-term oral contraceptive pills
    Answer:
    
    C
    
    When doing a blood pressure measurement, the water in the cuff should be completely removed to ensure the measurement is accurate. Determine if this statement is:
    A. Correct
    B. Incorrect
    Answer:
    
    B
    
    In the calculation of land value-added tax, the
    ===============================
    Prompt: The future of AI is
    Generated text:  in the hands of its users. Like any other technology, it has its advantages and disadvantages. Some users can use it with confidence while others find it cumbersome, even confusing. AI has its own unique characteristics that make it different from other technologies, but it also has a lot of commonalities with other technologies like machine learning, machine vision and natural language processing (NLP). In this article, we will explore the advantages and disadvantages of AI.
    Advantages of AI
    1. Automation and Efficiency
    AI can automate a wide range of tasks that would be tedious or time-consuming. This can lead to increased efficiency in various fields such as


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


    Generated text:  [Name], and I'm a [Job Title] at [Company Name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [Age], [Gender], [Nationality], [Occupation], and I have [Number] years of experience in [Field]. I'm always looking for new challenges and opportunities to grow and learn. What's your favorite hobby or activity? I enjoy [Favorite Hobby/Activity], and I'm always looking for ways to expand my horizons and try new things. What's your favorite book or movie? I love [Favorite Book/Movie
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a bustling city with a rich cultural heritage and is a major tourist destination. It is also known for its fashion industry, with Paris Fashion Week being one of the largest in the world. The city is home to many notable French artists, including Pablo Picasso and Henri Matisse. Paris is a city of contrasts, with its modern architecture and historical landmarks blending together to create a unique and fascinating city
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some of the most likely trends in AI that are expected to shape the future:
    
    1. Increased automation: One of the most significant trends in AI is the increasing automation of tasks that were previously done by humans. This could include tasks such as data analysis, decision-making, and problem-solving, as well as tasks that are typically done by machines.
    
    2. Improved privacy and security: As AI becomes more integrated into our daily lives, there will be an increasing need for privacy and security. This will likely
    


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
    Generated text:  [Your Name], and I am a software engineer with a deep passion for technology and innovation. I have been programming for over 10 years, and my experience in the field has allowed me to develop a strong technical foundation, as well as a keen eye for understanding user needs and workflows. I enjoy collaborating with cross-functional teams and translating technical concepts into practical solutions that help users achieve their goals. In my spare time, I enjoy exploring new technologies and learning new skills to expand my horizons. I am a dedicated and self-motivated individual, driven by a desire to make a positive impact on the world through my work. Thank
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, which is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French parliament building, and the city's most famous landmark, the Arc de Triomphe. Paris is a bustling city with a rich history and culture, attracting millions of visitors each year. It is a popular tourist destination, with the city being the largest city in France and one of the world's most popular tourist destinations. The city is also known for its fashion, architecture, and cuisine. Paris is a city of contrasts, with its narrow streets
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by a number of trends, some of which are already taking shape and others that are yet to be fully developed. Here are some potential trends that could shape the future of AI:
    
    1. Automation: One of the most obvious trends is the increasing automation of tasks that are currently done by humans. This could include tasks such as data analysis, customer service, and routine maintenance. By automating these tasks, AI could potentially reduce the need for human workers and free up more resources for innovation and experimentation.
    
    2. Interdisciplinary integration: AI is becoming increasingly integrated with other fields, such as natural language processing, computer vision,


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

     am

     a

     [

    job

     title

    ],

     [

    job

     title

    ]

    !

     I

    'm

     passionate

     about

     [

    job

     title

    ]

     and

     I

    'm

     excited

     to

     learn

     more

     about

     the

     field

    !

     I

     enjoy

     [

    job

     title

    ]

     because

     it

     allows

     me

     to

     [

    specific

     action

     or

     experience

    ]

     and

     I

    'm

     always

     eager

     to

     learn

     more

     about

     [

    job

     title

    ]

    !

     I

     hope

     to

     have

     fun

     with

     you

     and

     see

     you

     at

     the

     next

     [

    job

     title

    ]

    !

     Let

    's

     make

     this

     experience

     unforgettable

    !

     [

    Name

    ]

     [

    Job

     title

    ]

     (

    City

    ,

     State

    )

     

    📚

    ✍

    ️

    🌟

    ✨

    ✨

    
    


    Remember

     to

     personalize

     your

     introduction

     with

     your

     own

     unique

     perspective

     and

     experiences

    .

     Your

     introduction

     should

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     
    


    ###

     Fact

    :


    Paris

     is

     the

     largest

     city

     in

     France

     and

     the

     capital

     of

     France

    .

     It

     is

     located

     in

     the

     center

     of

     the

     country

    ,

     along

     the

     River

     Se

    ine

    ,

     on

     the

     banks

     of

     the

     River

     Mar

    ne

    ,

     near

     the

     beach

     of

     the

     Bay

     of

     Saint

    -E

    t

    ienne

    .

     Paris

     is

     known

     for

     its

     rich

     history

    ,

     iconic

     landmarks

    ,

     and

     vibrant

     culture

    .

     The

     city

     is

     also

     a

     major

     financial

     center

    ,

     home

     to

     many

     of

     the

     world

    's

     most

     prestigious

     institutions

     and

     a

     major

     tourist

     destination

    .

     Paris

     is

     a

     city

     of

     contrasts

    ,

     with

     a

     mix

     of

     traditional

     architecture

    ,

     modern

     design

    ,

     and

     French

     heritage

    .

     Its

     rich

     history

     and

     annual

     Les

     Films

     d

    '

    Or

    ,

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     be

     characterized

     by

     a

     wide

     range

     of

     trends

     and

     technologies

     that

     will

     shape

     the

     way

     we

     live

    ,

     work

    ,

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

     Increased

     automation

     and

     artificial

     intelligence

    :

     As

     automation

     and

     AI

     continue

     to

     evolve

    ,

     we

     can

     expect

     more

     automation

     in

     various

     industries

    ,

     including

     manufacturing

    ,

     transportation

    ,

     and

     customer

     service

    .

     We

     can

     also

     expect

     AI

     to

     be

     integrated

     into

     our

     daily

     lives

    ,

     from

     voice

     assistants

     to

     virtual

     assistants

     and

     self

    -driving

     cars

    .
    


    2

    .

     Improved

     healthcare

    :

     AI

     is

     expected

     to

     play

     a

     significant

     role

     in

     improving

     healthcare

     by

     analyzing

     vast

     amounts

     of

     medical

     data

     to

     identify

     patterns

     and

     predict

     disease

    .

     AI

    -powered

     diagnostics

     and

    



```python
llm.shutdown()
```
