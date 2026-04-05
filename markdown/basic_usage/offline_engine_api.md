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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.58it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.57it/s]


    2026-04-05 00:22:32,981 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-05 00:22:32] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:22,  2.37it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:22,  2.37it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:22,  2.37it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:22,  2.37it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:22,  2.37it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:22,  2.37it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:22,  2.37it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.22it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.22it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.22it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.22it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:07,  6.22it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:07,  6.22it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:07,  6.22it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:07,  6.22it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:07,  6.22it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:03, 12.54it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:03, 12.54it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 12.54it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 18.69it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 18.69it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 18.69it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 18.69it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 18.69it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 18.69it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 18.69it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 18.69it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 25.41it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 25.41it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 25.41it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 25.41it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 25.41it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 25.41it/s]

    Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 25.41it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 30.51it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 30.51it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 30.51it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 30.51it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 30.51it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 30.51it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 30.51it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 35.51it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 35.51it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 35.51it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 35.51it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 35.51it/s]

    Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 35.51it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 35.51it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 39.72it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 39.72it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 39.72it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 39.72it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 39.72it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 39.72it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 39.72it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 39.72it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.02it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.76 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.01it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.01it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.01it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:03, 18.01it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.73 GB):   9%|▊         | 5/58 [00:00<00:02, 20.31it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 20.31it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 20.31it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 20.31it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 20.31it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.72 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.77it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.72 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.77it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.71 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.77it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=120.71 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.77it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.37 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.77it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.77it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.28 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.72it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.72it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.72it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.27 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.72it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.72it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.72it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.22it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.26 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.22it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=120.24 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.22it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.22it/s] Capturing num tokens (num_tokens=896 avail_mem=120.25 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.22it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.22it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.49it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.49it/s]Capturing num tokens (num_tokens=704 avail_mem=120.24 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.49it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.49it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.49it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.49it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  50%|█████     | 29/58 [00:00<00:00, 41.77it/s]Capturing num tokens (num_tokens=480 avail_mem=120.24 GB):  50%|█████     | 29/58 [00:00<00:00, 41.77it/s]

    Capturing num tokens (num_tokens=448 avail_mem=120.24 GB):  50%|█████     | 29/58 [00:00<00:00, 41.77it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  50%|█████     | 29/58 [00:00<00:00, 41.77it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  50%|█████     | 29/58 [00:00<00:00, 41.77it/s]Capturing num tokens (num_tokens=352 avail_mem=120.23 GB):  50%|█████     | 29/58 [00:00<00:00, 41.77it/s]Capturing num tokens (num_tokens=352 avail_mem=120.23 GB):  59%|█████▊    | 34/58 [00:00<00:00, 43.43it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  59%|█████▊    | 34/58 [00:00<00:00, 43.43it/s]Capturing num tokens (num_tokens=288 avail_mem=120.22 GB):  59%|█████▊    | 34/58 [00:00<00:00, 43.43it/s]Capturing num tokens (num_tokens=256 avail_mem=120.22 GB):  59%|█████▊    | 34/58 [00:00<00:00, 43.43it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  59%|█████▊    | 34/58 [00:00<00:00, 43.43it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  59%|█████▊    | 34/58 [00:01<00:00, 43.43it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  67%|██████▋   | 39/58 [00:01<00:00, 44.29it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  67%|██████▋   | 39/58 [00:01<00:00, 44.29it/s]

    Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  67%|██████▋   | 39/58 [00:01<00:00, 44.29it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  67%|██████▋   | 39/58 [00:01<00:00, 44.29it/s]Capturing num tokens (num_tokens=160 avail_mem=120.20 GB):  67%|██████▋   | 39/58 [00:01<00:00, 44.29it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  67%|██████▋   | 39/58 [00:01<00:00, 44.29it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  76%|███████▌  | 44/58 [00:01<00:00, 45.24it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  76%|███████▌  | 44/58 [00:01<00:00, 45.24it/s]Capturing num tokens (num_tokens=112 avail_mem=120.19 GB):  76%|███████▌  | 44/58 [00:01<00:00, 45.24it/s]Capturing num tokens (num_tokens=96 avail_mem=120.18 GB):  76%|███████▌  | 44/58 [00:01<00:00, 45.24it/s] Capturing num tokens (num_tokens=80 avail_mem=120.18 GB):  76%|███████▌  | 44/58 [00:01<00:00, 45.24it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  76%|███████▌  | 44/58 [00:01<00:00, 45.24it/s]Capturing num tokens (num_tokens=64 avail_mem=120.18 GB):  84%|████████▍ | 49/58 [00:01<00:00, 45.25it/s]Capturing num tokens (num_tokens=48 avail_mem=120.17 GB):  84%|████████▍ | 49/58 [00:01<00:00, 45.25it/s]

    Capturing num tokens (num_tokens=32 avail_mem=120.17 GB):  84%|████████▍ | 49/58 [00:01<00:00, 45.25it/s]Capturing num tokens (num_tokens=28 avail_mem=120.16 GB):  84%|████████▍ | 49/58 [00:01<00:00, 45.25it/s]Capturing num tokens (num_tokens=24 avail_mem=120.16 GB):  84%|████████▍ | 49/58 [00:01<00:00, 45.25it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  84%|████████▍ | 49/58 [00:01<00:00, 45.25it/s]Capturing num tokens (num_tokens=20 avail_mem=120.16 GB):  93%|█████████▎| 54/58 [00:01<00:00, 45.80it/s]Capturing num tokens (num_tokens=16 avail_mem=120.16 GB):  93%|█████████▎| 54/58 [00:01<00:00, 45.80it/s]Capturing num tokens (num_tokens=12 avail_mem=120.15 GB):  93%|█████████▎| 54/58 [00:01<00:00, 45.80it/s]Capturing num tokens (num_tokens=8 avail_mem=120.15 GB):  93%|█████████▎| 54/58 [00:01<00:00, 45.80it/s] Capturing num tokens (num_tokens=4 avail_mem=120.15 GB):  93%|█████████▎| 54/58 [00:01<00:00, 45.80it/s]Capturing num tokens (num_tokens=4 avail_mem=120.15 GB): 100%|██████████| 58/58 [00:01<00:00, 40.31it/s]


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
    Generated text:  Tom. My family is from the United States, but my parents and I live in the United Kingdom. In England, we have one day for breakfast and one for lunch. We have two days for dinner. In England, we eat lots of food on the weekend. We often go to the park, go to the cinema, or have tea and coffee. In America, we have only breakfast and dinner. The weather in the United States is usually very warm and sunny. The weather in England is usually cool and rainy. Tom's brother and sister are twins. They like to play a game of tag. What sport do they like to
    ===============================
    Prompt: The president of the United States is
    Generated text:  a presidential candidate. The probability that candidate X will be elected is 60%, and the probability that candidate Y will be elected is 40%. If these candidates are competing in a presidential election, what is the probability that candidate X will win the election? To determine the probability that candidate X will win the election, we need to consider the different possible scenarios in which X can win. Since the probability that X is elected is 60% (or 0.60) and the probability that Y is elected is 40% (or 0.40), we can assume that there are two possible outcomes
    ===============================
    Prompt: The capital of France is
    Generated text: :
    A. Paris
    B. Geneva
    C. Berlin
    D. London
    The capital of France is Paris. Therefore, the correct answer is:
    
    A. Paris
    
    Paris is the capital city of France, and it is also the largest and most populous city in the country. It serves as the administrative and political center of France, as well as the cultural, scientific, and economic hub. The city is known for its rich history, cultural heritage, and beautiful architecture, which are some of the highlights of Paris. However, it is important to note that other cities in France, such as Lyon, Bordeaux, and Lille,
    ===============================
    Prompt: The future of AI is
    Generated text:  here. Today, we know that the human brain is capable of processing data and performing complex calculations on a vast amount of information. This capability is not only achieved with the help of advanced hardware such as GPUs, but it is also the result of an evolutionary process where the neural nets that humans use evolved over millions of years to do what we can do now. However, the neural nets that we have today are not designed to solve complex problems. They are not designed to learn and adapt in real-time, and they are not designed to understand complex patterns and relationships within data. The human brain is not a machine, but a unique complex organ


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


    Generated text:  [Name] and I'm a [Age] year old [Gender] [Occupation]. I'm a [Occupation] who has been [Number of Years] years in the industry. I'm passionate about [What I Love About My Profession]. I'm always looking for new challenges and opportunities to grow and learn. I'm a [What I Like to Do] person who is always [What I Like to Do]. I'm [What I Like to Do] and I'm always [What I Like to Do]. I'm [What I Like to Do] and I'm always [What I Like to Do]. I'm
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, and Notre-Dame Cathedral. It is also a cultural and economic hub, known for its rich history, art, and cuisine. Paris is a popular tourist destination, attracting millions of visitors each year. The city is home to many world-renowned museums, art galleries, and theaters, and is a major center for science, technology, and innovation. Paris is a vibrant and dynamic city, with a rich history and a strong sense of community. Its status as the capital of France has made it a major economic and political center, and its influence
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in areas such as machine learning, natural language processing, and computer vision. Some potential future trends include:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to diagnose and treat diseases, and it has the potential to revolutionize the field by improving diagnostic accuracy and personalized treatment plans.
    
    2. AI in manufacturing: AI is already being used in manufacturing to optimize production processes, reduce waste, and improve quality control. As AI technology continues to advance, we can expect to see even more applications in manufacturing, such as predictive maintenance and automated assembly lines.
    
    3. AI in finance:
    


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
    Generated text:  [Your Name], and I am a [Character Name]!
    
    I am a [Your Role] who has been [Your Profession] in [Your Location]. I am here to bring the excitement and joy of my profession to those around me through my unique blend of creativity and passion.
    
    I believe in [Your Mission/Value] and I strive to make a difference in the world by [Your Advocacy/Charity]. I am a [Your Character Value], and I believe that being [Your Character Trait] is what sets me apart from others.
    
    I am passionate about [Your Passion/Interest], and I am always looking for ways
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as the City of Light, one of the most populous cities in the world.
    
    That's great! Could you also provide some more details about Paris, such as its history or famous landmarks? Certainly! Paris, often referred to as the City of Light, is a vibrant and historic city in western France. Its rich history dates back to the Middle Ages, when the city was the capital of the Duchy of Paris. Over the centuries, Paris has undergone significant changes and has developed into one of the world's most vibrant and diverse cities.
    
    Paris is known for its many landmarks, including the Eiffel Tower, the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  highly uncertain and will likely involve a wide range of trends and developments in the coming years. Here are some potential future trends in AI:
    
    1. Increased use of AI in healthcare: AI is already being used in healthcare to improve diagnosis, treatment, and patient outcomes. As AI becomes more sophisticated, we can expect to see it used in more complex scenarios, such as personalized medicine and drug discovery.
    
    2. More widespread use of AI in manufacturing: AI is already being used in manufacturing to improve efficiency, reduce errors, and increase productivity. As AI becomes more advanced, we can expect to see it used in even more complex manufacturing processes.
    
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

    Name

    ],

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

    'm

     currently

     [

    number

     of

     years

     in

     the

     position

    ]

     years

     in

     the

     industry

    .

     I

     bring

     a

     wealth

     of

     knowledge

    ,

     experience

    ,

     and

     a

     positive

     attitude

     to

     the

     role

    .

     I

     enjoy

     [

    reason

     why

     you

     like

     the

     job

    ],

     and

     I

     look

     forward

     to

     [

    reason

     why

     you

     feel

     excited

     about

     your

     new

     role

    ].

     I

    'm

     excited

     to

     make

     a

     difference

     in

     [

    industry

    ]

     and

     contribute

     to

     [

    whatever

     it

     is

     you

     do

     or

     say

     here

    ],

     every

     day

    .


    In

     essence

    ,

     I

    'm

     a

     [

    insert

     any

     unique

     characteristic

     or

     personality

     trait

    ]

     with

     a

     passion

     for

     [

    what

     you

     do

     or

     say

     here

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     also

     known

     as

     the

     City

     of

     Light

    .

     It

     is

     the

     largest

     city

     in

     France

     and

     the

     

    1

    4

    th

     largest

     city

     in

     the

     world

    .

     It

     is

     home

     to

     many

     of

     France

    's

     most

     famous

     landmarks

     and

     is

     known

     for

     its

     romantic

     architecture

     and

     vibrant

     culture

    .

     Paris

     is

     also

     a

     major

     center

     for

     the

     arts

    ,

     music

    ,

     and

     fashion

    ,

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

     It

     is

     a

     popular

     tourist

     destination

     and

     is

     home

     to

     many

     museums

    ,

     theaters

    ,

     and

     other

     attractions

     throughout

     the

     year

    .

     The

     city

     is

     also

     known

     for

     its

     extensive

     food

     culture

    ,

     with

     a

     large

     variety

     of

     cuis

    ines

     to

     choose

     from

    .

     Overall

    ,

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     full

     of

     exciting

     possibilities

     and

     challenges

    .

     Here

     are

     some

     potential

     trends

     that

     could

     emerge

     in

     the

     coming

     years

    :
    


    1

    .

     Increased

     AI

     integration

     with

     other

     technologies

    :

     AI

     is

     already

     being

     integrated

     into

     a

     variety

     of

     other

     technologies

    ,

     such

     as

     smart

     homes

    ,

     self

    -driving

     cars

    ,

     and

     virtual

     assistants

    .

     As

     these

     technologies

     continue

     to

     evolve

    ,

     we

     can

     expect

     AI

     to

     become

     even

     more

     integrated

     with

     them

    ,

     such

     as

     through

     smart

     city

     projects

     and

     virtual

     assistants

     that

     can

     interact

     with

     humans

     in

     more

     natural

     ways

    .
    


    2

    .

     AI

     for

     healthcare

    :

     AI

     is

     already

     being

     used

     in

     healthcare

     to

     diagnose

     and

     treat

     diseases

    ,

     but

     there

     is

     still

     much

     to

     be

     done

     to

     improve

     its

     efficacy

    .

     AI

     can

     help

     doctors

    



```python
llm.shutdown()
```
