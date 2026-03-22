# OpenAI APIs - Completions

SGLang provides OpenAI-compatible APIs to enable a smooth transition from OpenAI services to self-hosted local models.
A complete reference for the API is available in the [OpenAI API Reference](https://platform.openai.com/docs/api-reference).

This tutorial covers the following popular APIs:

- `chat/completions`
- `completions`

Check out other tutorials to learn about [vision APIs](openai_api_vision.ipynb) for vision-language models and [embedding APIs](openai_api_embeddings.ipynb) for embedding models.

## Launch A Server

Launch the server in your terminal and wait for it to initialize.


```python
from sglang.test.doc_patch import launch_server_cmd
from sglang.utils import wait_for_server, print_highlight, terminate_process

server_process, port = launch_server_cmd(
    "python3 -m sglang.launch_server --model-path qwen/qwen2.5-0.5b-instruct --host 0.0.0.0 --log-level warning"
)

wait_for_server(f"http://localhost:{port}", process=server_process)
print(f"Server started on http://localhost:{port}")
```

    [2026-03-22 18:20:01] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-22 18:20:01] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-22 18:20:01] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-22 18:20:05] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-22 18:20:05] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-22 18:20:05] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    [2026-03-22 18:20:07] WARNING model_config.py:1098: Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.
    [2026-03-22 18:20:07] INFO server_args.py:2239: Attention backend not specified. Use fa3 backend by default.


    [2026-03-22 18:20:07] INFO server_args.py:3522: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-22 18:20:08] Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-22 18:20:13] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-22 18:20:13] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-22 18:20:13] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-22 18:20:13] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-22 18:20:13] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-22 18:20:13] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-22 18:20:15] Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-22 18:20:17] Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.13it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.12it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:22,  2.49s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:22,  2.49s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:22,  2.49s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:22,  2.49s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:27,  2.00it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:27,  2.00it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:27,  2.00it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:27,  2.00it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:27,  2.00it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:27,  2.00it/s]

    Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:27,  2.00it/s]Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.09it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.09it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.09it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.09it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.09it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.09it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.09it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.09it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.09it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 12.79it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 12.79it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:03, 12.79it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:03, 12.79it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:03, 12.79it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:03, 12.79it/s]

    Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:03, 12.79it/s]Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:03, 12.79it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 19.11it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 19.11it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 19.11it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 19.11it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 19.11it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 19.11it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 19.11it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 19.11it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 25.94it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 25.94it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 25.94it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 25.94it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 25.94it/s]

    Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 25.94it/s]Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 25.94it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 30.48it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 30.48it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 30.48it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 30.48it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 30.48it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 30.48it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 30.48it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 35.83it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 35.83it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 35.83it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 35.83it/s] 

    Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 35.83it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 35.83it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 35.83it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 39.01it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 39.01it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 39.01it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 39.01it/s]Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 39.01it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 39.01it/s]Compiling num tokens (num_tokens=12):  86%|████████▌ | 50/58 [00:03<00:00, 39.01it/s]Compiling num tokens (num_tokens=8):  86%|████████▌ | 50/58 [00:03<00:00, 39.01it/s] Compiling num tokens (num_tokens=4):  86%|████████▌ | 50/58 [00:03<00:00, 39.01it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.63it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.99 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.96 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.96 GB):   3%|▎         | 2/58 [00:00<00:04, 13.51it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.96 GB):   3%|▎         | 2/58 [00:00<00:04, 13.51it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.95 GB):   3%|▎         | 2/58 [00:00<00:04, 13.51it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=118.95 GB):   7%|▋         | 4/58 [00:00<00:08,  6.45it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.96 GB):   7%|▋         | 4/58 [00:00<00:08,  6.45it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.95 GB):   7%|▋         | 4/58 [00:00<00:08,  6.45it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.95 GB):   7%|▋         | 4/58 [00:00<00:08,  6.45it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.95 GB):  12%|█▏        | 7/58 [00:00<00:04, 11.12it/s]Capturing num tokens (num_tokens=4608 avail_mem=118.95 GB):  12%|█▏        | 7/58 [00:00<00:04, 11.12it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.95 GB):  12%|█▏        | 7/58 [00:00<00:04, 11.12it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.94 GB):  12%|█▏        | 7/58 [00:00<00:04, 11.12it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.94 GB):  12%|█▏        | 7/58 [00:00<00:04, 11.12it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=118.94 GB):  19%|█▉        | 11/58 [00:00<00:02, 17.32it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.94 GB):  19%|█▉        | 11/58 [00:00<00:02, 17.32it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.93 GB):  19%|█▉        | 11/58 [00:00<00:02, 17.32it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.93 GB):  19%|█▉        | 11/58 [00:00<00:02, 17.32it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.93 GB):  19%|█▉        | 11/58 [00:00<00:02, 17.32it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.92 GB):  19%|█▉        | 11/58 [00:00<00:02, 17.32it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.92 GB):  28%|██▊       | 16/58 [00:00<00:01, 24.26it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.92 GB):  28%|██▊       | 16/58 [00:00<00:01, 24.26it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.92 GB):  28%|██▊       | 16/58 [00:00<00:01, 24.26it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.91 GB):  28%|██▊       | 16/58 [00:00<00:01, 24.26it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.91 GB):  28%|██▊       | 16/58 [00:00<00:01, 24.26it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=118.89 GB):  28%|██▊       | 16/58 [00:01<00:01, 24.26it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.89 GB):  36%|███▌      | 21/58 [00:01<00:01, 29.73it/s]Capturing num tokens (num_tokens=960 avail_mem=118.90 GB):  36%|███▌      | 21/58 [00:01<00:01, 29.73it/s] Capturing num tokens (num_tokens=896 avail_mem=118.90 GB):  36%|███▌      | 21/58 [00:01<00:01, 29.73it/s]Capturing num tokens (num_tokens=832 avail_mem=118.90 GB):  36%|███▌      | 21/58 [00:01<00:01, 29.73it/s]Capturing num tokens (num_tokens=768 avail_mem=118.89 GB):  36%|███▌      | 21/58 [00:01<00:01, 29.73it/s]Capturing num tokens (num_tokens=704 avail_mem=118.89 GB):  36%|███▌      | 21/58 [00:01<00:01, 29.73it/s]Capturing num tokens (num_tokens=704 avail_mem=118.89 GB):  45%|████▍     | 26/58 [00:01<00:00, 33.98it/s]Capturing num tokens (num_tokens=640 avail_mem=118.89 GB):  45%|████▍     | 26/58 [00:01<00:00, 33.98it/s]Capturing num tokens (num_tokens=576 avail_mem=118.89 GB):  45%|████▍     | 26/58 [00:01<00:00, 33.98it/s]Capturing num tokens (num_tokens=512 avail_mem=118.88 GB):  45%|████▍     | 26/58 [00:01<00:00, 33.98it/s]Capturing num tokens (num_tokens=480 avail_mem=118.89 GB):  45%|████▍     | 26/58 [00:01<00:00, 33.98it/s]

    Capturing num tokens (num_tokens=448 avail_mem=118.89 GB):  45%|████▍     | 26/58 [00:01<00:00, 33.98it/s]Capturing num tokens (num_tokens=448 avail_mem=118.89 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.14it/s]Capturing num tokens (num_tokens=416 avail_mem=118.89 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.14it/s]Capturing num tokens (num_tokens=384 avail_mem=118.89 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.14it/s]Capturing num tokens (num_tokens=352 avail_mem=118.88 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.14it/s]Capturing num tokens (num_tokens=320 avail_mem=118.88 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.14it/s]Capturing num tokens (num_tokens=288 avail_mem=118.87 GB):  53%|█████▎    | 31/58 [00:01<00:00, 37.14it/s]Capturing num tokens (num_tokens=288 avail_mem=118.87 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.35it/s]Capturing num tokens (num_tokens=256 avail_mem=118.87 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.35it/s]Capturing num tokens (num_tokens=240 avail_mem=118.87 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.35it/s]Capturing num tokens (num_tokens=224 avail_mem=118.86 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.35it/s]

    Capturing num tokens (num_tokens=208 avail_mem=118.86 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.35it/s]Capturing num tokens (num_tokens=192 avail_mem=118.86 GB):  62%|██████▏   | 36/58 [00:01<00:00, 39.35it/s]Capturing num tokens (num_tokens=192 avail_mem=118.86 GB):  71%|███████   | 41/58 [00:01<00:00, 40.70it/s]Capturing num tokens (num_tokens=176 avail_mem=118.86 GB):  71%|███████   | 41/58 [00:01<00:00, 40.70it/s]Capturing num tokens (num_tokens=160 avail_mem=118.85 GB):  71%|███████   | 41/58 [00:01<00:00, 40.70it/s]Capturing num tokens (num_tokens=144 avail_mem=118.85 GB):  71%|███████   | 41/58 [00:01<00:00, 40.70it/s]Capturing num tokens (num_tokens=128 avail_mem=118.85 GB):  71%|███████   | 41/58 [00:01<00:00, 40.70it/s]Capturing num tokens (num_tokens=112 avail_mem=118.84 GB):  71%|███████   | 41/58 [00:01<00:00, 40.70it/s]Capturing num tokens (num_tokens=112 avail_mem=118.84 GB):  79%|███████▉  | 46/58 [00:01<00:00, 41.97it/s]Capturing num tokens (num_tokens=96 avail_mem=118.84 GB):  79%|███████▉  | 46/58 [00:01<00:00, 41.97it/s] Capturing num tokens (num_tokens=80 avail_mem=118.84 GB):  79%|███████▉  | 46/58 [00:01<00:00, 41.97it/s]

    Capturing num tokens (num_tokens=64 avail_mem=118.83 GB):  79%|███████▉  | 46/58 [00:01<00:00, 41.97it/s]Capturing num tokens (num_tokens=48 avail_mem=118.83 GB):  79%|███████▉  | 46/58 [00:01<00:00, 41.97it/s]Capturing num tokens (num_tokens=32 avail_mem=118.82 GB):  79%|███████▉  | 46/58 [00:01<00:00, 41.97it/s]Capturing num tokens (num_tokens=32 avail_mem=118.82 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.65it/s]Capturing num tokens (num_tokens=28 avail_mem=118.82 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.65it/s]Capturing num tokens (num_tokens=24 avail_mem=118.82 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.65it/s]Capturing num tokens (num_tokens=20 avail_mem=118.81 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.65it/s]Capturing num tokens (num_tokens=16 avail_mem=118.81 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.65it/s]Capturing num tokens (num_tokens=12 avail_mem=118.81 GB):  88%|████████▊ | 51/58 [00:01<00:00, 42.65it/s]Capturing num tokens (num_tokens=12 avail_mem=118.81 GB):  97%|█████████▋| 56/58 [00:01<00:00, 43.69it/s]Capturing num tokens (num_tokens=8 avail_mem=118.81 GB):  97%|█████████▋| 56/58 [00:01<00:00, 43.69it/s] Capturing num tokens (num_tokens=4 avail_mem=118.80 GB):  97%|█████████▋| 56/58 [00:01<00:00, 43.69it/s]

    Capturing num tokens (num_tokens=4 avail_mem=118.80 GB): 100%|██████████| 58/58 [00:01<00:00, 31.38it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:116: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


    Server started on http://localhost:32826


## Chat Completions

### Usage

The server fully implements the OpenAI API.
It will automatically apply the chat template specified in the Hugging Face tokenizer, if one is available.
You can also specify a custom chat template with `--chat-template` when launching the server.


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

print_highlight(f"Response: {response}")
```


<strong style='color: #00008B;'>Response: ChatCompletion(id='8f530538465a4b0d96967c5aa2576e54', choices=[Choice(finish_reason='stop', index=0, logprobs=None, message=ChatCompletionMessage(content='Sure, here are three countries and their respective capitals:\n\n1. **United States** - Washington, D.C.\n2. **Canada** - Ottawa\n3. **Australia** - Canberra', refusal=None, role='assistant', annotations=None, audio=None, function_call=None, tool_calls=None, reasoning_content=None), matched_stop=151645)], created=1774203634, model='qwen/qwen2.5-0.5b-instruct', object='chat.completion', service_tier=None, system_fingerprint=None, usage=CompletionUsage(completion_tokens=39, prompt_tokens=37, total_tokens=76, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>


### Model Thinking/Reasoning Support

Some models support internal reasoning or thinking processes that can be exposed in the API response. SGLang provides unified support for various reasoning models through the `chat_template_kwargs` parameter and compatible reasoning parsers.

#### Supported Models and Configuration

| Model Family | Chat Template Parameter | Reasoning Parser | Notes |
|--------------|------------------------|------------------|--------|
| DeepSeek-R1 (R1, R1-0528, R1-Distill) | `enable_thinking` | `--reasoning-parser deepseek-r1` | Standard reasoning models |
| DeepSeek-V3.1 | `thinking` | `--reasoning-parser deepseek-v3` | Hybrid model (thinking/non-thinking modes) |
| Qwen3 (standard) | `enable_thinking` | `--reasoning-parser qwen3` | Hybrid model (thinking/non-thinking modes) |
| Qwen3-Thinking | N/A (always enabled) | `--reasoning-parser qwen3-thinking` | Always generates reasoning |
| Kimi | N/A (always enabled) | `--reasoning-parser kimi` | Kimi thinking models |
| Gpt-Oss | N/A (always enabled) | `--reasoning-parser gpt-oss` | Gpt-Oss thinking models |

#### Basic Usage

To enable reasoning output, you need to:
1. Launch the server with the appropriate reasoning parser
2. Set the model-specific parameter in `chat_template_kwargs`
3. Optionally use `separate_reasoning: False` to not get reasoning content separately (default to `True`)

**Note for Qwen3-Thinking models:** These models always generate thinking content and do not support the `enable_thinking` parameter. Use `--reasoning-parser qwen3-thinking` or `--reasoning-parser qwen3` to parse the thinking content.


#### Example: Qwen3 Models

```python
# Launch server:
# python3 -m sglang.launch_server --model Qwen/Qwen3-4B --reasoning-parser qwen3

from openai import OpenAI

client = OpenAI(
    api_key="EMPTY",
    base_url=f"http://127.0.0.1:30000/v1",
)

model = "Qwen/Qwen3-4B"
messages = [{"role": "user", "content": "How many r's are in 'strawberry'?"}]

response = client.chat.completions.create(
    model=model,
    messages=messages,
    extra_body={
        "chat_template_kwargs": {"enable_thinking": True},
        "separate_reasoning": True
    }
)

print("Reasoning:", response.choices[0].message.reasoning_content)
print("-"*100)
print("Answer:", response.choices[0].message.content)
```

**ExampleOutput:**
```
Reasoning: Okay, so the user is asking how many 'r's are in the word 'strawberry'. Let me think. First, I need to make sure I have the word spelled correctly. Strawberry... S-T-R-A-W-B-E-R-R-Y. Wait, is that right? Let me break it down.

Starting with 'strawberry', let's write out the letters one by one. S, T, R, A, W, B, E, R, R, Y. Hmm, wait, that's 10 letters. Let me check again. S (1), T (2), R (3), A (4), W (5), B (6), E (7), R (8), R (9), Y (10). So the letters are S-T-R-A-W-B-E-R-R-Y. 
...
Therefore, the answer should be three R's in 'strawberry'. But I need to make sure I'm not counting any other letters as R. Let me check again. S, T, R, A, W, B, E, R, R, Y. No other R's. So three in total. Yeah, that seems right.

----------------------------------------------------------------------------------------------------
Answer: The word "strawberry" contains **three** letters 'r'. Here's the breakdown:

1. **S-T-R-A-W-B-E-R-R-Y**  
   - The **third letter** is 'R'.  
   - The **eighth and ninth letters** are also 'R's.  

Thus, the total count is **3**.  

**Answer:** 3.
```

**Note:** Setting `"enable_thinking": False` (or omitting it) will result in `reasoning_content` being `None`. Qwen3-Thinking models always generate reasoning content and don't support the `enable_thinking` parameter.


#### Logit Bias Support

SGLang supports the `logit_bias` parameter for both chat completions and completions APIs. This parameter allows you to modify the likelihood of specific tokens being generated by adding bias values to their logits. The bias values can range from -100 to 100, where:

- **Positive values** (0 to 100) increase the likelihood of the token being selected
- **Negative values** (-100 to 0) decrease the likelihood of the token being selected
- **-100** effectively prevents the token from being generated

The `logit_bias` parameter accepts a dictionary where keys are token IDs (as strings) and values are the bias amounts (as floats).


#### Getting Token IDs

To use `logit_bias` effectively, you need to know the token IDs for the words you want to bias. Here's how to get token IDs:

```python
# Get tokenizer to find token IDs
import tiktoken

# For OpenAI models, use the appropriate encoding
tokenizer = tiktoken.encoding_for_model("gpt-3.5-turbo")  # or your model

# Get token IDs for specific words
word = "sunny"
token_ids = tokenizer.encode(word)
print(f"Token IDs for '{word}': {token_ids}")

# For SGLang models, you can access the tokenizer through the client
# and get token IDs for bias
```

**Important:** The `logit_bias` parameter uses token IDs as string keys, not the actual words.


#### Example: DeepSeek-V3 Models

DeepSeek-V3 models support thinking mode through the `thinking` parameter:

```python
# Launch server:
# python3 -m sglang.launch_server --model deepseek-ai/DeepSeek-V3.1 --tp 8  --reasoning-parser deepseek-v3

from openai import OpenAI

client = OpenAI(
    api_key="EMPTY",
    base_url=f"http://127.0.0.1:30000/v1",
)

model = "deepseek-ai/DeepSeek-V3.1"
messages = [{"role": "user", "content": "How many r's are in 'strawberry'?"}]

response = client.chat.completions.create(
    model=model,
    messages=messages,
    extra_body={
        "chat_template_kwargs": {"thinking": True},
        "separate_reasoning": True
    }
)

print("Reasoning:", response.choices[0].message.reasoning_content)
print("-"*100)
print("Answer:", response.choices[0].message.content)
```

**Example Output:**
```
Reasoning: First, the question is: "How many r's are in 'strawberry'?"

I need to count the number of times the letter 'r' appears in the word "strawberry".

Let me write out the word: S-T-R-A-W-B-E-R-R-Y.

Now, I'll go through each letter and count the 'r's.
...
So, I have three 'r's in "strawberry".

I should double-check. The word is spelled S-T-R-A-W-B-E-R-R-Y. The letters are at positions: 3, 8, and 9 are 'r's. Yes, that's correct.

Therefore, the answer should be 3.
----------------------------------------------------------------------------------------------------
Answer: The word "strawberry" contains **3** instances of the letter "r". Here's a breakdown for clarity:

- The word is spelled: S-T-R-A-W-B-E-R-R-Y
- The "r" appears at the 3rd, 8th, and 9th positions.
```

**Note:** DeepSeek-V3 models use the `thinking` parameter (not `enable_thinking`) to control reasoning output.



```python
# Example with logit_bias parameter
# Note: You need to get the actual token IDs from your tokenizer
# For demonstration, we'll use some example token IDs
response = client.chat.completions.create(
    model="qwen/qwen2.5-0.5b-instruct",
    messages=[
        {"role": "user", "content": "Complete this sentence: The weather today is"}
    ],
    temperature=0.7,
    max_tokens=20,
    logit_bias={
        "12345": 50,  # Increase likelihood of token ID 12345
        "67890": -50,  # Decrease likelihood of token ID 67890
        "11111": 25,  # Slightly increase likelihood of token ID 11111
    },
)

print_highlight(f"Response with logit bias: {response.choices[0].message.content}")
```


<strong style='color: #00008B;'>Response with logit bias:  privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy</strong>


### Parameters

The chat completions API accepts OpenAI Chat Completions API's parameters. Refer to [OpenAI Chat Completions API](https://platform.openai.com/docs/api-reference/chat/create) for more details.

SGLang extends the standard API with the `extra_body` parameter, allowing for additional customization. One key option within `extra_body` is `chat_template_kwargs`, which can be used to pass arguments to the chat template processor.


```python
response = client.chat.completions.create(
    model="qwen/qwen2.5-0.5b-instruct",
    messages=[
        {
            "role": "system",
            "content": "You are a knowledgeable historian who provides concise responses.",
        },
        {"role": "user", "content": "Tell me about ancient Rome"},
        {
            "role": "assistant",
            "content": "Ancient Rome was a civilization centered in Italy.",
        },
        {"role": "user", "content": "What were their major achievements?"},
    ],
    temperature=0.3,  # Lower temperature for more focused responses
    max_tokens=128,  # Reasonable length for a concise response
    top_p=0.95,  # Slightly higher for better fluency
    presence_penalty=0.2,  # Mild penalty to avoid repetition
    frequency_penalty=0.2,  # Mild penalty for more natural language
    n=1,  # Single response is usually more stable
    seed=42,  # Keep for reproducibility
)

print_highlight(response.choices[0].message.content)
```


<strong style='color: #00008B;'>Ancient Rome was a major civilization that thrived from the 8th to the 4th century BCE. It was known for its advanced engineering, art, literature, and philosophy. Some of their major achievements include:<br><br>1. **Architecture**: The Romans built magnificent temples, aqueducts, and public buildings that showcased their engineering prowess.<br><br>2. **Art**: They were renowned for their art, particularly in sculpture, architecture, and painting. Famous works include the Colosseum and the Pantheon.<br><br>3. **Literature**: Ancient Rome produced some of the world's earliest surviving literature, including the works of Virgil (</strong>


Streaming mode is also supported.

#### Logit Bias Support

The completions API also supports the `logit_bias` parameter with the same functionality as described in the chat completions section above.



```python
stream = client.chat.completions.create(
    model="qwen/qwen2.5-0.5b-instruct",
    messages=[{"role": "user", "content": "Say this is a test"}],
    stream=True,
)
for chunk in stream:
    if chunk.choices[0].delta.content is not None:
        print(chunk.choices[0].delta.content, end="")
```

    Yes, that is a test. I am Qwen, a large language model created by Alibaba Cloud. I am designed to assist with

     a wide range of tasks, including answering questions, providing information, and even participating in creative writing and language exercises. If you have any

     questions or need help with anything, feel free to ask, and I'll do my best to assist you.

#### Returning Routed Experts (MoE Models)

For MoE models, set `return_routed_experts: true` in `extra_body` to return expert routing data. Requires `--enable-return-routed-experts` server flag. The `routed_experts` field will be returned in the `sgl_ext` object on each choice, containing base64-encoded int32 expert IDs as a flattened array with logical shape `[num_tokens, num_layers, top_k]`.


```python
# Example with logit_bias parameter for completions API
# Note: You need to get the actual token IDs from your tokenizer
# For demonstration, we'll use some example token IDs
response = client.completions.create(
    model="qwen/qwen2.5-0.5b-instruct",
    prompt="The best programming language for AI is",
    temperature=0.7,
    max_tokens=20,
    logit_bias={
        "12345": 75,  # Strongly favor token ID 12345
        "67890": -100,  # Completely avoid token ID 67890
        "11111": -25,  # Slightly discourage token ID 11111
    },
)

print_highlight(f"Response with logit bias: {response.choices[0].text}")
```


<strong style='color: #00008B;'>Response with logit bias:  privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy privacy</strong>


## Completions

### Usage
Completions API is similar to Chat Completions API, but without the `messages` parameter or chat templates.


```python
response = client.completions.create(
    model="qwen/qwen2.5-0.5b-instruct",
    prompt="List 3 countries and their capitals.",
    temperature=0,
    max_tokens=64,
    n=1,
    stop=None,
)

print_highlight(f"Response: {response}")
```


<strong style='color: #00008B;'>Response: Completion(id='f5402659a57e421784f88389f8b022bc', choices=[CompletionChoice(finish_reason='length', index=0, logprobs=None, text=' 1. United States - Washington D.C.\n2. Canada - Ottawa\n3. France - Paris\n4. Germany - Berlin\n5. Japan - Tokyo\n6. Italy - Rome\n7. Spain - Madrid\n8. United Kingdom - London\n9. Australia - Canberra\n10. New Zealand', matched_stop=None)], created=1774203636, model='qwen/qwen2.5-0.5b-instruct', object='text_completion', system_fingerprint=None, usage=CompletionUsage(completion_tokens=64, prompt_tokens=8, total_tokens=72, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>


### Parameters

The completions API accepts OpenAI Completions API's parameters.  Refer to [OpenAI Completions API](https://platform.openai.com/docs/api-reference/completions/create) for more details.

Here is an example of a detailed completions request:


```python
response = client.completions.create(
    model="qwen/qwen2.5-0.5b-instruct",
    prompt="Write a short story about a space explorer.",
    temperature=0.7,  # Moderate temperature for creative writing
    max_tokens=150,  # Longer response for a story
    top_p=0.9,  # Balanced diversity in word choice
    stop=["\n\n", "THE END"],  # Multiple stop sequences
    presence_penalty=0.3,  # Encourage novel elements
    frequency_penalty=0.3,  # Reduce repetitive phrases
    n=1,  # Generate one completion
    seed=123,  # For reproducible results
)

print_highlight(f"Response: {response}")
```


<strong style='color: #00008B;'>Response: Completion(id='90a66eca46bc4cee9e3d4489747cda39', choices=[CompletionChoice(finish_reason='stop', index=0, logprobs=None, text=' Once upon a time, there was a space explorer named Dr. Amelia Hart. She had been fascinated by the mysteries of the universe since she was a child, and her passion only grew stronger with each passing day. One day, while exploring a new planet, Dr. Hart discovered an ancient civilization that had left behind a strange artifact.', matched_stop='\n\n')], created=1774203637, model='qwen/qwen2.5-0.5b-instruct', object='text_completion', system_fingerprint=None, usage=CompletionUsage(completion_tokens=67, prompt_tokens=9, total_tokens=76, completion_tokens_details=None, prompt_tokens_details=None, reasoning_tokens=0), metadata={'weight_version': 'default'})</strong>


#### Returning Routed Experts (MoE Models)

For MoE models, set `return_routed_experts: true` in `extra_body` to return expert routing data. Requires `--enable-return-routed-experts` server flag. The `routed_experts` field will be returned in the `sgl_ext` object on each choice, containing base64-encoded int32 expert IDs as a flattened array with logical shape `[num_tokens, num_layers, top_k]`.

## Structured Outputs (JSON, Regex, EBNF)

For OpenAI compatible structured outputs API, refer to [Structured Outputs](../advanced_features/structured_outputs.ipynb) for more details.


## Using LoRA Adapters

SGLang supports LoRA (Low-Rank Adaptation) adapters with OpenAI-compatible APIs. You can specify which adapter to use directly in the `model` parameter using the `base-model:adapter-name` syntax.

**Server Setup:**
```bash
python -m sglang.launch_server \
    --model-path qwen/qwen2.5-0.5b-instruct \
    --enable-lora \
    --lora-paths adapter_a=/path/to/adapter_a adapter_b=/path/to/adapter_b
```

For more details on LoRA serving configuration, see the [LoRA documentation](../advanced_features/lora.ipynb).

**API Call:**

(Recommended) Use the `model:adapter` syntax to specify which adapter to use:
```python
response = client.chat.completions.create(
    model="qwen/qwen2.5-0.5b-instruct:adapter_a",  # ← base-model:adapter-name
    messages=[{"role": "user", "content": "Convert to SQL: show all users"}],
    max_tokens=50,
)
```

**Backward Compatible: Using `extra_body`**

The old `extra_body` method is still supported for backward compatibility:
```python
# Backward compatible method
response = client.chat.completions.create(
    model="qwen/qwen2.5-0.5b-instruct",
    messages=[{"role": "user", "content": "Convert to SQL: show all users"}],
    extra_body={"lora_path": "adapter_a"},  # ← old method
    max_tokens=50,
)
```
**Note:** When both `model:adapter` and `extra_body["lora_path"]` are specified, the `model:adapter` syntax takes precedence.


```python
terminate_process(server_process)
```
