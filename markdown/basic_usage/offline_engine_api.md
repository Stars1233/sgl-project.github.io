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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.55it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.53it/s]


    2026-04-05 16:43:43,890 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-05 16:43:43] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:34,  2.71s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:29,  1.85it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.69it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:03<00:08,  5.69it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:03<00:08,  5.69it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 12.05it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 12.05it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.20it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.20it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.20it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.20it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.20it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.20it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.20it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.20it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.95it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.95it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.95it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.95it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.95it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.95it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.95it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 30.04it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 30.04it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 30.04it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 30.04it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 30.04it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 30.04it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 30.04it/s]Compiling num tokens (num_tokens=128):  66%|██████▌   | 38/58 [00:03<00:00, 30.04it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 35.78it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 35.78it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 35.78it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 35.78it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 35.78it/s]

    Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 35.78it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 35.78it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 39.78it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 39.78it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 39.78it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 39.78it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 39.78it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 39.78it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 39.78it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 39.78it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.79it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.76 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:04, 12.45it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:04, 12.45it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:04, 12.45it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.73 GB):   3%|▎         | 2/58 [00:00<00:04, 12.45it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.73 GB):   9%|▊         | 5/58 [00:00<00:02, 18.16it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 18.16it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 18.16it/s]Capturing num tokens (num_tokens=4608 avail_mem=120.72 GB):   9%|▊         | 5/58 [00:00<00:02, 18.16it/s]Capturing num tokens (num_tokens=4096 avail_mem=120.39 GB):   9%|▊         | 5/58 [00:00<00:02, 18.16it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=120.39 GB):  16%|█▌        | 9/58 [00:00<00:02, 23.99it/s]Capturing num tokens (num_tokens=3840 avail_mem=120.29 GB):  16%|█▌        | 9/58 [00:00<00:02, 23.99it/s]Capturing num tokens (num_tokens=3584 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:02, 23.99it/s]Capturing num tokens (num_tokens=3328 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:02, 23.99it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.28 GB):  16%|█▌        | 9/58 [00:00<00:02, 23.99it/s]Capturing num tokens (num_tokens=3072 avail_mem=120.28 GB):  22%|██▏       | 13/58 [00:00<00:01, 29.16it/s]Capturing num tokens (num_tokens=2816 avail_mem=120.28 GB):  22%|██▏       | 13/58 [00:00<00:01, 29.16it/s]Capturing num tokens (num_tokens=2560 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 29.16it/s]Capturing num tokens (num_tokens=2304 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 29.16it/s]Capturing num tokens (num_tokens=2048 avail_mem=120.27 GB):  22%|██▏       | 13/58 [00:00<00:01, 29.16it/s]Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  22%|██▏       | 13/58 [00:00<00:01, 29.16it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=120.26 GB):  31%|███       | 18/58 [00:00<00:01, 34.35it/s]Capturing num tokens (num_tokens=1536 avail_mem=120.26 GB):  31%|███       | 18/58 [00:00<00:01, 34.35it/s]Capturing num tokens (num_tokens=1280 avail_mem=120.26 GB):  31%|███       | 18/58 [00:00<00:01, 34.35it/s]Capturing num tokens (num_tokens=1024 avail_mem=120.24 GB):  31%|███       | 18/58 [00:00<00:01, 34.35it/s]Capturing num tokens (num_tokens=960 avail_mem=120.25 GB):  31%|███       | 18/58 [00:00<00:01, 34.35it/s] Capturing num tokens (num_tokens=896 avail_mem=120.25 GB):  31%|███       | 18/58 [00:00<00:01, 34.35it/s]Capturing num tokens (num_tokens=896 avail_mem=120.25 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.10it/s]Capturing num tokens (num_tokens=832 avail_mem=120.24 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.10it/s]Capturing num tokens (num_tokens=768 avail_mem=120.24 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.10it/s]Capturing num tokens (num_tokens=704 avail_mem=120.24 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.10it/s]Capturing num tokens (num_tokens=640 avail_mem=120.23 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.10it/s]Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  40%|███▉      | 23/58 [00:00<00:00, 38.10it/s]

    Capturing num tokens (num_tokens=576 avail_mem=120.23 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.65it/s]Capturing num tokens (num_tokens=512 avail_mem=120.22 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.65it/s]Capturing num tokens (num_tokens=480 avail_mem=120.24 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.65it/s]Capturing num tokens (num_tokens=448 avail_mem=120.24 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.65it/s]Capturing num tokens (num_tokens=416 avail_mem=120.23 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.65it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  48%|████▊     | 28/58 [00:00<00:00, 40.65it/s]Capturing num tokens (num_tokens=384 avail_mem=120.23 GB):  57%|█████▋    | 33/58 [00:00<00:00, 42.36it/s]Capturing num tokens (num_tokens=352 avail_mem=120.22 GB):  57%|█████▋    | 33/58 [00:00<00:00, 42.36it/s]Capturing num tokens (num_tokens=320 avail_mem=120.22 GB):  57%|█████▋    | 33/58 [00:00<00:00, 42.36it/s]Capturing num tokens (num_tokens=288 avail_mem=120.22 GB):  57%|█████▋    | 33/58 [00:00<00:00, 42.36it/s]Capturing num tokens (num_tokens=256 avail_mem=120.21 GB):  57%|█████▋    | 33/58 [00:01<00:00, 42.36it/s]Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  57%|█████▋    | 33/58 [00:01<00:00, 42.36it/s]

    Capturing num tokens (num_tokens=240 avail_mem=120.21 GB):  66%|██████▌   | 38/58 [00:01<00:00, 43.55it/s]Capturing num tokens (num_tokens=224 avail_mem=120.21 GB):  66%|██████▌   | 38/58 [00:01<00:00, 43.55it/s]Capturing num tokens (num_tokens=208 avail_mem=120.20 GB):  66%|██████▌   | 38/58 [00:01<00:00, 43.55it/s]Capturing num tokens (num_tokens=192 avail_mem=120.20 GB):  66%|██████▌   | 38/58 [00:01<00:00, 43.55it/s]Capturing num tokens (num_tokens=176 avail_mem=120.20 GB):  66%|██████▌   | 38/58 [00:01<00:00, 43.55it/s]Capturing num tokens (num_tokens=160 avail_mem=120.20 GB):  66%|██████▌   | 38/58 [00:01<00:00, 43.55it/s]Capturing num tokens (num_tokens=160 avail_mem=120.20 GB):  74%|███████▍  | 43/58 [00:01<00:00, 44.59it/s]Capturing num tokens (num_tokens=144 avail_mem=120.19 GB):  74%|███████▍  | 43/58 [00:01<00:00, 44.59it/s]Capturing num tokens (num_tokens=128 avail_mem=120.19 GB):  74%|███████▍  | 43/58 [00:01<00:00, 44.59it/s]Capturing num tokens (num_tokens=112 avail_mem=120.23 GB):  74%|███████▍  | 43/58 [00:01<00:00, 44.59it/s]Capturing num tokens (num_tokens=96 avail_mem=120.22 GB):  74%|███████▍  | 43/58 [00:01<00:00, 44.59it/s] Capturing num tokens (num_tokens=80 avail_mem=119.00 GB):  74%|███████▍  | 43/58 [00:01<00:00, 44.59it/s]

    Capturing num tokens (num_tokens=80 avail_mem=119.00 GB):  83%|████████▎ | 48/58 [00:01<00:00, 44.31it/s]Capturing num tokens (num_tokens=64 avail_mem=118.90 GB):  83%|████████▎ | 48/58 [00:01<00:00, 44.31it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  83%|████████▎ | 48/58 [00:01<00:00, 44.31it/s]Capturing num tokens (num_tokens=32 avail_mem=118.89 GB):  83%|████████▎ | 48/58 [00:01<00:00, 44.31it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  83%|████████▎ | 48/58 [00:01<00:00, 44.31it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  83%|████████▎ | 48/58 [00:01<00:00, 44.31it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.85it/s]Capturing num tokens (num_tokens=20 avail_mem=118.88 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.85it/s]Capturing num tokens (num_tokens=16 avail_mem=118.88 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.85it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.85it/s]Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.85it/s] Capturing num tokens (num_tokens=4 avail_mem=118.87 GB):  91%|█████████▏| 53/58 [00:01<00:00, 44.85it/s]

    Capturing num tokens (num_tokens=4 avail_mem=118.87 GB): 100%|██████████| 58/58 [00:01<00:00, 45.47it/s]Capturing num tokens (num_tokens=4 avail_mem=118.87 GB): 100%|██████████| 58/58 [00:01<00:00, 38.70it/s]


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
    Generated text:  Ali and I am a software engineer. I have a degree in Computer Science and I currently work at a company that specializes in artificial intelligence and machine learning. I've always loved solving problems and helping others, so I'm excited about the opportunities that AI and machine learning can provide.
    My background is strong in programming and has been used in various programming languages including Python, Java, C++, and C#. I have worked on a wide range of projects, including web development, data analysis, and natural language processing.
    I am fluent in English and have a good command of the Spanish language as well. I enjoy learning new things and always strive to
    ===============================
    Prompt: The president of the United States is
    Generated text:  a very important person in the country. This is because he or she is the leader of the government and the first person in the country to make decisions. The president is the head of the country. Therefore, the president plays a very important role in the country. The president is the head of the military. He or she can make important decisions to help keep the country safe. The president is also very important in the economy. He or she makes decisions to help the country grow. The president is a very important person in the country. The president makes sure that everyone in the country gets along with each other. The president is very important
    ===============================
    Prompt: The capital of France is
    Generated text:  _______. A. Paris B. Paris
    
    The capital of France is Paris. 
    
    So, the correct answer is A. Paris. 
    
    To elaborate: Paris is the capital and most populous city of France, located in the south of the country on the Île de France. It is known for its vibrant culture, historical sites, and fashion, while also being the home of the French royal family. 
    
    While Paris is famous for its art, architecture, and fashion, its population is also relatively small compared to some of the other major French cities like Lyon or Nice, which are larger and more populous. Paris also serves as the French
    ===============================
    Prompt: The future of AI is
    Generated text:  up in the air, but a group of experts believe we are getting there sooner than we might think. According to a recent study, a huge number of developers, designers, and industry experts are working together to get AI to the next stage. As these experts continue to collaborate on the future of AI, it is crucial that we stay informed about what is happening in the world of AI, and to be able to help support the development of the future. As an AI research company, we are constantly learning and evolving, and we are committed to staying up-to-date with the latest developments in the field. Our team of experts is always looking


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm a [job title] at [company name], and I'm excited to meet you. I'm a [job title] at [company name], and I'm excited to meet you. I'm a [job title] at [company name], and I'm excited to meet you. I'm a [job title] at [company name], and I'm excited to meet you. I'm a [job title] at [company name], and I'm excited to meet you. I'm a [job title] at [company name],
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major cultural and economic center, hosting numerous museums, theaters, and other attractions. Paris is a popular tourist destination and a major hub for international business and diplomacy. The city is known for its rich history, art, and cuisine, and is a major center of politics and politics. It is also home to the French Parliament and the French Academy of Sciences. Paris is a vibrant and diverse city with a rich cultural heritage and a strong sense of community. The city is also known for
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: As AI becomes more advanced, it is likely to become more integrated with human intelligence, allowing for more complex and nuanced interactions. This could lead to more sophisticated forms of AI, such as those that can understand and adapt to human emotions and behaviors.
    
    2. Greater reliance on data: AI will become more data-driven, with more emphasis on collecting and analyzing large amounts of data to improve its performance. This could lead to more efficient and effective AI systems, as well as more accurate predictions and insights.
    
    3. Increased use of machine learning: Machine learning will continue
    


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
    Generated text:  [Your Name], and I am a/an [age] year-old [field of study or occupation]. I recently graduated from [university or college] with a degree in [major of study]. Currently, I am a/an [occupation or hobby], and I'm passionate about [interest/ambition]. I'm an [character trait] person, and I enjoy [activity or hobby]. How would you describe yourself? [Your Name] is a [fill-in-the-blank] who is [fill-in-the-blank] and [fill-in-the-blank]. I'm [fill-in-the-blank] and [fill-in
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, the world-renowned French city. Paris is a bustling metropolis of over 2 million people, known for its architectural wonders, iconic landmarks, and rich cultural heritage. Known as the "City of Light," Paris is a UNESCO World Heritage site and a major center for fashion, entertainment, and arts. It is also a popular tourist destination, known for its famous cafes, food, and nightlife. 
    
    Paris is a multicultural city with a rich history, including the Roman ruins, Gothic architecture, and the Louvre Museum. It also hosts many events and festivals throughout the year, attracting visitors from all over the world. Paris is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be dominated by the development of super-intelligent machines that can not only perform tasks with a high degree of accuracy and speed, but also have the ability to learn and adapt in novel and complex ways. Some possible future trends in AI include:
    
    1. Increased use of AI in healthcare: As AI becomes more advanced, it is expected to play a more important role in healthcare, particularly in areas such as diagnosis, treatment, and personalized medicine.
    
    2. Increased focus on ethical and social implications of AI: As AI becomes more advanced and sophisticated, it is likely that we will see more emphasis on the ethical and social implications of AI use


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

    'm

     

    2

    5

     years

     old

    ,

     with

     an

     outgoing

     personality

     and

     a

     sense

     of

     humor

    .

     I

     enjoy

     spending

     time

     with

     friends

     and

     family

    ,

     and

     I

     love

     trying

     new

     foods

     and

     exploring

     new

     places

    .

     I

    'm

     passionate

     about

     literature

    ,

     and

     I

     love

     to

     read

    .

     I

    'm

     always

     looking

     for

     new

     and

     exciting

     ways

     to

     learn

     and

     grow

    ,

     and

     I

    'm

     always

     looking

     for

     new

     experiences

     to

     try

    .

     I

    'm

     confident

     in

     myself

    ,

     and

     I

    'm

     always

     eager

     to

     improve

    .

     What

    's

     your

     name

    ?

     What

    's

     your

     profession

    ?

     What

    's

     your

     favorite

     book

     or

     movie

     to

     watch

    ?

     What

    's

     your

     biggest

     dream

    ?

     What

    's

     your

     favorite

     hobby

    ?

     What

    's

     your

     biggest

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    Paris

     is

     the

     largest

     city

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

     on

     the

     right

     bank

     of

     the

     Se

    ine

     River

    ,

     and

     is

     the

     fifth

     largest

     city

     in

     the

     European

     Union

     by

     population

    .

     It

     is

     known

     for

     its

     rich

     history

    ,

     stunning

     architecture

    ,

     and

     vibrant

     culture

    .

     The

     city

     has

     a

     rich

     cultural

     heritage

    ,

     including

     art

    ,

     music

    ,

     and

     literature

    .

     Paris

     is

     also

     famous

     for

     its

     fashion

    ,

     food

    ,

     and

     nightlife

    .

     Paris

     is

     known

     as

     the

     "

    City

     of

     Love

    "

     and

     is

     home

     to

     many

     famous

     landmarks

     and

     attractions

    .

     It

     is

     a

     major

     economic

     and

     cultural

     center

    ,

     and

     continues

     to

     be

     a

     dynamic

     and

     vibrant

     city

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     uncertain

    ,

     but

     there

     are

     several

     potential

     trends

     that

     could

     shape

     its

     development

     in

     the

     coming

     decades

    .

     Here

     are

     some

     of

     the

     most

     likely

     developments

    :
    


    1

    .

     Increased

     focus

     on

     ethical

     considerations

    :

     As

     more

     people

     become

     aware

     of

     the

     potential

     risks

     and

     benefits

     of

     AI

    ,

     there

     will

     be

     increasing

     pressure

     to

     prioritize

     ethical

     considerations

     from

     the

     start

     of

     AI

     development

    .

     This

     could

     lead

     to

     greater

     regulation

     and

     scrutiny

     of

     AI

     systems

    ,

     as

     well

     as

     increased

     scrutiny

     of

     their

     potential

     impact

     on

     society

    .
    


    2

    .

     More

     advanced

     AI

     algorithms

    :

     As

     AI

     systems

     become

     more

     complex

     and

     sophisticated

    ,

     they

     will

     likely

     become

     even

     more

     capable

     of

     learning

     and

     adapting

    .

     This

     could

     lead

     to

     significant

     advancements

     in

     areas

     like

     image

    



```python
llm.shutdown()
```
