# Sending Requests
This notebook provides a quick-start guide to use SGLang in chat completions after installation. Once your server is running, API documentation is available at `http://localhost:30000/docs` (Swagger UI), `http://localhost:30000/redoc` (ReDoc), or `http://localhost:30000/openapi.json` (OpenAPI spec, useful for AI agents). Replace `30000` with your port if using a different one.

- For Vision Language Models, see [OpenAI APIs - Vision](openai_api_vision.ipynb).
- For Embedding Models, see [OpenAI APIs - Embedding](openai_api_embeddings.ipynb) and [Encode (embedding model)](native_api.html#Encode-(embedding-model)).
- For Reward Models, see [Classify (reward model)](native_api.html#Classify-(reward-model)).

## Launch A Server


```python
from sglang.test.doc_patch import launch_server_cmd
from sglang.utils import wait_for_server, print_highlight, terminate_process

# This is equivalent to running the following command in your terminal
# python3 -m sglang.launch_server --model-path qwen/qwen2.5-0.5b-instruct --host 0.0.0.0

server_process, port = launch_server_cmd("""
python3 -m sglang.launch_server --model-path qwen/qwen2.5-0.5b-instruct \
 --host 0.0.0.0 --log-level warning
""")

wait_for_server(f"http://localhost:{port}", process=server_process)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:176: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-30 08:24:39.387 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:24:39] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:24:39.387 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:24:39] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:24:39.387 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:24:39] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:24:39.387 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:24:39] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 08:24:39.387 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 08:24:39] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  3.45it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  3.44it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:14,  2.37s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:00,  1.08s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:00,  1.08s/it]

    Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:00,  1.08s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:24,  2.17it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:02<00:14,  3.57it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:02<00:14,  3.57it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:02<00:14,  3.57it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:03<00:09,  5.10it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:03<00:09,  5.10it/s]Compiling num tokens (num_tokens=3840):  14%|█▍        | 8/58 [00:03<00:09,  5.10it/s]

    Compiling num tokens (num_tokens=3584):  14%|█▍        | 8/58 [00:03<00:09,  5.10it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:05,  7.99it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:05,  7.99it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:05,  7.99it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:05,  7.99it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:05,  7.99it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:03<00:03, 12.61it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:03<00:03, 12.61it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:03<00:03, 12.61it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:03<00:03, 12.61it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:03<00:03, 12.61it/s]Compiling num tokens (num_tokens=1280):  26%|██▌       | 15/58 [00:03<00:03, 12.61it/s]

    Compiling num tokens (num_tokens=1024):  26%|██▌       | 15/58 [00:03<00:03, 12.61it/s]Compiling num tokens (num_tokens=960):  26%|██▌       | 15/58 [00:03<00:03, 12.61it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:01, 22.16it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:01, 22.16it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:01, 22.16it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:01, 22.16it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:01, 22.16it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 25.31it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 25.31it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 25.31it/s]

    Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 25.31it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 25.31it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:03<00:01, 27.38it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:03<00:01, 27.38it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:03<00:01, 27.38it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:03<00:01, 27.38it/s]Compiling num tokens (num_tokens=352):  52%|█████▏    | 30/58 [00:03<00:01, 27.38it/s]Compiling num tokens (num_tokens=320):  52%|█████▏    | 30/58 [00:03<00:01, 27.38it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 30.98it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 30.98it/s]

    Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:03<00:00, 30.98it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:03<00:00, 30.98it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:03<00:00, 30.98it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 31.50it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 31.50it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 31.50it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 31.50it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 31.50it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:03<00:00, 33.17it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:03<00:00, 33.17it/s]

    Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:03<00:00, 33.17it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:04<00:00, 33.17it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:04<00:00, 33.17it/s] Compiling num tokens (num_tokens=80):  74%|███████▍  | 43/58 [00:04<00:00, 33.17it/s]Compiling num tokens (num_tokens=64):  74%|███████▍  | 43/58 [00:04<00:00, 33.17it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:04<00:00, 38.87it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:04<00:00, 38.87it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:04<00:00, 38.87it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:04<00:00, 38.87it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:04<00:00, 38.87it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:04<00:00, 38.87it/s]

    Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:04<00:00, 39.53it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:04<00:00, 39.53it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:04<00:00, 39.53it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:04<00:00, 39.53it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:04<00:00, 39.53it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 13.58it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=58.17 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=58.14 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=58.14 GB):   3%|▎         | 2/58 [00:00<00:03, 14.10it/s]Capturing num tokens (num_tokens=7168 avail_mem=58.14 GB):   3%|▎         | 2/58 [00:00<00:03, 14.10it/s]Capturing num tokens (num_tokens=6656 avail_mem=58.13 GB):   3%|▎         | 2/58 [00:00<00:03, 14.10it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=58.14 GB):   3%|▎         | 2/58 [00:00<00:03, 14.10it/s]Capturing num tokens (num_tokens=6144 avail_mem=58.14 GB):   9%|▊         | 5/58 [00:00<00:02, 19.84it/s]Capturing num tokens (num_tokens=5632 avail_mem=58.13 GB):   9%|▊         | 5/58 [00:00<00:02, 19.84it/s]Capturing num tokens (num_tokens=5120 avail_mem=58.13 GB):   9%|▊         | 5/58 [00:00<00:02, 19.84it/s]Capturing num tokens (num_tokens=4608 avail_mem=58.13 GB):   9%|▊         | 5/58 [00:00<00:02, 19.84it/s]Capturing num tokens (num_tokens=4096 avail_mem=58.13 GB):   9%|▊         | 5/58 [00:00<00:02, 19.84it/s]Capturing num tokens (num_tokens=4096 avail_mem=58.13 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.58it/s]Capturing num tokens (num_tokens=3840 avail_mem=58.12 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.58it/s]Capturing num tokens (num_tokens=3584 avail_mem=58.12 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.58it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=58.12 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.58it/s]Capturing num tokens (num_tokens=3072 avail_mem=58.11 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.58it/s]Capturing num tokens (num_tokens=2816 avail_mem=58.11 GB):  16%|█▌        | 9/58 [00:00<00:01, 25.58it/s]Capturing num tokens (num_tokens=2816 avail_mem=58.11 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.83it/s]Capturing num tokens (num_tokens=2560 avail_mem=58.11 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.83it/s]Capturing num tokens (num_tokens=2304 avail_mem=58.10 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.83it/s]Capturing num tokens (num_tokens=2048 avail_mem=58.10 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.83it/s]Capturing num tokens (num_tokens=1792 avail_mem=58.09 GB):  24%|██▍       | 14/58 [00:00<00:01, 31.83it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=58.09 GB):  31%|███       | 18/58 [00:00<00:01, 25.97it/s]Capturing num tokens (num_tokens=1536 avail_mem=58.09 GB):  31%|███       | 18/58 [00:00<00:01, 25.97it/s]Capturing num tokens (num_tokens=1280 avail_mem=58.09 GB):  31%|███       | 18/58 [00:00<00:01, 25.97it/s]Capturing num tokens (num_tokens=1024 avail_mem=58.07 GB):  31%|███       | 18/58 [00:00<00:01, 25.97it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=58.07 GB):  36%|███▌      | 21/58 [00:00<00:01, 19.48it/s]Capturing num tokens (num_tokens=960 avail_mem=58.08 GB):  36%|███▌      | 21/58 [00:00<00:01, 19.48it/s] Capturing num tokens (num_tokens=896 avail_mem=58.08 GB):  36%|███▌      | 21/58 [00:01<00:01, 19.48it/s]Capturing num tokens (num_tokens=832 avail_mem=58.07 GB):  36%|███▌      | 21/58 [00:01<00:01, 19.48it/s]

    Capturing num tokens (num_tokens=832 avail_mem=58.07 GB):  41%|████▏     | 24/58 [00:01<00:01, 17.20it/s]Capturing num tokens (num_tokens=768 avail_mem=58.07 GB):  41%|████▏     | 24/58 [00:01<00:01, 17.20it/s]Capturing num tokens (num_tokens=704 avail_mem=58.07 GB):  41%|████▏     | 24/58 [00:01<00:01, 17.20it/s]Capturing num tokens (num_tokens=640 avail_mem=58.07 GB):  41%|████▏     | 24/58 [00:01<00:01, 17.20it/s]Capturing num tokens (num_tokens=576 avail_mem=58.07 GB):  41%|████▏     | 24/58 [00:01<00:01, 17.20it/s]Capturing num tokens (num_tokens=512 avail_mem=58.05 GB):  41%|████▏     | 24/58 [00:01<00:01, 17.20it/s]Capturing num tokens (num_tokens=480 avail_mem=58.07 GB):  41%|████▏     | 24/58 [00:01<00:01, 17.20it/s]Capturing num tokens (num_tokens=480 avail_mem=58.07 GB):  52%|█████▏    | 30/58 [00:01<00:01, 24.49it/s]Capturing num tokens (num_tokens=448 avail_mem=58.07 GB):  52%|█████▏    | 30/58 [00:01<00:01, 24.49it/s]Capturing num tokens (num_tokens=416 avail_mem=58.07 GB):  52%|█████▏    | 30/58 [00:01<00:01, 24.49it/s]Capturing num tokens (num_tokens=384 avail_mem=58.06 GB):  52%|█████▏    | 30/58 [00:01<00:01, 24.49it/s]Capturing num tokens (num_tokens=352 avail_mem=58.06 GB):  52%|█████▏    | 30/58 [00:01<00:01, 24.49it/s]Capturing num tokens (num_tokens=320 avail_mem=58.05 GB):  52%|█████▏    | 30/58 [00:01<00:01, 24.49it/s]

    Capturing num tokens (num_tokens=288 avail_mem=58.05 GB):  52%|█████▏    | 30/58 [00:01<00:01, 24.49it/s]Capturing num tokens (num_tokens=288 avail_mem=58.05 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.77it/s]Capturing num tokens (num_tokens=256 avail_mem=58.05 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.77it/s]Capturing num tokens (num_tokens=240 avail_mem=58.05 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.77it/s]Capturing num tokens (num_tokens=224 avail_mem=58.04 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.77it/s]Capturing num tokens (num_tokens=208 avail_mem=58.04 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.77it/s]Capturing num tokens (num_tokens=192 avail_mem=58.04 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.77it/s]Capturing num tokens (num_tokens=176 avail_mem=58.03 GB):  62%|██████▏   | 36/58 [00:01<00:00, 30.77it/s]Capturing num tokens (num_tokens=176 avail_mem=58.03 GB):  72%|███████▏  | 42/58 [00:01<00:00, 35.94it/s]Capturing num tokens (num_tokens=160 avail_mem=58.03 GB):  72%|███████▏  | 42/58 [00:01<00:00, 35.94it/s]Capturing num tokens (num_tokens=144 avail_mem=57.86 GB):  72%|███████▏  | 42/58 [00:01<00:00, 35.94it/s]Capturing num tokens (num_tokens=128 avail_mem=57.86 GB):  72%|███████▏  | 42/58 [00:01<00:00, 35.94it/s]

    Capturing num tokens (num_tokens=112 avail_mem=57.86 GB):  72%|███████▏  | 42/58 [00:01<00:00, 35.94it/s]Capturing num tokens (num_tokens=96 avail_mem=57.86 GB):  72%|███████▏  | 42/58 [00:01<00:00, 35.94it/s] Capturing num tokens (num_tokens=96 avail_mem=57.86 GB):  81%|████████  | 47/58 [00:01<00:00, 38.80it/s]Capturing num tokens (num_tokens=80 avail_mem=57.74 GB):  81%|████████  | 47/58 [00:01<00:00, 38.80it/s]Capturing num tokens (num_tokens=64 avail_mem=57.38 GB):  81%|████████  | 47/58 [00:01<00:00, 38.80it/s]Capturing num tokens (num_tokens=48 avail_mem=57.38 GB):  81%|████████  | 47/58 [00:01<00:00, 38.80it/s]Capturing num tokens (num_tokens=32 avail_mem=57.38 GB):  81%|████████  | 47/58 [00:01<00:00, 38.80it/s]Capturing num tokens (num_tokens=28 avail_mem=57.19 GB):  81%|████████  | 47/58 [00:01<00:00, 38.80it/s]

    Capturing num tokens (num_tokens=28 avail_mem=57.19 GB):  90%|████████▉ | 52/58 [00:01<00:00, 32.81it/s]Capturing num tokens (num_tokens=24 avail_mem=57.19 GB):  90%|████████▉ | 52/58 [00:01<00:00, 32.81it/s]Capturing num tokens (num_tokens=20 avail_mem=57.18 GB):  90%|████████▉ | 52/58 [00:01<00:00, 32.81it/s]Capturing num tokens (num_tokens=16 avail_mem=57.18 GB):  90%|████████▉ | 52/58 [00:01<00:00, 32.81it/s]

    Capturing num tokens (num_tokens=12 avail_mem=57.18 GB):  90%|████████▉ | 52/58 [00:02<00:00, 32.81it/s]Capturing num tokens (num_tokens=12 avail_mem=57.18 GB):  97%|█████████▋| 56/58 [00:02<00:00, 24.69it/s]Capturing num tokens (num_tokens=8 avail_mem=57.18 GB):  97%|█████████▋| 56/58 [00:02<00:00, 24.69it/s] Capturing num tokens (num_tokens=4 avail_mem=57.17 GB):  97%|█████████▋| 56/58 [00:02<00:00, 24.69it/s]Capturing num tokens (num_tokens=4 avail_mem=57.17 GB): 100%|██████████| 58/58 [00:02<00:00, 26.06it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


## Using cURL



```python
import subprocess, json

curl_command = f"""
curl -s http://localhost:{port}/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{{"model": "qwen/qwen2.5-0.5b-instruct", "messages": [{{"role": "user", "content": "What is the capital of France?"}}]}}'
"""

response = json.loads(subprocess.check_output(curl_command, shell=True))
print_highlight(response)
```


<strong style='color: #00008B;'>{'id': 'e6ea0ba07d704163a0ab932d9b2b2a64', 'object': 'chat.completion', 'created': 1774859095, 'model': 'qwen/qwen2.5-0.5b-instruct', 'choices': [{'index': 0, 'message': {'role': 'assistant', 'content': 'The capital of France is Paris.', 'reasoning_content': None, 'tool_calls': None}, 'logprobs': None, 'finish_reason': 'stop', 'matched_stop': 151645}], 'usage': {'prompt_tokens': 36, 'total_tokens': 44, 'completion_tokens': 8, 'prompt_tokens_details': None, 'reasoning_tokens': 0}, 'metadata': {'weight_version': 'default'}}</strong>


## Using Python Requests


```python
import requests

url = f"http://localhost:{port}/v1/chat/completions"

data = {
    "model": "qwen/qwen2.5-0.5b-instruct",
    "messages": [{"role": "user", "content": "What is the capital of France?"}],
}

response = requests.post(url, json=data)
print_highlight(response.json())
```


<strong style='color: #00008B;'>{'id': '2afbbcab776f411fa88c3517fb960f39', 'object': 'chat.completion', 'created': 1774859095, 'model': 'qwen/qwen2.5-0.5b-instruct', 'choices': [{'index': 0, 'message': {'role': 'assistant', 'content': 'The capital of France is Paris.', 'reasoning_content': None, 'tool_calls': None}, 'logprobs': None, 'finish_reason': 'stop', 'matched_stop': 151645}], 'usage': {'prompt_tokens': 36, 'total_tokens': 44, 'completion_tokens': 8, 'prompt_tokens_details': None, 'reasoning_tokens': 0}, 'metadata': {'weight_version': 'default'}}</strong>


## Using OpenAI Python Client


```python
import openai

client = openai.Client(base_url=f"http://127.0.0.1:{port}/v1", api_key="None")

response = client.chat.completions.create(
    model="qwen/qwen2.5-0.5b-instruct",
    messages=[
        {"role": "user", "content": "List 3 countries and their capitals."},
    ],
    temperature=0,
    max_tokens=64,
)
print_highlight(response)
```


<strong style='color: #00008B;'>ChatCompletion(id='d44f859c570e4b69b70a20cea2fcb9d5', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='Sure, here are three countries and their capitals:\n\n1. **United States** - Washington, D.C.\n2. **Canada** - Ottawa\n3. **Australia** - Canberra', refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=None, reasoning_content=None), matched_stop=151645)], created=1774859096, model='qwen/qwen2.5-0.5b-instruct', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=38, prompt_tokens=37, total_tokens=75, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>


### Streaming


```python
import openai

client = openai.Client(base_url=f"http://127.0.0.1:{port}/v1", api_key="None")

# Use stream=True for streaming responses
response = client.chat.completions.create(
    model="qwen/qwen2.5-0.5b-instruct",
    messages=[
        {"role": "user", "content": "List 3 countries and their capitals."},
    ],
    temperature=0,
    max_tokens=64,
    stream=True,
)

# Handle the streaming output
for chunk in response:
    if chunk.choices[0].delta.content:
        print(chunk.choices[0].delta.content, end="", flush=True)
```

    Sure

    ,

     here

     are

     three

     countries

     and

     their

     capitals

    :
    


    1

    .

     **

    United

     States

    **

     -

     Washington

    ,

     D

    .C

    .


    2

    .

     **

    Canada

    **

     -

     Ottawa

    


    3

    .

     **

    Australia

    **

     -

     Canberra

## Using Native Generation APIs

You can also use the native `/generate` endpoint with requests, which provides more flexibility. An API reference is available at [Sampling Parameters](sampling_params.md).


```python
import requests

response = requests.post(
    f"http://localhost:{port}/generate",
    json={
        "text": "The capital of France is",
        "sampling_params": {
            "temperature": 0,
            "max_new_tokens": 32,
        },
    },
)

print_highlight(response.json())
```


<strong style='color: #00008B;'>{'text': ' Paris. It is the largest city in Europe and the second largest city in the world. It is located in the south of France, on the banks of the', 'output_ids': [12095, 13, 1084, 374, 279, 7772, 3283, 304, 4505, 323, 279, 2086, 7772, 3283, 304, 279, 1879, 13, 1084, 374, 7407, 304, 279, 9806, 315, 9625, 11, 389, 279, 13959, 315, 279], 'meta_info': {'id': '2d3b2903ae404a23b4cce64cbe04e28d', 'finish_reason': {'type': 'length', 'length': 32}, 'prompt_tokens': 5, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 32, 'cached_tokens': 2, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.1853880180278793, 'response_sent_to_client_ts': 1774859097.017495}}</strong>


### Streaming


```python
import requests, json

response = requests.post(
    f"http://localhost:{port}/generate",
    json={
        "text": "The capital of France is",
        "sampling_params": {
            "temperature": 0,
            "max_new_tokens": 32,
        },
        "stream": True,
    },
    stream=True,
)

prev = 0
for chunk in response.iter_lines(decode_unicode=False):
    chunk = chunk.decode("utf-8")
    if chunk and chunk.startswith("data:"):
        if chunk == "data: [DONE]":
            break
        data = json.loads(chunk[5:].strip("\n"))
        output = data["text"]
        print(output[prev:], end="", flush=True)
        prev = len(output)
```

     Paris

    .

     It

     is

     the

     largest

     city

     in

     Europe

     and

     the

     second

     largest

     city

     in

     the

     world

    .

     It

     is

     located

     in

     the

     south

     of

     France

    ,

     on

     the

     banks

     of

     the


```python
terminate_process(server_process)
```
