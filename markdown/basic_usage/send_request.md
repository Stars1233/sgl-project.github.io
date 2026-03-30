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
    2026-03-30 11:51:41.776 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 11:51:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 11:51:41.776 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 11:51:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 11:51:41.776 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 11:51:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 11:51:41.776 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 11:51:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-30 11:51:41.776 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-30 11:51:41] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.16it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.15it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:07,  2.24s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:07,  2.24s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:07,  2.24s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:07,  2.24s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.70it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.70it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.70it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.70it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.70it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.70it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.70it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.70it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.70it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:02, 14.04it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:02, 14.04it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:02, 14.04it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:02, 14.04it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:02, 14.04it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:02, 14.04it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:02, 14.04it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:02, 14.04it/s]Compiling num tokens (num_tokens=704):  31%|███       | 18/58 [00:02<00:02, 14.04it/s]

    Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:02<00:01, 22.07it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:02<00:01, 22.07it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:02<00:01, 22.07it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:02<00:01, 22.07it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:02<00:01, 22.07it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:02<00:01, 22.07it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:02<00:01, 22.07it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:02<00:01, 22.07it/s]Compiling num tokens (num_tokens=352):  45%|████▍     | 26/58 [00:02<00:01, 22.07it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:02<00:00, 30.19it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:02<00:00, 30.19it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:02<00:00, 30.19it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:02<00:00, 30.19it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:02<00:00, 30.19it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:02<00:00, 30.19it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:02<00:00, 30.19it/s]

    Compiling num tokens (num_tokens=192):  59%|█████▊    | 34/58 [00:02<00:00, 30.19it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:02<00:00, 36.55it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:02<00:00, 36.55it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:02<00:00, 36.55it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:02<00:00, 36.55it/s]Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:02<00:00, 36.55it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:02<00:00, 36.55it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:02<00:00, 36.55it/s] Compiling num tokens (num_tokens=80):  71%|███████   | 41/58 [00:02<00:00, 36.55it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:02<00:00, 42.34it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:02<00:00, 42.34it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:03<00:00, 42.34it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:03<00:00, 42.34it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:03<00:00, 42.34it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:03<00:00, 42.34it/s]Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:03<00:00, 42.34it/s]Compiling num tokens (num_tokens=16):  83%|████████▎ | 48/58 [00:03<00:00, 42.34it/s]

    Compiling num tokens (num_tokens=12):  83%|████████▎ | 48/58 [00:03<00:00, 42.34it/s]Compiling num tokens (num_tokens=8):  83%|████████▎ | 48/58 [00:03<00:00, 42.34it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:03<00:00, 52.40it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:03<00:00, 52.40it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 18.68it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=59.17 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=59.14 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=59.14 GB):   3%|▎         | 2/58 [00:00<00:02, 19.27it/s]Capturing num tokens (num_tokens=7168 avail_mem=59.13 GB):   3%|▎         | 2/58 [00:00<00:02, 19.27it/s]Capturing num tokens (num_tokens=6656 avail_mem=59.13 GB):   3%|▎         | 2/58 [00:00<00:02, 19.27it/s]Capturing num tokens (num_tokens=6144 avail_mem=59.13 GB):   3%|▎         | 2/58 [00:00<00:02, 19.27it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=59.13 GB):   9%|▊         | 5/58 [00:00<00:02, 22.29it/s]Capturing num tokens (num_tokens=5632 avail_mem=59.13 GB):   9%|▊         | 5/58 [00:00<00:02, 22.29it/s]Capturing num tokens (num_tokens=5120 avail_mem=59.13 GB):   9%|▊         | 5/58 [00:00<00:02, 22.29it/s]Capturing num tokens (num_tokens=4608 avail_mem=59.13 GB):   9%|▊         | 5/58 [00:00<00:02, 22.29it/s]Capturing num tokens (num_tokens=4096 avail_mem=59.13 GB):   9%|▊         | 5/58 [00:00<00:02, 22.29it/s]Capturing num tokens (num_tokens=4096 avail_mem=59.13 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.33it/s]Capturing num tokens (num_tokens=3840 avail_mem=59.12 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.33it/s]Capturing num tokens (num_tokens=3584 avail_mem=59.12 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.33it/s]Capturing num tokens (num_tokens=3328 avail_mem=59.11 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.33it/s]Capturing num tokens (num_tokens=3072 avail_mem=59.11 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.33it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=59.11 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.33it/s]Capturing num tokens (num_tokens=2816 avail_mem=59.11 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.27it/s]Capturing num tokens (num_tokens=2560 avail_mem=59.10 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.27it/s]Capturing num tokens (num_tokens=2304 avail_mem=59.10 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.27it/s]Capturing num tokens (num_tokens=2048 avail_mem=59.10 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.27it/s]Capturing num tokens (num_tokens=1792 avail_mem=59.09 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.27it/s]Capturing num tokens (num_tokens=1536 avail_mem=59.09 GB):  24%|██▍       | 14/58 [00:00<00:01, 33.27it/s]Capturing num tokens (num_tokens=1536 avail_mem=59.09 GB):  33%|███▎      | 19/58 [00:00<00:01, 38.54it/s]Capturing num tokens (num_tokens=1280 avail_mem=59.09 GB):  33%|███▎      | 19/58 [00:00<00:01, 38.54it/s]Capturing num tokens (num_tokens=1024 avail_mem=59.07 GB):  33%|███▎      | 19/58 [00:00<00:01, 38.54it/s]Capturing num tokens (num_tokens=960 avail_mem=59.08 GB):  33%|███▎      | 19/58 [00:00<00:01, 38.54it/s] Capturing num tokens (num_tokens=896 avail_mem=59.08 GB):  33%|███▎      | 19/58 [00:00<00:01, 38.54it/s]

    Capturing num tokens (num_tokens=832 avail_mem=59.08 GB):  33%|███▎      | 19/58 [00:00<00:01, 38.54it/s]Capturing num tokens (num_tokens=768 avail_mem=59.07 GB):  33%|███▎      | 19/58 [00:00<00:01, 38.54it/s]Capturing num tokens (num_tokens=768 avail_mem=59.07 GB):  43%|████▎     | 25/58 [00:00<00:00, 43.02it/s]Capturing num tokens (num_tokens=704 avail_mem=59.07 GB):  43%|████▎     | 25/58 [00:00<00:00, 43.02it/s]Capturing num tokens (num_tokens=640 avail_mem=59.07 GB):  43%|████▎     | 25/58 [00:00<00:00, 43.02it/s]Capturing num tokens (num_tokens=576 avail_mem=59.07 GB):  43%|████▎     | 25/58 [00:00<00:00, 43.02it/s]Capturing num tokens (num_tokens=512 avail_mem=59.05 GB):  43%|████▎     | 25/58 [00:00<00:00, 43.02it/s]Capturing num tokens (num_tokens=480 avail_mem=59.07 GB):  43%|████▎     | 25/58 [00:00<00:00, 43.02it/s]Capturing num tokens (num_tokens=448 avail_mem=59.07 GB):  43%|████▎     | 25/58 [00:00<00:00, 43.02it/s]Capturing num tokens (num_tokens=448 avail_mem=59.07 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.79it/s]Capturing num tokens (num_tokens=416 avail_mem=59.07 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.79it/s]Capturing num tokens (num_tokens=384 avail_mem=59.06 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.79it/s]Capturing num tokens (num_tokens=352 avail_mem=59.06 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.79it/s]

    Capturing num tokens (num_tokens=320 avail_mem=59.05 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.79it/s]Capturing num tokens (num_tokens=288 avail_mem=59.05 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.79it/s]Capturing num tokens (num_tokens=256 avail_mem=59.05 GB):  53%|█████▎    | 31/58 [00:00<00:00, 45.79it/s]Capturing num tokens (num_tokens=256 avail_mem=59.05 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.95it/s]Capturing num tokens (num_tokens=240 avail_mem=59.05 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.95it/s]Capturing num tokens (num_tokens=224 avail_mem=59.04 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.95it/s]Capturing num tokens (num_tokens=208 avail_mem=59.02 GB):  64%|██████▍   | 37/58 [00:00<00:00, 47.95it/s]Capturing num tokens (num_tokens=192 avail_mem=59.02 GB):  64%|██████▍   | 37/58 [00:01<00:00, 47.95it/s]Capturing num tokens (num_tokens=176 avail_mem=59.02 GB):  64%|██████▍   | 37/58 [00:01<00:00, 47.95it/s]

    Capturing num tokens (num_tokens=176 avail_mem=59.02 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.43it/s]Capturing num tokens (num_tokens=160 avail_mem=59.01 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.43it/s]Capturing num tokens (num_tokens=144 avail_mem=59.01 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.43it/s]Capturing num tokens (num_tokens=128 avail_mem=58.99 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.43it/s]Capturing num tokens (num_tokens=112 avail_mem=58.51 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.43it/s]Capturing num tokens (num_tokens=96 avail_mem=58.41 GB):  72%|███████▏  | 42/58 [00:01<00:00, 41.43it/s] Capturing num tokens (num_tokens=96 avail_mem=58.41 GB):  81%|████████  | 47/58 [00:01<00:00, 40.10it/s]Capturing num tokens (num_tokens=80 avail_mem=58.34 GB):  81%|████████  | 47/58 [00:01<00:00, 40.10it/s]Capturing num tokens (num_tokens=64 avail_mem=58.34 GB):  81%|████████  | 47/58 [00:01<00:00, 40.10it/s]Capturing num tokens (num_tokens=48 avail_mem=58.34 GB):  81%|████████  | 47/58 [00:01<00:00, 40.10it/s]Capturing num tokens (num_tokens=32 avail_mem=58.33 GB):  81%|████████  | 47/58 [00:01<00:00, 40.10it/s]

    Capturing num tokens (num_tokens=28 avail_mem=58.33 GB):  81%|████████  | 47/58 [00:01<00:00, 40.10it/s]Capturing num tokens (num_tokens=24 avail_mem=58.32 GB):  81%|████████  | 47/58 [00:01<00:00, 40.10it/s]Capturing num tokens (num_tokens=24 avail_mem=58.32 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.33it/s]Capturing num tokens (num_tokens=20 avail_mem=58.32 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.33it/s]Capturing num tokens (num_tokens=16 avail_mem=58.32 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.33it/s]Capturing num tokens (num_tokens=12 avail_mem=58.32 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.33it/s]Capturing num tokens (num_tokens=8 avail_mem=58.31 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.33it/s] Capturing num tokens (num_tokens=4 avail_mem=58.31 GB):  91%|█████████▏| 53/58 [00:01<00:00, 43.33it/s]Capturing num tokens (num_tokens=4 avail_mem=58.31 GB): 100%|██████████| 58/58 [00:01<00:00, 40.81it/s]


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


