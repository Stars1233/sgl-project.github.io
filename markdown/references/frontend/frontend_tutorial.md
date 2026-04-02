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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    2026-04-02 16:15:02.191 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:15:02] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:15:02.191 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:15:02] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:15:02.191 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:15:02] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:15:02.191 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:15:02] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:15:02.191 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:15:02] Persistent cache disabled, using in-memory JIT cache


    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Multi-thread loading shards:  25% Completed | 1/4 [00:00<00:02,  1.44it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:01<00:01,  1.34it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:02<00:00,  1.34it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:03<00:00,  1.30it/s]Multi-thread loading shards: 100% Completed | 4/4 [00:03<00:00,  1.32it/s]


    2026-04-02 16:15:09,410 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 16:15:09] Unexpected error during package walk: cutlass.cute.experimental


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:50,  2.99s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:50,  2.99s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:22,  1.48s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:22,  1.48s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:03<00:49,  1.12it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:03<00:49,  1.12it/s]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:33,  1.63it/s]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:04<00:18,  2.87it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:04<00:18,  2.87it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:04<00:14,  3.55it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:04<00:14,  3.55it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:04<00:11,  4.33it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:04<00:11,  4.33it/s]Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:04<00:09,  5.21it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:04<00:09,  5.21it/s]

    Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:04<00:09,  5.21it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:04<00:06,  6.88it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:04<00:06,  6.88it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:04<00:06,  6.88it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:04<00:05,  8.38it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:04<00:05,  8.38it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:04<00:05,  8.38it/s]Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:04<00:04,  9.95it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:04<00:04,  9.95it/s]Compiling num tokens (num_tokens=2048):  26%|██▌       | 15/58 [00:05<00:04,  9.95it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:05<00:03, 11.69it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:05<00:03, 11.69it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:05<00:03, 11.69it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:05<00:02, 13.04it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:05<00:02, 13.04it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:05<00:02, 13.04it/s]

    Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:05<00:02, 14.16it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:05<00:02, 14.16it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:05<00:02, 14.16it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:05<00:02, 15.10it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:05<00:02, 15.10it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:05<00:02, 15.10it/s]

    Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:05<00:02, 15.97it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:05<00:02, 15.97it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:05<00:02, 15.97it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:05<00:02, 15.97it/s]Compiling num tokens (num_tokens=576):  48%|████▊     | 28/58 [00:05<00:01, 17.71it/s]Compiling num tokens (num_tokens=512):  48%|████▊     | 28/58 [00:05<00:01, 17.71it/s]Compiling num tokens (num_tokens=480):  48%|████▊     | 28/58 [00:05<00:01, 17.71it/s]

    Compiling num tokens (num_tokens=448):  48%|████▊     | 28/58 [00:05<00:01, 17.71it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:05<00:01, 19.56it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:05<00:01, 19.56it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:05<00:01, 19.56it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:05<00:01, 19.56it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:05<00:01, 20.65it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:05<00:01, 20.65it/s]

    Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:05<00:01, 20.65it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:06<00:01, 20.65it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:06<00:01, 19.45it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:06<00:01, 19.45it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:06<00:01, 19.45it/s]

    Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:06<00:00, 19.57it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:06<00:00, 19.57it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:06<00:00, 19.57it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:06<00:00, 18.86it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:06<00:00, 18.86it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:06<00:00, 18.86it/s]

    Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:06<00:00, 18.86it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:06<00:00, 19.86it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:06<00:00, 19.86it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:06<00:00, 19.86it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:06<00:00, 19.66it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:06<00:00, 19.66it/s] 

    Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:06<00:00, 19.66it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:06<00:00, 18.51it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:06<00:00, 18.51it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:06<00:00, 18.51it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:06<00:00, 18.13it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:06<00:00, 18.13it/s]

    Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:06<00:00, 18.13it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:06<00:00, 18.31it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:06<00:00, 18.31it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:06<00:00, 18.31it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:06<00:00, 18.31it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:07<00:00, 19.22it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:07<00:00, 19.22it/s]

    Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:07<00:00, 19.22it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:07<00:00, 19.22it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:07<00:00, 21.44it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:07<00:00,  8.11it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=24.33 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=24.33 GB):   2%|▏         | 1/58 [00:00<00:53,  1.07it/s]Capturing num tokens (num_tokens=7680 avail_mem=24.30 GB):   2%|▏         | 1/58 [00:00<00:53,  1.07it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=24.30 GB):   3%|▎         | 2/58 [00:01<00:48,  1.15it/s]Capturing num tokens (num_tokens=7168 avail_mem=24.30 GB):   3%|▎         | 2/58 [00:01<00:48,  1.15it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=24.30 GB):   5%|▌         | 3/58 [00:02<00:45,  1.22it/s]Capturing num tokens (num_tokens=6656 avail_mem=24.31 GB):   5%|▌         | 3/58 [00:02<00:45,  1.22it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=24.31 GB):   7%|▋         | 4/58 [00:03<00:37,  1.44it/s]Capturing num tokens (num_tokens=6144 avail_mem=24.31 GB):   7%|▋         | 4/58 [00:03<00:37,  1.44it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=24.31 GB):   9%|▊         | 5/58 [00:03<00:35,  1.49it/s]Capturing num tokens (num_tokens=5632 avail_mem=24.31 GB):   9%|▊         | 5/58 [00:03<00:35,  1.49it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=24.31 GB):  10%|█         | 6/58 [00:04<00:33,  1.57it/s]Capturing num tokens (num_tokens=5120 avail_mem=24.31 GB):  10%|█         | 6/58 [00:04<00:33,  1.57it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=24.31 GB):  12%|█▏        | 7/58 [00:04<00:30,  1.67it/s]Capturing num tokens (num_tokens=4608 avail_mem=24.32 GB):  12%|█▏        | 7/58 [00:04<00:30,  1.67it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=24.32 GB):  14%|█▍        | 8/58 [00:05<00:25,  1.95it/s]Capturing num tokens (num_tokens=4096 avail_mem=24.32 GB):  14%|█▍        | 8/58 [00:05<00:25,  1.95it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=24.32 GB):  16%|█▌        | 9/58 [00:05<00:21,  2.26it/s]Capturing num tokens (num_tokens=3840 avail_mem=23.70 GB):  16%|█▌        | 9/58 [00:05<00:21,  2.26it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=23.70 GB):  17%|█▋        | 10/58 [00:05<00:20,  2.40it/s]Capturing num tokens (num_tokens=3584 avail_mem=23.52 GB):  17%|█▋        | 10/58 [00:05<00:20,  2.40it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=23.52 GB):  19%|█▉        | 11/58 [00:06<00:18,  2.55it/s]Capturing num tokens (num_tokens=3328 avail_mem=23.52 GB):  19%|█▉        | 11/58 [00:06<00:18,  2.55it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=23.52 GB):  21%|██        | 12/58 [00:06<00:16,  2.78it/s]Capturing num tokens (num_tokens=3072 avail_mem=23.51 GB):  21%|██        | 12/58 [00:06<00:16,  2.78it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=23.51 GB):  22%|██▏       | 13/58 [00:06<00:14,  3.19it/s]Capturing num tokens (num_tokens=2816 avail_mem=23.51 GB):  22%|██▏       | 13/58 [00:06<00:14,  3.19it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=23.51 GB):  24%|██▍       | 14/58 [00:06<00:13,  3.17it/s]Capturing num tokens (num_tokens=2560 avail_mem=43.21 GB):  24%|██▍       | 14/58 [00:06<00:13,  3.17it/s]Capturing num tokens (num_tokens=2304 avail_mem=43.21 GB):  24%|██▍       | 14/58 [00:06<00:13,  3.17it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=43.21 GB):  28%|██▊       | 16/58 [00:07<00:09,  4.43it/s]Capturing num tokens (num_tokens=2048 avail_mem=42.17 GB):  28%|██▊       | 16/58 [00:07<00:09,  4.43it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=42.17 GB):  29%|██▉       | 17/58 [00:07<00:09,  4.43it/s]Capturing num tokens (num_tokens=1792 avail_mem=42.17 GB):  29%|██▉       | 17/58 [00:07<00:09,  4.43it/s]Capturing num tokens (num_tokens=1792 avail_mem=42.17 GB):  31%|███       | 18/58 [00:07<00:08,  4.61it/s]Capturing num tokens (num_tokens=1536 avail_mem=43.17 GB):  31%|███       | 18/58 [00:07<00:08,  4.61it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=43.17 GB):  33%|███▎      | 19/58 [00:07<00:08,  4.86it/s]Capturing num tokens (num_tokens=1280 avail_mem=42.35 GB):  33%|███▎      | 19/58 [00:07<00:08,  4.86it/s]Capturing num tokens (num_tokens=1280 avail_mem=42.35 GB):  34%|███▍      | 20/58 [00:07<00:07,  5.07it/s]Capturing num tokens (num_tokens=1024 avail_mem=43.18 GB):  34%|███▍      | 20/58 [00:07<00:07,  5.07it/s]

    Capturing num tokens (num_tokens=960 avail_mem=43.17 GB):  34%|███▍      | 20/58 [00:08<00:07,  5.07it/s] Capturing num tokens (num_tokens=960 avail_mem=43.17 GB):  38%|███▊      | 22/58 [00:08<00:06,  5.68it/s]Capturing num tokens (num_tokens=896 avail_mem=42.40 GB):  38%|███▊      | 22/58 [00:08<00:06,  5.68it/s]

    Capturing num tokens (num_tokens=896 avail_mem=42.40 GB):  40%|███▉      | 23/58 [00:08<00:05,  6.02it/s]Capturing num tokens (num_tokens=832 avail_mem=42.40 GB):  40%|███▉      | 23/58 [00:08<00:05,  6.02it/s]Capturing num tokens (num_tokens=832 avail_mem=42.40 GB):  41%|████▏     | 24/58 [00:08<00:05,  6.48it/s]Capturing num tokens (num_tokens=768 avail_mem=43.16 GB):  41%|████▏     | 24/58 [00:08<00:05,  6.48it/s]

    Capturing num tokens (num_tokens=768 avail_mem=43.16 GB):  43%|████▎     | 25/58 [00:08<00:04,  7.04it/s]Capturing num tokens (num_tokens=704 avail_mem=42.45 GB):  43%|████▎     | 25/58 [00:08<00:04,  7.04it/s]Capturing num tokens (num_tokens=704 avail_mem=42.45 GB):  45%|████▍     | 26/58 [00:08<00:04,  7.28it/s]Capturing num tokens (num_tokens=640 avail_mem=42.45 GB):  45%|████▍     | 26/58 [00:08<00:04,  7.28it/s]

    Capturing num tokens (num_tokens=640 avail_mem=42.45 GB):  47%|████▋     | 27/58 [00:08<00:04,  7.71it/s]Capturing num tokens (num_tokens=576 avail_mem=43.16 GB):  47%|████▋     | 27/58 [00:08<00:04,  7.71it/s]Capturing num tokens (num_tokens=512 avail_mem=42.49 GB):  47%|████▋     | 27/58 [00:08<00:04,  7.71it/s]

    Capturing num tokens (num_tokens=512 avail_mem=42.49 GB):  50%|█████     | 29/58 [00:08<00:03,  8.36it/s]Capturing num tokens (num_tokens=480 avail_mem=42.49 GB):  50%|█████     | 29/58 [00:08<00:03,  8.36it/s]Capturing num tokens (num_tokens=448 avail_mem=43.15 GB):  50%|█████     | 29/58 [00:09<00:03,  8.36it/s]Capturing num tokens (num_tokens=448 avail_mem=43.15 GB):  53%|█████▎    | 31/58 [00:09<00:03,  8.96it/s]Capturing num tokens (num_tokens=416 avail_mem=42.54 GB):  53%|█████▎    | 31/58 [00:09<00:03,  8.96it/s]

    Capturing num tokens (num_tokens=416 avail_mem=42.54 GB):  55%|█████▌    | 32/58 [00:09<00:02,  8.98it/s]Capturing num tokens (num_tokens=384 avail_mem=42.54 GB):  55%|█████▌    | 32/58 [00:09<00:02,  8.98it/s]Capturing num tokens (num_tokens=352 avail_mem=43.14 GB):  55%|█████▌    | 32/58 [00:09<00:02,  8.98it/s]Capturing num tokens (num_tokens=352 avail_mem=43.14 GB):  59%|█████▊    | 34/58 [00:09<00:02,  9.57it/s]Capturing num tokens (num_tokens=320 avail_mem=42.59 GB):  59%|█████▊    | 34/58 [00:09<00:02,  9.57it/s]

    Capturing num tokens (num_tokens=288 avail_mem=43.14 GB):  59%|█████▊    | 34/58 [00:09<00:02,  9.57it/s]Capturing num tokens (num_tokens=288 avail_mem=43.14 GB):  62%|██████▏   | 36/58 [00:09<00:02, 10.46it/s]Capturing num tokens (num_tokens=256 avail_mem=42.64 GB):  62%|██████▏   | 36/58 [00:09<00:02, 10.46it/s]

    Capturing num tokens (num_tokens=240 avail_mem=43.14 GB):  62%|██████▏   | 36/58 [00:09<00:02, 10.46it/s]Capturing num tokens (num_tokens=240 avail_mem=43.14 GB):  66%|██████▌   | 38/58 [00:09<00:01, 10.31it/s]Capturing num tokens (num_tokens=224 avail_mem=43.13 GB):  66%|██████▌   | 38/58 [00:09<00:01, 10.31it/s]Capturing num tokens (num_tokens=208 avail_mem=42.69 GB):  66%|██████▌   | 38/58 [00:09<00:01, 10.31it/s]

    Capturing num tokens (num_tokens=208 avail_mem=42.69 GB):  69%|██████▉   | 40/58 [00:10<00:01, 10.72it/s]Capturing num tokens (num_tokens=192 avail_mem=43.13 GB):  69%|██████▉   | 40/58 [00:10<00:01, 10.72it/s]Capturing num tokens (num_tokens=176 avail_mem=42.71 GB):  69%|██████▉   | 40/58 [00:10<00:01, 10.72it/s]Capturing num tokens (num_tokens=176 avail_mem=42.71 GB):  72%|███████▏  | 42/58 [00:10<00:01, 11.11it/s]Capturing num tokens (num_tokens=160 avail_mem=42.71 GB):  72%|███████▏  | 42/58 [00:10<00:01, 11.11it/s]

    Capturing num tokens (num_tokens=144 avail_mem=43.11 GB):  72%|███████▏  | 42/58 [00:10<00:01, 11.11it/s]Capturing num tokens (num_tokens=144 avail_mem=43.11 GB):  76%|███████▌  | 44/58 [00:10<00:01, 11.82it/s]Capturing num tokens (num_tokens=128 avail_mem=42.74 GB):  76%|███████▌  | 44/58 [00:10<00:01, 11.82it/s]Capturing num tokens (num_tokens=112 avail_mem=43.12 GB):  76%|███████▌  | 44/58 [00:10<00:01, 11.82it/s]

    Capturing num tokens (num_tokens=112 avail_mem=43.12 GB):  79%|███████▉  | 46/58 [00:10<00:00, 12.31it/s]Capturing num tokens (num_tokens=96 avail_mem=42.76 GB):  79%|███████▉  | 46/58 [00:10<00:00, 12.31it/s] Capturing num tokens (num_tokens=80 avail_mem=43.11 GB):  79%|███████▉  | 46/58 [00:10<00:00, 12.31it/s]Capturing num tokens (num_tokens=80 avail_mem=43.11 GB):  83%|████████▎ | 48/58 [00:10<00:00, 13.06it/s]Capturing num tokens (num_tokens=64 avail_mem=42.78 GB):  83%|████████▎ | 48/58 [00:10<00:00, 13.06it/s]

    Capturing num tokens (num_tokens=48 avail_mem=43.11 GB):  83%|████████▎ | 48/58 [00:10<00:00, 13.06it/s]Capturing num tokens (num_tokens=48 avail_mem=43.11 GB):  86%|████████▌ | 50/58 [00:10<00:00, 13.38it/s]Capturing num tokens (num_tokens=32 avail_mem=42.80 GB):  86%|████████▌ | 50/58 [00:10<00:00, 13.38it/s]Capturing num tokens (num_tokens=28 avail_mem=43.10 GB):  86%|████████▌ | 50/58 [00:10<00:00, 13.38it/s]Capturing num tokens (num_tokens=28 avail_mem=43.10 GB):  90%|████████▉ | 52/58 [00:10<00:00, 13.76it/s]Capturing num tokens (num_tokens=24 avail_mem=42.82 GB):  90%|████████▉ | 52/58 [00:10<00:00, 13.76it/s]

    Capturing num tokens (num_tokens=20 avail_mem=43.09 GB):  90%|████████▉ | 52/58 [00:10<00:00, 13.76it/s]Capturing num tokens (num_tokens=20 avail_mem=43.09 GB):  93%|█████████▎| 54/58 [00:11<00:00, 14.25it/s]Capturing num tokens (num_tokens=16 avail_mem=42.84 GB):  93%|█████████▎| 54/58 [00:11<00:00, 14.25it/s]Capturing num tokens (num_tokens=12 avail_mem=43.08 GB):  93%|█████████▎| 54/58 [00:11<00:00, 14.25it/s]Capturing num tokens (num_tokens=12 avail_mem=43.08 GB):  97%|█████████▋| 56/58 [00:11<00:00, 14.79it/s]Capturing num tokens (num_tokens=8 avail_mem=43.08 GB):  97%|█████████▋| 56/58 [00:11<00:00, 14.79it/s] 

    Capturing num tokens (num_tokens=4 avail_mem=42.88 GB):  97%|█████████▋| 56/58 [00:11<00:00, 14.79it/s]Capturing num tokens (num_tokens=4 avail_mem=42.88 GB): 100%|██████████| 58/58 [00:11<00:00, 16.03it/s]Capturing num tokens (num_tokens=4 avail_mem=42.88 GB): 100%|██████████| 58/58 [00:11<00:00,  5.16it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


    Server started on http://localhost:35877


Set the default backend. Note: Besides the local server, you may use also `OpenAI` or other API endpoints.


```python
set_default_backend(RuntimeEndpoint(f"http://localhost:{port}"))
```

    [2026-04-02 16:15:35] Endpoint '/get_model_info' is deprecated and will be removed in a future version. Please use '/model_info' instead.


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


<strong style='color: #00008B;'>Sure! Here are three countries along with their capitals:<br><br>1. **France** - Paris<br>2. **Australia** - Canberra<br>3. **Japan** - Tokyo</strong>


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


<strong style='color: #00008B;'>Certainly! Here are three countries along with their respective capitals:<br><br>1. **France** - Paris<br>2. **Australia** - Canberra<br>3. **Canada** - Ottawa</strong>



<strong style='color: #00008B;'>Of course! Here’s another list of three countries and their capitals:<br><br>1. **India** - New Delhi<br>2. **Japan** - Tokyo<br>3. **Germany** - Berlin</strong>


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



<strong style='color: #00008B;'>2 * 2.<br><br>Let's perform the calculation:<br><br>2 * 2 = 4<br><br>Thus, the result of 2 * 2 is 4.<br><br>No calculator was needed for this simple multiplication.</strong>


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


<strong style='color: #00008B;'>1. **Balanced Diet**: Consuming a variety of foods in the right proportions to meet your nutritional needs. Include plenty of fruits, vegetables, whole grains, lean proteins, and healthy fats. Limit processed foods, sugars, and unhealthy fats.<br><br>2. **Regular Exercise**: Engaging in physical activity regularly to maintain overall health. Aim for at least 150 minutes of moderate aerobic activity or 75 minutes of vigorous aerobic activity per week, combined with strength training exercises. This helps in maintaining a healthy weight, reducing chronic diseases, improving mental health, and enhancing energy levels.<br><br>Both are crucial for a healthy lifestyle!</strong>


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


<strong style='color: #00008B;'>{<br>    "name": "Harry Potter",<br>    "house": "Gryffindor",<br>    "blood status": "Half-blood",<br>    "occupation": "student",<br>    "wand": {<br>        "wood": "Holly",<br>        "core": "Phoenix feather",<br>        "length": 11.0<br>    },<br>    "alive": "Alive",<br>    "patronus": "Stag",<br>    "bogart": "Nicolas Flamel"<br>}</strong>


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

    100%|██████████| 3/3 [00:00<00:00, 31.93it/s]

    



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


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    2026-04-02 16:15:47.866 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:15:47] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:15:47.866 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:15:47] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:15:47.866 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:15:47] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:15:47.866 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:15:47] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:15:47.866 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:15:47] Persistent cache disabled, using in-memory JIT cache


    2026-04-02 16:15:56.947 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:15:56] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:15:56.947 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:15:56] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:15:56.947 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:15:56] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:15:56.947 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:15:56] Persistent cache disabled, using in-memory JIT cache
    2026-04-02 16:15:56.947 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-04-02 16:15:56] Persistent cache disabled, using in-memory JIT cache


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Multi-thread loading shards:   0% Completed | 0/5 [00:00<?, ?it/s]

    Multi-thread loading shards:  20% Completed | 1/5 [00:00<00:02,  1.68it/s]

    Multi-thread loading shards:  40% Completed | 2/5 [00:01<00:02,  1.43it/s]

    Multi-thread loading shards:  60% Completed | 3/5 [00:01<00:01,  1.88it/s]

    Multi-thread loading shards:  80% Completed | 4/5 [00:02<00:00,  1.70it/s]

    Multi-thread loading shards: 100% Completed | 5/5 [00:03<00:00,  1.44it/s]Multi-thread loading shards: 100% Completed | 5/5 [00:03<00:00,  1.54it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)


    2026-04-02 16:16:09,060 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-02 16:16:09] Unexpected error during package walk: cutlass.cute.experimental



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


    Server started on http://localhost:38329



```python
set_default_backend(RuntimeEndpoint(f"http://localhost:{port}"))
```

    [2026-04-02 16:16:13] Endpoint '/get_model_info' is deprecated and will be removed in a future version. Please use '/model_info' instead.


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



<strong style='color: #00008B;'>The image shows a person ironing clothes on the tailgate of a taxi cab in an urban street setting. The person is using a handheld iron and has a metal ironing board placed on the back of the vehicle. The atmosphere suggests activity in a busy city area, with yellow taxis and other vehicles visible in the background.</strong>



```python
terminate_process(server_process)
```
