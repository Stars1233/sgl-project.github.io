# SGLang Native APIs

Apart from the OpenAI compatible APIs, the SGLang Runtime also provides its native server APIs. We introduce the following APIs:

- `/generate` (text generation model)
- `/get_model_info`
- `/server_info`
- `/health`
- `/health_generate`
- `/flush_cache`
- `/update_weights`
- `/encode`(embedding model)
- `/v1/rerank`(cross encoder rerank model)
- `/v1/score`(decoder-only scoring)
- `/classify`(reward model)
- `/start_expert_distribution_record`
- `/stop_expert_distribution_record`
- `/dump_expert_distribution_record`
- `/tokenize`
- `/detokenize`
- A full list of these APIs can be found at [http_server.py](https://github.com/sgl-project/sglang/blob/main/python/sglang/srt/entrypoints/http_server.py)

We mainly use `requests` to test these APIs in the following examples. You can also use `curl`.


## Launch A Server


```python
from sglang.test.doc_patch import launch_server_cmd
from sglang.utils import wait_for_server, print_highlight, terminate_process

server_process, port = launch_server_cmd(
    "python3 -m sglang.launch_server --model-path qwen/qwen2.5-0.5b-instruct --host 0.0.0.0 --log-level warning"
)

wait_for_server(f"http://localhost:{port}", process=server_process)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-04-03 18:29:28.114 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:29:28] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:29:28.114 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:29:28] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:29:28.114 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:29:28] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:29:28.114 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:29:28] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:29:28.114 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:29:28] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.98it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.97it/s]


    2026-04-03 18:29:30,596 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 18:29:30] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:31,  2.66s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:28,  1.88it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.77it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.77it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.77it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.77it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.77it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.77it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.77it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.77it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.77it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 12.20it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 12.20it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:03<00:03, 12.20it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:03<00:03, 12.20it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 12.20it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 12.20it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 12.20it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 12.20it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.26it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.26it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.26it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.26it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.26it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.26it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.26it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.26it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 24.81it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 24.81it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 24.81it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 24.81it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 24.81it/s]

    Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 24.81it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 24.81it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 29.51it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 29.51it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 29.51it/s]

    Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 29.51it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 29.51it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 29.51it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 29.51it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 27.98it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 27.98it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 27.98it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 27.98it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 27.98it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 27.98it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 31.10it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 31.10it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 31.10it/s]

    Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 31.10it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 31.10it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 31.10it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 31.10it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 31.10it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:03<00:00, 38.36it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:03<00:00, 38.36it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:03<00:00, 38.36it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.25it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=132.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=132.39 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=132.39 GB):   3%|▎         | 2/58 [00:00<00:03, 18.46it/s]Capturing num tokens (num_tokens=7168 avail_mem=132.38 GB):   3%|▎         | 2/58 [00:00<00:03, 18.46it/s]Capturing num tokens (num_tokens=6656 avail_mem=132.38 GB):   3%|▎         | 2/58 [00:00<00:03, 18.46it/s]Capturing num tokens (num_tokens=6144 avail_mem=132.38 GB):   3%|▎         | 2/58 [00:00<00:03, 18.46it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=132.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.47it/s]Capturing num tokens (num_tokens=5632 avail_mem=132.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.47it/s]Capturing num tokens (num_tokens=5120 avail_mem=132.38 GB):   9%|▊         | 5/58 [00:00<00:02, 21.47it/s]Capturing num tokens (num_tokens=4608 avail_mem=132.37 GB):   9%|▊         | 5/58 [00:00<00:02, 21.47it/s]Capturing num tokens (num_tokens=4608 avail_mem=132.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.91it/s]Capturing num tokens (num_tokens=4096 avail_mem=132.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.91it/s]Capturing num tokens (num_tokens=3840 avail_mem=132.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.91it/s]Capturing num tokens (num_tokens=3584 avail_mem=132.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.91it/s]Capturing num tokens (num_tokens=3328 avail_mem=132.36 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.91it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=132.36 GB):  21%|██        | 12/58 [00:00<00:01, 29.58it/s]Capturing num tokens (num_tokens=3072 avail_mem=132.36 GB):  21%|██        | 12/58 [00:00<00:01, 29.58it/s]Capturing num tokens (num_tokens=2816 avail_mem=132.36 GB):  21%|██        | 12/58 [00:00<00:01, 29.58it/s]Capturing num tokens (num_tokens=2560 avail_mem=132.35 GB):  21%|██        | 12/58 [00:00<00:01, 29.58it/s]Capturing num tokens (num_tokens=2304 avail_mem=132.35 GB):  21%|██        | 12/58 [00:00<00:01, 29.58it/s]Capturing num tokens (num_tokens=2048 avail_mem=132.35 GB):  21%|██        | 12/58 [00:00<00:01, 29.58it/s]Capturing num tokens (num_tokens=2048 avail_mem=132.35 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.41it/s]Capturing num tokens (num_tokens=1792 avail_mem=132.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.41it/s]Capturing num tokens (num_tokens=1536 avail_mem=132.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.41it/s]Capturing num tokens (num_tokens=1280 avail_mem=132.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.41it/s]Capturing num tokens (num_tokens=1024 avail_mem=132.32 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.41it/s]

    Capturing num tokens (num_tokens=960 avail_mem=132.33 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.41it/s] Capturing num tokens (num_tokens=960 avail_mem=132.33 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.09it/s]Capturing num tokens (num_tokens=896 avail_mem=132.33 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.09it/s]Capturing num tokens (num_tokens=832 avail_mem=132.32 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.09it/s]Capturing num tokens (num_tokens=768 avail_mem=132.32 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.09it/s]Capturing num tokens (num_tokens=704 avail_mem=132.32 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.09it/s]Capturing num tokens (num_tokens=640 avail_mem=132.31 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.09it/s]Capturing num tokens (num_tokens=640 avail_mem=132.31 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.21it/s]Capturing num tokens (num_tokens=576 avail_mem=132.31 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.21it/s]Capturing num tokens (num_tokens=512 avail_mem=132.30 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.21it/s]Capturing num tokens (num_tokens=480 avail_mem=132.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.21it/s]

    Capturing num tokens (num_tokens=448 avail_mem=132.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.21it/s]Capturing num tokens (num_tokens=416 avail_mem=132.31 GB):  47%|████▋     | 27/58 [00:00<00:00, 39.21it/s]Capturing num tokens (num_tokens=416 avail_mem=132.31 GB):  55%|█████▌    | 32/58 [00:00<00:00, 40.11it/s]Capturing num tokens (num_tokens=384 avail_mem=132.31 GB):  55%|█████▌    | 32/58 [00:00<00:00, 40.11it/s]Capturing num tokens (num_tokens=352 avail_mem=132.31 GB):  55%|█████▌    | 32/58 [00:00<00:00, 40.11it/s]Capturing num tokens (num_tokens=320 avail_mem=132.30 GB):  55%|█████▌    | 32/58 [00:00<00:00, 40.11it/s]Capturing num tokens (num_tokens=288 avail_mem=132.30 GB):  55%|█████▌    | 32/58 [00:00<00:00, 40.11it/s]Capturing num tokens (num_tokens=256 avail_mem=132.30 GB):  55%|█████▌    | 32/58 [00:01<00:00, 40.11it/s]Capturing num tokens (num_tokens=256 avail_mem=132.30 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.06it/s]Capturing num tokens (num_tokens=240 avail_mem=132.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.06it/s]Capturing num tokens (num_tokens=224 avail_mem=132.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.06it/s]

    Capturing num tokens (num_tokens=208 avail_mem=132.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.06it/s]Capturing num tokens (num_tokens=192 avail_mem=132.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.06it/s]Capturing num tokens (num_tokens=176 avail_mem=132.28 GB):  64%|██████▍   | 37/58 [00:01<00:00, 41.06it/s]Capturing num tokens (num_tokens=176 avail_mem=132.28 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.84it/s]Capturing num tokens (num_tokens=160 avail_mem=132.28 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.84it/s]Capturing num tokens (num_tokens=144 avail_mem=132.28 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.84it/s]Capturing num tokens (num_tokens=128 avail_mem=132.27 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.84it/s]Capturing num tokens (num_tokens=112 avail_mem=132.27 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.84it/s]Capturing num tokens (num_tokens=96 avail_mem=132.27 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.84it/s] Capturing num tokens (num_tokens=96 avail_mem=132.27 GB):  81%|████████  | 47/58 [00:01<00:00, 42.29it/s]Capturing num tokens (num_tokens=80 avail_mem=132.26 GB):  81%|████████  | 47/58 [00:01<00:00, 42.29it/s]

    Capturing num tokens (num_tokens=64 avail_mem=132.26 GB):  81%|████████  | 47/58 [00:01<00:00, 42.29it/s]Capturing num tokens (num_tokens=48 avail_mem=132.26 GB):  81%|████████  | 47/58 [00:01<00:00, 42.29it/s]Capturing num tokens (num_tokens=32 avail_mem=132.25 GB):  81%|████████  | 47/58 [00:01<00:00, 42.29it/s]Capturing num tokens (num_tokens=28 avail_mem=132.25 GB):  81%|████████  | 47/58 [00:01<00:00, 42.29it/s]Capturing num tokens (num_tokens=28 avail_mem=132.25 GB):  90%|████████▉ | 52/58 [00:01<00:00, 41.96it/s]Capturing num tokens (num_tokens=24 avail_mem=132.25 GB):  90%|████████▉ | 52/58 [00:01<00:00, 41.96it/s]Capturing num tokens (num_tokens=20 avail_mem=132.24 GB):  90%|████████▉ | 52/58 [00:01<00:00, 41.96it/s]Capturing num tokens (num_tokens=16 avail_mem=132.24 GB):  90%|████████▉ | 52/58 [00:01<00:00, 41.96it/s]

    Capturing num tokens (num_tokens=12 avail_mem=132.22 GB):  90%|████████▉ | 52/58 [00:01<00:00, 41.96it/s]Capturing num tokens (num_tokens=8 avail_mem=132.22 GB):  90%|████████▉ | 52/58 [00:01<00:00, 41.96it/s] Capturing num tokens (num_tokens=8 avail_mem=132.22 GB):  98%|█████████▊| 57/58 [00:01<00:00, 32.05it/s]Capturing num tokens (num_tokens=4 avail_mem=132.21 GB):  98%|█████████▊| 57/58 [00:01<00:00, 32.05it/s]Capturing num tokens (num_tokens=4 avail_mem=132.21 GB): 100%|██████████| 58/58 [00:01<00:00, 35.13it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


## Generate (text generation model)
Generate completions. This is similar to the `/v1/completions` in OpenAI API. Detailed parameters can be found in the [sampling parameters](sampling_params.md).


```python
import requests

url = f"http://localhost:{port}/generate"
data = {"text": "What is the capital of France?"}

response = requests.post(url, json=data)
print_highlight(response.json())
```


<strong style='color: #00008B;'>{'text': ' The capital of France is Paris. Paris is the largest city in both area and population and the official capital of France. It is also a flourishing metropolitan centre, as it is a major center for finance and commerce as well as the home of important cultural institutions such as the Louvre.\n\nParis, widely known by its errors, except those which generally show displeasure or prejudice, has a delicate conception of the relation between man and nature as a whole. Its cogito (principle of first principles) and alleicopter (architecture with vertically oriented courtyards and low height of definite plane) ideestions are referred to various national and universal', 'output_ids': [576, 6722, 315, 9625, 374, 12095, 13, 12095, 374, 279, 7772, 3283, 304, 2176, 3082, 323, 7042, 323, 279, 3946, 6722, 315, 9625, 13, 1084, 374, 1083, 264, 98259, 57406, 12261, 11, 438, 432, 374, 264, 3598, 4126, 369, 17017, 323, 35654, 438, 1632, 438, 279, 2114, 315, 2989, 12752, 14336, 1741, 438, 279, 9729, 48506, 382, 59604, 11, 13570, 3881, 553, 1181, 5975, 11, 3650, 1846, 892, 8789, 1473, 84234, 3970, 476, 47686, 11, 702, 264, 35201, 42556, 315, 279, 12687, 1948, 883, 323, 6993, 438, 264, 4361, 13, 11445, 61566, 6357, 320, 649, 15996, 315, 1156, 16170, 8, 323, 12304, 87223, 320, 80419, 448, 50935, 41108, 2081, 1881, 2347, 323, 3347, 2608, 315, 43770, 11031, 8, 2614, 477, 908, 525, 13862, 311, 5257, 5313, 323, 20178], 'meta_info': {'id': '623597d3a67a491bb2fb3434b510f3aa', 'finish_reason': {'type': 'length', 'length': 128}, 'prompt_tokens': 7, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 128, 'cached_tokens': 0, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 1.124718474689871, 'response_sent_to_client_ts': 1775240985.5500526}}</strong>


## Get Model Info

Get the information of the model.

- `model_path`: The path/name of the model.
- `is_generation`: Whether the model is used as generation model or embedding model.
- `tokenizer_path`: The path/name of the tokenizer.
- `preferred_sampling_params`: The default sampling params specified via `--preferred-sampling-params`. `None` is returned in this example as we did not explicitly configure it in server args.
- `weight_version`: This field contains the version of the model weights. This is often used to track changes or updates to the model’s trained parameters.
- `has_image_understanding`: Whether the model has image-understanding capability.
- `has_audio_understanding`: Whether the model has audio-understanding capability.
- `model_type`: The model type from the HuggingFace config (e.g., "qwen2", "llama").
- `architectures`: The model architectures from the HuggingFace config (e.g., ["Qwen2ForCausalLM"]).


```python
url = f"http://localhost:{port}/get_model_info"

response = requests.get(url)
response_json = response.json()
print_highlight(response_json)
assert response_json["model_path"] == "qwen/qwen2.5-0.5b-instruct"
assert response_json["is_generation"] is True
assert response_json["tokenizer_path"] == "qwen/qwen2.5-0.5b-instruct"
assert response_json["preferred_sampling_params"] is None
assert response_json.keys() == {
    "model_path",
    "is_generation",
    "tokenizer_path",
    "preferred_sampling_params",
    "weight_version",
    "has_image_understanding",
    "has_audio_understanding",
    "model_type",
    "architectures",
}
```

    [2026-04-03 18:29:45] Endpoint '/get_model_info' is deprecated and will be removed in a future version. Please use '/model_info' instead.



<strong style='color: #00008B;'>{'model_path': 'qwen/qwen2.5-0.5b-instruct', 'tokenizer_path': 'qwen/qwen2.5-0.5b-instruct', 'is_generation': True, 'preferred_sampling_params': None, 'weight_version': 'default', 'has_image_understanding': False, 'has_audio_understanding': False, 'model_type': 'qwen2', 'architectures': ['Qwen2ForCausalLM']}</strong>


## Get Server Info
Gets the server information including CLI arguments, token limits, and memory pool sizes.
- Note: `get_server_info` merges the following deprecated endpoints:
  - `get_server_args`
  - `get_memory_pool_size`
  - `get_max_total_num_tokens`


```python
url = f"http://localhost:{port}/server_info"

response = requests.get(url)
print_highlight(response.text)
```


<strong style='color: #00008B;'>{"model_path":"qwen/qwen2.5-0.5b-instruct","tokenizer_path":"qwen/qwen2.5-0.5b-instruct","tokenizer_mode":"auto","tokenizer_worker_num":1,"skip_tokenizer_init":false,"load_format":"auto","model_loader_extra_config":"{}","trust_remote_code":false,"context_length":null,"is_embedding":false,"enable_multimodal":null,"revision":null,"model_impl":"auto","host":"0.0.0.0","port":31885,"fastapi_root_path":"","grpc_mode":false,"skip_server_warmup":false,"warmups":null,"nccl_port":null,"checkpoint_engine_wait_weights_before_ready":false,"ssl_keyfile":null,"ssl_certfile":null,"ssl_ca_certs":null,"ssl_keyfile_password":null,"enable_ssl_refresh":false,"dtype":"auto","quantization":null,"quantization_param_path":null,"kv_cache_dtype":"auto","enable_fp32_lm_head":false,"modelopt_quant":null,"modelopt_checkpoint_restore_path":null,"modelopt_checkpoint_save_path":null,"modelopt_export_path":null,"quantize_and_serve":false,"rl_quant_profile":null,"mem_fraction_static":0.907,"max_running_requests":128,"max_queued_requests":null,"max_total_tokens":20480,"chunked_prefill_size":8192,"enable_dynamic_chunking":false,"max_prefill_tokens":16384,"prefill_max_requests":null,"schedule_policy":"fcfs","enable_priority_scheduling":false,"disable_priority_preemption":false,"default_priority_value":null,"abort_on_priority_when_disabled":false,"schedule_low_priority_values_first":false,"priority_scheduling_preemption_threshold":10,"schedule_conservativeness":1.0,"page_size":1,"swa_full_tokens_ratio":0.8,"disable_hybrid_swa_memory":false,"radix_eviction_policy":"lru","enable_prefill_delayer":false,"prefill_delayer_max_delay_passes":30,"prefill_delayer_token_usage_low_watermark":null,"prefill_delayer_forward_passes_buckets":null,"prefill_delayer_wait_seconds_buckets":null,"device":"cuda","tp_size":1,"pp_size":1,"pp_max_micro_batch_size":null,"pp_async_batch_depth":0,"stream_interval":1,"incremental_streaming_output":false,"enable_streaming_session":false,"random_seed":281995258,"constrained_json_whitespace_pattern":null,"constrained_json_disable_any_whitespace":false,"watchdog_timeout":300,"soft_watchdog_timeout":300,"dist_timeout":null,"download_dir":null,"model_checksum":null,"base_gpu_id":0,"gpu_id_step":1,"sleep_on_idle":false,"use_ray":false,"custom_sigquit_handler":null,"log_level":"warning","log_level_http":null,"log_requests":false,"log_requests_level":2,"log_requests_format":"text","log_requests_target":null,"uvicorn_access_log_exclude_prefixes":[],"crash_dump_folder":null,"show_time_cost":false,"enable_metrics":false,"enable_mfu_metrics":false,"enable_metrics_for_all_schedulers":false,"tokenizer_metrics_custom_labels_header":"x-custom-labels","tokenizer_metrics_allowed_custom_labels":null,"extra_metric_labels":null,"bucket_time_to_first_token":null,"bucket_inter_token_latency":null,"bucket_e2e_request_latency":null,"collect_tokens_histogram":false,"prompt_tokens_buckets":null,"generation_tokens_buckets":null,"gc_warning_threshold_secs":0.0,"decode_log_interval":40,"enable_request_time_stats_logging":false,"kv_events_config":null,"enable_trace":false,"otlp_traces_endpoint":"localhost:4317","export_metrics_to_file":false,"export_metrics_to_file_dir":null,"api_key":null,"admin_api_key":null,"served_model_name":"qwen/qwen2.5-0.5b-instruct","weight_version":"default","chat_template":null,"hf_chat_template_name":null,"completion_template":null,"file_storage_path":"sglang_storage","enable_cache_report":false,"reasoning_parser":null,"tool_call_parser":null,"tool_server":null,"sampling_defaults":"model","dp_size":1,"load_balance_method":"round_robin","attn_cp_size":1,"moe_dp_size":1,"dist_init_addr":null,"nnodes":1,"node_rank":0,"json_model_override_args":"{}","preferred_sampling_params":null,"enable_lora":null,"enable_lora_overlap_loading":null,"max_lora_rank":null,"lora_target_modules":null,"lora_paths":null,"max_loaded_loras":null,"max_loras_per_batch":8,"lora_eviction_policy":"lru","lora_backend":"csgmv","max_lora_chunk_size":16,"experts_shared_outer_loras":null,"attention_backend":"fa3","decode_attention_backend":null,"prefill_attention_backend":null,"sampling_backend":"flashinfer","grammar_backend":"xgrammar","mm_attention_backend":null,"fp8_gemm_runner_backend":"auto","fp4_gemm_runner_backend":"auto","nsa_prefill_backend":null,"nsa_decode_backend":null,"disable_flashinfer_autotune":false,"mamba_backend":"triton","speculative_algorithm":null,"speculative_draft_model_path":null,"speculative_draft_model_revision":null,"speculative_draft_load_format":null,"speculative_num_steps":null,"speculative_eagle_topk":null,"speculative_num_draft_tokens":null,"speculative_accept_threshold_single":1.0,"speculative_accept_threshold_acc":1.0,"speculative_token_map":null,"speculative_attention_mode":"prefill","speculative_draft_attention_backend":null,"speculative_moe_runner_backend":"auto","speculative_moe_a2a_backend":null,"speculative_draft_model_quantization":null,"speculative_ngram_min_bfs_breadth":1,"speculative_ngram_max_bfs_breadth":10,"speculative_ngram_match_type":"BFS","speculative_ngram_max_trie_depth":18,"speculative_ngram_capacity":10000000,"enable_multi_layer_eagle":false,"ep_size":1,"moe_a2a_backend":"none","moe_runner_backend":"auto","flashinfer_mxfp4_moe_precision":"default","enable_flashinfer_allreduce_fusion":false,"enforce_disable_flashinfer_allreduce_fusion":false,"enable_aiter_allreduce_fusion":false,"deepep_mode":"auto","ep_num_redundant_experts":0,"ep_dispatch_algorithm":null,"init_expert_location":"trivial","enable_eplb":false,"eplb_algorithm":"auto","eplb_rebalance_num_iterations":1000,"eplb_rebalance_layers_per_chunk":null,"eplb_min_rebalancing_utilization_threshold":1.0,"expert_distribution_recorder_mode":null,"expert_distribution_recorder_buffer_size":1000,"enable_expert_distribution_metrics":false,"deepep_config":null,"moe_dense_tp_size":null,"elastic_ep_backend":null,"enable_elastic_expert_backup":false,"mooncake_ib_device":null,"max_mamba_cache_size":null,"mamba_ssm_dtype":null,"mamba_full_memory_ratio":0.9,"mamba_scheduler_strategy":"no_buffer","mamba_track_interval":256,"linear_attn_backend":"triton","linear_attn_decode_backend":null,"linear_attn_prefill_backend":null,"enable_hierarchical_cache":false,"hicache_ratio":2.0,"hicache_size":0,"hicache_write_policy":"write_through","hicache_io_backend":"kernel","hicache_mem_layout":"layer_first","hicache_storage_backend":null,"hicache_storage_prefetch_policy":"best_effort","hicache_storage_backend_extra_config":null,"enable_hisparse":false,"hisparse_config":null,"enable_lmcache":false,"kt_weight_path":null,"kt_method":"AMXINT4","kt_cpuinfer":null,"kt_threadpool_count":2,"kt_num_gpu_experts":null,"kt_max_deferred_experts_per_token":null,"dllm_algorithm":null,"dllm_algorithm_config":null,"enable_double_sparsity":false,"ds_channel_config_path":null,"ds_heavy_channel_num":32,"ds_heavy_token_num":256,"ds_heavy_channel_type":"qk","ds_sparse_decode_threshold":4096,"cpu_offload_gb":0,"offload_group_size":-1,"offload_num_in_group":1,"offload_prefetch_step":1,"offload_mode":"cpu","multi_item_scoring_delimiter":null,"disable_radix_cache":false,"cuda_graph_max_bs":4,"cuda_graph_bs":[1,2,4],"disable_cuda_graph":true,"disable_cuda_graph_padding":false,"enable_profile_cuda_graph":false,"enable_cudagraph_gc":false,"enable_layerwise_nvtx_marker":false,"enable_nccl_nvls":false,"enable_symm_mem":false,"disable_flashinfer_cutlass_moe_fp4_allgather":false,"enable_tokenizer_batch_encode":false,"disable_tokenizer_batch_decode":false,"disable_outlines_disk_cache":false,"disable_custom_all_reduce":false,"enable_mscclpp":false,"enable_torch_symm_mem":false,"pre_warm_nccl":false,"disable_overlap_schedule":false,"enable_mixed_chunk":false,"enable_dp_attention":false,"enable_dp_lm_head":false,"enable_two_batch_overlap":false,"enable_single_batch_overlap":false,"tbo_token_distribution_threshold":0.48,"enable_torch_compile":false,"disable_piecewise_cuda_graph":false,"enforce_piecewise_cuda_graph":false,"enable_torch_compile_debug_mode":false,"torch_compile_max_bs":32,"piecewise_cuda_graph_max_tokens":8192,"piecewise_cuda_graph_tokens":[4,8,12,16,20,24,28,32,48,64,80,96,112,128,144,160,176,192,208,224,240,256,288,320,352,384,416,448,480,512,576,640,704,768,832,896,960,1024,1280,1536,1792,2048,2304,2560,2816,3072,3328,3584,3840,4096,4608,5120,5632,6144,6656,7168,7680,8192],"piecewise_cuda_graph_compiler":"eager","torchao_config":"","enable_nan_detection":false,"enable_p2p_check":false,"triton_attention_reduce_in_fp32":false,"triton_attention_num_kv_splits":8,"triton_attention_split_tile_size":null,"num_continuous_decode_steps":1,"delete_ckpt_after_loading":false,"enable_memory_saver":false,"enable_weights_cpu_backup":false,"enable_draft_weights_cpu_backup":false,"allow_auto_truncate":false,"enable_custom_logit_processor":false,"flashinfer_mla_disable_ragged":false,"disable_shared_experts_fusion":false,"disable_chunked_prefix_cache":false,"disable_fast_image_processor":false,"keep_mm_feature_on_device":false,"enable_return_hidden_states":false,"enable_return_routed_experts":false,"scheduler_recv_interval":1,"numa_node":null,"enable_deterministic_inference":false,"rl_on_policy_target":null,"enable_attn_tp_input_scattered":false,"gc_threshold":null,"enable_nsa_prefill_context_parallel":false,"nsa_prefill_cp_mode":"round-robin-split","enable_fused_qk_norm_rope":false,"enable_precise_embedding_interpolation":false,"enable_fused_moe_sum_all_reduce":false,"enable_prefill_context_parallel":false,"prefill_cp_mode":"in-seq-split","enable_dynamic_batch_tokenizer":false,"dynamic_batch_tokenizer_batch_size":32,"dynamic_batch_tokenizer_batch_timeout":0.002,"debug_tensor_dump_output_folder":null,"debug_tensor_dump_layers":null,"debug_tensor_dump_input_file":null,"debug_tensor_dump_inject":false,"disaggregation_mode":"null","disaggregation_transfer_backend":"mooncake","disaggregation_bootstrap_port":8998,"disaggregation_ib_device":null,"disaggregation_decode_enable_offload_kvcache":false,"num_reserved_decode_tokens":512,"disaggregation_decode_polling_interval":1,"encoder_only":false,"language_only":false,"encoder_transfer_backend":"zmq_to_scheduler","encoder_urls":[],"enable_adaptive_dispatch_to_encoder":false,"custom_weight_loader":[],"weight_loader_disable_mmap":false,"remote_instance_weight_loader_seed_instance_ip":null,"remote_instance_weight_loader_seed_instance_service_port":null,"remote_instance_weight_loader_send_weights_group_ports":null,"remote_instance_weight_loader_backend":"nccl","remote_instance_weight_loader_start_seed_via_transfer_engine":false,"engine_info_bootstrap_port":6789,"modelexpress_config":null,"enable_pdmux":false,"pdmux_config_path":null,"sm_group_num":8,"enable_broadcast_mm_inputs_process":false,"enable_prefix_mm_cache":false,"mm_enable_dp_encoder":false,"mm_process_config":{},"limit_mm_data_per_request":null,"enable_mm_global_cache":false,"decrypted_config_file":null,"decrypted_draft_config_file":null,"forward_hooks":null,"status":"ready","max_total_num_tokens":20480,"max_req_input_len":20474,"internal_states":[{"model_path":"qwen/qwen2.5-0.5b-instruct","tokenizer_path":"qwen/qwen2.5-0.5b-instruct","tokenizer_mode":"auto","tokenizer_worker_num":1,"skip_tokenizer_init":false,"load_format":"auto","model_loader_extra_config":"{}","trust_remote_code":false,"context_length":null,"is_embedding":false,"enable_multimodal":null,"revision":null,"model_impl":"auto","host":"0.0.0.0","port":31885,"fastapi_root_path":"","grpc_mode":false,"skip_server_warmup":false,"warmups":null,"nccl_port":null,"checkpoint_engine_wait_weights_before_ready":false,"ssl_keyfile":null,"ssl_certfile":null,"ssl_ca_certs":null,"ssl_keyfile_password":null,"enable_ssl_refresh":false,"dtype":"auto","quantization":null,"quantization_param_path":null,"kv_cache_dtype":"auto","enable_fp32_lm_head":false,"modelopt_quant":null,"modelopt_checkpoint_restore_path":null,"modelopt_checkpoint_save_path":null,"modelopt_export_path":null,"quantize_and_serve":false,"rl_quant_profile":null,"mem_fraction_static":0.907,"max_running_requests":128,"max_queued_requests":null,"max_total_tokens":20480,"chunked_prefill_size":8192,"enable_dynamic_chunking":false,"max_prefill_tokens":16384,"prefill_max_requests":null,"schedule_policy":"fcfs","enable_priority_scheduling":false,"disable_priority_preemption":false,"default_priority_value":null,"abort_on_priority_when_disabled":false,"schedule_low_priority_values_first":false,"priority_scheduling_preemption_threshold":10,"schedule_conservativeness":1.0,"page_size":1,"swa_full_tokens_ratio":0.8,"disable_hybrid_swa_memory":false,"radix_eviction_policy":"lru","enable_prefill_delayer":false,"prefill_delayer_max_delay_passes":30,"prefill_delayer_token_usage_low_watermark":null,"prefill_delayer_forward_passes_buckets":null,"prefill_delayer_wait_seconds_buckets":null,"device":"cuda","tp_size":1,"pp_size":1,"pp_max_micro_batch_size":128,"pp_async_batch_depth":0,"stream_interval":1,"incremental_streaming_output":false,"enable_streaming_session":false,"random_seed":281995258,"constrained_json_whitespace_pattern":null,"constrained_json_disable_any_whitespace":false,"watchdog_timeout":300,"soft_watchdog_timeout":300,"dist_timeout":null,"download_dir":null,"model_checksum":null,"base_gpu_id":0,"gpu_id_step":1,"sleep_on_idle":false,"use_ray":false,"custom_sigquit_handler":null,"log_level":"warning","log_level_http":null,"log_requests":false,"log_requests_level":2,"log_requests_format":"text","log_requests_target":null,"uvicorn_access_log_exclude_prefixes":[],"crash_dump_folder":null,"show_time_cost":false,"enable_metrics":false,"enable_mfu_metrics":false,"enable_metrics_for_all_schedulers":false,"tokenizer_metrics_custom_labels_header":"x-custom-labels","tokenizer_metrics_allowed_custom_labels":null,"extra_metric_labels":null,"bucket_time_to_first_token":null,"bucket_inter_token_latency":null,"bucket_e2e_request_latency":null,"collect_tokens_histogram":false,"prompt_tokens_buckets":null,"generation_tokens_buckets":null,"gc_warning_threshold_secs":0.0,"decode_log_interval":40,"enable_request_time_stats_logging":false,"kv_events_config":null,"enable_trace":false,"otlp_traces_endpoint":"localhost:4317","export_metrics_to_file":false,"export_metrics_to_file_dir":null,"api_key":null,"admin_api_key":null,"served_model_name":"qwen/qwen2.5-0.5b-instruct","weight_version":"default","chat_template":null,"hf_chat_template_name":null,"completion_template":null,"file_storage_path":"sglang_storage","enable_cache_report":false,"reasoning_parser":null,"tool_call_parser":null,"tool_server":null,"sampling_defaults":"model","dp_size":1,"load_balance_method":"round_robin","attn_cp_size":1,"moe_dp_size":1,"dist_init_addr":null,"nnodes":1,"node_rank":0,"json_model_override_args":"{}","preferred_sampling_params":null,"enable_lora":null,"enable_lora_overlap_loading":null,"max_lora_rank":null,"lora_target_modules":null,"lora_paths":null,"max_loaded_loras":null,"max_loras_per_batch":8,"lora_eviction_policy":"lru","lora_backend":"csgmv","max_lora_chunk_size":16,"experts_shared_outer_loras":null,"attention_backend":"fa3","decode_attention_backend":"fa3","prefill_attention_backend":"fa3","sampling_backend":"flashinfer","grammar_backend":"xgrammar","mm_attention_backend":null,"fp8_gemm_runner_backend":"auto","fp4_gemm_runner_backend":"auto","nsa_prefill_backend":null,"nsa_decode_backend":null,"disable_flashinfer_autotune":false,"mamba_backend":"triton","speculative_algorithm":null,"speculative_draft_model_path":null,"speculative_draft_model_revision":null,"speculative_draft_load_format":null,"speculative_num_steps":null,"speculative_eagle_topk":null,"speculative_num_draft_tokens":null,"speculative_accept_threshold_single":1.0,"speculative_accept_threshold_acc":1.0,"speculative_token_map":null,"speculative_attention_mode":"prefill","speculative_draft_attention_backend":null,"speculative_moe_runner_backend":"auto","speculative_moe_a2a_backend":null,"speculative_draft_model_quantization":null,"speculative_ngram_min_bfs_breadth":1,"speculative_ngram_max_bfs_breadth":10,"speculative_ngram_match_type":"BFS","speculative_ngram_max_trie_depth":18,"speculative_ngram_capacity":10000000,"enable_multi_layer_eagle":false,"ep_size":1,"moe_a2a_backend":"none","moe_runner_backend":"auto","flashinfer_mxfp4_moe_precision":"default","enable_flashinfer_allreduce_fusion":false,"enforce_disable_flashinfer_allreduce_fusion":false,"enable_aiter_allreduce_fusion":false,"deepep_mode":"auto","ep_num_redundant_experts":0,"ep_dispatch_algorithm":null,"init_expert_location":"trivial","enable_eplb":false,"eplb_algorithm":"auto","eplb_rebalance_num_iterations":1000,"eplb_rebalance_layers_per_chunk":null,"eplb_min_rebalancing_utilization_threshold":1.0,"expert_distribution_recorder_mode":null,"expert_distribution_recorder_buffer_size":1000,"enable_expert_distribution_metrics":false,"deepep_config":null,"moe_dense_tp_size":null,"elastic_ep_backend":null,"enable_elastic_expert_backup":false,"mooncake_ib_device":null,"max_mamba_cache_size":null,"mamba_ssm_dtype":null,"mamba_full_memory_ratio":0.9,"mamba_scheduler_strategy":"no_buffer","mamba_track_interval":256,"linear_attn_backend":"triton","linear_attn_decode_backend":null,"linear_attn_prefill_backend":null,"enable_hierarchical_cache":false,"hicache_ratio":2.0,"hicache_size":0,"hicache_write_policy":"write_through","hicache_io_backend":"kernel","hicache_mem_layout":"layer_first","hicache_storage_backend":null,"hicache_storage_prefetch_policy":"best_effort","hicache_storage_backend_extra_config":null,"enable_hisparse":false,"hisparse_config":null,"enable_lmcache":false,"kt_weight_path":null,"kt_method":"AMXINT4","kt_cpuinfer":null,"kt_threadpool_count":2,"kt_num_gpu_experts":null,"kt_max_deferred_experts_per_token":null,"dllm_algorithm":null,"dllm_algorithm_config":null,"enable_double_sparsity":false,"ds_channel_config_path":null,"ds_heavy_channel_num":32,"ds_heavy_token_num":256,"ds_heavy_channel_type":"qk","ds_sparse_decode_threshold":4096,"cpu_offload_gb":0,"offload_group_size":-1,"offload_num_in_group":1,"offload_prefetch_step":1,"offload_mode":"cpu","multi_item_scoring_delimiter":null,"disable_radix_cache":false,"cuda_graph_max_bs":4,"cuda_graph_bs":[1,2,4],"disable_cuda_graph":true,"disable_cuda_graph_padding":false,"enable_profile_cuda_graph":false,"enable_cudagraph_gc":false,"enable_layerwise_nvtx_marker":false,"enable_nccl_nvls":false,"enable_symm_mem":false,"disable_flashinfer_cutlass_moe_fp4_allgather":false,"enable_tokenizer_batch_encode":false,"disable_tokenizer_batch_decode":false,"disable_outlines_disk_cache":false,"disable_custom_all_reduce":false,"enable_mscclpp":false,"enable_torch_symm_mem":false,"pre_warm_nccl":false,"disable_overlap_schedule":false,"enable_mixed_chunk":false,"enable_dp_attention":false,"enable_dp_lm_head":false,"enable_two_batch_overlap":false,"enable_single_batch_overlap":false,"tbo_token_distribution_threshold":0.48,"enable_torch_compile":false,"disable_piecewise_cuda_graph":false,"enforce_piecewise_cuda_graph":false,"enable_torch_compile_debug_mode":false,"torch_compile_max_bs":32,"piecewise_cuda_graph_max_tokens":8192,"piecewise_cuda_graph_tokens":[4,8,12,16,20,24,28,32,48,64,80,96,112,128,144,160,176,192,208,224,240,256,288,320,352,384,416,448,480,512,576,640,704,768,832,896,960,1024,1280,1536,1792,2048,2304,2560,2816,3072,3328,3584,3840,4096,4608,5120,5632,6144,6656,7168,7680,8192],"piecewise_cuda_graph_compiler":"eager","torchao_config":"","enable_nan_detection":false,"enable_p2p_check":false,"triton_attention_reduce_in_fp32":false,"triton_attention_num_kv_splits":8,"triton_attention_split_tile_size":null,"num_continuous_decode_steps":1,"delete_ckpt_after_loading":false,"enable_memory_saver":false,"enable_weights_cpu_backup":false,"enable_draft_weights_cpu_backup":false,"allow_auto_truncate":false,"enable_custom_logit_processor":false,"flashinfer_mla_disable_ragged":false,"disable_shared_experts_fusion":false,"disable_chunked_prefix_cache":true,"disable_fast_image_processor":false,"keep_mm_feature_on_device":false,"enable_return_hidden_states":false,"enable_return_routed_experts":false,"scheduler_recv_interval":1,"numa_node":null,"enable_deterministic_inference":false,"rl_on_policy_target":null,"enable_attn_tp_input_scattered":false,"gc_threshold":null,"enable_nsa_prefill_context_parallel":false,"nsa_prefill_cp_mode":"round-robin-split","enable_fused_qk_norm_rope":false,"enable_precise_embedding_interpolation":false,"enable_fused_moe_sum_all_reduce":false,"enable_prefill_context_parallel":false,"prefill_cp_mode":"in-seq-split","enable_dynamic_batch_tokenizer":false,"dynamic_batch_tokenizer_batch_size":32,"dynamic_batch_tokenizer_batch_timeout":0.002,"debug_tensor_dump_output_folder":null,"debug_tensor_dump_layers":null,"debug_tensor_dump_input_file":null,"debug_tensor_dump_inject":false,"disaggregation_mode":"null","disaggregation_transfer_backend":"mooncake","disaggregation_bootstrap_port":8998,"disaggregation_ib_device":null,"disaggregation_decode_enable_offload_kvcache":false,"num_reserved_decode_tokens":512,"disaggregation_decode_polling_interval":1,"encoder_only":false,"language_only":false,"encoder_transfer_backend":"zmq_to_scheduler","encoder_urls":[],"enable_adaptive_dispatch_to_encoder":false,"custom_weight_loader":[],"weight_loader_disable_mmap":false,"remote_instance_weight_loader_seed_instance_ip":null,"remote_instance_weight_loader_seed_instance_service_port":null,"remote_instance_weight_loader_send_weights_group_ports":null,"remote_instance_weight_loader_backend":"nccl","remote_instance_weight_loader_start_seed_via_transfer_engine":false,"engine_info_bootstrap_port":6789,"modelexpress_config":null,"enable_pdmux":false,"pdmux_config_path":null,"sm_group_num":8,"enable_broadcast_mm_inputs_process":false,"enable_prefix_mm_cache":false,"mm_enable_dp_encoder":false,"mm_process_config":{},"limit_mm_data_per_request":null,"enable_mm_global_cache":false,"decrypted_config_file":null,"decrypted_draft_config_file":null,"forward_hooks":null,"use_mla_backend":false,"_mx_config_cache":{},"last_gen_throughput":125.73340023064716,"memory_usage":{"weight":0.98,"kvcache":0.23,"token_capacity":20480,"graph":0},"effective_max_running_requests_per_dp":128}],"version":"0.0.0.dev1+ged3435e37"}</strong>


## Health Check
- `/health`: Check the health of the server.
- `/health_generate`: Check the health of the server by generating one token.


```python
url = f"http://localhost:{port}/health_generate"

response = requests.get(url)
print_highlight(response.text)
```


<strong style='color: #00008B;'></strong>



```python
url = f"http://localhost:{port}/health"

response = requests.get(url)
print_highlight(response.text)
```


<strong style='color: #00008B;'></strong>


## Flush Cache

Flush the radix cache. It will be automatically triggered when the model weights are updated by the `/update_weights` API.

Parameters:
- `timeout` (query, float, default `0`, unit: seconds): Wait time for idle state before flushing. `0` means fail fast if not idle. When HiCache async operations are in-flight, a non-zero timeout allows the server to wait until idle before flushing, avoiding unnecessary 400 errors.

```bash
# With timeout (wait up to 30s for idle state)
curl -s -X POST "http://127.0.0.1:30000/flush_cache?timeout=30"
```


```python
url = f"http://localhost:{port}/flush_cache"

response = requests.post(url)
print_highlight(response.text)
```


<strong style='color: #00008B;'>Cache flushed.<br>Please check backend logs for more details. (When there are running or waiting requests, the operation will not be performed.)<br></strong>


## Update Weights From Disk

Update model weights from disk without restarting the server. Only applicable for models with the same architecture and parameter size.

SGLang support `update_weights_from_disk` API for continuous evaluation during training (save checkpoint to disk and update weights from disk).



```python
# successful update with same architecture and size

url = f"http://localhost:{port}/update_weights_from_disk"
data = {"model_path": "qwen/qwen2.5-0.5b-instruct"}

response = requests.post(url, json=data)
print_highlight(response.text)
assert response.json()["success"] is True
assert response.json()["message"] == "Succeeded to update model weights."
```

    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.37it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.34it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>{"success":true,"message":"Succeeded to update model weights.","num_paused_requests":0}</strong>



```python
# failed update with different parameter size or wrong name

url = f"http://localhost:{port}/update_weights_from_disk"
data = {"model_path": "qwen/qwen2.5-0.5b-instruct-wrong"}

response = requests.post(url, json=data)
response_json = response.json()
print_highlight(response_json)
assert response_json["success"] is False
assert response_json["message"] == (
    "Failed to get weights iterator: "
    "qwen/qwen2.5-0.5b-instruct-wrong"
    " (repository not found)."
)
```

    [2026-04-03 18:29:48] Failed to get weights iterator: qwen/qwen2.5-0.5b-instruct-wrong (repository not found).



<strong style='color: #00008B;'>{'success': False, 'message': 'Failed to get weights iterator: qwen/qwen2.5-0.5b-instruct-wrong (repository not found).', 'num_paused_requests': 0}</strong>



```python
terminate_process(server_process)
```

## Encode (embedding model)

Encode text into embeddings. Note that this API is only available for [embedding models](openai_api_embeddings.ipynb) and will raise an error for generation models.
Therefore, we launch a new server to server an embedding model.


```python
embedding_process, port = launch_server_cmd("""
python3 -m sglang.launch_server --model-path Alibaba-NLP/gte-Qwen2-1.5B-instruct \
    --host 0.0.0.0 --is-embedding --log-level warning
""")

wait_for_server(f"http://localhost:{port}", process=embedding_process)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-04-03 18:30:04.730 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:30:04] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:30:04.730 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:30:04] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:30:04.730 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:30:04] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:30:04.730 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:30:04] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:30:04.730 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:30:04] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:00<00:00,  1.52it/s]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.18s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.10s/it]


    2026-04-03 18:30:09,536 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 18:30:09] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:50,  2.99s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:50,  2.99s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:12,  1.30s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:12,  1.30s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:03<01:12,  1.30s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:28,  1.92it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:28,  1.92it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:28,  1.92it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:03<00:28,  1.92it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:12,  4.04it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:12,  4.04it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:12,  4.04it/s]Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:12,  4.04it/s]

    Compiling num tokens (num_tokens=3584):  12%|█▏        | 7/58 [00:03<00:12,  4.04it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:06,  7.50it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:06,  7.50it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:06,  7.50it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:06,  7.50it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:06,  7.50it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:06,  7.50it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:06,  7.50it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:03<00:02, 13.71it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:03<00:02, 13.71it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:03<00:02, 13.71it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:03<00:02, 13.71it/s]Compiling num tokens (num_tokens=1024):  29%|██▉       | 17/58 [00:03<00:02, 13.71it/s]

    Compiling num tokens (num_tokens=960):  29%|██▉       | 17/58 [00:03<00:02, 13.71it/s] Compiling num tokens (num_tokens=896):  29%|██▉       | 17/58 [00:03<00:02, 13.71it/s]Compiling num tokens (num_tokens=832):  29%|██▉       | 17/58 [00:03<00:02, 13.71it/s]Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:03<00:01, 21.53it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:03<00:01, 21.53it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:03<00:01, 21.53it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:03<00:01, 21.53it/s]Compiling num tokens (num_tokens=576):  41%|████▏     | 24/58 [00:03<00:01, 21.53it/s]Compiling num tokens (num_tokens=512):  41%|████▏     | 24/58 [00:03<00:01, 21.53it/s]Compiling num tokens (num_tokens=480):  41%|████▏     | 24/58 [00:03<00:01, 21.53it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:03<00:01, 27.84it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:03<00:01, 27.84it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:03<00:01, 27.84it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:03<00:01, 27.84it/s]

    Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:03<00:01, 27.84it/s]Compiling num tokens (num_tokens=320):  52%|█████▏    | 30/58 [00:03<00:01, 27.84it/s]Compiling num tokens (num_tokens=288):  52%|█████▏    | 30/58 [00:03<00:01, 27.84it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:03<00:00, 33.48it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:03<00:00, 33.48it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:03<00:00, 33.48it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:03<00:00, 33.48it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:03<00:00, 33.48it/s]Compiling num tokens (num_tokens=192):  62%|██████▏   | 36/58 [00:03<00:00, 33.48it/s]Compiling num tokens (num_tokens=176):  62%|██████▏   | 36/58 [00:04<00:00, 33.48it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:04<00:00, 38.17it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:04<00:00, 38.17it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:04<00:00, 38.17it/s]

    Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:04<00:00, 38.17it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:04<00:00, 38.17it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:04<00:00, 38.17it/s] Compiling num tokens (num_tokens=80):  72%|███████▏  | 42/58 [00:04<00:00, 38.17it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:04<00:00, 41.13it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:04<00:00, 41.13it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:04<00:00, 41.13it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:04<00:00, 41.13it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:04<00:00, 41.13it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:04<00:00, 41.13it/s]Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:04<00:00, 41.13it/s]Compiling num tokens (num_tokens=16):  83%|████████▎ | 48/58 [00:04<00:00, 41.13it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:04<00:00, 46.21it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:04<00:00, 46.21it/s]

    Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:04<00:00, 46.21it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:04<00:00, 46.21it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 13.49it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=134.25 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=134.25 GB):   2%|▏         | 1/58 [00:00<00:08,  6.80it/s]Capturing num tokens (num_tokens=7680 avail_mem=134.22 GB):   2%|▏         | 1/58 [00:00<00:08,  6.80it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=134.22 GB):   2%|▏         | 1/58 [00:00<00:08,  6.80it/s]Capturing num tokens (num_tokens=7168 avail_mem=134.22 GB):   5%|▌         | 3/58 [00:00<00:05,  9.36it/s]Capturing num tokens (num_tokens=6656 avail_mem=134.22 GB):   5%|▌         | 3/58 [00:00<00:05,  9.36it/s]Capturing num tokens (num_tokens=6144 avail_mem=134.22 GB):   5%|▌         | 3/58 [00:00<00:05,  9.36it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=134.22 GB):   9%|▊         | 5/58 [00:00<00:04, 10.91it/s]Capturing num tokens (num_tokens=5632 avail_mem=134.22 GB):   9%|▊         | 5/58 [00:00<00:04, 10.91it/s]Capturing num tokens (num_tokens=5120 avail_mem=134.22 GB):   9%|▊         | 5/58 [00:00<00:04, 10.91it/s]Capturing num tokens (num_tokens=5120 avail_mem=134.22 GB):  12%|█▏        | 7/58 [00:00<00:04, 12.64it/s]Capturing num tokens (num_tokens=4608 avail_mem=134.22 GB):  12%|█▏        | 7/58 [00:00<00:04, 12.64it/s]Capturing num tokens (num_tokens=4096 avail_mem=134.22 GB):  12%|█▏        | 7/58 [00:00<00:04, 12.64it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=134.22 GB):  16%|█▌        | 9/58 [00:00<00:03, 12.82it/s]Capturing num tokens (num_tokens=3840 avail_mem=134.22 GB):  16%|█▌        | 9/58 [00:00<00:03, 12.82it/s]Capturing num tokens (num_tokens=3584 avail_mem=134.22 GB):  16%|█▌        | 9/58 [00:00<00:03, 12.82it/s]Capturing num tokens (num_tokens=3584 avail_mem=134.22 GB):  19%|█▉        | 11/58 [00:00<00:03, 14.09it/s]Capturing num tokens (num_tokens=3328 avail_mem=134.21 GB):  19%|█▉        | 11/58 [00:00<00:03, 14.09it/s]Capturing num tokens (num_tokens=3072 avail_mem=134.21 GB):  19%|█▉        | 11/58 [00:00<00:03, 14.09it/s]Capturing num tokens (num_tokens=2816 avail_mem=134.21 GB):  19%|█▉        | 11/58 [00:00<00:03, 14.09it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=134.21 GB):  24%|██▍       | 14/58 [00:00<00:02, 17.63it/s]Capturing num tokens (num_tokens=2560 avail_mem=134.21 GB):  24%|██▍       | 14/58 [00:00<00:02, 17.63it/s]Capturing num tokens (num_tokens=2304 avail_mem=134.20 GB):  24%|██▍       | 14/58 [00:01<00:02, 17.63it/s]Capturing num tokens (num_tokens=2048 avail_mem=134.19 GB):  24%|██▍       | 14/58 [00:01<00:02, 17.63it/s]Capturing num tokens (num_tokens=1792 avail_mem=134.19 GB):  24%|██▍       | 14/58 [00:01<00:02, 17.63it/s]Capturing num tokens (num_tokens=1792 avail_mem=134.19 GB):  31%|███       | 18/58 [00:01<00:01, 22.06it/s]Capturing num tokens (num_tokens=1536 avail_mem=134.19 GB):  31%|███       | 18/58 [00:01<00:01, 22.06it/s]Capturing num tokens (num_tokens=1280 avail_mem=134.19 GB):  31%|███       | 18/58 [00:01<00:01, 22.06it/s]Capturing num tokens (num_tokens=1024 avail_mem=134.18 GB):  31%|███       | 18/58 [00:01<00:01, 22.06it/s]

    Capturing num tokens (num_tokens=960 avail_mem=134.15 GB):  31%|███       | 18/58 [00:01<00:01, 22.06it/s] Capturing num tokens (num_tokens=960 avail_mem=134.15 GB):  38%|███▊      | 22/58 [00:01<00:01, 26.09it/s]Capturing num tokens (num_tokens=896 avail_mem=134.15 GB):  38%|███▊      | 22/58 [00:01<00:01, 26.09it/s]Capturing num tokens (num_tokens=832 avail_mem=134.16 GB):  38%|███▊      | 22/58 [00:01<00:01, 26.09it/s]Capturing num tokens (num_tokens=768 avail_mem=134.17 GB):  38%|███▊      | 22/58 [00:01<00:01, 26.09it/s]Capturing num tokens (num_tokens=704 avail_mem=134.16 GB):  38%|███▊      | 22/58 [00:01<00:01, 26.09it/s]Capturing num tokens (num_tokens=704 avail_mem=134.16 GB):  45%|████▍     | 26/58 [00:01<00:01, 29.13it/s]Capturing num tokens (num_tokens=640 avail_mem=134.16 GB):  45%|████▍     | 26/58 [00:01<00:01, 29.13it/s]Capturing num tokens (num_tokens=576 avail_mem=134.16 GB):  45%|████▍     | 26/58 [00:01<00:01, 29.13it/s]

    Capturing num tokens (num_tokens=512 avail_mem=134.15 GB):  45%|████▍     | 26/58 [00:01<00:01, 29.13it/s]Capturing num tokens (num_tokens=480 avail_mem=134.15 GB):  45%|████▍     | 26/58 [00:01<00:01, 29.13it/s]Capturing num tokens (num_tokens=480 avail_mem=134.15 GB):  52%|█████▏    | 30/58 [00:01<00:01, 25.02it/s]Capturing num tokens (num_tokens=448 avail_mem=134.15 GB):  52%|█████▏    | 30/58 [00:01<00:01, 25.02it/s]Capturing num tokens (num_tokens=416 avail_mem=134.14 GB):  52%|█████▏    | 30/58 [00:01<00:01, 25.02it/s]

    Capturing num tokens (num_tokens=384 avail_mem=134.14 GB):  52%|█████▏    | 30/58 [00:01<00:01, 25.02it/s]Capturing num tokens (num_tokens=384 avail_mem=134.14 GB):  57%|█████▋    | 33/58 [00:01<00:01, 25.00it/s]Capturing num tokens (num_tokens=352 avail_mem=134.13 GB):  57%|█████▋    | 33/58 [00:01<00:01, 25.00it/s]Capturing num tokens (num_tokens=320 avail_mem=134.13 GB):  57%|█████▋    | 33/58 [00:01<00:01, 25.00it/s]Capturing num tokens (num_tokens=288 avail_mem=134.14 GB):  57%|█████▋    | 33/58 [00:01<00:01, 25.00it/s]Capturing num tokens (num_tokens=256 avail_mem=134.13 GB):  57%|█████▋    | 33/58 [00:01<00:01, 25.00it/s]Capturing num tokens (num_tokens=256 avail_mem=134.13 GB):  64%|██████▍   | 37/58 [00:01<00:00, 26.96it/s]Capturing num tokens (num_tokens=240 avail_mem=134.13 GB):  64%|██████▍   | 37/58 [00:01<00:00, 26.96it/s]Capturing num tokens (num_tokens=224 avail_mem=134.13 GB):  64%|██████▍   | 37/58 [00:01<00:00, 26.96it/s]

    Capturing num tokens (num_tokens=208 avail_mem=134.12 GB):  64%|██████▍   | 37/58 [00:01<00:00, 26.96it/s]Capturing num tokens (num_tokens=192 avail_mem=134.12 GB):  64%|██████▍   | 37/58 [00:01<00:00, 26.96it/s]Capturing num tokens (num_tokens=192 avail_mem=134.12 GB):  71%|███████   | 41/58 [00:01<00:00, 29.36it/s]Capturing num tokens (num_tokens=176 avail_mem=134.12 GB):  71%|███████   | 41/58 [00:01<00:00, 29.36it/s]Capturing num tokens (num_tokens=160 avail_mem=134.12 GB):  71%|███████   | 41/58 [00:01<00:00, 29.36it/s]Capturing num tokens (num_tokens=144 avail_mem=134.11 GB):  71%|███████   | 41/58 [00:01<00:00, 29.36it/s]Capturing num tokens (num_tokens=128 avail_mem=134.11 GB):  71%|███████   | 41/58 [00:01<00:00, 29.36it/s]Capturing num tokens (num_tokens=128 avail_mem=134.11 GB):  78%|███████▊  | 45/58 [00:02<00:00, 31.28it/s]Capturing num tokens (num_tokens=112 avail_mem=134.10 GB):  78%|███████▊  | 45/58 [00:02<00:00, 31.28it/s]Capturing num tokens (num_tokens=96 avail_mem=134.10 GB):  78%|███████▊  | 45/58 [00:02<00:00, 31.28it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=134.09 GB):  78%|███████▊  | 45/58 [00:02<00:00, 31.28it/s]Capturing num tokens (num_tokens=64 avail_mem=134.09 GB):  78%|███████▊  | 45/58 [00:02<00:00, 31.28it/s]Capturing num tokens (num_tokens=64 avail_mem=134.09 GB):  84%|████████▍ | 49/58 [00:02<00:00, 32.44it/s]Capturing num tokens (num_tokens=48 avail_mem=134.09 GB):  84%|████████▍ | 49/58 [00:02<00:00, 32.44it/s]Capturing num tokens (num_tokens=32 avail_mem=134.08 GB):  84%|████████▍ | 49/58 [00:02<00:00, 32.44it/s]Capturing num tokens (num_tokens=28 avail_mem=134.08 GB):  84%|████████▍ | 49/58 [00:02<00:00, 32.44it/s]Capturing num tokens (num_tokens=24 avail_mem=134.08 GB):  84%|████████▍ | 49/58 [00:02<00:00, 32.44it/s]Capturing num tokens (num_tokens=24 avail_mem=134.08 GB):  91%|█████████▏| 53/58 [00:02<00:00, 33.61it/s]Capturing num tokens (num_tokens=20 avail_mem=134.07 GB):  91%|█████████▏| 53/58 [00:02<00:00, 33.61it/s]Capturing num tokens (num_tokens=16 avail_mem=134.07 GB):  91%|█████████▏| 53/58 [00:02<00:00, 33.61it/s]

    Capturing num tokens (num_tokens=12 avail_mem=134.06 GB):  91%|█████████▏| 53/58 [00:02<00:00, 33.61it/s]Capturing num tokens (num_tokens=8 avail_mem=134.06 GB):  91%|█████████▏| 53/58 [00:02<00:00, 33.61it/s] Capturing num tokens (num_tokens=8 avail_mem=134.06 GB):  98%|█████████▊| 57/58 [00:02<00:00, 34.69it/s]Capturing num tokens (num_tokens=4 avail_mem=134.06 GB):  98%|█████████▊| 57/58 [00:02<00:00, 34.69it/s]Capturing num tokens (num_tokens=4 avail_mem=134.06 GB): 100%|██████████| 58/58 [00:02<00:00, 24.54it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



```python
# successful encode for embedding model

url = f"http://localhost:{port}/encode"
data = {"model": "Alibaba-NLP/gte-Qwen2-1.5B-instruct", "text": "Once upon a time"}

response = requests.post(url, json=data)
response_json = response.json()
print_highlight(f"Text embedding (first 10): {response_json['embedding'][:10]}")
```


<strong style='color: #00008B;'>Text embedding (first 10): [-0.00024890899658203125, -0.0499267578125, -0.003246307373046875, 0.01108551025390625, -0.01406097412109375, 0.016021728515625, -0.01442718505859375, 0.005908966064453125, -0.0228424072265625, 0.0272979736328125]</strong>



```python
terminate_process(embedding_process)
```

## v1/rerank (cross encoder rerank model)
Rerank a list of documents given a query using a cross-encoder model. Note that this API is only available for cross encoder model like [BAAI/bge-reranker-v2-m3](https://huggingface.co/BAAI/bge-reranker-v2-m3) with `attention-backend` `triton` and `torch_native`.



```python
reranker_process, port = launch_server_cmd("""
python3 -m sglang.launch_server --model-path BAAI/bge-reranker-v2-m3 \
    --host 0.0.0.0 --disable-radix-cache --chunked-prefill-size -1 --attention-backend triton --is-embedding --log-level warning
""")

wait_for_server(f"http://localhost:{port}", process=reranker_process)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-04-03 18:30:34] No HuggingFace chat template found


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-04-03 18:30:42.703 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:30:42] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:30:42.703 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:30:42] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:30:42.703 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:30:42] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:30:42.703 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:30:42] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:30:42.703 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:30:42] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  1.76it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  1.76it/s]


    [2026-04-03 18:30:45] Disable piecewise CUDA graph because the model is not a language model


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



```python
# compute rerank scores for query and documents

url = f"http://localhost:{port}/v1/rerank"
data = {
    "model": "BAAI/bge-reranker-v2-m3",
    "query": "what is panda?",
    "documents": [
        "hi",
        "The giant panda (Ailuropoda melanoleuca), sometimes called a panda bear or simply panda, is a bear species endemic to China.",
    ],
}

response = requests.post(url, json=data)
response_json = response.json()
for item in response_json:
    print_highlight(f"Score: {item['score']:.2f} - Document: '{item['document']}'")
```


<strong style='color: #00008B;'>Score: 5.27 - Document: 'The giant panda (Ailuropoda melanoleuca), sometimes called a panda bear or simply panda, is a bear species endemic to China.'</strong>



<strong style='color: #00008B;'>Score: -8.19 - Document: 'hi'</strong>



```python
terminate_process(reranker_process)
```

## v1/score (decoder-only scoring)

Compute token probabilities for specified tokens given a query and items. This is useful for classification tasks, scoring responses, or computing log-probabilities.

Parameters:
- `query`: Query text
- `items`: Item text(s) to score
- `label_token_ids`: Token IDs to compute probabilities for
- `apply_softmax`: Whether to apply softmax to get normalized probabilities (default: False)
- `item_first`: Whether items come first in concatenation order (default: False)
- `model`: Model name

The response contains `scores` - a list of probability lists, one per item, each in the order of `label_token_ids`.


```python
score_process, port = launch_server_cmd("""
python3 -m sglang.launch_server --model-path qwen/qwen2.5-0.5b-instruct \
    --host 0.0.0.0 --log-level warning
""")

wait_for_server(f"http://localhost:{port}", process=score_process)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-04-03 18:31:11.269 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:31:11] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:31:11.269 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:31:11] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:31:11.269 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:31:11] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:31:11.269 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:31:11] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:31:11.269 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:31:11] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.68it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  7.67it/s]


    2026-04-03 18:31:13,744 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 18:31:13] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:33,  2.68s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:33,  2.68s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:33,  2.68s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:02<00:40,  1.36it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:02<00:40,  1.36it/s]Compiling num tokens (num_tokens=6144):   5%|▌         | 3/58 [00:02<00:40,  1.36it/s]Compiling num tokens (num_tokens=5632):   5%|▌         | 3/58 [00:02<00:40,  1.36it/s]Compiling num tokens (num_tokens=5120):   5%|▌         | 3/58 [00:02<00:40,  1.36it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:02<00:13,  3.91it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:02<00:13,  3.91it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:02<00:13,  3.91it/s]Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:02<00:13,  3.91it/s]Compiling num tokens (num_tokens=3584):  12%|█▏        | 7/58 [00:02<00:13,  3.91it/s]Compiling num tokens (num_tokens=3328):  12%|█▏        | 7/58 [00:03<00:13,  3.91it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:03<00:05,  7.78it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:03<00:05,  7.78it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:03<00:05,  7.78it/s]Compiling num tokens (num_tokens=2560):  21%|██        | 12/58 [00:03<00:05,  7.78it/s]Compiling num tokens (num_tokens=2304):  21%|██        | 12/58 [00:03<00:05,  7.78it/s]Compiling num tokens (num_tokens=2048):  21%|██        | 12/58 [00:03<00:05,  7.78it/s]Compiling num tokens (num_tokens=1792):  21%|██        | 12/58 [00:03<00:05,  7.78it/s]

    Compiling num tokens (num_tokens=1536):  21%|██        | 12/58 [00:03<00:05,  7.78it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:02, 14.36it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:02, 14.36it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:02, 14.36it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:02, 14.36it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:02, 14.36it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:02, 14.36it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:02, 14.36it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:02, 14.36it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 21.34it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 21.34it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 21.34it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 21.34it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 21.34it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 21.34it/s]

    Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 21.34it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 21.34it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 28.46it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 28.46it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 28.46it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 28.46it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 28.46it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 28.46it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 28.46it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 32.33it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 32.33it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 32.33it/s]

    Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 32.33it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 32.33it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 32.33it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 32.33it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 31.52it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 31.52it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 31.52it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 31.52it/s]

    Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 31.52it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 31.52it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 32.29it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 32.29it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 32.29it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 32.29it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 32.29it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 32.29it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:03<00:00, 33.21it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:03<00:00, 33.21it/s]

    Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:04<00:00, 33.21it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:04<00:00, 33.21it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 14.37it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=100.50 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=100.50 GB):   2%|▏         | 1/58 [00:00<00:08,  6.55it/s]Capturing num tokens (num_tokens=7680 avail_mem=100.47 GB):   2%|▏         | 1/58 [00:00<00:08,  6.55it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=100.47 GB):   3%|▎         | 2/58 [00:00<00:07,  7.49it/s]Capturing num tokens (num_tokens=7168 avail_mem=101.33 GB):   3%|▎         | 2/58 [00:00<00:07,  7.49it/s]Capturing num tokens (num_tokens=7168 avail_mem=101.33 GB):   5%|▌         | 3/58 [00:00<00:06,  8.00it/s]Capturing num tokens (num_tokens=6656 avail_mem=100.54 GB):   5%|▌         | 3/58 [00:00<00:06,  8.00it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=100.54 GB):   7%|▋         | 4/58 [00:00<00:06,  7.93it/s]Capturing num tokens (num_tokens=6144 avail_mem=100.54 GB):   7%|▋         | 4/58 [00:00<00:06,  7.93it/s]Capturing num tokens (num_tokens=5632 avail_mem=101.20 GB):   7%|▋         | 4/58 [00:00<00:06,  7.93it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=101.20 GB):  10%|█         | 6/58 [00:00<00:05,  8.85it/s]Capturing num tokens (num_tokens=5120 avail_mem=100.60 GB):  10%|█         | 6/58 [00:00<00:05,  8.85it/s]Capturing num tokens (num_tokens=4608 avail_mem=100.81 GB):  10%|█         | 6/58 [00:00<00:05,  8.85it/s]Capturing num tokens (num_tokens=4608 avail_mem=100.81 GB):  14%|█▍        | 8/58 [00:00<00:04, 10.13it/s]Capturing num tokens (num_tokens=4096 avail_mem=101.32 GB):  14%|█▍        | 8/58 [00:00<00:04, 10.13it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=100.66 GB):  14%|█▍        | 8/58 [00:00<00:04, 10.13it/s]Capturing num tokens (num_tokens=3840 avail_mem=100.66 GB):  17%|█▋        | 10/58 [00:01<00:04, 10.61it/s]Capturing num tokens (num_tokens=3584 avail_mem=101.32 GB):  17%|█▋        | 10/58 [00:01<00:04, 10.61it/s]Capturing num tokens (num_tokens=3328 avail_mem=100.89 GB):  17%|█▋        | 10/58 [00:01<00:04, 10.61it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=100.89 GB):  21%|██        | 12/58 [00:01<00:04, 11.44it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.72 GB):  21%|██        | 12/58 [00:01<00:04, 11.44it/s]Capturing num tokens (num_tokens=2816 avail_mem=101.32 GB):  21%|██        | 12/58 [00:01<00:04, 11.44it/s]Capturing num tokens (num_tokens=2816 avail_mem=101.32 GB):  24%|██▍       | 14/58 [00:01<00:03, 12.70it/s]Capturing num tokens (num_tokens=2560 avail_mem=100.79 GB):  24%|██▍       | 14/58 [00:01<00:03, 12.70it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=100.79 GB):  24%|██▍       | 14/58 [00:01<00:03, 12.70it/s]Capturing num tokens (num_tokens=2304 avail_mem=100.79 GB):  28%|██▊       | 16/58 [00:01<00:03, 13.33it/s]Capturing num tokens (num_tokens=2048 avail_mem=101.31 GB):  28%|██▊       | 16/58 [00:01<00:03, 13.33it/s]Capturing num tokens (num_tokens=1792 avail_mem=100.81 GB):  28%|██▊       | 16/58 [00:01<00:03, 13.33it/s]Capturing num tokens (num_tokens=1792 avail_mem=100.81 GB):  31%|███       | 18/58 [00:01<00:02, 13.68it/s]Capturing num tokens (num_tokens=1536 avail_mem=101.30 GB):  31%|███       | 18/58 [00:01<00:02, 13.68it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=100.83 GB):  31%|███       | 18/58 [00:01<00:02, 13.68it/s]Capturing num tokens (num_tokens=1280 avail_mem=100.83 GB):  34%|███▍      | 20/58 [00:01<00:02, 14.14it/s]Capturing num tokens (num_tokens=1024 avail_mem=100.92 GB):  34%|███▍      | 20/58 [00:01<00:02, 14.14it/s]Capturing num tokens (num_tokens=960 avail_mem=101.29 GB):  34%|███▍      | 20/58 [00:01<00:02, 14.14it/s] Capturing num tokens (num_tokens=960 avail_mem=101.29 GB):  38%|███▊      | 22/58 [00:01<00:02, 15.03it/s]Capturing num tokens (num_tokens=896 avail_mem=100.86 GB):  38%|███▊      | 22/58 [00:01<00:02, 15.03it/s]

    Capturing num tokens (num_tokens=832 avail_mem=101.28 GB):  38%|███▊      | 22/58 [00:01<00:02, 15.03it/s]Capturing num tokens (num_tokens=832 avail_mem=101.28 GB):  41%|████▏     | 24/58 [00:01<00:02, 15.29it/s]Capturing num tokens (num_tokens=768 avail_mem=100.88 GB):  41%|████▏     | 24/58 [00:01<00:02, 15.29it/s]Capturing num tokens (num_tokens=704 avail_mem=101.28 GB):  41%|████▏     | 24/58 [00:02<00:02, 15.29it/s]Capturing num tokens (num_tokens=704 avail_mem=101.28 GB):  45%|████▍     | 26/58 [00:02<00:02, 15.59it/s]Capturing num tokens (num_tokens=640 avail_mem=100.91 GB):  45%|████▍     | 26/58 [00:02<00:02, 15.59it/s]

    Capturing num tokens (num_tokens=576 avail_mem=101.27 GB):  45%|████▍     | 26/58 [00:02<00:02, 15.59it/s]Capturing num tokens (num_tokens=576 avail_mem=101.27 GB):  48%|████▊     | 28/58 [00:02<00:01, 16.60it/s]Capturing num tokens (num_tokens=512 avail_mem=101.01 GB):  48%|████▊     | 28/58 [00:02<00:01, 16.60it/s]Capturing num tokens (num_tokens=480 avail_mem=100.98 GB):  48%|████▊     | 28/58 [00:02<00:01, 16.60it/s]Capturing num tokens (num_tokens=480 avail_mem=100.98 GB):  52%|█████▏    | 30/58 [00:02<00:01, 17.36it/s]Capturing num tokens (num_tokens=448 avail_mem=101.28 GB):  52%|█████▏    | 30/58 [00:02<00:01, 17.36it/s]

    Capturing num tokens (num_tokens=416 avail_mem=101.01 GB):  52%|█████▏    | 30/58 [00:02<00:01, 17.36it/s]Capturing num tokens (num_tokens=416 avail_mem=101.01 GB):  55%|█████▌    | 32/58 [00:02<00:01, 17.96it/s]Capturing num tokens (num_tokens=384 avail_mem=101.27 GB):  55%|█████▌    | 32/58 [00:02<00:01, 17.96it/s]Capturing num tokens (num_tokens=352 avail_mem=101.27 GB):  55%|█████▌    | 32/58 [00:02<00:01, 17.96it/s]Capturing num tokens (num_tokens=352 avail_mem=101.27 GB):  59%|█████▊    | 34/58 [00:02<00:01, 18.29it/s]Capturing num tokens (num_tokens=320 avail_mem=101.05 GB):  59%|█████▊    | 34/58 [00:02<00:01, 18.29it/s]Capturing num tokens (num_tokens=288 avail_mem=101.24 GB):  59%|█████▊    | 34/58 [00:02<00:01, 18.29it/s]

    Capturing num tokens (num_tokens=256 avail_mem=101.26 GB):  59%|█████▊    | 34/58 [00:02<00:01, 18.29it/s]Capturing num tokens (num_tokens=256 avail_mem=101.26 GB):  64%|██████▍   | 37/58 [00:02<00:01, 20.20it/s]Capturing num tokens (num_tokens=240 avail_mem=101.25 GB):  64%|██████▍   | 37/58 [00:02<00:01, 20.20it/s]Capturing num tokens (num_tokens=224 avail_mem=101.12 GB):  64%|██████▍   | 37/58 [00:02<00:01, 20.20it/s]Capturing num tokens (num_tokens=208 avail_mem=101.22 GB):  64%|██████▍   | 37/58 [00:02<00:01, 20.20it/s]Capturing num tokens (num_tokens=208 avail_mem=101.22 GB):  69%|██████▉   | 40/58 [00:02<00:00, 22.28it/s]Capturing num tokens (num_tokens=192 avail_mem=101.23 GB):  69%|██████▉   | 40/58 [00:02<00:00, 22.28it/s]Capturing num tokens (num_tokens=176 avail_mem=101.23 GB):  69%|██████▉   | 40/58 [00:02<00:00, 22.28it/s]

    Capturing num tokens (num_tokens=160 avail_mem=101.22 GB):  69%|██████▉   | 40/58 [00:02<00:00, 22.28it/s]Capturing num tokens (num_tokens=160 avail_mem=101.22 GB):  74%|███████▍  | 43/58 [00:02<00:00, 23.08it/s]Capturing num tokens (num_tokens=144 avail_mem=101.12 GB):  74%|███████▍  | 43/58 [00:02<00:00, 23.08it/s]Capturing num tokens (num_tokens=128 avail_mem=101.12 GB):  74%|███████▍  | 43/58 [00:02<00:00, 23.08it/s]Capturing num tokens (num_tokens=112 avail_mem=101.12 GB):  74%|███████▍  | 43/58 [00:02<00:00, 23.08it/s]Capturing num tokens (num_tokens=112 avail_mem=101.12 GB):  79%|███████▉  | 46/58 [00:02<00:00, 24.75it/s]Capturing num tokens (num_tokens=96 avail_mem=101.19 GB):  79%|███████▉  | 46/58 [00:02<00:00, 24.75it/s] Capturing num tokens (num_tokens=80 avail_mem=101.19 GB):  79%|███████▉  | 46/58 [00:02<00:00, 24.75it/s]

    Capturing num tokens (num_tokens=64 avail_mem=101.18 GB):  79%|███████▉  | 46/58 [00:03<00:00, 24.75it/s]Capturing num tokens (num_tokens=64 avail_mem=101.18 GB):  84%|████████▍ | 49/58 [00:03<00:00, 26.08it/s]Capturing num tokens (num_tokens=48 avail_mem=101.18 GB):  84%|████████▍ | 49/58 [00:03<00:00, 26.08it/s]Capturing num tokens (num_tokens=32 avail_mem=101.17 GB):  84%|████████▍ | 49/58 [00:03<00:00, 26.08it/s]Capturing num tokens (num_tokens=28 avail_mem=101.16 GB):  84%|████████▍ | 49/58 [00:03<00:00, 26.08it/s]Capturing num tokens (num_tokens=28 avail_mem=101.16 GB):  90%|████████▉ | 52/58 [00:03<00:00, 25.00it/s]Capturing num tokens (num_tokens=24 avail_mem=101.15 GB):  90%|████████▉ | 52/58 [00:03<00:00, 25.00it/s]Capturing num tokens (num_tokens=20 avail_mem=101.14 GB):  90%|████████▉ | 52/58 [00:03<00:00, 25.00it/s]

    Capturing num tokens (num_tokens=16 avail_mem=101.14 GB):  90%|████████▉ | 52/58 [00:03<00:00, 25.00it/s]Capturing num tokens (num_tokens=12 avail_mem=101.13 GB):  90%|████████▉ | 52/58 [00:03<00:00, 25.00it/s]Capturing num tokens (num_tokens=12 avail_mem=101.13 GB):  97%|█████████▋| 56/58 [00:03<00:00, 27.26it/s]Capturing num tokens (num_tokens=8 avail_mem=101.11 GB):  97%|█████████▋| 56/58 [00:03<00:00, 27.26it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=100.12 GB):  97%|█████████▋| 56/58 [00:03<00:00, 27.26it/s]Capturing num tokens (num_tokens=4 avail_mem=100.12 GB): 100%|██████████| 58/58 [00:03<00:00, 15.95it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



```python
# Score the probability of different completions given a query
query = "The capital of France is"
items = ["Paris", "London", "Berlin"]

url = f"http://localhost:{port}/v1/score"
data = {
    "model": "qwen/qwen2.5-0.5b-instruct",
    "query": query,
    "items": items,
    "label_token_ids": [9454, 2753],  # e.g. "Yes" and "No" token ids
    "apply_softmax": True,  # Normalize probabilities to sum to 1
}

response = requests.post(url, json=data)
response_json = response.json()

# Display scores for each item
for item, scores in zip(items, response_json["scores"]):
    print_highlight(f"Item '{item}': probabilities = {[f'{s:.4f}' for s in scores]}")
```


<strong style='color: #00008B;'>Item 'Paris': probabilities = ['0.0268', '0.9732']</strong>



<strong style='color: #00008B;'>Item 'London': probabilities = ['0.0302', '0.9698']</strong>



<strong style='color: #00008B;'>Item 'Berlin': probabilities = ['0.0675', '0.9325']</strong>



```python
terminate_process(score_process)
```

## Classify (reward model)

SGLang Runtime also supports reward models. Here we use a reward model to classify the quality of pairwise generations.


```python
# Note that SGLang now treats embedding models and reward models as the same type of models.
# This will be updated in the future.

reward_process, port = launch_server_cmd("""
python3 -m sglang.launch_server --model-path Skywork/Skywork-Reward-Llama-3.1-8B-v0.2 --host 0.0.0.0 --is-embedding --log-level warning
""")

wait_for_server(f"http://localhost:{port}", process=reward_process)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-04-03 18:31:47.959 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:31:47] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:31:47.959 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:31:47] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:31:47.959 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:31:47] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:31:47.960 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:31:47] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:31:47.960 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:31:47] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:00<00:00,  3.24it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:01<00:00,  1.95it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.63it/s]Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.82it/s]


    2026-04-03 18:31:52,810 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 18:31:52] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:10,  3.34s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:10,  3.34s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:45,  1.89s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:45,  1.89s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:07,  1.22s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:07,  1.22s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<00:48,  1.12it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<00:48,  1.12it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:37,  1.42it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:37,  1.42it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:29,  1.75it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:29,  1.75it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:06<00:24,  2.09it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:06<00:24,  2.09it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:20,  2.43it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:20,  2.43it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:17,  2.82it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:17,  2.82it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:06<00:14,  3.24it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:06<00:14,  3.24it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:12,  3.62it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:12,  3.62it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:07<00:11,  4.04it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:07<00:11,  4.04it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:10,  4.43it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:10,  4.43it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:08,  4.90it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:08,  4.90it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:07,  5.41it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:07,  5.41it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:07<00:06,  6.11it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:07<00:06,  6.11it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:07<00:06,  6.69it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:07<00:06,  6.69it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:07<00:06,  6.69it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:07<00:04,  8.15it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:07<00:04,  8.15it/s]

    Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:08<00:04,  8.15it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:08<00:03,  9.95it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:08<00:03,  9.95it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:08<00:03,  9.95it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:08<00:02, 12.02it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:08<00:02, 12.02it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:08<00:02, 12.02it/s]

    Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:08<00:02, 12.02it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:08<00:01, 16.08it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:08<00:01, 16.08it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:08<00:01, 16.08it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:08<00:01, 16.08it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:08<00:01, 18.79it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:08<00:01, 18.79it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:08<00:01, 18.79it/s]

    Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:08<00:01, 18.79it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:08<00:01, 20.98it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:08<00:01, 20.98it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:08<00:01, 20.98it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:08<00:01, 20.98it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:08<00:00, 23.30it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:08<00:00, 23.30it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:08<00:00, 23.30it/s]

    Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:08<00:00, 23.30it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:08<00:00, 23.30it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:08<00:00, 26.68it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:08<00:00, 26.68it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:08<00:00, 26.68it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:08<00:00, 26.68it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:08<00:00, 26.68it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:08<00:00, 29.83it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:08<00:00, 29.83it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:08<00:00, 29.83it/s]

    Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:08<00:00, 29.83it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:08<00:00, 29.83it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:08<00:00, 32.32it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:08<00:00, 32.32it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:09<00:00, 32.32it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:09<00:00, 32.32it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:09<00:00, 32.32it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:09<00:00, 32.32it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:09<00:00, 35.60it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:09<00:00, 35.60it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:09<00:00, 35.60it/s]

    Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:09<00:00, 35.60it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:09<00:00, 35.60it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:09<00:00, 35.60it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:09<00:00, 38.27it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:09<00:00, 38.27it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  6.29it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=87.25 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=87.25 GB):   2%|▏         | 1/58 [00:00<00:39,  1.45it/s]Capturing num tokens (num_tokens=7680 avail_mem=87.22 GB):   2%|▏         | 1/58 [00:00<00:39,  1.45it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=87.22 GB):   3%|▎         | 2/58 [00:01<00:37,  1.51it/s]Capturing num tokens (num_tokens=7168 avail_mem=87.22 GB):   3%|▎         | 2/58 [00:01<00:37,  1.51it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=87.22 GB):   5%|▌         | 3/58 [00:01<00:33,  1.66it/s]Capturing num tokens (num_tokens=6656 avail_mem=87.23 GB):   5%|▌         | 3/58 [00:01<00:33,  1.66it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=87.23 GB):   7%|▋         | 4/58 [00:02<00:30,  1.75it/s]Capturing num tokens (num_tokens=6144 avail_mem=87.23 GB):   7%|▋         | 4/58 [00:02<00:30,  1.75it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=87.23 GB):   9%|▊         | 5/58 [00:02<00:30,  1.72it/s]Capturing num tokens (num_tokens=5632 avail_mem=84.44 GB):   9%|▊         | 5/58 [00:02<00:30,  1.72it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=84.44 GB):  10%|█         | 6/58 [00:03<00:27,  1.91it/s]Capturing num tokens (num_tokens=5120 avail_mem=84.45 GB):  10%|█         | 6/58 [00:03<00:27,  1.91it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=84.45 GB):  12%|█▏        | 7/58 [00:03<00:24,  2.11it/s]Capturing num tokens (num_tokens=4608 avail_mem=84.45 GB):  12%|█▏        | 7/58 [00:03<00:24,  2.11it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=84.45 GB):  14%|█▍        | 8/58 [00:04<00:21,  2.35it/s]Capturing num tokens (num_tokens=4096 avail_mem=84.45 GB):  14%|█▍        | 8/58 [00:04<00:21,  2.35it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=84.45 GB):  16%|█▌        | 9/58 [00:04<00:18,  2.60it/s]Capturing num tokens (num_tokens=3840 avail_mem=84.46 GB):  16%|█▌        | 9/58 [00:04<00:18,  2.60it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=84.46 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.85it/s]Capturing num tokens (num_tokens=3584 avail_mem=84.46 GB):  17%|█▋        | 10/58 [00:04<00:16,  2.85it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=84.46 GB):  19%|█▉        | 11/58 [00:04<00:15,  3.13it/s]Capturing num tokens (num_tokens=3328 avail_mem=84.46 GB):  19%|█▉        | 11/58 [00:04<00:15,  3.13it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=84.46 GB):  21%|██        | 12/58 [00:05<00:13,  3.40it/s]Capturing num tokens (num_tokens=3072 avail_mem=84.46 GB):  21%|██        | 12/58 [00:05<00:13,  3.40it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=84.46 GB):  22%|██▏       | 13/58 [00:05<00:12,  3.70it/s]Capturing num tokens (num_tokens=2816 avail_mem=84.46 GB):  22%|██▏       | 13/58 [00:05<00:12,  3.70it/s]Capturing num tokens (num_tokens=2816 avail_mem=84.46 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.06it/s]Capturing num tokens (num_tokens=2560 avail_mem=84.46 GB):  24%|██▍       | 14/58 [00:05<00:10,  4.06it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=84.46 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.47it/s]Capturing num tokens (num_tokens=2304 avail_mem=84.46 GB):  26%|██▌       | 15/58 [00:05<00:09,  4.47it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=84.46 GB):  28%|██▊       | 16/58 [00:05<00:09,  4.38it/s]Capturing num tokens (num_tokens=2048 avail_mem=84.45 GB):  28%|██▊       | 16/58 [00:05<00:09,  4.38it/s]Capturing num tokens (num_tokens=2048 avail_mem=84.45 GB):  29%|██▉       | 17/58 [00:06<00:08,  5.03it/s]Capturing num tokens (num_tokens=1792 avail_mem=84.45 GB):  29%|██▉       | 17/58 [00:06<00:08,  5.03it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=84.45 GB):  31%|███       | 18/58 [00:06<00:07,  5.71it/s]Capturing num tokens (num_tokens=1536 avail_mem=84.45 GB):  31%|███       | 18/58 [00:06<00:07,  5.71it/s]Capturing num tokens (num_tokens=1536 avail_mem=84.45 GB):  33%|███▎      | 19/58 [00:06<00:06,  6.45it/s]Capturing num tokens (num_tokens=1280 avail_mem=84.45 GB):  33%|███▎      | 19/58 [00:06<00:06,  6.45it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=84.45 GB):  33%|███▎      | 19/58 [00:06<00:06,  6.45it/s]Capturing num tokens (num_tokens=1024 avail_mem=84.45 GB):  36%|███▌      | 21/58 [00:06<00:04,  8.06it/s]Capturing num tokens (num_tokens=960 avail_mem=84.45 GB):  36%|███▌      | 21/58 [00:06<00:04,  8.06it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=84.45 GB):  38%|███▊      | 22/58 [00:06<00:04,  7.61it/s]Capturing num tokens (num_tokens=896 avail_mem=84.44 GB):  38%|███▊      | 22/58 [00:06<00:04,  7.61it/s]Capturing num tokens (num_tokens=832 avail_mem=84.44 GB):  38%|███▊      | 22/58 [00:06<00:04,  7.61it/s]Capturing num tokens (num_tokens=832 avail_mem=84.44 GB):  41%|████▏     | 24/58 [00:06<00:03,  8.92it/s]Capturing num tokens (num_tokens=768 avail_mem=84.44 GB):  41%|████▏     | 24/58 [00:06<00:03,  8.92it/s]

    Capturing num tokens (num_tokens=704 avail_mem=84.43 GB):  41%|████▏     | 24/58 [00:06<00:03,  8.92it/s]Capturing num tokens (num_tokens=704 avail_mem=84.43 GB):  45%|████▍     | 26/58 [00:06<00:03, 10.37it/s]Capturing num tokens (num_tokens=640 avail_mem=84.43 GB):  45%|████▍     | 26/58 [00:06<00:03, 10.37it/s]Capturing num tokens (num_tokens=576 avail_mem=84.43 GB):  45%|████▍     | 26/58 [00:07<00:03, 10.37it/s]Capturing num tokens (num_tokens=576 avail_mem=84.43 GB):  48%|████▊     | 28/58 [00:07<00:02, 11.65it/s]Capturing num tokens (num_tokens=512 avail_mem=84.42 GB):  48%|████▊     | 28/58 [00:07<00:02, 11.65it/s]

    Capturing num tokens (num_tokens=480 avail_mem=84.42 GB):  48%|████▊     | 28/58 [00:07<00:02, 11.65it/s]Capturing num tokens (num_tokens=480 avail_mem=84.42 GB):  52%|█████▏    | 30/58 [00:07<00:02, 10.70it/s]Capturing num tokens (num_tokens=448 avail_mem=83.41 GB):  52%|█████▏    | 30/58 [00:07<00:02, 10.70it/s]

    Capturing num tokens (num_tokens=416 avail_mem=83.41 GB):  52%|█████▏    | 30/58 [00:07<00:02, 10.70it/s]Capturing num tokens (num_tokens=416 avail_mem=83.41 GB):  55%|█████▌    | 32/58 [00:07<00:03,  7.73it/s]Capturing num tokens (num_tokens=384 avail_mem=83.40 GB):  55%|█████▌    | 32/58 [00:07<00:03,  7.73it/s]

    Capturing num tokens (num_tokens=384 avail_mem=83.40 GB):  57%|█████▋    | 33/58 [00:07<00:03,  7.06it/s]Capturing num tokens (num_tokens=352 avail_mem=83.40 GB):  57%|█████▋    | 33/58 [00:07<00:03,  7.06it/s]Capturing num tokens (num_tokens=352 avail_mem=83.40 GB):  59%|█████▊    | 34/58 [00:08<00:03,  6.54it/s]Capturing num tokens (num_tokens=320 avail_mem=83.40 GB):  59%|█████▊    | 34/58 [00:08<00:03,  6.54it/s]

    Capturing num tokens (num_tokens=320 avail_mem=83.40 GB):  60%|██████    | 35/58 [00:08<00:03,  6.26it/s]Capturing num tokens (num_tokens=288 avail_mem=83.39 GB):  60%|██████    | 35/58 [00:08<00:03,  6.26it/s]Capturing num tokens (num_tokens=288 avail_mem=83.39 GB):  62%|██████▏   | 36/58 [00:08<00:03,  5.89it/s]Capturing num tokens (num_tokens=256 avail_mem=83.39 GB):  62%|██████▏   | 36/58 [00:08<00:03,  5.89it/s]

    Capturing num tokens (num_tokens=256 avail_mem=83.39 GB):  64%|██████▍   | 37/58 [00:08<00:03,  5.66it/s]Capturing num tokens (num_tokens=240 avail_mem=83.38 GB):  64%|██████▍   | 37/58 [00:08<00:03,  5.66it/s]

    Capturing num tokens (num_tokens=240 avail_mem=83.38 GB):  66%|██████▌   | 38/58 [00:09<00:05,  3.83it/s]Capturing num tokens (num_tokens=224 avail_mem=83.38 GB):  66%|██████▌   | 38/58 [00:09<00:05,  3.83it/s]

    Capturing num tokens (num_tokens=224 avail_mem=83.38 GB):  67%|██████▋   | 39/58 [00:09<00:04,  4.00it/s]Capturing num tokens (num_tokens=208 avail_mem=83.37 GB):  67%|██████▋   | 39/58 [00:09<00:04,  4.00it/s]

    Capturing num tokens (num_tokens=208 avail_mem=83.37 GB):  69%|██████▉   | 40/58 [00:09<00:04,  3.89it/s]Capturing num tokens (num_tokens=192 avail_mem=83.37 GB):  69%|██████▉   | 40/58 [00:09<00:04,  3.89it/s]

    Capturing num tokens (num_tokens=192 avail_mem=83.37 GB):  71%|███████   | 41/58 [00:09<00:04,  4.14it/s]Capturing num tokens (num_tokens=176 avail_mem=83.36 GB):  71%|███████   | 41/58 [00:09<00:04,  4.14it/s]Capturing num tokens (num_tokens=176 avail_mem=83.36 GB):  72%|███████▏  | 42/58 [00:10<00:03,  4.44it/s]Capturing num tokens (num_tokens=160 avail_mem=83.36 GB):  72%|███████▏  | 42/58 [00:10<00:03,  4.44it/s]

    Capturing num tokens (num_tokens=160 avail_mem=83.36 GB):  74%|███████▍  | 43/58 [00:10<00:03,  4.77it/s]Capturing num tokens (num_tokens=144 avail_mem=83.35 GB):  74%|███████▍  | 43/58 [00:10<00:03,  4.77it/s]Capturing num tokens (num_tokens=144 avail_mem=83.35 GB):  76%|███████▌  | 44/58 [00:10<00:02,  5.03it/s]Capturing num tokens (num_tokens=128 avail_mem=83.35 GB):  76%|███████▌  | 44/58 [00:10<00:02,  5.03it/s]

    Capturing num tokens (num_tokens=128 avail_mem=83.35 GB):  78%|███████▊  | 45/58 [00:10<00:02,  5.20it/s]Capturing num tokens (num_tokens=112 avail_mem=83.36 GB):  78%|███████▊  | 45/58 [00:10<00:02,  5.20it/s]Capturing num tokens (num_tokens=112 avail_mem=83.36 GB):  79%|███████▉  | 46/58 [00:10<00:02,  5.38it/s]Capturing num tokens (num_tokens=96 avail_mem=83.36 GB):  79%|███████▉  | 46/58 [00:10<00:02,  5.38it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=83.36 GB):  81%|████████  | 47/58 [00:10<00:01,  5.51it/s]Capturing num tokens (num_tokens=80 avail_mem=83.35 GB):  81%|████████  | 47/58 [00:10<00:01,  5.51it/s]Capturing num tokens (num_tokens=80 avail_mem=83.35 GB):  83%|████████▎ | 48/58 [00:11<00:01,  5.58it/s]Capturing num tokens (num_tokens=64 avail_mem=83.35 GB):  83%|████████▎ | 48/58 [00:11<00:01,  5.58it/s]

    Capturing num tokens (num_tokens=64 avail_mem=83.35 GB):  84%|████████▍ | 49/58 [00:11<00:01,  5.58it/s]Capturing num tokens (num_tokens=48 avail_mem=83.35 GB):  84%|████████▍ | 49/58 [00:11<00:01,  5.58it/s]

    Capturing num tokens (num_tokens=48 avail_mem=83.35 GB):  86%|████████▌ | 50/58 [00:11<00:01,  5.22it/s]Capturing num tokens (num_tokens=32 avail_mem=83.34 GB):  86%|████████▌ | 50/58 [00:11<00:01,  5.22it/s]

    Capturing num tokens (num_tokens=32 avail_mem=83.34 GB):  88%|████████▊ | 51/58 [00:11<00:01,  4.32it/s]Capturing num tokens (num_tokens=28 avail_mem=83.34 GB):  88%|████████▊ | 51/58 [00:11<00:01,  4.32it/s]Capturing num tokens (num_tokens=28 avail_mem=83.34 GB):  90%|████████▉ | 52/58 [00:11<00:01,  4.69it/s]Capturing num tokens (num_tokens=24 avail_mem=83.33 GB):  90%|████████▉ | 52/58 [00:11<00:01,  4.69it/s]

    Capturing num tokens (num_tokens=24 avail_mem=83.33 GB):  91%|█████████▏| 53/58 [00:12<00:00,  5.04it/s]Capturing num tokens (num_tokens=20 avail_mem=83.33 GB):  91%|█████████▏| 53/58 [00:12<00:00,  5.04it/s]Capturing num tokens (num_tokens=20 avail_mem=83.33 GB):  93%|█████████▎| 54/58 [00:12<00:00,  5.40it/s]Capturing num tokens (num_tokens=16 avail_mem=83.32 GB):  93%|█████████▎| 54/58 [00:12<00:00,  5.40it/s]

    Capturing num tokens (num_tokens=16 avail_mem=83.32 GB):  95%|█████████▍| 55/58 [00:12<00:00,  5.63it/s]Capturing num tokens (num_tokens=12 avail_mem=83.32 GB):  95%|█████████▍| 55/58 [00:12<00:00,  5.63it/s]Capturing num tokens (num_tokens=12 avail_mem=83.32 GB):  97%|█████████▋| 56/58 [00:12<00:00,  5.99it/s]Capturing num tokens (num_tokens=8 avail_mem=83.32 GB):  97%|█████████▋| 56/58 [00:12<00:00,  5.99it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=83.32 GB):  98%|█████████▊| 57/58 [00:12<00:00,  5.57it/s]Capturing num tokens (num_tokens=4 avail_mem=83.31 GB):  98%|█████████▊| 57/58 [00:12<00:00,  5.57it/s]Capturing num tokens (num_tokens=4 avail_mem=83.31 GB): 100%|██████████| 58/58 [00:12<00:00,  6.10it/s]Capturing num tokens (num_tokens=4 avail_mem=83.31 GB): 100%|██████████| 58/58 [00:12<00:00,  4.48it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



```python
from transformers import AutoTokenizer

PROMPT = (
    "What is the range of the numeric output of a sigmoid node in a neural network?"
)

RESPONSE1 = "The output of a sigmoid node is bounded between -1 and 1."
RESPONSE2 = "The output of a sigmoid node is bounded between 0 and 1."

CONVS = [
    [{"role": "user", "content": PROMPT}, {"role": "assistant", "content": RESPONSE1}],
    [{"role": "user", "content": PROMPT}, {"role": "assistant", "content": RESPONSE2}],
]

tokenizer = AutoTokenizer.from_pretrained("Skywork/Skywork-Reward-Llama-3.1-8B-v0.2")
prompts = tokenizer.apply_chat_template(CONVS, tokenize=False, return_dict=False)

url = f"http://localhost:{port}/classify"
data = {"model": "Skywork/Skywork-Reward-Llama-3.1-8B-v0.2", "text": prompts}

responses = requests.post(url, json=data).json()
for response in responses:
    print_highlight(f"reward: {response['embedding'][0]}")
```


<strong style='color: #00008B;'>reward: -24.125</strong>



<strong style='color: #00008B;'>reward: 1.21875</strong>



```python
terminate_process(reward_process)
```

## Capture expert selection distribution in MoE models

SGLang Runtime supports recording the number of times an expert is selected in a MoE model run for each expert in the model. This is useful when analyzing the throughput of the model and plan for optimization.

*Note: We only print out the first 10 lines of the csv below for better readability. Please adjust accordingly if you want to analyze the results more deeply.*


```python
expert_record_server_process, port = launch_server_cmd(
    "python3 -m sglang.launch_server --model-path Qwen/Qwen1.5-MoE-A2.7B --host 0.0.0.0 --expert-distribution-recorder-mode stat --log-level warning"
)

wait_for_server(f"http://localhost:{port}", process=expert_record_server_process)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-04-03 18:32:43.138 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:32:43] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:32:43.139 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:32:43] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:32:43.139 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:32:43] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:32:43.139 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:32:43] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:32:43.139 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:32:43] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/8 [00:00<?, ?it/s]

    Multi-thread loading shards:  12% Completed | 1/8 [00:00<00:05,  1.24it/s]

    Multi-thread loading shards:  25% Completed | 2/8 [00:01<00:05,  1.17it/s]

    Multi-thread loading shards:  38% Completed | 3/8 [00:02<00:04,  1.14it/s]

    Multi-thread loading shards:  50% Completed | 4/8 [00:03<00:03,  1.17it/s]

    Multi-thread loading shards:  62% Completed | 5/8 [00:04<00:02,  1.27it/s]

    Multi-thread loading shards:  75% Completed | 6/8 [00:04<00:01,  1.65it/s]

    Multi-thread loading shards:  88% Completed | 7/8 [00:04<00:00,  1.73it/s]

    Multi-thread loading shards: 100% Completed | 8/8 [00:05<00:00,  1.64it/s]Multi-thread loading shards: 100% Completed | 8/8 [00:05<00:00,  1.44it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)


    2026-04-03 18:32:54,250 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 18:32:54] Unexpected error during package walk: cutlass.cute.experimental


    [2026-04-03 18:32:54] Using default MoE kernel config. Performance might be sub-optimal! Config file not found at /actions-runner/_work/sglang/sglang/python/sglang/srt/layers/moe/fused_moe_triton/configs/triton_3_5_1/E=60,N=1408,device_name=NVIDIA_H200.json, you can create them with https://github.com/sgl-project/sglang/tree/main/benchmark/kernels/fused_moe_triton
    [2026-04-03 18:32:54] Using MoE kernel config with down_moe=False. Performance might be sub-optimal! Config file not found at /actions-runner/_work/sglang/sglang/python/sglang/srt/layers/moe/fused_moe_triton/configs/triton_3_5_1/E=60,N=1408,device_name=NVIDIA_H200_down.json, you can create them with https://github.com/sgl-project/sglang/tree/main/benchmark/kernels/fused_moe_triton



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



