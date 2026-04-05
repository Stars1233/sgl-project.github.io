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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.74it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.73it/s]


    2026-04-05 07:52:26,058 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-05 07:52:26] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:31,  2.65s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:31,  2.65s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:31,  2.65s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:31,  2.65s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:28,  1.89it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:08,  5.78it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.01it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.01it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:02, 13.01it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:02, 13.01it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:02, 13.01it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:02, 13.01it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:02, 13.01it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:02, 13.01it/s]Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:03<00:02, 13.01it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 20.10it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 20.10it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 20.10it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 20.10it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 20.10it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 20.10it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 20.10it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:03<00:01, 20.10it/s]Compiling num tokens (num_tokens=320):  47%|████▋     | 27/58 [00:03<00:01, 20.10it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 27.74it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 27.74it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:03<00:00, 27.74it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:03<00:00, 27.74it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:03<00:00, 27.74it/s]

    Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:03<00:00, 27.74it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:03<00:00, 27.74it/s]Compiling num tokens (num_tokens=176):  60%|██████    | 35/58 [00:03<00:00, 27.74it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:03<00:00, 34.13it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:03<00:00, 34.13it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:03<00:00, 34.13it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:03<00:00, 34.13it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:03<00:00, 34.13it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:03<00:00, 34.13it/s] Compiling num tokens (num_tokens=80):  72%|███████▏  | 42/58 [00:03<00:00, 34.13it/s]Compiling num tokens (num_tokens=64):  72%|███████▏  | 42/58 [00:03<00:00, 34.13it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 40.12it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 40.12it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 40.12it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 40.12it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 40.12it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 40.12it/s]

    Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 40.12it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 40.12it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:03<00:00, 40.12it/s] Compiling num tokens (num_tokens=4):  84%|████████▍ | 49/58 [00:03<00:00, 40.12it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.50it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=71.05 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=71.05 GB):   2%|▏         | 1/58 [00:00<00:13,  4.08it/s]Capturing num tokens (num_tokens=7680 avail_mem=71.02 GB):   2%|▏         | 1/58 [00:00<00:13,  4.08it/s]Capturing num tokens (num_tokens=7168 avail_mem=71.01 GB):   2%|▏         | 1/58 [00:00<00:13,  4.08it/s]Capturing num tokens (num_tokens=6656 avail_mem=71.01 GB):   2%|▏         | 1/58 [00:00<00:13,  4.08it/s]Capturing num tokens (num_tokens=6656 avail_mem=71.01 GB):   7%|▋         | 4/58 [00:00<00:04, 11.92it/s]Capturing num tokens (num_tokens=6144 avail_mem=71.01 GB):   7%|▋         | 4/58 [00:00<00:04, 11.92it/s]Capturing num tokens (num_tokens=5632 avail_mem=71.01 GB):   7%|▋         | 4/58 [00:00<00:04, 11.92it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=71.01 GB):   7%|▋         | 4/58 [00:00<00:04, 11.92it/s]Capturing num tokens (num_tokens=5120 avail_mem=71.01 GB):  12%|█▏        | 7/58 [00:00<00:02, 17.41it/s]Capturing num tokens (num_tokens=4608 avail_mem=71.00 GB):  12%|█▏        | 7/58 [00:00<00:02, 17.41it/s]Capturing num tokens (num_tokens=4096 avail_mem=71.00 GB):  12%|█▏        | 7/58 [00:00<00:02, 17.41it/s]Capturing num tokens (num_tokens=3840 avail_mem=71.00 GB):  12%|█▏        | 7/58 [00:00<00:02, 17.41it/s]Capturing num tokens (num_tokens=3584 avail_mem=71.00 GB):  12%|█▏        | 7/58 [00:00<00:02, 17.41it/s]Capturing num tokens (num_tokens=3584 avail_mem=71.00 GB):  19%|█▉        | 11/58 [00:00<00:01, 23.81it/s]Capturing num tokens (num_tokens=3328 avail_mem=70.99 GB):  19%|█▉        | 11/58 [00:00<00:01, 23.81it/s]Capturing num tokens (num_tokens=3072 avail_mem=70.99 GB):  19%|█▉        | 11/58 [00:00<00:01, 23.81it/s]Capturing num tokens (num_tokens=2816 avail_mem=70.99 GB):  19%|█▉        | 11/58 [00:00<00:01, 23.81it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=70.98 GB):  19%|█▉        | 11/58 [00:00<00:01, 23.81it/s]Capturing num tokens (num_tokens=2304 avail_mem=70.98 GB):  19%|█▉        | 11/58 [00:00<00:01, 23.81it/s]Capturing num tokens (num_tokens=2304 avail_mem=70.98 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.87it/s]Capturing num tokens (num_tokens=2048 avail_mem=70.98 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.87it/s]Capturing num tokens (num_tokens=1792 avail_mem=70.97 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.87it/s]Capturing num tokens (num_tokens=1536 avail_mem=70.97 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.87it/s]Capturing num tokens (num_tokens=1280 avail_mem=70.97 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.87it/s]Capturing num tokens (num_tokens=1024 avail_mem=70.95 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.87it/s]Capturing num tokens (num_tokens=960 avail_mem=70.96 GB):  28%|██▊       | 16/58 [00:00<00:01, 30.87it/s] Capturing num tokens (num_tokens=960 avail_mem=70.96 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.86it/s]Capturing num tokens (num_tokens=896 avail_mem=70.96 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.86it/s]Capturing num tokens (num_tokens=832 avail_mem=70.95 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.86it/s]Capturing num tokens (num_tokens=768 avail_mem=70.95 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.86it/s]

    Capturing num tokens (num_tokens=704 avail_mem=70.95 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.86it/s]Capturing num tokens (num_tokens=640 avail_mem=70.94 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.86it/s]Capturing num tokens (num_tokens=576 avail_mem=70.94 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.86it/s]Capturing num tokens (num_tokens=576 avail_mem=70.94 GB):  48%|████▊     | 28/58 [00:00<00:00, 42.30it/s]Capturing num tokens (num_tokens=512 avail_mem=70.93 GB):  48%|████▊     | 28/58 [00:00<00:00, 42.30it/s]Capturing num tokens (num_tokens=480 avail_mem=70.95 GB):  48%|████▊     | 28/58 [00:00<00:00, 42.30it/s]Capturing num tokens (num_tokens=448 avail_mem=70.95 GB):  48%|████▊     | 28/58 [00:00<00:00, 42.30it/s]Capturing num tokens (num_tokens=416 avail_mem=70.94 GB):  48%|████▊     | 28/58 [00:00<00:00, 42.30it/s]Capturing num tokens (num_tokens=384 avail_mem=70.94 GB):  48%|████▊     | 28/58 [00:01<00:00, 42.30it/s]Capturing num tokens (num_tokens=352 avail_mem=70.94 GB):  48%|████▊     | 28/58 [00:01<00:00, 42.30it/s]Capturing num tokens (num_tokens=352 avail_mem=70.94 GB):  59%|█████▊    | 34/58 [00:01<00:00, 45.04it/s]Capturing num tokens (num_tokens=320 avail_mem=70.93 GB):  59%|█████▊    | 34/58 [00:01<00:00, 45.04it/s]Capturing num tokens (num_tokens=288 avail_mem=70.93 GB):  59%|█████▊    | 34/58 [00:01<00:00, 45.04it/s]

    Capturing num tokens (num_tokens=256 avail_mem=70.93 GB):  59%|█████▊    | 34/58 [00:01<00:00, 45.04it/s]Capturing num tokens (num_tokens=240 avail_mem=70.92 GB):  59%|█████▊    | 34/58 [00:01<00:00, 45.04it/s]Capturing num tokens (num_tokens=224 avail_mem=70.92 GB):  59%|█████▊    | 34/58 [00:01<00:00, 45.04it/s]Capturing num tokens (num_tokens=208 avail_mem=70.92 GB):  59%|█████▊    | 34/58 [00:01<00:00, 45.04it/s]Capturing num tokens (num_tokens=208 avail_mem=70.92 GB):  69%|██████▉   | 40/58 [00:01<00:00, 47.09it/s]Capturing num tokens (num_tokens=192 avail_mem=70.92 GB):  69%|██████▉   | 40/58 [00:01<00:00, 47.09it/s]Capturing num tokens (num_tokens=176 avail_mem=70.91 GB):  69%|██████▉   | 40/58 [00:01<00:00, 47.09it/s]Capturing num tokens (num_tokens=160 avail_mem=70.91 GB):  69%|██████▉   | 40/58 [00:01<00:00, 47.09it/s]Capturing num tokens (num_tokens=144 avail_mem=70.91 GB):  69%|██████▉   | 40/58 [00:01<00:00, 47.09it/s]Capturing num tokens (num_tokens=128 avail_mem=70.90 GB):  69%|██████▉   | 40/58 [00:01<00:00, 47.09it/s]Capturing num tokens (num_tokens=128 avail_mem=70.90 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.11it/s]Capturing num tokens (num_tokens=112 avail_mem=70.90 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.11it/s]

    Capturing num tokens (num_tokens=96 avail_mem=70.90 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.11it/s] Capturing num tokens (num_tokens=80 avail_mem=70.89 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.11it/s]Capturing num tokens (num_tokens=64 avail_mem=70.89 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.11it/s]Capturing num tokens (num_tokens=48 avail_mem=70.89 GB):  78%|███████▊  | 45/58 [00:01<00:00, 45.11it/s]Capturing num tokens (num_tokens=48 avail_mem=70.89 GB):  86%|████████▌ | 50/58 [00:01<00:00, 44.16it/s]Capturing num tokens (num_tokens=32 avail_mem=70.89 GB):  86%|████████▌ | 50/58 [00:01<00:00, 44.16it/s]Capturing num tokens (num_tokens=28 avail_mem=70.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 44.16it/s]Capturing num tokens (num_tokens=24 avail_mem=70.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 44.16it/s]Capturing num tokens (num_tokens=20 avail_mem=70.87 GB):  86%|████████▌ | 50/58 [00:01<00:00, 44.16it/s]Capturing num tokens (num_tokens=16 avail_mem=70.87 GB):  86%|████████▌ | 50/58 [00:01<00:00, 44.16it/s]

    Capturing num tokens (num_tokens=16 avail_mem=70.87 GB):  95%|█████████▍| 55/58 [00:01<00:00, 43.59it/s]Capturing num tokens (num_tokens=12 avail_mem=70.87 GB):  95%|█████████▍| 55/58 [00:01<00:00, 43.59it/s]Capturing num tokens (num_tokens=8 avail_mem=76.61 GB):  95%|█████████▍| 55/58 [00:01<00:00, 43.59it/s] Capturing num tokens (num_tokens=4 avail_mem=76.61 GB):  95%|█████████▍| 55/58 [00:01<00:00, 43.59it/s]Capturing num tokens (num_tokens=4 avail_mem=76.61 GB): 100%|██████████| 58/58 [00:01<00:00, 34.51it/s]


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
    Generated text:  Jake, I am a good friend of yours, and I am a student at the University of California, Berkeley. I am the director of a project in the field of computer vision. My work involves developing computer vision algorithms that enable humans to better understand the world around them.
    
    I have a particularly important and challenging task, one that I have spent hours working on. I am currently trying to develop a computer vision algorithm that will enable humans to better understand and appreciate the beauty of nature. This project is called "NatureWise," and it involves using advanced computer vision techniques to analyze images and videos, and then use this information to identify and
    ===============================
    Prompt: The president of the United States is
    Generated text:  6 feet 2 inches tall. If the height of the building in question is 500 feet, how much taller is the president than the building?
    To determine how much taller the president is compared to the building, we need to follow these steps:
    
    1. Convert the height of the president from feet and inches to just inches.
    2. Subtract the height of the president from the height of the building.
    
    First, let's convert the president's height from feet and inches to just inches. The president is 6 feet and 2 inches tall. Since there are 12 inches in a foot, we can calculate the
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. ____
    A. 正确
    B. 错误
    答案: B
    
    未满16周岁的未成年人可以到银行办理挂失业务。
    A. 正确
    B. 错误
    答案: B
    
    原则上,客户大额的存款需要提供大额存单或国债证明等佐证材料。
    A. 正确
    B. 错误
    答案: A
    
    持有人对银行业金融机构做出的有关收缴或鉴定假币的具体行政行为有异议,可自收到假币的_______个工作日内向直接监管该银行业金融机构的中国人民银行分支机构申请
    ===============================
    Prompt: The future of AI is
    Generated text:  very exciting and has the potential to change everything from how we live and work to how we interact with each other. However, as with any technology, there are also risks and challenges that need to be taken into consideration. One of the most significant risks is the potential for bias in AI systems, which could lead to unfair or discriminatory outcomes.
    AI systems are not inherently biased, but they can be influenced by factors such as data bias, training data, and the ethical and legal frameworks in which they are used. When these factors are not taken into account, AI systems can perpetuate biases and discrimination, leading to unfair outcomes.
    To mitigate this


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


    Generated text:  [Name], and I'm a [Age] year old [Occupation]. I'm a [Type of Character] who has always been [What motivates you to be a [Type of Character]].
    
    I'm a [Type of Character] because [Why you are a [Type of Character]]. I'm a [Type of Character] because [Why you are a [Type of Character]]. I'm a [Type of Character] because [Why you are a [Type of Character]]. I'm a [Type of Character] because [Why you are a [Type of Character]]. I'm a [Type of Character] because [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major cultural and economic center, hosting numerous museums, theaters, and other attractions. Paris is known for its rich history, including the influence of French culture and literature, and its role in shaping modern France. It is a popular tourist destination and a major economic power in Europe. The city is also home to many notable French artists and writers, including Pablo Picasso and André Breton. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly. Its status as
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the development of the technology in the coming years. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased Use of AI in Healthcare: AI is expected to play a significant role in healthcare in the coming years. AI can be used to improve the accuracy and speed of diagnosis, reduce the cost of treatments, and improve patient outcomes. AI can also be used to develop new treatments for diseases that are currently difficult to treat.
    
    2. Increased Use of AI in Finance: AI is expected to play a significant role in finance in
    


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
    Generated text:  [Name] and I'm a [insert your profession here]. I'm here to meet you, and to be honest, I'm a little surprised at you here, but I have the best intentions of hearing you out and learning about you. How can I assist you today? [What do you need or want?]
    
    Remember, every interaction with me is a learning experience. So, please feel free to share your story, your problems, and your goals. I am here to listen and help you find solutions. [Start your conversation]
    
    I am a [insert your profession here]. I am here to meet you, and to be
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    Paris is the largest city in France and the most populous city in the European Union. It is known for its rich history, beautiful architecture, and vibrant culture. The city is home to many iconic landmarks such as the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. Paris is also known for its annual celebrations such as the Marne-la-Vallée Race and the annual Les Trois Rivas Festival. With its many museums, art galleries, and shopping districts, Paris is a popular tourist destination for people from all over the world. Paris is an important cultural and historical center in France and the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be characterized by a combination of rapid innovation, advancements in technology, and increased adoption by businesses and individuals. Here are some possible future trends in AI:
    
    1. Increased automation and artificial intelligence in industries: As AI technology continues to advance, it is expected that automation and AI will become more prevalent in various industries. This could lead to significant job displacement, but it could also create new job opportunities and opportunities for innovative solutions.
    
    2. Development of more sophisticated AI systems: As AI technology continues to evolve, it is expected that more sophisticated AI systems will be developed, leading to a more accurate and efficient decision-making process. This could lead


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

     [

    Your

     Age

    ]

     years

     old

    .

     I

     am

     a

     [

    Your

     Profession

     or

     Hobby

    ]

     with

     a

     passion

     for

     [

    Your

     Special

     Interest

     or

     Area

     of

     Expert

    ise

    ].

     My

     experience

     ranges

     from

     [

    Number

     of

     years

     in

     the

     Industry

    ]

     years

     of

     experience

    ,

     and

     I

     am

     [

    Your

     Personality

     or

     Character

     Traits

    ].

     I

     am

     always

     ready

     to

     learn

     and

     constantly

     improve

    .

     I

     love

     [

    Your

     Job

     Title

    ]

     and

     I

     am

     passionate

     about

     [

    Your

     Areas

     of

     Expert

    ise

     or

     Interest

    ].

     I

     am

     excited

     to

     meet

     you

     and

     learn

     more

     about

     your

     journey

     so

     far

    .

     Let

    's

     get

     to

     know

     each

     other

    !

     [

    Name

    ]

     [

    Job

     Title

    ]

     [

    Area

     of

     Expert

    ise

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     the

     city

     that

     serves

     as

     the

     cultural

    ,

     economic

    ,

     and

     political

     center

     of

     the

     nation

    .

     Here

    ,

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

     the

     Notre

     Dame

     Cathedral

    ,

     and

     many

     other

     historical

     and

     modern

     landmarks

     can

     be

     found

    .

     Paris

     is

     known

     for

     its

     architecture

    ,

     its

     world

    -ren

    owned

     art

     and

     literature

     scene

    ,

     and

     its

     rich

     history

    .

     It

     is

     a

     major

     transportation

     hub

     and

     a

     popular

     tourist

     destination

    .

     The

     city

     has

     a

     diverse

     population

     with

     people

     of

     various

     national

    ities

     and

     ethnic

    ities

    .

     Despite

     its

     modern

     and

     skys

    craper

    -filled

     skyline

    ,

     Paris

     still

     retains

     its

     medieval

     and

     historical

     character

    .

     It

     is

     considered

     one

     of

     the

     most

     important

     cities

     in

     the

     world

     and

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     exciting

    ,

     and

     there

     are

     many

     potential

     trends

     and

     advancements

     that

     we

     can

     expect

     to

     see

     in

     the

     coming

     years

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

     AI

     research

     and

     development

    :
    


    1

    .

     **

    Increased

     Integration

     with

     Human

     Experts

    **:

     AI

     is

     becoming

     more

     integrated

     with

     human

     experts

    ,

     leading

     to

     more

     accurate

     and

     reliable

     outcomes

    .

     AI

     systems

     are

     learning

     from

     feedback

    ,

     and

     this

     feedback

     is

     often

     incorporated

     into

     AI

     training

     data

    .

     As

     AI

     systems

     learn

     from

     humans

    ,

     they

     can

     provide

     more

     nuanced

     and

     context

    -aware

     answers

    .
    


    2

    .

     **

    Ad

    aptive

     Learning

    **:

     AI

     is

     becoming

     more

     adaptive

    ,

     able

     to

     learn

     from

     new

     data

     and

     improve

     over

     time

    .

     This

     adaptive

     learning

     is

     crucial

     for

     applications

     like

    



```python
llm.shutdown()
```
