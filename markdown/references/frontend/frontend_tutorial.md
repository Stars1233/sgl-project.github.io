# SGLang Frontend Language

SGLang frontend language can be used to define simple and easy prompts in a convenient, structured way.

## Launch A Server

Launch the server in your terminal and wait for it to initialize.


```python
from sglang import assistant_begin, assistant_end
from sglang import assistant, function, gen, system, user
from sglang import image
from sglang import RuntimeEndpoint
from sglang.lang.api import set_default_backend
from sglang.srt.utils import load_image
from sglang.test.doc_patch import launch_server_cmd
from sglang.utils import print_highlight, terminate_process, wait_for_server

server_process, port = launch_server_cmd(
    "python -m sglang.launch_server --model-path Qwen/Qwen2.5-7B-Instruct --host 0.0.0.0 --log-level warning"
)

wait_for_server(f"http://localhost:{port}", process=server_process)
print(f"Server started on http://localhost:{port}")
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-27 22:45:41.138 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:45:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 22:45:41.138 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:45:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 22:45:41.138 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:45:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 22:45:41.139 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:45:41] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 22:45:41.139 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:45:41] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:01,  1.55it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.35it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:02<00:00,  1.34it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.32it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.34it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:39,  2.80s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:39,  2.80s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:18,  1.39s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:18,  1.39s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:46,  1.18it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:46,  1.18it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:31,  1.71it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:31,  1.71it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:22,  2.32it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:22,  2.32it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:03<00:17,  2.97it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:03<00:17,  2.97it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:13,  3.70it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:13,  3.70it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.48it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.48it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.37it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.37it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:04<00:07,  6.25it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:04<00:07,  6.25it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:04<00:07,  6.25it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:04<00:05,  7.87it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:04<00:05,  7.87it/s]

    Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:04<00:05,  7.87it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:04<00:04,  9.13it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:04<00:04,  9.13it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:04<00:04,  9.13it/s]

    Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:04<00:03, 10.94it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:04<00:03, 10.94it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:04<00:03, 10.94it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:04<00:03, 10.94it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:04<00:02, 14.01it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:04<00:02, 14.01it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:05<00:02, 14.01it/s]

    Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:05<00:02, 14.01it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:05<00:02, 14.01it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:05<00:01, 18.82it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:05<00:01, 18.82it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:05<00:01, 18.82it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:05<00:01, 18.82it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:05<00:01, 18.82it/s]Compiling num tokens (num_tokens=576):  40%|███▉      | 23/58 [00:05<00:01, 18.82it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:05<00:01, 25.38it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:05<00:01, 25.38it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:05<00:01, 25.38it/s]

    Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:05<00:01, 25.38it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:05<00:01, 25.38it/s]Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:05<00:01, 25.38it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:05<00:00, 31.31it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:05<00:00, 31.31it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:05<00:00, 31.31it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:05<00:00, 31.31it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:05<00:00, 31.31it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:05<00:00, 31.31it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:05<00:00, 31.31it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:05<00:00, 38.13it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:05<00:00, 38.13it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:05<00:00, 38.13it/s]

    Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:05<00:00, 38.13it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:05<00:00, 38.13it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:05<00:00, 38.13it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:05<00:00, 38.13it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:05<00:00, 42.87it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:05<00:00, 42.87it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:05<00:00, 42.87it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:05<00:00, 42.87it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:05<00:00, 42.87it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:05<00:00, 42.87it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:05<00:00, 42.87it/s]Compiling num tokens (num_tokens=28):  78%|███████▊  | 45/58 [00:05<00:00, 42.87it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:05<00:00, 48.63it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:05<00:00, 48.63it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:05<00:00, 48.63it/s]

    Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:05<00:00, 48.63it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:05<00:00, 48.63it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:05<00:00, 48.63it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:05<00:00, 48.63it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:05<00:00, 10.18it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=44.22 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=44.22 GB):   2%|▏         | 1/58 [00:00<00:19,  2.99it/s]Capturing num tokens (num_tokens=7680 avail_mem=44.19 GB):   2%|▏         | 1/58 [00:00<00:19,  2.99it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=44.19 GB):   3%|▎         | 2/58 [00:00<00:16,  3.39it/s]Capturing num tokens (num_tokens=7168 avail_mem=44.19 GB):   3%|▎         | 2/58 [00:00<00:16,  3.39it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=44.19 GB):   5%|▌         | 3/58 [00:00<00:14,  3.70it/s]Capturing num tokens (num_tokens=6656 avail_mem=44.19 GB):   5%|▌         | 3/58 [00:00<00:14,  3.70it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=44.19 GB):   7%|▋         | 4/58 [00:01<00:13,  4.03it/s]Capturing num tokens (num_tokens=6144 avail_mem=44.20 GB):   7%|▋         | 4/58 [00:01<00:13,  4.03it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=44.20 GB):   9%|▊         | 5/58 [00:01<00:12,  4.32it/s]Capturing num tokens (num_tokens=5632 avail_mem=44.20 GB):   9%|▊         | 5/58 [00:01<00:12,  4.32it/s]Capturing num tokens (num_tokens=5632 avail_mem=44.20 GB):  10%|█         | 6/58 [00:01<00:11,  4.73it/s]Capturing num tokens (num_tokens=5120 avail_mem=44.20 GB):  10%|█         | 6/58 [00:01<00:11,  4.73it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=44.20 GB):  12%|█▏        | 7/58 [00:01<00:09,  5.13it/s]Capturing num tokens (num_tokens=4608 avail_mem=44.21 GB):  12%|█▏        | 7/58 [00:01<00:09,  5.13it/s]Capturing num tokens (num_tokens=4608 avail_mem=44.21 GB):  14%|█▍        | 8/58 [00:01<00:08,  5.65it/s]Capturing num tokens (num_tokens=4096 avail_mem=44.21 GB):  14%|█▍        | 8/58 [00:01<00:08,  5.65it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=44.21 GB):  16%|█▌        | 9/58 [00:01<00:07,  6.13it/s]Capturing num tokens (num_tokens=3840 avail_mem=43.21 GB):  16%|█▌        | 9/58 [00:01<00:07,  6.13it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=43.21 GB):  17%|█▋        | 10/58 [00:02<00:09,  4.80it/s]Capturing num tokens (num_tokens=3584 avail_mem=43.21 GB):  17%|█▋        | 10/58 [00:02<00:09,  4.80it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=43.21 GB):  19%|█▉        | 11/58 [00:02<00:10,  4.30it/s]Capturing num tokens (num_tokens=3328 avail_mem=43.21 GB):  19%|█▉        | 11/58 [00:02<00:10,  4.30it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=43.21 GB):  21%|██        | 12/58 [00:02<00:11,  4.12it/s]Capturing num tokens (num_tokens=3072 avail_mem=43.21 GB):  21%|██        | 12/58 [00:02<00:11,  4.12it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=43.21 GB):  22%|██▏       | 13/58 [00:02<00:10,  4.11it/s]Capturing num tokens (num_tokens=2816 avail_mem=43.21 GB):  22%|██▏       | 13/58 [00:02<00:10,  4.11it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=43.21 GB):  24%|██▍       | 14/58 [00:03<00:10,  4.25it/s]Capturing num tokens (num_tokens=2560 avail_mem=43.21 GB):  24%|██▍       | 14/58 [00:03<00:10,  4.25it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=43.21 GB):  26%|██▌       | 15/58 [00:03<00:09,  4.35it/s]Capturing num tokens (num_tokens=2304 avail_mem=43.21 GB):  26%|██▌       | 15/58 [00:03<00:09,  4.35it/s]Capturing num tokens (num_tokens=2304 avail_mem=43.21 GB):  28%|██▊       | 16/58 [00:03<00:09,  4.58it/s]Capturing num tokens (num_tokens=2048 avail_mem=43.21 GB):  28%|██▊       | 16/58 [00:03<00:09,  4.58it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=43.21 GB):  29%|██▉       | 17/58 [00:03<00:08,  4.80it/s]Capturing num tokens (num_tokens=1792 avail_mem=43.21 GB):  29%|██▉       | 17/58 [00:03<00:08,  4.80it/s]Capturing num tokens (num_tokens=1792 avail_mem=43.21 GB):  31%|███       | 18/58 [00:03<00:07,  5.07it/s]Capturing num tokens (num_tokens=1536 avail_mem=43.21 GB):  31%|███       | 18/58 [00:03<00:07,  5.07it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=43.21 GB):  33%|███▎      | 19/58 [00:04<00:07,  5.41it/s]Capturing num tokens (num_tokens=1280 avail_mem=43.21 GB):  33%|███▎      | 19/58 [00:04<00:07,  5.41it/s]Capturing num tokens (num_tokens=1280 avail_mem=43.21 GB):  34%|███▍      | 20/58 [00:04<00:06,  5.74it/s]Capturing num tokens (num_tokens=1024 avail_mem=43.21 GB):  34%|███▍      | 20/58 [00:04<00:06,  5.74it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=43.21 GB):  36%|███▌      | 21/58 [00:04<00:05,  6.24it/s]Capturing num tokens (num_tokens=960 avail_mem=43.20 GB):  36%|███▌      | 21/58 [00:04<00:05,  6.24it/s] Capturing num tokens (num_tokens=960 avail_mem=43.20 GB):  38%|███▊      | 22/58 [00:04<00:05,  6.64it/s]Capturing num tokens (num_tokens=896 avail_mem=43.20 GB):  38%|███▊      | 22/58 [00:04<00:05,  6.64it/s]

    Capturing num tokens (num_tokens=896 avail_mem=43.20 GB):  40%|███▉      | 23/58 [00:04<00:05,  6.87it/s]Capturing num tokens (num_tokens=832 avail_mem=43.20 GB):  40%|███▉      | 23/58 [00:04<00:05,  6.87it/s]Capturing num tokens (num_tokens=832 avail_mem=43.20 GB):  41%|████▏     | 24/58 [00:04<00:04,  7.23it/s]Capturing num tokens (num_tokens=768 avail_mem=43.19 GB):  41%|████▏     | 24/58 [00:04<00:04,  7.23it/s]

    Capturing num tokens (num_tokens=768 avail_mem=43.19 GB):  43%|████▎     | 25/58 [00:04<00:04,  7.53it/s]Capturing num tokens (num_tokens=704 avail_mem=43.19 GB):  43%|████▎     | 25/58 [00:04<00:04,  7.53it/s]Capturing num tokens (num_tokens=704 avail_mem=43.19 GB):  45%|████▍     | 26/58 [00:04<00:04,  7.93it/s]Capturing num tokens (num_tokens=640 avail_mem=43.19 GB):  45%|████▍     | 26/58 [00:04<00:04,  7.93it/s]

    Capturing num tokens (num_tokens=640 avail_mem=43.19 GB):  47%|████▋     | 27/58 [00:05<00:03,  8.19it/s]Capturing num tokens (num_tokens=576 avail_mem=43.18 GB):  47%|████▋     | 27/58 [00:05<00:03,  8.19it/s]Capturing num tokens (num_tokens=576 avail_mem=43.18 GB):  48%|████▊     | 28/58 [00:05<00:03,  8.37it/s]Capturing num tokens (num_tokens=512 avail_mem=43.18 GB):  48%|████▊     | 28/58 [00:05<00:03,  8.37it/s]

    Capturing num tokens (num_tokens=480 avail_mem=43.17 GB):  48%|████▊     | 28/58 [00:05<00:03,  8.37it/s]Capturing num tokens (num_tokens=480 avail_mem=43.17 GB):  52%|█████▏    | 30/58 [00:05<00:03,  9.11it/s]Capturing num tokens (num_tokens=448 avail_mem=43.17 GB):  52%|█████▏    | 30/58 [00:05<00:03,  9.11it/s]

    Capturing num tokens (num_tokens=448 avail_mem=43.17 GB):  53%|█████▎    | 31/58 [00:05<00:03,  8.95it/s]Capturing num tokens (num_tokens=416 avail_mem=43.17 GB):  53%|█████▎    | 31/58 [00:05<00:03,  8.95it/s]Capturing num tokens (num_tokens=384 avail_mem=43.16 GB):  53%|█████▎    | 31/58 [00:05<00:03,  8.95it/s]Capturing num tokens (num_tokens=384 avail_mem=43.16 GB):  57%|█████▋    | 33/58 [00:05<00:02, 10.13it/s]Capturing num tokens (num_tokens=352 avail_mem=43.16 GB):  57%|█████▋    | 33/58 [00:05<00:02, 10.13it/s]

    Capturing num tokens (num_tokens=320 avail_mem=43.16 GB):  57%|█████▋    | 33/58 [00:05<00:02, 10.13it/s]Capturing num tokens (num_tokens=320 avail_mem=43.16 GB):  60%|██████    | 35/58 [00:05<00:02, 10.96it/s]Capturing num tokens (num_tokens=288 avail_mem=43.15 GB):  60%|██████    | 35/58 [00:05<00:02, 10.96it/s]Capturing num tokens (num_tokens=256 avail_mem=43.15 GB):  60%|██████    | 35/58 [00:05<00:02, 10.96it/s]

    Capturing num tokens (num_tokens=256 avail_mem=43.15 GB):  64%|██████▍   | 37/58 [00:05<00:01, 12.14it/s]Capturing num tokens (num_tokens=240 avail_mem=43.14 GB):  64%|██████▍   | 37/58 [00:05<00:01, 12.14it/s]Capturing num tokens (num_tokens=224 avail_mem=43.14 GB):  64%|██████▍   | 37/58 [00:06<00:01, 12.14it/s]Capturing num tokens (num_tokens=224 avail_mem=43.14 GB):  67%|██████▋   | 39/58 [00:06<00:01, 13.25it/s]Capturing num tokens (num_tokens=208 avail_mem=43.14 GB):  67%|██████▋   | 39/58 [00:06<00:01, 13.25it/s]

    Capturing num tokens (num_tokens=192 avail_mem=43.13 GB):  67%|██████▋   | 39/58 [00:06<00:01, 13.25it/s]Capturing num tokens (num_tokens=192 avail_mem=43.13 GB):  71%|███████   | 41/58 [00:06<00:01, 13.88it/s]Capturing num tokens (num_tokens=176 avail_mem=43.13 GB):  71%|███████   | 41/58 [00:06<00:01, 13.88it/s]Capturing num tokens (num_tokens=160 avail_mem=43.13 GB):  71%|███████   | 41/58 [00:06<00:01, 13.88it/s]Capturing num tokens (num_tokens=144 avail_mem=43.12 GB):  71%|███████   | 41/58 [00:06<00:01, 13.88it/s]Capturing num tokens (num_tokens=144 avail_mem=43.12 GB):  76%|███████▌  | 44/58 [00:06<00:00, 16.88it/s]Capturing num tokens (num_tokens=128 avail_mem=43.13 GB):  76%|███████▌  | 44/58 [00:06<00:00, 16.88it/s]

    Capturing num tokens (num_tokens=112 avail_mem=43.13 GB):  76%|███████▌  | 44/58 [00:06<00:00, 16.88it/s]Capturing num tokens (num_tokens=96 avail_mem=43.12 GB):  76%|███████▌  | 44/58 [00:06<00:00, 16.88it/s] Capturing num tokens (num_tokens=96 avail_mem=43.12 GB):  81%|████████  | 47/58 [00:06<00:00, 19.26it/s]Capturing num tokens (num_tokens=80 avail_mem=43.11 GB):  81%|████████  | 47/58 [00:06<00:00, 19.26it/s]Capturing num tokens (num_tokens=64 avail_mem=43.11 GB):  81%|████████  | 47/58 [00:06<00:00, 19.26it/s]

    Capturing num tokens (num_tokens=64 avail_mem=43.11 GB):  84%|████████▍ | 49/58 [00:06<00:00, 17.38it/s]Capturing num tokens (num_tokens=48 avail_mem=43.11 GB):  84%|████████▍ | 49/58 [00:06<00:00, 17.38it/s]Capturing num tokens (num_tokens=32 avail_mem=43.10 GB):  84%|████████▍ | 49/58 [00:06<00:00, 17.38it/s]Capturing num tokens (num_tokens=28 avail_mem=43.10 GB):  84%|████████▍ | 49/58 [00:06<00:00, 17.38it/s]Capturing num tokens (num_tokens=28 avail_mem=43.10 GB):  90%|████████▉ | 52/58 [00:06<00:00, 19.69it/s]Capturing num tokens (num_tokens=24 avail_mem=43.10 GB):  90%|████████▉ | 52/58 [00:06<00:00, 19.69it/s]Capturing num tokens (num_tokens=20 avail_mem=43.09 GB):  90%|████████▉ | 52/58 [00:06<00:00, 19.69it/s]Capturing num tokens (num_tokens=16 avail_mem=43.09 GB):  90%|████████▉ | 52/58 [00:06<00:00, 19.69it/s]Capturing num tokens (num_tokens=12 avail_mem=43.09 GB):  90%|████████▉ | 52/58 [00:06<00:00, 19.69it/s]

    Capturing num tokens (num_tokens=8 avail_mem=43.08 GB):  90%|████████▉ | 52/58 [00:06<00:00, 19.69it/s] Capturing num tokens (num_tokens=8 avail_mem=43.08 GB):  98%|█████████▊| 57/58 [00:06<00:00, 26.01it/s]Capturing num tokens (num_tokens=4 avail_mem=43.08 GB):  98%|█████████▊| 57/58 [00:06<00:00, 26.01it/s]Capturing num tokens (num_tokens=4 avail_mem=43.08 GB): 100%|██████████| 58/58 [00:06<00:00,  8.42it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


    Server started on http://localhost:36717


Set the default backend. Note: Besides the local server, you may use also `OpenAI` or other API endpoints.


```python
set_default_backend(RuntimeEndpoint(f"http://localhost:{port}"))
```

    [2026-03-27 22:46:06] Endpoint '/get_model_info' is deprecated and will be removed in a future version. Please use '/model_info' instead.


## Basic Usage

The most simple way of using SGLang frontend language is a simple question answer dialog between a user and an assistant.


```python
@function
def basic_qa(s, question):
    s += system(f"You are a helpful assistant than can answer questions.")
    s += user(question)
    s += assistant(gen("answer", max_tokens=512))
