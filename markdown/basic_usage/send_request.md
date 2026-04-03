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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.00it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00,  5.00it/s]


    2026-04-03 21:06:46,163 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-03 21:06:46] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:16,  2.40s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:16,  2.40s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:16,  2.40s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:16,  2.40s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.08it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.08it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.08it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.08it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:25,  2.08it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:25,  2.08it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:25,  2.08it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.33it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.33it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.33it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.33it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.33it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.33it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.33it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.33it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.33it/s]Compiling num tokens (num_tokens=1536):  17%|█▋        | 10/58 [00:02<00:07,  6.33it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 14.15it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 14.15it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 14.15it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 14.15it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 14.15it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 14.15it/s]Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 14.15it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:02<00:02, 14.15it/s]

    Compiling num tokens (num_tokens=640):  33%|███▎      | 19/58 [00:02<00:02, 14.15it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:02<00:01, 21.73it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:02<00:01, 21.73it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:02<00:01, 21.73it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:02<00:01, 21.73it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:02<00:01, 21.73it/s]Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:02<00:01, 21.73it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:02<00:01, 21.73it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:02<00:01, 21.73it/s]Compiling num tokens (num_tokens=320):  47%|████▋     | 27/58 [00:02<00:01, 21.73it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:02<00:00, 29.78it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:02<00:00, 29.78it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:02<00:00, 29.78it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:02<00:00, 29.78it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:02<00:00, 29.78it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:03<00:00, 29.78it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:03<00:00, 29.78it/s]

    Compiling num tokens (num_tokens=176):  60%|██████    | 35/58 [00:03<00:00, 29.78it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:03<00:00, 36.39it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:03<00:00, 36.39it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:03<00:00, 36.39it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:03<00:00, 36.39it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:03<00:00, 36.39it/s]Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:03<00:00, 36.39it/s] Compiling num tokens (num_tokens=80):  72%|███████▏  | 42/58 [00:03<00:00, 36.39it/s]Compiling num tokens (num_tokens=64):  72%|███████▏  | 42/58 [00:03<00:00, 36.39it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 42.45it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 42.45it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 42.45it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 42.45it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 42.45it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 42.45it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 42.45it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 42.45it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:03<00:00, 42.45it/s] 

    Compiling num tokens (num_tokens=4):  84%|████████▍ | 49/58 [00:03<00:00, 42.45it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 17.92it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=43.64 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=43.61 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=43.61 GB):   3%|▎         | 2/58 [00:00<00:02, 19.55it/s]Capturing num tokens (num_tokens=7168 avail_mem=43.61 GB):   3%|▎         | 2/58 [00:00<00:02, 19.55it/s]Capturing num tokens (num_tokens=6656 avail_mem=43.61 GB):   3%|▎         | 2/58 [00:00<00:02, 19.55it/s]Capturing num tokens (num_tokens=6144 avail_mem=43.61 GB):   3%|▎         | 2/58 [00:00<00:02, 19.55it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=43.61 GB):   9%|▊         | 5/58 [00:00<00:02, 23.04it/s]Capturing num tokens (num_tokens=5632 avail_mem=43.60 GB):   9%|▊         | 5/58 [00:00<00:02, 23.04it/s]Capturing num tokens (num_tokens=5120 avail_mem=43.61 GB):   9%|▊         | 5/58 [00:00<00:02, 23.04it/s]Capturing num tokens (num_tokens=4608 avail_mem=43.60 GB):   9%|▊         | 5/58 [00:00<00:02, 23.04it/s]Capturing num tokens (num_tokens=4096 avail_mem=43.60 GB):   9%|▊         | 5/58 [00:00<00:02, 23.04it/s]Capturing num tokens (num_tokens=4096 avail_mem=43.60 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.93it/s]Capturing num tokens (num_tokens=3840 avail_mem=43.60 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.93it/s]Capturing num tokens (num_tokens=3584 avail_mem=43.59 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.93it/s]Capturing num tokens (num_tokens=3328 avail_mem=43.59 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.93it/s]Capturing num tokens (num_tokens=3072 avail_mem=43.59 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.93it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=43.59 GB):  16%|█▌        | 9/58 [00:00<00:01, 27.93it/s]Capturing num tokens (num_tokens=2816 avail_mem=43.59 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.13it/s]Capturing num tokens (num_tokens=2560 avail_mem=43.58 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.13it/s]Capturing num tokens (num_tokens=2304 avail_mem=43.58 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.13it/s]Capturing num tokens (num_tokens=2048 avail_mem=43.57 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.13it/s]Capturing num tokens (num_tokens=1792 avail_mem=43.57 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.13it/s]Capturing num tokens (num_tokens=1536 avail_mem=43.57 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.13it/s]Capturing num tokens (num_tokens=1280 avail_mem=43.56 GB):  24%|██▍       | 14/58 [00:00<00:01, 34.13it/s]Capturing num tokens (num_tokens=1280 avail_mem=43.56 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.41it/s]Capturing num tokens (num_tokens=1024 avail_mem=43.54 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.41it/s]Capturing num tokens (num_tokens=960 avail_mem=43.56 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.41it/s] Capturing num tokens (num_tokens=896 avail_mem=43.56 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.41it/s]Capturing num tokens (num_tokens=832 avail_mem=43.55 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.41it/s]

    Capturing num tokens (num_tokens=768 avail_mem=43.55 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.41it/s]Capturing num tokens (num_tokens=704 avail_mem=43.55 GB):  34%|███▍      | 20/58 [00:00<00:00, 40.41it/s]Capturing num tokens (num_tokens=704 avail_mem=43.55 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.64it/s]Capturing num tokens (num_tokens=640 avail_mem=43.54 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.64it/s]Capturing num tokens (num_tokens=576 avail_mem=43.54 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.64it/s]Capturing num tokens (num_tokens=512 avail_mem=43.53 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.64it/s]Capturing num tokens (num_tokens=480 avail_mem=43.55 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.64it/s]Capturing num tokens (num_tokens=448 avail_mem=43.54 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.64it/s]Capturing num tokens (num_tokens=416 avail_mem=43.54 GB):  45%|████▍     | 26/58 [00:00<00:00, 44.64it/s]Capturing num tokens (num_tokens=416 avail_mem=43.54 GB):  55%|█████▌    | 32/58 [00:00<00:00, 46.52it/s]Capturing num tokens (num_tokens=384 avail_mem=43.54 GB):  55%|█████▌    | 32/58 [00:00<00:00, 46.52it/s]Capturing num tokens (num_tokens=352 avail_mem=43.53 GB):  55%|█████▌    | 32/58 [00:00<00:00, 46.52it/s]Capturing num tokens (num_tokens=320 avail_mem=43.53 GB):  55%|█████▌    | 32/58 [00:00<00:00, 46.52it/s]

    Capturing num tokens (num_tokens=288 avail_mem=43.53 GB):  55%|█████▌    | 32/58 [00:00<00:00, 46.52it/s]Capturing num tokens (num_tokens=256 avail_mem=43.52 GB):  55%|█████▌    | 32/58 [00:00<00:00, 46.52it/s]Capturing num tokens (num_tokens=240 avail_mem=43.52 GB):  55%|█████▌    | 32/58 [00:00<00:00, 46.52it/s]Capturing num tokens (num_tokens=240 avail_mem=43.52 GB):  66%|██████▌   | 38/58 [00:00<00:00, 48.56it/s]Capturing num tokens (num_tokens=224 avail_mem=43.52 GB):  66%|██████▌   | 38/58 [00:00<00:00, 48.56it/s]Capturing num tokens (num_tokens=208 avail_mem=43.51 GB):  66%|██████▌   | 38/58 [00:00<00:00, 48.56it/s]Capturing num tokens (num_tokens=192 avail_mem=43.51 GB):  66%|██████▌   | 38/58 [00:00<00:00, 48.56it/s]Capturing num tokens (num_tokens=176 avail_mem=43.51 GB):  66%|██████▌   | 38/58 [00:00<00:00, 48.56it/s]Capturing num tokens (num_tokens=160 avail_mem=43.51 GB):  66%|██████▌   | 38/58 [00:00<00:00, 48.56it/s]Capturing num tokens (num_tokens=144 avail_mem=43.50 GB):  66%|██████▌   | 38/58 [00:01<00:00, 48.56it/s]Capturing num tokens (num_tokens=144 avail_mem=43.50 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.10it/s]Capturing num tokens (num_tokens=128 avail_mem=43.50 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.10it/s]Capturing num tokens (num_tokens=112 avail_mem=43.50 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.10it/s]

    Capturing num tokens (num_tokens=96 avail_mem=43.49 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.10it/s] Capturing num tokens (num_tokens=80 avail_mem=43.49 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.10it/s]Capturing num tokens (num_tokens=64 avail_mem=43.49 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.10it/s]Capturing num tokens (num_tokens=48 avail_mem=43.48 GB):  76%|███████▌  | 44/58 [00:01<00:00, 50.10it/s]Capturing num tokens (num_tokens=48 avail_mem=43.48 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.66it/s]Capturing num tokens (num_tokens=32 avail_mem=43.48 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.66it/s]Capturing num tokens (num_tokens=28 avail_mem=43.48 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.66it/s]Capturing num tokens (num_tokens=24 avail_mem=43.47 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.66it/s]Capturing num tokens (num_tokens=20 avail_mem=43.47 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.66it/s]Capturing num tokens (num_tokens=16 avail_mem=43.47 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.66it/s]Capturing num tokens (num_tokens=12 avail_mem=43.46 GB):  86%|████████▌ | 50/58 [00:01<00:00, 50.66it/s]Capturing num tokens (num_tokens=12 avail_mem=43.46 GB):  97%|█████████▋| 56/58 [00:01<00:00, 51.37it/s]Capturing num tokens (num_tokens=8 avail_mem=43.46 GB):  97%|█████████▋| 56/58 [00:01<00:00, 51.37it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=43.46 GB):  97%|█████████▋| 56/58 [00:01<00:00, 51.37it/s]Capturing num tokens (num_tokens=4 avail_mem=43.46 GB): 100%|██████████| 58/58 [00:01<00:00, 44.70it/s]


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


