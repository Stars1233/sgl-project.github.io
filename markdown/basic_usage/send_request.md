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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:173: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-04-01 15:56:26.303 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:56:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 15:56:26.303 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:56:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 15:56:26.303 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:56:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 15:56:26.303 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:56:26] Persistent cache disabled, using in-memory JIT cache
    2026-04-01 15:56:26.304 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-01 15:56:26] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.58it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.58it/s]


    2026-04-01 15:56:28,779 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-01 15:56:28] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:47,  2.94s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:47,  2.94s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:47,  2.94s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:03<02:47,  2.94s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:31,  1.70it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:31,  1.70it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:31,  1.70it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:03<00:31,  1.70it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:03<00:31,  1.70it/s]

    Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:03<00:31,  1.70it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:10,  4.63it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:10,  4.63it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:10,  4.63it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:10,  4.63it/s]Compiling num tokens (num_tokens=3072):  16%|█▌        | 9/58 [00:03<00:10,  4.63it/s]Compiling num tokens (num_tokens=2816):  16%|█▌        | 9/58 [00:03<00:10,  4.63it/s]Compiling num tokens (num_tokens=2560):  16%|█▌        | 9/58 [00:03<00:10,  4.63it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:03<00:04,  9.03it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:03<00:04,  9.03it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:03<00:04,  9.03it/s]Compiling num tokens (num_tokens=1792):  26%|██▌       | 15/58 [00:03<00:04,  9.03it/s]Compiling num tokens (num_tokens=1536):  26%|██▌       | 15/58 [00:03<00:04,  9.03it/s]Compiling num tokens (num_tokens=1280):  26%|██▌       | 15/58 [00:03<00:04,  9.03it/s]

    Compiling num tokens (num_tokens=1024):  26%|██▌       | 15/58 [00:03<00:04,  9.03it/s]Compiling num tokens (num_tokens=960):  26%|██▌       | 15/58 [00:03<00:04,  9.03it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:02, 15.06it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:02, 15.06it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:02, 15.06it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:02, 15.06it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:02, 15.06it/s]Compiling num tokens (num_tokens=640):  38%|███▊      | 22/58 [00:03<00:02, 15.06it/s]Compiling num tokens (num_tokens=576):  38%|███▊      | 22/58 [00:03<00:02, 15.06it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:03<00:01, 20.62it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:03<00:01, 20.62it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:03<00:01, 20.62it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:03<00:01, 20.62it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:03<00:01, 20.62it/s]

    Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:03<00:01, 20.62it/s]Compiling num tokens (num_tokens=352):  48%|████▊     | 28/58 [00:03<00:01, 20.62it/s]Compiling num tokens (num_tokens=320):  48%|████▊     | 28/58 [00:03<00:01, 20.62it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:03<00:00, 27.32it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:03<00:00, 27.32it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:03<00:00, 27.32it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:03<00:00, 27.32it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:03<00:00, 27.32it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:03<00:00, 27.32it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:03<00:00, 27.32it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:03<00:00, 31.65it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:03<00:00, 31.65it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:03<00:00, 31.65it/s]

    Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:03<00:00, 31.65it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:03<00:00, 31.65it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:03<00:00, 31.65it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:03<00:00, 31.65it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:03<00:00, 35.39it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:03<00:00, 35.39it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:03<00:00, 35.39it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:03<00:00, 35.39it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:03<00:00, 35.39it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:03<00:00, 35.39it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:03<00:00, 35.39it/s]Compiling num tokens (num_tokens=20):  81%|████████  | 47/58 [00:03<00:00, 35.39it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:03<00:00, 41.73it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:03<00:00, 41.73it/s]

    Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:03<00:00, 41.73it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:04<00:00, 41.73it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:04<00:00, 41.73it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 14.41it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.56 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.53 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.53 GB):   3%|▎         | 2/58 [00:00<00:03, 18.28it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.52 GB):   3%|▎         | 2/58 [00:00<00:03, 18.28it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.52 GB):   3%|▎         | 2/58 [00:00<00:03, 18.28it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.52 GB):   3%|▎         | 2/58 [00:00<00:03, 18.28it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=118.52 GB):   9%|▊         | 5/58 [00:00<00:02, 21.43it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.52 GB):   9%|▊         | 5/58 [00:00<00:02, 21.43it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.52 GB):   9%|▊         | 5/58 [00:00<00:02, 21.43it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.52 GB):   9%|▊         | 5/58 [00:00<00:02, 21.43it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.52 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.85it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.52 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.85it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.51 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.85it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.51 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.85it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.51 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.85it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=118.51 GB):  21%|██        | 12/58 [00:00<00:01, 29.58it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.50 GB):  21%|██        | 12/58 [00:00<00:01, 29.58it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.50 GB):  21%|██        | 12/58 [00:00<00:01, 29.58it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.50 GB):  21%|██        | 12/58 [00:00<00:01, 29.58it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.49 GB):  21%|██        | 12/58 [00:00<00:01, 29.58it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.49 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.60it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.49 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.60it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.49 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.60it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.48 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.60it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.48 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.60it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=118.46 GB):  28%|██▊       | 16/58 [00:00<00:01, 32.60it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.46 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.29it/s]Capturing num tokens (num_tokens=960 avail_mem=118.47 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.29it/s] Capturing num tokens (num_tokens=896 avail_mem=118.47 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.29it/s]Capturing num tokens (num_tokens=832 avail_mem=118.47 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.29it/s]Capturing num tokens (num_tokens=768 avail_mem=118.46 GB):  36%|███▌      | 21/58 [00:00<00:01, 35.29it/s]

    Capturing num tokens (num_tokens=768 avail_mem=118.46 GB):  43%|████▎     | 25/58 [00:00<00:01, 29.49it/s]Capturing num tokens (num_tokens=704 avail_mem=118.46 GB):  43%|████▎     | 25/58 [00:00<00:01, 29.49it/s]Capturing num tokens (num_tokens=640 avail_mem=118.46 GB):  43%|████▎     | 25/58 [00:00<00:01, 29.49it/s]Capturing num tokens (num_tokens=576 avail_mem=118.46 GB):  43%|████▎     | 25/58 [00:00<00:01, 29.49it/s]Capturing num tokens (num_tokens=512 avail_mem=118.44 GB):  43%|████▎     | 25/58 [00:00<00:01, 29.49it/s]Capturing num tokens (num_tokens=512 avail_mem=118.44 GB):  50%|█████     | 29/58 [00:00<00:00, 31.60it/s]Capturing num tokens (num_tokens=480 avail_mem=118.46 GB):  50%|█████     | 29/58 [00:00<00:00, 31.60it/s]Capturing num tokens (num_tokens=448 avail_mem=118.46 GB):  50%|█████     | 29/58 [00:00<00:00, 31.60it/s]Capturing num tokens (num_tokens=416 avail_mem=118.46 GB):  50%|█████     | 29/58 [00:01<00:00, 31.60it/s]Capturing num tokens (num_tokens=384 avail_mem=118.45 GB):  50%|█████     | 29/58 [00:01<00:00, 31.60it/s]Capturing num tokens (num_tokens=352 avail_mem=118.45 GB):  50%|█████     | 29/58 [00:01<00:00, 31.60it/s]

    Capturing num tokens (num_tokens=352 avail_mem=118.45 GB):  59%|█████▊    | 34/58 [00:01<00:00, 35.18it/s]Capturing num tokens (num_tokens=320 avail_mem=118.44 GB):  59%|█████▊    | 34/58 [00:01<00:00, 35.18it/s]Capturing num tokens (num_tokens=288 avail_mem=118.44 GB):  59%|█████▊    | 34/58 [00:01<00:00, 35.18it/s]Capturing num tokens (num_tokens=256 avail_mem=118.44 GB):  59%|█████▊    | 34/58 [00:01<00:00, 35.18it/s]Capturing num tokens (num_tokens=240 avail_mem=118.44 GB):  59%|█████▊    | 34/58 [00:01<00:00, 35.18it/s]Capturing num tokens (num_tokens=224 avail_mem=118.43 GB):  59%|█████▊    | 34/58 [00:01<00:00, 35.18it/s]Capturing num tokens (num_tokens=224 avail_mem=118.43 GB):  67%|██████▋   | 39/58 [00:01<00:00, 36.09it/s]Capturing num tokens (num_tokens=208 avail_mem=118.43 GB):  67%|██████▋   | 39/58 [00:01<00:00, 36.09it/s]Capturing num tokens (num_tokens=192 avail_mem=118.43 GB):  67%|██████▋   | 39/58 [00:01<00:00, 36.09it/s]Capturing num tokens (num_tokens=176 avail_mem=118.42 GB):  67%|██████▋   | 39/58 [00:01<00:00, 36.09it/s]Capturing num tokens (num_tokens=160 avail_mem=118.42 GB):  67%|██████▋   | 39/58 [00:01<00:00, 36.09it/s]

    Capturing num tokens (num_tokens=144 avail_mem=118.42 GB):  67%|██████▋   | 39/58 [00:01<00:00, 36.09it/s]Capturing num tokens (num_tokens=144 avail_mem=118.42 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.85it/s]Capturing num tokens (num_tokens=128 avail_mem=118.41 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.85it/s]Capturing num tokens (num_tokens=112 avail_mem=118.41 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.85it/s]Capturing num tokens (num_tokens=96 avail_mem=118.41 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.85it/s] Capturing num tokens (num_tokens=80 avail_mem=118.40 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.85it/s]Capturing num tokens (num_tokens=64 avail_mem=118.40 GB):  76%|███████▌  | 44/58 [00:01<00:00, 37.85it/s]Capturing num tokens (num_tokens=64 avail_mem=118.40 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.48it/s]Capturing num tokens (num_tokens=48 avail_mem=118.40 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.48it/s]Capturing num tokens (num_tokens=32 avail_mem=118.39 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.48it/s]Capturing num tokens (num_tokens=28 avail_mem=118.39 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.48it/s]

    Capturing num tokens (num_tokens=24 avail_mem=118.39 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.48it/s]Capturing num tokens (num_tokens=20 avail_mem=118.38 GB):  84%|████████▍ | 49/58 [00:01<00:00, 39.48it/s]Capturing num tokens (num_tokens=20 avail_mem=118.38 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.89it/s]Capturing num tokens (num_tokens=16 avail_mem=118.38 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.89it/s]Capturing num tokens (num_tokens=12 avail_mem=118.38 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.89it/s]Capturing num tokens (num_tokens=8 avail_mem=118.37 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.89it/s] Capturing num tokens (num_tokens=4 avail_mem=118.37 GB):  93%|█████████▎| 54/58 [00:01<00:00, 40.89it/s]Capturing num tokens (num_tokens=4 avail_mem=118.37 GB): 100%|██████████| 58/58 [00:01<00:00, 35.01it/s]


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


