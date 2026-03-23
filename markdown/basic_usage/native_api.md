# SGLang Native APIs

Apart from the OpenAI compatible APIs, the SGLang Runtime also provides its native server APIs. We introduce the following APIs:

- `/generate` (text generation model)
- `/get_model_info`
- `/get_server_info`
- `/health`
- `/health_generate`
- `/flush_cache`
- `/update_weights`
- `/encode`(embedding model)
- `/v1/rerank`(cross encoder rerank model)
- `/v1/score`(decoder-only scoring)
- `/classify`(reward model)
- `/start_expert_distribution_record`
- `/stop_expert_distribution_record`
- `/dump_expert_distribution_record`
- `/tokenize`
- `/detokenize`
- A full list of these APIs can be found at [http_server.py](https://github.com/sgl-project/sglang/blob/main/python/sglang/srt/entrypoints/http_server.py)

We mainly use `requests` to test these APIs in the following examples. You can also use `curl`.


## Launch A Server


```python
from sglang.test.doc_patch import launch_server_cmd
from sglang.utils import wait_for_server, print_highlight, terminate_process

server_process, port = launch_server_cmd(
    "python3 -m sglang.launch_server --model-path qwen/qwen2.5-0.5b-instruct --host 0.0.0.0 --log-level warning"
)

wait_for_server(f"http://localhost:{port}", process=server_process)
```

    [2026-03-23 00:10:10] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-23 00:10:10] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-23 00:10:10] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-23 00:10:15] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:10:15] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:10:15] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    [2026-03-23 00:10:17] WARNING model_config.py:1098: Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.
    [2026-03-23 00:10:17] INFO server_args.py:2239: Attention backend not specified. Use fa3 backend by default.
    [2026-03-23 00:10:17] INFO server_args.py:3522: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-23 00:10:18] Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-23 00:10:22] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:10:22] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:10:22] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-23 00:10:23] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:10:23] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:10:23] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-23 00:10:25] Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-23 00:10:26] Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  8.03it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  8.02it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:20,  2.47s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:20,  2.47s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:20,  2.47s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:20,  2.47s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:26,  2.02it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:26,  2.02it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:26,  2.02it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:26,  2.02it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:26,  2.02it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:26,  2.02it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:26,  2.02it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.14it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:03, 12.85it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:03, 12.85it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:03, 12.85it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:03, 12.85it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:03, 12.85it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:03, 12.85it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:03, 12.85it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:03, 12.85it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 19.13it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 19.13it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 19.13it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 19.13it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 19.13it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 19.13it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 19.13it/s]Compiling num tokens (num_tokens=448):  53%|█████▎    | 31/58 [00:03<00:01, 24.55it/s]Compiling num tokens (num_tokens=416):  53%|█████▎    | 31/58 [00:03<00:01, 24.55it/s]Compiling num tokens (num_tokens=384):  53%|█████▎    | 31/58 [00:03<00:01, 24.55it/s]Compiling num tokens (num_tokens=352):  53%|█████▎    | 31/58 [00:03<00:01, 24.55it/s]Compiling num tokens (num_tokens=320):  53%|█████▎    | 31/58 [00:03<00:01, 24.55it/s]Compiling num tokens (num_tokens=288):  53%|█████▎    | 31/58 [00:03<00:01, 24.55it/s]

    Compiling num tokens (num_tokens=256):  53%|█████▎    | 31/58 [00:03<00:01, 24.55it/s]Compiling num tokens (num_tokens=256):  64%|██████▍   | 37/58 [00:03<00:00, 30.06it/s]Compiling num tokens (num_tokens=240):  64%|██████▍   | 37/58 [00:03<00:00, 30.06it/s]Compiling num tokens (num_tokens=224):  64%|██████▍   | 37/58 [00:03<00:00, 30.06it/s]Compiling num tokens (num_tokens=208):  64%|██████▍   | 37/58 [00:03<00:00, 30.06it/s]Compiling num tokens (num_tokens=192):  64%|██████▍   | 37/58 [00:03<00:00, 30.06it/s]Compiling num tokens (num_tokens=176):  64%|██████▍   | 37/58 [00:03<00:00, 30.06it/s]Compiling num tokens (num_tokens=160):  64%|██████▍   | 37/58 [00:03<00:00, 30.06it/s]Compiling num tokens (num_tokens=160):  74%|███████▍  | 43/58 [00:03<00:00, 35.38it/s]Compiling num tokens (num_tokens=144):  74%|███████▍  | 43/58 [00:03<00:00, 35.38it/s]Compiling num tokens (num_tokens=128):  74%|███████▍  | 43/58 [00:03<00:00, 35.38it/s]Compiling num tokens (num_tokens=112):  74%|███████▍  | 43/58 [00:03<00:00, 35.38it/s]Compiling num tokens (num_tokens=96):  74%|███████▍  | 43/58 [00:03<00:00, 35.38it/s] 

    Compiling num tokens (num_tokens=80):  74%|███████▍  | 43/58 [00:03<00:00, 35.38it/s]Compiling num tokens (num_tokens=64):  74%|███████▍  | 43/58 [00:03<00:00, 35.38it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:03<00:00, 38.77it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:03<00:00, 38.77it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:03<00:00, 38.77it/s]Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:03<00:00, 38.77it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:03<00:00, 38.77it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:03<00:00, 38.77it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:03<00:00, 38.77it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:03<00:00, 38.77it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:03<00:00, 38.77it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:03<00:00, 47.35it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:03<00:00, 47.35it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.70it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=132.42 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=132.39 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=132.39 GB):   3%|▎         | 2/58 [00:00<00:03, 17.34it/s]Capturing num tokens (num_tokens=7168 avail_mem=132.38 GB):   3%|▎         | 2/58 [00:00<00:03, 17.34it/s]Capturing num tokens (num_tokens=6656 avail_mem=132.38 GB):   3%|▎         | 2/58 [00:00<00:03, 17.34it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=132.38 GB):   3%|▎         | 2/58 [00:00<00:03, 17.34it/s]Capturing num tokens (num_tokens=6144 avail_mem=132.38 GB):   9%|▊         | 5/58 [00:00<00:02, 20.89it/s]Capturing num tokens (num_tokens=5632 avail_mem=132.38 GB):   9%|▊         | 5/58 [00:00<00:02, 20.89it/s]Capturing num tokens (num_tokens=5120 avail_mem=132.38 GB):   9%|▊         | 5/58 [00:00<00:02, 20.89it/s]Capturing num tokens (num_tokens=4608 avail_mem=132.37 GB):   9%|▊         | 5/58 [00:00<00:02, 20.89it/s]Capturing num tokens (num_tokens=4608 avail_mem=132.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.51it/s]Capturing num tokens (num_tokens=4096 avail_mem=132.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.51it/s]Capturing num tokens (num_tokens=3840 avail_mem=132.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.51it/s]Capturing num tokens (num_tokens=3584 avail_mem=132.37 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.51it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=132.36 GB):  14%|█▍        | 8/58 [00:00<00:02, 24.51it/s]Capturing num tokens (num_tokens=3328 avail_mem=132.36 GB):  21%|██        | 12/58 [00:00<00:01, 29.40it/s]Capturing num tokens (num_tokens=3072 avail_mem=132.36 GB):  21%|██        | 12/58 [00:00<00:01, 29.40it/s]Capturing num tokens (num_tokens=2816 avail_mem=132.36 GB):  21%|██        | 12/58 [00:00<00:01, 29.40it/s]Capturing num tokens (num_tokens=2560 avail_mem=132.35 GB):  21%|██        | 12/58 [00:00<00:01, 29.40it/s]Capturing num tokens (num_tokens=2304 avail_mem=132.35 GB):  21%|██        | 12/58 [00:00<00:01, 29.40it/s]Capturing num tokens (num_tokens=2048 avail_mem=132.35 GB):  21%|██        | 12/58 [00:00<00:01, 29.40it/s]Capturing num tokens (num_tokens=2048 avail_mem=132.35 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.46it/s]Capturing num tokens (num_tokens=1792 avail_mem=132.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.46it/s]Capturing num tokens (num_tokens=1536 avail_mem=132.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.46it/s]Capturing num tokens (num_tokens=1280 avail_mem=132.34 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.46it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=132.32 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.46it/s]Capturing num tokens (num_tokens=960 avail_mem=132.33 GB):  29%|██▉       | 17/58 [00:00<00:01, 34.46it/s] Capturing num tokens (num_tokens=960 avail_mem=132.33 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.64it/s]Capturing num tokens (num_tokens=896 avail_mem=132.33 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.64it/s]Capturing num tokens (num_tokens=832 avail_mem=132.32 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.64it/s]Capturing num tokens (num_tokens=768 avail_mem=132.32 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.64it/s]Capturing num tokens (num_tokens=704 avail_mem=132.32 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.64it/s]Capturing num tokens (num_tokens=640 avail_mem=132.31 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.64it/s]Capturing num tokens (num_tokens=640 avail_mem=132.31 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.28it/s]Capturing num tokens (num_tokens=576 avail_mem=132.31 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.28it/s]Capturing num tokens (num_tokens=512 avail_mem=132.30 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.28it/s]Capturing num tokens (num_tokens=480 avail_mem=132.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.28it/s]

    Capturing num tokens (num_tokens=448 avail_mem=132.32 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.28it/s]Capturing num tokens (num_tokens=416 avail_mem=132.31 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.28it/s]Capturing num tokens (num_tokens=416 avail_mem=132.31 GB):  55%|█████▌    | 32/58 [00:00<00:00, 42.04it/s]Capturing num tokens (num_tokens=384 avail_mem=132.31 GB):  55%|█████▌    | 32/58 [00:00<00:00, 42.04it/s]Capturing num tokens (num_tokens=352 avail_mem=132.31 GB):  55%|█████▌    | 32/58 [00:00<00:00, 42.04it/s]Capturing num tokens (num_tokens=320 avail_mem=132.30 GB):  55%|█████▌    | 32/58 [00:00<00:00, 42.04it/s]Capturing num tokens (num_tokens=288 avail_mem=132.30 GB):  55%|█████▌    | 32/58 [00:00<00:00, 42.04it/s]Capturing num tokens (num_tokens=256 avail_mem=132.30 GB):  55%|█████▌    | 32/58 [00:00<00:00, 42.04it/s]Capturing num tokens (num_tokens=256 avail_mem=132.30 GB):  64%|██████▍   | 37/58 [00:01<00:00, 43.12it/s]Capturing num tokens (num_tokens=240 avail_mem=132.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 43.12it/s]Capturing num tokens (num_tokens=224 avail_mem=132.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 43.12it/s]Capturing num tokens (num_tokens=208 avail_mem=132.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 43.12it/s]

    Capturing num tokens (num_tokens=192 avail_mem=132.29 GB):  64%|██████▍   | 37/58 [00:01<00:00, 43.12it/s]Capturing num tokens (num_tokens=176 avail_mem=132.28 GB):  64%|██████▍   | 37/58 [00:01<00:00, 43.12it/s]Capturing num tokens (num_tokens=176 avail_mem=132.28 GB):  72%|███████▏  | 42/58 [00:01<00:00, 44.06it/s]Capturing num tokens (num_tokens=160 avail_mem=132.28 GB):  72%|███████▏  | 42/58 [00:01<00:00, 44.06it/s]Capturing num tokens (num_tokens=144 avail_mem=132.28 GB):  72%|███████▏  | 42/58 [00:01<00:00, 44.06it/s]Capturing num tokens (num_tokens=128 avail_mem=132.27 GB):  72%|███████▏  | 42/58 [00:01<00:00, 44.06it/s]Capturing num tokens (num_tokens=112 avail_mem=132.27 GB):  72%|███████▏  | 42/58 [00:01<00:00, 44.06it/s]Capturing num tokens (num_tokens=96 avail_mem=132.27 GB):  72%|███████▏  | 42/58 [00:01<00:00, 44.06it/s] Capturing num tokens (num_tokens=96 avail_mem=132.27 GB):  81%|████████  | 47/58 [00:01<00:00, 44.42it/s]Capturing num tokens (num_tokens=80 avail_mem=132.26 GB):  81%|████████  | 47/58 [00:01<00:00, 44.42it/s]Capturing num tokens (num_tokens=64 avail_mem=132.26 GB):  81%|████████  | 47/58 [00:01<00:00, 44.42it/s]Capturing num tokens (num_tokens=48 avail_mem=132.26 GB):  81%|████████  | 47/58 [00:01<00:00, 44.42it/s]

    Capturing num tokens (num_tokens=32 avail_mem=132.25 GB):  81%|████████  | 47/58 [00:01<00:00, 44.42it/s]Capturing num tokens (num_tokens=28 avail_mem=132.25 GB):  81%|████████  | 47/58 [00:01<00:00, 44.42it/s]Capturing num tokens (num_tokens=28 avail_mem=132.25 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.81it/s]Capturing num tokens (num_tokens=24 avail_mem=132.25 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.81it/s]Capturing num tokens (num_tokens=20 avail_mem=132.24 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.81it/s]Capturing num tokens (num_tokens=16 avail_mem=132.24 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.81it/s]Capturing num tokens (num_tokens=12 avail_mem=132.24 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.81it/s]Capturing num tokens (num_tokens=8 avail_mem=132.23 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.81it/s] Capturing num tokens (num_tokens=8 avail_mem=132.23 GB):  98%|█████████▊| 57/58 [00:01<00:00, 45.51it/s]Capturing num tokens (num_tokens=4 avail_mem=132.23 GB):  98%|█████████▊| 57/58 [00:01<00:00, 45.51it/s]Capturing num tokens (num_tokens=4 avail_mem=132.23 GB): 100%|██████████| 58/58 [00:01<00:00, 39.54it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:116: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


## Generate (text generation model)
Generate completions. This is similar to the `/v1/completions` in OpenAI API. Detailed parameters can be found in the [sampling parameters](sampling_params.md).


```python
import requests

url = f"http://localhost:{port}/generate"
data = {"text": "What is the capital of France?"}

response = requests.post(url, json=data)
print_highlight(response.json())
```


<strong style='color: #00008B;'>{'text': ' The capital of France is Paris. Is there anything else you would like to know about Paris? Please let me know how else you\'d like to proceed. The climate in Paris is generally mild and summer temperatures are around 20 degrees Celsius, with mild winters that reach as low as 10 degrees Celsius. There are about 3.6 million residents of Paris, about three-quarters of them citizens, while the rest constitute visitors. The city\'s golden jubilee has been celebrated on July 17th since 1900, with thousands of people in "le Big Ben" taking part, and becoming celebrated around the', 'output_ids': [576, 6722, 315, 9625, 374, 12095, 13, 2160, 1052, 4113, 770, 498, 1035, 1075, 311, 1414, 911, 12095, 30, 5209, 1077, 752, 1414, 1246, 770, 498, 4172, 1075, 311, 10354, 13, 576, 9977, 304, 12095, 374, 8789, 23034, 323, 7324, 19879, 525, 2163, 220, 17, 15, 12348, 61347, 11, 448, 23034, 84982, 429, 5545, 438, 3347, 438, 220, 16, 15, 12348, 61347, 13, 2619, 525, 911, 220, 18, 13, 21, 3526, 10826, 315, 12095, 11, 911, 2326, 82541, 315, 1105, 10283, 11, 1393, 279, 2732, 34156, 15255, 13, 576, 3283, 594, 20748, 96343, 63066, 702, 1012, 27185, 389, 5768, 220, 16, 22, 339, 2474, 220, 16, 24, 15, 15, 11, 448, 9037, 315, 1251, 304, 330, 273, 6164, 7355, 1, 4633, 949, 11, 323, 10454, 27185, 2163, 279], 'meta_info': {'id': '9386078dc2784b5db6bd635e41427eb5', 'finish_reason': {'type': 'length', 'length': 128}, 'prompt_tokens': 7, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 128, 'cached_tokens': 0, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 1.0490214019082487, 'response_sent_to_client_ts': 1774224643.616079}}</strong>


## Get Model Info

Get the information of the model.

- `model_path`: The path/name of the model.
- `is_generation`: Whether the model is used as generation model or embedding model.
- `tokenizer_path`: The path/name of the tokenizer.
- `preferred_sampling_params`: The default sampling params specified via `--preferred-sampling-params`. `None` is returned in this example as we did not explicitly configure it in server args.
- `weight_version`: This field contains the version of the model weights. This is often used to track changes or updates to the model’s trained parameters.
- `has_image_understanding`: Whether the model has image-understanding capability.
- `has_audio_understanding`: Whether the model has audio-understanding capability.
- `model_type`: The model type from the HuggingFace config (e.g., "qwen2", "llama").
- `architectures`: The model architectures from the HuggingFace config (e.g., ["Qwen2ForCausalLM"]).


```python
url = f"http://localhost:{port}/get_model_info"

response = requests.get(url)
response_json = response.json()
print_highlight(response_json)
assert response_json["model_path"] == "qwen/qwen2.5-0.5b-instruct"
assert response_json["is_generation"] is True
assert response_json["tokenizer_path"] == "qwen/qwen2.5-0.5b-instruct"
assert response_json["preferred_sampling_params"] is None
assert response_json.keys() == {
    "model_path",
    "is_generation",
    "tokenizer_path",
    "preferred_sampling_params",
    "weight_version",
    "has_image_understanding",
    "has_audio_understanding",
    "model_type",
    "architectures",
}
```

    [2026-03-23 00:10:43] Endpoint '/get_model_info' is deprecated and will be removed in a future version. Please use '/model_info' instead.



<strong style='color: #00008B;'>{'model_path': 'qwen/qwen2.5-0.5b-instruct', 'tokenizer_path': 'qwen/qwen2.5-0.5b-instruct', 'is_generation': True, 'preferred_sampling_params': None, 'weight_version': 'default', 'has_image_understanding': False, 'has_audio_understanding': False, 'model_type': 'qwen2', 'architectures': ['Qwen2ForCausalLM']}</strong>


## Get Server Info
Gets the server information including CLI arguments, token limits, and memory pool sizes.
- Note: `get_server_info` merges the following deprecated endpoints:
  - `get_server_args`
  - `get_memory_pool_size`
  - `get_max_total_num_tokens`


```python
url = f"http://localhost:{port}/get_server_info"

response = requests.get(url)
print_highlight(response.text)
```

    [2026-03-23 00:10:43] Endpoint '/get_server_info' is deprecated and will be removed in a future version. Please use '/server_info' instead.



<strong style='color: #00008B;'>{"model_path":"qwen/qwen2.5-0.5b-instruct","tokenizer_path":"qwen/qwen2.5-0.5b-instruct","tokenizer_mode":"auto","tokenizer_worker_num":1,"skip_tokenizer_init":false,"load_format":"auto","model_loader_extra_config":"{}","trust_remote_code":false,"context_length":null,"is_embedding":false,"enable_multimodal":null,"revision":null,"model_impl":"auto","host":"0.0.0.0","port":38324,"fastapi_root_path":"","grpc_mode":false,"skip_server_warmup":false,"warmups":null,"nccl_port":null,"checkpoint_engine_wait_weights_before_ready":false,"ssl_keyfile":null,"ssl_certfile":null,"ssl_ca_certs":null,"ssl_keyfile_password":null,"enable_ssl_refresh":false,"dtype":"auto","quantization":null,"quantization_param_path":null,"kv_cache_dtype":"auto","enable_fp32_lm_head":false,"modelopt_quant":null,"modelopt_checkpoint_restore_path":null,"modelopt_checkpoint_save_path":null,"modelopt_export_path":null,"quantize_and_serve":false,"rl_quant_profile":null,"mem_fraction_static":0.907,"max_running_requests":128,"max_queued_requests":null,"max_total_tokens":20480,"chunked_prefill_size":8192,"enable_dynamic_chunking":false,"max_prefill_tokens":16384,"prefill_max_requests":null,"schedule_policy":"fcfs","enable_priority_scheduling":false,"disable_priority_preemption":false,"default_priority_value":null,"abort_on_priority_when_disabled":false,"schedule_low_priority_values_first":false,"priority_scheduling_preemption_threshold":10,"schedule_conservativeness":1.0,"page_size":1,"swa_full_tokens_ratio":0.8,"disable_hybrid_swa_memory":false,"radix_eviction_policy":"lru","enable_prefill_delayer":false,"prefill_delayer_max_delay_passes":30,"prefill_delayer_token_usage_low_watermark":null,"prefill_delayer_forward_passes_buckets":null,"prefill_delayer_wait_seconds_buckets":null,"device":"cuda","tp_size":1,"pp_size":1,"pp_max_micro_batch_size":null,"pp_async_batch_depth":0,"stream_interval":1,"incremental_streaming_output":false,"enable_streaming_session":false,"random_seed":250348221,"constrained_json_whitespace_pattern":null,"constrained_json_disable_any_whitespace":false,"watchdog_timeout":300,"soft_watchdog_timeout":300,"dist_timeout":null,"download_dir":null,"model_checksum":null,"base_gpu_id":0,"gpu_id_step":1,"sleep_on_idle":false,"use_ray":false,"custom_sigquit_handler":null,"log_level":"warning","log_level_http":null,"log_requests":false,"log_requests_level":2,"log_requests_format":"text","log_requests_target":null,"uvicorn_access_log_exclude_prefixes":[],"crash_dump_folder":null,"show_time_cost":false,"enable_metrics":false,"enable_metrics_for_all_schedulers":false,"tokenizer_metrics_custom_labels_header":"x-custom-labels","tokenizer_metrics_allowed_custom_labels":null,"extra_metric_labels":null,"bucket_time_to_first_token":null,"bucket_inter_token_latency":null,"bucket_e2e_request_latency":null,"collect_tokens_histogram":false,"prompt_tokens_buckets":null,"generation_tokens_buckets":null,"gc_warning_threshold_secs":0.0,"decode_log_interval":40,"enable_request_time_stats_logging":false,"kv_events_config":null,"enable_trace":false,"otlp_traces_endpoint":"localhost:4317","export_metrics_to_file":false,"export_metrics_to_file_dir":null,"api_key":null,"admin_api_key":null,"served_model_name":"qwen/qwen2.5-0.5b-instruct","weight_version":"default","chat_template":null,"hf_chat_template_name":null,"completion_template":null,"file_storage_path":"sglang_storage","enable_cache_report":false,"reasoning_parser":null,"tool_call_parser":null,"tool_server":null,"sampling_defaults":"model","dp_size":1,"load_balance_method":"round_robin","attn_cp_size":1,"moe_dp_size":1,"dist_init_addr":null,"nnodes":1,"node_rank":0,"json_model_override_args":"{}","preferred_sampling_params":null,"enable_lora":null,"enable_lora_overlap_loading":null,"max_lora_rank":null,"lora_target_modules":null,"lora_paths":null,"max_loaded_loras":null,"max_loras_per_batch":8,"lora_eviction_policy":"lru","lora_backend":"csgmv","max_lora_chunk_size":16,"attention_backend":"fa3","decode_attention_backend":null,"prefill_attention_backend":null,"sampling_backend":"flashinfer","grammar_backend":"xgrammar","mm_attention_backend":null,"fp8_gemm_runner_backend":"auto","fp4_gemm_runner_backend":"auto","nsa_prefill_backend":null,"nsa_decode_backend":null,"disable_flashinfer_autotune":false,"mamba_backend":"triton","speculative_algorithm":null,"speculative_draft_model_path":null,"speculative_draft_model_revision":null,"speculative_draft_load_format":null,"speculative_num_steps":null,"speculative_eagle_topk":null,"speculative_num_draft_tokens":null,"speculative_accept_threshold_single":1.0,"speculative_accept_threshold_acc":1.0,"speculative_token_map":null,"speculative_attention_mode":"prefill","speculative_draft_attention_backend":null,"speculative_moe_runner_backend":"auto","speculative_moe_a2a_backend":null,"speculative_draft_model_quantization":null,"speculative_ngram_min_match_window_size":1,"speculative_ngram_max_match_window_size":12,"speculative_ngram_min_bfs_breadth":1,"speculative_ngram_max_bfs_breadth":10,"speculative_ngram_match_type":"BFS","speculative_ngram_branch_length":18,"speculative_ngram_capacity":10000000,"enable_multi_layer_eagle":false,"ep_size":1,"moe_a2a_backend":"none","moe_runner_backend":"auto","flashinfer_mxfp4_moe_precision":"default","enable_flashinfer_allreduce_fusion":false,"enable_aiter_allreduce_fusion":false,"deepep_mode":"auto","ep_num_redundant_experts":0,"ep_dispatch_algorithm":null,"init_expert_location":"trivial","enable_eplb":false,"eplb_algorithm":"auto","eplb_rebalance_num_iterations":1000,"eplb_rebalance_layers_per_chunk":null,"eplb_min_rebalancing_utilization_threshold":1.0,"expert_distribution_recorder_mode":null,"expert_distribution_recorder_buffer_size":1000,"enable_expert_distribution_metrics":false,"deepep_config":null,"moe_dense_tp_size":null,"elastic_ep_backend":null,"enable_elastic_expert_backup":false,"mooncake_ib_device":null,"max_mamba_cache_size":null,"mamba_ssm_dtype":null,"mamba_full_memory_ratio":0.9,"mamba_scheduler_strategy":"no_buffer","mamba_track_interval":256,"linear_attn_backend":"triton","linear_attn_decode_backend":null,"linear_attn_prefill_backend":null,"enable_hierarchical_cache":false,"hicache_ratio":2.0,"hicache_size":0,"hicache_write_policy":"write_through","hicache_io_backend":"kernel","hicache_mem_layout":"layer_first","disable_hicache_numa_detect":false,"hicache_storage_backend":null,"hicache_storage_prefetch_policy":"best_effort","hicache_storage_backend_extra_config":null,"hierarchical_sparse_attention_extra_config":null,"enable_lmcache":false,"kt_weight_path":null,"kt_method":"AMXINT4","kt_cpuinfer":null,"kt_threadpool_count":2,"kt_num_gpu_experts":null,"kt_max_deferred_experts_per_token":null,"dllm_algorithm":null,"dllm_algorithm_config":null,"enable_double_sparsity":false,"ds_channel_config_path":null,"ds_heavy_channel_num":32,"ds_heavy_token_num":256,"ds_heavy_channel_type":"qk","ds_sparse_decode_threshold":4096,"cpu_offload_gb":0,"offload_group_size":-1,"offload_num_in_group":1,"offload_prefetch_step":1,"offload_mode":"cpu","multi_item_scoring_delimiter":null,"disable_radix_cache":false,"cuda_graph_max_bs":4,"cuda_graph_bs":[1,2,4],"disable_cuda_graph":true,"disable_cuda_graph_padding":false,"enable_profile_cuda_graph":false,"enable_cudagraph_gc":false,"enable_layerwise_nvtx_marker":false,"enable_nccl_nvls":false,"enable_symm_mem":false,"disable_flashinfer_cutlass_moe_fp4_allgather":false,"enable_tokenizer_batch_encode":false,"disable_tokenizer_batch_decode":false,"disable_outlines_disk_cache":false,"disable_custom_all_reduce":false,"enable_mscclpp":false,"enable_torch_symm_mem":false,"pre_warm_nccl":false,"disable_overlap_schedule":false,"enable_mixed_chunk":false,"enable_dp_attention":false,"enable_dp_lm_head":false,"enable_two_batch_overlap":false,"enable_single_batch_overlap":false,"tbo_token_distribution_threshold":0.48,"enable_torch_compile":false,"disable_piecewise_cuda_graph":false,"enforce_piecewise_cuda_graph":false,"enable_torch_compile_debug_mode":false,"torch_compile_max_bs":32,"piecewise_cuda_graph_max_tokens":8192,"piecewise_cuda_graph_tokens":[4,8,12,16,20,24,28,32,48,64,80,96,112,128,144,160,176,192,208,224,240,256,288,320,352,384,416,448,480,512,576,640,704,768,832,896,960,1024,1280,1536,1792,2048,2304,2560,2816,3072,3328,3584,3840,4096,4608,5120,5632,6144,6656,7168,7680,8192],"piecewise_cuda_graph_compiler":"eager","torchao_config":"","enable_nan_detection":false,"enable_p2p_check":false,"triton_attention_reduce_in_fp32":false,"triton_attention_num_kv_splits":8,"triton_attention_split_tile_size":null,"num_continuous_decode_steps":1,"delete_ckpt_after_loading":false,"enable_memory_saver":false,"enable_weights_cpu_backup":false,"enable_draft_weights_cpu_backup":false,"allow_auto_truncate":false,"enable_custom_logit_processor":false,"flashinfer_mla_disable_ragged":false,"disable_shared_experts_fusion":false,"disable_chunked_prefix_cache":false,"disable_fast_image_processor":false,"keep_mm_feature_on_device":false,"enable_return_hidden_states":false,"enable_return_routed_experts":false,"scheduler_recv_interval":1,"numa_node":null,"enable_deterministic_inference":false,"rl_on_policy_target":null,"enable_attn_tp_input_scattered":false,"enable_nsa_prefill_context_parallel":false,"nsa_prefill_cp_mode":"round-robin-split","enable_fused_qk_norm_rope":false,"enable_precise_embedding_interpolation":false,"enable_fused_moe_sum_all_reduce":false,"enable_prefill_context_parallel":false,"prefill_cp_mode":"in-seq-split","enable_dynamic_batch_tokenizer":false,"dynamic_batch_tokenizer_batch_size":32,"dynamic_batch_tokenizer_batch_timeout":0.002,"debug_tensor_dump_output_folder":null,"debug_tensor_dump_layers":null,"debug_tensor_dump_input_file":null,"debug_tensor_dump_inject":false,"disaggregation_mode":"null","disaggregation_transfer_backend":"mooncake","disaggregation_bootstrap_port":8998,"disaggregation_ib_device":null,"disaggregation_decode_enable_offload_kvcache":false,"num_reserved_decode_tokens":512,"disaggregation_decode_polling_interval":1,"encoder_only":false,"language_only":false,"encoder_transfer_backend":"zmq_to_scheduler","encoder_urls":[],"enable_adaptive_dispatch_to_encoder":false,"custom_weight_loader":[],"weight_loader_disable_mmap":false,"remote_instance_weight_loader_seed_instance_ip":null,"remote_instance_weight_loader_seed_instance_service_port":null,"remote_instance_weight_loader_send_weights_group_ports":null,"remote_instance_weight_loader_backend":"nccl","remote_instance_weight_loader_start_seed_via_transfer_engine":false,"modelexpress_config":null,"enable_pdmux":false,"pdmux_config_path":null,"sm_group_num":8,"mm_max_concurrent_calls":32,"mm_per_request_timeout":10.0,"enable_broadcast_mm_inputs_process":false,"enable_prefix_mm_cache":false,"mm_enable_dp_encoder":false,"mm_process_config":{},"limit_mm_data_per_request":null,"enable_mm_global_cache":false,"decrypted_config_file":null,"decrypted_draft_config_file":null,"forward_hooks":null,"status":"ready","max_total_num_tokens":20480,"max_req_input_len":20474,"internal_states":[{"model_path":"qwen/qwen2.5-0.5b-instruct","tokenizer_path":"qwen/qwen2.5-0.5b-instruct","tokenizer_mode":"auto","tokenizer_worker_num":1,"skip_tokenizer_init":false,"load_format":"auto","model_loader_extra_config":"{}","trust_remote_code":false,"context_length":null,"is_embedding":false,"enable_multimodal":null,"revision":null,"model_impl":"auto","host":"0.0.0.0","port":38324,"fastapi_root_path":"","grpc_mode":false,"skip_server_warmup":false,"warmups":null,"nccl_port":null,"checkpoint_engine_wait_weights_before_ready":false,"ssl_keyfile":null,"ssl_certfile":null,"ssl_ca_certs":null,"ssl_keyfile_password":null,"enable_ssl_refresh":false,"dtype":"auto","quantization":null,"quantization_param_path":null,"kv_cache_dtype":"auto","enable_fp32_lm_head":false,"modelopt_quant":null,"modelopt_checkpoint_restore_path":null,"modelopt_checkpoint_save_path":null,"modelopt_export_path":null,"quantize_and_serve":false,"rl_quant_profile":null,"mem_fraction_static":0.907,"max_running_requests":128,"max_queued_requests":null,"max_total_tokens":20480,"chunked_prefill_size":8192,"enable_dynamic_chunking":false,"max_prefill_tokens":16384,"prefill_max_requests":null,"schedule_policy":"fcfs","enable_priority_scheduling":false,"disable_priority_preemption":false,"default_priority_value":null,"abort_on_priority_when_disabled":false,"schedule_low_priority_values_first":false,"priority_scheduling_preemption_threshold":10,"schedule_conservativeness":1.0,"page_size":1,"swa_full_tokens_ratio":0.8,"disable_hybrid_swa_memory":false,"radix_eviction_policy":"lru","enable_prefill_delayer":false,"prefill_delayer_max_delay_passes":30,"prefill_delayer_token_usage_low_watermark":null,"prefill_delayer_forward_passes_buckets":null,"prefill_delayer_wait_seconds_buckets":null,"device":"cuda","tp_size":1,"pp_size":1,"pp_max_micro_batch_size":128,"pp_async_batch_depth":0,"stream_interval":1,"incremental_streaming_output":false,"enable_streaming_session":false,"random_seed":250348221,"constrained_json_whitespace_pattern":null,"constrained_json_disable_any_whitespace":false,"watchdog_timeout":300,"soft_watchdog_timeout":300,"dist_timeout":null,"download_dir":null,"model_checksum":null,"base_gpu_id":0,"gpu_id_step":1,"sleep_on_idle":false,"use_ray":false,"custom_sigquit_handler":null,"log_level":"warning","log_level_http":null,"log_requests":false,"log_requests_level":2,"log_requests_format":"text","log_requests_target":null,"uvicorn_access_log_exclude_prefixes":[],"crash_dump_folder":null,"show_time_cost":false,"enable_metrics":false,"enable_metrics_for_all_schedulers":false,"tokenizer_metrics_custom_labels_header":"x-custom-labels","tokenizer_metrics_allowed_custom_labels":null,"extra_metric_labels":null,"bucket_time_to_first_token":null,"bucket_inter_token_latency":null,"bucket_e2e_request_latency":null,"collect_tokens_histogram":false,"prompt_tokens_buckets":null,"generation_tokens_buckets":null,"gc_warning_threshold_secs":0.0,"decode_log_interval":40,"enable_request_time_stats_logging":false,"kv_events_config":null,"enable_trace":false,"otlp_traces_endpoint":"localhost:4317","export_metrics_to_file":false,"export_metrics_to_file_dir":null,"api_key":null,"admin_api_key":null,"served_model_name":"qwen/qwen2.5-0.5b-instruct","weight_version":"default","chat_template":null,"hf_chat_template_name":null,"completion_template":null,"file_storage_path":"sglang_storage","enable_cache_report":false,"reasoning_parser":null,"tool_call_parser":null,"tool_server":null,"sampling_defaults":"model","dp_size":1,"load_balance_method":"round_robin","attn_cp_size":1,"moe_dp_size":1,"dist_init_addr":null,"nnodes":1,"node_rank":0,"json_model_override_args":"{}","preferred_sampling_params":null,"enable_lora":null,"enable_lora_overlap_loading":null,"max_lora_rank":null,"lora_target_modules":null,"lora_paths":null,"max_loaded_loras":null,"max_loras_per_batch":8,"lora_eviction_policy":"lru","lora_backend":"csgmv","max_lora_chunk_size":16,"attention_backend":"fa3","decode_attention_backend":"fa3","prefill_attention_backend":"fa3","sampling_backend":"flashinfer","grammar_backend":"xgrammar","mm_attention_backend":null,"fp8_gemm_runner_backend":"auto","fp4_gemm_runner_backend":"auto","nsa_prefill_backend":null,"nsa_decode_backend":null,"disable_flashinfer_autotune":false,"mamba_backend":"triton","speculative_algorithm":null,"speculative_draft_model_path":null,"speculative_draft_model_revision":null,"speculative_draft_load_format":null,"speculative_num_steps":null,"speculative_eagle_topk":null,"speculative_num_draft_tokens":null,"speculative_accept_threshold_single":1.0,"speculative_accept_threshold_acc":1.0,"speculative_token_map":null,"speculative_attention_mode":"prefill","speculative_draft_attention_backend":null,"speculative_moe_runner_backend":"auto","speculative_moe_a2a_backend":null,"speculative_draft_model_quantization":null,"speculative_ngram_min_match_window_size":1,"speculative_ngram_max_match_window_size":12,"speculative_ngram_min_bfs_breadth":1,"speculative_ngram_max_bfs_breadth":10,"speculative_ngram_match_type":"BFS","speculative_ngram_branch_length":18,"speculative_ngram_capacity":10000000,"enable_multi_layer_eagle":false,"ep_size":1,"moe_a2a_backend":"none","moe_runner_backend":"auto","flashinfer_mxfp4_moe_precision":"default","enable_flashinfer_allreduce_fusion":false,"enable_aiter_allreduce_fusion":false,"deepep_mode":"auto","ep_num_redundant_experts":0,"ep_dispatch_algorithm":null,"init_expert_location":"trivial","enable_eplb":false,"eplb_algorithm":"auto","eplb_rebalance_num_iterations":1000,"eplb_rebalance_layers_per_chunk":null,"eplb_min_rebalancing_utilization_threshold":1.0,"expert_distribution_recorder_mode":null,"expert_distribution_recorder_buffer_size":1000,"enable_expert_distribution_metrics":false,"deepep_config":null,"moe_dense_tp_size":null,"elastic_ep_backend":null,"enable_elastic_expert_backup":false,"mooncake_ib_device":null,"max_mamba_cache_size":null,"mamba_ssm_dtype":null,"mamba_full_memory_ratio":0.9,"mamba_scheduler_strategy":"no_buffer","mamba_track_interval":256,"linear_attn_backend":"triton","linear_attn_decode_backend":null,"linear_attn_prefill_backend":null,"enable_hierarchical_cache":false,"hicache_ratio":2.0,"hicache_size":0,"hicache_write_policy":"write_through","hicache_io_backend":"kernel","hicache_mem_layout":"layer_first","disable_hicache_numa_detect":false,"hicache_storage_backend":null,"hicache_storage_prefetch_policy":"best_effort","hicache_storage_backend_extra_config":null,"hierarchical_sparse_attention_extra_config":null,"enable_lmcache":false,"kt_weight_path":null,"kt_method":"AMXINT4","kt_cpuinfer":null,"kt_threadpool_count":2,"kt_num_gpu_experts":null,"kt_max_deferred_experts_per_token":null,"dllm_algorithm":null,"dllm_algorithm_config":null,"enable_double_sparsity":false,"ds_channel_config_path":null,"ds_heavy_channel_num":32,"ds_heavy_token_num":256,"ds_heavy_channel_type":"qk","ds_sparse_decode_threshold":4096,"cpu_offload_gb":0,"offload_group_size":-1,"offload_num_in_group":1,"offload_prefetch_step":1,"offload_mode":"cpu","multi_item_scoring_delimiter":null,"disable_radix_cache":false,"cuda_graph_max_bs":4,"cuda_graph_bs":[1,2,4],"disable_cuda_graph":true,"disable_cuda_graph_padding":false,"enable_profile_cuda_graph":false,"enable_cudagraph_gc":false,"enable_layerwise_nvtx_marker":false,"enable_nccl_nvls":false,"enable_symm_mem":false,"disable_flashinfer_cutlass_moe_fp4_allgather":false,"enable_tokenizer_batch_encode":false,"disable_tokenizer_batch_decode":false,"disable_outlines_disk_cache":false,"disable_custom_all_reduce":false,"enable_mscclpp":false,"enable_torch_symm_mem":false,"pre_warm_nccl":false,"disable_overlap_schedule":false,"enable_mixed_chunk":false,"enable_dp_attention":false,"enable_dp_lm_head":false,"enable_two_batch_overlap":false,"enable_single_batch_overlap":false,"tbo_token_distribution_threshold":0.48,"enable_torch_compile":false,"disable_piecewise_cuda_graph":false,"enforce_piecewise_cuda_graph":false,"enable_torch_compile_debug_mode":false,"torch_compile_max_bs":32,"piecewise_cuda_graph_max_tokens":8192,"piecewise_cuda_graph_tokens":[4,8,12,16,20,24,28,32,48,64,80,96,112,128,144,160,176,192,208,224,240,256,288,320,352,384,416,448,480,512,576,640,704,768,832,896,960,1024,1280,1536,1792,2048,2304,2560,2816,3072,3328,3584,3840,4096,4608,5120,5632,6144,6656,7168,7680,8192],"piecewise_cuda_graph_compiler":"eager","torchao_config":"","enable_nan_detection":false,"enable_p2p_check":false,"triton_attention_reduce_in_fp32":false,"triton_attention_num_kv_splits":8,"triton_attention_split_tile_size":null,"num_continuous_decode_steps":1,"delete_ckpt_after_loading":false,"enable_memory_saver":false,"enable_weights_cpu_backup":false,"enable_draft_weights_cpu_backup":false,"allow_auto_truncate":false,"enable_custom_logit_processor":false,"flashinfer_mla_disable_ragged":false,"disable_shared_experts_fusion":false,"disable_chunked_prefix_cache":true,"disable_fast_image_processor":false,"keep_mm_feature_on_device":false,"enable_return_hidden_states":false,"enable_return_routed_experts":false,"scheduler_recv_interval":1,"numa_node":null,"enable_deterministic_inference":false,"rl_on_policy_target":null,"enable_attn_tp_input_scattered":false,"enable_nsa_prefill_context_parallel":false,"nsa_prefill_cp_mode":"round-robin-split","enable_fused_qk_norm_rope":false,"enable_precise_embedding_interpolation":false,"enable_fused_moe_sum_all_reduce":false,"enable_prefill_context_parallel":false,"prefill_cp_mode":"in-seq-split","enable_dynamic_batch_tokenizer":false,"dynamic_batch_tokenizer_batch_size":32,"dynamic_batch_tokenizer_batch_timeout":0.002,"debug_tensor_dump_output_folder":null,"debug_tensor_dump_layers":null,"debug_tensor_dump_input_file":null,"debug_tensor_dump_inject":false,"disaggregation_mode":"null","disaggregation_transfer_backend":"mooncake","disaggregation_bootstrap_port":8998,"disaggregation_ib_device":null,"disaggregation_decode_enable_offload_kvcache":false,"num_reserved_decode_tokens":512,"disaggregation_decode_polling_interval":1,"encoder_only":false,"language_only":false,"encoder_transfer_backend":"zmq_to_scheduler","encoder_urls":[],"enable_adaptive_dispatch_to_encoder":false,"custom_weight_loader":[],"weight_loader_disable_mmap":false,"remote_instance_weight_loader_seed_instance_ip":null,"remote_instance_weight_loader_seed_instance_service_port":null,"remote_instance_weight_loader_send_weights_group_ports":null,"remote_instance_weight_loader_backend":"nccl","remote_instance_weight_loader_start_seed_via_transfer_engine":false,"modelexpress_config":null,"enable_pdmux":false,"pdmux_config_path":null,"sm_group_num":8,"mm_max_concurrent_calls":32,"mm_per_request_timeout":10.0,"enable_broadcast_mm_inputs_process":false,"enable_prefix_mm_cache":false,"mm_enable_dp_encoder":false,"mm_process_config":{},"limit_mm_data_per_request":null,"enable_mm_global_cache":false,"decrypted_config_file":null,"decrypted_draft_config_file":null,"forward_hooks":null,"use_mla_backend":false,"_mx_config_cache":{},"last_gen_throughput":135.72951214883506,"memory_usage":{"weight":0.98,"kvcache":0.23,"token_capacity":20480,"graph":0},"effective_max_running_requests_per_dp":128}],"version":"0.0.0.dev1+g999bad5ab"}</strong>


## Health Check
- `/health`: Check the health of the server.
- `/health_generate`: Check the health of the server by generating one token.


```python
url = f"http://localhost:{port}/health_generate"

response = requests.get(url)
print_highlight(response.text)
```


<strong style='color: #00008B;'></strong>



```python
url = f"http://localhost:{port}/health"

response = requests.get(url)
print_highlight(response.text)
```


<strong style='color: #00008B;'></strong>


## Flush Cache

Flush the radix cache. It will be automatically triggered when the model weights are updated by the `/update_weights` API.


```python
url = f"http://localhost:{port}/flush_cache"

response = requests.post(url)
print_highlight(response.text)
```


<strong style='color: #00008B;'>Cache flushed.<br>Please check backend logs for more details. (When there are running or waiting requests, the operation will not be performed.)<br></strong>


## Update Weights From Disk

Update model weights from disk without restarting the server. Only applicable for models with the same architecture and parameter size.

SGLang support `update_weights_from_disk` API for continuous evaluation during training (save checkpoint to disk and update weights from disk).



```python
# successful update with same architecture and size

url = f"http://localhost:{port}/update_weights_from_disk"
data = {"model_path": "qwen/qwen2.5-0.5b-instruct"}

response = requests.post(url, json=data)
print_highlight(response.text)
assert response.json()["success"] is True
assert response.json()["message"] == "Succeeded to update model weights."
```

    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  8.12it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  8.10it/s]
    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:324: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)



