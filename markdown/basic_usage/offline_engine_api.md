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

    Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  4.46it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  4.46it/s]


    2026-04-04 15:22:52,784 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 15:22:52] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:33,  2.70s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:33,  2.70s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:33,  2.70s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:33,  2.70s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:33,  2.70s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.15it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.15it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.15it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.15it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:07,  6.15it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:07,  6.15it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:07,  6.15it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:07,  6.15it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  6.15it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.41it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.41it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.41it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.41it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.41it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.41it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.41it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.41it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.45it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 25.02it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 25.02it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 25.02it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 25.02it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 25.02it/s]

    Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 25.02it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 25.02it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 34.84it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 34.84it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 34.84it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 34.84it/s]

    Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 34.84it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 34.84it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 34.84it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 38.89it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 38.89it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 38.89it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 38.89it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 38.89it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 38.89it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 38.89it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 38.89it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.79it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.05 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.02 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:03, 18.27it/s]Capturing num tokens (num_tokens=7168 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:03, 18.27it/s]Capturing num tokens (num_tokens=6656 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:03, 18.27it/s]Capturing num tokens (num_tokens=6144 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:03, 18.27it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=119.02 GB):   9%|▊         | 5/58 [00:00<00:02, 21.61it/s]Capturing num tokens (num_tokens=5632 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:00<00:02, 21.61it/s]Capturing num tokens (num_tokens=5120 avail_mem=119.02 GB):   9%|▊         | 5/58 [00:00<00:02, 21.61it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:00<00:02, 21.61it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:00<00:02, 21.61it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.01 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.54it/s]Capturing num tokens (num_tokens=3840 avail_mem=119.01 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.54it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.54it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.54it/s]Capturing num tokens (num_tokens=3072 avail_mem=119.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.54it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=119.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.54it/s]Capturing num tokens (num_tokens=2816 avail_mem=119.00 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.32it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.99 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.32it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.99 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.32it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.98 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.32it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.98 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.32it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.97 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.32it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.97 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.50it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.97 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.50it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.95 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.50it/s]Capturing num tokens (num_tokens=960 avail_mem=118.96 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.50it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=118.96 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.50it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.50it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.39it/s]Capturing num tokens (num_tokens=768 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.39it/s]Capturing num tokens (num_tokens=704 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.39it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.39it/s]Capturing num tokens (num_tokens=576 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.39it/s]Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.39it/s]Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  50%|█████     | 29/58 [00:00<00:00, 41.35it/s]Capturing num tokens (num_tokens=480 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:00<00:00, 41.35it/s]Capturing num tokens (num_tokens=448 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:00<00:00, 41.35it/s]Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:00<00:00, 41.35it/s]

    Capturing num tokens (num_tokens=384 avail_mem=118.94 GB):  50%|█████     | 29/58 [00:00<00:00, 41.35it/s]Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  50%|█████     | 29/58 [00:00<00:00, 41.35it/s]Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.28it/s]Capturing num tokens (num_tokens=320 avail_mem=118.94 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.28it/s]Capturing num tokens (num_tokens=288 avail_mem=118.93 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.28it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.28it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.28it/s]Capturing num tokens (num_tokens=224 avail_mem=118.92 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.28it/s]Capturing num tokens (num_tokens=224 avail_mem=118.92 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.22it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.22it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.22it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.22it/s]

    Capturing num tokens (num_tokens=160 avail_mem=118.91 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.22it/s]Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.22it/s]Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.06it/s]Capturing num tokens (num_tokens=128 avail_mem=118.91 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.06it/s]Capturing num tokens (num_tokens=112 avail_mem=118.90 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.06it/s]Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.06it/s] Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.06it/s]Capturing num tokens (num_tokens=64 avail_mem=118.89 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.06it/s]Capturing num tokens (num_tokens=64 avail_mem=118.89 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.29it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.29it/s]Capturing num tokens (num_tokens=32 avail_mem=118.88 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.29it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.29it/s]

    Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.29it/s]Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.29it/s]Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.72it/s]Capturing num tokens (num_tokens=16 avail_mem=118.87 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.72it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.72it/s]Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.72it/s] Capturing num tokens (num_tokens=4 avail_mem=118.86 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.72it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:01<00:00, 39.93it/s]


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
    Generated text:  Mariana, 19 years old, and I have a 7500mAh battery. I am looking to upgrade my bike to an electric bike. Is there any battery upgrade that I could do to increase the battery's capacity? I understand that my battery can currently provide 100 miles of range. I am currently using an average of 40 miles on my regular rides. I would like to increase the capacity of my battery. I have two options. Is it possible to purchase a battery with a higher capacity? Or should I buy a battery with more cells? Or is there a specific type of battery that
    ===============================
    Prompt: The president of the United States is
    Generated text:  a political office. He serves in the position for a term of four years. This is the highest office in the United States. The president is also the leader of the executive branch of the government.
    
    The president is the head of the executive branch of the government. The other branches of the government include the legislature, the judiciary, and the executive branch.
    
    The president can make important decisions on the country's economic policies and plans. He has the power to grant pardons, declare war, declare a national emergency, and issue executive orders. He also appoints ambassadors, justices of the Supreme Court, and other important government officials.
    
    The president
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris.
    
    A. 正确
    
    B. 错误
    
    答案：B
    
    解析：巴黎（Pariament）是法国的首都，位于法国东南部的巴黎塞纳河畔，是法国的“政治、文化、艺术、教育、科学和体育中心”。巴黎是法国的首都和重镇，也是世界上最有名的城市之一。巴黎以其丰富的历史文化遗产、浪漫的时尚文化、美食和艺术活动而闻名。巴黎的市内广场、埃菲尔铁塔、卢浮宫、圣母院、凡尔赛宫等著名景点吸引了无数游客。此外，巴黎也是法国
    ===============================
    Prompt: The future of AI is
    Generated text:  in the hands of the people. I’m talking to David, the CEO of the world’s leading AI company, about how to harness the power of AI to benefit society, not just benefit the executives who work for them. David shares his thoughts on the ethical implications of AI, the role of the government in the AI landscape, and how AI is being used in the world today. He also talks about the importance of AI in healthcare, education, and transportation, and how AI is being integrated into the world’s top companies. Tune in to learn more about the future of AI and how it is shaping the future of society. Welcome,


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


    Generated text:  Paris, the city that serves as the political, cultural, and economic center of the country. It is known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Arc de Triomphe. Paris is also famous for its rich history, including the French Revolution and the French Revolution Monument. The city is home to many famous museums, including the Louvre and the Musée d'Orsay, as well as the Notre-Dame Cathedral. Paris is a vibrant and diverse city with a rich cultural scene, and it is a popular tourist destination. The city is also known for
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn and adapt to human behavior and preferences. This could lead to more personalized and efficient AI systems.
    
    2. Greater emphasis on ethical considerations: As AI becomes more prevalent in our lives, there will be a greater emphasis on ethical considerations, such as privacy, bias, and accountability. This could lead to more rigorous testing and regulation of AI systems.
    
    3. Greater use of AI in healthcare:
    


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
    Generated text:  [name], and I am a [introduction]. Before I started working, I was a [colleague's job title]. But I've always been [character's past]. In my spare time, I enjoy [aspect of my hobby]. Thanks for asking! [name]... I'm here to help you find the perfect [job title] and to assist you with any questions you might have. I'm [character's name]. Can I talk to you for a moment? [name]... I'm here to help you find the perfect [job title] and to assist you with any questions you might have. I'm [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. It is a historic, cultural, and cosmopolitan city located in the south of the country. The city is home to the French Parliament, the Louvre Museum, and the Eiffel Tower, among other landmarks. France's capital is known for its rich history, vibrant culture, and cosmopolitan atmosphere. It is the largest city in France by population and the fourth largest city in the world by land area. Paris is also known for its diverse food culture, fashion, and music scenes. The city's status as the capital of France has made it a major hub for the country's political, economic, and cultural life.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be shaped by a number of trends and developments, including:
    
    1. Enhanced accuracy and precision in machine learning algorithms: One of the biggest challenges facing AI is getting it to perform accurately and precisely. Future advancements in machine learning will likely lead to more powerful and accurate models, as well as more sophisticated ways of training them to handle complex data and varying inputs.
    
    2. Integration with other fields: AI is already being used in a wide range of fields, from healthcare and finance to transportation and entertainment. As the technology continues to evolve, we can expect to see more integration with other industries, leading to new applications and business opportunities.
    
    3


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

    ],

     and

     I

     am

     a

     [

    Your

     Profession

    ]

     with

     a

     passion

     for

     [

    Your

     field

     of

     interest

    ].

     I

     am

     always

     looking

     for

     ways

     to

     expand

     my

     skills

     and

     knowledge

    ,

     and

     I

     am

     eager

     to

     learn

     more

     about

     [

    Your

     profession

    ]

     and

     get

     involved

     in

     [

    Your

     interests

    ,

     hobbies

    ,

     or

     community

     activities

    ].

     Whether

     it

    's

     through

     engaging

     with

     my

     fellow

     enthusiasts

    ,

     attending

     conferences

    ,

     or

     volunteering

    ,

     I

     am

     always

     looking

     to

     contribute

     to

     the

     growth

     and

     success

     of

     [

    Your

     professional

     field

    ].

     Thank

     you

     for

     asking

    !

     How

     can

     I

     assist

     you

     today

    ?

     [

    Your

     Name

    ]

     [

    Your

     Profession

    ]

     Hello

    !

     How

     can

     I

     assist

     you

     today

    ?

     [

    Your

     Name

    ]

     [

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     the

     city

     where

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

     Arc

     de

     Tri

    omp

    he

     are

     located

    .

     It

     is

     also

     known

     as

     the

     "

    City

     of

     Light

    "

     and

     the

     "

    City

     of

     Arts

     and

     Letters

    ."

     Paris

     is

     the

     third

     most

     populous

     city

     in

     the

     world

    ,

     and

     is

     home

     to

     millions

     of

     people

     from

     around

     the

     world

     who

     come

     to

     enjoy

     its

     world

    -class

     museums

    ,

     cafes

    ,

     and

     food

     scene

    .

     As

     the

     heart

     of

     the

     French

     cultural

     and

     political

     center

    ,

     it

     plays

     a

     significant

     role

     in

     French

     culture

     and

     politics

    .

     Paris

     has

     been

     a

     hub

     of

     culture

     and

     industry

     since

     the

     

    1

    2

    th

     century

    ,

     and

     its

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     shaped

     by

     a

     complex

     inter

    play

     of

     technological

     advancements

    ,

     societal

     changes

    ,

     and

     ethical

     considerations

    .

     Here

     are

     some

     potential

     trends

     that

     are

     shaping

     the

     future

     of

     AI

    :
    


    1

    .

     Increased

     focus

     on

     ethical

     and

     social

     issues

    :

     AI

     systems

     are

     becoming

     more

     complex

     and

     more

     personalized

    ,

     and

     their

     development

     and

     deployment

     are

     increasingly

     guided

     by

     ethical

     and

     social

     concerns

    ,

     such

     as

     privacy

    ,

     bias

    ,

     and

     accountability

    .

     As

     a

     result

    ,

     there

     will

     be

     increased

     pressure

     on

     AI

     developers

     and

     researchers

     to

     consider

     the

     broader

     implications

     of

     their

     technologies

     on

     society

    .
    


    2

    .

     More

     emphasis

     on

     AI

     for

     human

     enhancement

    :

     As

     AI

     systems

     become

     more

     advanced

     and

     capable

    ,

     their

     potential

     for

     human

     enhancement

     will

     likely

     increase

    .

     This

     could

    



```python
llm.shutdown()
```
