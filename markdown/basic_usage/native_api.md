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
    2026-04-02 23:05:36.453 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:05:36] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:05:36.453 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:05:36] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:05:36.453 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:05:36] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:05:36.453 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:05:36] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:05:36.453 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:05:36] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.92it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.91it/s]


    2026-04-02 23:05:40,711 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 23:05:40] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:30,  2.64s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:30,  2.64s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:30,  2.64s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:30,  2.64s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:28,  1.90it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:28,  1.90it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:28,  1.90it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:28,  1.90it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:28,  1.90it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:28,  1.90it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:28,  1.90it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:08,  5.82it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:08,  5.82it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:08,  5.82it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:08,  5.82it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:08,  5.82it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:08,  5.82it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:08,  5.82it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:08,  5.82it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:08,  5.82it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 12.28it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 12.28it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:03, 12.28it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:03, 12.28it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:03<00:03, 12.28it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:03<00:03, 12.28it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:03<00:03, 12.28it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:03<00:03, 12.28it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 18.46it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 18.46it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 18.46it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 18.46it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 18.46it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.46it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.46it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.46it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 25.19it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 25.19it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 25.19it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 25.19it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 25.19it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 25.19it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 25.19it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 30.19it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 30.19it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 30.19it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 30.19it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 30.19it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 30.19it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 30.19it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.13it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.13it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.13it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.13it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.13it/s]

    Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.13it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.13it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 38.36it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 38.36it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 38.36it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 38.36it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 38.36it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 38.36it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 38.36it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 38.36it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 38.36it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.01it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=133.22 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=133.22 GB):   2%|▏         | 1/58 [00:00<00:07,  7.88it/s]Capturing num tokens (num_tokens=7680 avail_mem=133.17 GB):   2%|▏         | 1/58 [00:00<00:07,  7.88it/s]Capturing num tokens (num_tokens=7168 avail_mem=132.69 GB):   2%|▏         | 1/58 [00:00<00:07,  7.88it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=132.69 GB):   5%|▌         | 3/58 [00:00<00:03, 14.06it/s]Capturing num tokens (num_tokens=6656 avail_mem=132.53 GB):   5%|▌         | 3/58 [00:00<00:03, 14.06it/s]Capturing num tokens (num_tokens=6144 avail_mem=132.53 GB):   5%|▌         | 3/58 [00:00<00:03, 14.06it/s]Capturing num tokens (num_tokens=5632 avail_mem=132.53 GB):   5%|▌         | 3/58 [00:00<00:03, 14.06it/s]Capturing num tokens (num_tokens=5632 avail_mem=132.53 GB):  10%|█         | 6/58 [00:00<00:02, 19.47it/s]Capturing num tokens (num_tokens=5120 avail_mem=131.46 GB):  10%|█         | 6/58 [00:00<00:02, 19.47it/s]Capturing num tokens (num_tokens=4608 avail_mem=131.45 GB):  10%|█         | 6/58 [00:00<00:02, 19.47it/s]Capturing num tokens (num_tokens=4096 avail_mem=126.57 GB):  10%|█         | 6/58 [00:00<00:02, 19.47it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=126.57 GB):  16%|█▌        | 9/58 [00:00<00:02, 23.14it/s]Capturing num tokens (num_tokens=3840 avail_mem=117.75 GB):  16%|█▌        | 9/58 [00:00<00:02, 23.14it/s]Capturing num tokens (num_tokens=3584 avail_mem=116.82 GB):  16%|█▌        | 9/58 [00:00<00:02, 23.14it/s]Capturing num tokens (num_tokens=3328 avail_mem=116.82 GB):  16%|█▌        | 9/58 [00:00<00:02, 23.14it/s]Capturing num tokens (num_tokens=3072 avail_mem=116.81 GB):  16%|█▌        | 9/58 [00:00<00:02, 23.14it/s]Capturing num tokens (num_tokens=3072 avail_mem=116.81 GB):  22%|██▏       | 13/58 [00:00<00:01, 28.48it/s]Capturing num tokens (num_tokens=2816 avail_mem=116.81 GB):  22%|██▏       | 13/58 [00:00<00:01, 28.48it/s]Capturing num tokens (num_tokens=2560 avail_mem=116.81 GB):  22%|██▏       | 13/58 [00:00<00:01, 28.48it/s]Capturing num tokens (num_tokens=2304 avail_mem=116.81 GB):  22%|██▏       | 13/58 [00:00<00:01, 28.48it/s]Capturing num tokens (num_tokens=2048 avail_mem=116.80 GB):  22%|██▏       | 13/58 [00:00<00:01, 28.48it/s]Capturing num tokens (num_tokens=1792 avail_mem=116.80 GB):  22%|██▏       | 13/58 [00:00<00:01, 28.48it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=116.80 GB):  31%|███       | 18/58 [00:00<00:01, 33.92it/s]Capturing num tokens (num_tokens=1536 avail_mem=116.80 GB):  31%|███       | 18/58 [00:00<00:01, 33.92it/s]Capturing num tokens (num_tokens=1280 avail_mem=116.79 GB):  31%|███       | 18/58 [00:00<00:01, 33.92it/s]Capturing num tokens (num_tokens=1024 avail_mem=116.77 GB):  31%|███       | 18/58 [00:00<00:01, 33.92it/s]Capturing num tokens (num_tokens=960 avail_mem=116.78 GB):  31%|███       | 18/58 [00:00<00:01, 33.92it/s] Capturing num tokens (num_tokens=896 avail_mem=116.78 GB):  31%|███       | 18/58 [00:00<00:01, 33.92it/s]Capturing num tokens (num_tokens=896 avail_mem=116.78 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.51it/s]Capturing num tokens (num_tokens=832 avail_mem=116.78 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.51it/s]Capturing num tokens (num_tokens=768 avail_mem=116.77 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.51it/s]Capturing num tokens (num_tokens=704 avail_mem=116.77 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.51it/s]Capturing num tokens (num_tokens=640 avail_mem=116.77 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.51it/s]Capturing num tokens (num_tokens=576 avail_mem=116.77 GB):  40%|███▉      | 23/58 [00:00<00:00, 37.51it/s]

    Capturing num tokens (num_tokens=576 avail_mem=116.77 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.83it/s]Capturing num tokens (num_tokens=512 avail_mem=116.37 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.83it/s]Capturing num tokens (num_tokens=480 avail_mem=115.95 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.83it/s]Capturing num tokens (num_tokens=448 avail_mem=115.95 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.83it/s]Capturing num tokens (num_tokens=416 avail_mem=115.95 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.83it/s]Capturing num tokens (num_tokens=384 avail_mem=115.95 GB):  48%|████▊     | 28/58 [00:00<00:00, 39.83it/s]Capturing num tokens (num_tokens=384 avail_mem=115.95 GB):  57%|█████▋    | 33/58 [00:01<00:00, 41.12it/s]Capturing num tokens (num_tokens=352 avail_mem=115.94 GB):  57%|█████▋    | 33/58 [00:01<00:00, 41.12it/s]Capturing num tokens (num_tokens=320 avail_mem=115.94 GB):  57%|█████▋    | 33/58 [00:01<00:00, 41.12it/s]Capturing num tokens (num_tokens=288 avail_mem=115.94 GB):  57%|█████▋    | 33/58 [00:01<00:00, 41.12it/s]Capturing num tokens (num_tokens=256 avail_mem=115.93 GB):  57%|█████▋    | 33/58 [00:01<00:00, 41.12it/s]

    Capturing num tokens (num_tokens=240 avail_mem=115.93 GB):  57%|█████▋    | 33/58 [00:01<00:00, 41.12it/s]Capturing num tokens (num_tokens=240 avail_mem=115.93 GB):  66%|██████▌   | 38/58 [00:01<00:00, 42.23it/s]Capturing num tokens (num_tokens=224 avail_mem=115.93 GB):  66%|██████▌   | 38/58 [00:01<00:00, 42.23it/s]Capturing num tokens (num_tokens=208 avail_mem=115.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 42.23it/s]Capturing num tokens (num_tokens=192 avail_mem=115.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 42.23it/s]Capturing num tokens (num_tokens=176 avail_mem=115.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 42.23it/s]Capturing num tokens (num_tokens=160 avail_mem=115.92 GB):  66%|██████▌   | 38/58 [00:01<00:00, 42.23it/s]Capturing num tokens (num_tokens=160 avail_mem=115.92 GB):  74%|███████▍  | 43/58 [00:01<00:00, 43.24it/s]Capturing num tokens (num_tokens=144 avail_mem=115.91 GB):  74%|███████▍  | 43/58 [00:01<00:00, 43.24it/s]Capturing num tokens (num_tokens=128 avail_mem=115.91 GB):  74%|███████▍  | 43/58 [00:01<00:00, 43.24it/s]Capturing num tokens (num_tokens=112 avail_mem=115.91 GB):  74%|███████▍  | 43/58 [00:01<00:00, 43.24it/s]Capturing num tokens (num_tokens=96 avail_mem=115.90 GB):  74%|███████▍  | 43/58 [00:01<00:00, 43.24it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=115.90 GB):  74%|███████▍  | 43/58 [00:01<00:00, 43.24it/s]Capturing num tokens (num_tokens=80 avail_mem=115.90 GB):  83%|████████▎ | 48/58 [00:01<00:00, 38.28it/s]Capturing num tokens (num_tokens=64 avail_mem=115.65 GB):  83%|████████▎ | 48/58 [00:01<00:00, 38.28it/s]Capturing num tokens (num_tokens=48 avail_mem=115.64 GB):  83%|████████▎ | 48/58 [00:01<00:00, 38.28it/s]

    Capturing num tokens (num_tokens=32 avail_mem=115.64 GB):  83%|████████▎ | 48/58 [00:01<00:00, 38.28it/s]Capturing num tokens (num_tokens=28 avail_mem=115.63 GB):  83%|████████▎ | 48/58 [00:01<00:00, 38.28it/s]Capturing num tokens (num_tokens=28 avail_mem=115.63 GB):  90%|████████▉ | 52/58 [00:01<00:00, 25.99it/s]Capturing num tokens (num_tokens=24 avail_mem=115.63 GB):  90%|████████▉ | 52/58 [00:01<00:00, 25.99it/s]

    Capturing num tokens (num_tokens=20 avail_mem=115.63 GB):  90%|████████▉ | 52/58 [00:01<00:00, 25.99it/s]Capturing num tokens (num_tokens=16 avail_mem=115.63 GB):  90%|████████▉ | 52/58 [00:01<00:00, 25.99it/s]Capturing num tokens (num_tokens=12 avail_mem=115.62 GB):  90%|████████▉ | 52/58 [00:01<00:00, 25.99it/s]

    Capturing num tokens (num_tokens=12 avail_mem=115.62 GB):  97%|█████████▋| 56/58 [00:01<00:00, 20.94it/s]Capturing num tokens (num_tokens=8 avail_mem=115.62 GB):  97%|█████████▋| 56/58 [00:01<00:00, 20.94it/s] Capturing num tokens (num_tokens=4 avail_mem=115.62 GB):  97%|█████████▋| 56/58 [00:02<00:00, 20.94it/s]Capturing num tokens (num_tokens=4 avail_mem=115.62 GB): 100%|██████████| 58/58 [00:02<00:00, 27.36it/s]


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


<strong style='color: #00008B;'>{'text': ' Most of the people speak French, and as a result, standard French is a regulated and vocalized version of French. By contrast a variety of dialects exists throughout the rest of the country.\n\nOn the Ferris wheel you can watch the water. This 15-meter (50 foot) wheel-spinning attraction is tucked into a minor docking layer on Lake Constance near the town of Hof, about 95 km (60 miles) south of Frankfurt.\n\nProduced by Festival Braunschweig Festival, this interactive technology show involves the hands of people standing near the speaker. Smashing, singing, and ringing the playback', 'output_ids': [7496, 315, 279, 1251, 6468, 8585, 11, 323, 438, 264, 1102, 11, 5297, 8585, 374, 264, 34219, 323, 25407, 1506, 2319, 315, 8585, 13, 3216, 12872, 264, 8045, 315, 42279, 82, 6724, 6814, 279, 2732, 315, 279, 3146, 382, 1925, 279, 27942, 285, 13284, 498, 646, 3736, 279, 3015, 13, 1096, 220, 16, 20, 72501, 320, 20, 15, 4478, 8, 13284, 80906, 1229, 32364, 374, 60118, 1119, 264, 8922, 88210, 6193, 389, 11678, 1200, 1309, 3143, 279, 6290, 315, 71712, 11, 911, 220, 24, 20, 13136, 320, 21, 15, 8756, 8, 9806, 315, 55184, 382, 18510, 1998, 553, 17322, 25453, 80791, 896, 343, 17322, 11, 419, 20753, 5440, 1473, 17601, 279, 6078, 315, 1251, 11259, 3143, 279, 18601, 13, 4388, 19049, 11, 25083, 11, 323, 67043, 279, 31959], 'meta_info': {'id': '406dbd7387d64d0bacb7743e0a904025', 'finish_reason': {'type': 'length', 'length': 128}, 'prompt_tokens': 7, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 128, 'cached_tokens': 0, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 1.1081069819629192, 'response_sent_to_client_ts': 1775171154.9395468}}</strong>


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

    [2026-04-02 23:05:54] Endpoint '/get_model_info' is deprecated and will be removed in a future version. Please use '/model_info' instead.



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