<strong style='color: #00008B;'>{"success":true,"message":"Succeeded to update model weights.","num_paused_requests":0}</strong>



```python
# failed update with different parameter size or wrong name

url = f"http://localhost:{port}/update_weights_from_disk"
data = {"model_path": "qwen/qwen2.5-0.5b-instruct-wrong"}

response = requests.post(url, json=data)
response_json = response.json()
print_highlight(response_json)
assert response_json["success"] is False
assert response_json["message"] == (
    "Failed to get weights iterator: "
    "qwen/qwen2.5-0.5b-instruct-wrong"
    " (repository not found)."
)
```

    [2026-03-23 00:10:46] Failed to get weights iterator: qwen/qwen2.5-0.5b-instruct-wrong (repository not found).



<strong style='color: #00008B;'>{'success': False, 'message': 'Failed to get weights iterator: qwen/qwen2.5-0.5b-instruct-wrong (repository not found).', 'num_paused_requests': 0}</strong>



```python
terminate_process(server_process)
```

## Encode (embedding model)

Encode text into embeddings. Note that this API is only available for [embedding models](openai_api_embeddings.ipynb) and will raise an error for generation models.
Therefore, we launch a new server to server an embedding model.


```python
embedding_process, port = launch_server_cmd("""
python3 -m sglang.launch_server --model-path Alibaba-NLP/gte-Qwen2-1.5B-instruct \
    --host 0.0.0.0 --is-embedding --log-level warning
""")

wait_for_server(f"http://localhost:{port}", process=embedding_process)
```

    [2026-03-23 00:10:53] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:10:53] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:10:53] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    [2026-03-23 00:10:55] INFO model_config.py:1247: Downcasting torch.float32 to torch.float16.
    [2026-03-23 00:10:55] WARNING model_config.py:1098: Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.
    [2026-03-23 00:10:55] INFO server_args.py:2239: Attention backend not specified. Use fa3 backend by default.


    [2026-03-23 00:10:55] INFO server_args.py:3522: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-23 00:10:56] Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-23 00:11:00] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:11:00] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:11:00] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-23 00:11:00] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:11:00] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:11:00] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-23 00:11:03] Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-23 00:11:04] Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Loading safetensors checkpoint shards:   0% Completed | 0/2 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 1/2 [00:00<00:00,  1.20it/s]

    Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.32s/it]Loading safetensors checkpoint shards: 100% Completed | 2/2 [00:02<00:00,  1.25s/it]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:01,  3.18s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:01,  3.18s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:03<01:17,  1.38s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:03<01:17,  1.38s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:03<01:17,  1.38s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:29,  1.82it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:29,  1.82it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:29,  1.82it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:03<00:29,  1.82it/s]Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:13,  3.84it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:13,  3.84it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:13,  3.84it/s]Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:13,  3.84it/s]

    Compiling num tokens (num_tokens=3584):  12%|█▏        | 7/58 [00:03<00:13,  3.84it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:06,  7.16it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:06,  7.16it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:06,  7.16it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:06,  7.16it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:06,  7.16it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:06,  7.16it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:03<00:03, 12.19it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:03<00:03, 12.19it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:03<00:03, 12.19it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:03<00:03, 12.19it/s]Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:03<00:03, 12.19it/s]Compiling num tokens (num_tokens=1024):  28%|██▊       | 16/58 [00:03<00:03, 12.19it/s]

    Compiling num tokens (num_tokens=960):  28%|██▊       | 16/58 [00:03<00:03, 12.19it/s] Compiling num tokens (num_tokens=896):  28%|██▊       | 16/58 [00:03<00:03, 12.19it/s]Compiling num tokens (num_tokens=896):  40%|███▉      | 23/58 [00:03<00:01, 20.02it/s]Compiling num tokens (num_tokens=832):  40%|███▉      | 23/58 [00:03<00:01, 20.02it/s]Compiling num tokens (num_tokens=768):  40%|███▉      | 23/58 [00:03<00:01, 20.02it/s]Compiling num tokens (num_tokens=704):  40%|███▉      | 23/58 [00:03<00:01, 20.02it/s]Compiling num tokens (num_tokens=640):  40%|███▉      | 23/58 [00:03<00:01, 20.02it/s]Compiling num tokens (num_tokens=576):  40%|███▉      | 23/58 [00:03<00:01, 20.02it/s]Compiling num tokens (num_tokens=512):  40%|███▉      | 23/58 [00:03<00:01, 20.02it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:03<00:01, 26.44it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:03<00:01, 26.44it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:04<00:01, 26.44it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:04<00:01, 26.44it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:04<00:01, 26.44it/s]

    Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:04<00:01, 26.44it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:04<00:01, 26.44it/s]Compiling num tokens (num_tokens=320):  60%|██████    | 35/58 [00:04<00:00, 32.47it/s]Compiling num tokens (num_tokens=288):  60%|██████    | 35/58 [00:04<00:00, 32.47it/s]Compiling num tokens (num_tokens=256):  60%|██████    | 35/58 [00:04<00:00, 32.47it/s]Compiling num tokens (num_tokens=240):  60%|██████    | 35/58 [00:04<00:00, 32.47it/s]Compiling num tokens (num_tokens=224):  60%|██████    | 35/58 [00:04<00:00, 32.47it/s]Compiling num tokens (num_tokens=208):  60%|██████    | 35/58 [00:04<00:00, 32.47it/s]Compiling num tokens (num_tokens=192):  60%|██████    | 35/58 [00:04<00:00, 32.47it/s]Compiling num tokens (num_tokens=192):  71%|███████   | 41/58 [00:04<00:00, 37.00it/s]Compiling num tokens (num_tokens=176):  71%|███████   | 41/58 [00:04<00:00, 37.00it/s]Compiling num tokens (num_tokens=160):  71%|███████   | 41/58 [00:04<00:00, 37.00it/s]Compiling num tokens (num_tokens=144):  71%|███████   | 41/58 [00:04<00:00, 37.00it/s]

    Compiling num tokens (num_tokens=128):  71%|███████   | 41/58 [00:04<00:00, 37.00it/s]Compiling num tokens (num_tokens=112):  71%|███████   | 41/58 [00:04<00:00, 37.00it/s]Compiling num tokens (num_tokens=96):  71%|███████   | 41/58 [00:04<00:00, 37.00it/s] Compiling num tokens (num_tokens=96):  81%|████████  | 47/58 [00:04<00:00, 40.60it/s]Compiling num tokens (num_tokens=80):  81%|████████  | 47/58 [00:04<00:00, 40.60it/s]Compiling num tokens (num_tokens=64):  81%|████████  | 47/58 [00:04<00:00, 40.60it/s]Compiling num tokens (num_tokens=48):  81%|████████  | 47/58 [00:04<00:00, 40.60it/s]Compiling num tokens (num_tokens=32):  81%|████████  | 47/58 [00:04<00:00, 40.60it/s]Compiling num tokens (num_tokens=28):  81%|████████  | 47/58 [00:04<00:00, 40.60it/s]Compiling num tokens (num_tokens=24):  81%|████████  | 47/58 [00:04<00:00, 40.60it/s]Compiling num tokens (num_tokens=24):  91%|█████████▏| 53/58 [00:04<00:00, 44.32it/s]Compiling num tokens (num_tokens=20):  91%|█████████▏| 53/58 [00:04<00:00, 44.32it/s]Compiling num tokens (num_tokens=16):  91%|█████████▏| 53/58 [00:04<00:00, 44.32it/s]

    Compiling num tokens (num_tokens=12):  91%|█████████▏| 53/58 [00:04<00:00, 44.32it/s]Compiling num tokens (num_tokens=8):  91%|█████████▏| 53/58 [00:04<00:00, 44.32it/s] Compiling num tokens (num_tokens=4):  91%|█████████▏| 53/58 [00:04<00:00, 44.32it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 12.88it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=134.25 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=134.25 GB):   2%|▏         | 1/58 [00:00<00:06,  8.88it/s]Capturing num tokens (num_tokens=7680 avail_mem=134.22 GB):   2%|▏         | 1/58 [00:00<00:06,  8.88it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=134.22 GB):   3%|▎         | 2/58 [00:00<00:05,  9.45it/s]Capturing num tokens (num_tokens=7168 avail_mem=134.22 GB):   3%|▎         | 2/58 [00:00<00:05,  9.45it/s]Capturing num tokens (num_tokens=6656 avail_mem=134.22 GB):   3%|▎         | 2/58 [00:00<00:05,  9.45it/s]Capturing num tokens (num_tokens=6656 avail_mem=134.22 GB):   7%|▋         | 4/58 [00:00<00:05, 10.71it/s]Capturing num tokens (num_tokens=6144 avail_mem=134.22 GB):   7%|▋         | 4/58 [00:00<00:05, 10.71it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=134.22 GB):   7%|▋         | 4/58 [00:00<00:05, 10.71it/s]Capturing num tokens (num_tokens=5632 avail_mem=134.22 GB):  10%|█         | 6/58 [00:00<00:04, 12.00it/s]Capturing num tokens (num_tokens=5120 avail_mem=134.22 GB):  10%|█         | 6/58 [00:00<00:04, 12.00it/s]Capturing num tokens (num_tokens=4608 avail_mem=134.22 GB):  10%|█         | 6/58 [00:00<00:04, 12.00it/s]Capturing num tokens (num_tokens=4608 avail_mem=134.22 GB):  14%|█▍        | 8/58 [00:00<00:03, 13.40it/s]Capturing num tokens (num_tokens=4096 avail_mem=134.22 GB):  14%|█▍        | 8/58 [00:00<00:03, 13.40it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=134.22 GB):  14%|█▍        | 8/58 [00:00<00:03, 13.40it/s]Capturing num tokens (num_tokens=3584 avail_mem=134.22 GB):  14%|█▍        | 8/58 [00:00<00:03, 13.40it/s]Capturing num tokens (num_tokens=3584 avail_mem=134.22 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.28it/s]Capturing num tokens (num_tokens=3328 avail_mem=134.21 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.28it/s]Capturing num tokens (num_tokens=3072 avail_mem=134.21 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.28it/s]Capturing num tokens (num_tokens=2816 avail_mem=134.21 GB):  19%|█▉        | 11/58 [00:00<00:02, 16.28it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=134.21 GB):  24%|██▍       | 14/58 [00:00<00:02, 18.76it/s]Capturing num tokens (num_tokens=2560 avail_mem=134.21 GB):  24%|██▍       | 14/58 [00:00<00:02, 18.76it/s]Capturing num tokens (num_tokens=2304 avail_mem=134.20 GB):  24%|██▍       | 14/58 [00:00<00:02, 18.76it/s]Capturing num tokens (num_tokens=2048 avail_mem=134.19 GB):  24%|██▍       | 14/58 [00:00<00:02, 18.76it/s]Capturing num tokens (num_tokens=2048 avail_mem=134.19 GB):  29%|██▉       | 17/58 [00:01<00:01, 21.59it/s]Capturing num tokens (num_tokens=1792 avail_mem=134.19 GB):  29%|██▉       | 17/58 [00:01<00:01, 21.59it/s]Capturing num tokens (num_tokens=1536 avail_mem=134.19 GB):  29%|██▉       | 17/58 [00:01<00:01, 21.59it/s]Capturing num tokens (num_tokens=1280 avail_mem=134.19 GB):  29%|██▉       | 17/58 [00:01<00:01, 21.59it/s]Capturing num tokens (num_tokens=1024 avail_mem=134.18 GB):  29%|██▉       | 17/58 [00:01<00:01, 21.59it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=134.18 GB):  36%|███▌      | 21/58 [00:01<00:01, 25.07it/s]Capturing num tokens (num_tokens=960 avail_mem=134.15 GB):  36%|███▌      | 21/58 [00:01<00:01, 25.07it/s] Capturing num tokens (num_tokens=896 avail_mem=134.15 GB):  36%|███▌      | 21/58 [00:01<00:01, 25.07it/s]Capturing num tokens (num_tokens=832 avail_mem=134.16 GB):  36%|███▌      | 21/58 [00:01<00:01, 25.07it/s]Capturing num tokens (num_tokens=832 avail_mem=134.16 GB):  41%|████▏     | 24/58 [00:01<00:01, 23.58it/s]Capturing num tokens (num_tokens=768 avail_mem=134.15 GB):  41%|████▏     | 24/58 [00:01<00:01, 23.58it/s]Capturing num tokens (num_tokens=704 avail_mem=134.14 GB):  41%|████▏     | 24/58 [00:01<00:01, 23.58it/s]

    Capturing num tokens (num_tokens=640 avail_mem=134.14 GB):  41%|████▏     | 24/58 [00:01<00:01, 23.58it/s]Capturing num tokens (num_tokens=640 avail_mem=134.14 GB):  47%|████▋     | 27/58 [00:01<00:01, 22.88it/s]Capturing num tokens (num_tokens=576 avail_mem=134.14 GB):  47%|████▋     | 27/58 [00:01<00:01, 22.88it/s]Capturing num tokens (num_tokens=512 avail_mem=134.11 GB):  47%|████▋     | 27/58 [00:01<00:01, 22.88it/s]Capturing num tokens (num_tokens=480 avail_mem=133.64 GB):  47%|████▋     | 27/58 [00:01<00:01, 22.88it/s]Capturing num tokens (num_tokens=480 avail_mem=133.64 GB):  52%|█████▏    | 30/58 [00:01<00:01, 23.30it/s]Capturing num tokens (num_tokens=448 avail_mem=133.63 GB):  52%|█████▏    | 30/58 [00:01<00:01, 23.30it/s]Capturing num tokens (num_tokens=416 avail_mem=133.53 GB):  52%|█████▏    | 30/58 [00:01<00:01, 23.30it/s]

    Capturing num tokens (num_tokens=384 avail_mem=133.46 GB):  52%|█████▏    | 30/58 [00:01<00:01, 23.30it/s]Capturing num tokens (num_tokens=352 avail_mem=133.46 GB):  52%|█████▏    | 30/58 [00:01<00:01, 23.30it/s]Capturing num tokens (num_tokens=352 avail_mem=133.46 GB):  59%|█████▊    | 34/58 [00:01<00:00, 27.07it/s]Capturing num tokens (num_tokens=320 avail_mem=133.46 GB):  59%|█████▊    | 34/58 [00:01<00:00, 27.07it/s]Capturing num tokens (num_tokens=288 avail_mem=133.47 GB):  59%|█████▊    | 34/58 [00:01<00:00, 27.07it/s]Capturing num tokens (num_tokens=256 avail_mem=133.46 GB):  59%|█████▊    | 34/58 [00:01<00:00, 27.07it/s]Capturing num tokens (num_tokens=240 avail_mem=133.46 GB):  59%|█████▊    | 34/58 [00:01<00:00, 27.07it/s]Capturing num tokens (num_tokens=240 avail_mem=133.46 GB):  66%|██████▌   | 38/58 [00:01<00:00, 30.11it/s]Capturing num tokens (num_tokens=224 avail_mem=133.46 GB):  66%|██████▌   | 38/58 [00:01<00:00, 30.11it/s]Capturing num tokens (num_tokens=208 avail_mem=133.45 GB):  66%|██████▌   | 38/58 [00:01<00:00, 30.11it/s]

    Capturing num tokens (num_tokens=192 avail_mem=133.45 GB):  66%|██████▌   | 38/58 [00:01<00:00, 30.11it/s]Capturing num tokens (num_tokens=176 avail_mem=133.45 GB):  66%|██████▌   | 38/58 [00:01<00:00, 30.11it/s]Capturing num tokens (num_tokens=176 avail_mem=133.45 GB):  72%|███████▏  | 42/58 [00:01<00:00, 30.16it/s]Capturing num tokens (num_tokens=160 avail_mem=133.44 GB):  72%|███████▏  | 42/58 [00:01<00:00, 30.16it/s]Capturing num tokens (num_tokens=144 avail_mem=133.44 GB):  72%|███████▏  | 42/58 [00:01<00:00, 30.16it/s]Capturing num tokens (num_tokens=128 avail_mem=133.43 GB):  72%|███████▏  | 42/58 [00:01<00:00, 30.16it/s]Capturing num tokens (num_tokens=112 avail_mem=133.43 GB):  72%|███████▏  | 42/58 [00:01<00:00, 30.16it/s]Capturing num tokens (num_tokens=112 avail_mem=133.43 GB):  79%|███████▉  | 46/58 [00:02<00:00, 31.36it/s]Capturing num tokens (num_tokens=96 avail_mem=133.43 GB):  79%|███████▉  | 46/58 [00:02<00:00, 31.36it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=133.42 GB):  79%|███████▉  | 46/58 [00:02<00:00, 31.36it/s]Capturing num tokens (num_tokens=64 avail_mem=133.42 GB):  79%|███████▉  | 46/58 [00:02<00:00, 31.36it/s]Capturing num tokens (num_tokens=48 avail_mem=133.42 GB):  79%|███████▉  | 46/58 [00:02<00:00, 31.36it/s]Capturing num tokens (num_tokens=48 avail_mem=133.42 GB):  86%|████████▌ | 50/58 [00:02<00:00, 31.20it/s]Capturing num tokens (num_tokens=32 avail_mem=133.41 GB):  86%|████████▌ | 50/58 [00:02<00:00, 31.20it/s]Capturing num tokens (num_tokens=28 avail_mem=133.41 GB):  86%|████████▌ | 50/58 [00:02<00:00, 31.20it/s]Capturing num tokens (num_tokens=24 avail_mem=133.41 GB):  86%|████████▌ | 50/58 [00:02<00:00, 31.20it/s]Capturing num tokens (num_tokens=20 avail_mem=133.40 GB):  86%|████████▌ | 50/58 [00:02<00:00, 31.20it/s]

    Capturing num tokens (num_tokens=20 avail_mem=133.40 GB):  93%|█████████▎| 54/58 [00:02<00:00, 29.99it/s]Capturing num tokens (num_tokens=16 avail_mem=133.40 GB):  93%|█████████▎| 54/58 [00:02<00:00, 29.99it/s]Capturing num tokens (num_tokens=12 avail_mem=133.39 GB):  93%|█████████▎| 54/58 [00:02<00:00, 29.99it/s]Capturing num tokens (num_tokens=8 avail_mem=133.39 GB):  93%|█████████▎| 54/58 [00:02<00:00, 29.99it/s] Capturing num tokens (num_tokens=4 avail_mem=133.39 GB):  93%|█████████▎| 54/58 [00:02<00:00, 29.99it/s]Capturing num tokens (num_tokens=4 avail_mem=133.39 GB): 100%|██████████| 58/58 [00:02<00:00, 28.74it/s]Capturing num tokens (num_tokens=4 avail_mem=133.39 GB): 100%|██████████| 58/58 [00:02<00:00, 23.83it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:116: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



```python
# successful encode for embedding model

url = f"http://localhost:{port}/encode"
data = {"model": "Alibaba-NLP/gte-Qwen2-1.5B-instruct", "text": "Once upon a time"}

response = requests.post(url, json=data)
response_json = response.json()
print_highlight(f"Text embedding (first 10): {response_json['embedding'][:10]}")
```


<strong style='color: #00008B;'>Text embedding (first 10): [-0.00023698806762695312, -0.0499267578125, -0.0032749176025390625, 0.0110931396484375, -0.01406097412109375, 0.016021728515625, -0.01444244384765625, 0.005901336669921875, -0.022796630859375, 0.0272979736328125]</strong>



```python
terminate_process(embedding_process)
```

## v1/rerank (cross encoder rerank model)
Rerank a list of documents given a query using a cross-encoder model. Note that this API is only available for cross encoder model like [BAAI/bge-reranker-v2-m3](https://huggingface.co/BAAI/bge-reranker-v2-m3) with `attention-backend` `triton` and `torch_native`.



```python
reranker_process, port = launch_server_cmd("""
python3 -m sglang.launch_server --model-path BAAI/bge-reranker-v2-m3 \
    --host 0.0.0.0 --disable-radix-cache --chunked-prefill-size -1 --attention-backend triton --is-embedding --log-level warning
""")

wait_for_server(f"http://localhost:{port}", process=reranker_process)
```

    [2026-03-23 00:11:28] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:11:28] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:11:28] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    [2026-03-23 00:11:30] INFO model_config.py:1247: Downcasting torch.float32 to torch.float16.
    [2026-03-23 00:11:30] WARNING model_config.py:1098: Transformers version 5.3.0 is used for model type xlm-roberta. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.
    [2026-03-23 00:11:31] INFO server_args.py:3522: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-23 00:11:31] Transformers version 5.3.0 is used for model type xlm-roberta. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-23 00:11:34] No HuggingFace chat template found


    [2026-03-23 00:11:36] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:11:36] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:11:36] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-23 00:11:36] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:11:36] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:11:36] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-23 00:11:38] Transformers version 5.3.0 is used for model type xlm-roberta. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-23 00:11:41] Transformers version 5.3.0 is used for model type xlm-roberta. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  1.34it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  1.34it/s]


    [2026-03-23 00:11:44] Disable piecewise CUDA graph because the model is not a language model


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:116: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