```


```python
state = basic_qa("List 3 countries and their capitals.")
print_highlight(state["answer"])
```


<strong style='color: #00008B;'>Sure! Here are three countries and their capitals:<br><br>1. France - Paris<br>2. Germany - Berlin<br>3. Japan - Tokyo</strong>


## Multi-turn Dialog

SGLang frontend language can also be used to define multi-turn dialogs.


```python
@function
def multi_turn_qa(s):
    s += system(f"You are a helpful assistant than can answer questions.")
    s += user("Please give me a list of 3 countries and their capitals.")
    s += assistant(gen("first_answer", max_tokens=512))
    s += user("Please give me another list of 3 countries and their capitals.")
    s += assistant(gen("second_answer", max_tokens=512))
    return s


state = multi_turn_qa()
print_highlight(state["first_answer"])
print_highlight(state["second_answer"])
```


<strong style='color: #00008B;'>Certainly! Here is a list of three countries along with their capitals:<br><br>1. Japan - Tokyo<br>2. Brazil - Brasília<br>3. Germany - Berlin</strong>



<strong style='color: #00008B;'>Of course! Here is another list of three countries along with their capitals:<br><br>1. Egypt - Cairo<br>2. Canada - Ottawa<br>3. Spain - Madrid</strong>


## Control flow

You may use any Python code within the function to define more complex control flows.


```python
@function
def tool_use(s, question):
    s += assistant(
        "To answer this question: "
        + question
        + ". I need to use a "
        + gen("tool", choices=["calculator", "search engine"])
        + ". "
    )

    if s["tool"] == "calculator":
        s += assistant("The math expression is: " + gen("expression"))
    elif s["tool"] == "search engine":
        s += assistant("The key word to search is: " + gen("word"))