<strong style='color: #00008B;'>{"model_path":"qwen/qwen2.5-0.5b-instruct","tokenizer_path":"qwen/qwen2.5-0.5b-instruct","tokenizer_mode":"auto","tokenizer_worker_num":1,"skip_tokenizer_init":false,"load_format":"auto","model_loader_extra_config":"{}","trust_remote_code":false,"context_length":null,"is_embedding":false,"enable_multimodal":null,"revision":null,"model_impl":"auto","host":"0.0.0.0","port":36109,"fastapi_root_path":"","grpc_mode":false,"skip_server_warmup":false,"warmups":null,"nccl_port":null,"checkpoint_engine_wait_weights_before_ready":false,"ssl_keyfile":null,"ssl_certfile":null,"ssl_ca_certs":null,"ssl_keyfile_password":null,"enable_ssl_refresh":false,"dtype":"auto","quantization":null,"quantization_param_path":null,"kv_cache_dtype":"auto","enable_fp32_lm_head":false,"modelopt_quant":null,"modelopt_checkpoint_restore_path":null,"modelopt_checkpoint_save_path":null,"modelopt_export_path":null,"quantize_and_serve":false,"rl_quant_profile":null,"mem_fraction_static":0.907,"max_running_requests":128,"max_queued_requests":null,"max_total_tokens":20480,"chunked_prefill_size":8192,"enable_dynamic_chunking":false,"max_prefill_tokens":16384,"prefill_max_requests":null,"schedule_policy":"fcfs","enable_priority_scheduling":false,"disable_priority_preemption":false,"default_priority_value":null,"abort_on_priority_when_disabled":false,"schedule_low_priority_values_first":false,"priority_scheduling_preemption_threshold":10,"schedule_conservativeness":1.0,"page_size":1,"swa_full_tokens_ratio":0.8,"disable_hybrid_swa_memory":false,"radix_eviction_policy":"lru","enable_prefill_delayer":false,"prefill_delayer_max_delay_passes":30,"prefill_delayer_token_usage_low_watermark":null,"prefill_delayer_forward_passes_buckets":null,"prefill_delayer_wait_seconds_buckets":null,"device":"cuda","tp_size":1,"pp_size":1,"pp_max_micro_batch_size":null,"pp_async_batch_depth":0,"stream_interval":1,"incremental_streaming_output":false,"enable_streaming_session":false,"random_seed":305889980,"constrained_json_whitespace_pattern":null,"constrained_json_disable_any_whitespace":false,"watchdog_timeout":300,"soft_watchdog_timeout":300,"dist_timeout":null,"download_dir":null,"model_checksum":null,"base_gpu_id":0,"gpu_id_step":1,"sleep_on_idle":false,"use_ray":false,"custom_sigquit_handler":null,"log_level":"warning","log_level_http":null,"log_requests":false,"log_requests_level":2,"log_requests_format":"text","log_requests_target":null,"uvicorn_access_log_exclude_prefixes":[],"crash_dump_folder":null,"show_time_cost":false,"enable_metrics":false,"enable_mfu_metrics":false,"enable_metrics_for_all_schedulers":false,"tokenizer_metrics_custom_labels_header":"x-custom-labels","tokenizer_metrics_allowed_custom_labels":null,"extra_metric_labels":null,"bucket_time_to_first_token":null,"bucket_inter_token_latency":null,"bucket_e2e_request_latency":null,"collect_tokens_histogram":false,"prompt_tokens_buckets":null,"generation_tokens_buckets":null,"gc_warning_threshold_secs":0.0,"decode_log_interval":40,"enable_request_time_stats_logging":false,"kv_events_config":null,"enable_trace":false,"otlp_traces_endpoint":"localhost:4317","export_metrics_to_file":false,"export_metrics_to_file_dir":null,"api_key":null,"admin_api_key":null,"served_model_name":"qwen/qwen2.5-0.5b-instruct","weight_version":"default","chat_template":null,"hf_chat_template_name":null,"completion_template":null,"file_storage_path":"sglang_storage","enable_cache_report":false,"reasoning_parser":null,"tool_call_parser":null,"tool_server":null,"sampling_defaults":"model","dp_size":1,"load_balance_method":"round_robin","attn_cp_size":1,"moe_dp_size":1,"dist_init_addr":null,"nnodes":1,"node_rank":0,"json_model_override_args":"{}","preferred_sampling_params":null,"enable_lora":null,"enable_lora_overlap_loading":null,"max_lora_rank":null,"lora_target_modules":null,"lora_paths":null,"max_loaded_loras":null,"max_loras_per_batch":8,"lora_eviction_policy":"lru","lora_backend":"csgmv","max_lora_chunk_size":16,"experts_shared_outer_loras":null,"attention_backend":"fa3","decode_attention_backend":null,"prefill_attention_backend":null,"sampling_backend":"flashinfer","grammar_backend":"xgrammar","mm_attention_backend":null,"fp8_gemm_runner_backend":"auto","fp4_gemm_runner_backend":"auto","nsa_prefill_backend":null,"nsa_decode_backend":null,"disable_flashinfer_autotune":false,"mamba_backend":"triton","speculative_algorithm":null,"speculative_draft_model_path":null,"speculative_draft_model_revision":null,"speculative_draft_load_format":null,"speculative_num_steps":null,"speculative_eagle_topk":null,"speculative_num_draft_tokens":null,"speculative_accept_threshold_single":1.0,"speculative_accept_threshold_acc":1.0,"speculative_token_map":null,"speculative_attention_mode":"prefill","speculative_draft_attention_backend":null,"speculative_moe_runner_backend":"auto","speculative_moe_a2a_backend":null,"speculative_draft_model_quantization":null,"speculative_ngram_min_bfs_breadth":1,"speculative_ngram_max_bfs_breadth":10,"speculative_ngram_match_type":"BFS","speculative_ngram_max_trie_depth":18,"speculative_ngram_capacity":10000000,"enable_multi_layer_eagle":false,"ep_size":1,"moe_a2a_backend":"none","moe_runner_backend":"auto","flashinfer_mxfp4_moe_precision":"default","enable_flashinfer_allreduce_fusion":false,"enforce_disable_flashinfer_allreduce_fusion":false,"enable_aiter_allreduce_fusion":false,"deepep_mode":"auto","ep_num_redundant_experts":0,"ep_dispatch_algorithm":null,"init_expert_location":"trivial","enable_eplb":false,"eplb_algorithm":"auto","eplb_rebalance_num_iterations":1000,"eplb_rebalance_layers_per_chunk":null,"eplb_min_rebalancing_utilization_threshold":1.0,"expert_distribution_recorder_mode":null,"expert_distribution_recorder_buffer_size":1000,"enable_expert_distribution_metrics":false,"deepep_config":null,"moe_dense_tp_size":null,"elastic_ep_backend":null,"enable_elastic_expert_backup":false,"mooncake_ib_device":null,"max_mamba_cache_size":null,"mamba_ssm_dtype":null,"mamba_full_memory_ratio":0.9,"mamba_scheduler_strategy":"no_buffer","mamba_track_interval":256,"linear_attn_backend":"triton","linear_attn_decode_backend":null,"linear_attn_prefill_backend":null,"enable_hierarchical_cache":false,"hicache_ratio":2.0,"hicache_size":0,"hicache_write_policy":"write_through","hicache_io_backend":"kernel","hicache_mem_layout":"layer_first","hicache_storage_backend":null,"hicache_storage_prefetch_policy":"best_effort","hicache_storage_backend_extra_config":null,"enable_hisparse":false,"hisparse_config":null,"enable_lmcache":false,"kt_weight_path":null,"kt_method":"AMXINT4","kt_cpuinfer":null,"kt_threadpool_count":2,"kt_num_gpu_experts":null,"kt_max_deferred_experts_per_token":null,"dllm_algorithm":null,"dllm_algorithm_config":null,"enable_double_sparsity":false,"ds_channel_config_path":null,"ds_heavy_channel_num":32,"ds_heavy_token_num":256,"ds_heavy_channel_type":"qk","ds_sparse_decode_threshold":4096,"cpu_offload_gb":0,"offload_group_size":-1,"offload_num_in_group":1,"offload_prefetch_step":1,"offload_mode":"cpu","multi_item_scoring_delimiter":null,"disable_radix_cache":false,"cuda_graph_max_bs":4,"cuda_graph_bs":[1,2,4],"disable_cuda_graph":true,"disable_cuda_graph_padding":false,"enable_profile_cuda_graph":false,"enable_cudagraph_gc":false,"enable_layerwise_nvtx_marker":false,"enable_nccl_nvls":false,"enable_symm_mem":false,"disable_flashinfer_cutlass_moe_fp4_allgather":false,"enable_tokenizer_batch_encode":false,"disable_tokenizer_batch_decode":false,"disable_outlines_disk_cache":false,"disable_custom_all_reduce":false,"enable_mscclpp":false,"enable_torch_symm_mem":false,"pre_warm_nccl":false,"disable_overlap_schedule":false,"enable_mixed_chunk":false,"enable_dp_attention":false,"enable_dp_lm_head":false,"enable_two_batch_overlap":false,"enable_single_batch_overlap":false,"tbo_token_distribution_threshold":0.48,"enable_torch_compile":false,"disable_piecewise_cuda_graph":false,"enforce_piecewise_cuda_graph":false,"enable_torch_compile_debug_mode":false,"torch_compile_max_bs":32,"piecewise_cuda_graph_max_tokens":8192,"piecewise_cuda_graph_tokens":[4,8,12,16,20,24,28,32,48,64,80,96,112,128,144,160,176,192,208,224,240,256,288,320,352,384,416,448,480,512,576,640,704,768,832,896,960,1024,1280,1536,1792,2048,2304,2560,2816,3072,3328,3584,3840,4096,4608,5120,5632,6144,6656,7168,7680,8192],"piecewise_cuda_graph_compiler":"eager","torchao_config":"","enable_nan_detection":false,"enable_p2p_check":false,"triton_attention_reduce_in_fp32":false,"triton_attention_num_kv_splits":8,"triton_attention_split_tile_size":null,"num_continuous_decode_steps":1,"delete_ckpt_after_loading":false,"enable_memory_saver":false,"enable_weights_cpu_backup":false,"enable_draft_weights_cpu_backup":false,"allow_auto_truncate":false,"enable_custom_logit_processor":false,"flashinfer_mla_disable_ragged":false,"disable_shared_experts_fusion":false,"disable_chunked_prefix_cache":false,"disable_fast_image_processor":false,"keep_mm_feature_on_device":false,"enable_return_hidden_states":false,"enable_return_routed_experts":false,"scheduler_recv_interval":1,"numa_node":null,"enable_deterministic_inference":false,"rl_on_policy_target":null,"enable_attn_tp_input_scattered":false,"gc_threshold":null,"enable_nsa_prefill_context_parallel":false,"nsa_prefill_cp_mode":"round-robin-split","enable_fused_qk_norm_rope":false,"enable_precise_embedding_interpolation":false,"enable_fused_moe_sum_all_reduce":false,"enable_prefill_context_parallel":false,"prefill_cp_mode":"in-seq-split","enable_dynamic_batch_tokenizer":false,"dynamic_batch_tokenizer_batch_size":32,"dynamic_batch_tokenizer_batch_timeout":0.002,"debug_tensor_dump_output_folder":null,"debug_tensor_dump_layers":null,"debug_tensor_dump_input_file":null,"debug_tensor_dump_inject":false,"disaggregation_mode":"null","disaggregation_transfer_backend":"mooncake","disaggregation_bootstrap_port":8998,"disaggregation_ib_device":null,"disaggregation_decode_enable_offload_kvcache":false,"num_reserved_decode_tokens":512,"disaggregation_decode_polling_interval":1,"encoder_only":false,"language_only":false,"encoder_transfer_backend":"zmq_to_scheduler","encoder_urls":[],"enable_adaptive_dispatch_to_encoder":false,"custom_weight_loader":[],"weight_loader_disable_mmap":false,"remote_instance_weight_loader_seed_instance_ip":null,"remote_instance_weight_loader_seed_instance_service_port":null,"remote_instance_weight_loader_send_weights_group_ports":null,"remote_instance_weight_loader_backend":"nccl","remote_instance_weight_loader_start_seed_via_transfer_engine":false,"engine_info_bootstrap_port":6789,"modelexpress_config":null,"enable_pdmux":false,"pdmux_config_path":null,"sm_group_num":8,"enable_broadcast_mm_inputs_process":false,"enable_prefix_mm_cache":false,"mm_enable_dp_encoder":false,"mm_process_config":{},"limit_mm_data_per_request":null,"enable_mm_global_cache":false,"decrypted_config_file":null,"decrypted_draft_config_file":null,"forward_hooks":null,"status":"ready","max_total_num_tokens":20480,"max_req_input_len":20474,"internal_states":[{"model_path":"qwen/qwen2.5-0.5b-instruct","tokenizer_path":"qwen/qwen2.5-0.5b-instruct","tokenizer_mode":"auto","tokenizer_worker_num":1,"skip_tokenizer_init":false,"load_format":"auto","model_loader_extra_config":"{}","trust_remote_code":false,"context_length":null,"is_embedding":false,"enable_multimodal":null,"revision":null,"model_impl":"auto","host":"0.0.0.0","port":36109,"fastapi_root_path":"","grpc_mode":false,"skip_server_warmup":false,"warmups":null,"nccl_port":null,"checkpoint_engine_wait_weights_before_ready":false,"ssl_keyfile":null,"ssl_certfile":null,"ssl_ca_certs":null,"ssl_keyfile_password":null,"enable_ssl_refresh":false,"dtype":"auto","quantization":null,"quantization_param_path":null,"kv_cache_dtype":"auto","enable_fp32_lm_head":false,"modelopt_quant":null,"modelopt_checkpoint_restore_path":null,"modelopt_checkpoint_save_path":null,"modelopt_export_path":null,"quantize_and_serve":false,"rl_quant_profile":null,"mem_fraction_static":0.907,"max_running_requests":128,"max_queued_requests":null,"max_total_tokens":20480,"chunked_prefill_size":8192,"enable_dynamic_chunking":false,"max_prefill_tokens":16384,"prefill_max_requests":null,"schedule_policy":"fcfs","enable_priority_scheduling":false,"disable_priority_preemption":false,"default_priority_value":null,"abort_on_priority_when_disabled":false,"schedule_low_priority_values_first":false,"priority_scheduling_preemption_threshold":10,"schedule_conservativeness":1.0,"page_size":1,"swa_full_tokens_ratio":0.8,"disable_hybrid_swa_memory":false,"radix_eviction_policy":"lru","enable_prefill_delayer":false,"prefill_delayer_max_delay_passes":30,"prefill_delayer_token_usage_low_watermark":null,"prefill_delayer_forward_passes_buckets":null,"prefill_delayer_wait_seconds_buckets":null,"device":"cuda","tp_size":1,"pp_size":1,"pp_max_micro_batch_size":128,"pp_async_batch_depth":0,"stream_interval":1,"incremental_streaming_output":false,"enable_streaming_session":false,"random_seed":305889980,"constrained_json_whitespace_pattern":null,"constrained_json_disable_any_whitespace":false,"watchdog_timeout":300,"soft_watchdog_timeout":300,"dist_timeout":null,"download_dir":null,"model_checksum":null,"base_gpu_id":0,"gpu_id_step":1,"sleep_on_idle":false,"use_ray":false,"custom_sigquit_handler":null,"log_level":"warning","log_level_http":null,"log_requests":false,"log_requests_level":2,"log_requests_format":"text","log_requests_target":null,"uvicorn_access_log_exclude_prefixes":[],"crash_dump_folder":null,"show_time_cost":false,"enable_metrics":false,"enable_mfu_metrics":false,"enable_metrics_for_all_schedulers":false,"tokenizer_metrics_custom_labels_header":"x-custom-labels","tokenizer_metrics_allowed_custom_labels":null,"extra_metric_labels":null,"bucket_time_to_first_token":null,"bucket_inter_token_latency":null,"bucket_e2e_request_latency":null,"collect_tokens_histogram":false,"prompt_tokens_buckets":null,"generation_tokens_buckets":null,"gc_warning_threshold_secs":0.0,"decode_log_interval":40,"enable_request_time_stats_logging":false,"kv_events_config":null,"enable_trace":false,"otlp_traces_endpoint":"localhost:4317","export_metrics_to_file":false,"export_metrics_to_file_dir":null,"api_key":null,"admin_api_key":null,"served_model_name":"qwen/qwen2.5-0.5b-instruct","weight_version":"default","chat_template":null,"hf_chat_template_name":null,"completion_template":null,"file_storage_path":"sglang_storage","enable_cache_report":false,"reasoning_parser":null,"tool_call_parser":null,"tool_server":null,"sampling_defaults":"model","dp_size":1,"load_balance_method":"round_robin","attn_cp_size":1,"moe_dp_size":1,"dist_init_addr":null,"nnodes":1,"node_rank":0,"json_model_override_args":"{}","preferred_sampling_params":null,"enable_lora":null,"enable_lora_overlap_loading":null,"max_lora_rank":null,"lora_target_modules":null,"lora_paths":null,"max_loaded_loras":null,"max_loras_per_batch":8,"lora_eviction_policy":"lru","lora_backend":"csgmv","max_lora_chunk_size":16,"experts_shared_outer_loras":null,"attention_backend":"fa3","decode_attention_backend":"fa3","prefill_attention_backend":"fa3","sampling_backend":"flashinfer","grammar_backend":"xgrammar","mm_attention_backend":null,"fp8_gemm_runner_backend":"auto","fp4_gemm_runner_backend":"auto","nsa_prefill_backend":null,"nsa_decode_backend":null,"disable_flashinfer_autotune":false,"mamba_backend":"triton","speculative_algorithm":null,"speculative_draft_model_path":null,"speculative_draft_model_revision":null,"speculative_draft_load_format":null,"speculative_num_steps":null,"speculative_eagle_topk":null,"speculative_num_draft_tokens":null,"speculative_accept_threshold_single":1.0,"speculative_accept_threshold_acc":1.0,"speculative_token_map":null,"speculative_attention_mode":"prefill","speculative_draft_attention_backend":null,"speculative_moe_runner_backend":"auto","speculative_moe_a2a_backend":null,"speculative_draft_model_quantization":null,"speculative_ngram_min_bfs_breadth":1,"speculative_ngram_max_bfs_breadth":10,"speculative_ngram_match_type":"BFS","speculative_ngram_max_trie_depth":18,"speculative_ngram_capacity":10000000,"enable_multi_layer_eagle":false,"ep_size":1,"moe_a2a_backend":"none","moe_runner_backend":"auto","flashinfer_mxfp4_moe_precision":"default","enable_flashinfer_allreduce_fusion":false,"enforce_disable_flashinfer_allreduce_fusion":false,"enable_aiter_allreduce_fusion":false,"deepep_mode":"auto","ep_num_redundant_experts":0,"ep_dispatch_algorithm":null,"init_expert_location":"trivial","enable_eplb":false,"eplb_algorithm":"auto","eplb_rebalance_num_iterations":1000,"eplb_rebalance_layers_per_chunk":null,"eplb_min_rebalancing_utilization_threshold":1.0,"expert_distribution_recorder_mode":null,"expert_distribution_recorder_buffer_size":1000,"enable_expert_distribution_metrics":false,"deepep_config":null,"moe_dense_tp_size":null,"elastic_ep_backend":null,"enable_elastic_expert_backup":false,"mooncake_ib_device":null,"max_mamba_cache_size":null,"mamba_ssm_dtype":null,"mamba_full_memory_ratio":0.9,"mamba_scheduler_strategy":"no_buffer","mamba_track_interval":256,"linear_attn_backend":"triton","linear_attn_decode_backend":null,"linear_attn_prefill_backend":null,"enable_hierarchical_cache":false,"hicache_ratio":2.0,"hicache_size":0,"hicache_write_policy":"write_through","hicache_io_backend":"kernel","hicache_mem_layout":"layer_first","hicache_storage_backend":null,"hicache_storage_prefetch_policy":"best_effort","hicache_storage_backend_extra_config":null,"enable_hisparse":false,"hisparse_config":null,"enable_lmcache":false,"kt_weight_path":null,"kt_method":"AMXINT4","kt_cpuinfer":null,"kt_threadpool_count":2,"kt_num_gpu_experts":null,"kt_max_deferred_experts_per_token":null,"dllm_algorithm":null,"dllm_algorithm_config":null,"enable_double_sparsity":false,"ds_channel_config_path":null,"ds_heavy_channel_num":32,"ds_heavy_token_num":256,"ds_heavy_channel_type":"qk","ds_sparse_decode_threshold":4096,"cpu_offload_gb":0,"offload_group_size":-1,"offload_num_in_group":1,"offload_prefetch_step":1,"offload_mode":"cpu","multi_item_scoring_delimiter":null,"disable_radix_cache":false,"cuda_graph_max_bs":4,"cuda_graph_bs":[1,2,4],"disable_cuda_graph":true,"disable_cuda_graph_padding":false,"enable_profile_cuda_graph":false,"enable_cudagraph_gc":false,"enable_layerwise_nvtx_marker":false,"enable_nccl_nvls":false,"enable_symm_mem":false,"disable_flashinfer_cutlass_moe_fp4_allgather":false,"enable_tokenizer_batch_encode":false,"disable_tokenizer_batch_decode":false,"disable_outlines_disk_cache":false,"disable_custom_all_reduce":false,"enable_mscclpp":false,"enable_torch_symm_mem":false,"pre_warm_nccl":false,"disable_overlap_schedule":false,"enable_mixed_chunk":false,"enable_dp_attention":false,"enable_dp_lm_head":false,"enable_two_batch_overlap":false,"enable_single_batch_overlap":false,"tbo_token_distribution_threshold":0.48,"enable_torch_compile":false,"disable_piecewise_cuda_graph":false,"enforce_piecewise_cuda_graph":false,"enable_torch_compile_debug_mode":false,"torch_compile_max_bs":32,"piecewise_cuda_graph_max_tokens":8192,"piecewise_cuda_graph_tokens":[4,8,12,16,20,24,28,32,48,64,80,96,112,128,144,160,176,192,208,224,240,256,288,320,352,384,416,448,480,512,576,640,704,768,832,896,960,1024,1280,1536,1792,2048,2304,2560,2816,3072,3328,3584,3840,4096,4608,5120,5632,6144,6656,7168,7680,8192],"piecewise_cuda_graph_compiler":"eager","torchao_config":"","enable_nan_detection":false,"enable_p2p_check":false,"triton_attention_reduce_in_fp32":false,"triton_attention_num_kv_splits":8,"triton_attention_split_tile_size":null,"num_continuous_decode_steps":1,"delete_ckpt_after_loading":false,"enable_memory_saver":false,"enable_weights_cpu_backup":false,"enable_draft_weights_cpu_backup":false,"allow_auto_truncate":false,"enable_custom_logit_processor":false,"flashinfer_mla_disable_ragged":false,"disable_shared_experts_fusion":false,"disable_chunked_prefix_cache":true,"disable_fast_image_processor":false,"keep_mm_feature_on_device":false,"enable_return_hidden_states":false,"enable_return_routed_experts":false,"scheduler_recv_interval":1,"numa_node":null,"enable_deterministic_inference":false,"rl_on_policy_target":null,"enable_attn_tp_input_scattered":false,"gc_threshold":null,"enable_nsa_prefill_context_parallel":false,"nsa_prefill_cp_mode":"round-robin-split","enable_fused_qk_norm_rope":false,"enable_precise_embedding_interpolation":false,"enable_fused_moe_sum_all_reduce":false,"enable_prefill_context_parallel":false,"prefill_cp_mode":"in-seq-split","enable_dynamic_batch_tokenizer":false,"dynamic_batch_tokenizer_batch_size":32,"dynamic_batch_tokenizer_batch_timeout":0.002,"debug_tensor_dump_output_folder":null,"debug_tensor_dump_layers":null,"debug_tensor_dump_input_file":null,"debug_tensor_dump_inject":false,"disaggregation_mode":"null","disaggregation_transfer_backend":"mooncake","disaggregation_bootstrap_port":8998,"disaggregation_ib_device":null,"disaggregation_decode_enable_offload_kvcache":false,"num_reserved_decode_tokens":512,"disaggregation_decode_polling_interval":1,"encoder_only":false,"language_only":false,"encoder_transfer_backend":"zmq_to_scheduler","encoder_urls":[],"enable_adaptive_dispatch_to_encoder":false,"custom_weight_loader":[],"weight_loader_disable_mmap":false,"remote_instance_weight_loader_seed_instance_ip":null,"remote_instance_weight_loader_seed_instance_service_port":null,"remote_instance_weight_loader_send_weights_group_ports":null,"remote_instance_weight_loader_backend":"nccl","remote_instance_weight_loader_start_seed_via_transfer_engine":false,"engine_info_bootstrap_port":6789,"modelexpress_config":null,"enable_pdmux":false,"pdmux_config_path":null,"sm_group_num":8,"enable_broadcast_mm_inputs_process":false,"enable_prefix_mm_cache":false,"mm_enable_dp_encoder":false,"mm_process_config":{},"limit_mm_data_per_request":null,"enable_mm_global_cache":false,"decrypted_config_file":null,"decrypted_draft_config_file":null,"forward_hooks":null,"use_mla_backend":false,"_mx_config_cache":{},"last_gen_throughput":127.02911392226856,"memory_usage":{"weight":0.98,"kvcache":0.23,"token_capacity":20480,"graph":0},"effective_max_running_requests_per_dp":128}],"version":"0.0.0.dev1+g34ddf135f"}</strong>


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

    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.86it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  6.85it/s]
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

    [2026-04-02 23:05:57] Failed to get weights iterator: qwen/qwen2.5-0.5b-instruct-wrong (repository not found).



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
    2026-04-02 23:06:14.951 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:06:14] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:06:14.951 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:06:14] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:06:14.951 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:06:14] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:06:14.951 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:06:14] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:06:14.951 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:06:14] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 1/2 [00:00<00:00,  1.34it/s]

    Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.32s/it]Multi-thread loading shards: 100% Completed | 2/2 [00:02<00:00,  1.23s/it]


    2026-04-02 23:06:21,652 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 23:06:21] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:53,  3.04s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:53,  3.04s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:13,  1.32s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:13,  1.32s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:03<01:13,  1.32s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:28,  1.89it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:28,  1.89it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:28,  1.89it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:03<00:28,  1.89it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:12,  3.99it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:12,  3.99it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:12,  3.99it/s]Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:12,  3.99it/s]

    Compiling num tokens (num_tokens=3584):  12%|█▏        | 7/58 [00:03<00:12,  3.99it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:06,  7.40it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:06,  7.40it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:06,  7.40it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:06,  7.40it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:06,  7.40it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:06,  7.40it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:03<00:03, 12.56it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:03<00:03, 12.56it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:03<00:03, 12.56it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:03<00:03, 12.56it/s]Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:03<00:03, 12.56it/s]Compiling num tokens (num_tokens=1024):  28%|██▊       | 16/58 [00:03<00:03, 12.56it/s]

    Compiling num tokens (num_tokens=960):  28%|██▊       | 16/58 [00:03<00:03, 12.56it/s] Compiling num tokens (num_tokens=896):  28%|██▊       | 16/58 [00:03<00:03, 12.56it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:03<00:01, 20.59it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:03<00:01, 20.59it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:03<00:01, 20.59it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:03<00:01, 20.59it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:03<00:01, 20.59it/s]Compiling num tokens (num_tokens=576):  40%|███▉      | 23/58 [00:03<00:01, 20.59it/s]Compiling num tokens (num_tokens=512):  40%|███▉      | 23/58 [00:03<00:01, 20.59it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:03<00:01, 27.23it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:03<00:01, 27.23it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:03<00:01, 27.23it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:03<00:01, 27.23it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:03<00:01, 27.23it/s]

    Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:03<00:01, 27.23it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:03<00:01, 27.23it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 33.30it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 33.30it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:03<00:00, 33.30it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:03<00:00, 33.30it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:04<00:00, 33.30it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:04<00:00, 33.30it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:04<00:00, 33.30it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:04<00:00, 37.89it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:04<00:00, 37.89it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:04<00:00, 37.89it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:04<00:00, 37.89it/s]

    Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:04<00:00, 37.89it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:04<00:00, 37.89it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:04<00:00, 37.89it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:04<00:00, 41.38it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:04<00:00, 41.38it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:04<00:00, 41.38it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:04<00:00, 41.38it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:04<00:00, 41.38it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:04<00:00, 41.38it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:04<00:00, 41.38it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:04<00:00, 45.32it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:04<00:00, 45.32it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:04<00:00, 45.32it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:04<00:00, 45.32it/s]

    Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:04<00:00, 45.32it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:04<00:00, 45.32it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 13.35it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=134.25 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=134.25 GB):   2%|▏         | 1/58 [00:00<00:06,  9.11it/s]Capturing num tokens (num_tokens=7680 avail_mem=134.22 GB):   2%|▏         | 1/58 [00:00<00:06,  9.11it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=134.22 GB):   2%|▏         | 1/58 [00:00<00:06,  9.11it/s]Capturing num tokens (num_tokens=7168 avail_mem=134.22 GB):   5%|▌         | 3/58 [00:00<00:05, 10.23it/s]Capturing num tokens (num_tokens=6656 avail_mem=134.22 GB):   5%|▌         | 3/58 [00:00<00:05, 10.23it/s]Capturing num tokens (num_tokens=6144 avail_mem=134.22 GB):   5%|▌         | 3/58 [00:00<00:05, 10.23it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=134.22 GB):   9%|▊         | 5/58 [00:00<00:04, 11.45it/s]Capturing num tokens (num_tokens=5632 avail_mem=134.22 GB):   9%|▊         | 5/58 [00:00<00:04, 11.45it/s]Capturing num tokens (num_tokens=5120 avail_mem=134.22 GB):   9%|▊         | 5/58 [00:00<00:04, 11.45it/s]Capturing num tokens (num_tokens=5120 avail_mem=134.22 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.05it/s]Capturing num tokens (num_tokens=4608 avail_mem=134.22 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.05it/s]Capturing num tokens (num_tokens=4096 avail_mem=134.22 GB):  12%|█▏        | 7/58 [00:00<00:03, 13.05it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=134.22 GB):  16%|█▌        | 9/58 [00:00<00:03, 14.96it/s]Capturing num tokens (num_tokens=3840 avail_mem=134.22 GB):  16%|█▌        | 9/58 [00:00<00:03, 14.96it/s]Capturing num tokens (num_tokens=3584 avail_mem=134.22 GB):  16%|█▌        | 9/58 [00:00<00:03, 14.96it/s]Capturing num tokens (num_tokens=3328 avail_mem=134.21 GB):  16%|█▌        | 9/58 [00:00<00:03, 14.96it/s]Capturing num tokens (num_tokens=3328 avail_mem=134.21 GB):  21%|██        | 12/58 [00:00<00:02, 17.85it/s]Capturing num tokens (num_tokens=3072 avail_mem=134.21 GB):  21%|██        | 12/58 [00:00<00:02, 17.85it/s]Capturing num tokens (num_tokens=2816 avail_mem=134.21 GB):  21%|██        | 12/58 [00:00<00:02, 17.85it/s]Capturing num tokens (num_tokens=2560 avail_mem=134.21 GB):  21%|██        | 12/58 [00:00<00:02, 17.85it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=134.21 GB):  26%|██▌       | 15/58 [00:00<00:02, 20.91it/s]Capturing num tokens (num_tokens=2304 avail_mem=134.20 GB):  26%|██▌       | 15/58 [00:00<00:02, 20.91it/s]Capturing num tokens (num_tokens=2048 avail_mem=134.19 GB):  26%|██▌       | 15/58 [00:00<00:02, 20.91it/s]Capturing num tokens (num_tokens=1792 avail_mem=134.19 GB):  26%|██▌       | 15/58 [00:00<00:02, 20.91it/s]Capturing num tokens (num_tokens=1536 avail_mem=134.19 GB):  26%|██▌       | 15/58 [00:01<00:02, 20.91it/s]Capturing num tokens (num_tokens=1536 avail_mem=134.19 GB):  33%|███▎      | 19/58 [00:01<00:01, 25.04it/s]Capturing num tokens (num_tokens=1280 avail_mem=134.19 GB):  33%|███▎      | 19/58 [00:01<00:01, 25.04it/s]Capturing num tokens (num_tokens=1024 avail_mem=134.18 GB):  33%|███▎      | 19/58 [00:01<00:01, 25.04it/s]Capturing num tokens (num_tokens=960 avail_mem=134.15 GB):  33%|███▎      | 19/58 [00:01<00:01, 25.04it/s] Capturing num tokens (num_tokens=896 avail_mem=134.15 GB):  33%|███▎      | 19/58 [00:01<00:01, 25.04it/s]

    Capturing num tokens (num_tokens=896 avail_mem=134.15 GB):  40%|███▉      | 23/58 [00:01<00:01, 28.88it/s]Capturing num tokens (num_tokens=832 avail_mem=134.16 GB):  40%|███▉      | 23/58 [00:01<00:01, 28.88it/s]Capturing num tokens (num_tokens=768 avail_mem=134.17 GB):  40%|███▉      | 23/58 [00:01<00:01, 28.88it/s]Capturing num tokens (num_tokens=704 avail_mem=134.16 GB):  40%|███▉      | 23/58 [00:01<00:01, 28.88it/s]Capturing num tokens (num_tokens=640 avail_mem=134.16 GB):  40%|███▉      | 23/58 [00:01<00:01, 28.88it/s]Capturing num tokens (num_tokens=640 avail_mem=134.16 GB):  47%|████▋     | 27/58 [00:01<00:00, 31.55it/s]Capturing num tokens (num_tokens=576 avail_mem=134.16 GB):  47%|████▋     | 27/58 [00:01<00:00, 31.55it/s]Capturing num tokens (num_tokens=512 avail_mem=134.15 GB):  47%|████▋     | 27/58 [00:01<00:00, 31.55it/s]Capturing num tokens (num_tokens=480 avail_mem=134.15 GB):  47%|████▋     | 27/58 [00:01<00:00, 31.55it/s]Capturing num tokens (num_tokens=448 avail_mem=134.15 GB):  47%|████▋     | 27/58 [00:01<00:00, 31.55it/s]

    Capturing num tokens (num_tokens=448 avail_mem=134.15 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.58it/s]Capturing num tokens (num_tokens=416 avail_mem=134.14 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.58it/s]Capturing num tokens (num_tokens=384 avail_mem=134.14 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.58it/s]Capturing num tokens (num_tokens=352 avail_mem=134.13 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.58it/s]Capturing num tokens (num_tokens=320 avail_mem=134.13 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.58it/s]Capturing num tokens (num_tokens=320 avail_mem=134.13 GB):  60%|██████    | 35/58 [00:01<00:00, 35.06it/s]Capturing num tokens (num_tokens=288 avail_mem=134.14 GB):  60%|██████    | 35/58 [00:01<00:00, 35.06it/s]Capturing num tokens (num_tokens=256 avail_mem=134.13 GB):  60%|██████    | 35/58 [00:01<00:00, 35.06it/s]Capturing num tokens (num_tokens=240 avail_mem=134.13 GB):  60%|██████    | 35/58 [00:01<00:00, 35.06it/s]Capturing num tokens (num_tokens=224 avail_mem=134.13 GB):  60%|██████    | 35/58 [00:01<00:00, 35.06it/s]

    Capturing num tokens (num_tokens=224 avail_mem=134.13 GB):  67%|██████▋   | 39/58 [00:01<00:00, 36.00it/s]Capturing num tokens (num_tokens=208 avail_mem=134.12 GB):  67%|██████▋   | 39/58 [00:01<00:00, 36.00it/s]Capturing num tokens (num_tokens=192 avail_mem=134.12 GB):  67%|██████▋   | 39/58 [00:01<00:00, 36.00it/s]Capturing num tokens (num_tokens=176 avail_mem=134.12 GB):  67%|██████▋   | 39/58 [00:01<00:00, 36.00it/s]Capturing num tokens (num_tokens=160 avail_mem=134.12 GB):  67%|██████▋   | 39/58 [00:01<00:00, 36.00it/s]Capturing num tokens (num_tokens=160 avail_mem=134.12 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.60it/s]Capturing num tokens (num_tokens=144 avail_mem=134.11 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.60it/s]Capturing num tokens (num_tokens=128 avail_mem=134.11 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.60it/s]Capturing num tokens (num_tokens=112 avail_mem=134.10 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.60it/s]Capturing num tokens (num_tokens=96 avail_mem=134.10 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.60it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=134.10 GB):  81%|████████  | 47/58 [00:01<00:00, 37.07it/s]Capturing num tokens (num_tokens=80 avail_mem=134.09 GB):  81%|████████  | 47/58 [00:01<00:00, 37.07it/s]Capturing num tokens (num_tokens=64 avail_mem=134.09 GB):  81%|████████  | 47/58 [00:01<00:00, 37.07it/s]Capturing num tokens (num_tokens=48 avail_mem=134.09 GB):  81%|████████  | 47/58 [00:01<00:00, 37.07it/s]Capturing num tokens (num_tokens=32 avail_mem=134.08 GB):  81%|████████  | 47/58 [00:01<00:00, 37.07it/s]Capturing num tokens (num_tokens=32 avail_mem=134.08 GB):  88%|████████▊ | 51/58 [00:01<00:00, 37.57it/s]Capturing num tokens (num_tokens=28 avail_mem=134.08 GB):  88%|████████▊ | 51/58 [00:01<00:00, 37.57it/s]Capturing num tokens (num_tokens=24 avail_mem=134.08 GB):  88%|████████▊ | 51/58 [00:01<00:00, 37.57it/s]Capturing num tokens (num_tokens=20 avail_mem=134.07 GB):  88%|████████▊ | 51/58 [00:01<00:00, 37.57it/s]Capturing num tokens (num_tokens=16 avail_mem=134.07 GB):  88%|████████▊ | 51/58 [00:01<00:00, 37.57it/s]

    Capturing num tokens (num_tokens=16 avail_mem=134.07 GB):  95%|█████████▍| 55/58 [00:01<00:00, 38.06it/s]Capturing num tokens (num_tokens=12 avail_mem=134.07 GB):  95%|█████████▍| 55/58 [00:01<00:00, 38.06it/s]Capturing num tokens (num_tokens=8 avail_mem=134.06 GB):  95%|█████████▍| 55/58 [00:01<00:00, 38.06it/s] Capturing num tokens (num_tokens=4 avail_mem=134.06 GB):  95%|█████████▍| 55/58 [00:02<00:00, 38.06it/s]Capturing num tokens (num_tokens=4 avail_mem=134.06 GB): 100%|██████████| 58/58 [00:02<00:00, 28.43it/s]


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


