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

    [2026-03-25 11:03:33] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-25 11:03:33] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-25 11:03:33] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-25 11:03:38] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 11:03:38] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 11:03:38] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    [2026-03-25 11:03:40] INFO server_args.py:2246: Attention backend not specified. Use fa3 backend by default.
    [2026-03-25 11:03:40] INFO server_args.py:3544: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-25 11:03:46] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 11:03:46] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 11:03:46] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-25 11:03:47] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-25 11:03:47] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-25 11:03:47] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-25 11:03:51.651 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:03:51] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:03:51.651 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:03:51] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:03:51.651 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:03:51] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:03:51.651 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:03:51] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 11:03:51.651 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 11:03:51] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.29it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.28it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:15,  2.38s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:15,  2.38s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:15,  2.38s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:02<00:36,  1.49it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:02<00:36,  1.49it/s]Compiling num tokens (num_tokens=6144):   5%|▌         | 3/58 [00:02<00:36,  1.49it/s]

    Compiling num tokens (num_tokens=5632):   5%|▌         | 3/58 [00:02<00:36,  1.49it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:02<00:14,  3.52it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:02<00:14,  3.52it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:02<00:14,  3.52it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:02<00:14,  3.52it/s]Compiling num tokens (num_tokens=3840):  10%|█         | 6/58 [00:02<00:14,  3.52it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.80it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.80it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.80it/s]

    Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.80it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.80it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.80it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:02<00:03, 11.70it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:02<00:03, 11.70it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:02<00:03, 11.70it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:02<00:03, 11.70it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:02<00:03, 11.70it/s]Compiling num tokens (num_tokens=1280):  26%|██▌       | 15/58 [00:02<00:03, 11.70it/s]Compiling num tokens (num_tokens=1024):  26%|██▌       | 15/58 [00:02<00:03, 11.70it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:02<00:02, 18.26it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:02<00:02, 18.26it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:03<00:02, 18.26it/s]

    Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:03<00:02, 18.26it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:03<00:02, 18.26it/s]Compiling num tokens (num_tokens=704):  36%|███▌      | 21/58 [00:03<00:02, 18.26it/s]Compiling num tokens (num_tokens=640):  36%|███▌      | 21/58 [00:03<00:02, 18.26it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 25.00it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 25.00it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 25.00it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 25.00it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 25.00it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 25.00it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 25.00it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 31.48it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 31.48it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 31.48it/s]

    Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 31.48it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 31.48it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 31.48it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 35.10it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 35.10it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 35.10it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 35.10it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 35.10it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 35.10it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 35.10it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 40.03it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 40.03it/s]

    Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 40.03it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 40.03it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 40.03it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 40.03it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 40.03it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 41.74it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 41.74it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 41.74it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 41.74it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 41.74it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 41.74it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 41.74it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 41.74it/s] 

    Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 41.74it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 50.33it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.93it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.42 GB):   2%|▏         | 1/58 [00:00<00:05,  9.92it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.69 GB):   2%|▏         | 1/58 [00:00<00:05,  9.92it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.71 GB):   2%|▏         | 1/58 [00:00<00:05,  9.92it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=118.71 GB):   5%|▌         | 3/58 [00:00<00:04, 12.15it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.46 GB):   5%|▌         | 3/58 [00:00<00:04, 12.15it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.48 GB):   5%|▌         | 3/58 [00:00<00:04, 12.15it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.48 GB):   9%|▊         | 5/58 [00:00<00:04, 12.81it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.48 GB):   9%|▊         | 5/58 [00:00<00:04, 12.81it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=118.67 GB):   9%|▊         | 5/58 [00:00<00:04, 12.81it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.67 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.39it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.51 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.39it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.52 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.39it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.57 GB):  12%|█▏        | 7/58 [00:00<00:03, 14.39it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.57 GB):  17%|█▋        | 10/58 [00:00<00:02, 17.61it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.65 GB):  17%|█▋        | 10/58 [00:00<00:02, 17.61it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=118.53 GB):  17%|█▋        | 10/58 [00:00<00:02, 17.61it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.63 GB):  17%|█▋        | 10/58 [00:00<00:02, 17.61it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.63 GB):  22%|██▏       | 13/58 [00:00<00:02, 19.26it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.63 GB):  22%|██▏       | 13/58 [00:00<00:02, 19.26it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.62 GB):  22%|██▏       | 13/58 [00:00<00:02, 19.26it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.53 GB):  22%|██▏       | 13/58 [00:00<00:02, 19.26it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=118.53 GB):  28%|██▊       | 16/58 [00:00<00:01, 21.21it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.55 GB):  28%|██▊       | 16/58 [00:00<00:01, 21.21it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.55 GB):  28%|██▊       | 16/58 [00:00<00:01, 21.21it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.60 GB):  28%|██▊       | 16/58 [00:00<00:01, 21.21it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.60 GB):  33%|███▎      | 19/58 [00:00<00:01, 23.32it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.57 GB):  33%|███▎      | 19/58 [00:00<00:01, 23.32it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.56 GB):  33%|███▎      | 19/58 [00:01<00:01, 23.32it/s]Capturing num tokens (num_tokens=960 avail_mem=118.58 GB):  33%|███▎      | 19/58 [00:01<00:01, 23.32it/s] Capturing num tokens (num_tokens=896 avail_mem=118.57 GB):  33%|███▎      | 19/58 [00:01<00:01, 23.32it/s]

    Capturing num tokens (num_tokens=896 avail_mem=118.57 GB):  40%|███▉      | 23/58 [00:01<00:01, 26.38it/s]Capturing num tokens (num_tokens=832 avail_mem=118.56 GB):  40%|███▉      | 23/58 [00:01<00:01, 26.38it/s]Capturing num tokens (num_tokens=768 avail_mem=118.55 GB):  40%|███▉      | 23/58 [00:01<00:01, 26.38it/s]Capturing num tokens (num_tokens=704 avail_mem=118.55 GB):  40%|███▉      | 23/58 [00:01<00:01, 26.38it/s]Capturing num tokens (num_tokens=640 avail_mem=118.54 GB):  40%|███▉      | 23/58 [00:01<00:01, 26.38it/s]Capturing num tokens (num_tokens=640 avail_mem=118.54 GB):  47%|████▋     | 27/58 [00:01<00:01, 27.98it/s]Capturing num tokens (num_tokens=576 avail_mem=118.53 GB):  47%|████▋     | 27/58 [00:01<00:01, 27.98it/s]Capturing num tokens (num_tokens=512 avail_mem=118.52 GB):  47%|████▋     | 27/58 [00:01<00:01, 27.98it/s]Capturing num tokens (num_tokens=480 avail_mem=118.53 GB):  47%|████▋     | 27/58 [00:01<00:01, 27.98it/s]

    Capturing num tokens (num_tokens=448 avail_mem=118.52 GB):  47%|████▋     | 27/58 [00:01<00:01, 27.98it/s]Capturing num tokens (num_tokens=448 avail_mem=118.52 GB):  53%|█████▎    | 31/58 [00:01<00:00, 29.49it/s]Capturing num tokens (num_tokens=416 avail_mem=118.54 GB):  53%|█████▎    | 31/58 [00:01<00:00, 29.49it/s]Capturing num tokens (num_tokens=384 avail_mem=118.53 GB):  53%|█████▎    | 31/58 [00:01<00:00, 29.49it/s]Capturing num tokens (num_tokens=352 avail_mem=118.52 GB):  53%|█████▎    | 31/58 [00:01<00:00, 29.49it/s]Capturing num tokens (num_tokens=320 avail_mem=118.52 GB):  53%|█████▎    | 31/58 [00:01<00:00, 29.49it/s]Capturing num tokens (num_tokens=320 avail_mem=118.52 GB):  60%|██████    | 35/58 [00:01<00:00, 30.70it/s]Capturing num tokens (num_tokens=288 avail_mem=118.51 GB):  60%|██████    | 35/58 [00:01<00:00, 30.70it/s]Capturing num tokens (num_tokens=256 avail_mem=118.50 GB):  60%|██████    | 35/58 [00:01<00:00, 30.70it/s]

    Capturing num tokens (num_tokens=240 avail_mem=118.50 GB):  60%|██████    | 35/58 [00:01<00:00, 30.70it/s]Capturing num tokens (num_tokens=224 avail_mem=118.47 GB):  60%|██████    | 35/58 [00:01<00:00, 30.70it/s]Capturing num tokens (num_tokens=224 avail_mem=118.47 GB):  67%|██████▋   | 39/58 [00:01<00:00, 30.36it/s]Capturing num tokens (num_tokens=208 avail_mem=118.49 GB):  67%|██████▋   | 39/58 [00:01<00:00, 30.36it/s]Capturing num tokens (num_tokens=192 avail_mem=118.48 GB):  67%|██████▋   | 39/58 [00:01<00:00, 30.36it/s]Capturing num tokens (num_tokens=176 avail_mem=118.47 GB):  67%|██████▋   | 39/58 [00:01<00:00, 30.36it/s]Capturing num tokens (num_tokens=160 avail_mem=118.47 GB):  67%|██████▋   | 39/58 [00:01<00:00, 30.36it/s]Capturing num tokens (num_tokens=160 avail_mem=118.47 GB):  74%|███████▍  | 43/58 [00:01<00:00, 32.08it/s]Capturing num tokens (num_tokens=144 avail_mem=118.46 GB):  74%|███████▍  | 43/58 [00:01<00:00, 32.08it/s]

    Capturing num tokens (num_tokens=128 avail_mem=118.45 GB):  74%|███████▍  | 43/58 [00:01<00:00, 32.08it/s]Capturing num tokens (num_tokens=112 avail_mem=118.45 GB):  74%|███████▍  | 43/58 [00:01<00:00, 32.08it/s]Capturing num tokens (num_tokens=96 avail_mem=118.44 GB):  74%|███████▍  | 43/58 [00:01<00:00, 32.08it/s] Capturing num tokens (num_tokens=96 avail_mem=118.44 GB):  81%|████████  | 47/58 [00:01<00:00, 32.55it/s]Capturing num tokens (num_tokens=80 avail_mem=118.44 GB):  81%|████████  | 47/58 [00:01<00:00, 32.55it/s]Capturing num tokens (num_tokens=64 avail_mem=118.43 GB):  81%|████████  | 47/58 [00:01<00:00, 32.55it/s]Capturing num tokens (num_tokens=48 avail_mem=118.42 GB):  81%|████████  | 47/58 [00:01<00:00, 32.55it/s]Capturing num tokens (num_tokens=32 avail_mem=118.41 GB):  81%|████████  | 47/58 [00:01<00:00, 32.55it/s]Capturing num tokens (num_tokens=32 avail_mem=118.41 GB):  88%|████████▊ | 51/58 [00:01<00:00, 33.71it/s]Capturing num tokens (num_tokens=28 avail_mem=118.41 GB):  88%|████████▊ | 51/58 [00:01<00:00, 33.71it/s]

    Capturing num tokens (num_tokens=24 avail_mem=118.40 GB):  88%|████████▊ | 51/58 [00:01<00:00, 33.71it/s]Capturing num tokens (num_tokens=20 avail_mem=118.39 GB):  88%|████████▊ | 51/58 [00:02<00:00, 33.71it/s]Capturing num tokens (num_tokens=16 avail_mem=118.39 GB):  88%|████████▊ | 51/58 [00:02<00:00, 33.71it/s]Capturing num tokens (num_tokens=16 avail_mem=118.39 GB):  95%|█████████▍| 55/58 [00:02<00:00, 33.72it/s]Capturing num tokens (num_tokens=12 avail_mem=118.38 GB):  95%|█████████▍| 55/58 [00:02<00:00, 33.72it/s]Capturing num tokens (num_tokens=8 avail_mem=118.38 GB):  95%|█████████▍| 55/58 [00:02<00:00, 33.72it/s] Capturing num tokens (num_tokens=4 avail_mem=118.37 GB):  95%|█████████▍| 55/58 [00:02<00:00, 33.72it/s]Capturing num tokens (num_tokens=4 avail_mem=118.37 GB): 100%|██████████| 58/58 [00:02<00:00, 26.73it/s]


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