```python
response = requests.post(f"http://localhost:{port}/start_expert_distribution_record")
print_highlight(response)

url = f"http://localhost:{port}/generate"
data = {"text": "What is the capital of France?"}

response = requests.post(url, json=data)
print_highlight(response.json())

response = requests.post(f"http://localhost:{port}/stop_expert_distribution_record")
print_highlight(response)

response = requests.post(f"http://localhost:{port}/dump_expert_distribution_record")
print_highlight(response)
```


<strong style='color: #00008B;'><Response [200]></strong>



<strong style='color: #00008B;'>{'text': ' 開頭Of those years.\ndoes\nwas\nare\nbe\n答案: C', 'output_ids': [18137, 244, 233, 104172, 2124, 1846, 1635, 624, 27057, 198, 16123, 198, 546, 198, 1371, 198, 102349, 25, 356, 151643], 'meta_info': {'id': '36602fa04d7e42ca92907ba0e450d066', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 7, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 20, 'cached_tokens': 0, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 1.3385266130790114, 'response_sent_to_client_ts': 1775241179.4432611}}</strong>



<strong style='color: #00008B;'><Response [200]></strong>



<strong style='color: #00008B;'><Response [200]></strong>



```python
terminate_process(expert_record_server_process)
```

## Tokenize/Detokenize Example (Round Trip)