<strong style='color: #00008B;'>Text embedding (first 10): [-0.00023698806762695312, -0.0499267578125, -0.0032749176025390625, 0.0110931396484375, -0.01406097412109375, 0.016021728515625, -0.01444244384765625, 0.005901336669921875, -0.022796630859375, 0.0272979736328125]</strong>



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


    [2026-04-02 23:06:45] No HuggingFace chat template found


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-04-02 23:06:53.578 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:06:53] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:06:53.578 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:06:53] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:06:53.578 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:06:53] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:06:53.578 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:06:53] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:06:53.578 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:06:53] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  1.60it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  1.59it/s]


    [2026-04-02 23:06:58] Disable piecewise CUDA graph because the model is not a language model


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
    2026-04-02 23:07:24.523 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:07:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:07:24.523 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:07:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:07:24.523 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:07:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:07:24.523 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:07:24] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:07:24.523 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:07:24] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  3.17it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  3.17it/s]


    2026-04-02 23:07:29,292 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 23:07:29] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:34,  2.72s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:06,  1.19s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:06,  1.19s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:02<00:38,  1.44it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:02<00:38,  1.44it/s]Compiling num tokens (num_tokens=6144):   5%|▌         | 3/58 [00:03<00:38,  1.44it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:18,  2.85it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:18,  2.85it/s]

    Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:03<00:18,  2.85it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:11,  4.37it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:11,  4.37it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:11,  4.37it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:08,  6.09it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:08,  6.09it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:08,  6.09it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:05,  8.05it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:05,  8.05it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:05,  8.05it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:05,  8.05it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:03<00:03, 11.52it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:03<00:03, 11.52it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:03<00:03, 11.52it/s]Compiling num tokens (num_tokens=2048):  24%|██▍       | 14/58 [00:03<00:03, 11.52it/s]Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:03<00:02, 14.31it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:03<00:02, 14.31it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:03<00:02, 14.31it/s]

    Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:03<00:02, 14.31it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:03<00:02, 16.12it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:03<00:02, 16.12it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:03<00:02, 16.12it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:03<00:02, 16.12it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:04<00:01, 18.08it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:04<00:01, 18.08it/s]

    Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:04<00:01, 18.08it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:04<00:01, 18.08it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:04<00:01, 19.75it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:04<00:01, 19.75it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:04<00:01, 19.75it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:04<00:01, 19.75it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:04<00:01, 19.75it/s]

    Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:04<00:01, 22.56it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:04<00:01, 22.56it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:04<00:01, 22.56it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:04<00:01, 22.56it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:04<00:01, 23.38it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:04<00:01, 23.38it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:04<00:01, 23.38it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:04<00:01, 23.38it/s]

    Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:04<00:00, 23.71it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:04<00:00, 23.71it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:04<00:00, 23.71it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:04<00:00, 23.71it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:04<00:00, 22.98it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:04<00:00, 22.98it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:04<00:00, 22.98it/s]

    Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:04<00:00, 22.98it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:04<00:00, 23.96it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:04<00:00, 23.96it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:04<00:00, 23.96it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:04<00:00, 23.96it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:04<00:00, 24.03it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:04<00:00, 24.03it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:04<00:00, 24.03it/s] 

    Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:04<00:00, 24.03it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:05<00:00, 23.17it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:05<00:00, 23.17it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:05<00:00, 23.17it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:05<00:00, 23.17it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:05<00:00, 22.90it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:05<00:00, 22.90it/s]

    Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:05<00:00, 22.90it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:05<00:00, 22.90it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:05<00:00, 24.59it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:05<00:00, 24.59it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:05<00:00, 24.59it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:05<00:00, 24.59it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:05<00:00, 25.70it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:05<00:00, 25.70it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00, 10.74it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=100.36 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=100.36 GB):   2%|▏         | 1/58 [00:00<00:13,  4.35it/s]Capturing num tokens (num_tokens=7680 avail_mem=99.61 GB):   2%|▏         | 1/58 [00:00<00:13,  4.35it/s] Capturing num tokens (num_tokens=7680 avail_mem=99.61 GB):   3%|▎         | 2/58 [00:00<00:11,  4.80it/s]Capturing num tokens (num_tokens=7168 avail_mem=100.33 GB):   3%|▎         | 2/58 [00:00<00:11,  4.80it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=100.33 GB):   5%|▌         | 3/58 [00:00<00:12,  4.48it/s]Capturing num tokens (num_tokens=6656 avail_mem=99.67 GB):   5%|▌         | 3/58 [00:00<00:12,  4.48it/s] Capturing num tokens (num_tokens=6656 avail_mem=99.67 GB):   7%|▋         | 4/58 [00:00<00:10,  5.13it/s]Capturing num tokens (num_tokens=6144 avail_mem=100.34 GB):   7%|▋         | 4/58 [00:00<00:10,  5.13it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=100.34 GB):   9%|▊         | 5/58 [00:00<00:09,  5.39it/s]Capturing num tokens (num_tokens=5632 avail_mem=99.74 GB):   9%|▊         | 5/58 [00:00<00:09,  5.39it/s] Capturing num tokens (num_tokens=5632 avail_mem=99.74 GB):  10%|█         | 6/58 [00:01<00:08,  6.07it/s]Capturing num tokens (num_tokens=5120 avail_mem=100.34 GB):  10%|█         | 6/58 [00:01<00:08,  6.07it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=100.34 GB):  12%|█▏        | 7/58 [00:01<00:08,  6.36it/s]Capturing num tokens (num_tokens=4608 avail_mem=99.81 GB):  12%|█▏        | 7/58 [00:01<00:08,  6.36it/s] Capturing num tokens (num_tokens=4096 avail_mem=100.34 GB):  12%|█▏        | 7/58 [00:01<00:08,  6.36it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=100.34 GB):  16%|█▌        | 9/58 [00:01<00:06,  7.51it/s]Capturing num tokens (num_tokens=3840 avail_mem=99.84 GB):  16%|█▌        | 9/58 [00:01<00:06,  7.51it/s] Capturing num tokens (num_tokens=3584 avail_mem=100.33 GB):  16%|█▌        | 9/58 [00:01<00:06,  7.51it/s]Capturing num tokens (num_tokens=3584 avail_mem=100.33 GB):  19%|█▉        | 11/58 [00:01<00:05,  8.69it/s]Capturing num tokens (num_tokens=3328 avail_mem=99.86 GB):  19%|█▉        | 11/58 [00:01<00:05,  8.69it/s] 

    Capturing num tokens (num_tokens=3328 avail_mem=99.86 GB):  21%|██        | 12/58 [00:01<00:05,  7.92it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.32 GB):  21%|██        | 12/58 [00:01<00:05,  7.92it/s]Capturing num tokens (num_tokens=2816 avail_mem=99.88 GB):  21%|██        | 12/58 [00:01<00:05,  7.92it/s] Capturing num tokens (num_tokens=2816 avail_mem=99.88 GB):  24%|██▍       | 14/58 [00:01<00:04,  9.05it/s]Capturing num tokens (num_tokens=2560 avail_mem=100.12 GB):  24%|██▍       | 14/58 [00:01<00:04,  9.05it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=100.30 GB):  24%|██▍       | 14/58 [00:02<00:04,  9.05it/s]Capturing num tokens (num_tokens=2304 avail_mem=100.30 GB):  28%|██▊       | 16/58 [00:02<00:03, 10.50it/s]Capturing num tokens (num_tokens=2048 avail_mem=99.90 GB):  28%|██▊       | 16/58 [00:02<00:03, 10.50it/s] Capturing num tokens (num_tokens=1792 avail_mem=100.30 GB):  28%|██▊       | 16/58 [00:02<00:03, 10.50it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=100.30 GB):  31%|███       | 18/58 [00:02<00:03, 11.87it/s]Capturing num tokens (num_tokens=1536 avail_mem=99.93 GB):  31%|███       | 18/58 [00:02<00:03, 11.87it/s] Capturing num tokens (num_tokens=1280 avail_mem=100.29 GB):  31%|███       | 18/58 [00:02<00:03, 11.87it/s]Capturing num tokens (num_tokens=1280 avail_mem=100.29 GB):  34%|███▍      | 20/58 [00:02<00:02, 13.45it/s]Capturing num tokens (num_tokens=1024 avail_mem=99.94 GB):  34%|███▍      | 20/58 [00:02<00:02, 13.45it/s] Capturing num tokens (num_tokens=960 avail_mem=100.28 GB):  34%|███▍      | 20/58 [00:02<00:02, 13.45it/s]

    Capturing num tokens (num_tokens=896 avail_mem=100.30 GB):  34%|███▍      | 20/58 [00:02<00:02, 13.45it/s]Capturing num tokens (num_tokens=896 avail_mem=100.30 GB):  40%|███▉      | 23/58 [00:02<00:02, 14.64it/s]Capturing num tokens (num_tokens=832 avail_mem=99.24 GB):  40%|███▉      | 23/58 [00:02<00:02, 14.64it/s] 

    Capturing num tokens (num_tokens=768 avail_mem=99.24 GB):  40%|███▉      | 23/58 [00:02<00:02, 14.64it/s]Capturing num tokens (num_tokens=768 avail_mem=99.24 GB):  43%|████▎     | 25/58 [00:02<00:02, 11.93it/s]Capturing num tokens (num_tokens=704 avail_mem=99.01 GB):  43%|████▎     | 25/58 [00:02<00:02, 11.93it/s]

    Capturing num tokens (num_tokens=640 avail_mem=100.23 GB):  43%|████▎     | 25/58 [00:02<00:02, 11.93it/s]Capturing num tokens (num_tokens=640 avail_mem=100.23 GB):  47%|████▋     | 27/58 [00:03<00:02, 10.76it/s]Capturing num tokens (num_tokens=576 avail_mem=100.24 GB):  47%|████▋     | 27/58 [00:03<00:02, 10.76it/s]Capturing num tokens (num_tokens=512 avail_mem=99.23 GB):  47%|████▋     | 27/58 [00:03<00:02, 10.76it/s] 

    Capturing num tokens (num_tokens=512 avail_mem=99.23 GB):  50%|█████     | 29/58 [00:03<00:02, 10.69it/s]Capturing num tokens (num_tokens=480 avail_mem=99.42 GB):  50%|█████     | 29/58 [00:03<00:02, 10.69it/s]Capturing num tokens (num_tokens=448 avail_mem=99.41 GB):  50%|█████     | 29/58 [00:03<00:02, 10.69it/s]

    Capturing num tokens (num_tokens=448 avail_mem=99.41 GB):  53%|█████▎    | 31/58 [00:03<00:02,  9.27it/s]Capturing num tokens (num_tokens=416 avail_mem=100.17 GB):  53%|█████▎    | 31/58 [00:03<00:02,  9.27it/s]Capturing num tokens (num_tokens=384 avail_mem=100.23 GB):  53%|█████▎    | 31/58 [00:03<00:02,  9.27it/s]Capturing num tokens (num_tokens=384 avail_mem=100.23 GB):  57%|█████▋    | 33/58 [00:03<00:02,  9.77it/s]Capturing num tokens (num_tokens=352 avail_mem=99.45 GB):  57%|█████▋    | 33/58 [00:03<00:02,  9.77it/s] 

    Capturing num tokens (num_tokens=320 avail_mem=99.45 GB):  57%|█████▋    | 33/58 [00:03<00:02,  9.77it/s]Capturing num tokens (num_tokens=320 avail_mem=99.45 GB):  60%|██████    | 35/58 [00:03<00:02,  9.51it/s]Capturing num tokens (num_tokens=288 avail_mem=99.34 GB):  60%|██████    | 35/58 [00:03<00:02,  9.51it/s]Capturing num tokens (num_tokens=256 avail_mem=100.21 GB):  60%|██████    | 35/58 [00:03<00:02,  9.51it/s]

    Capturing num tokens (num_tokens=256 avail_mem=100.21 GB):  64%|██████▍   | 37/58 [00:04<00:02, 10.10it/s]Capturing num tokens (num_tokens=240 avail_mem=100.20 GB):  64%|██████▍   | 37/58 [00:04<00:02, 10.10it/s]Capturing num tokens (num_tokens=224 avail_mem=99.48 GB):  64%|██████▍   | 37/58 [00:04<00:02, 10.10it/s] Capturing num tokens (num_tokens=224 avail_mem=99.48 GB):  67%|██████▋   | 39/58 [00:04<00:01, 10.12it/s]Capturing num tokens (num_tokens=208 avail_mem=99.48 GB):  67%|██████▋   | 39/58 [00:04<00:01, 10.12it/s]

    Capturing num tokens (num_tokens=192 avail_mem=100.13 GB):  67%|██████▋   | 39/58 [00:04<00:01, 10.12it/s]Capturing num tokens (num_tokens=192 avail_mem=100.13 GB):  71%|███████   | 41/58 [00:04<00:01, 10.60it/s]Capturing num tokens (num_tokens=176 avail_mem=100.18 GB):  71%|███████   | 41/58 [00:04<00:01, 10.60it/s]Capturing num tokens (num_tokens=160 avail_mem=99.52 GB):  71%|███████   | 41/58 [00:04<00:01, 10.60it/s] 

    Capturing num tokens (num_tokens=160 avail_mem=99.52 GB):  74%|███████▍  | 43/58 [00:04<00:01, 10.59it/s]Capturing num tokens (num_tokens=144 avail_mem=99.51 GB):  74%|███████▍  | 43/58 [00:04<00:01, 10.59it/s]Capturing num tokens (num_tokens=128 avail_mem=100.17 GB):  74%|███████▍  | 43/58 [00:04<00:01, 10.59it/s]Capturing num tokens (num_tokens=128 avail_mem=100.17 GB):  78%|███████▊  | 45/58 [00:04<00:01, 11.20it/s]Capturing num tokens (num_tokens=112 avail_mem=99.56 GB):  78%|███████▊  | 45/58 [00:04<00:01, 11.20it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=99.51 GB):  78%|███████▊  | 45/58 [00:04<00:01, 11.20it/s] Capturing num tokens (num_tokens=96 avail_mem=99.51 GB):  81%|████████  | 47/58 [00:04<00:00, 11.05it/s]Capturing num tokens (num_tokens=80 avail_mem=99.54 GB):  81%|████████  | 47/58 [00:04<00:00, 11.05it/s]Capturing num tokens (num_tokens=64 avail_mem=100.14 GB):  81%|████████  | 47/58 [00:05<00:00, 11.05it/s]

    Capturing num tokens (num_tokens=64 avail_mem=100.14 GB):  84%|████████▍ | 49/58 [00:05<00:00, 11.56it/s]Capturing num tokens (num_tokens=48 avail_mem=99.59 GB):  84%|████████▍ | 49/58 [00:05<00:00, 11.56it/s] Capturing num tokens (num_tokens=32 avail_mem=99.58 GB):  84%|████████▍ | 49/58 [00:05<00:00, 11.56it/s]Capturing num tokens (num_tokens=32 avail_mem=99.58 GB):  88%|████████▊ | 51/58 [00:05<00:00, 11.76it/s]Capturing num tokens (num_tokens=28 avail_mem=100.12 GB):  88%|████████▊ | 51/58 [00:05<00:00, 11.76it/s]

    Capturing num tokens (num_tokens=24 avail_mem=99.63 GB):  88%|████████▊ | 51/58 [00:05<00:00, 11.76it/s] Capturing num tokens (num_tokens=24 avail_mem=99.63 GB):  91%|█████████▏| 53/58 [00:05<00:00, 11.63it/s]Capturing num tokens (num_tokens=20 avail_mem=100.15 GB):  91%|█████████▏| 53/58 [00:05<00:00, 11.63it/s]Capturing num tokens (num_tokens=16 avail_mem=100.11 GB):  91%|█████████▏| 53/58 [00:05<00:00, 11.63it/s]

    Capturing num tokens (num_tokens=16 avail_mem=100.11 GB):  95%|█████████▍| 55/58 [00:05<00:00, 12.25it/s]Capturing num tokens (num_tokens=12 avail_mem=99.66 GB):  95%|█████████▍| 55/58 [00:05<00:00, 12.25it/s] Capturing num tokens (num_tokens=8 avail_mem=100.09 GB):  95%|█████████▍| 55/58 [00:05<00:00, 12.25it/s]Capturing num tokens (num_tokens=8 avail_mem=100.09 GB):  98%|█████████▊| 57/58 [00:05<00:00, 13.22it/s]Capturing num tokens (num_tokens=4 avail_mem=99.68 GB):  98%|█████████▊| 57/58 [00:05<00:00, 13.22it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=99.68 GB): 100%|██████████| 58/58 [00:05<00:00, 10.00it/s]


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


