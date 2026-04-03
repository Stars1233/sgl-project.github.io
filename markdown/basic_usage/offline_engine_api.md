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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  8.13it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  8.12it/s]


    2026-04-03 22:26:24,299 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 22:26:24] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:36,  2.75s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:29,  1.83it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.62it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.62it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.62it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.62it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:03<00:08,  5.62it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:03<00:08,  5.62it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:03<00:08,  5.62it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:03<00:08,  5.62it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:08,  5.62it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 11.91it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 11.91it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 11.91it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 11.91it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 11.91it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 11.91it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 11.91it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 11.91it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.01it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.01it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.01it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.01it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.01it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.01it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.01it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.01it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.73it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.73it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.73it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.73it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.73it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.73it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.73it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.84it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.84it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.84it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.84it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.84it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.84it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.84it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.25it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.25it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.25it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.25it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.25it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.25it/s]

    Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.25it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.68it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.68it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.68it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.68it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.68it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.68it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.68it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.68it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.68it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.63it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.02 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.99 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.99 GB):   3%|▎         | 2/58 [00:00<00:03, 18.65it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.99 GB):   3%|▎         | 2/58 [00:00<00:03, 18.65it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.95 GB):   3%|▎         | 2/58 [00:00<00:03, 18.65it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.96 GB):   3%|▎         | 2/58 [00:00<00:03, 18.65it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=118.96 GB):   9%|▊         | 5/58 [00:00<00:02, 21.68it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.95 GB):   9%|▊         | 5/58 [00:00<00:02, 21.68it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.95 GB):   9%|▊         | 5/58 [00:00<00:02, 21.68it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.95 GB):   9%|▊         | 5/58 [00:00<00:02, 21.68it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.95 GB):   9%|▊         | 5/58 [00:00<00:02, 21.68it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.95 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.41it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.94 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.41it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.92 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.41it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=118.92 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.41it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.92 GB):  21%|██        | 12/58 [00:00<00:02, 21.99it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.91 GB):  21%|██        | 12/58 [00:00<00:02, 21.99it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.91 GB):  21%|██        | 12/58 [00:00<00:02, 21.99it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.82 GB):  21%|██        | 12/58 [00:00<00:02, 21.99it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=118.82 GB):  26%|██▌       | 15/58 [00:00<00:02, 21.23it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.41 GB):  26%|██▌       | 15/58 [00:00<00:02, 21.23it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.40 GB):  26%|██▌       | 15/58 [00:00<00:02, 21.23it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.24 GB):  26%|██▌       | 15/58 [00:00<00:02, 21.23it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.24 GB):  26%|██▌       | 15/58 [00:00<00:02, 21.23it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.23 GB):  26%|██▌       | 15/58 [00:00<00:02, 21.23it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.23 GB):  34%|███▍      | 20/58 [00:00<00:01, 27.90it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.21 GB):  34%|███▍      | 20/58 [00:00<00:01, 27.90it/s]Capturing num tokens (num_tokens=960 avail_mem=118.23 GB):  34%|███▍      | 20/58 [00:00<00:01, 27.90it/s] Capturing num tokens (num_tokens=896 avail_mem=118.23 GB):  34%|███▍      | 20/58 [00:00<00:01, 27.90it/s]Capturing num tokens (num_tokens=832 avail_mem=118.22 GB):  34%|███▍      | 20/58 [00:00<00:01, 27.90it/s]Capturing num tokens (num_tokens=768 avail_mem=118.22 GB):  34%|███▍      | 20/58 [00:00<00:01, 27.90it/s]

    Capturing num tokens (num_tokens=768 avail_mem=118.22 GB):  43%|████▎     | 25/58 [00:00<00:00, 33.10it/s]Capturing num tokens (num_tokens=704 avail_mem=118.22 GB):  43%|████▎     | 25/58 [00:00<00:00, 33.10it/s]Capturing num tokens (num_tokens=640 avail_mem=118.21 GB):  43%|████▎     | 25/58 [00:00<00:00, 33.10it/s]Capturing num tokens (num_tokens=576 avail_mem=118.21 GB):  43%|████▎     | 25/58 [00:00<00:00, 33.10it/s]Capturing num tokens (num_tokens=512 avail_mem=118.20 GB):  43%|████▎     | 25/58 [00:00<00:00, 33.10it/s]Capturing num tokens (num_tokens=480 avail_mem=118.22 GB):  43%|████▎     | 25/58 [00:00<00:00, 33.10it/s]Capturing num tokens (num_tokens=480 avail_mem=118.22 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.88it/s]Capturing num tokens (num_tokens=448 avail_mem=118.21 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.88it/s]Capturing num tokens (num_tokens=416 avail_mem=118.21 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.88it/s]Capturing num tokens (num_tokens=384 avail_mem=118.21 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.88it/s]Capturing num tokens (num_tokens=352 avail_mem=118.21 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.88it/s]Capturing num tokens (num_tokens=320 avail_mem=118.20 GB):  52%|█████▏    | 30/58 [00:01<00:00, 36.88it/s]

    Capturing num tokens (num_tokens=320 avail_mem=118.20 GB):  60%|██████    | 35/58 [00:01<00:00, 36.76it/s]Capturing num tokens (num_tokens=288 avail_mem=118.20 GB):  60%|██████    | 35/58 [00:01<00:00, 36.76it/s]Capturing num tokens (num_tokens=256 avail_mem=118.20 GB):  60%|██████    | 35/58 [00:01<00:00, 36.76it/s]Capturing num tokens (num_tokens=240 avail_mem=118.19 GB):  60%|██████    | 35/58 [00:01<00:00, 36.76it/s]Capturing num tokens (num_tokens=224 avail_mem=118.19 GB):  60%|██████    | 35/58 [00:01<00:00, 36.76it/s]Capturing num tokens (num_tokens=224 avail_mem=118.19 GB):  67%|██████▋   | 39/58 [00:01<00:00, 37.19it/s]Capturing num tokens (num_tokens=208 avail_mem=118.19 GB):  67%|██████▋   | 39/58 [00:01<00:00, 37.19it/s]Capturing num tokens (num_tokens=192 avail_mem=118.19 GB):  67%|██████▋   | 39/58 [00:01<00:00, 37.19it/s]

    Capturing num tokens (num_tokens=176 avail_mem=118.18 GB):  67%|██████▋   | 39/58 [00:01<00:00, 37.19it/s]Capturing num tokens (num_tokens=160 avail_mem=118.18 GB):  67%|██████▋   | 39/58 [00:01<00:00, 37.19it/s]Capturing num tokens (num_tokens=160 avail_mem=118.18 GB):  74%|███████▍  | 43/58 [00:01<00:00, 29.33it/s]Capturing num tokens (num_tokens=144 avail_mem=118.17 GB):  74%|███████▍  | 43/58 [00:01<00:00, 29.33it/s]Capturing num tokens (num_tokens=128 avail_mem=118.17 GB):  74%|███████▍  | 43/58 [00:01<00:00, 29.33it/s]Capturing num tokens (num_tokens=112 avail_mem=118.17 GB):  74%|███████▍  | 43/58 [00:01<00:00, 29.33it/s]

    Capturing num tokens (num_tokens=96 avail_mem=118.16 GB):  74%|███████▍  | 43/58 [00:01<00:00, 29.33it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=118.16 GB):  81%|████████  | 47/58 [00:02<00:00, 15.76it/s]Capturing num tokens (num_tokens=80 avail_mem=118.16 GB):  81%|████████  | 47/58 [00:02<00:00, 15.76it/s]Capturing num tokens (num_tokens=64 avail_mem=118.16 GB):  81%|████████  | 47/58 [00:02<00:00, 15.76it/s]Capturing num tokens (num_tokens=48 avail_mem=118.16 GB):  81%|████████  | 47/58 [00:02<00:00, 15.76it/s]Capturing num tokens (num_tokens=32 avail_mem=118.15 GB):  81%|████████  | 47/58 [00:02<00:00, 15.76it/s]Capturing num tokens (num_tokens=32 avail_mem=118.15 GB):  88%|████████▊ | 51/58 [00:02<00:00, 18.74it/s]Capturing num tokens (num_tokens=28 avail_mem=118.15 GB):  88%|████████▊ | 51/58 [00:02<00:00, 18.74it/s]Capturing num tokens (num_tokens=24 avail_mem=118.14 GB):  88%|████████▊ | 51/58 [00:02<00:00, 18.74it/s]Capturing num tokens (num_tokens=20 avail_mem=118.14 GB):  88%|████████▊ | 51/58 [00:02<00:00, 18.74it/s]Capturing num tokens (num_tokens=16 avail_mem=118.14 GB):  88%|████████▊ | 51/58 [00:02<00:00, 18.74it/s]

    Capturing num tokens (num_tokens=12 avail_mem=118.13 GB):  88%|████████▊ | 51/58 [00:02<00:00, 18.74it/s]Capturing num tokens (num_tokens=12 avail_mem=118.13 GB):  97%|█████████▋| 56/58 [00:02<00:00, 23.57it/s]Capturing num tokens (num_tokens=8 avail_mem=118.13 GB):  97%|█████████▋| 56/58 [00:02<00:00, 23.57it/s] Capturing num tokens (num_tokens=4 avail_mem=118.13 GB):  97%|█████████▋| 56/58 [00:02<00:00, 23.57it/s]Capturing num tokens (num_tokens=4 avail_mem=118.13 GB): 100%|██████████| 58/58 [00:02<00:00, 25.51it/s]


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
    Generated text:  Gabriel, and I am an artificial intelligence designed to help users find information and answer their questions. How can I assist you today? Are you looking for information or advice on a specific topic or issue? Please feel free to ask your question, and I will do my best to provide you with accurate and helpful answers. What topic are you interested in learning more about? Let me know and I'll do my best to assist you. Do you have any specific areas of interest or areas of expertise that you'd like to know more about? Once you provide me with that information, I'll do my best to help you with your query. I
    ===============================
    Prompt: The president of the United States is
    Generated text:  from the _________ country.
    
    A. United Kingdom  
    B. Canada  
    C. Germany  
    D. France  
    E. Australia  
    F. United States
    To determine which country the president of the United States is from, we need to recall that the president of the United States is the head of the executive branch of the federal government. The head of the executive branch is the President of the United States.
    
    The President of the United States is also known as the Chief of Staff of the President. The President and the Chief of Staff are typically members of the President's Cabinet, which is the highest level of government in the United States.
    
    
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris, the capital of the United States is Washington, D.C., the capital of Canada is Ottawa, and the capital of the United Kingdom is London. Which country has the largest population?
    To determine which country has the largest population, we need to look at the population statistics for each of the capitals mentioned: Paris, Washington, D.C., Ottawa, and London.
    
    1. **Paris, France**:
       - Population: 2,764,867 (as of the latest data available)
    
    2. **Washington, D. C., United States**:
       - Population: 676,64
    ===============================
    Prompt: The future of AI is
    Generated text:  digital – with AI transforming many industries and the way we work. While it is crucial to establish the right foundation for AI, the role of the human being is essential in developing solutions that can be beneficial to the individual, society and the environment. 
    
    What is AI and how does it work?
    
    AI is an umbrella term which includes the field of computer science and software engineering that creates artificial intelligence. AI uses algorithms to enable machines to process information, solve problems, and make decisions. The specific applications of AI vary from the basic tasks of identifying patterns and making decisions to more complex tasks such as the creation of virtual assistants that can assist with


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


    Generated text:  [Name], and I'm a [Job Title] at [Company Name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [Age], [Gender], [Nationality], [Occupation], [Hobbies], and [Favorite Food]. I'm always looking for new experiences and learning new things, so I'm always eager to share my knowledge and insights with you. Thank you for having me! [Name] [Company Name] [Company Address] [Company Phone Number] [Company Email] [Company Website] [Company LinkedIn Profile] [Company Twitter Profile]
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major center for French culture, cuisine, and fashion. Paris is a popular tourist destination and a major economic center, with a rich history dating back to the Roman Empire. It is home to many famous museums, including the Louvre and the Musée d'Orsay, as well as the Notre-Dame Cathedral. The city is also known for its vibrant nightlife and its role in the French Revolution. Paris is a city of contrasts, with its historical architecture and modern culture blending
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased integration with human intelligence: As AI becomes more advanced, it is likely to become more integrated with human intelligence, allowing it to learn from and adapt to human behavior and decision-making processes.
    
    2. Greater emphasis on ethical considerations: As AI becomes more advanced, there will be a greater emphasis on ethical considerations, including issues such as bias, transparency, and accountability.
    
    3. Increased use of AI in healthcare: AI is already being used in healthcare to improve patient outcomes and
    


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
    Generated text:  [Name] and I am a [Age], [Occupation]. I have [Number of Years Worked in Field] years of experience in [Your Field/Job]. I am the founder of [Your Company Name] and I am passionate about [Your Passion/Motivational Word]. I believe that everyone has the potential to succeed, and I strive to help others reach their full potential. I am a [Type of Person] who is [Your Personality], and I am always looking for ways to improve myself and my team. What would you like me to say next? [Name]: I am a [Name], [Age
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    A concise factual statement about France's capital city is that it is Paris. 
    
    Q: What is the capital of France?
    A: Paris is the capital of France. 
    
    Does that help to explain it? Let me know if you need anything else! 📚🤗✨
    3
    You are a helpful assistant, just now 🚀🌟✨
    
    Thank you! 🚀🌟✨
    1
    You are a helpful assistant, just now 📚✨
    
    Always ready to assist! 🚀✨
    0
    You are a helpful assistant, just now 🚀✨
    
    I hope this helps
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  looking very promising, with many potential trends and advancements that could shape the industry in the coming years.
    
    One of the key trends in AI is the increasing integration of more advanced machine learning algorithms, artificial neural networks, and other complex models into everyday applications. This will lead to new and innovative solutions to complex problems, such as image recognition, natural language processing, and predictive analytics.
    
    Another important trend is the development of AI that can adapt and learn from new data sources. This means that AI models will become more capable of recognizing patterns and trends in complex data sets, and will be able to learn from feedback to improve their performance.
    
    AI will also


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

     or

     position

    ]

     in

     [

    company

     name

    ].

     I

    've

     always

     had

     a

     love

     for

     [

    favorite

     hobby

    ,

     hobby

    ,

     or

     project

    ].

     I

    'm

     always

     looking

     for

     new

     experiences

     and

     learning

     opportunities

    ,

     and

     I

    'm

     a

     great

     communicator

     and

     listener

    .

     I

    'm

     always

     looking

     for

     new

     ways

     to

     improve

     myself

     and

     help

     others

     in

     my

     community

    .

     Thank

     you

     for

     asking

    !

     

    📚

    💻

    💼

    ✨

    
    


    How

     can

     I

     assist

     you

     today

    ?

     

    📚

    💻

    💼

    ✨

    
    


    Can

     you

     provide

     me

     with

     some

     advice

     on

     how

     to

     improve

     my

     communication

     skills

    ?

     

    📚

    💻

    💼

    ✨

    
    


    What

     are

     some

     of

     the

     most

     exciting

     projects

     you

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .


    Paris

     is

     the

     cultural

     and

     economic

     center

     of

     France

     and

     is

     known

     as

     the

     "

    city

     of

     a

     thousand

     gardens

    "

     for

     its

     extensive

     and

     beautiful

     bou

    lev

    ards

    . It

     is

     home

     to

     many

     famous

     landmarks

    ,

     including

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

    .

     It

     is

     also

     known

     for

     its

     rich

     history

     and

     numerous

     museums

    ,

     including

     the

     Mus

    ée

     d

    '

    Or

    say

    ,

     the

     Mus

    ée

     Rod

    in

    ,

     and

     the

     Mus

    ée

     Rod

    in

    -B

    asil

    ique

    .

     Paris

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

     restaurants

    ,

     cafes

    ,

     and

     shops

    .

     It

     is

     also

     the

     seat

     of

     France

    's

     government

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     anticipated

     and

     will

     likely

     involve

     numerous

     developments

     and

     advancements

    .

     Here

     are

     some

     potential

     trends

     that

     are

     likely

     to

     shape

     the

     future

     of

     AI

    :
    


    1

    .

     Enhanced

     Cognitive

     Function

    :

     With

     advances

     in

     artificial

     intelligence

    ,

     it

     is

     likely

     that

     cognitive

     functions

     such

     as

     memory

    ,

     perception

    ,

     and

     problem

    -solving

     will

     continue

     to

     improve

    .

     This

     will

     result

     in

     more

     accurate

     and

     efficient

     AI

     systems

     that

     can

     handle

     a

     wider

     range

     of

     tasks

    .
    


    2

    .

     Self

    -

    Driving

     Cars

    :

     As

     autonomous

     vehicle

     technology

     continues

     to

     improve

    ,

     it

     is

     likely

     that

     self

    -driving

     cars

     will

     become

     more

     common

    .

     These

     vehicles

     will

     use

     AI

     algorithms

     to

     navigate

     roads

     and

     handle

     traffic

    ,

     making

     them

     more

     reliable

     and

     safer

    .
    


    3

    .

     Medical

     Imaging

    



```python
llm.shutdown()
```