<strong style='color: #00008B;'>{'id': '856582af5b8046a1b93a7683c7a4f079', 'object': 'chat.completion', 'created': 1775250418, 'model': 'qwen/qwen2.5-0.5b-instruct', 'choices': [{'index': 0, 'message': {'role': 'assistant', 'content': 'The capital of France is Paris.', 'reasoning_content': None, 'tool_calls': None}, 'logprobs': None, 'finish_reason': 'stop', 'matched_stop': 151645}], 'usage': {'prompt_tokens': 36, 'total_tokens': 44, 'completion_tokens': 8, 'prompt_tokens_details': None, 'reasoning_tokens': 0}, 'metadata': {'weight_version': 'default'}}</strong>


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


<strong style='color: #00008B;'>{'id': '5552b4f910cd43fdaaa38deb76cd6648', 'object': 'chat.completion', 'created': 1775250418, 'model': 'qwen/qwen2.5-0.5b-instruct', 'choices': [{'index': 0, 'message': {'role': 'assistant', 'content': 'The capital of France is Paris.', 'reasoning_content': None, 'tool_calls': None}, 'logprobs': None, 'finish_reason': 'stop', 'matched_stop': 151645}], 'usage': {'prompt_tokens': 36, 'total_tokens': 44, 'completion_tokens': 8, 'prompt_tokens_details': None, 'reasoning_tokens': 0}, 'metadata': {'weight_version': 'default'}}</strong>


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