state = tool_use("What is 2 * 2?")
print_highlight(state["tool"])
print_highlight(state["expression"])
```


<strong style='color: #00008B;'>calculator</strong>



<strong style='color: #00008B;'>2 * 2. <br><br>Let's calculate it step by step:<br><br>1. Multiply 2 by 2.<br>2. \(2 * 2 = 4\).<br><br>So, 2 * 2 = 4. There was no need for a calculator in this case, as the multiplication is straightforward.</strong>


## Parallelism

Use `fork` to launch parallel prompts. Because `sgl.gen` is non-blocking, the for loop below issues two generation calls in parallel.


```python
@function
def tip_suggestion(s):
    s += assistant(
        "Here are two tips for staying healthy: "
        "1. Balanced Diet. 2. Regular Exercise.\n\n"
    )

    forks = s.fork(2)
    for i, f in enumerate(forks):
        f += assistant(
            f"Now, expand tip {i+1} into a paragraph:\n"
            + gen("detailed_tip", max_tokens=256, stop="\n\n")
        )

    s += assistant("Tip 1:" + forks[0]["detailed_tip"] + "\n")
    s += assistant("Tip 2:" + forks[1]["detailed_tip"] + "\n")
    s += assistant(
        "To summarize the above two tips, I can say:\n" + gen("summary", max_tokens=512)
    )