<strong style='color: #00008B;'>{'id': '9c0d021f8fe24585a7b8f7507e72f197', 'object': 'chat.completion', 'created': 1774871516, 'model': 'qwen/qwen2.5-0.5b-instruct', 'choices': [{'index': 0, 'message': {'role': 'assistant', 'content': 'The capital of France is Paris.', 'reasoning_content': None, 'tool_calls': None}, 'logprobs': None, 'finish_reason': 'stop', 'matched_stop': 151645}], 'usage': {'prompt_tokens': 36, 'total_tokens': 44, 'completion_tokens': 8, 'prompt_tokens_details': None, 'reasoning_tokens': 0}, 'metadata': {'weight_version': 'default'}}</strong>


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


<strong style='color: #00008B;'>{'id': 'e67f119fd06e48f3a914c9eb28122317', 'object': 'chat.completion', 'created': 1774871516, 'model': 'qwen/qwen2.5-0.5b-instruct', 'choices': [{'index': 0, 'message': {'role': 'assistant', 'content': 'The capital of France is Paris.', 'reasoning_content': None, 'tool_calls': None}, 'logprobs': None, 'finish_reason': 'stop', 'matched_stop': 151645}], 'usage': {'prompt_tokens': 36, 'total_tokens': 44, 'completion_tokens': 8, 'prompt_tokens_details': None, 'reasoning_tokens': 0}, 'metadata': {'weight_version': 'default'}}</strong>


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


<strong style='color: #00008B;'>ChatCompletion(id='4e0ae71386d547efabe6062a6855039c', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='Sure, here are three countries and their capitals:\n\n1. **United States** - Washington, D.C.\n2. **Canada** - Ottawa\n3. **Australia** - Canberra', refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=None, reasoning_content=None), matched_stop=151645)], created=1774871517, model='qwen/qwen2.5-0.5b-instruct', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=38, prompt_tokens=37, total_tokens=75, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>


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


<strong style='color: #00008B;'>{'text': ' Paris. It is the largest city in Europe and the second largest city in the world. It is located in the south of France, on the banks of the', 'output_ids': [12095, 13, 1084, 374, 279, 7772, 3283, 304, 4505, 323, 279, 2086, 7772, 3283, 304, 279, 1879, 13, 1084, 374, 7407, 304, 279, 9806, 315, 9625, 11, 389, 279, 13959, 315, 279], 'meta_info': {'id': '116b29b6989e45c0bdbacdbbfbb143f3', 'finish_reason': {'type': 'length', 'length': 32}, 'prompt_tokens': 5, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 32, 'cached_tokens': 2, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 0.1922815249999985, 'response_sent_to_client_ts': 1774871517.9867964}}</strong>


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