```python
# compute rerank scores for query and documents

url = f"http://localhost:{port}/v1/rerank"
data = {
    "model": "BAAI/bge-reranker-v2-m3",
    "query": "what is panda?",
    "documents": [
        "hi",
        "The giant panda (Ailuropoda melanoleuca), sometimes called a panda bear or simply panda, is a bear species endemic to China.",
    ],
}

response = requests.post(url, json=data)
response_json = response.json()
for item in response_json:
    print_highlight(f"Score: {item['score']:.2f} - Document: '{item['document']}'")
```


<strong style='color: #00008B;'>Score: 5.27 - Document: 'The giant panda (Ailuropoda melanoleuca), sometimes called a panda bear or simply panda, is a bear species endemic to China.'</strong>



<strong style='color: #00008B;'>Score: -8.19 - Document: 'hi'</strong>



```python
terminate_process(reranker_process)
```

## v1/score (decoder-only scoring)

Compute token probabilities for specified tokens given a query and items. This is useful for classification tasks, scoring responses, or computing log-probabilities.

Parameters:
- `query`: Query text
- `items`: Item text(s) to score
- `label_token_ids`: Token IDs to compute probabilities for
- `apply_softmax`: Whether to apply softmax to get normalized probabilities (default: False)
- `item_first`: Whether items come first in concatenation order (default: False)
- `model`: Model name

