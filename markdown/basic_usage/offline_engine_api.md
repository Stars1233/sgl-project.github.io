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

    Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  3.76it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  3.75it/s]


    2026-04-04 10:48:00,846 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 10:48:00] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:27,  2.60s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:27,  2.60s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:27,  2.60s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:02<00:39,  1.39it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:02<00:39,  1.39it/s]Compiling num tokens (num_tokens=6144):   5%|▌         | 3/58 [00:02<00:39,  1.39it/s]Compiling num tokens (num_tokens=5632):   5%|▌         | 3/58 [00:02<00:39,  1.39it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:02<00:15,  3.34it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:02<00:15,  3.34it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:02<00:15,  3.34it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:02<00:15,  3.34it/s]Compiling num tokens (num_tokens=3840):  10%|█         | 6/58 [00:02<00:15,  3.34it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.59it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.59it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.59it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.59it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.59it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:03<00:07,  6.59it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:03<00:03, 11.33it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:03<00:03, 11.33it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:03<00:03, 11.33it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:03<00:03, 11.33it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:03<00:03, 11.33it/s]Compiling num tokens (num_tokens=1280):  26%|██▌       | 15/58 [00:03<00:03, 11.33it/s]Compiling num tokens (num_tokens=1024):  26%|██▌       | 15/58 [00:03<00:03, 11.33it/s]Compiling num tokens (num_tokens=960):  26%|██▌       | 15/58 [00:03<00:03, 11.33it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:01, 19.11it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:01, 19.11it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:01, 19.11it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:01, 19.11it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:01, 19.11it/s]Compiling num tokens (num_tokens=640):  38%|███▊      | 22/58 [00:03<00:01, 19.11it/s]

    Compiling num tokens (num_tokens=576):  38%|███▊      | 22/58 [00:03<00:01, 19.11it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:03<00:01, 25.22it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:03<00:01, 25.22it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:03<00:01, 25.22it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:01, 25.22it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:03<00:01, 25.22it/s]Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:03<00:01, 25.22it/s]Compiling num tokens (num_tokens=352):  48%|████▊     | 28/58 [00:03<00:01, 25.22it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:03<00:00, 31.09it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:03<00:00, 31.09it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:03<00:00, 31.09it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:00, 31.09it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:00, 31.09it/s]

    Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:03<00:00, 31.09it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:03<00:00, 31.09it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:03<00:00, 36.40it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:03<00:00, 36.40it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:03<00:00, 36.40it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:03<00:00, 36.40it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:03<00:00, 36.40it/s]Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:03<00:00, 36.40it/s]Compiling num tokens (num_tokens=112):  69%|██████▉   | 40/58 [00:03<00:00, 36.40it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:03<00:00, 40.36it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:03<00:00, 40.36it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:03<00:00, 40.36it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:03<00:00, 40.36it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:03<00:00, 40.36it/s]

    Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:03<00:00, 40.36it/s]Compiling num tokens (num_tokens=28):  79%|███████▉  | 46/58 [00:03<00:00, 40.36it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:03<00:00, 44.72it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:03<00:00, 44.72it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:03<00:00, 44.72it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:03<00:00, 44.72it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:03<00:00, 44.72it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:03<00:00, 44.72it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:03<00:00, 44.72it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.40it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=56.50 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=56.47 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=56.47 GB):   3%|▎         | 2/58 [00:00<00:02, 18.91it/s]Capturing num tokens (num_tokens=7168 avail_mem=56.47 GB):   3%|▎         | 2/58 [00:00<00:02, 18.91it/s]Capturing num tokens (num_tokens=6656 avail_mem=56.46 GB):   3%|▎         | 2/58 [00:00<00:02, 18.91it/s]Capturing num tokens (num_tokens=6144 avail_mem=56.47 GB):   3%|▎         | 2/58 [00:00<00:02, 18.91it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=56.47 GB):   9%|▊         | 5/58 [00:00<00:02, 22.52it/s]Capturing num tokens (num_tokens=5632 avail_mem=56.46 GB):   9%|▊         | 5/58 [00:00<00:02, 22.52it/s]Capturing num tokens (num_tokens=5120 avail_mem=56.46 GB):   9%|▊         | 5/58 [00:00<00:02, 22.52it/s]Capturing num tokens (num_tokens=4608 avail_mem=56.46 GB):   9%|▊         | 5/58 [00:00<00:02, 22.52it/s]Capturing num tokens (num_tokens=4096 avail_mem=56.46 GB):   9%|▊         | 5/58 [00:00<00:02, 22.52it/s]Capturing num tokens (num_tokens=4096 avail_mem=56.46 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.72it/s]Capturing num tokens (num_tokens=3840 avail_mem=56.45 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.72it/s]Capturing num tokens (num_tokens=3584 avail_mem=56.45 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.72it/s]Capturing num tokens (num_tokens=3328 avail_mem=56.45 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.72it/s]Capturing num tokens (num_tokens=3072 avail_mem=56.44 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.72it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=56.44 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.72it/s]Capturing num tokens (num_tokens=2816 avail_mem=56.44 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.88it/s]Capturing num tokens (num_tokens=2560 avail_mem=56.44 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.88it/s]Capturing num tokens (num_tokens=2304 avail_mem=56.43 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.88it/s]Capturing num tokens (num_tokens=2048 avail_mem=56.43 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.88it/s]Capturing num tokens (num_tokens=1792 avail_mem=56.43 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.88it/s]Capturing num tokens (num_tokens=1536 avail_mem=56.42 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.88it/s]Capturing num tokens (num_tokens=1280 avail_mem=56.42 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.88it/s]Capturing num tokens (num_tokens=1280 avail_mem=56.42 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.03it/s]Capturing num tokens (num_tokens=1024 avail_mem=56.40 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.03it/s]Capturing num tokens (num_tokens=960 avail_mem=56.41 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.03it/s] Capturing num tokens (num_tokens=896 avail_mem=56.41 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.03it/s]Capturing num tokens (num_tokens=832 avail_mem=56.41 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.03it/s]

    Capturing num tokens (num_tokens=768 avail_mem=56.40 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.03it/s]Capturing num tokens (num_tokens=704 avail_mem=56.40 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.03it/s]Capturing num tokens (num_tokens=704 avail_mem=56.40 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.21it/s]Capturing num tokens (num_tokens=640 avail_mem=56.40 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.21it/s]Capturing num tokens (num_tokens=576 avail_mem=56.40 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.21it/s]Capturing num tokens (num_tokens=512 avail_mem=56.39 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.21it/s]Capturing num tokens (num_tokens=480 avail_mem=56.40 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.21it/s]Capturing num tokens (num_tokens=448 avail_mem=56.40 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.21it/s]Capturing num tokens (num_tokens=416 avail_mem=56.40 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.21it/s]Capturing num tokens (num_tokens=416 avail_mem=56.40 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.35it/s]Capturing num tokens (num_tokens=384 avail_mem=56.40 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.35it/s]Capturing num tokens (num_tokens=352 avail_mem=56.39 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.35it/s]Capturing num tokens (num_tokens=320 avail_mem=56.39 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.35it/s]

    Capturing num tokens (num_tokens=288 avail_mem=56.38 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.35it/s]Capturing num tokens (num_tokens=256 avail_mem=56.38 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.35it/s]Capturing num tokens (num_tokens=240 avail_mem=56.38 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.35it/s]Capturing num tokens (num_tokens=240 avail_mem=56.38 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.24it/s]Capturing num tokens (num_tokens=224 avail_mem=56.37 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.24it/s]Capturing num tokens (num_tokens=208 avail_mem=56.37 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.24it/s]Capturing num tokens (num_tokens=192 avail_mem=56.37 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.24it/s]Capturing num tokens (num_tokens=176 avail_mem=56.37 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.24it/s]Capturing num tokens (num_tokens=160 avail_mem=56.36 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.24it/s]Capturing num tokens (num_tokens=144 avail_mem=56.36 GB):  66%|██████▌   | 38/58 [00:01<00:00, 49.24it/s]Capturing num tokens (num_tokens=144 avail_mem=56.36 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.48it/s]Capturing num tokens (num_tokens=128 avail_mem=56.36 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.48it/s]Capturing num tokens (num_tokens=112 avail_mem=56.35 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.48it/s]

    Capturing num tokens (num_tokens=96 avail_mem=56.35 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.48it/s] Capturing num tokens (num_tokens=80 avail_mem=56.35 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.48it/s]Capturing num tokens (num_tokens=64 avail_mem=56.34 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.48it/s]Capturing num tokens (num_tokens=48 avail_mem=56.34 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.48it/s]Capturing num tokens (num_tokens=48 avail_mem=56.34 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.94it/s]Capturing num tokens (num_tokens=32 avail_mem=56.34 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.94it/s]Capturing num tokens (num_tokens=28 avail_mem=56.33 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.94it/s]Capturing num tokens (num_tokens=24 avail_mem=56.33 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.94it/s]Capturing num tokens (num_tokens=20 avail_mem=56.32 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.94it/s]Capturing num tokens (num_tokens=16 avail_mem=56.32 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.94it/s]Capturing num tokens (num_tokens=12 avail_mem=56.32 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.94it/s]Capturing num tokens (num_tokens=12 avail_mem=56.32 GB):  97%|█████████▋| 56/58 [00:01<00:00, 51.56it/s]Capturing num tokens (num_tokens=8 avail_mem=56.32 GB):  97%|█████████▋| 56/58 [00:01<00:00, 51.56it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=56.31 GB):  97%|█████████▋| 56/58 [00:01<00:00, 51.56it/s]Capturing num tokens (num_tokens=4 avail_mem=56.31 GB): 100%|██████████| 58/58 [00:01<00:00, 44.74it/s]


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
    Generated text:  Lisa and I am a student in the College of Arts and Sciences. I am an environmental science student and this semester I am in the Environmental Science course, which is conducted by Professor Armando S. Gutierrez.
    What type of subject did Lisa study? 
    Answer:
    
    Lisa studied Environmental Science, which is a course taught by Professor Armando S. Gutierrez in the College of Arts and Sciences. She is a student and part of a group of students in the course, focusing on environmental issues and sustainable practices. The course is designed to provide students with knowledge and skills necessary for understanding and addressing environmental challenges. Lisa aims to develop her environmental
    ===============================
    Prompt: The president of the United States is
    Generated text:  a high-ranking government official and a member of the executive branch of the government of the United States of America. His/her primary responsibility is to run the country in accordance with the Constitution of the United States of America. His/her office is a national symbol of the government of the United States of America.
    The president of the United States, who is not a US citizen, will be declared dead if he/she dies. However, the US government and legal system will continue to operate on the death of the president. The US government will continue to pay the president's bills. The US government may also pay the estate of the president's family.
    ===============================
    Prompt: The capital of France is
    Generated text:  the city of Paris. Which of the following statements is incorrect?
    A. It is located in the western part of France.
    B. It is surrounded by the Seine river.
    C. It is located in the central part of the country.
    D. It is a city of moderate size.
    Answer:
    
    C
    
    The Chinese government has always been committed to the building of a community with a shared future for mankind, actively promoting the building of a new type of international relations characterized by ____. ① mutual respect ② mutual benefit and win-win ③ win-win cooperation ④ consultation and joint construction
    A. 
    ===============================
    Prompt: The future of AI is
    Generated text:  uncertain, but it is clear that the technology will continue to change the way we live and work. AI will allow for more efficient use of resources and will allow for the creation of more advanced and effective solutions to some of the world's most pressing issues. However, as with any technology, there are also potential risks and challenges to consider.
    
    One of the biggest concerns is the potential for AI to be used to discriminate against certain groups of people. The use of biased algorithms and the potential for AI systems to perpetuate existing biases in society are just two examples of potential risks that need to be addressed.
    
    Another concern is the potential for AI to


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [insert a brief description of your profession or role]. I enjoy [insert a short description of your hobbies or interests]. What brings you to this company? I'm drawn to [insert a reason why you're interested in this company]. What's your favorite part of your job? I love [insert a reason why you love your job]. What's your favorite part of your life? I love [insert a reason why you love your life].
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also the birthplace of French literature, art, and music, and is a major center for business, politics, and culture. Paris is a bustling metropolis with a rich history and a diverse population of over 2.5 million people. It is the largest city in France and the second-largest city in the world by population. The city is known for its fashion, food, and wine, and is a popular tourist destination. Paris is a cultural and artistic center that has been a
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased integration with human intelligence: As AI becomes more sophisticated, it is likely to become more integrated with human intelligence, allowing it to learn and adapt to new situations more effectively.
    
    2. Greater use of machine learning: Machine learning is likely to become more prevalent, with more sophisticated algorithms and models being developed to improve AI performance.
    
    3. Increased focus on ethical considerations: As AI becomes more integrated with human intelligence, there will be increased focus on ethical considerations, such as privacy, bias, and accountability.
    
    4. Greater use of AI in healthcare: AI is likely to play a greater role
    


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
    Generated text:  [Your Name] and I'm a [Career Stage] student at [Your School]. I'm passionate about [Your Career Goal] and I'm always looking to grow and improve myself. I'm always available to help when needed and I'm always ready to learn and grow. I'm confident and I'm excited to work with anyone who has the opportunity to learn and grow with me. Have a great day! 
    
    Yourself: [Your Name] (give any additional information you would like to include)
    Career Stage: [Career Stage]
    Career Goal: [Your Career Goal]
    Areas of Interest: [Areas of Interest]
    Favorite Activities
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    Paris is the largest city in France and the cultural, political, and economic center of the country. It is also the world's most populous city, with an estimated population of 20 million. The city was founded in the 9th century and has a rich history dating back over 1,000 years. Paris is known for its iconic landmarks such as Notre-Dame Cathedral, Eiffel Tower, and the Louvre Museum. It is also home to the French Parliament and the European Parliament, and hosts numerous museums, theaters, and events throughout the year. Paris is a bustling hub of life, with
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  rapidly evolving, with many possibilities and potential areas for growth. Here are some possible trends to watch out for:
    
    1. Autonomous vehicles: As autonomous driving technology becomes more advanced, we can expect to see more self-driving cars on the road. This could revolutionize traffic flow and reduce accidents.
    
    2. Personalized healthcare: AI can be used to create more personalized health plans for individuals based on their genetic information, medical history, and lifestyle. This could lead to better disease prevention and more efficient treatments.
    
    3. Artificial intelligence in finance: AI can be used to analyze financial data, identify trends and patterns, and make investment decisions. This could


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

     a

     [

    profession

    ]

     who

     specialize

     in

     [

    career

     objective

    ]

     [

    Name

    ].

     I

     am

     [

    age

    ]

     years

     old

     and

     currently

     living

     in

     [

    city

    ,

     country

    ].

     In

     my

     spare

     time

    ,

     I

     enjoy

     reading

     books

    ,

     going

     for

     walks

    ,

     and

     playing

     sports

     such

     as

     [

    sports

    ].

     I

     am

     passionate

     about

     [

    interest

     or

     hobby

    ]

     and

     I

     am

     always

     looking

     for

     new

     challenges

     to

     pursue

    .

     What

    ’s

     your

     name

    ,

     and

     what

     do

     you

     do

    ?


    [

    Name

    ]

     is

     a

     [

    profession

    ],

     specializing

     in

     [

    career

     objective

    ]

     [

    Name

    ].

     [

    Name

    ]

     is

     a

     [

    age

    ]

     year

     old

    ,

     currently

     living

     in

     [

    city

    ,

     country

    ].

     In

     my

     spare

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     which

     is

     known

     for

     its

     vibrant

     culture

    ,

     rich

     history

    ,

     and

     iconic

     landmarks

     like

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

    -D

    ame

     Cathedral

    .

     Additionally

    ,

     Paris

     is

     renowned

     for

     its

     world

    -class

     fashion

     industry

    ,

     art

     scene

    ,

     and

     cuisine

    .

     It

    's

     also

     home

     to

     the

     iconic

     Mont

    mart

    re

     district

     and

     is

     a

     popular

     tourist

     destination

    ,

     known

     for

     its

     historical

     sites

    ,

     elegant

     museums

    ,

     and

     romantic

     atmosphere

    .

     Paris

    's

     status

     as

     one

     of

     the

     world

    's

     most

     dynamic

     cities

     is

     reflected

     by

     its

     high

    -standard

     transportation

     systems

    ,

     including

     its

     famous

     subway

     system

    .

     The

     city

     is

     also

     known

     for

     its

     diverse

     cultural

     events

    ,

     including

     concerts

    ,

     film

     festivals

    ,

     and

     literary

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     full

     of

     exciting

     possibilities

     and

     potential

     challenges

    ,

     and

     it

     is

     likely

     to

     continue

     evolving

     and

     changing

     in

     significant

     ways

    .

     Here

     are

     some

     potential

     trends

     to

     watch

     out

     for

     in

     the

     coming

     years

    :
    


    1

    .

     Increased

     development

     of

     AI

     ethics

    :

     With

     the

     increasing

     awareness

     of

     AI

    's

     impact

     on

     society

    ,

     there

     is

     a

     growing

     focus

     on

     creating

     ethical

     guidelines

     for

     AI

     development

    .

     This

     includes

     creating

     AI

     systems

     that

     respect

     human

     values

     and

     rights

    ,

     such

     as

     privacy

     and

     autonomy

    .

     However

    ,

     there

     is

     also

     a

     growing

     concern

     about

     the

     potential

     for

     AI

     to

     exacerb

    ate

     existing

     social

     and

     economic

     inequalities

    .
    


    2

    .

     Greater

     integration

     of

     AI

     in

     natural

     language

     processing

     (

    N

    LP

    ):

     The

     ability

     of

     AI

     to

     understand

     and

    



```python
llm.shutdown()
```