<strong style='color: #00008B;'>{'id': '77732a3fcd834bf79fd8c4ba6599991e', 'object': 'chat.completion', 'created': 1774436659, 'model': 'qwen/qwen2.5-0.5b-instruct', 'choices': [{'index': 0, 'message': {'role': 'assistant', 'content': 'The capital of France is Paris.', 'reasoning_content': None, 'tool_calls': None}, 'logprobs': None, 'finish_reason': 'stop', 'matched_stop': 151645}], 'usage': {'prompt_tokens': 36, 'total_tokens': 44, 'completion_tokens': 8, 'prompt_tokens_details': None, 'reasoning_tokens': 0}, 'metadata': {'weight_version': 'default'}}</strong>


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


<strong style='color: #00008B;'>{'id': 'bdea11a239004b27a162340f03bb8560', 'object': 'chat.completion', 'created': 1774436659, 'model': 'qwen/qwen2.5-0.5b-instruct', 'choices': [{'index': 0, 'message': {'role': 'assistant', 'content': 'The capital of France is Paris.', 'reasoning_content': None, 'tool_calls': None}, 'logprobs': None, 'finish_reason': 'stop', 'matched_stop': 151645}], 'usage': {'prompt_tokens': 36, 'total_tokens': 44, 'completion_tokens': 8, 'prompt_tokens_details': None, 'reasoning_tokens': 0}, 'metadata': {'weight_version': 'default'}}</strong>


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


<strong style='color: #00008B;'>ChatCompletion(id='3e942395b0cc42f5bf5a70e17f48fb23', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='Sure, here are three countries and their respective capitals:\n\n1. **United States** - Washington, D.C.\n2. **Canada** - Ottawa\n3. **Australia** - Canberra', refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=None, reasoning_content=None), matched_stop=151645)], created=1774436660, model='qwen/qwen2.5-0.5b-instruct', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=39, prompt_tokens=37, total_tokens=76, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>


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


<strong style='color: #00008B;'>{'text': ' Paris. It is the largest city in Europe and the second largest city in the world. It is located in the south of France, on the banks of the', 'output_ids': [12095, 13, 1084, 374, 279, 7772, 3283, 304, 4505, 323, 279, 2086, 7772, 3283, 304, 279, 1879, 13, 1084, 374, 7407, 304, 279, 9806, 315, 9625, 11, 389, 279, 13959, 315, 279], 'meta_info': {'id': 'c994b12b241f4e6dad3235e297740d18', 'finish_reason': {'type': 'length', 'length': 32}, 'prompt_tokens': 5, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 32, 'cached_tokens': 2, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.24977578595280647, 'response_sent_to_client_ts': 1774436660.978554}}</strong>


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