The response contains `scores` - a list of probability lists, one per item, each in the order of `label_token_ids`.


```python
score_process, port = launch_server_cmd("""
python3 -m sglang.launch_server --model-path qwen/qwen2.5-0.5b-instruct \
    --host 0.0.0.0 --log-level warning
""")

wait_for_server(f"http://localhost:{port}", process=score_process)
```

    [2026-03-23 00:11:57] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:11:57] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:11:57] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    [2026-03-23 00:11:59] WARNING model_config.py:1098: Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.
    [2026-03-23 00:11:59] INFO server_args.py:2239: Attention backend not specified. Use fa3 backend by default.
    [2026-03-23 00:11:59] INFO server_args.py:3522: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-23 00:12:00] Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-23 00:12:05] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:12:05] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:12:05] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-23 00:12:05] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:12:05] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:12:05] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-23 00:12:07] Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-23 00:12:09] Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.68it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.68it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:23,  2.52s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:23,  2.52s/it]Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:02<01:01,  1.10s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:02<01:01,  1.10s/it]Compiling num tokens (num_tokens=6656):   3%|▎         | 2/58 [00:02<01:01,  1.10s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:24,  2.21it/s]Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:02<00:13,  3.79it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:02<00:13,  3.79it/s]Compiling num tokens (num_tokens=4608):  10%|█         | 6/58 [00:02<00:13,  3.79it/s]Compiling num tokens (num_tokens=4096):  10%|█         | 6/58 [00:02<00:13,  3.79it/s]Compiling num tokens (num_tokens=3840):  10%|█         | 6/58 [00:02<00:13,  3.79it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:06,  7.70it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:06,  7.70it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:03<00:06,  7.70it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:03<00:06,  7.70it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:03<00:04, 10.72it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:03<00:04, 10.72it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:03<00:04, 10.72it/s]Compiling num tokens (num_tokens=2304):  22%|██▏       | 13/58 [00:03<00:04, 10.72it/s]Compiling num tokens (num_tokens=2048):  22%|██▏       | 13/58 [00:03<00:04, 10.72it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:03<00:02, 15.12it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:03<00:02, 15.12it/s]Compiling num tokens (num_tokens=1536):  29%|██▉       | 17/58 [00:03<00:02, 15.12it/s]Compiling num tokens (num_tokens=1280):  29%|██▉       | 17/58 [00:03<00:02, 15.12it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:03<00:02, 17.68it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:03<00:02, 17.68it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:03<00:02, 17.68it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:03<00:02, 17.68it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:03<00:02, 17.68it/s]

    Compiling num tokens (num_tokens=768):  34%|███▍      | 20/58 [00:03<00:02, 17.68it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:03<00:01, 22.89it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:03<00:01, 22.89it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:03<00:01, 22.89it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:03<00:01, 22.89it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:03<00:01, 22.89it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:03<00:01, 25.31it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:03<00:01, 25.31it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:03<00:01, 25.31it/s]

    Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:03<00:01, 25.31it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:03<00:01, 25.31it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:03<00:01, 25.31it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:03<00:00, 29.49it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:03<00:00, 29.49it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:03<00:00, 29.49it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:00, 29.49it/s]

    Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:00, 29.49it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 26.31it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 26.31it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 26.31it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 26.31it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:04<00:00, 26.31it/s]

    Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:04<00:00, 25.18it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:04<00:00, 25.18it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:04<00:00, 25.18it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:04<00:00, 25.18it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:04<00:00, 24.90it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:04<00:00, 24.90it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:04<00:00, 24.90it/s] 

    Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:04<00:00, 24.90it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:04<00:00, 23.39it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:04<00:00, 23.39it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:04<00:00, 23.39it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:04<00:00, 23.39it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:04<00:00, 23.03it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:04<00:00, 23.03it/s]

    Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:04<00:00, 23.03it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:04<00:00, 23.03it/s]Compiling num tokens (num_tokens=20):  93%|█████████▎| 54/58 [00:04<00:00, 24.21it/s]Compiling num tokens (num_tokens=16):  93%|█████████▎| 54/58 [00:04<00:00, 24.21it/s]Compiling num tokens (num_tokens=12):  93%|█████████▎| 54/58 [00:04<00:00, 24.21it/s]Compiling num tokens (num_tokens=8):  93%|█████████▎| 54/58 [00:04<00:00, 24.21it/s] Compiling num tokens (num_tokens=4):  93%|█████████▎| 54/58 [00:04<00:00, 24.21it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 26.30it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:04<00:00, 12.33it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=99.71 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=99.71 GB):   2%|▏         | 1/58 [00:00<00:11,  4.99it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=99.75 GB):   2%|▏         | 1/58 [00:00<00:11,  4.99it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=99.75 GB):   3%|▎         | 2/58 [00:00<00:11,  4.73it/s]Capturing num tokens (num_tokens=7168 avail_mem=99.74 GB):   3%|▎         | 2/58 [00:00<00:11,  4.73it/s]Capturing num tokens (num_tokens=7168 avail_mem=99.74 GB):   5%|▌         | 3/58 [00:00<00:10,  5.19it/s]Capturing num tokens (num_tokens=6656 avail_mem=99.82 GB):   5%|▌         | 3/58 [00:00<00:10,  5.19it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=99.82 GB):   7%|▋         | 4/58 [00:00<00:10,  4.98it/s]Capturing num tokens (num_tokens=6144 avail_mem=99.82 GB):   7%|▋         | 4/58 [00:00<00:10,  4.98it/s]Capturing num tokens (num_tokens=6144 avail_mem=99.82 GB):   9%|▊         | 5/58 [00:00<00:09,  5.53it/s]Capturing num tokens (num_tokens=5632 avail_mem=100.34 GB):   9%|▊         | 5/58 [00:00<00:09,  5.53it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=100.34 GB):  10%|█         | 6/58 [00:01<00:11,  4.46it/s]Capturing num tokens (num_tokens=5120 avail_mem=100.34 GB):  10%|█         | 6/58 [00:01<00:11,  4.46it/s]Capturing num tokens (num_tokens=5120 avail_mem=100.34 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.98it/s]Capturing num tokens (num_tokens=4608 avail_mem=99.87 GB):  12%|█▏        | 7/58 [00:01<00:10,  4.98it/s] 

    Capturing num tokens (num_tokens=4608 avail_mem=99.87 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.28it/s]Capturing num tokens (num_tokens=4096 avail_mem=100.34 GB):  14%|█▍        | 8/58 [00:01<00:09,  5.28it/s]Capturing num tokens (num_tokens=4096 avail_mem=100.34 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.76it/s]Capturing num tokens (num_tokens=3840 avail_mem=99.90 GB):  16%|█▌        | 9/58 [00:01<00:08,  5.76it/s] 

    Capturing num tokens (num_tokens=3840 avail_mem=99.90 GB):  17%|█▋        | 10/58 [00:01<00:08,  5.92it/s]Capturing num tokens (num_tokens=3584 avail_mem=100.33 GB):  17%|█▋        | 10/58 [00:01<00:08,  5.92it/s]Capturing num tokens (num_tokens=3584 avail_mem=100.33 GB):  19%|█▉        | 11/58 [00:01<00:07,  6.51it/s]Capturing num tokens (num_tokens=3328 avail_mem=99.93 GB):  19%|█▉        | 11/58 [00:01<00:07,  6.51it/s] 

    Capturing num tokens (num_tokens=3328 avail_mem=99.93 GB):  21%|██        | 12/58 [00:02<00:07,  6.57it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.32 GB):  21%|██        | 12/58 [00:02<00:07,  6.57it/s]Capturing num tokens (num_tokens=3072 avail_mem=100.32 GB):  22%|██▏       | 13/58 [00:02<00:06,  7.00it/s]Capturing num tokens (num_tokens=2816 avail_mem=99.96 GB):  22%|██▏       | 13/58 [00:02<00:06,  7.00it/s] 

    Capturing num tokens (num_tokens=2816 avail_mem=99.96 GB):  24%|██▍       | 14/58 [00:02<00:06,  7.27it/s]Capturing num tokens (num_tokens=2560 avail_mem=100.32 GB):  24%|██▍       | 14/58 [00:02<00:06,  7.27it/s]Capturing num tokens (num_tokens=2560 avail_mem=100.32 GB):  26%|██▌       | 15/58 [00:02<00:06,  6.99it/s]Capturing num tokens (num_tokens=2304 avail_mem=99.98 GB):  26%|██▌       | 15/58 [00:02<00:06,  6.99it/s] 

    Capturing num tokens (num_tokens=2304 avail_mem=99.98 GB):  28%|██▊       | 16/58 [00:02<00:05,  7.49it/s]Capturing num tokens (num_tokens=2048 avail_mem=100.31 GB):  28%|██▊       | 16/58 [00:02<00:05,  7.49it/s]Capturing num tokens (num_tokens=2048 avail_mem=100.31 GB):  29%|██▉       | 17/58 [00:02<00:05,  7.43it/s]Capturing num tokens (num_tokens=1792 avail_mem=100.31 GB):  29%|██▉       | 17/58 [00:02<00:05,  7.43it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=100.31 GB):  31%|███       | 18/58 [00:02<00:05,  7.87it/s]Capturing num tokens (num_tokens=1536 avail_mem=100.04 GB):  31%|███       | 18/58 [00:02<00:05,  7.87it/s]Capturing num tokens (num_tokens=1536 avail_mem=100.04 GB):  33%|███▎      | 19/58 [00:03<00:04,  8.05it/s]Capturing num tokens (num_tokens=1280 avail_mem=100.30 GB):  33%|███▎      | 19/58 [00:03<00:04,  8.05it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=100.30 GB):  34%|███▍      | 20/58 [00:03<00:04,  8.00it/s]Capturing num tokens (num_tokens=1024 avail_mem=100.28 GB):  34%|███▍      | 20/58 [00:03<00:04,  8.00it/s]Capturing num tokens (num_tokens=960 avail_mem=100.28 GB):  34%|███▍      | 20/58 [00:03<00:04,  8.00it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=100.28 GB):  38%|███▊      | 22/58 [00:03<00:04,  8.77it/s]Capturing num tokens (num_tokens=896 avail_mem=100.29 GB):  38%|███▊      | 22/58 [00:03<00:04,  8.77it/s]Capturing num tokens (num_tokens=832 avail_mem=100.28 GB):  38%|███▊      | 22/58 [00:03<00:04,  8.77it/s]Capturing num tokens (num_tokens=832 avail_mem=100.28 GB):  41%|████▏     | 24/58 [00:03<00:03, 10.06it/s]Capturing num tokens (num_tokens=768 avail_mem=100.28 GB):  41%|████▏     | 24/58 [00:03<00:03, 10.06it/s]

    Capturing num tokens (num_tokens=704 avail_mem=100.27 GB):  41%|████▏     | 24/58 [00:03<00:03, 10.06it/s]Capturing num tokens (num_tokens=704 avail_mem=100.27 GB):  45%|████▍     | 26/58 [00:03<00:02, 10.92it/s]Capturing num tokens (num_tokens=640 avail_mem=100.14 GB):  45%|████▍     | 26/58 [00:03<00:02, 10.92it/s]Capturing num tokens (num_tokens=576 avail_mem=100.26 GB):  45%|████▍     | 26/58 [00:03<00:02, 10.92it/s]Capturing num tokens (num_tokens=576 avail_mem=100.26 GB):  48%|████▊     | 28/58 [00:03<00:02, 12.08it/s]Capturing num tokens (num_tokens=512 avail_mem=100.25 GB):  48%|████▊     | 28/58 [00:03<00:02, 12.08it/s]

    Capturing num tokens (num_tokens=480 avail_mem=100.26 GB):  48%|████▊     | 28/58 [00:03<00:02, 12.08it/s]Capturing num tokens (num_tokens=480 avail_mem=100.26 GB):  52%|█████▏    | 30/58 [00:03<00:02, 13.07it/s]Capturing num tokens (num_tokens=448 avail_mem=100.26 GB):  52%|█████▏    | 30/58 [00:03<00:02, 13.07it/s]Capturing num tokens (num_tokens=416 avail_mem=100.16 GB):  52%|█████▏    | 30/58 [00:03<00:02, 13.07it/s]Capturing num tokens (num_tokens=384 avail_mem=100.25 GB):  52%|█████▏    | 30/58 [00:04<00:02, 13.07it/s]Capturing num tokens (num_tokens=384 avail_mem=100.25 GB):  57%|█████▋    | 33/58 [00:04<00:01, 15.59it/s]

    Capturing num tokens (num_tokens=352 avail_mem=100.24 GB):  57%|█████▋    | 33/58 [00:04<00:01, 15.59it/s]Capturing num tokens (num_tokens=320 avail_mem=100.23 GB):  57%|█████▋    | 33/58 [00:04<00:01, 15.59it/s]Capturing num tokens (num_tokens=320 avail_mem=100.23 GB):  60%|██████    | 35/58 [00:04<00:01, 16.36it/s]Capturing num tokens (num_tokens=288 avail_mem=100.22 GB):  60%|██████    | 35/58 [00:04<00:01, 16.36it/s]Capturing num tokens (num_tokens=256 avail_mem=100.22 GB):  60%|██████    | 35/58 [00:04<00:01, 16.36it/s]

    Capturing num tokens (num_tokens=256 avail_mem=100.22 GB):  64%|██████▍   | 37/58 [00:04<00:01, 16.74it/s]Capturing num tokens (num_tokens=240 avail_mem=100.21 GB):  64%|██████▍   | 37/58 [00:04<00:01, 16.74it/s]Capturing num tokens (num_tokens=224 avail_mem=100.20 GB):  64%|██████▍   | 37/58 [00:04<00:01, 16.74it/s]Capturing num tokens (num_tokens=208 avail_mem=100.19 GB):  64%|██████▍   | 37/58 [00:04<00:01, 16.74it/s]Capturing num tokens (num_tokens=208 avail_mem=100.19 GB):  69%|██████▉   | 40/58 [00:04<00:00, 18.16it/s]Capturing num tokens (num_tokens=192 avail_mem=100.15 GB):  69%|██████▉   | 40/58 [00:04<00:00, 18.16it/s]Capturing num tokens (num_tokens=176 avail_mem=100.15 GB):  69%|██████▉   | 40/58 [00:04<00:00, 18.16it/s]

    Capturing num tokens (num_tokens=160 avail_mem=100.15 GB):  69%|██████▉   | 40/58 [00:04<00:00, 18.16it/s]Capturing num tokens (num_tokens=144 avail_mem=100.15 GB):  69%|██████▉   | 40/58 [00:04<00:00, 18.16it/s]Capturing num tokens (num_tokens=144 avail_mem=100.15 GB):  76%|███████▌  | 44/58 [00:04<00:00, 22.14it/s]Capturing num tokens (num_tokens=128 avail_mem=100.14 GB):  76%|███████▌  | 44/58 [00:04<00:00, 22.14it/s]Capturing num tokens (num_tokens=112 avail_mem=100.18 GB):  76%|███████▌  | 44/58 [00:04<00:00, 22.14it/s]Capturing num tokens (num_tokens=96 avail_mem=100.12 GB):  76%|███████▌  | 44/58 [00:04<00:00, 22.14it/s] Capturing num tokens (num_tokens=80 avail_mem=100.14 GB):  76%|███████▌  | 44/58 [00:04<00:00, 22.14it/s]Capturing num tokens (num_tokens=80 avail_mem=100.14 GB):  83%|████████▎ | 48/58 [00:04<00:00, 24.70it/s]Capturing num tokens (num_tokens=64 avail_mem=100.13 GB):  83%|████████▎ | 48/58 [00:04<00:00, 24.70it/s]

    Capturing num tokens (num_tokens=48 avail_mem=100.13 GB):  83%|████████▎ | 48/58 [00:04<00:00, 24.70it/s]Capturing num tokens (num_tokens=32 avail_mem=100.13 GB):  83%|████████▎ | 48/58 [00:04<00:00, 24.70it/s]Capturing num tokens (num_tokens=28 avail_mem=100.12 GB):  83%|████████▎ | 48/58 [00:04<00:00, 24.70it/s]Capturing num tokens (num_tokens=28 avail_mem=100.12 GB):  90%|████████▉ | 52/58 [00:04<00:00, 27.09it/s]Capturing num tokens (num_tokens=24 avail_mem=100.12 GB):  90%|████████▉ | 52/58 [00:04<00:00, 27.09it/s]Capturing num tokens (num_tokens=20 avail_mem=100.10 GB):  90%|████████▉ | 52/58 [00:04<00:00, 27.09it/s]Capturing num tokens (num_tokens=16 avail_mem=99.07 GB):  90%|████████▉ | 52/58 [00:04<00:00, 27.09it/s] 

    Capturing num tokens (num_tokens=16 avail_mem=99.07 GB):  95%|█████████▍| 55/58 [00:04<00:00, 22.40it/s]Capturing num tokens (num_tokens=12 avail_mem=99.06 GB):  95%|█████████▍| 55/58 [00:04<00:00, 22.40it/s]Capturing num tokens (num_tokens=8 avail_mem=99.06 GB):  95%|█████████▍| 55/58 [00:05<00:00, 22.40it/s] Capturing num tokens (num_tokens=4 avail_mem=100.71 GB):  95%|█████████▍| 55/58 [00:05<00:00, 22.40it/s]

    Capturing num tokens (num_tokens=4 avail_mem=100.71 GB): 100%|██████████| 58/58 [00:05<00:00, 18.68it/s]Capturing num tokens (num_tokens=4 avail_mem=100.71 GB): 100%|██████████| 58/58 [00:05<00:00, 11.11it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:116: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



```python
# Score the probability of different completions given a query
query = "The capital of France is"
items = ["Paris", "London", "Berlin"]

url = f"http://localhost:{port}/v1/score"
data = {
    "model": "qwen/qwen2.5-0.5b-instruct",
    "query": query,
    "items": items,
    "label_token_ids": [9454, 2753],  # e.g. "Yes" and "No" token ids
    "apply_softmax": True,  # Normalize probabilities to sum to 1
}

response = requests.post(url, json=data)
response_json = response.json()

# Display scores for each item
for item, scores in zip(items, response_json["scores"]):
    print_highlight(f"Item '{item}': probabilities = {[f'{s:.4f}' for s in scores]}")
```


<strong style='color: #00008B;'>Item 'Paris': probabilities = ['0.0237', '0.9763']</strong>



<strong style='color: #00008B;'>Item 'London': probabilities = ['0.0284', '0.9716']</strong>



<strong style='color: #00008B;'>Item 'Berlin': probabilities = ['0.0637', '0.9363']</strong>



```python
terminate_process(score_process)
```

## Classify (reward model)

SGLang Runtime also supports reward models. Here we use a reward model to classify the quality of pairwise generations.


```python
# Note that SGLang now treats embedding models and reward models as the same type of models.
# This will be updated in the future.

reward_process, port = launch_server_cmd("""
python3 -m sglang.launch_server --model-path Skywork/Skywork-Reward-Llama-3.1-8B-v0.2 --host 0.0.0.0 --is-embedding --log-level warning
""")

wait_for_server(f"http://localhost:{port}", process=reward_process)
```

    [2026-03-23 00:12:35] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:12:35] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:12:35] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    [2026-03-23 00:12:37] WARNING model_config.py:1098: Transformers version 5.3.0 is used for model type llama. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.
    [2026-03-23 00:12:37] INFO server_args.py:2239: Attention backend not specified. Use fa3 backend by default.
    [2026-03-23 00:12:37] INFO server_args.py:3522: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-23 00:12:38] Transformers version 5.3.0 is used for model type llama. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-23 00:12:42] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:12:42] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:12:42] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-23 00:12:42] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:12:42] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:12:42] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-23 00:12:44] Transformers version 5.3.0 is used for model type llama. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-23 00:12:47] Transformers version 5.3.0 is used for model type llama. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Loading safetensors checkpoint shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  50% Completed | 2/4 [00:00<00:00,  2.53it/s]

    Loading safetensors checkpoint shards:  75% Completed | 3/4 [00:01<00:00,  1.62it/s]

    Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.32it/s]Loading safetensors checkpoint shards: 100% Completed | 4/4 [00:02<00:00,  1.48it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:03<03:29,  3.67s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:03<03:29,  3.67s/it]

    Compiling num tokens (num_tokens=7680):   3%|▎         | 2/58 [00:04<01:53,  2.03s/it]Compiling num tokens (num_tokens=7168):   3%|▎         | 2/58 [00:04<01:53,  2.03s/it]

    Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:05<01:12,  1.31s/it]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:05<01:12,  1.31s/it]

    Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:05<00:51,  1.06it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:05<00:51,  1.06it/s]

    Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:05<00:39,  1.35it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:05<00:39,  1.35it/s]

    Compiling num tokens (num_tokens=5632):  10%|█         | 6/58 [00:06<00:31,  1.67it/s]Compiling num tokens (num_tokens=5120):  10%|█         | 6/58 [00:06<00:31,  1.67it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:06<00:25,  2.01it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:06<00:25,  2.01it/s]

    Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:06<00:21,  2.36it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:06<00:21,  2.36it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:06<00:17,  2.75it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:06<00:17,  2.75it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:07<00:15,  3.16it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:07<00:15,  3.16it/s]

    Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:07<00:13,  3.53it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:07<00:13,  3.53it/s]Compiling num tokens (num_tokens=3328):  21%|██        | 12/58 [00:07<00:11,  3.96it/s]Compiling num tokens (num_tokens=3072):  21%|██        | 12/58 [00:07<00:11,  3.96it/s]

    Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:07<00:10,  4.40it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:07<00:10,  4.40it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:07<00:09,  4.83it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:07<00:09,  4.83it/s]

    Compiling num tokens (num_tokens=2560):  26%|██▌       | 15/58 [00:07<00:08,  5.36it/s]Compiling num tokens (num_tokens=2304):  26%|██▌       | 15/58 [00:07<00:08,  5.36it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:08<00:07,  5.98it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:08<00:07,  5.98it/s]

    Compiling num tokens (num_tokens=2048):  29%|██▉       | 17/58 [00:08<00:06,  6.53it/s]Compiling num tokens (num_tokens=1792):  29%|██▉       | 17/58 [00:08<00:06,  6.53it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:08<00:05,  7.29it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:08<00:05,  7.29it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:08<00:05,  7.29it/s]

    Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:08<00:04,  8.91it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:08<00:04,  8.91it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:08<00:04,  8.91it/s] Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:08<00:03, 11.05it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:08<00:03, 11.05it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:08<00:03, 11.05it/s]

    Compiling num tokens (num_tokens=832):  41%|████▏     | 24/58 [00:08<00:02, 12.87it/s]Compiling num tokens (num_tokens=768):  41%|████▏     | 24/58 [00:08<00:02, 12.87it/s]Compiling num tokens (num_tokens=704):  41%|████▏     | 24/58 [00:08<00:02, 12.87it/s]Compiling num tokens (num_tokens=640):  41%|████▏     | 24/58 [00:08<00:02, 12.87it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:08<00:02, 15.27it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:08<00:02, 15.27it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:08<00:02, 15.27it/s]

    Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:08<00:02, 15.27it/s]Compiling num tokens (num_tokens=480):  52%|█████▏    | 30/58 [00:08<00:01, 18.05it/s]Compiling num tokens (num_tokens=448):  52%|█████▏    | 30/58 [00:08<00:01, 18.05it/s]Compiling num tokens (num_tokens=416):  52%|█████▏    | 30/58 [00:08<00:01, 18.05it/s]Compiling num tokens (num_tokens=384):  52%|█████▏    | 30/58 [00:09<00:01, 18.05it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:09<00:01, 19.54it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:09<00:01, 19.54it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:09<00:01, 19.54it/s]

    Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:09<00:01, 19.54it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:09<00:00, 22.01it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:09<00:00, 22.01it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:09<00:00, 22.01it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:09<00:00, 22.01it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:09<00:00, 22.01it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:09<00:00, 25.20it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:09<00:00, 25.20it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:09<00:00, 25.20it/s]

    Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:09<00:00, 25.20it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:09<00:00, 25.20it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:09<00:00, 28.14it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:09<00:00, 28.14it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:09<00:00, 28.14it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:09<00:00, 28.14it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:09<00:00, 28.14it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:09<00:00, 30.09it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:09<00:00, 30.09it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:09<00:00, 30.09it/s]

    Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:09<00:00, 30.09it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:09<00:00, 30.09it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:09<00:00, 32.62it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:09<00:00, 32.62it/s]Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:09<00:00, 32.62it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:09<00:00, 32.62it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:09<00:00, 32.62it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:09<00:00, 32.62it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:09<00:00, 35.77it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:09<00:00, 35.77it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:09<00:00,  5.94it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=87.25 GB):   0%|          | 0/58 [00:00<?, ?it/s]

    Capturing num tokens (num_tokens=8192 avail_mem=87.25 GB):   2%|▏         | 1/58 [00:00<00:40,  1.41it/s]Capturing num tokens (num_tokens=7680 avail_mem=84.43 GB):   2%|▏         | 1/58 [00:00<00:40,  1.41it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=84.43 GB):   3%|▎         | 2/58 [00:01<00:37,  1.48it/s]Capturing num tokens (num_tokens=7168 avail_mem=84.43 GB):   3%|▎         | 2/58 [00:01<00:37,  1.48it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=84.43 GB):   5%|▌         | 3/58 [00:01<00:33,  1.64it/s]Capturing num tokens (num_tokens=6656 avail_mem=84.44 GB):   5%|▌         | 3/58 [00:01<00:33,  1.64it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=84.44 GB):   7%|▋         | 4/58 [00:02<00:30,  1.75it/s]Capturing num tokens (num_tokens=6144 avail_mem=84.44 GB):   7%|▋         | 4/58 [00:02<00:30,  1.75it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=84.44 GB):   9%|▊         | 5/58 [00:02<00:27,  1.91it/s]Capturing num tokens (num_tokens=5632 avail_mem=84.44 GB):   9%|▊         | 5/58 [00:02<00:27,  1.91it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=84.44 GB):  10%|█         | 6/58 [00:03<00:25,  2.07it/s]Capturing num tokens (num_tokens=5120 avail_mem=84.45 GB):  10%|█         | 6/58 [00:03<00:25,  2.07it/s]

    Capturing num tokens (num_tokens=5120 avail_mem=84.45 GB):  12%|█▏        | 7/58 [00:03<00:22,  2.23it/s]Capturing num tokens (num_tokens=4608 avail_mem=84.45 GB):  12%|█▏        | 7/58 [00:03<00:22,  2.23it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=84.45 GB):  14%|█▍        | 8/58 [00:03<00:20,  2.42it/s]Capturing num tokens (num_tokens=4096 avail_mem=84.45 GB):  14%|█▍        | 8/58 [00:03<00:20,  2.42it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=84.45 GB):  16%|█▌        | 9/58 [00:04<00:19,  2.57it/s]Capturing num tokens (num_tokens=3840 avail_mem=83.46 GB):  16%|█▌        | 9/58 [00:04<00:19,  2.57it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=83.46 GB):  17%|█▋        | 10/58 [00:04<00:20,  2.35it/s]Capturing num tokens (num_tokens=3584 avail_mem=83.46 GB):  17%|█▋        | 10/58 [00:04<00:20,  2.35it/s]

    Capturing num tokens (num_tokens=3584 avail_mem=83.46 GB):  19%|█▉        | 11/58 [00:05<00:20,  2.30it/s]Capturing num tokens (num_tokens=3328 avail_mem=83.46 GB):  19%|█▉        | 11/58 [00:05<00:20,  2.30it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=83.46 GB):  21%|██        | 12/58 [00:05<00:19,  2.32it/s]Capturing num tokens (num_tokens=3072 avail_mem=83.46 GB):  21%|██        | 12/58 [00:05<00:19,  2.32it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=83.46 GB):  22%|██▏       | 13/58 [00:06<00:19,  2.33it/s]Capturing num tokens (num_tokens=2816 avail_mem=83.46 GB):  22%|██▏       | 13/58 [00:06<00:19,  2.33it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=83.46 GB):  24%|██▍       | 14/58 [00:06<00:18,  2.42it/s]Capturing num tokens (num_tokens=2560 avail_mem=83.46 GB):  24%|██▍       | 14/58 [00:06<00:18,  2.42it/s]

    Capturing num tokens (num_tokens=2560 avail_mem=83.46 GB):  26%|██▌       | 15/58 [00:06<00:16,  2.54it/s]Capturing num tokens (num_tokens=2304 avail_mem=83.46 GB):  26%|██▌       | 15/58 [00:06<00:16,  2.54it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=83.46 GB):  28%|██▊       | 16/58 [00:07<00:15,  2.64it/s]Capturing num tokens (num_tokens=2048 avail_mem=83.45 GB):  28%|██▊       | 16/58 [00:07<00:15,  2.64it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=83.45 GB):  29%|██▉       | 17/58 [00:07<00:14,  2.80it/s]Capturing num tokens (num_tokens=1792 avail_mem=83.45 GB):  29%|██▉       | 17/58 [00:07<00:14,  2.80it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=83.45 GB):  31%|███       | 18/58 [00:07<00:13,  2.97it/s]Capturing num tokens (num_tokens=1536 avail_mem=83.45 GB):  31%|███       | 18/58 [00:07<00:13,  2.97it/s]

    Capturing num tokens (num_tokens=1536 avail_mem=83.45 GB):  33%|███▎      | 19/58 [00:08<00:11,  3.26it/s]Capturing num tokens (num_tokens=1280 avail_mem=83.45 GB):  33%|███▎      | 19/58 [00:08<00:11,  3.26it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=83.45 GB):  34%|███▍      | 20/58 [00:08<00:10,  3.48it/s]Capturing num tokens (num_tokens=1024 avail_mem=83.45 GB):  34%|███▍      | 20/58 [00:08<00:10,  3.48it/s]

    Capturing num tokens (num_tokens=1024 avail_mem=83.45 GB):  36%|███▌      | 21/58 [00:08<00:10,  3.43it/s]Capturing num tokens (num_tokens=960 avail_mem=83.45 GB):  36%|███▌      | 21/58 [00:08<00:10,  3.43it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=83.45 GB):  38%|███▊      | 22/58 [00:08<00:10,  3.55it/s]Capturing num tokens (num_tokens=896 avail_mem=83.44 GB):  38%|███▊      | 22/58 [00:08<00:10,  3.55it/s]

    Capturing num tokens (num_tokens=896 avail_mem=83.44 GB):  40%|███▉      | 23/58 [00:09<00:09,  3.77it/s]Capturing num tokens (num_tokens=832 avail_mem=83.44 GB):  40%|███▉      | 23/58 [00:09<00:09,  3.77it/s]

    Capturing num tokens (num_tokens=832 avail_mem=83.44 GB):  41%|████▏     | 24/58 [00:09<00:08,  3.96it/s]Capturing num tokens (num_tokens=768 avail_mem=83.44 GB):  41%|████▏     | 24/58 [00:09<00:08,  3.96it/s]

    Capturing num tokens (num_tokens=768 avail_mem=83.44 GB):  43%|████▎     | 25/58 [00:09<00:08,  3.91it/s]Capturing num tokens (num_tokens=704 avail_mem=83.43 GB):  43%|████▎     | 25/58 [00:09<00:08,  3.91it/s]

    Capturing num tokens (num_tokens=704 avail_mem=83.43 GB):  45%|████▍     | 26/58 [00:09<00:07,  4.12it/s]Capturing num tokens (num_tokens=640 avail_mem=83.43 GB):  45%|████▍     | 26/58 [00:09<00:07,  4.12it/s]Capturing num tokens (num_tokens=640 avail_mem=83.43 GB):  47%|████▋     | 27/58 [00:09<00:07,  4.36it/s]Capturing num tokens (num_tokens=576 avail_mem=83.43 GB):  47%|████▋     | 27/58 [00:09<00:07,  4.36it/s]

    Capturing num tokens (num_tokens=576 avail_mem=83.43 GB):  48%|████▊     | 28/58 [00:10<00:06,  4.60it/s]Capturing num tokens (num_tokens=512 avail_mem=83.42 GB):  48%|████▊     | 28/58 [00:10<00:06,  4.60it/s]Capturing num tokens (num_tokens=512 avail_mem=83.42 GB):  50%|█████     | 29/58 [00:10<00:06,  4.82it/s]Capturing num tokens (num_tokens=480 avail_mem=83.42 GB):  50%|█████     | 29/58 [00:10<00:06,  4.82it/s]

    Capturing num tokens (num_tokens=480 avail_mem=83.42 GB):  52%|█████▏    | 30/58 [00:10<00:05,  5.13it/s]Capturing num tokens (num_tokens=448 avail_mem=83.41 GB):  52%|█████▏    | 30/58 [00:10<00:05,  5.13it/s]Capturing num tokens (num_tokens=448 avail_mem=83.41 GB):  53%|█████▎    | 31/58 [00:10<00:05,  5.16it/s]Capturing num tokens (num_tokens=416 avail_mem=83.41 GB):  53%|█████▎    | 31/58 [00:10<00:05,  5.16it/s]

    Capturing num tokens (num_tokens=416 avail_mem=83.41 GB):  55%|█████▌    | 32/58 [00:10<00:04,  5.52it/s]Capturing num tokens (num_tokens=384 avail_mem=83.40 GB):  55%|█████▌    | 32/58 [00:10<00:04,  5.52it/s]Capturing num tokens (num_tokens=384 avail_mem=83.40 GB):  57%|█████▋    | 33/58 [00:10<00:04,  6.03it/s]Capturing num tokens (num_tokens=352 avail_mem=83.40 GB):  57%|█████▋    | 33/58 [00:10<00:04,  6.03it/s]

    Capturing num tokens (num_tokens=320 avail_mem=83.40 GB):  57%|█████▋    | 33/58 [00:11<00:04,  6.03it/s]Capturing num tokens (num_tokens=320 avail_mem=83.40 GB):  60%|██████    | 35/58 [00:11<00:03,  6.54it/s]Capturing num tokens (num_tokens=288 avail_mem=83.39 GB):  60%|██████    | 35/58 [00:11<00:03,  6.54it/s]

    Capturing num tokens (num_tokens=288 avail_mem=83.39 GB):  62%|██████▏   | 36/58 [00:11<00:03,  6.81it/s]Capturing num tokens (num_tokens=256 avail_mem=83.39 GB):  62%|██████▏   | 36/58 [00:11<00:03,  6.81it/s]Capturing num tokens (num_tokens=256 avail_mem=83.39 GB):  64%|██████▍   | 37/58 [00:11<00:02,  7.30it/s]Capturing num tokens (num_tokens=240 avail_mem=83.38 GB):  64%|██████▍   | 37/58 [00:11<00:02,  7.30it/s]

    Capturing num tokens (num_tokens=240 avail_mem=83.38 GB):  66%|██████▌   | 38/58 [00:11<00:02,  7.65it/s]Capturing num tokens (num_tokens=224 avail_mem=83.37 GB):  66%|██████▌   | 38/58 [00:11<00:02,  7.65it/s]Capturing num tokens (num_tokens=224 avail_mem=83.37 GB):  67%|██████▋   | 39/58 [00:11<00:02,  8.13it/s]Capturing num tokens (num_tokens=208 avail_mem=83.37 GB):  67%|██████▋   | 39/58 [00:11<00:02,  8.13it/s]

    Capturing num tokens (num_tokens=208 avail_mem=83.37 GB):  69%|██████▉   | 40/58 [00:11<00:02,  8.58it/s]Capturing num tokens (num_tokens=192 avail_mem=83.37 GB):  69%|██████▉   | 40/58 [00:11<00:02,  8.58it/s]Capturing num tokens (num_tokens=192 avail_mem=83.37 GB):  71%|███████   | 41/58 [00:11<00:01,  8.70it/s]Capturing num tokens (num_tokens=176 avail_mem=83.36 GB):  71%|███████   | 41/58 [00:11<00:01,  8.70it/s]Capturing num tokens (num_tokens=160 avail_mem=83.35 GB):  71%|███████   | 41/58 [00:11<00:01,  8.70it/s]

    Capturing num tokens (num_tokens=160 avail_mem=83.35 GB):  74%|███████▍  | 43/58 [00:12<00:01, 10.87it/s]Capturing num tokens (num_tokens=144 avail_mem=83.35 GB):  74%|███████▍  | 43/58 [00:12<00:01, 10.87it/s]Capturing num tokens (num_tokens=128 avail_mem=83.35 GB):  74%|███████▍  | 43/58 [00:12<00:01, 10.87it/s]Capturing num tokens (num_tokens=128 avail_mem=83.35 GB):  78%|███████▊  | 45/58 [00:12<00:00, 13.22it/s]Capturing num tokens (num_tokens=112 avail_mem=83.35 GB):  78%|███████▊  | 45/58 [00:12<00:00, 13.22it/s]Capturing num tokens (num_tokens=96 avail_mem=83.35 GB):  78%|███████▊  | 45/58 [00:12<00:00, 13.22it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=83.35 GB):  81%|████████  | 47/58 [00:12<00:00, 11.85it/s]Capturing num tokens (num_tokens=80 avail_mem=83.35 GB):  81%|████████  | 47/58 [00:12<00:00, 11.85it/s]Capturing num tokens (num_tokens=64 avail_mem=82.31 GB):  81%|████████  | 47/58 [00:12<00:00, 11.85it/s]

    Capturing num tokens (num_tokens=64 avail_mem=82.31 GB):  84%|████████▍ | 49/58 [00:12<00:00,  9.02it/s]Capturing num tokens (num_tokens=48 avail_mem=82.31 GB):  84%|████████▍ | 49/58 [00:12<00:00,  9.02it/s]Capturing num tokens (num_tokens=32 avail_mem=82.30 GB):  84%|████████▍ | 49/58 [00:12<00:00,  9.02it/s]

    Capturing num tokens (num_tokens=32 avail_mem=82.30 GB):  88%|████████▊ | 51/58 [00:12<00:00,  7.65it/s]Capturing num tokens (num_tokens=28 avail_mem=83.30 GB):  88%|████████▊ | 51/58 [00:12<00:00,  7.65it/s]Capturing num tokens (num_tokens=28 avail_mem=83.30 GB):  90%|████████▉ | 52/58 [00:13<00:00,  7.43it/s]Capturing num tokens (num_tokens=24 avail_mem=82.47 GB):  90%|████████▉ | 52/58 [00:13<00:00,  7.43it/s]

    Capturing num tokens (num_tokens=24 avail_mem=82.47 GB):  91%|█████████▏| 53/58 [00:13<00:00,  6.85it/s]Capturing num tokens (num_tokens=20 avail_mem=82.47 GB):  91%|█████████▏| 53/58 [00:13<00:00,  6.85it/s]Capturing num tokens (num_tokens=20 avail_mem=82.47 GB):  93%|█████████▎| 54/58 [00:13<00:00,  6.50it/s]Capturing num tokens (num_tokens=16 avail_mem=82.47 GB):  93%|█████████▎| 54/58 [00:13<00:00,  6.50it/s]

    Capturing num tokens (num_tokens=16 avail_mem=82.47 GB):  95%|█████████▍| 55/58 [00:13<00:00,  6.42it/s]Capturing num tokens (num_tokens=12 avail_mem=83.29 GB):  95%|█████████▍| 55/58 [00:13<00:00,  6.42it/s]Capturing num tokens (num_tokens=12 avail_mem=83.29 GB):  97%|█████████▋| 56/58 [00:13<00:00,  6.53it/s]Capturing num tokens (num_tokens=8 avail_mem=83.28 GB):  97%|█████████▋| 56/58 [00:13<00:00,  6.53it/s] 

    Capturing num tokens (num_tokens=8 avail_mem=83.28 GB):  98%|█████████▊| 57/58 [00:13<00:00,  6.29it/s]Capturing num tokens (num_tokens=4 avail_mem=82.51 GB):  98%|█████████▊| 57/58 [00:13<00:00,  6.29it/s]Capturing num tokens (num_tokens=4 avail_mem=82.51 GB): 100%|██████████| 58/58 [00:14<00:00,  6.02it/s]Capturing num tokens (num_tokens=4 avail_mem=82.51 GB): 100%|██████████| 58/58 [00:14<00:00,  4.09it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:116: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



```python
from transformers import AutoTokenizer

PROMPT = (
    "What is the range of the numeric output of a sigmoid node in a neural network?"
)

RESPONSE1 = "The output of a sigmoid node is bounded between -1 and 1."
RESPONSE2 = "The output of a sigmoid node is bounded between 0 and 1."

CONVS = [
    [{"role": "user", "content": PROMPT}, {"role": "assistant", "content": RESPONSE1}],
    [{"role": "user", "content": PROMPT}, {"role": "assistant", "content": RESPONSE2}],
]

tokenizer = AutoTokenizer.from_pretrained("Skywork/Skywork-Reward-Llama-3.1-8B-v0.2")
prompts = tokenizer.apply_chat_template(CONVS, tokenize=False, return_dict=False)

url = f"http://localhost:{port}/classify"
data = {"model": "Skywork/Skywork-Reward-Llama-3.1-8B-v0.2", "text": prompts}

responses = requests.post(url, json=data).json()
for response in responses:
    print_highlight(f"reward: {response['embedding'][0]}")
```


<strong style='color: #00008B;'>reward: -24.25</strong>



<strong style='color: #00008B;'>reward: 1.0390625</strong>



```python
terminate_process(reward_process)
```

## Capture expert selection distribution in MoE models

SGLang Runtime supports recording the number of times an expert is selected in a MoE model run for each expert in the model. This is useful when analyzing the throughput of the model and plan for optimization.

*Note: We only print out the first 10 lines of the csv below for better readability. Please adjust accordingly if you want to analyze the results more deeply.*


```python
expert_record_server_process, port = launch_server_cmd(
    "python3 -m sglang.launch_server --model-path Qwen/Qwen1.5-MoE-A2.7B --host 0.0.0.0 --expert-distribution-recorder-mode stat --log-level warning"
)

wait_for_server(f"http://localhost:{port}", process=expert_record_server_process)
```

    [2026-03-23 00:13:32] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:13:32] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:13:32] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    [2026-03-23 00:13:34] WARNING model_config.py:1098: Transformers version 5.3.0 is used for model type qwen2_moe. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.
    [2026-03-23 00:13:34] INFO server_args.py:2239: Attention backend not specified. Use fa3 backend by default.
    [2026-03-23 00:13:34] INFO server_args.py:3522: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-23 00:13:35] Transformers version 5.3.0 is used for model type qwen2_moe. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-23 00:13:40] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:13:40] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:13:40] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-23 00:13:40] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:13:40] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:13:40] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-23 00:13:42] Transformers version 5.3.0 is used for model type qwen2_moe. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-23 00:13:43] Transformers version 5.3.0 is used for model type qwen2_moe. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Loading safetensors checkpoint shards:   0% Completed | 0/8 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards:  12% Completed | 1/8 [00:00<00:04,  1.53it/s]

    Loading safetensors checkpoint shards:  25% Completed | 2/8 [00:01<00:04,  1.26it/s]

    Loading safetensors checkpoint shards:  38% Completed | 3/8 [00:02<00:05,  1.02s/it]

    Loading safetensors checkpoint shards:  50% Completed | 4/8 [00:04<00:04,  1.19s/it]

    Loading safetensors checkpoint shards:  62% Completed | 5/8 [00:05<00:03,  1.03s/it]

    Loading safetensors checkpoint shards:  75% Completed | 6/8 [00:05<00:01,  1.33it/s]

    Loading safetensors checkpoint shards:  88% Completed | 7/8 [00:05<00:00,  1.45it/s]

    Loading safetensors checkpoint shards: 100% Completed | 8/8 [00:06<00:00,  1.43it/s]Loading safetensors checkpoint shards: 100% Completed | 8/8 [00:06<00:00,  1.23it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:116: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)


    [2026-03-23 00:13:56] Using default MoE kernel config. Performance might be sub-optimal! Config file not found at /actions-runner/_work/sglang/sglang/python/sglang/srt/layers/moe/fused_moe_triton/configs/triton_3_5_1/E=60,N=1408,device_name=NVIDIA_H200.json, you can create them with https://github.com/sgl-project/sglang/tree/main/benchmark/kernels/fused_moe_triton
    [2026-03-23 00:13:56] Using MoE kernel config with down_moe=False. Performance might be sub-optimal! Config file not found at /actions-runner/_work/sglang/sglang/python/sglang/srt/layers/moe/fused_moe_triton/configs/triton_3_5_1/E=60,N=1408,device_name=NVIDIA_H200_down.json, you can create them with https://github.com/sgl-project/sglang/tree/main/benchmark/kernels/fused_moe_triton



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



