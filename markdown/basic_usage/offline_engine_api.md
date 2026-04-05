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


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.85it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.85it/s]


    2026-04-05 15:25:33,681 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-05 15:25:33] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:18,  2.44s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:18,  2.44s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:18,  2.44s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:18,  2.44s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:26,  2.04it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.23it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.23it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.23it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.23it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.23it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.23it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.23it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.23it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.23it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:07,  6.23it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.94it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.94it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.94it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.94it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.94it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.94it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.94it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 13.94it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 20.31it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 20.31it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 20.31it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 20.31it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 20.31it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 20.31it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 20.31it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:02<00:01, 20.31it/s]Compiling num tokens (num_tokens=352):  45%|████▍     | 26/58 [00:02<00:01, 20.31it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:02<00:00, 28.52it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:02<00:00, 28.52it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:02<00:00, 28.52it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:00, 28.52it/s]

    Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:00, 28.52it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:03<00:00, 28.52it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:03<00:00, 28.52it/s]Compiling num tokens (num_tokens=192):  59%|█████▊    | 34/58 [00:03<00:00, 28.52it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:03<00:00, 33.75it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:03<00:00, 33.75it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:03<00:00, 33.75it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:03<00:00, 33.75it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:03<00:00, 33.75it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:03<00:00, 33.75it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:03<00:00, 33.75it/s] Compiling num tokens (num_tokens=80):  71%|███████   | 41/58 [00:03<00:00, 33.75it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:03<00:00, 40.16it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:03<00:00, 40.16it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:03<00:00, 40.16it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:03<00:00, 40.16it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:03<00:00, 40.16it/s]

    Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:03<00:00, 40.16it/s]Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:03<00:00, 40.16it/s]Compiling num tokens (num_tokens=16):  83%|████████▎ | 48/58 [00:03<00:00, 40.16it/s]Compiling num tokens (num_tokens=12):  83%|████████▎ | 48/58 [00:03<00:00, 40.16it/s]Compiling num tokens (num_tokens=8):  83%|████████▎ | 48/58 [00:03<00:00, 40.16it/s] Compiling num tokens (num_tokens=4):  83%|████████▎ | 48/58 [00:03<00:00, 40.16it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.50it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.12 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.09 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.09 GB):   3%|▎         | 2/58 [00:00<00:02, 19.28it/s]Capturing num tokens (num_tokens=7168 avail_mem=76.09 GB):   3%|▎         | 2/58 [00:00<00:02, 19.28it/s]Capturing num tokens (num_tokens=6656 avail_mem=76.09 GB):   3%|▎         | 2/58 [00:00<00:02, 19.28it/s]Capturing num tokens (num_tokens=6144 avail_mem=76.09 GB):   3%|▎         | 2/58 [00:00<00:02, 19.28it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=76.09 GB):   9%|▊         | 5/58 [00:00<00:02, 22.69it/s]Capturing num tokens (num_tokens=5632 avail_mem=76.08 GB):   9%|▊         | 5/58 [00:00<00:02, 22.69it/s]Capturing num tokens (num_tokens=5120 avail_mem=76.09 GB):   9%|▊         | 5/58 [00:00<00:02, 22.69it/s]Capturing num tokens (num_tokens=4608 avail_mem=76.08 GB):   9%|▊         | 5/58 [00:00<00:02, 22.69it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.08 GB):   9%|▊         | 5/58 [00:00<00:02, 22.69it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.08 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.87it/s]Capturing num tokens (num_tokens=3840 avail_mem=76.08 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.87it/s]Capturing num tokens (num_tokens=3584 avail_mem=76.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.87it/s]Capturing num tokens (num_tokens=3328 avail_mem=76.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.87it/s]Capturing num tokens (num_tokens=3072 avail_mem=76.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.87it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=76.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.87it/s]Capturing num tokens (num_tokens=2816 avail_mem=76.07 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.76it/s]Capturing num tokens (num_tokens=2560 avail_mem=76.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.76it/s]Capturing num tokens (num_tokens=2304 avail_mem=76.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.76it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.76it/s]Capturing num tokens (num_tokens=1792 avail_mem=76.05 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.76it/s]Capturing num tokens (num_tokens=1536 avail_mem=76.05 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.76it/s]Capturing num tokens (num_tokens=1280 avail_mem=76.04 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.76it/s]Capturing num tokens (num_tokens=1280 avail_mem=76.04 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.49it/s]Capturing num tokens (num_tokens=1024 avail_mem=76.02 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.49it/s]Capturing num tokens (num_tokens=960 avail_mem=76.04 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.49it/s] Capturing num tokens (num_tokens=896 avail_mem=76.04 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.49it/s]Capturing num tokens (num_tokens=832 avail_mem=76.03 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.49it/s]

    Capturing num tokens (num_tokens=768 avail_mem=76.03 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.49it/s]Capturing num tokens (num_tokens=704 avail_mem=76.03 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.49it/s]Capturing num tokens (num_tokens=704 avail_mem=76.03 GB):  45%|████▍     | 26/58 [00:00<00:00, 45.42it/s]Capturing num tokens (num_tokens=640 avail_mem=76.02 GB):  45%|████▍     | 26/58 [00:00<00:00, 45.42it/s]Capturing num tokens (num_tokens=576 avail_mem=76.02 GB):  45%|████▍     | 26/58 [00:00<00:00, 45.42it/s]Capturing num tokens (num_tokens=512 avail_mem=76.01 GB):  45%|████▍     | 26/58 [00:00<00:00, 45.42it/s]Capturing num tokens (num_tokens=480 avail_mem=76.03 GB):  45%|████▍     | 26/58 [00:00<00:00, 45.42it/s]Capturing num tokens (num_tokens=448 avail_mem=76.02 GB):  45%|████▍     | 26/58 [00:00<00:00, 45.42it/s]Capturing num tokens (num_tokens=416 avail_mem=76.02 GB):  45%|████▍     | 26/58 [00:00<00:00, 45.42it/s]Capturing num tokens (num_tokens=416 avail_mem=76.02 GB):  55%|█████▌    | 32/58 [00:00<00:00, 48.67it/s]Capturing num tokens (num_tokens=384 avail_mem=76.02 GB):  55%|█████▌    | 32/58 [00:00<00:00, 48.67it/s]Capturing num tokens (num_tokens=352 avail_mem=76.01 GB):  55%|█████▌    | 32/58 [00:00<00:00, 48.67it/s]Capturing num tokens (num_tokens=320 avail_mem=76.01 GB):  55%|█████▌    | 32/58 [00:00<00:00, 48.67it/s]Capturing num tokens (num_tokens=288 avail_mem=76.01 GB):  55%|█████▌    | 32/58 [00:00<00:00, 48.67it/s]

    Capturing num tokens (num_tokens=256 avail_mem=76.00 GB):  55%|█████▌    | 32/58 [00:00<00:00, 48.67it/s]Capturing num tokens (num_tokens=240 avail_mem=76.00 GB):  55%|█████▌    | 32/58 [00:00<00:00, 48.67it/s]Capturing num tokens (num_tokens=240 avail_mem=76.00 GB):  66%|██████▌   | 38/58 [00:00<00:00, 50.77it/s]Capturing num tokens (num_tokens=224 avail_mem=76.00 GB):  66%|██████▌   | 38/58 [00:00<00:00, 50.77it/s]Capturing num tokens (num_tokens=208 avail_mem=76.00 GB):  66%|██████▌   | 38/58 [00:00<00:00, 50.77it/s]Capturing num tokens (num_tokens=192 avail_mem=76.00 GB):  66%|██████▌   | 38/58 [00:00<00:00, 50.77it/s]Capturing num tokens (num_tokens=176 avail_mem=75.99 GB):  66%|██████▌   | 38/58 [00:00<00:00, 50.77it/s]Capturing num tokens (num_tokens=160 avail_mem=75.99 GB):  66%|██████▌   | 38/58 [00:00<00:00, 50.77it/s]Capturing num tokens (num_tokens=144 avail_mem=75.98 GB):  66%|██████▌   | 38/58 [00:00<00:00, 50.77it/s]Capturing num tokens (num_tokens=144 avail_mem=75.98 GB):  76%|███████▌  | 44/58 [00:01<00:00, 52.25it/s]Capturing num tokens (num_tokens=128 avail_mem=75.98 GB):  76%|███████▌  | 44/58 [00:01<00:00, 52.25it/s]Capturing num tokens (num_tokens=112 avail_mem=75.98 GB):  76%|███████▌  | 44/58 [00:01<00:00, 52.25it/s]Capturing num tokens (num_tokens=96 avail_mem=75.98 GB):  76%|███████▌  | 44/58 [00:01<00:00, 52.25it/s] Capturing num tokens (num_tokens=80 avail_mem=75.97 GB):  76%|███████▌  | 44/58 [00:01<00:00, 52.25it/s]

    Capturing num tokens (num_tokens=64 avail_mem=75.97 GB):  76%|███████▌  | 44/58 [00:01<00:00, 52.25it/s]Capturing num tokens (num_tokens=48 avail_mem=75.97 GB):  76%|███████▌  | 44/58 [00:01<00:00, 52.25it/s]Capturing num tokens (num_tokens=48 avail_mem=75.97 GB):  86%|████████▌ | 50/58 [00:01<00:00, 52.95it/s]Capturing num tokens (num_tokens=32 avail_mem=75.96 GB):  86%|████████▌ | 50/58 [00:01<00:00, 52.95it/s]Capturing num tokens (num_tokens=28 avail_mem=75.96 GB):  86%|████████▌ | 50/58 [00:01<00:00, 52.95it/s]Capturing num tokens (num_tokens=24 avail_mem=75.95 GB):  86%|████████▌ | 50/58 [00:01<00:00, 52.95it/s]Capturing num tokens (num_tokens=20 avail_mem=75.95 GB):  86%|████████▌ | 50/58 [00:01<00:00, 52.95it/s]Capturing num tokens (num_tokens=16 avail_mem=75.95 GB):  86%|████████▌ | 50/58 [00:01<00:00, 52.95it/s]Capturing num tokens (num_tokens=12 avail_mem=75.94 GB):  86%|████████▌ | 50/58 [00:01<00:00, 52.95it/s]Capturing num tokens (num_tokens=12 avail_mem=75.94 GB):  97%|█████████▋| 56/58 [00:01<00:00, 53.54it/s]Capturing num tokens (num_tokens=8 avail_mem=75.94 GB):  97%|█████████▋| 56/58 [00:01<00:00, 53.54it/s] Capturing num tokens (num_tokens=4 avail_mem=75.94 GB):  97%|█████████▋| 56/58 [00:01<00:00, 53.54it/s]Capturing num tokens (num_tokens=4 avail_mem=75.94 GB): 100%|██████████| 58/58 [00:01<00:00, 45.96it/s]


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
    Generated text:  Georgi, a 19-year-old high school student majoring in Biochemistry. I am a graduate student of the MIT Biochem PhD program and I am applying for a postdoc position. My goal is to study the role of the R111K mutation in Alzheimer's disease.
    My work is based on a genetic study of the R111K mutation. We have successfully linked the R111K mutation to an Alzheimer's disease causing variant of APOE4. The R111K mutation is an insular, commonly occurring mutation in the APOE4 gene. The mutation causes the
    ===============================
    Prompt: The president of the United States is
    Generated text:  a president who is a native-born American of what age and gender? The president of the United States is a president who is a native-born American of 45 years and gender male. This is because the Constitution of the United States requires that all citizens be of full age and gender, specifically stating that the president must be a "native-born citizen of the United States." The term "native-born" is used to refer to someone born within the United States, regardless of where their parents were born. Since the president is a 45-year-old male, they meet all the necessary criteria to be considered a qualified and qualified president.
    ===============================
    Prompt: The capital of France is
    Generated text: :
    A) Paris
    B) Nice
    C) Lyon
    D) Nice
    
    To determine the capital of France, we can follow these steps:
    
    1. Identify the question: The capital of France is a required answer.
    2. Recall the capital cities of France: The capital of France is Paris.
    3. Verify that the capital is correct: Paris is indeed the capital of France.
    4. Provide the answer: The capital of France is Paris.
    
    Therefore, the correct answer is \boxed{A}.
    ===============================
    Prompt: The future of AI is
    Generated text:  highly uncertain. While it will be very difficult to predict the extent of the change that it will bring to our lives, there is a clear and consistent message from the international community, that AI will have a profound impact on the way we live and work. However, it’s also important to note that there are still several challenges that the technology will need to overcome before it can have a truly beneficial impact on the world.
    
    One of the biggest challenges that AI faces is the need to maintain ethical and moral standards in the field. As AI systems become more sophisticated, the moral questions that arise will continue to grow. How will an AI system be


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a cultural and economic hub, with a rich history dating back to the Roman Empire and a modern city that has undergone significant development over the centuries. Paris is home to many world-renowned museums, art galleries, and theaters, and is a popular tourist destination for its beautiful architecture and vibrant culture. The city is also known for its food scene, with many famous restaurants and cafes serving traditional French cuisine. Overall, Paris is a city of contrasts and beauty that has captured the hearts of people from all
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction. Here are some of the most likely trends:
    
    1. Increased focus on ethical AI: As more people become aware of the potential risks of AI, there will be a greater emphasis on developing AI that is designed to be ethical and responsible. This could involve developing AI that is designed to minimize harm to individuals or society as a whole, or that is designed to be transparent and accountable.
    
    2. Greater integration with other technologies: AI is likely to become more integrated with other technologies, such as machine learning, natural language processing, and computer vision. This
    


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
    Generated text:  [Name] and I'm a [Age] year old self-proclaimed "Ex-Stranger". I'm an aspiring novelist, aspiring [insert an occupation or hobby here, such as writing, music, or social media] and a big fan of [insert a favorite author, song, or movie, if you know one].
    
    I'm a storyteller by profession, but I also love to write in my free time. My goal is to bring the world of fantasy and adventure to life, and I believe that literature can be a powerful tool for self-expression and personal growth. I also love to read, travel, and try new
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as "La République française." It is the largest city in France and one of the largest in the world by population. It is home to many iconic landmarks such as Notre-Dame Cathedral, the Eiffel Tower, and the Louvre Museum. The city is also known for its food culture, with famous dishes like Poutine and beignets. The city is home to many museums and attractions, including the Musée d'Orsay, the Musée d'Orsay, and the Louvre. Finally, Paris is a cultural hub with many art galleries, theaters, and concert halls. The
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be characterized by increasing integration with other technologies, especially in the areas of robotics, machine learning, and natural language processing. AI is also expected to continue to evolve, with new forms of AI being developed to solve complex problems that were previously beyond the scope of AI.
    
    One potential future trend is the development of AI that is more capable of understanding and appreciating emotions, which could lead to more compassionate and empathetic AI. AI could also be used to create more intelligent and personalized assistants, such as virtual assistants that can learn and adapt to the user's preferences and needs.
    
    Another trend is the development of AI that is more autonomous and


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

    role

    ]

     of

     [

    insert

     profession

    ]

     with

     over

     [

    number

    ]

     of

     years

     of

     experience

     in

     the

     industry

    .

     I

     have

     [

    number

    ]

     of

     years

     of

     hands

    -on

     experience

     in

     [

    insert

     field

    ]

     and

     have

     a

     keen

     eye

     for

     detail

    ,

     quick

     thinking

    ,

     and

     a

     strong

     work

     ethic

    .

     I

     am

     always

     looking

     for

     new

     opportunities

     to

     improve

     my

     skills

     and

     keep

     up

     with

     the

     latest

     trends

     in

     the

     field

    .

     I

     am

     a

     [

    insert

     occupation

    ]

     who

     enjoys

     [

    insert

     hobby

     or

     passion

    ],

     and

     I

    'm

     always

     ready

     to

     learn

     and

     grow

    .

     I

    'm

     a

     [

    insert

     personality

     trait

    ]

     and

     always

     try

     to

     make

     others

     happy

    .

     How

     would

     you

     describe

    
    
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

     France

     and

     the

     seat

     of

     the

     French

     government

     and

     the

     head

     of

     state

    .

     The

     city

     is

     known

     for

     its

     rich

     history

    ,

     artistic

     and

     cultural

     heritage

    ,

     and

     iconic

     landmarks

     such

     as

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

    -D

    ame

     Cathedral

    ,

     and

     the

     Ch

    amps

    -

    É

    lys

    ées

    .

     Paris

     is

     also

     home

     to

     the

     Lou

    vre

     Museum

    ,

     the

     most

     visited

     art

     museum

     in

     the

     world

    ,

     and

     the

     world

    's

     oldest

     continuously

     inhabited

     city

    .

     Its

     location

     on

     the

     Mediterranean

     Sea

    ,

     near

     the

     Atlantic

     Ocean

    ,

     makes

     it

     a

     major

     port

     and

     international

     financial

     center

    .

     Paris

     has

     a

     unique

     blend

     of

     traditional

     French

     culture

     and

     modern

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     incredibly

     exciting

     and

     unpredictable

    .

     Here

     are

     some

     possible

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

     Increased

     emphasis

     on

     ethical

     considerations

    :

     With

     the

     increasing

     number

     of

     ethical

     and

     social

     issues

     facing

     society

    ,

     there

     is

     a

     growing

     emphasis

     on

     AI

     to

     ensure

     that

     it

     is

     used

     eth

    ically

     and

     responsibly

    .

     This

     includes

     ensuring

     that

     AI

     systems

     are

     transparent

    ,

     accountable

    ,

     and

     responsible

    ,

     and

     that

     they

     do

     not

     harm

     individuals

     or

     communities

    .
    


    2

    .

     AI

     will

     become

     more

     sophisticated

    :

     As

     AI

     technology

     continues

     to

     advance

    ,

     we

     can

     expect

     to

     see

     more

     sophisticated

     and

     intelligent

     AI

     systems

    .

     This

     will

     likely

     lead

     to

     a

     greater

     ability

     to

     automate

     and

     optimize

     processes

    ,

     as

     well

     as

     to

     make

    



```python
llm.shutdown()
```