<strong style='color: #00008B;'>Item 'Paris': probabilities = ['0.0237', '0.9763']</strong>



<strong style='color: #00008B;'>Item 'London': probabilities = ['0.0284', '0.9716']</strong>



<strong style='color: #00008B;'>Item 'Berlin': probabilities = ['0.0637', '0.9363']</strong>



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
    2026-04-02 23:08:05.832 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:08:05] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:08:05.832 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:08:05] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:08:05.832 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:08:05] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:08:05.832 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:08:05] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:08:05.832 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:08:05] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:00<00:00,  3.37it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:01<00:00,  2.02it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.70it/s]Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.90it/s]


    2026-04-02 23:08:13,732 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 23:08:13] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:12,  3.38s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:12,  3.38s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<02:02,  2.19s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<02:02,  2.19s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:05<01:22,  1.49s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:05<01:22,  1.49s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:06<01:01,  1.14s/it]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:06<01:01,  1.14s/it]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:06<00:49,  1.07it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:06<00:49,  1.07it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:07<00:40,  1.28it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:07<00:40,  1.28it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:07<00:34,  1.48it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:07<00:34,  1.48it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:07<00:29,  1.70it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:07<00:29,  1.70it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:08<00:23,  2.06it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:08<00:23,  2.06it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:08<00:19,  2.50it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:08<00:19,  2.50it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:08<00:16,  2.93it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:08<00:16,  2.93it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:08<00:13,  3.41it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:08<00:13,  3.41it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:08<00:11,  3.91it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:08<00:11,  3.91it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:09<00:10,  4.38it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:09<00:10,  4.38it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:09<00:08,  4.98it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:09<00:08,  4.98it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:09<00:07,  5.67it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:09<00:07,  5.67it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:09<00:06,  6.31it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:09<00:06,  6.31it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:09<00:06,  6.31it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:09<00:04,  7.82it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:09<00:04,  7.82it/s]

    Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:09<00:04,  7.82it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:09<00:03,  9.56it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:09<00:03,  9.56it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:09<00:03,  9.56it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:09<00:03, 11.50it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:09<00:03, 11.50it/s]

    Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:09<00:03, 11.50it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:10<00:03, 11.50it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:10<00:02, 14.29it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:10<00:02, 14.29it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:10<00:02, 14.29it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:10<00:02, 14.29it/s]

    Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:10<00:01, 16.57it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:10<00:01, 16.57it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:10<00:01, 16.57it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:10<00:01, 16.57it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:10<00:01, 18.97it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:10<00:01, 18.97it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:10<00:01, 18.97it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:10<00:01, 18.97it/s]

    Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:10<00:01, 21.43it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:10<00:01, 21.43it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:10<00:01, 21.43it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:10<00:01, 21.43it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:10<00:01, 21.43it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:10<00:00, 24.64it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:10<00:00, 24.64it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:10<00:00, 24.64it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:10<00:00, 24.64it/s]

    Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:10<00:00, 24.64it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:10<00:00, 27.65it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:10<00:00, 27.65it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:10<00:00, 27.65it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:10<00:00, 27.65it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:10<00:00, 27.65it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:10<00:00, 30.36it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:10<00:00, 30.36it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:10<00:00, 30.36it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:10<00:00, 30.36it/s]

    Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:10<00:00, 30.36it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:10<00:00, 32.85it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:10<00:00, 32.85it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:10<00:00, 32.85it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:10<00:00, 32.85it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:10<00:00, 32.85it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:10<00:00, 32.85it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:10<00:00, 36.44it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:10<00:00, 36.44it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:10<00:00, 36.44it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:11<00:00,  5.27it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=84.46 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=84.46 GB):   2%|▏         | 1/58 [00:00<00:39,  1.44it/s]Capturing num tokens (num_tokens=7680 avail_mem=84.43 GB):   2%|▏         | 1/58 [00:00<00:39,  1.44it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=84.43 GB):   3%|▎         | 2/58 [00:01<00:45,  1.22it/s]Capturing num tokens (num_tokens=7168 avail_mem=83.43 GB):   3%|▎         | 2/58 [00:01<00:45,  1.22it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=83.43 GB):   5%|▌         | 3/58 [00:02<00:45,  1.21it/s]Capturing num tokens (num_tokens=6656 avail_mem=83.44 GB):   5%|▌         | 3/58 [00:02<00:45,  1.21it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=83.44 GB):   7%|▋         | 4/58 [00:03<00:44,  1.22it/s]Capturing num tokens (num_tokens=6144 avail_mem=83.44 GB):   7%|▋         | 4/58 [00:03<00:44,  1.22it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=83.44 GB):   9%|▊         | 5/58 [00:03<00:41,  1.29it/s]Capturing num tokens (num_tokens=5632 avail_mem=83.44 GB):   9%|▊         | 5/58 [00:03<00:41,  1.29it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=83.44 GB):  10%|█         | 6/58 [00:04<00:38,  1.37it/s]Capturing num tokens (num_tokens=5120 avail_mem=83.45 GB):  10%|█         | 6/58 [00:04<00:38,  1.37it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=83.45 GB):  12%|█▏        | 7/58 [00:05<00:34,  1.46it/s]Capturing num tokens (num_tokens=4608 avail_mem=83.45 GB):  12%|█▏        | 7/58 [00:05<00:34,  1.46it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=83.45 GB):  14%|█▍        | 8/58 [00:05<00:31,  1.57it/s]Capturing num tokens (num_tokens=4096 avail_mem=83.45 GB):  14%|█▍        | 8/58 [00:05<00:31,  1.57it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=83.45 GB):  16%|█▌        | 9/58 [00:06<00:28,  1.69it/s]Capturing num tokens (num_tokens=3840 avail_mem=83.46 GB):  16%|█▌        | 9/58 [00:06<00:28,  1.69it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=83.46 GB):  17%|█▋        | 10/58 [00:06<00:26,  1.81it/s]Capturing num tokens (num_tokens=3584 avail_mem=83.46 GB):  17%|█▋        | 10/58 [00:06<00:26,  1.81it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=83.46 GB):  19%|█▉        | 11/58 [00:07<00:24,  1.94it/s]Capturing num tokens (num_tokens=3328 avail_mem=83.46 GB):  19%|█▉        | 11/58 [00:07<00:24,  1.94it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=83.46 GB):  21%|██        | 12/58 [00:07<00:22,  2.06it/s]Capturing num tokens (num_tokens=3072 avail_mem=83.46 GB):  21%|██        | 12/58 [00:07<00:22,  2.06it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=83.46 GB):  22%|██▏       | 13/58 [00:07<00:21,  2.13it/s]Capturing num tokens (num_tokens=2816 avail_mem=83.46 GB):  22%|██▏       | 13/58 [00:07<00:21,  2.13it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=83.46 GB):  24%|██▍       | 14/58 [00:08<00:18,  2.35it/s]Capturing num tokens (num_tokens=2560 avail_mem=83.46 GB):  24%|██▍       | 14/58 [00:08<00:18,  2.35it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=83.46 GB):  26%|██▌       | 15/58 [00:08<00:16,  2.58it/s]Capturing num tokens (num_tokens=2304 avail_mem=83.46 GB):  26%|██▌       | 15/58 [00:08<00:16,  2.58it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=83.46 GB):  28%|██▊       | 16/58 [00:08<00:14,  2.85it/s]Capturing num tokens (num_tokens=2048 avail_mem=83.45 GB):  28%|██▊       | 16/58 [00:08<00:14,  2.85it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=83.45 GB):  29%|██▉       | 17/58 [00:09<00:12,  3.21it/s]Capturing num tokens (num_tokens=1792 avail_mem=83.45 GB):  29%|██▉       | 17/58 [00:09<00:12,  3.21it/s]Capturing num tokens (num_tokens=1792 avail_mem=83.45 GB):  31%|███       | 18/58 [00:09<00:11,  3.61it/s]Capturing num tokens (num_tokens=1536 avail_mem=83.45 GB):  31%|███       | 18/58 [00:09<00:11,  3.61it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=83.45 GB):  33%|███▎      | 19/58 [00:09<00:09,  4.09it/s]Capturing num tokens (num_tokens=1280 avail_mem=83.45 GB):  33%|███▎      | 19/58 [00:09<00:09,  4.09it/s]Capturing num tokens (num_tokens=1280 avail_mem=83.45 GB):  34%|███▍      | 20/58 [00:09<00:08,  4.62it/s]Capturing num tokens (num_tokens=1024 avail_mem=83.45 GB):  34%|███▍      | 20/58 [00:09<00:08,  4.62it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=83.45 GB):  36%|███▌      | 21/58 [00:09<00:07,  5.20it/s]Capturing num tokens (num_tokens=960 avail_mem=83.44 GB):  36%|███▌      | 21/58 [00:09<00:07,  5.20it/s] Capturing num tokens (num_tokens=960 avail_mem=83.44 GB):  38%|███▊      | 22/58 [00:09<00:06,  5.76it/s]Capturing num tokens (num_tokens=896 avail_mem=83.44 GB):  38%|███▊      | 22/58 [00:09<00:06,  5.76it/s]

    Capturing num tokens (num_tokens=832 avail_mem=83.44 GB):  38%|███▊      | 22/58 [00:09<00:06,  5.76it/s]Capturing num tokens (num_tokens=832 avail_mem=83.44 GB):  41%|████▏     | 24/58 [00:10<00:04,  7.60it/s]Capturing num tokens (num_tokens=768 avail_mem=83.43 GB):  41%|████▏     | 24/58 [00:10<00:04,  7.60it/s]Capturing num tokens (num_tokens=704 avail_mem=83.43 GB):  41%|████▏     | 24/58 [00:10<00:04,  7.60it/s]

    Capturing num tokens (num_tokens=704 avail_mem=83.43 GB):  45%|████▍     | 26/58 [00:10<00:03,  9.15it/s]Capturing num tokens (num_tokens=640 avail_mem=83.42 GB):  45%|████▍     | 26/58 [00:10<00:03,  9.15it/s]Capturing num tokens (num_tokens=576 avail_mem=83.42 GB):  45%|████▍     | 26/58 [00:10<00:03,  9.15it/s]

    Capturing num tokens (num_tokens=576 avail_mem=83.42 GB):  48%|████▊     | 28/58 [00:10<00:03,  8.07it/s]Capturing num tokens (num_tokens=512 avail_mem=82.38 GB):  48%|████▊     | 28/58 [00:10<00:03,  8.07it/s]

    Capturing num tokens (num_tokens=512 avail_mem=82.38 GB):  50%|█████     | 29/58 [00:10<00:04,  7.13it/s]Capturing num tokens (num_tokens=480 avail_mem=82.38 GB):  50%|█████     | 29/58 [00:10<00:04,  7.13it/s]Capturing num tokens (num_tokens=480 avail_mem=82.38 GB):  52%|█████▏    | 30/58 [00:10<00:04,  6.50it/s]Capturing num tokens (num_tokens=448 avail_mem=82.37 GB):  52%|█████▏    | 30/58 [00:10<00:04,  6.50it/s]

    Capturing num tokens (num_tokens=448 avail_mem=82.37 GB):  53%|█████▎    | 31/58 [00:11<00:04,  6.23it/s]Capturing num tokens (num_tokens=416 avail_mem=83.37 GB):  53%|█████▎    | 31/58 [00:11<00:04,  6.23it/s]Capturing num tokens (num_tokens=416 avail_mem=83.37 GB):  55%|█████▌    | 32/58 [00:11<00:04,  6.20it/s]Capturing num tokens (num_tokens=384 avail_mem=82.55 GB):  55%|█████▌    | 32/58 [00:11<00:04,  6.20it/s]

    Capturing num tokens (num_tokens=384 avail_mem=82.55 GB):  57%|█████▋    | 33/58 [00:11<00:04,  5.88it/s]Capturing num tokens (num_tokens=352 avail_mem=82.54 GB):  57%|█████▋    | 33/58 [00:11<00:04,  5.88it/s]Capturing num tokens (num_tokens=352 avail_mem=82.54 GB):  59%|█████▊    | 34/58 [00:11<00:04,  5.65it/s]Capturing num tokens (num_tokens=320 avail_mem=82.54 GB):  59%|█████▊    | 34/58 [00:11<00:04,  5.65it/s]

    Capturing num tokens (num_tokens=320 avail_mem=82.54 GB):  60%|██████    | 35/58 [00:11<00:03,  5.78it/s]Capturing num tokens (num_tokens=288 avail_mem=83.36 GB):  60%|██████    | 35/58 [00:11<00:03,  5.78it/s]Capturing num tokens (num_tokens=288 avail_mem=83.36 GB):  62%|██████▏   | 36/58 [00:11<00:03,  5.97it/s]Capturing num tokens (num_tokens=256 avail_mem=82.59 GB):  62%|██████▏   | 36/58 [00:11<00:03,  5.97it/s]

    Capturing num tokens (num_tokens=256 avail_mem=82.59 GB):  64%|██████▍   | 37/58 [00:12<00:03,  5.81it/s]Capturing num tokens (num_tokens=240 avail_mem=82.59 GB):  64%|██████▍   | 37/58 [00:12<00:03,  5.81it/s]Capturing num tokens (num_tokens=240 avail_mem=82.59 GB):  66%|██████▌   | 38/58 [00:12<00:03,  5.70it/s]Capturing num tokens (num_tokens=224 avail_mem=83.41 GB):  66%|██████▌   | 38/58 [00:12<00:03,  5.70it/s]

    Capturing num tokens (num_tokens=224 avail_mem=83.41 GB):  67%|██████▋   | 39/58 [00:12<00:03,  5.79it/s]Capturing num tokens (num_tokens=208 avail_mem=82.63 GB):  67%|██████▋   | 39/58 [00:12<00:03,  5.79it/s]Capturing num tokens (num_tokens=208 avail_mem=82.63 GB):  69%|██████▉   | 40/58 [00:12<00:03,  5.65it/s]Capturing num tokens (num_tokens=192 avail_mem=82.63 GB):  69%|██████▉   | 40/58 [00:12<00:03,  5.65it/s]

    Capturing num tokens (num_tokens=192 avail_mem=82.63 GB):  71%|███████   | 41/58 [00:12<00:03,  5.57it/s]Capturing num tokens (num_tokens=176 avail_mem=82.62 GB):  71%|███████   | 41/58 [00:12<00:03,  5.57it/s]Capturing num tokens (num_tokens=176 avail_mem=82.62 GB):  72%|███████▏  | 42/58 [00:12<00:02,  5.70it/s]Capturing num tokens (num_tokens=160 avail_mem=83.33 GB):  72%|███████▏  | 42/58 [00:12<00:02,  5.70it/s]

    Capturing num tokens (num_tokens=160 avail_mem=83.33 GB):  74%|███████▍  | 43/58 [00:13<00:02,  5.71it/s]Capturing num tokens (num_tokens=144 avail_mem=82.67 GB):  74%|███████▍  | 43/58 [00:13<00:02,  5.71it/s]Capturing num tokens (num_tokens=144 avail_mem=82.67 GB):  76%|███████▌  | 44/58 [00:13<00:02,  5.70it/s]Capturing num tokens (num_tokens=128 avail_mem=83.33 GB):  76%|███████▌  | 44/58 [00:13<00:02,  5.70it/s]

    Capturing num tokens (num_tokens=128 avail_mem=83.33 GB):  78%|███████▊  | 45/58 [00:13<00:02,  6.02it/s]Capturing num tokens (num_tokens=112 avail_mem=82.74 GB):  78%|███████▊  | 45/58 [00:13<00:02,  6.02it/s]

    Capturing num tokens (num_tokens=112 avail_mem=82.74 GB):  79%|███████▉  | 46/58 [00:13<00:02,  5.04it/s]Capturing num tokens (num_tokens=96 avail_mem=82.73 GB):  79%|███████▉  | 46/58 [00:13<00:02,  5.04it/s] Capturing num tokens (num_tokens=96 avail_mem=82.73 GB):  81%|████████  | 47/58 [00:13<00:02,  5.44it/s]Capturing num tokens (num_tokens=80 avail_mem=83.34 GB):  81%|████████  | 47/58 [00:13<00:02,  5.44it/s]

    Capturing num tokens (num_tokens=80 avail_mem=83.34 GB):  83%|████████▎ | 48/58 [00:14<00:01,  5.34it/s]Capturing num tokens (num_tokens=64 avail_mem=82.78 GB):  83%|████████▎ | 48/58 [00:14<00:01,  5.34it/s]Capturing num tokens (num_tokens=64 avail_mem=82.78 GB):  84%|████████▍ | 49/58 [00:14<00:01,  5.54it/s]Capturing num tokens (num_tokens=48 avail_mem=83.33 GB):  84%|████████▍ | 49/58 [00:14<00:01,  5.54it/s]

    Capturing num tokens (num_tokens=48 avail_mem=83.33 GB):  86%|████████▌ | 50/58 [00:14<00:01,  5.92it/s]Capturing num tokens (num_tokens=32 avail_mem=82.84 GB):  86%|████████▌ | 50/58 [00:14<00:01,  5.92it/s]Capturing num tokens (num_tokens=32 avail_mem=82.84 GB):  88%|████████▊ | 51/58 [00:14<00:01,  5.79it/s]Capturing num tokens (num_tokens=28 avail_mem=82.83 GB):  88%|████████▊ | 51/58 [00:14<00:01,  5.79it/s]

    Capturing num tokens (num_tokens=28 avail_mem=82.83 GB):  90%|████████▉ | 52/58 [00:14<00:00,  6.30it/s]Capturing num tokens (num_tokens=24 avail_mem=82.88 GB):  90%|████████▉ | 52/58 [00:14<00:00,  6.30it/s]Capturing num tokens (num_tokens=24 avail_mem=82.88 GB):  91%|█████████▏| 53/58 [00:14<00:00,  6.04it/s]Capturing num tokens (num_tokens=20 avail_mem=82.88 GB):  91%|█████████▏| 53/58 [00:14<00:00,  6.04it/s]

    Capturing num tokens (num_tokens=20 avail_mem=82.88 GB):  93%|█████████▎| 54/58 [00:15<00:00,  6.40it/s]Capturing num tokens (num_tokens=16 avail_mem=83.31 GB):  93%|█████████▎| 54/58 [00:15<00:00,  6.40it/s]Capturing num tokens (num_tokens=16 avail_mem=83.31 GB):  95%|█████████▍| 55/58 [00:15<00:00,  6.23it/s]Capturing num tokens (num_tokens=12 avail_mem=82.90 GB):  95%|█████████▍| 55/58 [00:15<00:00,  6.23it/s]

    Capturing num tokens (num_tokens=12 avail_mem=82.90 GB):  97%|█████████▋| 56/58 [00:15<00:00,  6.51it/s]Capturing num tokens (num_tokens=8 avail_mem=83.31 GB):  97%|█████████▋| 56/58 [00:15<00:00,  6.51it/s] Capturing num tokens (num_tokens=8 avail_mem=83.31 GB):  98%|█████████▊| 57/58 [00:15<00:00,  6.36it/s]Capturing num tokens (num_tokens=4 avail_mem=82.92 GB):  98%|█████████▊| 57/58 [00:15<00:00,  6.36it/s]

    Capturing num tokens (num_tokens=4 avail_mem=82.92 GB): 100%|██████████| 58/58 [00:15<00:00,  7.05it/s]Capturing num tokens (num_tokens=4 avail_mem=82.92 GB): 100%|██████████| 58/58 [00:15<00:00,  3.72it/s]


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