```python
response = requests.post(f"http://localhost:{port}/start_expert_distribution_record")
print_highlight(response)

url = f"http://localhost:{port}/generate"
data = {"text": "What is the capital of France?"}

response = requests.post(url, json=data)
print_highlight(response.json())

response = requests.post(f"http://localhost:{port}/stop_expert_distribution_record")
print_highlight(response)

response = requests.post(f"http://localhost:{port}/dump_expert_distribution_record")
print_highlight(response)
```


<strong style='color: #00008B;'><Response [200]></strong>



<strong style='color: #00008B;'>{'text': ' - london\nCarson initiated contact with their longtime rival.  Given the question "What will Carson want to do next?", is "lose" a valid answer?\nAnswer:\nStep 1: Understand the context of the question.\nThe question asks about Carson\'s next action after initiating contact with their longtime rival. We must determine if "lose" is a valid answer to this question.\n\nStep 2: Consider the goal or objective of Carson\'s action.\nCarson\'s goal would likely be to either make progress or improve their relationships with their rivals, depending on the outcome of initiating contact.\n\nStep 3: Assess the validity of the "', 'output_ids': [481, 62046, 198, 8852, 930, 32130, 3645, 448, 862, 35404, 20762, 13, 220, 16246, 279, 3405, 330, 3838, 686, 40176, 1366, 311, 653, 1790, 31011, 374, 330, 1469, 1, 264, 2697, 4226, 5267, 16141, 510, 8304, 220, 16, 25, 70894, 279, 2266, 315, 279, 3405, 624, 785, 3405, 17064, 911, 40176, 594, 1790, 1917, 1283, 78416, 3645, 448, 862, 35404, 20762, 13, 1205, 1969, 8253, 421, 330, 1469, 1, 374, 264, 2697, 4226, 311, 419, 3405, 382, 8304, 220, 17, 25, 21144, 279, 5795, 476, 16538, 315, 40176, 594, 1917, 624, 8852, 930, 594, 5795, 1035, 4363, 387, 311, 2987, 1281, 5098, 476, 7269, 862, 11871, 448, 862, 34838, 11, 11649, 389, 279, 15274, 315, 78416, 3645, 382, 8304, 220, 18, 25, 81835, 279, 31839, 315, 279, 330], 'meta_info': {'id': '954c141c51b841479d0e091b2ac6dad1', 'finish_reason': {'type': 'length', 'length': 128}, 'prompt_tokens': 7, 'weight_version': 'default', 'total_retractions': 0, 'completion_tokens': 128, 'cached_tokens': 0, 'cached_tokens_details': None, 'dp_rank': None, 'e2e_latency': 3.7204117560759187, 'response_sent_to_client_ts': 1774224845.038885}}</strong>



