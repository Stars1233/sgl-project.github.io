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


    2026-03-27 22:43:53.843 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:43:53] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 22:43:53.843 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:43:53] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 22:43:53.843 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:43:53] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 22:43:53.843 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:43:53] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 22:43:53.843 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:43:53] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.05it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.05it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:25,  2.10it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.40it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.40it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.40it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.40it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.40it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.40it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.40it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.40it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.40it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:07,  6.40it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 14.28it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 14.28it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 14.28it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 14.28it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 14.28it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 14.28it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 14.28it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 14.28it/s]

    Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 20.93it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 20.93it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 20.93it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 20.93it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 20.93it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 20.93it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 20.93it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:02<00:01, 20.93it/s]Compiling num tokens (num_tokens=352):  45%|████▍     | 26/58 [00:02<00:01, 20.93it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:02<00:00, 29.09it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:02<00:00, 29.09it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:02<00:00, 29.09it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:02<00:00, 29.09it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:02<00:00, 29.09it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:02<00:00, 29.09it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:02<00:00, 29.09it/s]

    Compiling num tokens (num_tokens=192):  59%|█████▊    | 34/58 [00:02<00:00, 29.09it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:03<00:00, 35.76it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:03<00:00, 35.76it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:03<00:00, 35.76it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:03<00:00, 35.76it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:03<00:00, 35.76it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:03<00:00, 35.76it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:03<00:00, 35.76it/s] Compiling num tokens (num_tokens=80):  71%|███████   | 41/58 [00:03<00:00, 35.76it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:03<00:00, 42.09it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:03<00:00, 42.09it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:03<00:00, 42.09it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:03<00:00, 42.09it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:03<00:00, 42.09it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:03<00:00, 42.09it/s]Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:03<00:00, 42.09it/s]Compiling num tokens (num_tokens=16):  83%|████████▎ | 48/58 [00:03<00:00, 42.09it/s]Compiling num tokens (num_tokens=12):  83%|████████▎ | 48/58 [00:03<00:00, 42.09it/s]

    Compiling num tokens (num_tokens=8):  83%|████████▎ | 48/58 [00:03<00:00, 42.09it/s] Compiling num tokens (num_tokens=4):  83%|████████▎ | 48/58 [00:03<00:00, 42.09it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 54.32it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 18.04it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.80 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=76.80 GB):   2%|▏         | 1/58 [00:00<00:08,  6.57it/s]Capturing num tokens (num_tokens=7680 avail_mem=76.77 GB):   2%|▏         | 1/58 [00:00<00:08,  6.57it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=76.76 GB):   2%|▏         | 1/58 [00:00<00:08,  6.57it/s]Capturing num tokens (num_tokens=7168 avail_mem=76.76 GB):   5%|▌         | 3/58 [00:00<00:05, 10.44it/s]Capturing num tokens (num_tokens=6656 avail_mem=76.76 GB):   5%|▌         | 3/58 [00:00<00:05, 10.44it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   5%|▌         | 3/58 [00:00<00:05, 10.44it/s]Capturing num tokens (num_tokens=6144 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:06,  8.33it/s]Capturing num tokens (num_tokens=5632 avail_mem=76.76 GB):   9%|▊         | 5/58 [00:00<00:06,  8.33it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=76.76 GB):  10%|█         | 6/58 [00:00<00:06,  7.94it/s]Capturing num tokens (num_tokens=5120 avail_mem=76.76 GB):  10%|█         | 6/58 [00:00<00:06,  7.94it/s]Capturing num tokens (num_tokens=4608 avail_mem=76.76 GB):  10%|█         | 6/58 [00:00<00:06,  7.94it/s]Capturing num tokens (num_tokens=4096 avail_mem=76.76 GB):  10%|█         | 6/58 [00:00<00:06,  7.94it/s]Capturing num tokens (num_tokens=3840 avail_mem=76.75 GB):  10%|█         | 6/58 [00:00<00:06,  7.94it/s]Capturing num tokens (num_tokens=3840 avail_mem=76.75 GB):  17%|█▋        | 10/58 [00:00<00:03, 14.95it/s]Capturing num tokens (num_tokens=3584 avail_mem=76.75 GB):  17%|█▋        | 10/58 [00:00<00:03, 14.95it/s]Capturing num tokens (num_tokens=3328 avail_mem=76.74 GB):  17%|█▋        | 10/58 [00:00<00:03, 14.95it/s]Capturing num tokens (num_tokens=3072 avail_mem=76.74 GB):  17%|█▋        | 10/58 [00:00<00:03, 14.95it/s]Capturing num tokens (num_tokens=2816 avail_mem=76.74 GB):  17%|█▋        | 10/58 [00:00<00:03, 14.95it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=76.73 GB):  17%|█▋        | 10/58 [00:00<00:03, 14.95it/s]Capturing num tokens (num_tokens=2560 avail_mem=76.73 GB):  26%|██▌       | 15/58 [00:00<00:01, 23.04it/s]Capturing num tokens (num_tokens=2304 avail_mem=76.73 GB):  26%|██▌       | 15/58 [00:00<00:01, 23.04it/s]Capturing num tokens (num_tokens=2048 avail_mem=76.73 GB):  26%|██▌       | 15/58 [00:00<00:01, 23.04it/s]Capturing num tokens (num_tokens=1792 avail_mem=76.72 GB):  26%|██▌       | 15/58 [00:00<00:01, 23.04it/s]Capturing num tokens (num_tokens=1536 avail_mem=76.72 GB):  26%|██▌       | 15/58 [00:01<00:01, 23.04it/s]Capturing num tokens (num_tokens=1280 avail_mem=76.72 GB):  26%|██▌       | 15/58 [00:01<00:01, 23.04it/s]Capturing num tokens (num_tokens=1024 avail_mem=76.70 GB):  26%|██▌       | 15/58 [00:01<00:01, 23.04it/s]Capturing num tokens (num_tokens=1024 avail_mem=76.70 GB):  36%|███▌      | 21/58 [00:01<00:01, 31.48it/s]Capturing num tokens (num_tokens=960 avail_mem=76.71 GB):  36%|███▌      | 21/58 [00:01<00:01, 31.48it/s] Capturing num tokens (num_tokens=896 avail_mem=76.71 GB):  36%|███▌      | 21/58 [00:01<00:01, 31.48it/s]Capturing num tokens (num_tokens=832 avail_mem=76.70 GB):  36%|███▌      | 21/58 [00:01<00:01, 31.48it/s]Capturing num tokens (num_tokens=768 avail_mem=76.70 GB):  36%|███▌      | 21/58 [00:01<00:01, 31.48it/s]

    Capturing num tokens (num_tokens=704 avail_mem=76.70 GB):  36%|███▌      | 21/58 [00:01<00:01, 31.48it/s]Capturing num tokens (num_tokens=640 avail_mem=76.69 GB):  36%|███▌      | 21/58 [00:01<00:01, 31.48it/s]Capturing num tokens (num_tokens=640 avail_mem=76.69 GB):  47%|████▋     | 27/58 [00:01<00:00, 38.03it/s]Capturing num tokens (num_tokens=576 avail_mem=76.69 GB):  47%|████▋     | 27/58 [00:01<00:00, 38.03it/s]Capturing num tokens (num_tokens=512 avail_mem=76.68 GB):  47%|████▋     | 27/58 [00:01<00:00, 38.03it/s]Capturing num tokens (num_tokens=480 avail_mem=76.70 GB):  47%|████▋     | 27/58 [00:01<00:00, 38.03it/s]Capturing num tokens (num_tokens=448 avail_mem=76.70 GB):  47%|████▋     | 27/58 [00:01<00:00, 38.03it/s]Capturing num tokens (num_tokens=416 avail_mem=76.69 GB):  47%|████▋     | 27/58 [00:01<00:00, 38.03it/s]Capturing num tokens (num_tokens=416 avail_mem=76.69 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.37it/s]Capturing num tokens (num_tokens=384 avail_mem=76.69 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.37it/s]Capturing num tokens (num_tokens=352 avail_mem=76.69 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.37it/s]

    Capturing num tokens (num_tokens=320 avail_mem=76.68 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.37it/s]Capturing num tokens (num_tokens=288 avail_mem=76.68 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.37it/s]Capturing num tokens (num_tokens=256 avail_mem=76.68 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.37it/s]Capturing num tokens (num_tokens=240 avail_mem=76.68 GB):  55%|█████▌    | 32/58 [00:01<00:00, 37.37it/s]Capturing num tokens (num_tokens=240 avail_mem=76.68 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.60it/s]Capturing num tokens (num_tokens=224 avail_mem=76.67 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.60it/s]Capturing num tokens (num_tokens=208 avail_mem=76.67 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.60it/s]Capturing num tokens (num_tokens=192 avail_mem=76.67 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.60it/s]Capturing num tokens (num_tokens=176 avail_mem=76.66 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.60it/s]Capturing num tokens (num_tokens=160 avail_mem=76.66 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.60it/s]Capturing num tokens (num_tokens=144 avail_mem=76.66 GB):  66%|██████▌   | 38/58 [00:01<00:00, 41.60it/s]

    Capturing num tokens (num_tokens=144 avail_mem=76.66 GB):  76%|███████▌  | 44/58 [00:01<00:00, 41.35it/s]Capturing num tokens (num_tokens=128 avail_mem=76.65 GB):  76%|███████▌  | 44/58 [00:01<00:00, 41.35it/s]Capturing num tokens (num_tokens=112 avail_mem=76.65 GB):  76%|███████▌  | 44/58 [00:01<00:00, 41.35it/s]Capturing num tokens (num_tokens=96 avail_mem=76.65 GB):  76%|███████▌  | 44/58 [00:01<00:00, 41.35it/s] Capturing num tokens (num_tokens=80 avail_mem=76.64 GB):  76%|███████▌  | 44/58 [00:01<00:00, 41.35it/s]Capturing num tokens (num_tokens=64 avail_mem=76.64 GB):  76%|███████▌  | 44/58 [00:01<00:00, 41.35it/s]Capturing num tokens (num_tokens=64 avail_mem=76.64 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.12it/s]Capturing num tokens (num_tokens=48 avail_mem=76.64 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.12it/s]Capturing num tokens (num_tokens=32 avail_mem=76.64 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.12it/s]Capturing num tokens (num_tokens=28 avail_mem=76.63 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.12it/s]

    Capturing num tokens (num_tokens=24 avail_mem=76.63 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.12it/s]Capturing num tokens (num_tokens=20 avail_mem=76.62 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.12it/s]Capturing num tokens (num_tokens=16 avail_mem=76.62 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.12it/s]Capturing num tokens (num_tokens=16 avail_mem=76.62 GB):  95%|█████████▍| 55/58 [00:01<00:00, 42.93it/s]Capturing num tokens (num_tokens=12 avail_mem=76.62 GB):  95%|█████████▍| 55/58 [00:01<00:00, 42.93it/s]Capturing num tokens (num_tokens=8 avail_mem=76.61 GB):  95%|█████████▍| 55/58 [00:01<00:00, 42.93it/s] Capturing num tokens (num_tokens=4 avail_mem=76.61 GB):  95%|█████████▍| 55/58 [00:01<00:00, 42.93it/s]Capturing num tokens (num_tokens=4 avail_mem=76.61 GB): 100%|██████████| 58/58 [00:01<00:00, 30.75it/s]


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
    Generated text:  Suzy. I'm a teacher in New York. My classroom is very big and it has many rooms. There are two language classrooms. The first one is for English and the second one is for Japanese. There are many computers and we use the computers for our lessons. We can watch English and Japanese TV shows and read English and Japanese books. We also use computers to play games and write letters. There are also three other rooms. We can use them when we have a lesson, too. Our class has a swimming pool, too. There are lots of fish in it. They are very friendly to us. I'm a teacher
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to persuade a new senator to sign a bill into law. He convinces the senator to consider a 3 year moratorium on the sale of federal bonds. The senator knows nothing about economics, but she believes that the sale of government bonds increases national debt. She also knows that she will lose her job if she refuses to sign the bill into law.
    
    Based on the information above, what should the president do? Based on the information provided, the president should consider the following steps:
    
    1. Inform the senator about the 3 year moratorium on the sale of federal bonds. This will likely satisfy the senator and provide a reason for
    ===============================
    Prompt: The capital of France is
    Generated text:  the capital of the country, not vice versa.
    The answer is no. The answer is no because the capital of France is not the capital of the country, but the other way around. France has Paris as its capital, which is in Paris. However, the capital of the country France is not Paris. The capital of France is the capital of France. Paris, therefore, is not the capital of France.
    
    Answer this question: is the capital of an island a capital?
    Options are:
     1). no
     2). yes
    
     1). no
    You are an AI assistant that helps people find information. You don’t
    ===============================
    Prompt: The future of AI is
    Generated text:  now, and it’s more important than ever to stay up-to-date with the latest in the field. That’s why we’ve listed some of the top AI companies that are creating cutting-edge research and development, as well as how you can get involved and stay ahead of the curve.
    1. Google
    Google is one of the biggest players in the AI market, and their latest projects include Super Human, a voice assistant that can answer all of your questions, Super Speech, a virtual assistant that can follow directions, and DeepMind, a team that researches and develops AI.
    2. Alibaba Cloud
    Alibaba Cloud is another leading player


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


    Generated text:  [Name], and I'm a [Job Title] at [Company Name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [Age], [Gender], [Nationality], [Occupation]. I have [Number] years of experience in [Field of Work]. I'm a [Type of Person], [Personality Traits], [Adjective]. I'm [Appearance], [Hobbies], and [Favorite Food]. I'm [Personality], [Adjective], and [Adjective]. I'm [Appearance], [Hobbies], and [Favorite Food]. I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a bustling metropolis with a rich cultural heritage and is a major economic and political center in Europe. It is also known for its fashion industry, with Paris Fashion Week being one of the largest in the world. The city is home to many famous landmarks and attractions, including the Louvre, the Champs-Élysées, and the Eiffel Tower. Paris is a popular
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we interact with technology and the world around us. Here are some of the most likely trends that are expected to shape the future of AI:
    
    1. Increased automation and robotics: As AI continues to advance, we are likely to see an increase in automation and robotics in various industries. This will lead to the development of more efficient and cost-effective solutions to many of the world's challenges, such as manufacturing, transportation, and healthcare.
    
    2. Improved privacy and security: As AI becomes more integrated into our daily lives, there will be an increasing need for privacy and
    


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
    Generated text:  [Name]. I’m [Age]. I love [What's your main hobby or interest?]. I work as a [Job Title], and my [Favorite Color] is [Favorite Color]. I enjoy spending time [How often do you spend time with friends and family?]. I'm always looking for new adventures and experiences to add to my life. I'm always looking for new places to travel, new places to work and, most importantly, new ways to make new friends. Can you tell me a little bit more about yourself? It would be great to know how you got started in your career and any personal goals you have.
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. 
    
    Detailed answer: Paris, the capital of France, is a global cultural and economic center, with a rich history dating back to the ancient Romans. Today, it is a bustling metropolis with a population of around 2. 4 million people, making it the third most populous city in the world. The city is known for its diverse neighborhoods, including the Seine River两岸, the Île de la Cité, and the Tuileries Garden. Paris is also a major cultural hub, with many museums, theaters, and museums, including the Louvre, the Musée d'Orsay, and the Centre
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  highly uncertain, but several trends are expected to shape it. One trend is the increasing use of AI in the healthcare industry, where AI is increasingly being used to analyze large amounts of medical data and make more accurate diagnoses. Another trend is the development of AI-driven robotics, which can be used for tasks such as manufacturing and logistics. The use of AI in manufacturing and supply chain management is also expected to increase, as companies seek to improve efficiency and reduce costs. Another trend is the development of AI-powered chatbots and virtual assistants, which can be used to provide customers with faster and more personalized support. Finally, the use of AI in the


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

    Type

     of

     Job

    ],

     [

    Job

     Title

    ]

     with

     [

    Number

     of

     Years

     in

     Position

    ]

     years

     of

     experience

     in

     [

    Industry

    ].

     I

    'm

     currently

     [

    State

     of

     Job

    ]

     and

     my

     primary

     focus

     is

     [

    What

     I

     do

    ],

     [

    What

     I

     do

     at

     work

    ],

     and

     [

    What

     I

    'm

     passionate

     about

    ].

     In

     my

     free

     time

    ,

     I

     enjoy

     [

    What

     I

     do

     on

     my

     free

     time

    ],

     [

    What

     I

     do

     in

     my

     free

     time

    ].

     Thank

     you

     for

     taking

     the

     time

     to

     meet

     me

    !

     I

    'm

     a

     [

    Type

     of

     Job

    ],

     [

    Job

     Title

    ]

     with

     [

    Number

     of

     Years

     in

     Position

    ]

     years

     of

     experience

     in

     [

    Industry

    ].

     I

    'm

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     which

     is

     known

     as

     the

     "

    City

     of

     a

     Hundred

     Gardens

    "

     due

     to

     its

     extensive

     network

     of

     parks

     and

     gardens

    .


    You

     are

     to

     answer

     this

     question

    :

     Which

     country

     is

     located

     in

     Asia

    ?

     North

     Korea

    


    You

     will

     be

     given

     the

     following

     question

    :

     "

    What

     is

     the

     name

     of

     the

     country

     located

     in

     Asia

     that

     has

     a

     population

     of

     

    1

    2

     million

    ?

     "

     Has

     the

     answer

     to

     this

     question

     been

     provided

     above

    ?

     No

    ,

     the

     answer

     to

     the

     question

     "

    What

     is

     the

     name

     of

     the

     country

     located

     in

     Asia

     that

     has

     a

     population

     of

     

    1

    2

     million

    ?

     "

     has

     not

     been

     provided

     in

     the

     given

     question

    .

     The

     question

     is

     about

     a

     country

     located

     in

     Asia

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     exciting

     and

     multif

    ac

    eted

    ,

     and

     it

    's

     likely

     that

     we

    'll

     see

     new

     and

     innovative

     applications

     of

     AI

     in

     many

     different

     areas

    .

     Here

     are

     some

     potential

     future

     trends

    :
    


    1

    .

     Increased

     use

     of

     AI

     in

     healthcare

    :

     AI

     can

     be

     used

     to

     analyze

     large

     amounts

     of

     medical

     data

     to

     identify

     potential

     health

     risks

     and

     areas

     for

     improvement

    .

     This

     could

     lead

     to

     new

     treatments

     and

     better

     medical

     outcomes

    .
    


    2

    .

     AI

     in

     finance

    :

     AI

     can

     be

     used

     to

     analyze

     financial

     data

     and

     identify

     patterns

     that

     can

     help

     financial

     institutions

     make

     more

     accurate

     predictions

     and

     investment

     decisions

    .

     This

     could

     lead

     to

     better

     risk

     management

     and

     increased

     efficiency

     in

     the

     financial

     industry

    .
    


    3

    .

     AI

     in

     automation

    :

     AI

     is

     already

     being

    



```python
llm.shutdown()
```
