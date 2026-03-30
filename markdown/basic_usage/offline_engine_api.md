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


    2026-03-30 14:29:19.235 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:29:19] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 14:29:19.235 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:29:19] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 14:29:19.235 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:29:19] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 14:29:19.235 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:29:19] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 14:29:19.235 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 14:29:19] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.33it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.32it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:02, 13.49it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:02, 13.49it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:02, 13.49it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:02, 13.49it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:02, 13.49it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:02, 13.49it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:02, 13.49it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:02, 13.49it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 20.03it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 20.03it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 20.03it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 20.03it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 20.03it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 20.03it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 20.03it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:02<00:01, 20.03it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 27.00it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 27.00it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 27.00it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:00, 27.00it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:00, 27.00it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:02<00:00, 27.00it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:02<00:00, 27.00it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:02<00:00, 31.94it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:02<00:00, 31.94it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 31.94it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 37.23it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 37.23it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 37.23it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 37.23it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 37.23it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 37.23it/s]

    Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 37.23it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 40.15it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 40.15it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 40.15it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 40.15it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 40.15it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 40.15it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 40.15it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 40.15it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 40.15it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 48.67it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.43it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.76 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.55it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.55it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.55it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.55it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.72it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.72it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 21.72it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 21.72it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.78it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.78it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.78it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.78it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.78it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  21%|██        | 12/58 [00:00<00:01, 29.21it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 29.21it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 29.21it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  21%|██        | 12/58 [00:00<00:01, 29.21it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  26%|██▌       | 15/58 [00:00<00:01, 27.67it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  26%|██▌       | 15/58 [00:00<00:01, 27.67it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.26 GB):  26%|██▌       | 15/58 [00:00<00:01, 27.67it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  26%|██▌       | 15/58 [00:00<00:01, 27.67it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  26%|██▌       | 15/58 [00:00<00:01, 27.67it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  26%|██▌       | 15/58 [00:00<00:01, 27.67it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.25 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.46it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.23 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.46it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.46it/s] Capturing num tokens (num_tokens=896 avail_mem=120.24 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.46it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.46it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  34%|███▍      | 20/58 [00:00<00:01, 32.46it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.50it/s]Capturing num tokens (num_tokens=704 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.50it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.50it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.50it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.50it/s]

    Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  43%|████▎     | 25/58 [00:00<00:00, 36.50it/s]Capturing num tokens (num_tokens=480 avail_mem=120.23 GB):  52%|█████▏    | 30/58 [00:00<00:00, 39.11it/s]Capturing num tokens (num_tokens=448 avail_mem=120.27 GB):  52%|█████▏    | 30/58 [00:00<00:00, 39.11it/s]Capturing num tokens (num_tokens=416 avail_mem=120.27 GB):  52%|█████▏    | 30/58 [00:00<00:00, 39.11it/s]Capturing num tokens (num_tokens=384 avail_mem=120.27 GB):  52%|█████▏    | 30/58 [00:00<00:00, 39.11it/s]Capturing num tokens (num_tokens=352 avail_mem=119.04 GB):  52%|█████▏    | 30/58 [00:00<00:00, 39.11it/s]Capturing num tokens (num_tokens=320 avail_mem=118.94 GB):  52%|█████▏    | 30/58 [00:01<00:00, 39.11it/s]Capturing num tokens (num_tokens=320 avail_mem=118.94 GB):  60%|██████    | 35/58 [00:01<00:00, 40.65it/s]Capturing num tokens (num_tokens=288 avail_mem=118.94 GB):  60%|██████    | 35/58 [00:01<00:00, 40.65it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  60%|██████    | 35/58 [00:01<00:00, 40.65it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  60%|██████    | 35/58 [00:01<00:00, 40.65it/s]

    Capturing num tokens (num_tokens=224 avail_mem=118.93 GB):  60%|██████    | 35/58 [00:01<00:00, 40.65it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  60%|██████    | 35/58 [00:01<00:00, 40.65it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  69%|██████▉   | 40/58 [00:01<00:00, 42.12it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  69%|██████▉   | 40/58 [00:01<00:00, 42.12it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  69%|██████▉   | 40/58 [00:01<00:00, 42.12it/s]Capturing num tokens (num_tokens=160 avail_mem=118.91 GB):  69%|██████▉   | 40/58 [00:01<00:00, 42.12it/s]Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  69%|██████▉   | 40/58 [00:01<00:00, 42.12it/s]Capturing num tokens (num_tokens=128 avail_mem=118.91 GB):  69%|██████▉   | 40/58 [00:01<00:00, 42.12it/s]Capturing num tokens (num_tokens=128 avail_mem=118.91 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.72it/s]Capturing num tokens (num_tokens=112 avail_mem=118.90 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.72it/s]Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.72it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.72it/s]Capturing num tokens (num_tokens=64 avail_mem=118.89 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.72it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  78%|███████▊  | 45/58 [00:01<00:00, 42.72it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  86%|████████▌ | 50/58 [00:01<00:00, 42.95it/s]Capturing num tokens (num_tokens=32 avail_mem=118.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 42.95it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 42.95it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 42.95it/s]Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  86%|████████▌ | 50/58 [00:01<00:00, 42.95it/s]Capturing num tokens (num_tokens=16 avail_mem=118.87 GB):  86%|████████▌ | 50/58 [00:01<00:00, 42.95it/s]Capturing num tokens (num_tokens=16 avail_mem=118.87 GB):  95%|█████████▍| 55/58 [00:01<00:00, 43.93it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  95%|█████████▍| 55/58 [00:01<00:00, 43.93it/s]Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  95%|█████████▍| 55/58 [00:01<00:00, 43.93it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=118.86 GB):  95%|█████████▍| 55/58 [00:01<00:00, 43.93it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:01<00:00, 37.75it/s]


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
    Generated text:  Alina and I am a computer science student at the University of Westminster, UK. I'm currently studying Computer Science and also have a passion for programming languages, particularly Python. As a software developer, I'm always looking for new challenges and learning new things to keep my skills sharp. I enjoy coding in Python and using it to solve complex problems. Additionally, I'm interested in the intersection of technology and art, and I'm always looking for ways to incorporate AI and machine learning into my work. I'm also passionate about sustainability and environmental conservation, and I believe that technology can play a crucial role in making the world a better place.
    ===============================
    Prompt: The president of the United States is
    Generated text:  a man named Trump. He resigned on January 20, 2021, becoming the first sitting president to do so since 1992. Trump left the White House after a long and controversial tenure. He left the presidency on a high note, with a 78% approval rating in the Gallup poll. Despite the success of his presidency, his allies are warning he may be removed from office again. Trump was elected in 2016, becoming the first and only president to serve two full terms. Can you name the first and only president to serve two full terms? The first and only president
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. If the number of people residing in Paris is 3,000,000, and you want to find out how many people are there in a billion (1,000,000,000) by multiplying the number of people in Paris by 3,000,000, how many people are there in a billion?
    To determine the number of people in a billion (1,000,000,000) based on the population of Paris, which is 3,000,000, we can follow these
    ===============================
    Prompt: The future of AI is
    Generated text:  being set by two of the biggest tech companies, Alphabet Inc. (GOOGL) and Microsoft Corp. (MSFT), who have been investing billions of dollars in the field to develop self-driving cars and other autonomous systems.
    Although the tech companies' investment is aimed at finding new markets for their existing offerings, they are also creating new revenue streams. These companies are on a mission to make the world a safer place by reducing traffic accidents.
    The investment is not the only reason why AI has become a hot topic in recent years. The technology has been in a bubble for years, and the public is largely unaware of its true potential.


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


    Generated text:  Paris, the city that serves as the political, cultural, and economic center of the country. It is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum, as well as its rich history and diverse cultural scene. Paris is also famous for its fashion industry, art scene, and its role in hosting major international events such as the Olympics and the World Cup. The city is a major tourist destination and is home to many famous landmarks and attractions. Paris is a vibrant and dynamic city that continues to evolve and change over time.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in several key areas, including:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior and decision-making processes.
    
    2. Enhanced machine learning capabilities: AI is likely to become more capable of learning from large amounts of data and making more accurate predictions and decisions.
    
    3. Increased focus on ethical considerations: As AI becomes more integrated into our daily lives, there will be increased focus on ethical considerations and the potential for AI to be used for good.
    
    4. Greater reliance on AI for decision-making: AI is likely to
    


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
    Generated text:  [insert your name here] and I'm a [insert your profession or role here]. I'm a software engineer, but I also enjoy reading and playing music, and I've been an avid fan of [insert your favorite artist or band]. If there's anything I enjoy about my work, it's that I enjoy coming up with creative solutions to problems, and being able to apply that knowledge to solve real-world issues. What's your favorite thing to do on a day off? Writing about writing, of course! How about you? Let's make our day off fun and memorable!
    [Your name], a [insert your profession or
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    Explanation for a five year old: Paris is the big city in France where important people work. It's called the "capital" because it's where the government makes important decisions. Paris is also very beautiful and has lots of fun things to see and do! 
    
    Is there anything else you'd like to know about Paris? Let me know if you'd like! 📸🤗
    
    #Paris #France #BigCity #Government #FunThingsToSee #Government #France #Paris #Education #Travel #FunThingsToDo #France #Education #Paris #France #Education #Paris #Education #Paris #Education
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  highly uncertain, but here are some possible trends that may occur:
    
    1. Increased automation and efficiency: As AI continues to advance, we can expect to see more automation in many areas, including manufacturing, transportation, and customer service. This could lead to increased efficiency and cost savings for businesses.
    
    2. AI and health: As AI becomes more sophisticated, we may see more use cases in healthcare, such as personalized medicine and predictive analytics for disease prevention. AI could also be used to develop new treatments for diseases that have not been fully understood.
    
    3. AI and cybersecurity: As AI systems become more sophisticated, we may see more use cases in


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

     am a

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

     have

     [

    number

     of

     years

     of

     experience

    ]

     years

     of

     experience

     in

     this

     role

    ,

     and

     I

     bring

     a

     wealth

     of

     knowledge

     and

     expertise

     in

     [

    mention

     a

     specific

     skill

     or

     area

     of

     expertise

    ].

     I

     am

     highly

     [

    int

    ention

     to

     share

     or

     ask

    ]

     your

     opinion

     on

     [

    mention

     a

     recent

     experience

     or

     accomplishment

     related

     to

     your

     skills

     or

     experience

    ].

     Welcome

     to

     the

     team

    !

     

    🌟

    
    


    ---
    


    **

    Self

    -

    Introduction

     for

     a

     fictional

     character

    **

    
    


    Hi

    ,

     my

     name

     is

     [

    Name

    ]

     and

     I

    'm

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

    've

     been

     with

     the

     company

     for

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     the

     city

     known

     as

     the

     "

    City

     of

     Love

    "

     for

     its

     romantic

     architecture

     and

     beautiful

     parks

    .

     It

     is

     located

     on

     the

     Se

    ine

     River

     and

     served

     as

     the

     seat

     of

     government

     from

     

    1

    8

    3

    0

     to

     

    1

    9

    6

    9

    ,

     before

     being

     renamed

     to

     Paris

     in

     

    1

    9

    7

    5

    .

     Paris

     is

     the

     most

     populous

     city

     in

     France

     and

     one

     of

     the

     most

     visited

     cities

     in

     the

     world

    .

     It

     is

     home

     to

     numerous

     museums

    ,

     theaters

    ,

     and

     landmarks

     such

     as

     Notre

    -D

    ame

     Cathedral

     and

     the

     E

    iff

    el

     Tower

    .

     With

     its

     rich

     history

     and

     vibrant

     culture

    ,

     Paris

     is

     a

     city

     that

     has

     captured

     the

     hearts

     of

     millions

     around

     the

     world

    .

     It

     is

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     likely

     to

     be

     characterized

     by

     significant

     growth

     and

     change

    ,

     with

     a

     number

     of

     key

     trends

     that

     are

     expected

     to

     shape

     the

     field

     in

     the

     coming

     years

    .

     Here

     are

     some

     potential

     areas

     of

     growth

     and

     change

     in

     AI

    :
    


    1

    .

     Deep

     learning

    :

     The

     use

     of

     deep

     learning

    ,

     a

     subset

     of

     machine

     learning

    ,

     is

     expected

     to

     continue

     to

     grow

     in

     importance

    .

     Deep

     learning

     algorithms

     are

     becoming

     more

     capable

     of

     handling

     complex

     tasks

     and

     have

     the

     potential

     to

     produce

     more

     accurate

     and

     efficient

     solutions

     to

     a

     wide

     range

     of

     problems

    .
    


    2

    .

     Autonomous

     vehicles

    :

     Autonomous

     vehicles

     are

     likely

     to

     become

     more

     prevalent

     in

     the

     future

    ,

     with

     many

     companies

     developing

     self

    -driving

     cars

    .

     As

     AI

     continues

     to

     improve

     and

     become

     more

    



```python
llm.shutdown()
```
