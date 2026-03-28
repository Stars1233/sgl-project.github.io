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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-28 06:00:50.472 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 06:00:50] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 06:00:50.472 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 06:00:50] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 06:00:50.472 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 06:00:50] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 06:00:50.472 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 06:00:50] Persistent cache disabled, using in-memory JIT cache
    2026-03-28 06:00:50.473 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-28 06:00:50] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.39it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.38it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:11,  2.31s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:11,  2.31s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<00:56,  1.02s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<00:56,  1.02s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<00:56,  1.02s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:22,  2.43it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:22,  2.43it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:22,  2.43it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:22,  2.43it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:02<00:10,  5.01it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:02<00:10,  5.01it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:02<00:10,  5.01it/s]Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:02<00:10,  5.01it/s]

    Compiling num tokens (num_tokens=3584):  12%|█▏        | 7/58 [00:02<00:10,  5.01it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:05,  8.99it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:05,  8.99it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:05,  8.99it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:05,  8.99it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:05,  8.99it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:05,  8.99it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:02<00:02, 14.63it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:02<00:02, 14.63it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:02<00:02, 14.63it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:02<00:02, 14.63it/s]Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:02<00:02, 14.63it/s]

    Compiling num tokens (num_tokens=1024):  28%|██▊       | 16/58 [00:02<00:02, 14.63it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:02<00:01, 20.31it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:02<00:01, 20.31it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:03<00:01, 20.31it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:03<00:01, 20.31it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:03<00:01, 20.31it/s]Compiling num tokens (num_tokens=704):  36%|███▌      | 21/58 [00:03<00:01, 20.31it/s]Compiling num tokens (num_tokens=640):  36%|███▌      | 21/58 [00:03<00:01, 20.31it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 27.75it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 27.75it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 27.75it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 27.75it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 27.75it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 27.75it/s]

    Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 27.75it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 34.08it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 34.08it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 34.08it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 34.08it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 34.08it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 34.08it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 37.54it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 37.54it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 37.54it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 37.54it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 37.54it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 37.54it/s]

    Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 37.54it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 42.08it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 42.08it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 42.08it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 42.08it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 42.08it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 42.08it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 42.60it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 42.60it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 42.60it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 42.60it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 42.60it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 42.60it/s]

    Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 42.60it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 42.60it/s]Compiling num tokens (num_tokens=12):  97%|█████████▋| 56/58 [00:03<00:00, 49.47it/s]Compiling num tokens (num_tokens=8):  97%|█████████▋| 56/58 [00:03<00:00, 49.47it/s] Compiling num tokens (num_tokens=4):  97%|█████████▋| 56/58 [00:03<00:00, 49.47it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.89it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.72 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.72 GB):   3%|▎         | 2/58 [00:00<00:05, 11.00it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.68 GB):   3%|▎         | 2/58 [00:00<00:05, 11.00it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=118.68 GB):   3%|▎         | 2/58 [00:00<00:05, 11.00it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.68 GB):   7%|▋         | 4/58 [00:00<00:04, 12.47it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.68 GB):   7%|▋         | 4/58 [00:00<00:04, 12.47it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.68 GB):   7%|▋         | 4/58 [00:00<00:04, 12.47it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.68 GB):  10%|█         | 6/58 [00:00<00:03, 14.03it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.68 GB):  10%|█         | 6/58 [00:00<00:03, 14.03it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=118.67 GB):  10%|█         | 6/58 [00:00<00:03, 14.03it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.67 GB):  10%|█         | 6/58 [00:00<00:03, 14.03it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.67 GB):  16%|█▌        | 9/58 [00:00<00:02, 16.85it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.66 GB):  16%|█▌        | 9/58 [00:00<00:02, 16.85it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.65 GB):  16%|█▌        | 9/58 [00:00<00:02, 16.85it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.64 GB):  16%|█▌        | 9/58 [00:00<00:02, 16.85it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=118.64 GB):  21%|██        | 12/58 [00:00<00:02, 19.41it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.64 GB):  21%|██        | 12/58 [00:00<00:02, 19.41it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.63 GB):  21%|██        | 12/58 [00:00<00:02, 19.41it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.62 GB):  21%|██        | 12/58 [00:00<00:02, 19.41it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.62 GB):  26%|██▌       | 15/58 [00:00<00:01, 21.97it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.62 GB):  26%|██▌       | 15/58 [00:00<00:01, 21.97it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.61 GB):  26%|██▌       | 15/58 [00:00<00:01, 21.97it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.60 GB):  26%|██▌       | 15/58 [00:00<00:01, 21.97it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.60 GB):  26%|██▌       | 15/58 [00:00<00:01, 21.97it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=118.60 GB):  33%|███▎      | 19/58 [00:00<00:01, 25.59it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.57 GB):  33%|███▎      | 19/58 [00:00<00:01, 25.59it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.57 GB):  33%|███▎      | 19/58 [00:00<00:01, 25.59it/s]Capturing num tokens (num_tokens=960 avail_mem=118.58 GB):  33%|███▎      | 19/58 [00:00<00:01, 25.59it/s] Capturing num tokens (num_tokens=896 avail_mem=118.57 GB):  33%|███▎      | 19/58 [00:01<00:01, 25.59it/s]Capturing num tokens (num_tokens=896 avail_mem=118.57 GB):  40%|███▉      | 23/58 [00:01<00:01, 29.17it/s]Capturing num tokens (num_tokens=832 avail_mem=118.57 GB):  40%|███▉      | 23/58 [00:01<00:01, 29.17it/s]Capturing num tokens (num_tokens=768 avail_mem=118.56 GB):  40%|███▉      | 23/58 [00:01<00:01, 29.17it/s]Capturing num tokens (num_tokens=704 avail_mem=118.55 GB):  40%|███▉      | 23/58 [00:01<00:01, 29.17it/s]Capturing num tokens (num_tokens=640 avail_mem=118.55 GB):  40%|███▉      | 23/58 [00:01<00:01, 29.17it/s]

    Capturing num tokens (num_tokens=640 avail_mem=118.55 GB):  47%|████▋     | 27/58 [00:01<00:00, 31.56it/s]Capturing num tokens (num_tokens=576 avail_mem=118.53 GB):  47%|████▋     | 27/58 [00:01<00:00, 31.56it/s]Capturing num tokens (num_tokens=512 avail_mem=118.54 GB):  47%|████▋     | 27/58 [00:01<00:00, 31.56it/s]Capturing num tokens (num_tokens=480 avail_mem=118.55 GB):  47%|████▋     | 27/58 [00:01<00:00, 31.56it/s]Capturing num tokens (num_tokens=448 avail_mem=118.55 GB):  47%|████▋     | 27/58 [00:01<00:00, 31.56it/s]Capturing num tokens (num_tokens=448 avail_mem=118.55 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.12it/s]Capturing num tokens (num_tokens=416 avail_mem=118.54 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.12it/s]Capturing num tokens (num_tokens=384 avail_mem=118.54 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.12it/s]Capturing num tokens (num_tokens=352 avail_mem=118.53 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.12it/s]Capturing num tokens (num_tokens=320 avail_mem=118.52 GB):  53%|█████▎    | 31/58 [00:01<00:00, 33.12it/s]

    Capturing num tokens (num_tokens=320 avail_mem=118.52 GB):  60%|██████    | 35/58 [00:01<00:00, 34.26it/s]Capturing num tokens (num_tokens=288 avail_mem=118.52 GB):  60%|██████    | 35/58 [00:01<00:00, 34.26it/s]Capturing num tokens (num_tokens=256 avail_mem=118.51 GB):  60%|██████    | 35/58 [00:01<00:00, 34.26it/s]Capturing num tokens (num_tokens=240 avail_mem=118.50 GB):  60%|██████    | 35/58 [00:01<00:00, 34.26it/s]Capturing num tokens (num_tokens=224 avail_mem=118.50 GB):  60%|██████    | 35/58 [00:01<00:00, 34.26it/s]Capturing num tokens (num_tokens=224 avail_mem=118.50 GB):  67%|██████▋   | 39/58 [00:01<00:00, 35.30it/s]Capturing num tokens (num_tokens=208 avail_mem=118.49 GB):  67%|██████▋   | 39/58 [00:01<00:00, 35.30it/s]Capturing num tokens (num_tokens=192 avail_mem=118.49 GB):  67%|██████▋   | 39/58 [00:01<00:00, 35.30it/s]Capturing num tokens (num_tokens=176 avail_mem=118.48 GB):  67%|██████▋   | 39/58 [00:01<00:00, 35.30it/s]Capturing num tokens (num_tokens=160 avail_mem=118.47 GB):  67%|██████▋   | 39/58 [00:01<00:00, 35.30it/s]

    Capturing num tokens (num_tokens=160 avail_mem=118.47 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.21it/s]Capturing num tokens (num_tokens=144 avail_mem=118.47 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.21it/s]Capturing num tokens (num_tokens=128 avail_mem=118.46 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.21it/s]Capturing num tokens (num_tokens=112 avail_mem=118.46 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.21it/s]Capturing num tokens (num_tokens=96 avail_mem=118.43 GB):  74%|███████▍  | 43/58 [00:01<00:00, 36.21it/s] Capturing num tokens (num_tokens=96 avail_mem=118.43 GB):  81%|████████  | 47/58 [00:01<00:00, 37.07it/s]Capturing num tokens (num_tokens=80 avail_mem=118.42 GB):  81%|████████  | 47/58 [00:01<00:00, 37.07it/s]Capturing num tokens (num_tokens=64 avail_mem=118.42 GB):  81%|████████  | 47/58 [00:01<00:00, 37.07it/s]Capturing num tokens (num_tokens=48 avail_mem=118.43 GB):  81%|████████  | 47/58 [00:01<00:00, 37.07it/s]Capturing num tokens (num_tokens=32 avail_mem=118.42 GB):  81%|████████  | 47/58 [00:01<00:00, 37.07it/s]

    Capturing num tokens (num_tokens=32 avail_mem=118.42 GB):  88%|████████▊ | 51/58 [00:01<00:00, 37.59it/s]Capturing num tokens (num_tokens=28 avail_mem=118.41 GB):  88%|████████▊ | 51/58 [00:01<00:00, 37.59it/s]Capturing num tokens (num_tokens=24 avail_mem=118.41 GB):  88%|████████▊ | 51/58 [00:01<00:00, 37.59it/s]Capturing num tokens (num_tokens=20 avail_mem=118.40 GB):  88%|████████▊ | 51/58 [00:01<00:00, 37.59it/s]Capturing num tokens (num_tokens=16 avail_mem=118.40 GB):  88%|████████▊ | 51/58 [00:01<00:00, 37.59it/s]Capturing num tokens (num_tokens=12 avail_mem=118.39 GB):  88%|████████▊ | 51/58 [00:01<00:00, 37.59it/s]Capturing num tokens (num_tokens=12 avail_mem=118.39 GB):  97%|█████████▋| 56/58 [00:01<00:00, 38.48it/s]Capturing num tokens (num_tokens=8 avail_mem=118.39 GB):  97%|█████████▋| 56/58 [00:01<00:00, 38.48it/s] Capturing num tokens (num_tokens=4 avail_mem=118.38 GB):  97%|█████████▋| 56/58 [00:01<00:00, 38.48it/s]Capturing num tokens (num_tokens=4 avail_mem=118.38 GB): 100%|██████████| 58/58 [00:01<00:00, 29.57it/s]


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