state = tip_suggestion()
print_highlight(state["summary"])
```


<strong style='color: #00008B;'>**1. Balanced Diet:**<br>A well-balanced diet is essential for maintaining good health. It involves consuming a variety of foods in the right proportions to ensure your body receives all the necessary nutrients for optimal function. Focus on including plenty of fruits and vegetables, which are rich in vitamins, minerals, and fiber. Whole grains and lean proteins are also important for providing sustained energy and supporting muscle health. Limiting processed foods, sugary drinks, and excessive saturated fats can help manage weight and reduce the risk of chronic diseases such as diabetes, heart disease, and certain cancers. Regularly consuming adequate amounts of water is also crucial for hydration and overall bodily functions.<br><br>**2. Regular Exercise:**<br>Regular exercise is crucial for maintaining overall health and well-being. It helps to strengthen your heart, lungs, and muscles, as well as improve your flexibility and coordination. Engaging in physical activity can boost your mood and reduce stress by releasing endorphins, which act as natural painkillers and mood elevators. Activities that raise your heart rate, such as running, swimming, or cycling, can help prevent heart disease and other cardiovascular issues. Strength training exercises can increase bone density and prevent osteoporosis, particularly in women. Regular physical activity also promotes better sleep, improves cognitive function, and can help with weight loss or maintenance. Therefore, it's important to make exercise a consistent part of your routine, aiming for at least 150 minutes of moderate aerobic activity or 75 minutes of vigorous activity each week, as recommended by health organizations.</strong>


## Constrained Decoding

Use `regex` to specify a regular expression as a decoding constraint. This is only supported for local models.


```python
@function
def regular_expression_gen(s):
    s += user("What is the IP address of the Google DNS servers?")
    s += assistant(
        gen(
            "answer",
            temperature=0,
            regex=r"((25[0-5]|2[0-4]\d|[01]?\d\d?).){3}(25[0-5]|2[0-4]\d|[01]?\d\d?)",
        )
    )