<strong style='color: #00008B;'>reward: -24.25</strong>



<strong style='color: #00008B;'>reward: 1.0390625</strong>



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
    2026-04-02 23:09:06.886 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:09:06] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:09:06.886 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:09:06] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:09:06.886 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:09:06] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:09:06.886 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:09:06] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:09:06.886 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:09:06] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/8 [00:00<?, ?it/s]

    Multi-thread loading shards:  12% Completed | 1/8 [00:00<00:05,  1.37it/s]

    Multi-thread loading shards:  25% Completed | 2/8 [00:01<00:04,  1.28it/s]

    Multi-thread loading shards:  38% Completed | 3/8 [00:02<00:04,  1.25it/s]

    Multi-thread loading shards:  50% Completed | 4/8 [00:03<00:03,  1.24it/s]

    Multi-thread loading shards:  62% Completed | 5/8 [00:04<00:02,  1.23it/s]

    Multi-thread loading shards:  75% Completed | 6/8 [00:04<00:01,  1.56it/s]

    Multi-thread loading shards:  88% Completed | 7/8 [00:04<00:00,  1.56it/s]

    Multi-thread loading shards: 100% Completed | 8/8 [00:05<00:00,  1.43it/s]Multi-thread loading shards: 100% Completed | 8/8 [00:05<00:00,  1.38it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)


    2026-04-02 23:09:20,320 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 23:09:20] Unexpected error during package walk: cutlass.cute.experimental


    [2026-04-02 23:09:20] Using default MoE kernel config. Performance might be sub-optimal! Config file not found at /actions-runner/_work/sglang/sglang/python/sglang/srt/layers/moe/fused_moe_triton/configs/triton_3_5_1/E=60,N=1408,device_name=NVIDIA_H200.json, you can create them with https://github.com/sgl-project/sglang/tree/main/benchmark/kernels/fused_moe_triton
    [2026-04-02 23:09:20] Using MoE kernel config with down_moe=False. Performance might be sub-optimal! Config file not found at /actions-runner/_work/sglang/sglang/python/sglang/srt/layers/moe/fused_moe_triton/configs/triton_3_5_1/E=60,N=1408,device_name=NVIDIA_H200_down.json, you can create them with https://github.com/sgl-project/sglang/tree/main/benchmark/kernels/fused_moe_triton



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



