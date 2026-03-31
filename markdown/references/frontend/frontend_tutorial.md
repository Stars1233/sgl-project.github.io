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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:173: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-31 21:14:05.720 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:14:05] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:14:05.721 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:14:05] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:14:05.721 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:14:05] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:14:05.721 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:14:05] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:14:05.721 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:14:05] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:01,  1.72it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.50it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:01<00:00,  1.54it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.56it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.56it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:42,  2.85s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:42,  2.85s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:19,  1.41s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:19,  1.41s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:47,  1.17it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:47,  1.17it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:31,  1.70it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:31,  1.70it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:22,  2.31it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:22,  2.31it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:03<00:18,  2.81it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:03<00:18,  2.81it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:17,  2.97it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:17,  2.97it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:15,  3.13it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:15,  3.13it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:14,  3.39it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:14,  3.39it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:12,  3.73it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:12,  3.73it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:11,  3.98it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:11,  3.98it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:10,  4.34it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:10,  4.34it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:05<00:09,  4.68it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:05<00:09,  4.68it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:05<00:08,  5.00it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:05<00:08,  5.00it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:05<00:07,  5.54it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:05<00:07,  5.54it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:07,  5.98it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:07,  5.98it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:06<00:06,  6.53it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:06<00:06,  6.53it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:06<00:06,  6.53it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:06<00:05,  7.77it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:06<00:05,  7.77it/s]

    Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:06<00:05,  7.77it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:06<00:04,  8.88it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:06<00:04,  8.88it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:06<00:04,  8.88it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:06<00:03, 10.70it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:06<00:03, 10.70it/s]

    Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:06<00:03, 10.70it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:06<00:02, 12.08it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:06<00:02, 12.08it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:06<00:02, 12.08it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:06<00:02, 13.80it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:06<00:02, 13.80it/s]

    Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:06<00:02, 13.80it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:06<00:01, 15.16it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:06<00:01, 15.16it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:07<00:01, 15.16it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:07<00:01, 15.16it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:07<00:01, 17.09it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:07<00:01, 17.09it/s]

    Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:07<00:01, 17.09it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:07<00:01, 17.09it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:07<00:01, 18.67it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:07<00:01, 18.67it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:07<00:01, 18.67it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:07<00:01, 18.67it/s]

    Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:07<00:00, 20.01it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:07<00:00, 20.01it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:07<00:00, 20.01it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:07<00:00, 20.01it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:07<00:00, 21.95it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:07<00:00, 21.95it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:07<00:00, 21.95it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:07<00:00, 21.95it/s]

    Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:07<00:00, 22.21it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:07<00:00, 22.21it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:07<00:00, 22.21it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:07<00:00, 22.21it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:07<00:00, 23.18it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:07<00:00, 23.18it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:07<00:00, 23.18it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:07<00:00, 23.18it/s]

    Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:07<00:00, 24.45it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:07<00:00, 24.45it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:07<00:00, 24.45it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:07<00:00, 24.45it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:07<00:00, 25.85it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:07<00:00, 25.85it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:07<00:00, 25.85it/s]Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:08<00:00, 25.85it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:08<00:00, 25.85it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:08<00:00, 25.85it/s]

    Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00, 30.77it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:08<00:00,  7.18it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=43.22 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=43.22 GB):   2%|▏         | 1/58 [00:00<00:33,  1.70it/s]Capturing num tokens (num_tokens=7680 avail_mem=43.19 GB):   2%|▏         | 1/58 [00:00<00:33,  1.70it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=43.19 GB):   3%|▎         | 2/58 [00:00<00:26,  2.11it/s]Capturing num tokens (num_tokens=7168 avail_mem=43.18 GB):   3%|▎         | 2/58 [00:00<00:26,  2.11it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=43.18 GB):   5%|▌         | 3/58 [00:01<00:21,  2.56it/s]Capturing num tokens (num_tokens=6656 avail_mem=43.19 GB):   5%|▌         | 3/58 [00:01<00:21,  2.56it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=43.19 GB):   7%|▋         | 4/58 [00:01<00:18,  2.85it/s]Capturing num tokens (num_tokens=6144 avail_mem=42.16 GB):   7%|▋         | 4/58 [00:01<00:18,  2.85it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=42.16 GB):   9%|▊         | 5/58 [00:02<00:20,  2.62it/s]Capturing num tokens (num_tokens=5632 avail_mem=43.16 GB):   9%|▊         | 5/58 [00:02<00:20,  2.62it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=43.16 GB):  10%|█         | 6/58 [00:02<00:19,  2.67it/s]Capturing num tokens (num_tokens=5120 avail_mem=42.34 GB):  10%|█         | 6/58 [00:02<00:19,  2.67it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=42.34 GB):  12%|█▏        | 7/58 [00:02<00:18,  2.76it/s]Capturing num tokens (num_tokens=4608 avail_mem=43.17 GB):  12%|█▏        | 7/58 [00:02<00:18,  2.76it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=43.17 GB):  14%|█▍        | 8/58 [00:02<00:16,  2.95it/s]Capturing num tokens (num_tokens=4096 avail_mem=42.41 GB):  14%|█▍        | 8/58 [00:02<00:16,  2.95it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=42.41 GB):  16%|█▌        | 9/58 [00:03<00:16,  3.02it/s]Capturing num tokens (num_tokens=3840 avail_mem=43.18 GB):  16%|█▌        | 9/58 [00:03<00:16,  3.02it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=43.18 GB):  17%|█▋        | 10/58 [00:03<00:15,  3.15it/s]Capturing num tokens (num_tokens=3584 avail_mem=42.47 GB):  17%|█▋        | 10/58 [00:03<00:15,  3.15it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=42.47 GB):  19%|█▉        | 11/58 [00:03<00:14,  3.32it/s]Capturing num tokens (num_tokens=3328 avail_mem=42.53 GB):  19%|█▉        | 11/58 [00:03<00:14,  3.32it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=42.53 GB):  21%|██        | 12/58 [00:04<00:13,  3.45it/s]Capturing num tokens (num_tokens=3072 avail_mem=42.53 GB):  21%|██        | 12/58 [00:04<00:13,  3.45it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=42.53 GB):  22%|██▏       | 13/58 [00:04<00:12,  3.73it/s]Capturing num tokens (num_tokens=2816 avail_mem=42.59 GB):  22%|██▏       | 13/58 [00:04<00:12,  3.73it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=42.59 GB):  24%|██▍       | 14/58 [00:04<00:11,  3.99it/s]Capturing num tokens (num_tokens=2560 avail_mem=43.19 GB):  24%|██▍       | 14/58 [00:04<00:11,  3.99it/s]Capturing num tokens (num_tokens=2560 avail_mem=43.19 GB):  26%|██▌       | 15/58 [00:04<00:09,  4.33it/s]Capturing num tokens (num_tokens=2304 avail_mem=42.65 GB):  26%|██▌       | 15/58 [00:04<00:09,  4.33it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=42.65 GB):  28%|██▊       | 16/58 [00:04<00:09,  4.38it/s]Capturing num tokens (num_tokens=2048 avail_mem=42.70 GB):  28%|██▊       | 16/58 [00:04<00:09,  4.38it/s]Capturing num tokens (num_tokens=2048 avail_mem=42.70 GB):  29%|██▉       | 17/58 [00:05<00:08,  4.80it/s]Capturing num tokens (num_tokens=1792 avail_mem=43.20 GB):  29%|██▉       | 17/58 [00:05<00:08,  4.80it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=43.20 GB):  31%|███       | 18/58 [00:05<00:07,  5.21it/s]Capturing num tokens (num_tokens=1536 avail_mem=42.76 GB):  31%|███       | 18/58 [00:05<00:07,  5.21it/s]Capturing num tokens (num_tokens=1536 avail_mem=42.76 GB):  33%|███▎      | 19/58 [00:05<00:06,  5.92it/s]Capturing num tokens (num_tokens=1280 avail_mem=42.79 GB):  33%|███▎      | 19/58 [00:05<00:06,  5.92it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=42.79 GB):  34%|███▍      | 20/58 [00:05<00:06,  6.15it/s]Capturing num tokens (num_tokens=1024 avail_mem=43.23 GB):  34%|███▍      | 20/58 [00:05<00:06,  6.15it/s]Capturing num tokens (num_tokens=960 avail_mem=42.81 GB):  34%|███▍      | 20/58 [00:05<00:06,  6.15it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=42.81 GB):  38%|███▊      | 22/58 [00:05<00:04,  7.40it/s]Capturing num tokens (num_tokens=896 avail_mem=43.19 GB):  38%|███▊      | 22/58 [00:05<00:04,  7.40it/s]Capturing num tokens (num_tokens=832 avail_mem=42.83 GB):  38%|███▊      | 22/58 [00:05<00:04,  7.40it/s]Capturing num tokens (num_tokens=832 avail_mem=42.83 GB):  41%|████▏     | 24/58 [00:05<00:04,  8.36it/s]Capturing num tokens (num_tokens=768 avail_mem=43.19 GB):  41%|████▏     | 24/58 [00:05<00:04,  8.36it/s]

    Capturing num tokens (num_tokens=704 avail_mem=42.85 GB):  41%|████▏     | 24/58 [00:06<00:04,  8.36it/s]Capturing num tokens (num_tokens=704 avail_mem=42.85 GB):  45%|████▍     | 26/58 [00:06<00:03,  9.34it/s]Capturing num tokens (num_tokens=640 avail_mem=43.18 GB):  45%|████▍     | 26/58 [00:06<00:03,  9.34it/s]Capturing num tokens (num_tokens=576 avail_mem=42.87 GB):  45%|████▍     | 26/58 [00:06<00:03,  9.34it/s]

    Capturing num tokens (num_tokens=576 avail_mem=42.87 GB):  48%|████▊     | 28/58 [00:06<00:02, 10.24it/s]Capturing num tokens (num_tokens=512 avail_mem=42.96 GB):  48%|████▊     | 28/58 [00:06<00:02, 10.24it/s]Capturing num tokens (num_tokens=480 avail_mem=43.17 GB):  48%|████▊     | 28/58 [00:06<00:02, 10.24it/s]Capturing num tokens (num_tokens=480 avail_mem=43.17 GB):  52%|█████▏    | 30/58 [00:06<00:02, 11.04it/s]Capturing num tokens (num_tokens=448 avail_mem=42.91 GB):  52%|█████▏    | 30/58 [00:06<00:02, 11.04it/s]

    Capturing num tokens (num_tokens=416 avail_mem=43.16 GB):  52%|█████▏    | 30/58 [00:06<00:02, 11.04it/s]Capturing num tokens (num_tokens=416 avail_mem=43.16 GB):  55%|█████▌    | 32/58 [00:06<00:02, 11.58it/s]Capturing num tokens (num_tokens=384 avail_mem=43.16 GB):  55%|█████▌    | 32/58 [00:06<00:02, 11.58it/s]Capturing num tokens (num_tokens=352 avail_mem=42.95 GB):  55%|█████▌    | 32/58 [00:06<00:02, 11.58it/s]Capturing num tokens (num_tokens=352 avail_mem=42.95 GB):  59%|█████▊    | 34/58 [00:06<00:01, 13.14it/s]Capturing num tokens (num_tokens=320 avail_mem=43.14 GB):  59%|█████▊    | 34/58 [00:06<00:01, 13.14it/s]

    Capturing num tokens (num_tokens=288 avail_mem=43.14 GB):  59%|█████▊    | 34/58 [00:06<00:01, 13.14it/s]Capturing num tokens (num_tokens=288 avail_mem=43.14 GB):  62%|██████▏   | 36/58 [00:06<00:01, 14.34it/s]Capturing num tokens (num_tokens=256 avail_mem=43.00 GB):  62%|██████▏   | 36/58 [00:06<00:01, 14.34it/s]Capturing num tokens (num_tokens=240 avail_mem=43.13 GB):  62%|██████▏   | 36/58 [00:06<00:01, 14.34it/s]Capturing num tokens (num_tokens=224 avail_mem=43.13 GB):  62%|██████▏   | 36/58 [00:06<00:01, 14.34it/s]Capturing num tokens (num_tokens=224 avail_mem=43.13 GB):  67%|██████▋   | 39/58 [00:06<00:01, 16.48it/s]Capturing num tokens (num_tokens=208 avail_mem=43.12 GB):  67%|██████▋   | 39/58 [00:06<00:01, 16.48it/s]

    Capturing num tokens (num_tokens=192 avail_mem=43.01 GB):  67%|██████▋   | 39/58 [00:06<00:01, 16.48it/s]Capturing num tokens (num_tokens=176 avail_mem=43.09 GB):  67%|██████▋   | 39/58 [00:07<00:01, 16.48it/s]Capturing num tokens (num_tokens=176 avail_mem=43.09 GB):  72%|███████▏  | 42/58 [00:07<00:00, 18.23it/s]Capturing num tokens (num_tokens=160 avail_mem=43.10 GB):  72%|███████▏  | 42/58 [00:07<00:00, 18.23it/s]Capturing num tokens (num_tokens=144 avail_mem=43.09 GB):  72%|███████▏  | 42/58 [00:07<00:00, 18.23it/s]Capturing num tokens (num_tokens=128 avail_mem=43.10 GB):  72%|███████▏  | 42/58 [00:07<00:00, 18.23it/s]

    Capturing num tokens (num_tokens=128 avail_mem=43.10 GB):  78%|███████▊  | 45/58 [00:07<00:00, 19.44it/s]Capturing num tokens (num_tokens=112 avail_mem=43.09 GB):  78%|███████▊  | 45/58 [00:07<00:00, 19.44it/s]Capturing num tokens (num_tokens=96 avail_mem=43.03 GB):  78%|███████▊  | 45/58 [00:07<00:00, 19.44it/s] Capturing num tokens (num_tokens=80 avail_mem=43.07 GB):  78%|███████▊  | 45/58 [00:07<00:00, 19.44it/s]Capturing num tokens (num_tokens=80 avail_mem=43.07 GB):  83%|████████▎ | 48/58 [00:07<00:00, 21.26it/s]Capturing num tokens (num_tokens=64 avail_mem=43.07 GB):  83%|████████▎ | 48/58 [00:07<00:00, 21.26it/s]Capturing num tokens (num_tokens=48 avail_mem=43.06 GB):  83%|████████▎ | 48/58 [00:07<00:00, 21.26it/s]

    Capturing num tokens (num_tokens=32 avail_mem=43.06 GB):  83%|████████▎ | 48/58 [00:07<00:00, 21.26it/s]Capturing num tokens (num_tokens=32 avail_mem=43.06 GB):  88%|████████▊ | 51/58 [00:07<00:00, 20.20it/s]Capturing num tokens (num_tokens=28 avail_mem=43.01 GB):  88%|████████▊ | 51/58 [00:07<00:00, 20.20it/s]Capturing num tokens (num_tokens=24 avail_mem=43.04 GB):  88%|████████▊ | 51/58 [00:07<00:00, 20.20it/s]Capturing num tokens (num_tokens=20 avail_mem=43.03 GB):  88%|████████▊ | 51/58 [00:07<00:00, 20.20it/s]Capturing num tokens (num_tokens=20 avail_mem=43.03 GB):  93%|█████████▎| 54/58 [00:07<00:00, 21.92it/s]Capturing num tokens (num_tokens=16 avail_mem=43.03 GB):  93%|█████████▎| 54/58 [00:07<00:00, 21.92it/s]Capturing num tokens (num_tokens=12 avail_mem=43.02 GB):  93%|█████████▎| 54/58 [00:07<00:00, 21.92it/s]

    Capturing num tokens (num_tokens=8 avail_mem=43.01 GB):  93%|█████████▎| 54/58 [00:07<00:00, 21.92it/s] Capturing num tokens (num_tokens=8 avail_mem=43.01 GB):  98%|█████████▊| 57/58 [00:07<00:00, 23.42it/s]Capturing num tokens (num_tokens=4 avail_mem=43.00 GB):  98%|█████████▊| 57/58 [00:07<00:00, 23.42it/s]Capturing num tokens (num_tokens=4 avail_mem=43.00 GB): 100%|██████████| 58/58 [00:07<00:00,  7.51it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


    Server started on http://localhost:34643


Set the default backend. Note: Besides the local server, you may use also `OpenAI` or other API endpoints.


```python
set_default_backend(RuntimeEndpoint(f"http://localhost:{port}"))
```

    [2026-03-31 21:14:33] Endpoint '/get_model_info' is deprecated and will be removed in a future version. Please use '/model_info' instead.


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


<strong style='color: #00008B;'>Sure! Here are three countries with their respective capitals:<br><br>1. **France** - Paris<br>2. **Japan** - Tokyo<br>3. **Australia** - Canberra</strong>


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


<strong style='color: #00008B;'>Certainly! Here's a list of three countries and their respective capitals:<br><br>1. **France** - Paris<br>2. **Japan** - Tokyo<br>3. **Australia** - Canberra</strong>



<strong style='color: #00008B;'>Of course! Here's another list of three countries and their capitals:<br><br>1. **Italy** - Rome<br>2. **Mexico** - Mexico City<br>3. **South Africa** - Pretoria (administrative capital), Cape Town (legislative capital), Bloemfontein (judicial capital)</strong>


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



<strong style='color: #00008B;'>2 * 2. <br><br>Let's calculate this:<br>\[ 2 * 2 = 4 \]<br><br>So, 2 * 2 equals 4. No calculator was needed for this simple multiplication.</strong>


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


<strong style='color: #00008B;'>1. **Balanced Diet**: Consuming a variety of healthy foods to ensure you get all necessary nutrients, including fruits, vegetables, whole grains, lean proteins, and healthy fats. This helps in maintaining energy levels, boosting the immune system, and improving overall health.<br>2. **Regular Exercise**: Engaging in physical activity regularly to keep your body fit and healthy. Aim for at least 150 to 300 minutes of moderate aerobic activity or 75 to 150 minutes of vigorous aerobic activity per week, plus strength training at least two days a week. This improves cardiovascular health, aids in weight management, and enhances overall well-being.<br><br>By combining these two healthy habits, you can significantly improve your quality of life and overall health.</strong>


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


<strong style='color: #00008B;'>{<br>    "name": "Harry Potter",<br>    "house": "Gryffindor",<br>    "blood status": "Half-blood",<br>    "occupation": "student",<br>    "wand": {<br>        "wood": "Holm oak",<br>        "core": "Phoenix feather",<br>        "length": 11.5<br>    },<br>    "alive": "Alive",<br>    "patronus": "Stag",<br>    "bogart": "A large rat"<br>}</strong>


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

    100%|██████████| 3/3 [00:00<00:00, 35.82it/s]

    



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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:173: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    2026-03-31 21:14:46.043 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:14:46] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:14:46.043 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:14:46] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:14:46.043 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:14:46] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:14:46.043 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:14:46] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:14:46.044 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:14:46] Persistent cache disabled, using in-memory JIT cache


    2026-03-31 21:14:54.943 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:14:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:14:54.943 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:14:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:14:54.943 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:14:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:14:54.943 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:14:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-31 21:14:54.943 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-31 21:14:54] Persistent cache disabled, using in-memory JIT cache


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Loading safetensors checkpoint shards:   0% Completed | 0/5 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  20% Completed | 1/5 [00:00<00:02,  1.57it/s]

    Loading safetensors checkpoint shards:  40% Completed | 2/5 [00:01<00:02,  1.36it/s]

    Loading safetensors checkpoint shards:  60% Completed | 3/5 [00:01<00:01,  1.86it/s]

    Loading safetensors checkpoint shards:  80% Completed | 4/5 [00:02<00:00,  1.75it/s]

    Loading safetensors checkpoint shards: 100% Completed | 5/5 [00:03<00:00,  1.54it/s]Loading safetensors checkpoint shards: 100% Completed | 5/5 [00:03<00:00,  1.58it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


    Server started on http://localhost:37261



```python
set_default_backend(RuntimeEndpoint(f"http://localhost:{port}"))
```

    [2026-03-31 21:15:08] Endpoint '/get_model_info' is deprecated and will be removed in a future version. Please use '/model_info' instead.


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

    /actions-runner/_work/sglang/sglang/python/sglang/srt/utils/common.py:799: UserWarning: The given buffer is not writable, and PyTorch does not support non-writable tensors. This means you can write to the underlying (supposedly non-writable) buffer using the tensor. You may want to copy the buffer to protect its data or make it writable before converting it to a tensor. This type of warning will be suppressed for the rest of this program. (Triggered internally at /pytorch/torch/csrc/utils/tensor_new.cpp:1581.)
      encoded_image = torch.frombuffer(image_bytes, dtype=torch.uint8)



<strong style='color: #00008B;'>The image shows a person ironing a pair of jeans on an ironing board that is set up on the back of a yellow SUV parked on a city street. The scene appears to be a public street setting, with a traffic light visible in the background, along with other vehicles such as a yellow taxi. There are tall buildings and banners, suggesting this is an urban area, possibly a busy city center.</strong>



```python
terminate_process(server_process)
```