state = regular_expression_gen()
print_highlight(state["answer"])
```


<strong style='color: #00008B;'>208.67.222.222</strong>


Use `regex` to define a `JSON` decoding schema.


```python
character_regex = (
    r"""\{\n"""
    + r"""    "name": "[\w\d\s]{1,16}",\n"""
    + r"""    "house": "(Gryffindor|Slytherin|Ravenclaw|Hufflepuff)",\n"""
    + r"""    "blood status": "(Pure-blood|Half-blood|Muggle-born)",\n"""
    + r"""    "occupation": "(student|teacher|auror|ministry of magic|death eater|order of the phoenix)",\n"""
    + r"""    "wand": \{\n"""
    + r"""        "wood": "[\w\d\s]{1,16}",\n"""
    + r"""        "core": "[\w\d\s]{1,16}",\n"""
    + r"""        "length": [0-9]{1,2}\.[0-9]{0,2}\n"""
    + r"""    \},\n"""
    + r"""    "alive": "(Alive|Deceased)",\n"""
    + r"""    "patronus": "[\w\d\s]{1,16}",\n"""
    + r"""    "bogart": "[\w\d\s]{1,16}"\n"""
    + r"""\}"""
)


@function
def character_gen(s, name):
    s += user(
        f"{name} is a character in Harry Potter. Please fill in the following information about this character."
    )
    s += assistant(gen("json_output", max_tokens=256, regex=character_regex))