<strong style='color: #00008B;'>{'id': 'e327bf171f52487897b98a997d3189bb', 'object': 'chat.completion', 'created': 1774677667, 'model': 'qwen/qwen2.5-0.5b-instruct', 'choices': [{'index': 0, 'message': {'role': 'assistant', 'content': 'The capital of France is Paris.', 'reasoning_content': None, 'tool_calls': None}, 'logprobs': None, 'finish_reason': 'stop', 'matched_stop': 151645}], 'usage': {'prompt_tokens': 36, 'total_tokens': 44, 'completion_tokens': 8, 'prompt_tokens_details': None, 'reasoning_tokens': 0}, 'metadata': {'weight_version': 'default'}}</strong>


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


<strong style='color: #00008B;'>{'id': 'fb4c32fa674b45d9a71317827fb79d90', 'object': 'chat.completion', 'created': 1774677667, 'model': 'qwen/qwen2.5-0.5b-instruct', 'choices': [{'index': 0, 'message': {'role': 'assistant', 'content': 'The capital of France is Paris.', 'reasoning_content': None, 'tool_calls': None}, 'logprobs': None, 'finish_reason': 'stop', 'matched_stop': 151645}], 'usage': {'prompt_tokens': 36, 'total_tokens': 44, 'completion_tokens': 8, 'prompt_tokens_details': None, 'reasoning_tokens': 0}, 'metadata': {'weight_version': 'default'}}</strong>


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


<strong style='color: #00008B;'>ChatCompletion(id='4c207bb4317e4ef7bba046d67bf0288a', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='Sure, here are three countries and their respective capitals:\n\n1. **United States** - Washington, D.C.\n2. **Canada** - Ottawa\n3. **Australia** - Canberra', refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=None, reasoning_content=None), matched_stop=151645)], created=1774677668, model='qwen/qwen2.5-0.5b-instruct', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=39, prompt_tokens=37, total_tokens=76, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>


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

     respective

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


<strong style='color: #00008B;'>{'text': ' Paris. It is the largest city in Europe and the second largest city in the world. It is located in the south of France, on the banks of the', 'output_ids': [12095, 13, 1084, 374, 279, 7772, 3283, 304, 4505, 323, 279, 2086, 7772, 3283, 304, 279, 1879, 13, 1084, 374, 7407, 304, 279, 9806, 315, 9625, 11, 389, 279, 13959, 315, 279], 'meta_info': {'id': 'e5bf6d7120ea40fda798493fffda25b2', 'finish_reason': {'type': 'length', 'length': 32}, 'prompt_tokens': 5, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 32, 'cached_tokens': 2, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.2583757066167891, 'response_sent_to_client_ts': 1774677669.228069}}</strong>


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