<strong style='color: #00008B;'><Response [200]></strong>



<strong style='color: #00008B;'><Response [200]></strong>



```python
terminate_process(expert_record_server_process)
```

## Tokenize/Detokenize Example (Round Trip)

This example demonstrates how to use the /tokenize and /detokenize endpoints together. We first tokenize a string, then detokenize the resulting IDs to reconstruct the original text. This workflow is useful when you need to handle tokenization externally but still leverage the server for detokenization.


```python
tokenizer_free_server_process, port = launch_server_cmd("""
python3 -m sglang.launch_server --model-path qwen/qwen2.5-0.5b-instruct
""")

wait_for_server(f"http://localhost:{port}", process=tokenizer_free_server_process)
```

    [2026-03-23 00:14:09] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:14:09] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:14:09] INFO utils.py:164: NumExpr defaulting to 16 threads.
    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    [2026-03-23 00:14:12] WARNING model_config.py:1098: Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.
    [2026-03-23 00:14:12] INFO server_args.py:2239: Attention backend not specified. Use fa3 backend by default.


    [2026-03-23 00:14:12] INFO server_args.py:3522: Set soft_watchdog_timeout since in CI


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:175: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [2026-03-23 00:14:13] server_args=ServerArgs(model_path='qwen/qwen2.5-0.5b-instruct', tokenizer_path='qwen/qwen2.5-0.5b-instruct', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=38630, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.907, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=117166534, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='info', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='qwen/qwen2.5-0.5b-instruct', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser=None, tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_match_window_size=1, speculative_ngram_max_match_window_size=12, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_branch_length=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', disable_hicache_numa_detect=False, hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, hierarchical_sparse_attention_extra_config=None, enable_lmcache=False, kt_weight_path=None, kt_method='AMXINT4', kt_cpuinfer=None, kt_threadpool_count=2, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, mm_max_concurrent_calls=32, mm_per_request_timeout=10.0, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)
    [2026-03-23 00:14:13] Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-23 00:14:14] Watchdog TokenizerManager initialized.
    [2026-03-23 00:14:14] Using default HuggingFace chat template with detected content format: string


    [2026-03-23 00:14:17] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:14:17] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:14:17] INFO utils.py:164: NumExpr defaulting to 16 threads.
    [2026-03-23 00:14:17] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.
    [2026-03-23 00:14:17] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.
    [2026-03-23 00:14:17] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-23 00:14:19] Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-23 00:14:21] Watchdog DetokenizerManager initialized.


    [2026-03-23 00:14:21] Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.
    [2026-03-23 00:14:21] Init torch distributed begin.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [2026-03-23 00:14:21] Init torch distributed ends. elapsed=0.46 s, mem usage=0.09 GB


    [2026-03-23 00:14:23] Load weight begin. avail mem=103.39 GB
    [2026-03-23 00:14:24] Found local HF snapshot for qwen/qwen2.5-0.5b-instruct at /root/.cache/huggingface/hub/models--qwen--qwen2.5-0.5b-instruct/snapshots/7ae557604adf67be50417f59c2c2f167def9a775; skipping download.


    [2026-03-23 00:14:24] No model.safetensors.index.json found in remote.
    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.72it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.72it/s]
    [2026-03-23 00:14:24] Load weight end. elapsed=0.78 s, type=Qwen2ForCausalLM, avail mem=102.51 GB, mem usage=0.88 GB.
    [2026-03-23 00:14:24] Using KV cache dtype: torch.bfloat16
    [2026-03-23 00:14:24] KV Cache is allocated. #tokens: 20480, K size: 0.12 GB, V size: 0.12 GB
    [2026-03-23 00:14:24] Memory pool end. avail mem=102.21 GB


    [2026-03-23 00:14:24] Capture piecewise CUDA graph begin. avail mem=102.51 GB
    [2026-03-23 00:14:24] Capture cuda graph num tokens [4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    [2026-03-23 00:14:26] Compiling a graph for dynamic shape takes 0.21 s


    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:41,  2.84s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:41,  2.84s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:41,  2.84s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:41,  2.84s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:41,  2.84s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:23,  2.23it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:23,  2.23it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:23,  2.23it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:23,  2.23it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:03<00:23,  2.23it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:03<00:07,  5.88it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:03<00:07,  5.88it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:03<00:07,  5.88it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:03<00:07,  5.88it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:03<00:07,  5.88it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:03<00:07,  5.88it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:03<00:07,  5.88it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:03<00:07,  5.88it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:03<00:07,  5.88it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:03<00:03, 11.95it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:03<00:03, 11.95it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:03<00:03, 11.95it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:03<00:03, 11.95it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:03<00:03, 11.95it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:03<00:03, 11.95it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:03<00:03, 11.95it/s]Compiling num tokens (num_tokens=704):  33%|███▎      | 19/58 [00:03<00:03, 11.95it/s]Compiling num tokens (num_tokens=704):  45%|████▍     | 26/58 [00:03<00:01, 17.80it/s]Compiling num tokens (num_tokens=640):  45%|████▍     | 26/58 [00:03<00:01, 17.80it/s]Compiling num tokens (num_tokens=576):  45%|████▍     | 26/58 [00:03<00:01, 17.80it/s]Compiling num tokens (num_tokens=512):  45%|████▍     | 26/58 [00:03<00:01, 17.80it/s]Compiling num tokens (num_tokens=480):  45%|████▍     | 26/58 [00:03<00:01, 17.80it/s]Compiling num tokens (num_tokens=448):  45%|████▍     | 26/58 [00:03<00:01, 17.80it/s]Compiling num tokens (num_tokens=416):  45%|████▍     | 26/58 [00:03<00:01, 17.80it/s]Compiling num tokens (num_tokens=384):  45%|████▍     | 26/58 [00:03<00:01, 17.80it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:01, 24.26it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:01, 24.26it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:01, 24.26it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:01, 24.26it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:01, 24.26it/s]

    Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:01, 24.26it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:01, 24.26it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 29.25it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 29.25it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 29.25it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 29.25it/s]Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 29.25it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 29.25it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 29.25it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 33.69it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 33.69it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 33.69it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 33.69it/s]

    Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 33.69it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 33.69it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 33.69it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 37.93it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 37.93it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 37.93it/s]Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 37.93it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 37.93it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 37.93it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 37.93it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 37.93it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.18it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.72 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.70 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.70 GB):   3%|▎         | 2/58 [00:00<00:02, 18.87it/s]Capturing num tokens (num_tokens=7168 avail_mem=119.69 GB):   3%|▎         | 2/58 [00:00<00:02, 18.87it/s]Capturing num tokens (num_tokens=6656 avail_mem=119.69 GB):   3%|▎         | 2/58 [00:00<00:02, 18.87it/s]Capturing num tokens (num_tokens=6144 avail_mem=119.69 GB):   3%|▎         | 2/58 [00:00<00:02, 18.87it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=119.69 GB):   9%|▊         | 5/58 [00:00<00:02, 22.03it/s]Capturing num tokens (num_tokens=5632 avail_mem=119.69 GB):   9%|▊         | 5/58 [00:00<00:02, 22.03it/s]Capturing num tokens (num_tokens=5120 avail_mem=119.69 GB):   9%|▊         | 5/58 [00:00<00:02, 22.03it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.68 GB):   9%|▊         | 5/58 [00:00<00:02, 22.03it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.68 GB):   9%|▊         | 5/58 [00:00<00:02, 22.03it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.68 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.70it/s]Capturing num tokens (num_tokens=3840 avail_mem=119.68 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.70it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.67 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.70it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.67 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.70it/s]

    Capturing num tokens (num_tokens=3072 avail_mem=119.67 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.70it/s]Capturing num tokens (num_tokens=2816 avail_mem=119.67 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.70it/s]Capturing num tokens (num_tokens=2816 avail_mem=119.67 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.29it/s]Capturing num tokens (num_tokens=2560 avail_mem=119.66 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.29it/s]Capturing num tokens (num_tokens=2304 avail_mem=119.66 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.29it/s]Capturing num tokens (num_tokens=2048 avail_mem=119.66 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.29it/s]Capturing num tokens (num_tokens=1792 avail_mem=119.65 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.29it/s]Capturing num tokens (num_tokens=1536 avail_mem=119.65 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.29it/s]Capturing num tokens (num_tokens=1536 avail_mem=119.65 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.52it/s]Capturing num tokens (num_tokens=1280 avail_mem=119.64 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.52it/s]Capturing num tokens (num_tokens=1024 avail_mem=119.62 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.52it/s]

    Capturing num tokens (num_tokens=960 avail_mem=119.64 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.52it/s] Capturing num tokens (num_tokens=896 avail_mem=119.64 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.52it/s]Capturing num tokens (num_tokens=832 avail_mem=119.63 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.52it/s]Capturing num tokens (num_tokens=832 avail_mem=119.63 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.37it/s]Capturing num tokens (num_tokens=768 avail_mem=119.63 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.37it/s]Capturing num tokens (num_tokens=704 avail_mem=119.63 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.37it/s]Capturing num tokens (num_tokens=640 avail_mem=119.62 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.37it/s]Capturing num tokens (num_tokens=576 avail_mem=119.62 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.37it/s]Capturing num tokens (num_tokens=512 avail_mem=119.61 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.37it/s]Capturing num tokens (num_tokens=512 avail_mem=119.61 GB):  50%|█████     | 29/58 [00:00<00:00, 41.28it/s]Capturing num tokens (num_tokens=480 avail_mem=119.62 GB):  50%|█████     | 29/58 [00:00<00:00, 41.28it/s]Capturing num tokens (num_tokens=448 avail_mem=119.62 GB):  50%|█████     | 29/58 [00:00<00:00, 41.28it/s]

    Capturing num tokens (num_tokens=416 avail_mem=119.62 GB):  50%|█████     | 29/58 [00:00<00:00, 41.28it/s]Capturing num tokens (num_tokens=384 avail_mem=119.62 GB):  50%|█████     | 29/58 [00:00<00:00, 41.28it/s]Capturing num tokens (num_tokens=352 avail_mem=119.61 GB):  50%|█████     | 29/58 [00:00<00:00, 41.28it/s]Capturing num tokens (num_tokens=352 avail_mem=119.61 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.67it/s]Capturing num tokens (num_tokens=320 avail_mem=119.61 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.67it/s]Capturing num tokens (num_tokens=288 avail_mem=119.61 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.67it/s]Capturing num tokens (num_tokens=256 avail_mem=119.60 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.67it/s]Capturing num tokens (num_tokens=240 avail_mem=119.60 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.67it/s]Capturing num tokens (num_tokens=224 avail_mem=119.60 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.67it/s]Capturing num tokens (num_tokens=224 avail_mem=119.60 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.43it/s]Capturing num tokens (num_tokens=208 avail_mem=119.59 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.43it/s]Capturing num tokens (num_tokens=192 avail_mem=119.59 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.43it/s]

    Capturing num tokens (num_tokens=176 avail_mem=119.59 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.43it/s]Capturing num tokens (num_tokens=160 avail_mem=119.59 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.43it/s]Capturing num tokens (num_tokens=144 avail_mem=119.58 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.43it/s]Capturing num tokens (num_tokens=144 avail_mem=119.58 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.14it/s]Capturing num tokens (num_tokens=128 avail_mem=119.58 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.14it/s]Capturing num tokens (num_tokens=112 avail_mem=119.58 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.14it/s]Capturing num tokens (num_tokens=96 avail_mem=119.52 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.14it/s] Capturing num tokens (num_tokens=80 avail_mem=119.52 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.14it/s]Capturing num tokens (num_tokens=64 avail_mem=119.51 GB):  76%|███████▌  | 44/58 [00:01<00:00, 44.14it/s]Capturing num tokens (num_tokens=64 avail_mem=119.51 GB):  84%|████████▍ | 49/58 [00:01<00:00, 43.71it/s]Capturing num tokens (num_tokens=48 avail_mem=119.51 GB):  84%|████████▍ | 49/58 [00:01<00:00, 43.71it/s]

    Capturing num tokens (num_tokens=32 avail_mem=119.51 GB):  84%|████████▍ | 49/58 [00:01<00:00, 43.71it/s]Capturing num tokens (num_tokens=28 avail_mem=119.50 GB):  84%|████████▍ | 49/58 [00:01<00:00, 43.71it/s]Capturing num tokens (num_tokens=24 avail_mem=119.50 GB):  84%|████████▍ | 49/58 [00:01<00:00, 43.71it/s]Capturing num tokens (num_tokens=20 avail_mem=119.47 GB):  84%|████████▍ | 49/58 [00:01<00:00, 43.71it/s]Capturing num tokens (num_tokens=20 avail_mem=119.47 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.72it/s]Capturing num tokens (num_tokens=16 avail_mem=119.44 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.72it/s]Capturing num tokens (num_tokens=12 avail_mem=119.41 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.72it/s]Capturing num tokens (num_tokens=8 avail_mem=119.39 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.72it/s] Capturing num tokens (num_tokens=4 avail_mem=119.39 GB):  93%|█████████▎| 54/58 [00:01<00:00, 43.72it/s]Capturing num tokens (num_tokens=4 avail_mem=119.39 GB): 100%|██████████| 58/58 [00:01<00:00, 39.45it/s]
    [2026-03-23 00:14:30] Capture piecewise CUDA graph end. Time elapsed: 5.90 s. mem usage=-16.87 GB. avail mem=119.38 GB.


    [2026-03-23 00:14:32] max_total_num_tokens=20480, chunked_prefill_size=8192, max_prefill_tokens=16384, max_running_requests=128, context_len=32768, available_gpu_mem=118.88 GB


    [2026-03-23 00:14:32] INFO:     Started server process [1111671]
    [2026-03-23 00:14:32] INFO:     Waiting for application startup.
    [2026-03-23 00:14:32] Using default chat sampling params from model generation config: {'repetition_penalty': 1.1, 'temperature': 0.7, 'top_k': 20, 'top_p': 0.8}
    [2026-03-23 00:14:32] INFO:     Application startup complete.
    [2026-03-23 00:14:32] INFO:     Uvicorn running on http://127.0.0.1:38630 (Press CTRL+C to quit)


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:116: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)
    [2026-03-23 00:14:33] INFO:     127.0.0.1:44740 - "GET /v1/models HTTP/1.1" 200 OK
    [2026-03-23 00:14:33] INFO:     127.0.0.1:44754 - "GET /model_info HTTP/1.1" 200 OK


    [2026-03-23 00:14:34] Prefill batch, #new-seq: 1, #new-token: 6, #cached-token: 0, token usage: 0.00, #running-req: 0, #queue-req: 0, cuda graph: True, input throughput (token/s): 0.00
    [2026-03-23 00:14:34] INFO:     127.0.0.1:44766 - "POST /generate HTTP/1.1" 200 OK
    [2026-03-23 00:14:34] The server is fired up and ready to roll!



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