state = character_gen("Harry Potter")
print_highlight(state["json_output"])
```


<strong style='color: #00008B;'>{<br>    "name": "Harry Potter",<br>    "house": "Gryffindor",<br>    "blood status": "Half-blood",<br>    "occupation": "student",<br>    "wand": {<br>        "wood": "Holly",<br>        "core": "Phoenix feather",<br>        "length": 11.0<br>    },<br>    "alive": "Alive",<br>    "patronus": "Stag",<br>    "bogart": "Cobweb"<br>}</strong>


## Batching 

Use `run_batch` to run a batch of prompts.


```python
@function
def text_qa(s, question):
    s += user(question)
    s += assistant(gen("answer", stop="\n"))


states = text_qa.run_batch(
    [
        {"question": "What is the capital of the United Kingdom?"},
        {"question": "What is the capital of France?"},
        {"question": "What is the capital of Japan?"},
    ],
    progress_bar=True,
)

for i, state in enumerate(states):
    print_highlight(f"Answer {i+1}: {states[i]['answer']}")
```

      0%|          | 0/3 [00:00<?, ?it/s]

    100%|██████████| 3/3 [00:00<00:00, 35.38it/s]

    



<strong style='color: #00008B;'>Answer 1: The capital of the United Kingdom is London.</strong>



<strong style='color: #00008B;'>Answer 2: The capital of France is Paris.</strong>



<strong style='color: #00008B;'>Answer 3: The capital of Japan is Tokyo.</strong>


## Streaming 

Use `stream` to stream the output to the user.


```python
@function
def text_qa(s, question):
    s += user(question)
    s += assistant(gen("answer", stop="\n"))