<strong style='color: #00008B;'>{'text': ' Mountage', 'output_ids': [10423, 424, 151643], 'meta_info': {'id': 'e3225e5f40d3464a94ad9c3dfb6924c5', 'finish_reason': {'type': 'stop', 'matched': 151643}, 'prompt_tokens': 7, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 3, 'cached_tokens': 0, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.9138565910980105, 'response_sent_to_client_ts': 1775171365.3121064}}</strong>



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
    [2026-04-02 23:09:34] server_args=ServerArgs(model_path='qwen/qwen2.5-0.5b-instruct', tokenizer_path='qwen/qwen2.5-0.5b-instruct', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=35083, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.907, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=330036764, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='info', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_mfu_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='qwen/qwen2.5-0.5b-instruct', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser=None, tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, experts_shared_outer_loras=None, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_max_trie_depth=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enforce_disable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, enable_hisparse=False, hisparse_config=None, enable_lmcache=False, kt_weight_path=None, kt_method='AMXINT4', kt_cpuinfer=None, kt_threadpool_count=2, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, gc_threshold=None, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, engine_info_bootstrap_port=6789, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    [2026-04-02 23:09:34] CI: patched _patch_mistral_regex to skip HF API calls


    [2026-04-02 23:09:35] Watchdog TokenizerManager initialized.
    [2026-04-02 23:09:35] Using default HuggingFace chat template with detected content format: string


    [2026-04-02 23:09:41] CI: patched _patch_mistral_regex to skip HF API calls


    [2026-04-02 23:09:42] CI: patched _patch_mistral_regex to skip HF API calls


    [2026-04-02 23:09:43] Watchdog DetokenizerManager initialized.


    [2026-04-02 23:09:43] Init torch distributed begin.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [2026-04-02 23:09:43] Init torch distributed ends. elapsed=0.37 s, mem usage=0.09 GB
    2026-04-02 23:09:43.866 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:09:43] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:09:43.867 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:09:43] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:09:43.867 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:09:43] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:09:43.867 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:09:43] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 23:09:43.867 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 23:09:43] Persistent cache disabled, using in-memory JIT cache


    [2026-04-02 23:09:45] Load weight begin. avail mem=103.12 GB
    [2026-04-02 23:09:45] Found local HF snapshot for qwen/qwen2.5-0.5b-instruct at /root/.cache/huggingface/hub/models--qwen--qwen2.5-0.5b-instruct/snapshots/7ae557604adf67be50417f59c2c2f167def9a775; skipping download.


    [2026-04-02 23:09:45] No model.safetensors.index.json found in remote.
    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  1.71it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  1.71it/s]
    [2026-04-02 23:09:46] Load weight end. elapsed=0.96 s, type=Qwen2ForCausalLM, avail mem=102.15 GB, mem usage=0.98 GB.
    [2026-04-02 23:09:46] Using KV cache dtype: torch.bfloat16
    [2026-04-02 23:09:46] KV Cache is allocated. #tokens: 20480, K size: 0.12 GB, V size: 0.12 GB
    [2026-04-02 23:09:46] Memory pool end. avail mem=101.82 GB


    [2026-04-02 23:09:46] Warming up fused_temperature_softmax (multi-pass autotune, vocab_size=151936, logits_dtype=torch.bfloat16) ...


    [2026-04-02 23:09:50] Multi-pass autotune result: BLOCK_SIZE=32768, num_warps=32, num_stages=4
    [2026-04-02 23:09:50] fused_temperature_softmax warmup done (vocab_size=151936).
    [2026-04-02 23:09:50] Capture piecewise CUDA graph begin. avail mem=101.72 GB
    [2026-04-02 23:09:50] Capture cuda graph num tokens [4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192]
    2026-04-02 23:09:51,134 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 23:09:51] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    [2026-04-02 23:09:53] Compiling a graph for dynamic shape takes 0.20 s


    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:33,  2.69s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:05,  1.17s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:05,  1.17s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:05,  1.17s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:03<00:25,  2.12it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:11,  4.40it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:11,  4.40it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:11,  4.40it/s]

    Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:11,  4.40it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:03<00:06,  7.13it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:03<00:06,  7.13it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:03<00:06,  7.13it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:03<00:06,  7.13it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:03<00:06,  7.13it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:03<00:03, 11.38it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:03<00:03, 11.38it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:03<00:03, 11.38it/s]Compiling num tokens (num_tokens=2048):  24%|██▍       | 14/58 [00:03<00:03, 11.38it/s]

    Compiling num tokens (num_tokens=1792):  24%|██▍       | 14/58 [00:03<00:03, 11.38it/s]Compiling num tokens (num_tokens=1536):  24%|██▍       | 14/58 [00:03<00:03, 11.38it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:02, 17.08it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:02, 17.08it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:02, 17.08it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:02, 17.08it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:02, 17.08it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:02, 17.08it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:02, 17.08it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 23.92it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 23.92it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 23.92it/s]

    Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 23.92it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 23.92it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 23.92it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:03<00:00, 28.93it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:03<00:00, 28.93it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:03<00:00, 28.93it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:03<00:00, 28.93it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:03<00:00, 28.93it/s]

    Compiling num tokens (num_tokens=320):  52%|█████▏    | 30/58 [00:03<00:00, 28.93it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 28.52it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 28.52it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:03<00:00, 28.52it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:03<00:00, 28.52it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:03<00:00, 28.52it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:03<00:00, 28.52it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:03<00:00, 32.07it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:03<00:00, 32.07it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:03<00:00, 32.07it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:03<00:00, 32.07it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:03<00:00, 32.07it/s]

    Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:03<00:00, 32.07it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 35.70it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 35.70it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:04<00:00, 35.70it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:04<00:00, 35.70it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:04<00:00, 35.70it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:04<00:00, 35.70it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:04<00:00, 37.74it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:04<00:00, 37.74it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:04<00:00, 37.74it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:04<00:00, 37.74it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:04<00:00, 37.74it/s]

    Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:04<00:00, 37.74it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:04<00:00, 37.74it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:04<00:00, 43.16it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:04<00:00, 43.16it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:04<00:00, 43.16it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 13.71it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.07 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.07 GB):   2%|▏         | 1/58 [00:00<00:06,  9.31it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.04 GB):   2%|▏         | 1/58 [00:00<00:06,  9.31it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=119.04 GB):   3%|▎         | 2/58 [00:00<00:05,  9.68it/s]Capturing num tokens (num_tokens=7168 avail_mem=119.04 GB):   3%|▎         | 2/58 [00:00<00:05,  9.68it/s]Capturing num tokens (num_tokens=7168 avail_mem=119.04 GB):   5%|▌         | 3/58 [00:00<00:08,  6.84it/s]Capturing num tokens (num_tokens=6656 avail_mem=119.03 GB):   5%|▌         | 3/58 [00:00<00:08,  6.84it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=119.03 GB):   7%|▋         | 4/58 [00:00<00:07,  7.40it/s]Capturing num tokens (num_tokens=6144 avail_mem=119.04 GB):   7%|▋         | 4/58 [00:00<00:07,  7.40it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=119.04 GB):   9%|▊         | 5/58 [00:00<00:09,  5.60it/s]Capturing num tokens (num_tokens=5632 avail_mem=119.03 GB):   9%|▊         | 5/58 [00:00<00:09,  5.60it/s]Capturing num tokens (num_tokens=5120 avail_mem=119.03 GB):   9%|▊         | 5/58 [00:00<00:09,  5.60it/s]Capturing num tokens (num_tokens=5120 avail_mem=119.03 GB):  12%|█▏        | 7/58 [00:00<00:06,  7.47it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.03 GB):  12%|█▏        | 7/58 [00:00<00:06,  7.47it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=119.03 GB):  12%|█▏        | 7/58 [00:01<00:06,  7.47it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.03 GB):  16%|█▌        | 9/58 [00:01<00:04, 10.00it/s]Capturing num tokens (num_tokens=3840 avail_mem=119.03 GB):  16%|█▌        | 9/58 [00:01<00:04, 10.00it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.02 GB):  16%|█▌        | 9/58 [00:01<00:04, 10.00it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.02 GB):  16%|█▌        | 9/58 [00:01<00:04, 10.00it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.02 GB):  21%|██        | 12/58 [00:01<00:03, 13.36it/s]Capturing num tokens (num_tokens=3072 avail_mem=119.01 GB):  21%|██        | 12/58 [00:01<00:03, 13.36it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=119.01 GB):  21%|██        | 12/58 [00:01<00:03, 13.36it/s]Capturing num tokens (num_tokens=2560 avail_mem=119.01 GB):  21%|██        | 12/58 [00:01<00:03, 13.36it/s]Capturing num tokens (num_tokens=2560 avail_mem=119.01 GB):  26%|██▌       | 15/58 [00:01<00:02, 16.48it/s]Capturing num tokens (num_tokens=2304 avail_mem=119.01 GB):  26%|██▌       | 15/58 [00:01<00:02, 16.48it/s]Capturing num tokens (num_tokens=2048 avail_mem=119.00 GB):  26%|██▌       | 15/58 [00:01<00:02, 16.48it/s]Capturing num tokens (num_tokens=1792 avail_mem=119.00 GB):  26%|██▌       | 15/58 [00:01<00:02, 16.48it/s]Capturing num tokens (num_tokens=1792 avail_mem=119.00 GB):  31%|███       | 18/58 [00:01<00:02, 19.01it/s]Capturing num tokens (num_tokens=1536 avail_mem=119.00 GB):  31%|███       | 18/58 [00:01<00:02, 19.01it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=118.99 GB):  31%|███       | 18/58 [00:01<00:02, 19.01it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.97 GB):  31%|███       | 18/58 [00:01<00:02, 19.01it/s]Capturing num tokens (num_tokens=960 avail_mem=118.98 GB):  31%|███       | 18/58 [00:01<00:02, 19.01it/s] Capturing num tokens (num_tokens=960 avail_mem=118.98 GB):  38%|███▊      | 22/58 [00:01<00:01, 22.60it/s]Capturing num tokens (num_tokens=896 avail_mem=118.98 GB):  38%|███▊      | 22/58 [00:01<00:01, 22.60it/s]Capturing num tokens (num_tokens=832 avail_mem=118.98 GB):  38%|███▊      | 22/58 [00:01<00:01, 22.60it/s]Capturing num tokens (num_tokens=768 avail_mem=118.97 GB):  38%|███▊      | 22/58 [00:01<00:01, 22.60it/s]Capturing num tokens (num_tokens=704 avail_mem=118.97 GB):  38%|███▊      | 22/58 [00:01<00:01, 22.60it/s]

    Capturing num tokens (num_tokens=704 avail_mem=118.97 GB):  45%|████▍     | 26/58 [00:01<00:01, 25.21it/s]Capturing num tokens (num_tokens=640 avail_mem=118.97 GB):  45%|████▍     | 26/58 [00:01<00:01, 25.21it/s]Capturing num tokens (num_tokens=576 avail_mem=118.97 GB):  45%|████▍     | 26/58 [00:01<00:01, 25.21it/s]Capturing num tokens (num_tokens=512 avail_mem=118.96 GB):  45%|████▍     | 26/58 [00:01<00:01, 25.21it/s]Capturing num tokens (num_tokens=480 avail_mem=118.97 GB):  45%|████▍     | 26/58 [00:01<00:01, 25.21it/s]Capturing num tokens (num_tokens=480 avail_mem=118.97 GB):  52%|█████▏    | 30/58 [00:01<00:01, 27.38it/s]Capturing num tokens (num_tokens=448 avail_mem=118.97 GB):  52%|█████▏    | 30/58 [00:01<00:01, 27.38it/s]Capturing num tokens (num_tokens=416 avail_mem=118.97 GB):  52%|█████▏    | 30/58 [00:01<00:01, 27.38it/s]Capturing num tokens (num_tokens=384 avail_mem=118.97 GB):  52%|█████▏    | 30/58 [00:01<00:01, 27.38it/s]

    Capturing num tokens (num_tokens=352 avail_mem=118.96 GB):  52%|█████▏    | 30/58 [00:01<00:01, 27.38it/s]Capturing num tokens (num_tokens=352 avail_mem=118.96 GB):  59%|█████▊    | 34/58 [00:01<00:00, 29.07it/s]Capturing num tokens (num_tokens=320 avail_mem=118.96 GB):  59%|█████▊    | 34/58 [00:01<00:00, 29.07it/s]Capturing num tokens (num_tokens=288 avail_mem=118.95 GB):  59%|█████▊    | 34/58 [00:01<00:00, 29.07it/s]Capturing num tokens (num_tokens=256 avail_mem=118.95 GB):  59%|█████▊    | 34/58 [00:02<00:00, 29.07it/s]Capturing num tokens (num_tokens=240 avail_mem=118.95 GB):  59%|█████▊    | 34/58 [00:02<00:00, 29.07it/s]Capturing num tokens (num_tokens=240 avail_mem=118.95 GB):  66%|██████▌   | 38/58 [00:02<00:00, 30.33it/s]Capturing num tokens (num_tokens=224 avail_mem=118.95 GB):  66%|██████▌   | 38/58 [00:02<00:00, 30.33it/s]Capturing num tokens (num_tokens=208 avail_mem=118.94 GB):  66%|██████▌   | 38/58 [00:02<00:00, 30.33it/s]

    Capturing num tokens (num_tokens=192 avail_mem=118.94 GB):  66%|██████▌   | 38/58 [00:02<00:00, 30.33it/s]Capturing num tokens (num_tokens=176 avail_mem=118.94 GB):  66%|██████▌   | 38/58 [00:02<00:00, 30.33it/s]Capturing num tokens (num_tokens=176 avail_mem=118.94 GB):  72%|███████▏  | 42/58 [00:02<00:00, 31.19it/s]Capturing num tokens (num_tokens=160 avail_mem=118.93 GB):  72%|███████▏  | 42/58 [00:02<00:00, 31.19it/s]Capturing num tokens (num_tokens=144 avail_mem=118.93 GB):  72%|███████▏  | 42/58 [00:02<00:00, 31.19it/s]Capturing num tokens (num_tokens=128 avail_mem=118.93 GB):  72%|███████▏  | 42/58 [00:02<00:00, 31.19it/s]Capturing num tokens (num_tokens=112 avail_mem=118.93 GB):  72%|███████▏  | 42/58 [00:02<00:00, 31.19it/s]Capturing num tokens (num_tokens=112 avail_mem=118.93 GB):  79%|███████▉  | 46/58 [00:02<00:00, 31.89it/s]Capturing num tokens (num_tokens=96 avail_mem=118.92 GB):  79%|███████▉  | 46/58 [00:02<00:00, 31.89it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=118.92 GB):  79%|███████▉  | 46/58 [00:02<00:00, 31.89it/s]Capturing num tokens (num_tokens=64 avail_mem=118.91 GB):  79%|███████▉  | 46/58 [00:02<00:00, 31.89it/s]Capturing num tokens (num_tokens=48 avail_mem=118.91 GB):  79%|███████▉  | 46/58 [00:02<00:00, 31.89it/s]Capturing num tokens (num_tokens=48 avail_mem=118.91 GB):  86%|████████▌ | 50/58 [00:02<00:00, 32.19it/s]Capturing num tokens (num_tokens=32 avail_mem=118.91 GB):  86%|████████▌ | 50/58 [00:02<00:00, 32.19it/s]Capturing num tokens (num_tokens=28 avail_mem=118.90 GB):  86%|████████▌ | 50/58 [00:02<00:00, 32.19it/s]Capturing num tokens (num_tokens=24 avail_mem=118.90 GB):  86%|████████▌ | 50/58 [00:02<00:00, 32.19it/s]Capturing num tokens (num_tokens=20 avail_mem=118.90 GB):  86%|████████▌ | 50/58 [00:02<00:00, 32.19it/s]

    Capturing num tokens (num_tokens=20 avail_mem=118.90 GB):  93%|█████████▎| 54/58 [00:02<00:00, 33.11it/s]Capturing num tokens (num_tokens=16 avail_mem=118.90 GB):  93%|█████████▎| 54/58 [00:02<00:00, 33.11it/s]Capturing num tokens (num_tokens=12 avail_mem=118.89 GB):  93%|█████████▎| 54/58 [00:02<00:00, 33.11it/s]Capturing num tokens (num_tokens=8 avail_mem=118.89 GB):  93%|█████████▎| 54/58 [00:02<00:00, 33.11it/s] Capturing num tokens (num_tokens=4 avail_mem=118.88 GB):  93%|█████████▎| 54/58 [00:02<00:00, 33.11it/s]Capturing num tokens (num_tokens=4 avail_mem=118.88 GB): 100%|██████████| 58/58 [00:02<00:00, 32.52it/s]Capturing num tokens (num_tokens=4 avail_mem=118.88 GB): 100%|██████████| 58/58 [00:02<00:00, 21.78it/s]
    [2026-04-02 23:09:58] Capture piecewise CUDA graph end. Time elapsed: 8.00 s. mem usage=-17.16 GB. avail mem=118.88 GB.


    [2026-04-02 23:10:00] max_total_num_tokens=20480, chunked_prefill_size=8192, max_prefill_tokens=16384, max_running_requests=128, context_len=32768, available_gpu_mem=118.88 GB


    [2026-04-02 23:10:00] INFO:     Started server process [3101932]
    [2026-04-02 23:10:00] INFO:     Waiting for application startup.
    [2026-04-02 23:10:00] Using default chat sampling params from model generation config: {'repetition_penalty': 1.1, 'temperature': 0.7, 'top_k': 20, 'top_p': 0.8}
    [2026-04-02 23:10:00] INFO:     Application startup complete.
    [2026-04-02 23:10:00] INFO:     Uvicorn running on http://127.0.0.1:35083 (Press CTRL+C to quit)


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)
    [2026-04-02 23:10:00] INFO:     127.0.0.1:35888 - "GET /v1/models HTTP/1.1" 200 OK


    [2026-04-02 23:10:01] INFO:     127.0.0.1:35904 - "GET /model_info HTTP/1.1" 200 OK
    [2026-04-02 23:10:01] Prefill batch, #new-seq: 1, #new-token: 6, #cached-token: 0, token usage: 0.00, #running-req: 0, #queue-req: 0, cuda graph: True, input throughput (token/s): 0.00
    [2026-04-02 23:10:01] INFO:     127.0.0.1:35906 - "POST /generate HTTP/1.1" 200 OK
    [2026-04-02 23:10:01] The server is fired up and ready to roll!



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


    [2026-04-02 23:10:05] INFO:     127.0.0.1:35914 - "POST /tokenize HTTP/1.1" 200 OK



<strong style='color: #00008B;'><br>Tokenized Output (IDs):<br>[50, 3825, 524, 5707, 11050, 3950, 2022, 36342, 13]</strong>



<strong style='color: #00008B;'>Token Count: 9</strong>



<strong style='color: #00008B;'>Max Model Length: 131072</strong>


    [2026-04-02 23:10:05] INFO:     127.0.0.1:35920 - "POST /detokenize HTTP/1.1" 200 OK



<strong style='color: #00008B;'><br>Detokenized Output (Text):<br>'SGLang provides efficient tokenization endpoints.'</strong>



<strong style='color: #00008B;'><br>Round Trip Successful: Original and reconstructed text match.</strong>



```python
terminate_process(tokenizer_free_server_process)
```
