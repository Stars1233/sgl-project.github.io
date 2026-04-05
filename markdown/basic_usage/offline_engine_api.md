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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.02it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.00it/s]


    2026-04-05 09:48:25,619 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-05 09:48:25] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:22,  2.35it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:07,  6.18it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 12.51it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 12.51it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.51it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.51it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.51it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.51it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.51it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.51it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.57it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.57it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.57it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.57it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.57it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.57it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.57it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.57it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 25.18it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 25.18it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 25.18it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 25.18it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 25.18it/s]

    Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 25.18it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 25.18it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 30.22it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 30.22it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 30.22it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 30.22it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 30.22it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 30.22it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 30.22it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 35.12it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 35.12it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 35.12it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 35.12it/s]

    Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 35.12it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 35.12it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 35.12it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 39.30it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 39.30it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 39.30it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 39.30it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 39.30it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 39.30it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 39.30it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 39.30it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.89it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=121.74 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.71 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=121.71 GB):   3%|▎         | 2/58 [00:00<00:02, 18.83it/s]Capturing num tokens (num_tokens=7168 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:02, 18.83it/s]Capturing num tokens (num_tokens=6656 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:02, 18.83it/s]Capturing num tokens (num_tokens=6144 avail_mem=121.70 GB):   3%|▎         | 2/58 [00:00<00:02, 18.83it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 21.51it/s]Capturing num tokens (num_tokens=5632 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 21.51it/s]Capturing num tokens (num_tokens=5120 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 21.51it/s]Capturing num tokens (num_tokens=4608 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 21.51it/s]Capturing num tokens (num_tokens=4096 avail_mem=121.70 GB):   9%|▊         | 5/58 [00:00<00:02, 21.51it/s]Capturing num tokens (num_tokens=4096 avail_mem=121.70 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.52it/s]Capturing num tokens (num_tokens=3840 avail_mem=121.69 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.52it/s]Capturing num tokens (num_tokens=3584 avail_mem=121.69 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.52it/s]Capturing num tokens (num_tokens=3328 avail_mem=121.69 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.52it/s]Capturing num tokens (num_tokens=3072 avail_mem=121.68 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.52it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=121.68 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.52it/s]Capturing num tokens (num_tokens=2816 avail_mem=121.68 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.16it/s]Capturing num tokens (num_tokens=2560 avail_mem=121.68 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.16it/s]Capturing num tokens (num_tokens=2304 avail_mem=121.67 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.16it/s]Capturing num tokens (num_tokens=2048 avail_mem=121.67 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.16it/s]Capturing num tokens (num_tokens=1792 avail_mem=121.67 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.16it/s]Capturing num tokens (num_tokens=1536 avail_mem=121.66 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.16it/s]Capturing num tokens (num_tokens=1536 avail_mem=121.66 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.97it/s]Capturing num tokens (num_tokens=1280 avail_mem=121.66 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.97it/s]Capturing num tokens (num_tokens=1024 avail_mem=121.64 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.97it/s]Capturing num tokens (num_tokens=960 avail_mem=121.65 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.97it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=121.65 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.97it/s]Capturing num tokens (num_tokens=832 avail_mem=121.65 GB):  33%|███▎      | 19/58 [00:00<00:01, 35.97it/s]Capturing num tokens (num_tokens=832 avail_mem=121.65 GB):  41%|████▏     | 24/58 [00:00<00:00, 38.77it/s]Capturing num tokens (num_tokens=768 avail_mem=121.64 GB):  41%|████▏     | 24/58 [00:00<00:00, 38.77it/s]Capturing num tokens (num_tokens=704 avail_mem=121.64 GB):  41%|████▏     | 24/58 [00:00<00:00, 38.77it/s]Capturing num tokens (num_tokens=640 avail_mem=121.64 GB):  41%|████▏     | 24/58 [00:00<00:00, 38.77it/s]Capturing num tokens (num_tokens=576 avail_mem=121.64 GB):  41%|████▏     | 24/58 [00:00<00:00, 38.77it/s]Capturing num tokens (num_tokens=512 avail_mem=121.62 GB):  41%|████▏     | 24/58 [00:00<00:00, 38.77it/s]Capturing num tokens (num_tokens=512 avail_mem=121.62 GB):  50%|█████     | 29/58 [00:00<00:00, 40.70it/s]Capturing num tokens (num_tokens=480 avail_mem=121.64 GB):  50%|█████     | 29/58 [00:00<00:00, 40.70it/s]Capturing num tokens (num_tokens=448 avail_mem=121.36 GB):  50%|█████     | 29/58 [00:00<00:00, 40.70it/s]

    Capturing num tokens (num_tokens=416 avail_mem=121.36 GB):  50%|█████     | 29/58 [00:00<00:00, 40.70it/s]Capturing num tokens (num_tokens=384 avail_mem=121.27 GB):  50%|█████     | 29/58 [00:00<00:00, 40.70it/s]Capturing num tokens (num_tokens=352 avail_mem=120.65 GB):  50%|█████     | 29/58 [00:00<00:00, 40.70it/s]Capturing num tokens (num_tokens=352 avail_mem=120.65 GB):  59%|█████▊    | 34/58 [00:00<00:00, 41.26it/s]Capturing num tokens (num_tokens=320 avail_mem=120.65 GB):  59%|█████▊    | 34/58 [00:00<00:00, 41.26it/s]Capturing num tokens (num_tokens=288 avail_mem=120.64 GB):  59%|█████▊    | 34/58 [00:00<00:00, 41.26it/s]Capturing num tokens (num_tokens=256 avail_mem=120.64 GB):  59%|█████▊    | 34/58 [00:00<00:00, 41.26it/s]Capturing num tokens (num_tokens=240 avail_mem=120.64 GB):  59%|█████▊    | 34/58 [00:01<00:00, 41.26it/s]Capturing num tokens (num_tokens=224 avail_mem=120.64 GB):  59%|█████▊    | 34/58 [00:01<00:00, 41.26it/s]Capturing num tokens (num_tokens=224 avail_mem=120.64 GB):  67%|██████▋   | 39/58 [00:01<00:00, 42.34it/s]Capturing num tokens (num_tokens=208 avail_mem=120.63 GB):  67%|██████▋   | 39/58 [00:01<00:00, 42.34it/s]

    Capturing num tokens (num_tokens=192 avail_mem=120.63 GB):  67%|██████▋   | 39/58 [00:01<00:00, 42.34it/s]Capturing num tokens (num_tokens=176 avail_mem=120.63 GB):  67%|██████▋   | 39/58 [00:01<00:00, 42.34it/s]Capturing num tokens (num_tokens=160 avail_mem=120.62 GB):  67%|██████▋   | 39/58 [00:01<00:00, 42.34it/s]Capturing num tokens (num_tokens=144 avail_mem=120.62 GB):  67%|██████▋   | 39/58 [00:01<00:00, 42.34it/s]Capturing num tokens (num_tokens=144 avail_mem=120.62 GB):  76%|███████▌  | 44/58 [00:01<00:00, 41.35it/s]Capturing num tokens (num_tokens=128 avail_mem=120.62 GB):  76%|███████▌  | 44/58 [00:01<00:00, 41.35it/s]Capturing num tokens (num_tokens=112 avail_mem=120.62 GB):  76%|███████▌  | 44/58 [00:01<00:00, 41.35it/s]Capturing num tokens (num_tokens=96 avail_mem=120.61 GB):  76%|███████▌  | 44/58 [00:01<00:00, 41.35it/s] Capturing num tokens (num_tokens=80 avail_mem=120.61 GB):  76%|███████▌  | 44/58 [00:01<00:00, 41.35it/s]Capturing num tokens (num_tokens=64 avail_mem=120.60 GB):  76%|███████▌  | 44/58 [00:01<00:00, 41.35it/s]

    Capturing num tokens (num_tokens=64 avail_mem=120.60 GB):  84%|████████▍ | 49/58 [00:01<00:00, 42.15it/s]Capturing num tokens (num_tokens=48 avail_mem=120.60 GB):  84%|████████▍ | 49/58 [00:01<00:00, 42.15it/s]Capturing num tokens (num_tokens=32 avail_mem=120.60 GB):  84%|████████▍ | 49/58 [00:01<00:00, 42.15it/s]Capturing num tokens (num_tokens=28 avail_mem=120.59 GB):  84%|████████▍ | 49/58 [00:01<00:00, 42.15it/s]Capturing num tokens (num_tokens=24 avail_mem=120.59 GB):  84%|████████▍ | 49/58 [00:01<00:00, 42.15it/s]Capturing num tokens (num_tokens=20 avail_mem=120.26 GB):  84%|████████▍ | 49/58 [00:01<00:00, 42.15it/s]Capturing num tokens (num_tokens=20 avail_mem=120.26 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.20it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.20it/s]Capturing num tokens (num_tokens=12 avail_mem=120.16 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.20it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.20it/s] Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.20it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 38.90it/s]


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
    Generated text:  Ehab Ismail. I'm 20 years old. I am from the USA. I just started a new job in a Chinese company. I'm not well enough to write here. Please let me know if you have any questions or not.
    
    I'd like to ask you some questions to find out what's really going on in your life. You can choose the questions you want to ask. Please start writing.
    
    I'm really happy with my life. I like to read, see movies, watch TV, eat good food, go to my favorite restaurants. I have a friend who is also a Chinese-American. I don't
    ===============================
    Prompt: The president of the United States is
    Generated text:  seeking a recommendation from the members of the American Psychological Association (APA) regarding the use of certain mental health professionals. The APA offers a list of professionals that the president can select from, and each professional can only be selected by the president if they have a prior history of providing mental health services. If the president wants to select a professional who has a history of providing mental health services and is also available for work, how many unique recommendations can the president make?
    
    To determine the number of unique recommendations the president can make, we need to consider the following:
    
    1. There are 7 professionals in the list of mental health professionals offered by the
    ===============================
    Prompt: The capital of France is
    Generated text: :
    A. Paris
    B. Lyon
    C. London
    D. Moscow
    Answer: A
    
    When a customer wishes to modify the payment terms for their credit card, what should the staff do?
    A. Only serve them with a polite reminder, and wait for them to arrive at the counter.
    B. Immediately ask the customer to contact their bank to ensure that the transaction is processed.
    C. Maintain a polite tone of voice and wait for them to arrive at the counter with their documents.
    D. Politely tell them to contact their bank to change the payment terms.
    Answer: C
    
    Which of the following options best represents the
    ===============================
    Prompt: The future of AI is
    Generated text:  growing every day. AI is transforming the way we live, work, and think. The potential benefits of AI include increased efficiency, better decision-making, and improved customer experiences. However, the potential risks of AI also exist, such as job displacement, privacy concerns, and ethical issues.
    One of the most significant risks of AI is the potential for job displacement. AI is currently being used in many industries, including healthcare, finance, and manufacturing. As AI continues to become more advanced, some jobs will be replaced by machines that can perform tasks more efficiently and accurately. This could lead to a decrease in the number of jobs available in certain industries


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


    Generated text:  [Name], and I'm a [Age] year old [Occupation]. I'm a [Skill/Ability] who has been [Career] for [Number of Years] years. I'm a [Favorite Activity] that I enjoy doing every day. I'm a [Personality Trait] who is [Positive/Neutral/Negative] about life. I'm [What I'm Looking for in a Partner]. I'm [What I'm Looking for in a Job]. I'm [What I'm Looking for in a Friend]. I'm [What I'm Looking for in a Family]. I'm [What I'm Looking for
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light, a historic city with a rich history and diverse culture. It is the largest city in France and the second-largest city in the European Union, with a population of over 2. 5 million people. Paris is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and Louvre Museum, as well as its vibrant arts scene and culinary delights. The city is also home to many museums, theaters, and other cultural institutions, making it a popular destination for tourists and locals alike. Paris is a city of contrasts, with its modern architecture and historical landmarks
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends that could be expected in the future:
    
    1. Increased integration of AI into everyday life: As AI becomes more integrated into our daily lives, we may see more widespread adoption of AI-powered technologies like voice assistants, self-driving cars, and virtual assistants. This could lead to a more seamless and efficient use of technology, and could also lead to new opportunities for businesses to innovate and create new products and services.
    
    2. Greater emphasis on ethical and responsible AI: As AI becomes more integrated into
    


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
    Generated text:  [Name], and I'm an experienced software developer with a passion for creating user-friendly and efficient software applications. I love coding in Python and can develop complex algorithms and data structures to solve problems effectively. I'm always looking to learn new technologies and stay up-to-date with the latest advancements in software development. I'm also very friendly and enjoy engaging with others, whether it's through coding challenges or coffee breaks. Thanks for considering me for a job! [Name]...
    I am a software developer with 5+ years of experience in creating efficient and user-friendly software applications. I am proficient in Python and have developed numerous successful projects in this
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    That's correct. Paris is the capital city of France and serves as its political, cultural, and economic center. It's home to the Eiffel Tower, the Louvre Museum, Notre Dame Cathedral, and many other iconic landmarks. The city is also known as "La Rose de l'Ouest" (the Rose of the West) due to its rose-colored flowers, and has a rich history dating back to ancient times. Paris is a unique and beautiful city that's renowned for its art, culture, and food. Its stunning architecture, rich history, and diverse lifestyle make it one of the most popular destinations in the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be characterized by rapid and significant advancements in several key areas. Here are some possible trends that could shape AI in the coming years:
    
    1. AI will continue to evolve and become more capable: One of the primary goals of AI research is to enhance and improve the capabilities of machines. As we continue to develop new algorithms and machine learning techniques, we can expect AI to become even more capable and efficient.
    
    2. AI will be integrated into everyday life: With the increasing adoption of AI in various sectors, such as healthcare, transportation, and manufacturing, we can expect AI to become more integrated into everyday life. AI-powered systems will be


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

    'm

     a

     [

    Job

     Title

    ]

     at

     [

    Company

     Name

    ].

     In

     my

     role

     as

     a

     [

    Role

    ],

     I

     strive

     to

     [

    Describe

     Your

     Goal

     or

     Mission

    ].

     I

    'm

     always

     looking

     for

     opportunities

     to

     [

    Provide

     an

     action

     or

     activity

    ].

     If

     you

     have

     any

     questions

     or

     would

     like

     to

     discuss

     my

     background

    ,

     please

     don

    't

     hesitate

     to

     reach

     out

    .

     Let

    's

     connect

     and

     explore

     how

     we

     can

     collaborate

     to

     [

    Describe

     Your

     Project

     or

     Goal

    ].

     [

    Name

    ]

     looks

     forward

     to

     working

     with

     you

     and

     learning

     more

     about

     our

     shared

     interests

     and

     goals

    .

     How

     can

     I

     get

     to

     know

     you

     better

    ?

     [

    Name

    ],

     how

     are

     you

     doing

     today

    ?

     What

     brings

     you

     to

     [

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     Its

     main

     landmarks

     include

     the

     E

    iff

    el

     Tower

    ,

     Notre

    -D

    ame

     Cathedral

    ,

     and

     the

     Lou

    vre

     Museum

    .

     French

     cuisine

     is

     also

     renowned

     for

     its

     diverse

     menu

     of

     dishes

    ,

     including

     cro

    iss

    ants

    ,

     cheese

    ,

     and

     past

    ries

    .

     Paris

     is

     known

     for

     its

     rich

     history

    ,

     art

    ,

     and

     cultural

     attractions

    ,

     including

     museums

    ,

     theaters

    ,

     and

     nightlife

    .

     The

     city

     is

     often

     referred

     to

     as

     the

     "

    Paris

     of

     the

     World

    "

     due

     to

     its

     vibrant

     and

     diverse

     population

     and

     rich

     cultural

     scene

    .

     
    


    Paris

     is

     also

     a

     major

     center

     of

     finance

     and

     business

    ,

     with

     numerous

     financial

     institutions

     and

     corporations

    .

     It

     is

     a

     city

     with

     a

     growing

     middle

     class

     and

     a

     diverse

     population

    ,

     making

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     be

     characterized

     by

     rapid

     development

     and

     adoption

    ,

     driven

     by

     advancements

     in

     computing

     power

    ,

     data

     storage

     and

     processing

     capabilities

    ,

     machine

     learning

    ,

     and

     deep

     learning

    .

     Here

     are

     some

     possible

     trends

     that

     may

     be

     observed

     in

     the

     future

     of

     artificial

     intelligence

    :
    


    1

    .

     Autonomous

     and

     semi

    -aut

    onomous

     vehicles

    :

     Autonomous

     and

     semi

    -aut

    onomous

     vehicles

     are

     expected

     to

     become

     more

     prevalent

    ,

     driving

     the

     development

     of

     AI

     systems

     that

     can

     navigate

     roads

     and

     traffic

     conditions

     independently

    .

     This

     could

     lead

     to

     reduced

     traffic

     congestion

     and

     improved

     safety

    ,

     but

     also

     raise

     ethical

     concerns

     about

     the

     use

     of

     AI

     in

     autonomous

     vehicles

    .
    


    2

    .

     Improved

     language

     models

    :

     Natural

     language

     processing

     (

    N

    LP

    )

     technologies

     are

     expected

     to

     become

     more

     advanced

    



```python
llm.shutdown()
```