state = text_qa.run(
    question="What is the capital of France?", temperature=0.1, stream=True
)

for out in state.text_iter():
    print(out, end="", flush=True)
```

    <|im_start|>system
    You are a helpful assistant.<|im_end|>
    <|im_start|>user
    What is the capital of France?<|im_end|>
    <|im_start|>assistant


    The

     capital

     of

     France

     is

     Paris

    .

    <|im_end|>


## Complex Prompts

You may use `{system|user|assistant}_{begin|end}` to define complex prompts.


```python
@function
def chat_example(s):
    s += system("You are a helpful assistant.")
    # Same as: s += s.system("You are a helpful assistant.")

    with s.user():
        s += "Question: What is the capital of France?"

    s += assistant_begin()
    s += "Answer: " + gen("answer", max_tokens=100, stop="\n")
    s += assistant_end()


state = chat_example()
print_highlight(state["answer"])
```


<strong style='color: #00008B;'> The capital of France is Paris.</strong>



```python
terminate_process(server_process)
```

## Multi-modal Generation

You may use SGLang frontend language to define multi-modal prompts.
See [here](https://docs.sglang.io/supported_models/text_generation/multimodal_language_models.html) for supported models.


```python
server_process, port = launch_server_cmd(
    "python -m sglang.launch_server --model-path Qwen/Qwen2.5-VL-7B-Instruct --host 0.0.0.0 --log-level warning"
)

