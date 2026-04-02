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


    2026-04-02 05:36:15.597 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 05:36:15] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 05:36:15.597 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 05:36:15] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 05:36:15.597 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 05:36:15] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 05:36:15.597 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 05:36:15] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 05:36:15.597 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 05:36:15] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  4.91it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  4.91it/s]


    2026-04-02 05:36:20,249 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 05:36:20] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:29,  2.62s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:29,  2.62s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:29,  2.62s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:29,  2.62s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:28,  1.91it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:28,  1.91it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:28,  1.91it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:28,  1.91it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:28,  1.91it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:28,  1.91it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:28,  1.91it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.85it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.85it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.85it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.85it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.85it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.85it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.85it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.85it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.85it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:08,  5.85it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.15it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.15it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.15it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.15it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.15it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:02, 13.15it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:02, 13.15it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:02, 13.15it/s]Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:03<00:02, 13.15it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 20.35it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 20.35it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 20.35it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 20.35it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 20.35it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 20.35it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 20.35it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:03<00:01, 20.35it/s]Compiling num tokens (num_tokens=320):  47%|████▋     | 27/58 [00:03<00:01, 20.35it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 28.09it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 28.09it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:03<00:00, 28.09it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:03<00:00, 28.09it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:03<00:00, 28.09it/s]

    Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:03<00:00, 28.09it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:03<00:00, 28.09it/s]Compiling num tokens (num_tokens=176):  60%|██████    | 35/58 [00:03<00:00, 28.09it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:03<00:00, 34.56it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:03<00:00, 34.56it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:03<00:00, 34.56it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:03<00:00, 34.56it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:03<00:00, 34.56it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:03<00:00, 34.56it/s] Compiling num tokens (num_tokens=80):  72%|███████▏  | 42/58 [00:03<00:00, 34.56it/s]Compiling num tokens (num_tokens=64):  72%|███████▏  | 42/58 [00:03<00:00, 34.56it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 40.63it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 40.63it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 40.63it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 40.63it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 40.63it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 40.63it/s]

    Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 40.63it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 40.63it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:03<00:00, 40.63it/s] Compiling num tokens (num_tokens=4):  84%|████████▍ | 49/58 [00:03<00:00, 40.63it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.71it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=72.11 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=72.08 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=72.08 GB):   3%|▎         | 2/58 [00:00<00:02, 19.54it/s]Capturing num tokens (num_tokens=7168 avail_mem=72.08 GB):   3%|▎         | 2/58 [00:00<00:02, 19.54it/s]Capturing num tokens (num_tokens=6656 avail_mem=72.08 GB):   3%|▎         | 2/58 [00:00<00:02, 19.54it/s]Capturing num tokens (num_tokens=6144 avail_mem=72.08 GB):   3%|▎         | 2/58 [00:00<00:02, 19.54it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=72.08 GB):   9%|▊         | 5/58 [00:00<00:02, 22.88it/s]Capturing num tokens (num_tokens=5632 avail_mem=72.07 GB):   9%|▊         | 5/58 [00:00<00:02, 22.88it/s]Capturing num tokens (num_tokens=5120 avail_mem=72.07 GB):   9%|▊         | 5/58 [00:00<00:02, 22.88it/s]Capturing num tokens (num_tokens=4608 avail_mem=72.07 GB):   9%|▊         | 5/58 [00:00<00:02, 22.88it/s]Capturing num tokens (num_tokens=4096 avail_mem=72.07 GB):   9%|▊         | 5/58 [00:00<00:02, 22.88it/s]Capturing num tokens (num_tokens=4096 avail_mem=72.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.82it/s]Capturing num tokens (num_tokens=3840 avail_mem=72.07 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.82it/s]Capturing num tokens (num_tokens=3584 avail_mem=72.06 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.82it/s]Capturing num tokens (num_tokens=3328 avail_mem=72.06 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.82it/s]Capturing num tokens (num_tokens=3072 avail_mem=72.06 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.82it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=72.06 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.82it/s]Capturing num tokens (num_tokens=2816 avail_mem=72.06 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.86it/s]Capturing num tokens (num_tokens=2560 avail_mem=72.05 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.86it/s]Capturing num tokens (num_tokens=2304 avail_mem=72.05 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.86it/s]Capturing num tokens (num_tokens=2048 avail_mem=72.04 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.86it/s]Capturing num tokens (num_tokens=1792 avail_mem=72.04 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.86it/s]Capturing num tokens (num_tokens=1536 avail_mem=72.04 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.86it/s]Capturing num tokens (num_tokens=1280 avail_mem=72.03 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.86it/s]Capturing num tokens (num_tokens=1280 avail_mem=72.03 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.18it/s]Capturing num tokens (num_tokens=1024 avail_mem=72.01 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.18it/s]Capturing num tokens (num_tokens=960 avail_mem=72.03 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.18it/s] Capturing num tokens (num_tokens=896 avail_mem=72.02 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.18it/s]Capturing num tokens (num_tokens=832 avail_mem=72.02 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.18it/s]

    Capturing num tokens (num_tokens=768 avail_mem=72.02 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.18it/s]Capturing num tokens (num_tokens=704 avail_mem=72.01 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.18it/s]Capturing num tokens (num_tokens=704 avail_mem=72.01 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.64it/s]Capturing num tokens (num_tokens=640 avail_mem=72.01 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.64it/s]Capturing num tokens (num_tokens=576 avail_mem=72.01 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.64it/s]Capturing num tokens (num_tokens=512 avail_mem=72.00 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.64it/s]Capturing num tokens (num_tokens=480 avail_mem=72.01 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.64it/s]Capturing num tokens (num_tokens=448 avail_mem=72.01 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.64it/s]Capturing num tokens (num_tokens=416 avail_mem=72.01 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.64it/s]Capturing num tokens (num_tokens=416 avail_mem=72.01 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.68it/s]Capturing num tokens (num_tokens=384 avail_mem=72.01 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.68it/s]Capturing num tokens (num_tokens=352 avail_mem=72.00 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.68it/s]Capturing num tokens (num_tokens=320 avail_mem=72.00 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.68it/s]

    Capturing num tokens (num_tokens=288 avail_mem=71.99 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.68it/s]Capturing num tokens (num_tokens=256 avail_mem=71.99 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.68it/s]Capturing num tokens (num_tokens=240 avail_mem=71.99 GB):  55%|█████▌    | 32/58 [00:00<00:00, 47.68it/s]Capturing num tokens (num_tokens=240 avail_mem=71.99 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.46it/s]Capturing num tokens (num_tokens=224 avail_mem=71.99 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.46it/s]Capturing num tokens (num_tokens=208 avail_mem=71.98 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.46it/s]Capturing num tokens (num_tokens=192 avail_mem=71.98 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.46it/s]Capturing num tokens (num_tokens=176 avail_mem=71.98 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.46it/s]Capturing num tokens (num_tokens=160 avail_mem=71.98 GB):  66%|██████▌   | 38/58 [00:00<00:00, 49.46it/s]Capturing num tokens (num_tokens=144 avail_mem=71.97 GB):  66%|██████▌   | 38/58 [00:01<00:00, 49.46it/s]Capturing num tokens (num_tokens=144 avail_mem=71.97 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.75it/s]Capturing num tokens (num_tokens=128 avail_mem=71.97 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.75it/s]Capturing num tokens (num_tokens=112 avail_mem=71.97 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.75it/s]

    Capturing num tokens (num_tokens=96 avail_mem=71.96 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.75it/s] Capturing num tokens (num_tokens=80 avail_mem=71.96 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.75it/s]Capturing num tokens (num_tokens=64 avail_mem=71.96 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.75it/s]Capturing num tokens (num_tokens=48 avail_mem=71.88 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.75it/s]Capturing num tokens (num_tokens=48 avail_mem=71.88 GB):  86%|████████▌ | 50/58 [00:01<00:00, 45.48it/s]Capturing num tokens (num_tokens=32 avail_mem=71.66 GB):  86%|████████▌ | 50/58 [00:01<00:00, 45.48it/s]Capturing num tokens (num_tokens=28 avail_mem=71.65 GB):  86%|████████▌ | 50/58 [00:01<00:00, 45.48it/s]

    Capturing num tokens (num_tokens=24 avail_mem=71.65 GB):  86%|████████▌ | 50/58 [00:01<00:00, 45.48it/s]Capturing num tokens (num_tokens=20 avail_mem=71.64 GB):  86%|████████▌ | 50/58 [00:01<00:00, 45.48it/s]Capturing num tokens (num_tokens=16 avail_mem=71.64 GB):  86%|████████▌ | 50/58 [00:01<00:00, 45.48it/s]Capturing num tokens (num_tokens=16 avail_mem=71.64 GB):  95%|█████████▍| 55/58 [00:01<00:00, 35.45it/s]Capturing num tokens (num_tokens=12 avail_mem=71.64 GB):  95%|█████████▍| 55/58 [00:01<00:00, 35.45it/s]Capturing num tokens (num_tokens=8 avail_mem=71.64 GB):  95%|█████████▍| 55/58 [00:01<00:00, 35.45it/s] Capturing num tokens (num_tokens=4 avail_mem=71.63 GB):  95%|█████████▍| 55/58 [00:01<00:00, 35.45it/s]

    Capturing num tokens (num_tokens=4 avail_mem=71.63 GB): 100%|██████████| 58/58 [00:01<00:00, 38.53it/s]


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
    Generated text:  Yana Tjian. I'm a sophomore in high school, I have this question for you: "What is the problem that the student is trying to solve?"
    As an AI language model, I do not have access to the specific question you are referring to. However, I can provide some general information on common problems students may face.
    
    Students may be struggling with various aspects of school, such as academic performance, organization, time management, or mental health. These challenges can lead to anxiety, stress, and difficulty achieving academic goals. 
    
    In some cases, students may also face social issues such as isolation or social anxiety, which can impact
    ===============================
    Prompt: The president of the United States is
    Generated text:  an elected official that represents the country. The position is often unique and has its own set of duties and responsibilities. Some of these duties include handling the country’s foreign policy, establishing guidelines for the federal government, and implementing the laws of the country. In some countries, presidents have the power to grant pardons and commutations. In the United States, the president is the head of government and is responsible for the execution of the laws passed by Congress. They are elected for a five-year term and are the representatives of the United States. The president also serves as the head of the executive branch and is responsible for managing the federal government and
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. It is the most populous city of France, as well as the largest city in Europe. Paris is famous for its architecture, art, and culture, and is a popular tourist destination. It is also a historical center of France, and a symbol of France.
    Who is the capital of France? Paris. Paris is the capital of France.
    Is Paris famous for its architecture?
    Yes, Paris is famous for its architecture.
    Is Paris famous for its food?
    Yes, Paris is famous for its food. It is the most famous French city for French cuisine, and it is also the most famous French restaurant.
    Is Paris famous for its
    ===============================
    Prompt: The future of AI is
    Generated text:  bleak, according to one expert. The technology has been evolving for over 25 years, but it still has a long way to go. The next five years will be the period where the world will see the most impact of the technology. The rapid development of AI will lead to the creation of a new generation of jobs that we won’t see for a long time.
    This is a good time to be careful with AI and its impact. There are a number of potential dangers that we should be aware of. One of the most serious is the risk of creating AI systems that are not ethical or that may be used for malicious purposes.


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


    Generated text:  [Name] and I'm a [job title] at [company name]. I'm excited to meet you and learn more about your career. What can I expect from our conversation? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about your career. What can I expect from our conversation? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about your career. What can I expect from our conversation? [Name] is a [job title] at [company name]. I'm excited to meet you and
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, which is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and Louvre Museum. It is also home to the French Parliament, the French National Museum, and the French Academy of Sciences. Paris is a cultural and historical center with a rich history dating back to the Roman Empire and the French Revolution. It is a major transportation hub, with the Eiffel Tower serving as a symbol of the city's status as a major transportation hub. Paris is also known for its cuisine, with its famous dishes such as croissants, boudin, and escargot. The city is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased integration with human intelligence: AI is likely to become more integrated with human intelligence, allowing machines to learn from and adapt to human behavior. This could lead to more sophisticated and personalized AI systems that can better understand and respond to human needs.
    
    2. Greater emphasis on ethical and social considerations: As AI becomes more integrated with human intelligence, there will be a greater emphasis on ethical and social considerations. This could lead to more transparent and accountable AI systems that are designed to minimize
    


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
    Generated text:  [name], and I'm a [main character's occupation]. I've always loved [main character's occupation] because I've always felt that it's a responsibility to [describe a trait or quality of the occupation that you love]. I have a background in [relevant education or training], and I'm always [describe a specific accomplishment or experience in this field].
    I'm [age], and I love [main character's occupation] because I feel that I can make a [specific impact or difference] in the world. I'm always learning and growing, and I'm always looking for new opportunities to contribute to the greater good. I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    Paris is the largest city in France and the third-largest city in the European Union. It is also the capital of the Ile-de-France region. Paris is known for its medieval architecture, cultural attractions, and fashion industry. The city is also home to the Eiffel Tower and the Louvre Museum, as well as a rich cultural and historical heritage. Paris is a popular tourist destination and a global center of business and finance. The city is also known for its cuisine, including French cuisine, and its nightlife scene. As of 2021, Paris had a population of over 2 million. Paris is
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be shaped by a variety of factors, including advances in computing power, advances in machine learning, and advances in ethical considerations. Some possible future trends in AI include:
    
    1. Enhanced Personalized AI: As AI systems become more capable, they will be able to learn from user behavior and provide personalized responses. This could lead to more efficient, effective, and enjoyable user experiences.
    
    2. More Robust AI: As AI systems become more complex, they will be able to perform more tasks independently and in a more efficient manner. This could lead to more efficient and effective workflows.
    
    3. Enhanced Robustness: AI systems will be


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

    ].

     I

     am

     a

     [

    X

    ]

     who

     is

     known

     for

     my

     [

    Y

    ]

     abilities

    .

     I

     am

     a

     [

    Z

    ]

     who

     has

     a

     lot

     to

     offer

    ,

     and

     I

     am

     always

     looking

     for

     the

     next

     big

     challenge

     to

     take

     on

    .

     I

    'm

     confident

     in

     my

     abilities

     and

     always

     strive

     to

     be

     the

     best

     I

     can

     be

    .

     What

    's

     your

     name

    ,

     and

     what

     can

     you

     do

     for

     me

     today

    ?

     I

     am

     excited

     to

     meet

     you

    !

     #

    self

    int

    roduction

     #

    career

     #

    g

    rowth

     #

    team

    work

     #

    leaders

    hip

     #

    goals

     #

    team

    work

     #

    leaders

    hip

     #

    career

     #

    growth

     #

    goals

     #

    team

    work

     #

    leaders

    hip

     #

    goals

     #

    team

    work

     #

    leaders

    hip

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    Paris

     is

     the

     largest

     city

     in

     France

     by

     population

     and

     the

     second

     largest

     city

     in

     the

     world

    ,

     with

     a

     population

     of

     around

     

    1

    .

    2

    4

     million

     people

    .

     It

     is

     the

     seat

     of

     government

    ,

     the

     cultural

    ,

     and

     commercial

     and

     political

     centre

     of

     France

     and

     the

     second

     largest

     city

     in

     France

     by

     economy

    .

     It

     has

     been

     an

     important

     political

     and

     cultural

     center

     since

     the

     

    1

    2

    th

     century

    .

     Paris

     is

     renowned

     for

     its

     art

    ,

     architecture

    ,

     fashion

    ,

     cinema

    ,

     and

     opera

    .

     It

     is

     also

     known

     as

     the

     "

    city

     of

     a

     thousand

     gates

    "

     due

     to

     its

     many

     can

    als

     and

     bridges

     that

     connect

     the

     city

    .

     Paris

     is

     home

     to

     many

     museums

    ,

     art

     galleries

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     incredibly

     dynamic

    ,

     and

     as

     the

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

     many

     exciting

     trends

     in

     the

     field

    .

     Here

     are

     some

     of

     the

     most

     likely

     possibilities

    :
    


    1

    .

     Increased

     Personal

    ization

    :

     As

     AI

     becomes

     more

     integrated

     into

     our

     daily

     lives

    ,

     we

     can

     expect

     to

     see

     more

     personalized

     experiences

    .

     This

     could

     be

     in

     the

     form

     of

     chat

    bots

     that

     can

     understand

     and

     respond

     to

     user

     requests

    ,

     or

     even

     AI

    -powered

     virtual

     assistants

     that

     can

     provide

     personalized

     recommendations

     based

     on

     our

     interests

     and

     preferences

    .
    


    2

    .

     Autonomous

     vehicles

    :

     In

     the

     near

     future

    ,

     we

     can

     expect

     to

     see

     more

     autonomous

     vehicles

     on

     the

     road

    ,

     thanks

     to

     advances

     in

     sensor

     technology

     and

     machine

     learning

    .

     These

     vehicles

     could

    



```python
llm.shutdown()
```