<strong style='color: #00008B;'>{'id': '6c991442cab24a2f8bdc476d98a83ab1', 'object': 'chat.completion', 'created': 1775059003, 'model': 'qwen/qwen2.5-0.5b-instruct', 'choices': [{'index': 0, 'message': {'role': 'assistant', 'content': 'The capital of France is Paris.', 'reasoning_content': None, 'tool_calls': None}, 'logprobs': None, 'finish_reason': 'stop', 'matched_stop': 151645}], 'usage': {'prompt_tokens': 36, 'total_tokens': 44, 'completion_tokens': 8, 'prompt_tokens_details': None, 'reasoning_tokens': 0}, 'metadata': {'weight_version': 'default'}}</strong>


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


<strong style='color: #00008B;'>{'id': 'de172d5876de4436b6106a5ec1c3de9d', 'object': 'chat.completion', 'created': 1775059003, 'model': 'qwen/qwen2.5-0.5b-instruct', 'choices': [{'index': 0, 'message': {'role': 'assistant', 'content': 'The capital of France is Paris.', 'reasoning_content': None, 'tool_calls': None}, 'logprobs': None, 'finish_reason': 'stop', 'matched_stop': 151645}], 'usage': {'prompt_tokens': 36, 'total_tokens': 44, 'completion_tokens': 8, 'prompt_tokens_details': None, 'reasoning_tokens': 0}, 'metadata': {'weight_version': 'default'}}</strong>


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


<strong style='color: #00008B;'>ChatCompletion(id='d2aa8fdd6a85488280b1f3d7b6b23da2', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='Sure, here are three countries and their respective capitals:\n\n1. **United States** - Washington D.C.\n2. **Canada** - Ottawa\n3. **Australia** - Canberra', refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=None, reasoning_content=None), matched_stop=151645)], created=1775059004, model='qwen/qwen2.5-0.5b-instruct', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=38, prompt_tokens=37, total_tokens=75, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>


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


<strong style='color: #00008B;'>{'text': ' Paris. It is the largest city in Europe and the second largest city in the world. It is located in the south of France, on the banks of the', 'output_ids': [12095, 13, 1084, 374, 279, 7772, 3283, 304, 4505, 323, 279, 2086, 7772, 3283, 304, 279, 1879, 13, 1084, 374, 7407, 304, 279, 9806, 315, 9625, 11, 389, 279, 13959, 315, 279], 'meta_info': {'id': 'a2916399ceb64dee9995cc4658fa3de0', 'finish_reason': {'type': 'length', 'length': 32}, 'prompt_tokens': 5, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 32, 'cached_tokens': 2, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.2593229808844626, 'response_sent_to_client_ts': 1775059005.14701}}</strong>


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

     in the

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