This example demonstrates how to use the /tokenize and /detokenize endpoints together. We first tokenize a string, then detokenize the resulting IDs to reconstruct the original text. This workflow is useful when you need to handle tokenization externally but still leverage the server for detokenization.


```python
tokenizer_free_server_process, port = launch_server_cmd("""
python3 -m sglang.launch_server --model-path qwen/qwen2.5-0.5b-instruct
""")

wait_for_server(f"http://localhost:{port}", process=tokenizer_free_server_process)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (
    [2026-04-03 18:33:07] server_args=ServerArgs(model_path='qwen/qwen2.5-0.5b-instruct', tokenizer_path='qwen/qwen2.5-0.5b-instruct', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=30691, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.907, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=907540213, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='info', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_mfu_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='qwen/qwen2.5-0.5b-instruct', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser=None, tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, experts_shared_outer_loras=None, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_max_trie_depth=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enforce_disable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, enable_hisparse=False, hisparse_config=None, enable_lmcache=False, kt_weight_path=None, kt_method='AMXINT4', kt_cpuinfer=None, kt_threadpool_count=2, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, gc_threshold=None, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, engine_info_bootstrap_port=6789, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    [2026-04-03 18:33:07] CI: patched _patch_mistral_regex to skip HF API calls


    [2026-04-03 18:33:09] Watchdog TokenizerManager initialized.
    [2026-04-03 18:33:09] Using default HuggingFace chat template with detected content format: string


    [2026-04-03 18:33:15] CI: patched _patch_mistral_regex to skip HF API calls


    [2026-04-03 18:33:15] CI: patched _patch_mistral_regex to skip HF API calls


    [2026-04-03 18:33:16] Watchdog DetokenizerManager initialized.


    [2026-04-03 18:33:17] Init torch distributed begin.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [2026-04-03 18:33:17] Init torch distributed ends. elapsed=0.45 s, mem usage=0.29 GB
    2026-04-03 18:33:17.877 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:33:17] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:33:17.877 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:33:17] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:33:17.877 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:33:17] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:33:17.877 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:33:17] Persistent cache disabled, using in-memory JIT cache
    2026-04-03 18:33:17.877 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-03 18:33:17] Persistent cache disabled, using in-memory JIT cache


    [2026-04-03 18:33:19] Load weight begin. avail mem=103.19 GB
    [2026-04-03 18:33:19] Found local HF snapshot for qwen/qwen2.5-0.5b-instruct at /root/.cache/huggingface/hub/models--qwen--qwen2.5-0.5b-instruct/snapshots/7ae557604adf67be50417f59c2c2f167def9a775; skipping download.


    [2026-04-03 18:33:19] No model.safetensors.index.json found in remote.
    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.86it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.85it/s]


    [2026-04-03 18:33:20] Load weight end. elapsed=0.58 s, type=Qwen2ForCausalLM, avail mem=102.21 GB, mem usage=0.99 GB.
    [2026-04-03 18:33:20] Using KV cache dtype: torch.bfloat16
    [2026-04-03 18:33:20] KV Cache is allocated. #tokens: 20480, K size: 0.12 GB, V size: 0.12 GB
    [2026-04-03 18:33:20] Memory pool end. avail mem=101.88 GB
    [2026-04-03 18:33:20] Capture piecewise CUDA graph begin. avail mem=101.78 GB
    [2026-04-03 18:33:20] Capture cuda graph num tokens [4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192]


    2026-04-03 18:33:20,455 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 18:33:20] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    [2026-04-03 18:33:23] Compiling a graph for dynamic shape takes 0.21 s


    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:41,  2.83s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:41,  2.83s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:08,  1.23s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:08,  1.23s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:03<01:08,  1.23s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:26,  2.03it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:26,  2.03it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:26,  2.03it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:03<00:26,  2.03it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:11,  4.28it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:11,  4.28it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:11,  4.28it/s]Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:11,  4.28it/s]

    Compiling num tokens (num_tokens=3584):  12%|█▏        | 7/58 [00:03<00:11,  4.28it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:05,  8.02it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:05,  8.02it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:05,  8.02it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:05,  8.02it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:05,  8.02it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:05,  8.02it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:03<00:03, 13.23it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:03<00:03, 13.23it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:03<00:03, 13.23it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:03<00:03, 13.23it/s]Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:03<00:03, 13.23it/s]

    Compiling num tokens (num_tokens=1024):  28%|██▊       | 16/58 [00:03<00:03, 13.23it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:03<00:01, 18.58it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:03<00:01, 18.58it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:03<00:01, 18.58it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:03<00:01, 18.58it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:03<00:01, 18.58it/s]Compiling num tokens (num_tokens=704):  36%|███▌      | 21/58 [00:03<00:01, 18.58it/s]Compiling num tokens (num_tokens=640):  36%|███▌      | 21/58 [00:03<00:01, 18.58it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 25.30it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 25.30it/s]

    Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 25.30it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 25.30it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 25.30it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 25.30it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:00, 27.98it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:00, 27.98it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:00, 27.98it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:00, 27.98it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:00, 27.98it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:00, 27.98it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:03<00:00, 31.99it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:03<00:00, 31.99it/s]

    Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:03<00:00, 31.99it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:03<00:00, 31.99it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:03<00:00, 31.99it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:03<00:00, 31.99it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:03<00:00, 35.68it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:03<00:00, 35.68it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:03<00:00, 35.68it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:04<00:00, 35.68it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:04<00:00, 35.68it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:04<00:00, 35.68it/s] 

    Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:04<00:00, 37.04it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:04<00:00, 37.04it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:04<00:00, 37.04it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:04<00:00, 37.04it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:04<00:00, 37.04it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:04<00:00, 37.04it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:04<00:00, 40.06it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:04<00:00, 40.06it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:04<00:00, 40.06it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:04<00:00, 40.06it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:04<00:00, 40.06it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:04<00:00, 40.06it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:04<00:00, 40.06it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 13.56it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=101.44 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=101.44 GB):   2%|▏         | 1/58 [00:00<00:05,  9.64it/s]Capturing num tokens (num_tokens=7680 avail_mem=101.41 GB):   2%|▏         | 1/58 [00:00<00:05,  9.64it/s]Capturing num tokens (num_tokens=7168 avail_mem=101.41 GB):   2%|▏         | 1/58 [00:00<00:05,  9.64it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=101.41 GB):   5%|▌         | 3/58 [00:00<00:05, 10.49it/s]Capturing num tokens (num_tokens=6656 avail_mem=101.40 GB):   5%|▌         | 3/58 [00:00<00:05, 10.49it/s]Capturing num tokens (num_tokens=6144 avail_mem=101.41 GB):   5%|▌         | 3/58 [00:00<00:05, 10.49it/s]Capturing num tokens (num_tokens=6144 avail_mem=101.41 GB):   9%|▊         | 5/58 [00:00<00:04, 11.65it/s]Capturing num tokens (num_tokens=5632 avail_mem=101.40 GB):   9%|▊         | 5/58 [00:00<00:04, 11.65it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=101.40 GB):   9%|▊         | 5/58 [00:00<00:04, 11.65it/s]Capturing num tokens (num_tokens=5120 avail_mem=101.40 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.12it/s]Capturing num tokens (num_tokens=4608 avail_mem=101.40 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.12it/s]Capturing num tokens (num_tokens=4096 avail_mem=101.40 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.12it/s]Capturing num tokens (num_tokens=4096 avail_mem=101.40 GB):  16%|█▌        | 9/58 [00:00<00:03, 14.99it/s]Capturing num tokens (num_tokens=3840 avail_mem=101.39 GB):  16%|█▌        | 9/58 [00:00<00:03, 14.99it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=101.39 GB):  16%|█▌        | 9/58 [00:00<00:03, 14.99it/s]Capturing num tokens (num_tokens=3328 avail_mem=101.39 GB):  16%|█▌        | 9/58 [00:00<00:03, 14.99it/s]Capturing num tokens (num_tokens=3328 avail_mem=101.39 GB):  21%|██        | 12/58 [00:00<00:02, 17.36it/s]Capturing num tokens (num_tokens=3072 avail_mem=101.38 GB):  21%|██        | 12/58 [00:00<00:02, 17.36it/s]Capturing num tokens (num_tokens=2816 avail_mem=101.38 GB):  21%|██        | 12/58 [00:00<00:02, 17.36it/s]Capturing num tokens (num_tokens=2560 avail_mem=101.38 GB):  21%|██        | 12/58 [00:00<00:02, 17.36it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=101.38 GB):  26%|██▌       | 15/58 [00:00<00:02, 19.44it/s]Capturing num tokens (num_tokens=2304 avail_mem=101.37 GB):  26%|██▌       | 15/58 [00:00<00:02, 19.44it/s]Capturing num tokens (num_tokens=2048 avail_mem=101.37 GB):  26%|██▌       | 15/58 [00:00<00:02, 19.44it/s]Capturing num tokens (num_tokens=1792 avail_mem=101.37 GB):  26%|██▌       | 15/58 [00:01<00:02, 19.44it/s]Capturing num tokens (num_tokens=1792 avail_mem=101.37 GB):  31%|███       | 18/58 [00:01<00:01, 21.73it/s]Capturing num tokens (num_tokens=1536 avail_mem=101.36 GB):  31%|███       | 18/58 [00:01<00:01, 21.73it/s]Capturing num tokens (num_tokens=1280 avail_mem=101.36 GB):  31%|███       | 18/58 [00:01<00:01, 21.73it/s]Capturing num tokens (num_tokens=1024 avail_mem=101.34 GB):  31%|███       | 18/58 [00:01<00:01, 21.73it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=101.34 GB):  36%|███▌      | 21/58 [00:01<00:01, 22.94it/s]Capturing num tokens (num_tokens=960 avail_mem=101.33 GB):  36%|███▌      | 21/58 [00:01<00:01, 22.94it/s] Capturing num tokens (num_tokens=896 avail_mem=101.33 GB):  36%|███▌      | 21/58 [00:01<00:01, 22.94it/s]Capturing num tokens (num_tokens=832 avail_mem=101.33 GB):  36%|███▌      | 21/58 [00:01<00:01, 22.94it/s]Capturing num tokens (num_tokens=768 avail_mem=101.32 GB):  36%|███▌      | 21/58 [00:01<00:01, 22.94it/s]Capturing num tokens (num_tokens=768 avail_mem=101.32 GB):  43%|████▎     | 25/58 [00:01<00:01, 25.95it/s]Capturing num tokens (num_tokens=704 avail_mem=101.32 GB):  43%|████▎     | 25/58 [00:01<00:01, 25.95it/s]Capturing num tokens (num_tokens=640 avail_mem=101.32 GB):  43%|████▎     | 25/58 [00:01<00:01, 25.95it/s]Capturing num tokens (num_tokens=576 avail_mem=101.32 GB):  43%|████▎     | 25/58 [00:01<00:01, 25.95it/s]

    Capturing num tokens (num_tokens=512 avail_mem=101.30 GB):  43%|████▎     | 25/58 [00:01<00:01, 25.95it/s]Capturing num tokens (num_tokens=512 avail_mem=101.30 GB):  50%|█████     | 29/58 [00:01<00:01, 27.57it/s]Capturing num tokens (num_tokens=480 avail_mem=101.32 GB):  50%|█████     | 29/58 [00:01<00:01, 27.57it/s]Capturing num tokens (num_tokens=448 avail_mem=101.32 GB):  50%|█████     | 29/58 [00:01<00:01, 27.57it/s]

    Capturing num tokens (num_tokens=416 avail_mem=101.32 GB):  50%|█████     | 29/58 [00:01<00:01, 27.57it/s]Capturing num tokens (num_tokens=416 avail_mem=101.32 GB):  55%|█████▌    | 32/58 [00:01<00:01, 16.71it/s]Capturing num tokens (num_tokens=384 avail_mem=101.31 GB):  55%|█████▌    | 32/58 [00:01<00:01, 16.71it/s]

    Capturing num tokens (num_tokens=352 avail_mem=101.31 GB):  55%|█████▌    | 32/58 [00:01<00:01, 16.71it/s]Capturing num tokens (num_tokens=320 avail_mem=101.30 GB):  55%|█████▌    | 32/58 [00:02<00:01, 16.71it/s]

    Capturing num tokens (num_tokens=320 avail_mem=101.30 GB):  60%|██████    | 35/58 [00:02<00:01, 12.60it/s]Capturing num tokens (num_tokens=288 avail_mem=101.30 GB):  60%|██████    | 35/58 [00:02<00:01, 12.60it/s]Capturing num tokens (num_tokens=256 avail_mem=101.30 GB):  60%|██████    | 35/58 [00:02<00:01, 12.60it/s]

    Capturing num tokens (num_tokens=256 avail_mem=101.30 GB):  64%|██████▍   | 37/58 [00:02<00:01, 11.26it/s]Capturing num tokens (num_tokens=240 avail_mem=101.30 GB):  64%|██████▍   | 37/58 [00:02<00:01, 11.26it/s]Capturing num tokens (num_tokens=224 avail_mem=101.29 GB):  64%|██████▍   | 37/58 [00:02<00:01, 11.26it/s]

    Capturing num tokens (num_tokens=224 avail_mem=101.29 GB):  67%|██████▋   | 39/58 [00:02<00:01, 10.77it/s]Capturing num tokens (num_tokens=208 avail_mem=101.29 GB):  67%|██████▋   | 39/58 [00:02<00:01, 10.77it/s]Capturing num tokens (num_tokens=192 avail_mem=101.29 GB):  67%|██████▋   | 39/58 [00:02<00:01, 10.77it/s]

    Capturing num tokens (num_tokens=192 avail_mem=101.29 GB):  71%|███████   | 41/58 [00:02<00:01,  9.93it/s]Capturing num tokens (num_tokens=176 avail_mem=101.28 GB):  71%|███████   | 41/58 [00:02<00:01,  9.93it/s]Capturing num tokens (num_tokens=160 avail_mem=101.28 GB):  71%|███████   | 41/58 [00:02<00:01,  9.93it/s]

    Capturing num tokens (num_tokens=160 avail_mem=101.28 GB):  74%|███████▍  | 43/58 [00:03<00:01,  9.35it/s]Capturing num tokens (num_tokens=144 avail_mem=101.27 GB):  74%|███████▍  | 43/58 [00:03<00:01,  9.35it/s]Capturing num tokens (num_tokens=128 avail_mem=101.27 GB):  74%|███████▍  | 43/58 [00:03<00:01,  9.35it/s]

    Capturing num tokens (num_tokens=128 avail_mem=101.27 GB):  78%|███████▊  | 45/58 [00:03<00:01,  8.64it/s]Capturing num tokens (num_tokens=112 avail_mem=101.27 GB):  78%|███████▊  | 45/58 [00:03<00:01,  8.64it/s]Capturing num tokens (num_tokens=112 avail_mem=101.27 GB):  79%|███████▉  | 46/58 [00:03<00:01,  8.17it/s]Capturing num tokens (num_tokens=96 avail_mem=101.27 GB):  79%|███████▉  | 46/58 [00:03<00:01,  8.17it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=101.27 GB):  81%|████████  | 47/58 [00:03<00:01,  8.15it/s]Capturing num tokens (num_tokens=80 avail_mem=101.26 GB):  81%|████████  | 47/58 [00:03<00:01,  8.15it/s]Capturing num tokens (num_tokens=64 avail_mem=101.26 GB):  81%|████████  | 47/58 [00:03<00:01,  8.15it/s]Capturing num tokens (num_tokens=48 avail_mem=101.26 GB):  81%|████████  | 47/58 [00:03<00:01,  8.15it/s]Capturing num tokens (num_tokens=48 avail_mem=101.26 GB):  86%|████████▌ | 50/58 [00:03<00:00, 10.50it/s]Capturing num tokens (num_tokens=32 avail_mem=101.25 GB):  86%|████████▌ | 50/58 [00:03<00:00, 10.50it/s]

    Capturing num tokens (num_tokens=28 avail_mem=101.25 GB):  86%|████████▌ | 50/58 [00:03<00:00, 10.50it/s]Capturing num tokens (num_tokens=28 avail_mem=101.25 GB):  90%|████████▉ | 52/58 [00:04<00:00,  9.67it/s]Capturing num tokens (num_tokens=24 avail_mem=101.25 GB):  90%|████████▉ | 52/58 [00:04<00:00,  9.67it/s]

    Capturing num tokens (num_tokens=24 avail_mem=101.25 GB):  91%|█████████▏| 53/58 [00:04<00:00,  9.28it/s]Capturing num tokens (num_tokens=20 avail_mem=101.24 GB):  91%|█████████▏| 53/58 [00:04<00:00,  9.28it/s]Capturing num tokens (num_tokens=20 avail_mem=101.24 GB):  93%|█████████▎| 54/58 [00:04<00:00,  9.06it/s]Capturing num tokens (num_tokens=16 avail_mem=101.24 GB):  93%|█████████▎| 54/58 [00:04<00:00,  9.06it/s]

    Capturing num tokens (num_tokens=16 avail_mem=101.24 GB):  95%|█████████▍| 55/58 [00:04<00:00,  8.76it/s]Capturing num tokens (num_tokens=12 avail_mem=101.24 GB):  95%|█████████▍| 55/58 [00:04<00:00,  8.76it/s]Capturing num tokens (num_tokens=12 avail_mem=101.24 GB):  97%|█████████▋| 56/58 [00:04<00:00,  8.63it/s]Capturing num tokens (num_tokens=8 avail_mem=101.23 GB):  97%|█████████▋| 56/58 [00:04<00:00,  8.63it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=101.23 GB):  98%|█████████▊| 57/58 [00:04<00:00,  8.88it/s]Capturing num tokens (num_tokens=4 avail_mem=101.23 GB):  98%|█████████▊| 57/58 [00:04<00:00,  8.88it/s]Capturing num tokens (num_tokens=4 avail_mem=101.23 GB): 100%|██████████| 58/58 [00:04<00:00, 12.24it/s]
    [2026-04-03 18:33:30] Capture piecewise CUDA graph end. Time elapsed: 10.34 s. mem usage=0.55 GB. avail mem=101.23 GB.


    [2026-04-03 18:33:31] max_total_num_tokens=20480, chunked_prefill_size=8192, max_prefill_tokens=16384, max_running_requests=128, context_len=32768, available_gpu_mem=101.23 GB


    [2026-04-03 18:33:32] INFO:     Started server process [3922911]
    [2026-04-03 18:33:32] INFO:     Waiting for application startup.
    [2026-04-03 18:33:32] Using default chat sampling params from model generation config: {'repetition_penalty': 1.1, 'temperature': 0.7, 'top_k': 20, 'top_p': 0.8}
    [2026-04-03 18:33:32] INFO:     Application startup complete.
    [2026-04-03 18:33:32] INFO:     Uvicorn running on http://127.0.0.1:30691 (Press CTRL+C to quit)


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)
    [2026-04-03 18:33:33] INFO:     127.0.0.1:41978 - "GET /v1/models HTTP/1.1" 200 OK
    [2026-04-03 18:33:33] INFO:     127.0.0.1:41992 - "GET /model_info HTTP/1.1" 200 OK


    [2026-04-03 18:33:33] Prefill batch, #new-seq: 1, #new-token: 6, #cached-token: 0, token usage: 0.00, #running-req: 0, #queue-req: 0, cuda graph: True, input throughput (token/s): 0.00
    [2026-04-03 18:33:33] INFO:     127.0.0.1:41996 - "POST /generate HTTP/1.1" 200 OK
    [2026-04-03 18:33:33] The server is fired up and ready to roll!



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



```python
import requests
from sglang.utils import print_highlight

