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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:176: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-29 09:01:06] numactl command not found, skipping NUMA node configuration for GPU. Install numactl (e.g., apt-get install numactl) to enable automatic NUMA binding.


    [2026-03-29 09:01:13] numactl command not found, skipping NUMA node configuration for GPU. Install numactl (e.g., apt-get install numactl) to enable automatic NUMA binding.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-29 09:01:15.987 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:01:15] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 09:01:15.988 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:01:15] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 09:01:15.988 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:01:15] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 09:01:15.988 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:01:15] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 09:01:15.988 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:01:15] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:01<00:05,  1.77s/it]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:03<00:03,  1.77s/it]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:05<00:01,  1.66s/it]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:06<00:00,  1.56s/it]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:06<00:00,  1.62s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<02:51,  3.02s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<02:51,  3.02s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:27,  1.56s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:27,  1.56s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:53,  1.03it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:53,  1.03it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:37,  1.46it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:37,  1.46it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:04<00:27,  1.93it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:04<00:27,  1.93it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:21,  2.39it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:21,  2.39it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:17,  2.94it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:17,  2.94it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:14,  3.51it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:14,  3.51it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:11,  4.14it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:11,  4.14it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:09,  4.82it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:09,  4.82it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:05<00:08,  5.49it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:05<00:08,  5.49it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:05<00:07,  6.19it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:05<00:07,  6.19it/s]Compiling num tokens (num_tokens=2816):  21%|██        | 12/58 [00:05<00:07,  6.19it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:05<00:05,  8.15it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:05<00:05,  8.15it/s]

    Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:05<00:05,  8.15it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:05<00:04, 10.24it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:05<00:04, 10.24it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:05<00:04, 10.24it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:05<00:04, 10.24it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:05<00:02, 13.61it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:05<00:02, 13.61it/s]

    Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:05<00:02, 13.61it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:05<00:02, 13.61it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:05<00:02, 13.61it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:05<00:01, 18.55it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:05<00:01, 18.55it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:05<00:01, 18.55it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:05<00:01, 18.55it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:05<00:01, 18.55it/s]Compiling num tokens (num_tokens=576):  40%|███▉      | 23/58 [00:06<00:01, 18.55it/s]

    Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:06<00:01, 25.12it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:06<00:01, 25.12it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:06<00:01, 25.12it/s]Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:06<00:01, 25.12it/s]Compiling num tokens (num_tokens=416):  48%|████▊     | 28/58 [00:06<00:01, 25.12it/s]Compiling num tokens (num_tokens=384):  48%|████▊     | 28/58 [00:06<00:01, 25.12it/s]Compiling num tokens (num_tokens=352):  48%|████▊     | 28/58 [00:06<00:01, 25.12it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:06<00:00, 32.71it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:06<00:00, 32.71it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:06<00:00, 32.71it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:06<00:00, 32.71it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:06<00:00, 32.71it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:06<00:00, 32.71it/s]

    Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:06<00:00, 32.71it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:06<00:00, 38.52it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:06<00:00, 38.52it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:06<00:00, 38.52it/s]Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:06<00:00, 38.52it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:06<00:00, 38.52it/s]Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:06<00:00, 38.52it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:06<00:00, 41.35it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:06<00:00, 41.35it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:06<00:00, 41.35it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:06<00:00, 41.35it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:06<00:00, 41.35it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:06<00:00, 41.35it/s]

    Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:06<00:00, 41.35it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:06<00:00, 45.36it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:06<00:00, 45.36it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:06<00:00, 45.36it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:06<00:00, 45.36it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:06<00:00, 45.36it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:06<00:00, 45.36it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:06<00:00, 45.36it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:06<00:00, 45.36it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:06<00:00,  8.83it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.74 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=119.74 GB):   2%|▏         | 1/58 [00:00<00:16,  3.53it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.71 GB):   2%|▏         | 1/58 [00:00<00:16,  3.53it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=119.71 GB):   3%|▎         | 2/58 [00:00<00:15,  3.66it/s]Capturing num tokens (num_tokens=7168 avail_mem=119.71 GB):   3%|▎         | 2/58 [00:00<00:15,  3.66it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=119.71 GB):   5%|▌         | 3/58 [00:00<00:14,  3.88it/s]Capturing num tokens (num_tokens=6656 avail_mem=119.71 GB):   5%|▌         | 3/58 [00:00<00:14,  3.88it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=119.71 GB):   7%|▋         | 4/58 [00:01<00:12,  4.16it/s]Capturing num tokens (num_tokens=6144 avail_mem=119.71 GB):   7%|▋         | 4/58 [00:01<00:12,  4.16it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=119.71 GB):   9%|▊         | 5/58 [00:01<00:12,  4.41it/s]Capturing num tokens (num_tokens=5632 avail_mem=119.72 GB):   9%|▊         | 5/58 [00:01<00:12,  4.41it/s]Capturing num tokens (num_tokens=5632 avail_mem=119.72 GB):  10%|█         | 6/58 [00:01<00:10,  4.80it/s]Capturing num tokens (num_tokens=5120 avail_mem=119.72 GB):  10%|█         | 6/58 [00:01<00:10,  4.80it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=119.72 GB):  12%|█▏        | 7/58 [00:01<00:10,  5.07it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.72 GB):  12%|█▏        | 7/58 [00:01<00:10,  5.07it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.72 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.34it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.73 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.34it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=119.73 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.89it/s]Capturing num tokens (num_tokens=3840 avail_mem=119.73 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.89it/s]Capturing num tokens (num_tokens=3840 avail_mem=119.73 GB):  17%|█▋        | 10/58 [00:01<00:07,  6.41it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.73 GB):  17%|█▋        | 10/58 [00:01<00:07,  6.41it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=119.73 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.93it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.73 GB):  19%|█▉        | 11/58 [00:02<00:06,  6.93it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.73 GB):  21%|██        | 12/58 [00:02<00:06,  7.02it/s]Capturing num tokens (num_tokens=3072 avail_mem=119.73 GB):  21%|██        | 12/58 [00:02<00:06,  7.02it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=119.73 GB):  22%|██▏       | 13/58 [00:02<00:06,  7.23it/s]Capturing num tokens (num_tokens=2816 avail_mem=119.73 GB):  22%|██▏       | 13/58 [00:02<00:06,  7.23it/s]Capturing num tokens (num_tokens=2816 avail_mem=119.73 GB):  24%|██▍       | 14/58 [00:02<00:05,  7.89it/s]Capturing num tokens (num_tokens=2560 avail_mem=119.73 GB):  24%|██▍       | 14/58 [00:02<00:05,  7.89it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=119.73 GB):  26%|██▌       | 15/58 [00:02<00:05,  8.26it/s]Capturing num tokens (num_tokens=2304 avail_mem=119.73 GB):  26%|██▌       | 15/58 [00:02<00:05,  8.26it/s]Capturing num tokens (num_tokens=2304 avail_mem=119.73 GB):  28%|██▊       | 16/58 [00:02<00:04,  8.70it/s]Capturing num tokens (num_tokens=2048 avail_mem=119.73 GB):  28%|██▊       | 16/58 [00:02<00:04,  8.70it/s]Capturing num tokens (num_tokens=1792 avail_mem=119.73 GB):  28%|██▊       | 16/58 [00:02<00:04,  8.70it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=119.73 GB):  31%|███       | 18/58 [00:02<00:04,  9.89it/s]Capturing num tokens (num_tokens=1536 avail_mem=119.72 GB):  31%|███       | 18/58 [00:02<00:04,  9.89it/s]Capturing num tokens (num_tokens=1280 avail_mem=119.73 GB):  31%|███       | 18/58 [00:02<00:04,  9.89it/s]Capturing num tokens (num_tokens=1280 avail_mem=119.73 GB):  34%|███▍      | 20/58 [00:02<00:03, 11.45it/s]Capturing num tokens (num_tokens=1024 avail_mem=119.73 GB):  34%|███▍      | 20/58 [00:02<00:03, 11.45it/s]Capturing num tokens (num_tokens=960 avail_mem=119.72 GB):  34%|███▍      | 20/58 [00:03<00:03, 11.45it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=119.72 GB):  38%|███▊      | 22/58 [00:03<00:02, 13.14it/s]Capturing num tokens (num_tokens=896 avail_mem=119.72 GB):  38%|███▊      | 22/58 [00:03<00:02, 13.14it/s]Capturing num tokens (num_tokens=832 avail_mem=119.72 GB):  38%|███▊      | 22/58 [00:03<00:02, 13.14it/s]Capturing num tokens (num_tokens=832 avail_mem=119.72 GB):  41%|████▏     | 24/58 [00:03<00:02, 14.66it/s]Capturing num tokens (num_tokens=768 avail_mem=119.71 GB):  41%|████▏     | 24/58 [00:03<00:02, 14.66it/s]Capturing num tokens (num_tokens=704 avail_mem=119.71 GB):  41%|████▏     | 24/58 [00:03<00:02, 14.66it/s]Capturing num tokens (num_tokens=640 avail_mem=119.70 GB):  41%|████▏     | 24/58 [00:03<00:02, 14.66it/s]

    Capturing num tokens (num_tokens=640 avail_mem=119.70 GB):  47%|████▋     | 27/58 [00:03<00:01, 17.96it/s]Capturing num tokens (num_tokens=576 avail_mem=119.70 GB):  47%|████▋     | 27/58 [00:03<00:01, 17.96it/s]Capturing num tokens (num_tokens=512 avail_mem=119.70 GB):  47%|████▋     | 27/58 [00:03<00:01, 17.96it/s]Capturing num tokens (num_tokens=480 avail_mem=119.69 GB):  47%|████▋     | 27/58 [00:03<00:01, 17.96it/s]Capturing num tokens (num_tokens=480 avail_mem=119.69 GB):  52%|█████▏    | 30/58 [00:03<00:01, 20.59it/s]Capturing num tokens (num_tokens=448 avail_mem=119.69 GB):  52%|█████▏    | 30/58 [00:03<00:01, 20.59it/s]Capturing num tokens (num_tokens=416 avail_mem=119.69 GB):  52%|█████▏    | 30/58 [00:03<00:01, 20.59it/s]

    Capturing num tokens (num_tokens=384 avail_mem=122.29 GB):  52%|█████▏    | 30/58 [00:03<00:01, 20.59it/s]Capturing num tokens (num_tokens=384 avail_mem=122.29 GB):  57%|█████▋    | 33/58 [00:03<00:01, 17.99it/s]Capturing num tokens (num_tokens=352 avail_mem=122.28 GB):  57%|█████▋    | 33/58 [00:03<00:01, 17.99it/s]Capturing num tokens (num_tokens=320 avail_mem=122.28 GB):  57%|█████▋    | 33/58 [00:03<00:01, 17.99it/s]Capturing num tokens (num_tokens=288 avail_mem=122.28 GB):  57%|█████▋    | 33/58 [00:03<00:01, 17.99it/s]Capturing num tokens (num_tokens=256 avail_mem=122.27 GB):  57%|█████▋    | 33/58 [00:03<00:01, 17.99it/s]Capturing num tokens (num_tokens=256 avail_mem=122.27 GB):  64%|██████▍   | 37/58 [00:03<00:00, 22.71it/s]Capturing num tokens (num_tokens=240 avail_mem=122.27 GB):  64%|██████▍   | 37/58 [00:03<00:00, 22.71it/s]Capturing num tokens (num_tokens=224 avail_mem=122.27 GB):  64%|██████▍   | 37/58 [00:03<00:00, 22.71it/s]Capturing num tokens (num_tokens=208 avail_mem=122.26 GB):  64%|██████▍   | 37/58 [00:03<00:00, 22.71it/s]

    Capturing num tokens (num_tokens=192 avail_mem=122.26 GB):  64%|██████▍   | 37/58 [00:03<00:00, 22.71it/s]Capturing num tokens (num_tokens=192 avail_mem=122.26 GB):  71%|███████   | 41/58 [00:03<00:00, 26.67it/s]Capturing num tokens (num_tokens=176 avail_mem=122.25 GB):  71%|███████   | 41/58 [00:03<00:00, 26.67it/s]Capturing num tokens (num_tokens=160 avail_mem=122.25 GB):  71%|███████   | 41/58 [00:03<00:00, 26.67it/s]Capturing num tokens (num_tokens=144 avail_mem=122.25 GB):  71%|███████   | 41/58 [00:03<00:00, 26.67it/s]Capturing num tokens (num_tokens=128 avail_mem=122.26 GB):  71%|███████   | 41/58 [00:03<00:00, 26.67it/s]Capturing num tokens (num_tokens=128 avail_mem=122.26 GB):  78%|███████▊  | 45/58 [00:03<00:00, 29.71it/s]Capturing num tokens (num_tokens=112 avail_mem=122.25 GB):  78%|███████▊  | 45/58 [00:03<00:00, 29.71it/s]Capturing num tokens (num_tokens=96 avail_mem=122.25 GB):  78%|███████▊  | 45/58 [00:03<00:00, 29.71it/s] Capturing num tokens (num_tokens=80 avail_mem=122.24 GB):  78%|███████▊  | 45/58 [00:03<00:00, 29.71it/s]

    Capturing num tokens (num_tokens=64 avail_mem=122.24 GB):  78%|███████▊  | 45/58 [00:04<00:00, 29.71it/s]Capturing num tokens (num_tokens=64 avail_mem=122.24 GB):  84%|████████▍ | 49/58 [00:04<00:00, 31.96it/s]Capturing num tokens (num_tokens=48 avail_mem=122.24 GB):  84%|████████▍ | 49/58 [00:04<00:00, 31.96it/s]Capturing num tokens (num_tokens=32 avail_mem=122.23 GB):  84%|████████▍ | 49/58 [00:04<00:00, 31.96it/s]Capturing num tokens (num_tokens=28 avail_mem=122.23 GB):  84%|████████▍ | 49/58 [00:04<00:00, 31.96it/s]Capturing num tokens (num_tokens=24 avail_mem=122.23 GB):  84%|████████▍ | 49/58 [00:04<00:00, 31.96it/s]Capturing num tokens (num_tokens=24 avail_mem=122.23 GB):  91%|█████████▏| 53/58 [00:04<00:00, 33.71it/s]Capturing num tokens (num_tokens=20 avail_mem=122.22 GB):  91%|█████████▏| 53/58 [00:04<00:00, 33.71it/s]Capturing num tokens (num_tokens=16 avail_mem=122.22 GB):  91%|█████████▏| 53/58 [00:04<00:00, 33.71it/s]Capturing num tokens (num_tokens=12 avail_mem=122.22 GB):  91%|█████████▏| 53/58 [00:04<00:00, 33.71it/s]

    Capturing num tokens (num_tokens=8 avail_mem=122.21 GB):  91%|█████████▏| 53/58 [00:04<00:00, 33.71it/s] Capturing num tokens (num_tokens=8 avail_mem=122.21 GB):  98%|█████████▊| 57/58 [00:04<00:00, 35.05it/s]Capturing num tokens (num_tokens=4 avail_mem=122.21 GB):  98%|█████████▊| 57/58 [00:04<00:00, 35.05it/s]Capturing num tokens (num_tokens=4 avail_mem=122.21 GB): 100%|██████████| 58/58 [00:04<00:00, 13.56it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


    Server started on http://localhost:38539


Set the default backend. Note: Besides the local server, you may use also `OpenAI` or other API endpoints.


```python
set_default_backend(RuntimeEndpoint(f"http://localhost:{port}"))
```

    [2026-03-29 09:01:43] Endpoint '/get_model_info' is deprecated and will be removed in a future version. Please use '/model_info' instead.


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


<strong style='color: #00008B;'>Sure! Here are three countries along with their capitals:<br><br>1. **France** - Paris<br>2. **Germany** - Berlin<br>3. **Japan** - Tokyo</strong>


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


<strong style='color: #00008B;'>Sure! Here's a list of three countries along with their capitals:<br><br>1. **France** - Paris<br>2. **Japan** - Tokyo<br>3. **Germany** - Berlin</strong>



<strong style='color: #00008B;'>Certainly! Here's another list of three countries along with their capitals:<br><br>1. **Italy** - Rome<br>2. **Canada** - Ottawa<br>3. **Mexico** - Mexico City</strong>


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



<strong style='color: #00008B;'>2 * 2.<br><br>Let's calculate it without a calculator:<br>2 * 2 = 4<br><br>So, 2 * 2 equals 4.</strong>


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


<strong style='color: #00008B;'>### Two Tips for Staying Healthy:<br><br>1. **Balanced Diet:**<br>   - **Variety:** Include a wide range of foods such as fruits, vegetables, whole grains, lean proteins, and healthy fats.<br>   - **Nutrients:** Ensure you get essential vitamins and minerals that support immune function, bone health, and overall well-being.<br>   - **Quality:** Opt for whole, unprocessed foods to avoid excessive intake of sugars and unhealthy fats.<br><br>2. **Regular Exercise:**<br>   - **Types:** Incorporate a mix of aerobic activities (like walking, running, cycling), strength training, and flexibility exercises (like yoga or stretching).<br>   - **Frequency:** Aim for at least 150 minutes of moderate aerobic activity or 75 minutes of vigorous activity per week, plus strength training on two or more days a week.<br>   - **Benefits:** Enhances cardiovascular health, strengthens muscles and bones, improves mood, reduces stress, and promotes overall well-being.<br><br>By integrating both a balanced diet and regular exercise into your lifestyle, you can significantly enhance your health and quality of life.</strong>


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


<strong style='color: #00008B;'>{<br>    "name": "Harry Potter",<br>    "house": "Gryffindor",<br>    "blood status": "Half-blood",<br>    "occupation": "student",<br>    "wand": {<br>        "wood": "Holm Oak",<br>        "core": " phoenix feather",<br>        "length": 11.0<br>    },<br>    "alive": "Alive",<br>    "patronus": "Stag",<br>    "bogart": "Caucus_feather"<br>}</strong>


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

    100%|██████████| 3/3 [00:00<00:00, 26.46it/s]

    100%|██████████| 3/3 [00:00<00:00, 26.12it/s]

    



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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:176: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-29 09:01:58] numactl command not found, skipping NUMA node configuration for GPU. Install numactl (e.g., apt-get install numactl) to enable automatic NUMA binding.
    2026-03-29 09:01:58.317 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:01:58] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 09:01:58.317 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:01:58] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 09:01:58.317 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:01:58] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 09:01:58.317 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:01:58] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 09:01:58.317 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:01:58] Persistent cache disabled, using in-memory JIT cache


    [2026-03-29 09:02:05] numactl command not found, skipping NUMA node configuration for GPU. Install numactl (e.g., apt-get install numactl) to enable automatic NUMA binding.


    2026-03-29 09:02:08.925 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:02:08] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 09:02:08.926 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:02:08] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 09:02:08.926 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:02:08] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 09:02:08.926 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:02:08] Persistent cache disabled, using in-memory JIT cache
    2026-03-29 09:02:08.926 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-29 09:02:08] Persistent cache disabled, using in-memory JIT cache


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Loading safetensors checkpoint shards:   0% Completed | 0/5 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  20% Completed | 1/5 [00:00<00:02,  1.91it/s]

    Loading safetensors checkpoint shards:  40% Completed | 2/5 [00:01<00:01,  1.61it/s]

    Loading safetensors checkpoint shards:  60% Completed | 3/5 [00:01<00:01,  1.52it/s]

    Loading safetensors checkpoint shards:  80% Completed | 4/5 [00:02<00:00,  1.38it/s]

    Loading safetensors checkpoint shards: 100% Completed | 5/5 [00:03<00:00,  1.69it/s]Loading safetensors checkpoint shards: 100% Completed | 5/5 [00:03<00:00,  1.61it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


    Server started on http://localhost:34817



```python
set_default_backend(RuntimeEndpoint(f"http://localhost:{port}"))
```

    [2026-03-29 09:02:22] Endpoint '/get_model_info' is deprecated and will be removed in a future version. Please use '/model_info' instead.


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


<strong style='color: #00008B;'>The image shows a person standing next to a yellow vehicle, possibly a taxi, in an urban setting. The person is using an ironing board and ironing clothes. They appear to be working on a pair of blue jeans, although the clothes are not being ironed on the vehicle itself. The setting includes a city street with another yellow vehicle visible in the background, and there are buildings with large windows in the background. The scene suggests a unique or humorous approach to road maintenance, but it is not a commonly seen method.</strong>



```python
terminate_process(server_process)
```