<strong style='color: #00008B;'>ChatCompletion(id='e0969fe509c1469da62a8735211f1369', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='Sure, here are three countries and their respective capitals:\n\n1. **United States** - Washington D.C.\n2. **Canada** - Ottawa\n3. **Australia** - Canberra', refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=None, reasoning_content=None), matched_stop=151645)], created=1775250418, model='qwen/qwen2.5-0.5b-instruct', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=38, prompt_tokens=37, total_tokens=75, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>


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


<strong style='color: #00008B;'>{'text': ' Paris. It is the largest city in Europe and the third largest in the world. It is located in the south of France, on the banks of the Se', 'output_ids': [12095, 13, 1084, 374, 279, 7772, 3283, 304, 4505, 323, 279, 4843, 7772, 304, 279, 1879, 13, 1084, 374, 7407, 304, 279, 9806, 315, 9625, 11, 389, 279, 13959, 315, 279, 1345], 'meta_info': {'id': 'a99e7ca215b14f39a4f3b298c28c177f', 'finish_reason': {'type': 'length', 'length': 32}, 'prompt_tokens': 5, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 32, 'cached_tokens': 2, 'cached_tokens_details': {'device': 2, 'host': 0}, 'dp_rank': None, 'e2e_latency': 0.1870074300095439, 'response_sent_to_client_ts': 1775250419.4197772}}</strong>


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

     third

     largest

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

     Se


```python
terminate_process(server_process)
```
