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

    [2026-03-24 06:42:38] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-24 06:42:38] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-24 06:42:38] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-24 06:42:43] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-24 06:42:43] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-24 06:42:43] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    [2026-03-24 06:42:45] INFO server_args.py:2240: Attention backend not specified. Use fa3 backend by default.
    [2026-03-24 06:42:45] INFO server_args.py:3523: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-24 06:42:50] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-24 06:42:50] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-24 06:42:50] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-24 06:42:50] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-24 06:42:50] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-24 06:42:50] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-24 06:42:54.291 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 06:42:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 06:42:54.291 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 06:42:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 06:42:54.291 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 06:42:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 06:42:54.291 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 06:42:54] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 06:42:54.291 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 06:42:54] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  25% Completed | 1/4 [00:00<00:01,  2.15it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:01<00:01,  1.63it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:01<00:00,  1.52it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.49it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.55it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:25,  3.60s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:25,  3.60s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:42,  1.84s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:42,  1.84s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:04<01:02,  1.13s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:04<01:02,  1.13s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:04<00:42,  1.26it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:04<00:42,  1.26it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:31,  1.69it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:31,  1.69it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:05<00:24,  2.12it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:05<00:24,  2.12it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:05<00:19,  2.63it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:05<00:19,  2.63it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:05<00:15,  3.15it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:05<00:15,  3.15it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:05<00:13,  3.74it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:05<00:13,  3.74it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:05<00:10,  4.37it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:05<00:10,  4.37it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:06<00:09,  4.99it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:06<00:09,  4.99it/s]

    Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:06<00:08,  5.64it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:06<00:08,  5.64it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:06<00:07,  6.31it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:06<00:07,  6.31it/s]

    Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:06<00:06,  7.00it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:06<00:06,  7.00it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:06<00:06,  7.00it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:06<00:04,  8.41it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:06<00:04,  8.41it/s]

    Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:06<00:04,  8.41it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:06<00:03, 10.18it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:06<00:03, 10.18it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:06<00:03, 10.18it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:06<00:03, 12.22it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:06<00:03, 12.22it/s]

    Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:06<00:03, 12.22it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:06<00:03, 12.22it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:06<00:02, 15.70it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:06<00:02, 15.70it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:06<00:02, 15.70it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:07<00:02, 15.70it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:07<00:02, 15.70it/s]

    Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:07<00:01, 20.49it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:07<00:01, 20.49it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:07<00:01, 20.49it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:07<00:01, 20.49it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:07<00:01, 20.49it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:07<00:01, 25.07it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:07<00:01, 25.07it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:07<00:01, 25.07it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:07<00:01, 25.07it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:07<00:01, 25.07it/s]

    Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:07<00:01, 25.07it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:07<00:00, 30.05it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:07<00:00, 30.05it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:07<00:00, 30.05it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:07<00:00, 30.05it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:07<00:00, 30.05it/s]Compiling num tokens (num_tokens=192):  62%|██████▏   | 36/58 [00:07<00:00, 30.05it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:07<00:00, 34.24it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:07<00:00, 34.24it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:07<00:00, 34.24it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:07<00:00, 34.24it/s]

    Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:07<00:00, 34.24it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:07<00:00, 34.24it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:07<00:00, 37.29it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:07<00:00, 37.29it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:07<00:00, 37.29it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:07<00:00, 37.29it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:07<00:00, 37.29it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:07<00:00, 37.29it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:07<00:00, 40.27it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:07<00:00, 40.27it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:07<00:00, 40.27it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:07<00:00, 40.27it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:07<00:00, 40.27it/s]

    Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:07<00:00, 40.27it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:07<00:00, 40.27it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:07<00:00, 40.27it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:07<00:00, 47.39it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:07<00:00,  7.50it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=117.15 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=117.15 GB):   2%|▏         | 1/58 [00:00<00:22,  2.52it/s]Capturing num tokens (num_tokens=7680 avail_mem=117.12 GB):   2%|▏         | 1/58 [00:00<00:22,  2.52it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=117.12 GB):   3%|▎         | 2/58 [00:00<00:20,  2.68it/s]Capturing num tokens (num_tokens=7168 avail_mem=117.12 GB):   3%|▎         | 2/58 [00:00<00:20,  2.68it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=117.12 GB):   5%|▌         | 3/58 [00:01<00:19,  2.81it/s]Capturing num tokens (num_tokens=6656 avail_mem=117.12 GB):   5%|▌         | 3/58 [00:01<00:19,  2.81it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=117.12 GB):   7%|▋         | 4/58 [00:01<00:17,  3.04it/s]Capturing num tokens (num_tokens=6144 avail_mem=117.12 GB):   7%|▋         | 4/58 [00:01<00:17,  3.04it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=117.12 GB):   9%|▊         | 5/58 [00:01<00:16,  3.19it/s]Capturing num tokens (num_tokens=5632 avail_mem=117.09 GB):   9%|▊         | 5/58 [00:01<00:16,  3.19it/s]Capturing num tokens (num_tokens=5632 avail_mem=117.09 GB):  10%|█         | 6/58 [00:01<00:14,  3.64it/s]Capturing num tokens (num_tokens=5120 avail_mem=117.09 GB):  10%|█         | 6/58 [00:01<00:14,  3.64it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=117.09 GB):  12%|█▏        | 7/58 [00:02<00:13,  3.70it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.72 GB):  12%|█▏        | 7/58 [00:02<00:13,  3.70it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.72 GB):  14%|█▍        | 8/58 [00:02<00:11,  4.37it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.73 GB):  14%|█▍        | 8/58 [00:02<00:11,  4.37it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=119.73 GB):  16%|█▌        | 9/58 [00:02<00:09,  5.03it/s]Capturing num tokens (num_tokens=3840 avail_mem=119.73 GB):  16%|█▌        | 9/58 [00:02<00:09,  5.03it/s]Capturing num tokens (num_tokens=3840 avail_mem=119.73 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.67it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.73 GB):  17%|█▋        | 10/58 [00:02<00:08,  5.67it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=119.73 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.35it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.73 GB):  19%|█▉        | 11/58 [00:02<00:07,  6.35it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.73 GB):  21%|██        | 12/58 [00:02<00:06,  7.07it/s]Capturing num tokens (num_tokens=3072 avail_mem=119.73 GB):  21%|██        | 12/58 [00:02<00:06,  7.07it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=119.73 GB):  21%|██        | 12/58 [00:02<00:06,  7.07it/s]Capturing num tokens (num_tokens=2816 avail_mem=119.73 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.40it/s]Capturing num tokens (num_tokens=2560 avail_mem=119.73 GB):  24%|██▍       | 14/58 [00:02<00:05,  8.40it/s]Capturing num tokens (num_tokens=2304 avail_mem=119.73 GB):  24%|██▍       | 14/58 [00:03<00:05,  8.40it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=119.73 GB):  28%|██▊       | 16/58 [00:03<00:04,  9.81it/s]Capturing num tokens (num_tokens=2048 avail_mem=119.73 GB):  28%|██▊       | 16/58 [00:03<00:04,  9.81it/s]Capturing num tokens (num_tokens=1792 avail_mem=119.73 GB):  28%|██▊       | 16/58 [00:03<00:04,  9.81it/s]Capturing num tokens (num_tokens=1792 avail_mem=119.73 GB):  31%|███       | 18/58 [00:03<00:03, 11.44it/s]Capturing num tokens (num_tokens=1536 avail_mem=119.72 GB):  31%|███       | 18/58 [00:03<00:03, 11.44it/s]Capturing num tokens (num_tokens=1280 avail_mem=119.73 GB):  31%|███       | 18/58 [00:03<00:03, 11.44it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=119.73 GB):  31%|███       | 18/58 [00:03<00:03, 11.44it/s]Capturing num tokens (num_tokens=1024 avail_mem=119.73 GB):  36%|███▌      | 21/58 [00:03<00:02, 14.43it/s]Capturing num tokens (num_tokens=960 avail_mem=119.72 GB):  36%|███▌      | 21/58 [00:03<00:02, 14.43it/s] Capturing num tokens (num_tokens=896 avail_mem=119.72 GB):  36%|███▌      | 21/58 [00:03<00:02, 14.43it/s]Capturing num tokens (num_tokens=832 avail_mem=119.72 GB):  36%|███▌      | 21/58 [00:03<00:02, 14.43it/s]Capturing num tokens (num_tokens=832 avail_mem=119.72 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.44it/s]Capturing num tokens (num_tokens=768 avail_mem=119.71 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.44it/s]Capturing num tokens (num_tokens=704 avail_mem=119.71 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.44it/s]

    Capturing num tokens (num_tokens=640 avail_mem=119.70 GB):  41%|████▏     | 24/58 [00:03<00:01, 17.44it/s]Capturing num tokens (num_tokens=640 avail_mem=119.70 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.39it/s]Capturing num tokens (num_tokens=576 avail_mem=119.70 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.39it/s]Capturing num tokens (num_tokens=512 avail_mem=119.70 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.39it/s]Capturing num tokens (num_tokens=480 avail_mem=119.69 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.39it/s]Capturing num tokens (num_tokens=448 avail_mem=119.69 GB):  47%|████▋     | 27/58 [00:03<00:01, 20.39it/s]Capturing num tokens (num_tokens=448 avail_mem=119.69 GB):  53%|█████▎    | 31/58 [00:03<00:01, 24.13it/s]Capturing num tokens (num_tokens=416 avail_mem=119.69 GB):  53%|█████▎    | 31/58 [00:03<00:01, 24.13it/s]Capturing num tokens (num_tokens=384 avail_mem=119.68 GB):  53%|█████▎    | 31/58 [00:03<00:01, 24.13it/s]

    Capturing num tokens (num_tokens=352 avail_mem=119.68 GB):  53%|█████▎    | 31/58 [00:03<00:01, 24.13it/s]Capturing num tokens (num_tokens=320 avail_mem=119.67 GB):  53%|█████▎    | 31/58 [00:03<00:01, 24.13it/s]Capturing num tokens (num_tokens=320 avail_mem=119.67 GB):  60%|██████    | 35/58 [00:03<00:00, 27.31it/s]Capturing num tokens (num_tokens=288 avail_mem=119.67 GB):  60%|██████    | 35/58 [00:03<00:00, 27.31it/s]Capturing num tokens (num_tokens=256 avail_mem=119.67 GB):  60%|██████    | 35/58 [00:03<00:00, 27.31it/s]Capturing num tokens (num_tokens=240 avail_mem=119.66 GB):  60%|██████    | 35/58 [00:03<00:00, 27.31it/s]Capturing num tokens (num_tokens=224 avail_mem=119.66 GB):  60%|██████    | 35/58 [00:03<00:00, 27.31it/s]Capturing num tokens (num_tokens=224 avail_mem=119.66 GB):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Capturing num tokens (num_tokens=208 avail_mem=119.65 GB):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Capturing num tokens (num_tokens=192 avail_mem=119.65 GB):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]

    Capturing num tokens (num_tokens=176 avail_mem=119.65 GB):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Capturing num tokens (num_tokens=160 avail_mem=119.65 GB):  67%|██████▋   | 39/58 [00:03<00:00, 30.01it/s]Capturing num tokens (num_tokens=160 avail_mem=119.65 GB):  74%|███████▍  | 43/58 [00:04<00:00, 32.16it/s]Capturing num tokens (num_tokens=144 avail_mem=119.64 GB):  74%|███████▍  | 43/58 [00:04<00:00, 32.16it/s]Capturing num tokens (num_tokens=128 avail_mem=119.65 GB):  74%|███████▍  | 43/58 [00:04<00:00, 32.16it/s]Capturing num tokens (num_tokens=112 avail_mem=119.65 GB):  74%|███████▍  | 43/58 [00:04<00:00, 32.16it/s]Capturing num tokens (num_tokens=96 avail_mem=119.64 GB):  74%|███████▍  | 43/58 [00:04<00:00, 32.16it/s] Capturing num tokens (num_tokens=96 avail_mem=119.64 GB):  81%|████████  | 47/58 [00:04<00:00, 32.77it/s]Capturing num tokens (num_tokens=80 avail_mem=119.64 GB):  81%|████████  | 47/58 [00:04<00:00, 32.77it/s]Capturing num tokens (num_tokens=64 avail_mem=119.63 GB):  81%|████████  | 47/58 [00:04<00:00, 32.77it/s]

    Capturing num tokens (num_tokens=48 avail_mem=119.63 GB):  81%|████████  | 47/58 [00:04<00:00, 32.77it/s]Capturing num tokens (num_tokens=32 avail_mem=119.63 GB):  81%|████████  | 47/58 [00:04<00:00, 32.77it/s]Capturing num tokens (num_tokens=32 avail_mem=119.63 GB):  88%|████████▊ | 51/58 [00:04<00:00, 33.77it/s]Capturing num tokens (num_tokens=28 avail_mem=119.63 GB):  88%|████████▊ | 51/58 [00:04<00:00, 33.77it/s]Capturing num tokens (num_tokens=24 avail_mem=119.62 GB):  88%|████████▊ | 51/58 [00:04<00:00, 33.77it/s]Capturing num tokens (num_tokens=20 avail_mem=119.62 GB):  88%|████████▊ | 51/58 [00:04<00:00, 33.77it/s]Capturing num tokens (num_tokens=16 avail_mem=119.62 GB):  88%|████████▊ | 51/58 [00:04<00:00, 33.77it/s]Capturing num tokens (num_tokens=16 avail_mem=119.62 GB):  95%|█████████▍| 55/58 [00:04<00:00, 34.68it/s]Capturing num tokens (num_tokens=12 avail_mem=119.61 GB):  95%|█████████▍| 55/58 [00:04<00:00, 34.68it/s]Capturing num tokens (num_tokens=8 avail_mem=119.61 GB):  95%|█████████▍| 55/58 [00:04<00:00, 34.68it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=119.60 GB):  95%|█████████▍| 55/58 [00:04<00:00, 34.68it/s]Capturing num tokens (num_tokens=4 avail_mem=119.60 GB): 100%|██████████| 58/58 [00:04<00:00, 13.11it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:116: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


    Server started on http://localhost:36543


Set the default backend. Note: Besides the local server, you may use also `OpenAI` or other API endpoints.


```python
set_default_backend(RuntimeEndpoint(f"http://localhost:{port}"))
```

    [2026-03-24 06:43:19] Endpoint '/get_model_info' is deprecated and will be removed in a future version. Please use '/model_info' instead.


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


<strong style='color: #00008B;'>Sure! Here are three countries along with their respective capitals:<br><br>1. France - Paris<br>2. Japan - Tokyo<br>3. Australia - Canberra</strong>


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


<strong style='color: #00008B;'>Certainly! Here's a list of three countries along with their capitals:<br><br>1. **France** - Paris<br>2. **Germany** - Berlin<br>3. **Italy** - Rome</strong>



<strong style='color: #00008B;'>Of course! Here's another list of three countries and their capitals:<br><br>1. **Japan** - Tokyo<br>2. **Australia** - Canberra<br>3. **Sweden** - Stockholm</strong>


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



<strong style='color: #00008B;'>2 * 2. <br><br>Let's calculate it without needing a calculator:<br><br>\[ 2 * 2 = 4 \]<br><br>So, \( 2 * 2 = 4 \).</strong>


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


<strong style='color: #00008B;'>1. **Balanced Diet:** Consuming a variety of nutrient-rich foods including fruits, vegetables, proteins, healthy fats, and complex carbohydrates is essential. This ensures your body gets all the necessary vitamins, minerals, and energy needed for optimal health.<br>2. **Regular Exercise:** Engaging in physical activity on a regular basis, such as walking, jogging, or strength training, helps improve cardiovascular health, boosts immune function, and enhances mental well-being. Aim for at least 150 minutes of moderate aerobic activity or 75 minutes of vigorous activity each week, along with muscle-strengthening exercises.<br><br>By combining these two strategies, you can significantly enhance your overall health and quality of life.</strong>


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


<strong style='color: #00008B;'>{<br>    "name": "Harry Potter",<br>    "house": "Gryffindor",<br>    "blood status": "Half-blood",<br>    "occupation": "student",<br>    "wand": {<br>        "wood": "holly",<br>        "core": "phoenix feather",<br>        "length": 11.0<br>    },<br>    "alive": "Alive",<br>    "patronus": "Stag",<br>    "bogart": "Dementor"<br>}</strong>


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

     33%|███▎      | 1/3 [00:00<00:00,  9.43it/s]

     67%|██████▋   | 2/3 [00:00<00:00, 14.90it/s]

    100%|██████████| 3/3 [00:00<00:00, 23.92it/s]

    



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

    [2026-03-24 06:43:31] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-24 06:43:31] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-24 06:43:31] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    [2026-03-24 06:43:33] INFO server_args.py:2240: Attention backend not specified. Use fa3 backend by default.
    [2026-03-24 06:43:33] INFO server_args.py:3523: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    2026-03-24 06:43:34.480 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 06:43:34] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 06:43:34.480 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 06:43:34] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 06:43:34.480 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 06:43:34] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 06:43:34.480 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 06:43:34] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 06:43:34.480 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 06:43:34] Persistent cache disabled, using in-memory JIT cache


    [2026-03-24 06:43:38] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-24 06:43:38] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-24 06:43:38] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-24 06:43:38] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-24 06:43:38] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-24 06:43:38] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-03-24 06:43:44.869 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 06:43:44] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 06:43:44.869 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 06:43:44] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 06:43:44.869 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 06:43:44] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 06:43:44.869 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 06:43:44] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 06:43:44.869 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 06:43:44] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/5 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  20% Completed | 1/5 [00:00<00:02,  1.73it/s]

    Loading safetensors checkpoint shards:  40% Completed | 2/5 [00:00<00:01,  2.07it/s]

    Loading safetensors checkpoint shards:  60% Completed | 3/5 [00:01<00:01,  1.81it/s]

    Loading safetensors checkpoint shards:  80% Completed | 4/5 [00:02<00:00,  1.54it/s]

    Loading safetensors checkpoint shards: 100% Completed | 5/5 [00:03<00:00,  1.37it/s]Loading safetensors checkpoint shards: 100% Completed | 5/5 [00:03<00:00,  1.52it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:116: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


    Server started on http://localhost:34161



```python
set_default_backend(RuntimeEndpoint(f"http://localhost:{port}"))
```

    [2026-03-24 06:43:59] Endpoint '/get_model_info' is deprecated and will be removed in a future version. Please use '/model_info' instead.


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


<strong style='color: #00008B;'>The image shows a public utility worker on the back of a yellow SUV, using an iron (or similar tool) and an ironing board to iron a piece of clothing. The setting appears to be in a city with visible yellow taxis, suggesting a busy urban environment, possibly New York City. The worker is working on a busy street, which could be part of a new York City pilot program for clothing maintenance services.</strong>



```python
terminate_process(server_process)
```