```python
import requests
from sglang.utils import print_highlight

base_url = f"http://localhost:{port}"
tokenize_url = f"{base_url}/tokenize"
detokenize_url = f"{base_url}/detokenize"

model_name = "qwen/qwen2.5-0.5b-instruct"
input_text = "SGLang provides efficient tokenization endpoints."
print_highlight(f"Original Input Text:\n'{input_text}'")

# --- tokenize the input text ---
tokenize_payload = {
    "model": model_name,
    "prompt": input_text,
    "add_special_tokens": False,
}
try:
    tokenize_response = requests.post(tokenize_url, json=tokenize_payload)
    tokenize_response.raise_for_status()
    tokenization_result = tokenize_response.json()
    token_ids = tokenization_result.get("tokens")

    if not token_ids:
        raise ValueError("Tokenization returned empty tokens.")

    print_highlight(f"\nTokenized Output (IDs):\n{token_ids}")
    print_highlight(f"Token Count: {tokenization_result.get('count')}")
    print_highlight(f"Max Model Length: {tokenization_result.get('max_model_len')}")

    # --- detokenize the obtained token IDs ---
    detokenize_payload = {
        "model": model_name,
        "tokens": token_ids,
        "skip_special_tokens": True,
    }

    detokenize_response = requests.post(detokenize_url, json=detokenize_payload)
    detokenize_response.raise_for_status()
    detokenization_result = detokenize_response.json()
    reconstructed_text = detokenization_result.get("text")

    print_highlight(f"\nDetokenized Output (Text):\n'{reconstructed_text}'")

    if input_text == reconstructed_text:
        print_highlight(
            "\nRound Trip Successful: Original and reconstructed text match."
        )
    else:
        print_highlight(
            "\nRound Trip Mismatch: Original and reconstructed text differ."
        )

except requests.exceptions.RequestException as e:
    print_highlight(f"\nHTTP Request Error: {e}")
except Exception as e:
    print_highlight(f"\nAn error occurred: {e}")
```


<strong style='color: #00008B;'>Original Input Text:<br>'SGLang provides efficient tokenization endpoints.'</strong>


    [2026-03-23 00:14:38] INFO:     127.0.0.1:44770 - "POST /tokenize HTTP/1.1" 200 OK



<strong style='color: #00008B;'><br>Tokenized Output (IDs):<br>[50, 3825, 524, 5707, 11050, 3950, 2022, 36342, 13]</strong>



<strong style='color: #00008B;'>Token Count: 9</strong>



<strong style='color: #00008B;'>Max Model Length: 131072</strong>


    [2026-03-23 00:14:38] INFO:     127.0.0.1:44784 - "POST /detokenize HTTP/1.1" 200 OK



<strong style='color: #00008B;'><br>Detokenized Output (Text):<br>'SGLang provides efficient tokenization endpoints.'</strong>



<strong style='color: #00008B;'><br>Round Trip Successful: Original and reconstructed text match.</strong>



```python
terminate_process(tokenizer_free_server_process)
```