base_url = f"http://localhost:{port}"
tokenize_url = f"{base_url}/tokenize"
detokenize_url = f"{base_url}/detokenize"

model_name = "qwen/qwen2.5-0.5b-instruct"
input_text = "SGLang provides efficient tokenization endpoints."
print_highlight(f"Original Input Text:\n'{input_text}'")

# --- tokenize the input text ---
tokenize_payload = {
    "model": model_name,
    "prompt": input_text,
    "add_special_tokens": False,
}
try:
    tokenize_response = requests.post(tokenize_url, json=tokenize_payload)
    tokenize_response.raise_for_status()
    tokenization_result = tokenize_response.json()
    token_ids = tokenization_result.get("tokens")

    if not token_ids:
        raise ValueError("Tokenization returned empty tokens.")

    print_highlight(f"\nTokenized Output (IDs):\n{token_ids}")
    print_highlight(f"Token Count: {tokenization_result.get('count')}")
    print_highlight(f"Max Model Length: {tokenization_result.get('max_model_len')}")

    # --- detokenize the obtained token IDs ---
    detokenize_payload = {
        "model": model_name,
        "tokens": token_ids,
        "skip_special_tokens": True,
    }

    detokenize_response = requests.post(detokenize_url, json=detokenize_payload)
    detokenize_response.raise_for_status()
    detokenization_result = detokenize_response.json()
    reconstructed_text = detokenization_result.get("text")

    print_highlight(f"\nDetokenized Output (Text):\n'{reconstructed_text}'")

    if input_text == reconstructed_text:
        print_highlight(
            "\nRound Trip Successful: Original and reconstructed text match."
        )
    else:
        print_highlight(
            "\nRound Trip Mismatch: Original and reconstructed text differ."
        )

except requests.exceptions.RequestException as e:
    print_highlight(f"\nHTTP Request Error: {e}")
except Exception as e:
    print_highlight(f"\nAn error occurred: {e}")
```


<strong style='color: #00008B;'>Original Input Text:<br>'SGLang provides efficient tokenization endpoints.'</strong>


    [2026-04-03 18:33:38] INFO:     127.0.0.1:42002 - "POST /tokenize HTTP/1.1" 200 OK



<strong style='color: #00008B;'><br>Tokenized Output (IDs):<br>[50, 3825, 524, 5707, 11050, 3950, 2022, 36342, 13]</strong>



<strong style='color: #00008B;'>Token Count: 9</strong>



<strong style='color: #00008B;'>Max Model Length: 131072</strong>


    [2026-04-03 18:33:38] INFO:     127.0.0.1:42010 - "POST /detokenize HTTP/1.1" 200 OK



<strong style='color: #00008B;'><br>Detokenized Output (Text):<br>'SGLang provides efficient tokenization endpoints.'</strong>



<strong style='color: #00008B;'><br>Round Trip Successful: Original and reconstructed text match.</strong>



```python
terminate_process(tokenizer_free_server_process)
```