wait_for_server(f"http://localhost:{port}", process=server_process)
print(f"Server started on http://localhost:{port}")
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    2026-03-27 22:46:20.008 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:46:20] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 22:46:20.008 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:46:20] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 22:46:20.008 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:46:20] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 22:46:20.008 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:46:20] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 22:46:20.008 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:46:20] Persistent cache disabled, using in-memory JIT cache


    2026-03-27 22:46:29.124 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:46:29] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 22:46:29.124 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:46:29] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 22:46:29.124 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:46:29] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 22:46:29.124 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:46:29] Persistent cache disabled, using in-memory JIT cache
    2026-03-27 22:46:29.124 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-27 22:46:29] Persistent cache disabled, using in-memory JIT cache


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Loading safetensors checkpoint shards:   0% Completed | 0/5 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  20% Completed | 1/5 [00:00<00:02,  1.88it/s]

    Loading safetensors checkpoint shards:  40% Completed | 2/5 [00:01<00:01,  1.73it/s]

    Loading safetensors checkpoint shards:  60% Completed | 3/5 [00:01<00:00,  2.34it/s]

    Loading safetensors checkpoint shards:  80% Completed | 4/5 [00:01<00:00,  2.05it/s]

    Loading safetensors checkpoint shards: 100% Completed | 5/5 [00:02<00:00,  1.72it/s]Loading safetensors checkpoint shards: 100% Completed | 5/5 [00:02<00:00,  1.84it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


    Server started on http://localhost:33947



```python
set_default_backend(RuntimeEndpoint(f"http://localhost:{port}"))
```

    [2026-03-27 22:46:42] Endpoint '/get_model_info' is deprecated and will be removed in a future version. Please use '/model_info' instead.


Ask a question about an image.


```python
@function
def image_qa(s, image_file, question):
    s += user(image(image_file) + question)
    s += assistant(gen("answer", max_tokens=256))


image_url = "https://raw.githubusercontent.com/sgl-project/sglang/main/examples/assets/example_image.png"
image_bytes, _ = load_image(image_url)
state = image_qa(image_bytes, "What is in the image?")
print_highlight(state["answer"])
```


<strong style='color: #00008B;'>The image shows a person ironing a shirt on an upside-down car in a__.街． The individual is using a table that has been placed under the car, likely to provide a stable surface for ironing. The setting suggests a busy street, with a yellow taxi and another vehicle visible in the background, along with urban buildings. The person appears to be cargo-haired, might be wearing a yellow shirt, and is focused on their task despite the unconventional method and location. This scene has a lighthearted or humorous tone, often associated with street activities or unconventional uses of everyday objects in promotional activities or performances.</strong>



```python
terminate_process(server_process)
```
