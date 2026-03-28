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


    2026-03-28 07:44:05.284 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 07:44:05] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 07:44:05.284 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 07:44:05] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 07:44:05.284 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 07:44:05] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 07:44:05.284 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 07:44:05] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 07:44:05.284 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 07:44:05] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.46it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.45it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:07,  2.24s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:07,  2.24s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:07,  2.24s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:07,  2.24s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.68it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.68it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.68it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.68it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.68it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.68it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.68it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.68it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.68it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:07,  6.68it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 14.82it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 14.82it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 14.82it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 14.82it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 14.82it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 14.82it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 14.82it/s]

    Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 14.82it/s]Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:02<00:02, 14.82it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:02<00:01, 22.50it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:02<00:01, 22.50it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:02<00:01, 22.50it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:02<00:01, 22.50it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:02<00:01, 22.50it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:02<00:01, 22.50it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:02<00:01, 22.50it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:02<00:01, 22.50it/s]Compiling num tokens (num_tokens=320):  47%|████▋     | 27/58 [00:02<00:01, 22.50it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:02<00:00, 30.69it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:02<00:00, 30.69it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:02<00:00, 30.69it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:02<00:00, 30.69it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:02<00:00, 30.69it/s]

    Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:02<00:00, 30.69it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:02<00:00, 30.69it/s]Compiling num tokens (num_tokens=176):  60%|██████    | 35/58 [00:02<00:00, 30.69it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:02<00:00, 34.85it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:02<00:00, 34.85it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:02<00:00, 34.85it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:02<00:00, 34.85it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:02<00:00, 34.85it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:03<00:00, 34.85it/s] Compiling num tokens (num_tokens=80):  72%|███████▏  | 42/58 [00:03<00:00, 34.85it/s]Compiling num tokens (num_tokens=64):  72%|███████▏  | 42/58 [00:03<00:00, 34.85it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 40.98it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 40.98it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 40.98it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 40.98it/s]

    Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 40.98it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 40.98it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 40.98it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 40.98it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:03<00:00, 40.98it/s] Compiling num tokens (num_tokens=4):  84%|████████▍ | 49/58 [00:03<00:00, 40.98it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 18.51it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.80 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.77 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.77 GB):   3%|▎         | 2/58 [00:00<00:03, 18.20it/s]Capturing num tokens (num_tokens=7168 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:03, 18.20it/s]Capturing num tokens (num_tokens=6656 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:03, 18.20it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   3%|▎         | 2/58 [00:00<00:03, 18.20it/s]Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.02it/s]Capturing num tokens (num_tokens=5632 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.02it/s]Capturing num tokens (num_tokens=5120 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.02it/s]Capturing num tokens (num_tokens=4608 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.02it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:02, 21.02it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.76 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.51it/s]Capturing num tokens (num_tokens=3840 avail_mem=76.75 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.51it/s]Capturing num tokens (num_tokens=3584 avail_mem=76.75 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.51it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=76.74 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.51it/s]Capturing num tokens (num_tokens=3072 avail_mem=76.74 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.51it/s]Capturing num tokens (num_tokens=2816 avail_mem=76.74 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.51it/s]Capturing num tokens (num_tokens=2816 avail_mem=76.74 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.84it/s]Capturing num tokens (num_tokens=2560 avail_mem=76.73 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.84it/s]Capturing num tokens (num_tokens=2304 avail_mem=76.73 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.84it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.73 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.84it/s]Capturing num tokens (num_tokens=1792 avail_mem=76.72 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.84it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=76.72 GB):  31%|███       | 18/58 [00:00<00:01, 30.03it/s]Capturing num tokens (num_tokens=1536 avail_mem=76.72 GB):  31%|███       | 18/58 [00:00<00:01, 30.03it/s]Capturing num tokens (num_tokens=1280 avail_mem=76.72 GB):  31%|███       | 18/58 [00:00<00:01, 30.03it/s]Capturing num tokens (num_tokens=1024 avail_mem=76.70 GB):  31%|███       | 18/58 [00:00<00:01, 30.03it/s]Capturing num tokens (num_tokens=960 avail_mem=76.71 GB):  31%|███       | 18/58 [00:00<00:01, 30.03it/s] Capturing num tokens (num_tokens=896 avail_mem=76.71 GB):  31%|███       | 18/58 [00:00<00:01, 30.03it/s]Capturing num tokens (num_tokens=896 avail_mem=76.71 GB):  40%|███▉      | 23/58 [00:00<00:01, 33.98it/s]Capturing num tokens (num_tokens=832 avail_mem=76.70 GB):  40%|███▉      | 23/58 [00:00<00:01, 33.98it/s]Capturing num tokens (num_tokens=768 avail_mem=76.70 GB):  40%|███▉      | 23/58 [00:00<00:01, 33.98it/s]Capturing num tokens (num_tokens=704 avail_mem=76.70 GB):  40%|███▉      | 23/58 [00:00<00:01, 33.98it/s]Capturing num tokens (num_tokens=640 avail_mem=76.69 GB):  40%|███▉      | 23/58 [00:00<00:01, 33.98it/s]Capturing num tokens (num_tokens=576 avail_mem=76.69 GB):  40%|███▉      | 23/58 [00:00<00:01, 33.98it/s]

    Capturing num tokens (num_tokens=512 avail_mem=76.68 GB):  40%|███▉      | 23/58 [00:00<00:01, 33.98it/s]Capturing num tokens (num_tokens=512 avail_mem=76.68 GB):  50%|█████     | 29/58 [00:00<00:00, 39.78it/s]Capturing num tokens (num_tokens=480 avail_mem=76.70 GB):  50%|█████     | 29/58 [00:00<00:00, 39.78it/s]Capturing num tokens (num_tokens=448 avail_mem=76.70 GB):  50%|█████     | 29/58 [00:00<00:00, 39.78it/s]Capturing num tokens (num_tokens=416 avail_mem=76.69 GB):  50%|█████     | 29/58 [00:00<00:00, 39.78it/s]Capturing num tokens (num_tokens=384 avail_mem=76.69 GB):  50%|█████     | 29/58 [00:00<00:00, 39.78it/s]Capturing num tokens (num_tokens=352 avail_mem=76.69 GB):  50%|█████     | 29/58 [00:00<00:00, 39.78it/s]Capturing num tokens (num_tokens=320 avail_mem=76.68 GB):  50%|█████     | 29/58 [00:00<00:00, 39.78it/s]Capturing num tokens (num_tokens=320 avail_mem=76.68 GB):  60%|██████    | 35/58 [00:00<00:00, 43.91it/s]Capturing num tokens (num_tokens=288 avail_mem=76.68 GB):  60%|██████    | 35/58 [00:00<00:00, 43.91it/s]Capturing num tokens (num_tokens=256 avail_mem=76.68 GB):  60%|██████    | 35/58 [00:00<00:00, 43.91it/s]Capturing num tokens (num_tokens=240 avail_mem=76.68 GB):  60%|██████    | 35/58 [00:01<00:00, 43.91it/s]Capturing num tokens (num_tokens=224 avail_mem=76.67 GB):  60%|██████    | 35/58 [00:01<00:00, 43.91it/s]

    Capturing num tokens (num_tokens=208 avail_mem=76.67 GB):  60%|██████    | 35/58 [00:01<00:00, 43.91it/s]Capturing num tokens (num_tokens=192 avail_mem=76.67 GB):  60%|██████    | 35/58 [00:01<00:00, 43.91it/s]Capturing num tokens (num_tokens=192 avail_mem=76.67 GB):  71%|███████   | 41/58 [00:01<00:00, 46.80it/s]Capturing num tokens (num_tokens=176 avail_mem=76.66 GB):  71%|███████   | 41/58 [00:01<00:00, 46.80it/s]Capturing num tokens (num_tokens=160 avail_mem=76.66 GB):  71%|███████   | 41/58 [00:01<00:00, 46.80it/s]Capturing num tokens (num_tokens=144 avail_mem=76.66 GB):  71%|███████   | 41/58 [00:01<00:00, 46.80it/s]Capturing num tokens (num_tokens=128 avail_mem=76.65 GB):  71%|███████   | 41/58 [00:01<00:00, 46.80it/s]Capturing num tokens (num_tokens=112 avail_mem=76.65 GB):  71%|███████   | 41/58 [00:01<00:00, 46.80it/s]Capturing num tokens (num_tokens=112 avail_mem=76.65 GB):  79%|███████▉  | 46/58 [00:01<00:00, 44.87it/s]Capturing num tokens (num_tokens=96 avail_mem=76.65 GB):  79%|███████▉  | 46/58 [00:01<00:00, 44.87it/s] Capturing num tokens (num_tokens=80 avail_mem=76.64 GB):  79%|███████▉  | 46/58 [00:01<00:00, 44.87it/s]

    Capturing num tokens (num_tokens=64 avail_mem=76.64 GB):  79%|███████▉  | 46/58 [00:01<00:00, 44.87it/s]Capturing num tokens (num_tokens=48 avail_mem=76.64 GB):  79%|███████▉  | 46/58 [00:01<00:00, 44.87it/s]Capturing num tokens (num_tokens=32 avail_mem=76.64 GB):  79%|███████▉  | 46/58 [00:01<00:00, 44.87it/s]

    Capturing num tokens (num_tokens=32 avail_mem=76.64 GB):  88%|████████▊ | 51/58 [00:01<00:00, 28.32it/s]Capturing num tokens (num_tokens=28 avail_mem=76.63 GB):  88%|████████▊ | 51/58 [00:01<00:00, 28.32it/s]Capturing num tokens (num_tokens=24 avail_mem=76.63 GB):  88%|████████▊ | 51/58 [00:01<00:00, 28.32it/s]Capturing num tokens (num_tokens=20 avail_mem=76.62 GB):  88%|████████▊ | 51/58 [00:01<00:00, 28.32it/s]Capturing num tokens (num_tokens=16 avail_mem=76.62 GB):  88%|████████▊ | 51/58 [00:01<00:00, 28.32it/s]Capturing num tokens (num_tokens=16 avail_mem=76.62 GB):  95%|█████████▍| 55/58 [00:01<00:00, 27.19it/s]Capturing num tokens (num_tokens=12 avail_mem=76.62 GB):  95%|█████████▍| 55/58 [00:01<00:00, 27.19it/s]Capturing num tokens (num_tokens=8 avail_mem=76.61 GB):  95%|█████████▍| 55/58 [00:01<00:00, 27.19it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=76.61 GB):  95%|█████████▍| 55/58 [00:01<00:00, 27.19it/s]Capturing num tokens (num_tokens=4 avail_mem=76.61 GB): 100%|██████████| 58/58 [00:01<00:00, 32.28it/s]


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
    Generated text:  Lisa. I'm a girl. I'm 12 years old. I like eating fruit and vegetables. My favorite fruit is apples. I like eating apples when I'm feeling good. I also like drinking milk. I like to drink milk when I have a cold. My favorite drink is orange juice. I like to drink orange juice when I have a stomachache. My favorite color is red. I like to wear red shirts and red pants. My favorite subject is history. I think it's interesting and useful. I like to play sports. I like to play soccer, football, tennis and basketball. I like to play computer
    ===============================
    Prompt: The president of the United States is
    Generated text:  a very important person. He is in charge of the country. He is the leader of the people of the country. He is the boss of the country. He is the most important person. The president of the United States is the president of the United States. He is the leader of the country. He is the leader of the country. He is the leader of the country. He is the leader of the country. He is the leader of the country. He is the leader of the country. He is the leader of the country. He is the leader of the country. He is the leader of the country. He is the
    ===============================
    Prompt: The capital of France is
    Generated text:  _______.
    A. Paris
    B. Brussels
    C. Lille
    D. Geneva
    Answer: A
    
    When dealing with leaks of flammable gases in the air, the correct emergency measure is to use a ____ to extinguish the leak.
    A. Sand
    B. Foam
    C. Water
    D. Dry powder
    Answer: B
    
    Which of the following statements about the chemical properties of ferrous iron is incorrect?
    A. Ferrous iron can be oxidized by oxygen in the air.
    B. Ferrous iron can be reduced by hydrogen gas.
    C. Ferrous iron reacts with copper sulfate solution.
    D.
    ===============================
    Prompt: The future of AI is
    Generated text:  bright, but it will be built on the strength of the current generation.
    That’s the key message conveyed in a recent report by the World Economic Forum on AI, which suggests that the future of AI is bright, but it will be built on the strength of the current generation.
    And it’s true that the world is at a crossroads. Global inequality is a major problem and one of the main reasons for inequality in the world is economic growth. The global economic crisis has also led to a reduction in government spending. This has led to a significant drop in job opportunities in the field of technology, which is driven by the need for new


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you and learn more about you. What
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, also known as the City of Light. It is the largest city in France and the second-largest city in the European Union. Paris is known for its iconic landmarks such as the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Arc de Triomphe. The city is also famous for its cuisine, fashion, and art scene. Paris is a cultural and economic hub of France and a major tourist destination. It is home to many world-renowned museums, theaters, and art galleries. The city is also known for its fashion industry, with many famous designers and boutiques. Paris is a vibrant and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in several key areas, including:
    
    1. Increased integration with human intelligence: AI systems are likely to become more integrated with human intelligence, allowing them to learn from and adapt to human behavior and decision-making processes.
    
    2. Enhanced machine learning capabilities: AI systems are likely to become more capable of learning from large amounts of data and making more accurate predictions and decisions.
    
    3. Improved natural language processing: AI systems are likely to become more capable of understanding and generating human-like language, allowing for more natural and intuitive interactions with humans.
    
    4. Increased use of AI in healthcare: AI is likely to become more integrated
    


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
    Generated text:  [Your Name], and I'm a [Occupation] who has always been fascinated by [Your Passion or Interest]. Throughout my life, I've always been driven to find ways to make the world a better place, even if it means taking risks. I enjoy being in the moment, connecting with others, and using my creativity to solve problems. Whether I'm crafting pottery, painting, or writing stories, I'm always pushing my boundaries and trying new things. What's your story? I love to create and I love to explore. I'm excited to learn more about you and get to know you better. [Your Name] [
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known as the City of Light and a major cultural and economic center. It was founded in the 9th century by the Vikings and became a major city during the Renaissance and the Enlightenment. Paris is home to the Eiffel Tower, the Louvre Museum, Notre Dame Cathedral, and the Louvre Palace. It is also one of the most visited cities in the world. Its population is over 2.5 million, with an estimated 1.5 million tourists visiting annually. Paris is a symbol of the French culture and history, and its architecture and artistry continue to inspire and captivate visitors and residents alike.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  rapidly evolving and is likely to involve a wide range of technological advancements, including the development of more advanced neural networks, the integration of machine learning techniques with other forms of data, and the development of more sophisticated ethical guidelines and regulation.
    
    One of the most significant trends in AI is the increased use of machine learning, which involves the use of algorithms and computational models to learn from data and make predictions or decisions without being explicitly programmed. This is likely to lead to more sophisticated and accurate AI systems that can handle a wider range of tasks and provide more accurate and efficient solutions.
    
    Another trend is the development of artificial intelligence systems that can reason and learn in


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

     __

    ________

     and

     I

    ’m

     a

    /an

     __

    ________

    .

     I

    ’m

     a

    /an

     ______

    __

     at

     the

     moment

    .

     And

     I

    ’m

     really

     good

     at

     ______

    __.

     Share

     a

     joke

     with

     us

     and

     we

    ’ll

     see

     how

     we

     can

     make

     you

     laugh

    .
    


    Hello

    ,

     my

     name

     is

     __

    ________

     and

     I

    ’m

     a

    /an

     __

    ________

    .

     I

    ’m

     a

    /an

     ______

    __

     at

     the

     moment

    .

     And

     I

    ’m

     really

     good

     at

     ______

    __.

     Share

     a

     joke

     with

     us

     and

     we

    ’ll

     see

     how

     we

     can

     make

     you laugh

    .

     [

    Optional

     humor

    ]

     Tell

     us

     a

     story

     or

     a

     joke

     about

     your

     childhood

    ,

     and

     let

    ’s

     see

     what

     we

     can

     learn

     together

    .

     [

    Optional

     humor

    ]

     Tell

     us

     a

     story

     or

     a

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     a

     historic

     city

     known

     for

     its

     French

     architecture

    ,

     beautiful

     parks

    ,

     and

     rich

     culture

    .
    


    Great

    ,

     can

     you

     please

     provide

     some

     interesting

     facts

     about

     Paris

    ?

     Sure

    ,

     here

     are

     some

     interesting

     facts

     about

     Paris

    :
    


    1

    .

     Paris

     is

     the

     cultural

     and

     political

     capital

     of

     France

    .


    2

    .

     The

     Lou

    vre

     Museum

    ,

     a

     world

    -ren

    owned

     art

     museum

    ,

     is

     located

     in

     the

     heart

     of

     Paris

    .


    3

    .

     The

     E

    iff

    el

     Tower

     is

     the

     highest

     structure

     in

     the

     world

    ,

     standing

     

    3

    2

    4

     meters

     tall

    .


    4

    .

     Paris

     is

     famous

     for

     its

     famous

     

    1

    9

    th

    -century

     fashion

    ,

     including

     the

     iconic

     E

    iff

    el

     Tower

    ,

     Chanel

    ,

     and

     D

    ior

    .


    5

    .

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     und

    eni

    ably

     exciting

    ,

     as

     it

     has

     the

     potential

     to

     revolution

    ize

     almost

     every

     aspect

     of

     our

     lives

    .

     Here

     are

     some

     possible

     future

     trends

     in

     AI

    :
    


    1

    .

     Increased

     Integration

     with

     Industry

    :

     As

     AI

     becomes

     more

     integrated

     with

     industry

    ,

     we

     can

     expect

     to

     see

     more

     AI

    -powered

     solutions

     being

     developed

     that

     can

     improve

     efficiency

    ,

     reduce

     costs

    ,

     and

     enhance

     productivity

    .

     This

     trend

     is

     already

     evident

     in

     the

     manufacturing

    ,

     healthcare

    ,

     and

     transportation

     industries

    .
    


    2

    .

     Enhanced

     Personal

    ization

    :

     AI

     will

     be

     able

     to

     personalize

     our

     interactions

     with

     AI

     assistants

    ,

     such

     as

     virtual

     assistants

    ,

     chat

    bots

    ,

     and

     personal

     assistants

    .

     This

     will

     allow

     us

     to

     have

     more

     personalized

     experiences

    ,

     from

     recommending

     movies

     or

     TV

     shows

    



```python
llm.shutdown()
```
