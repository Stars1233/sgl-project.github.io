# LoRA Serving

SGLang enables the use of [LoRA adapters](https://arxiv.org/abs/2106.09685) with a base model. By incorporating techniques from [S-LoRA](https://arxiv.org/pdf/2311.03285) and [Punica](https://arxiv.org/pdf/2310.18547), SGLang can efficiently support multiple LoRA adapters for different sequences within a single batch of inputs.

## Arguments for LoRA Serving

The following server arguments are relevant for multi-LoRA serving:

* `enable_lora`: Enable LoRA support for the model. This argument is automatically set to True if `--lora-paths` is provided for backward compatibility.

* `enable_lora_overlap_loading`: Enable asynchronous LoRA weight loading in order to overlap H2D transfers with GPU compute. This should be enabled if you find that your LoRA workloads are bottlenecked by adapter weight loading, for example when frequently loading large LoRA adapters.

* `lora_paths`: The list of LoRA adapters to load. Each adapter must be specified in one of the following formats: <PATH> | <NAME>=<PATH> | JSON with schema {"lora_name":str,"lora_path":str,"pinned":bool}.

* `max_loras_per_batch`: Maximum number of adaptors used by each batch. This argument can affect the amount of GPU memory reserved for multi-LoRA serving, so it should be set to a smaller value when memory is scarce. Defaults to be 8.

* `max_loaded_loras`: If specified, it limits the maximum number of LoRA adapters loaded in CPU memory at a time. The value must be greater than or equal to `max-loras-per-batch`.

* `lora_eviction_policy`: LoRA adapter eviction policy when GPU memory pool is full. `lru`: Least Recently Used (default, better cache efficiency). `fifo`: First-In-First-Out.

* `lora_backend`: The backend of running GEMM kernels for Lora modules. Currently we support Triton LoRA backend (`triton`) and Chunked SGMV backend (`csgmv`). In the future, faster backend built upon Cutlass or Cuda kernels will be added.

* `max_lora_rank`: The maximum LoRA rank that should be supported. If not specified, it will be automatically inferred from the adapters provided in `--lora-paths`. This argument is needed when you expect to dynamically load adapters of larger LoRA rank after server startup.

* `lora_target_modules`: The union set of all target modules where LoRA should be applied (e.g., `q_proj`, `k_proj`, `gate_proj`). If not specified, it will be automatically inferred from the adapters provided in `--lora-paths`. This argument is needed when you expect to dynamically load adapters of different target modules after server startup. You can also set it to `all` to enable LoRA for all supported modules. However, enabling LoRA on additional modules introduces a minor performance overhead. If your application is performance-sensitive, we recommend only specifying the modules for which you plan to load adapters.

* `--max-lora-chunk-size`: Maximum chunk size for the ChunkedSGMV LoRA backend. Only used when --lora-backend is 'csgmv'. Choosing a larger value might improve performance. Please tune this value based on your hardware and workload as needed. Defaults to 16.

* `tp_size`: LoRA serving along with Tensor Parallelism is supported by SGLang. `tp_size` controls the number of GPUs for tensor parallelism. More details on the tensor sharding strategy can be found in [S-Lora](https://arxiv.org/pdf/2311.03285) paper.

From client side, the user needs to provide a list of strings as input batch, and a list of adaptor names that each input sequence corresponds to.

## Usage

### Serving Single Adaptor

**Note:** SGLang supports LoRA adapters through two APIs:

1. **OpenAI-Compatible API** (`/v1/chat/completions`, `/v1/completions`): Use the `model:adapter-name` syntax. See [OpenAI API with LoRA](../basic_usage/openai_api_completions.ipynb#Using-LoRA-Adapters) for examples.

2. **Native API** (`/generate`): Pass `lora_path` in the request body (shown below).


```python
import json
import requests

from sglang.test.doc_patch import launch_server_cmd
from sglang.utils import wait_for_server, terminate_process
```


```python
server_process, port = launch_server_cmd(
    # Here we set max-loras-per-batch to 2: one slot for adaptor and another one for base model
    """
python3 -m sglang.launch_server --model-path meta-llama/Meta-Llama-3.1-8B-Instruct \
    --enable-lora \
    --lora-paths lora0=algoprog/fact-generation-llama-3.1-8b-instruct-lora \
    --max-loras-per-batch 2 \
    --log-level warning \
"""
)

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


    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Multi-thread loading shards:  25% Completed | 1/4 [00:00<00:02,  1.30it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:01<00:01,  1.28it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:01<00:00,  1.86it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.64it/s]Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.58it/s]


    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 12965.39it/s]
    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 84733.41it/s]
    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00, 168.22it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)


    2026-04-04 04:59:32,821 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 04:59:32] Unexpected error during package walk: cutlass.cute.experimental



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



```python
url = f"http://127.0.0.1:{port}"
json_data = {
    "text": [
        "List 3 countries and their capitals.",
        "List 3 countries and their capitals.",
    ],
    "sampling_params": {"max_new_tokens": 32, "temperature": 0},
    # The first input uses lora0, and the second input uses the base model
    "lora_path": ["lora0", None],
}
response = requests.post(
    url + "/generate",
    json=json_data,
)
print(f"Output 0: {response.json()[0]['text']}")
print(f"Output 1: {response.json()[1]['text']}")
```

    Output 0:  Each country and capital should be on a new line. 
    France, Paris
    Japan, Tokyo
    Brazil, Brasília
    List 3 countries and their capitals
    Output 1:  1. 2. 3.
    1.  United States - Washington D.C. 2.  Japan - Tokyo 3.  Australia -



```python
terminate_process(server_process)
```

### Serving Multiple Adaptors


```python
server_process, port = launch_server_cmd("""
python3 -m sglang.launch_server --model-path meta-llama/Meta-Llama-3.1-8B-Instruct \
    --enable-lora \
    --lora-paths lora0=algoprog/fact-generation-llama-3.1-8b-instruct-lora \
    lora1=Nutanix/Meta-Llama-3.1-8B-Instruct_SFT_lora_4_alpha_16_humaneval_raw_json \
    --max-loras-per-batch 2 \
    --log-level warning \
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


    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Multi-thread loading shards:  25% Completed | 1/4 [00:00<00:02,  1.30it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:01<00:01,  1.22it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:01<00:00,  1.74it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.58it/s]Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.52it/s]


    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 20360.70it/s]
    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 11932.59it/s]
    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00, 165.65it/s]


    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 83055.52it/s]
    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 93727.46it/s]
    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00, 270.46it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)


    2026-04-04 05:00:06,194 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 05:00:06] Unexpected error during package walk: cutlass.cute.experimental



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



```python
url = f"http://127.0.0.1:{port}"
json_data = {
    "text": [
        "List 3 countries and their capitals.",
        "List 3 countries and their capitals.",
    ],
    "sampling_params": {"max_new_tokens": 32, "temperature": 0},
    # The first input uses lora0, and the second input uses lora1
    "lora_path": ["lora0", "lora1"],
}
response = requests.post(
    url + "/generate",
    json=json_data,
)
print(f"Output 0: {response.json()[0]['text']}")
print(f"Output 1: {response.json()[1]['text']}")
```

    Output 0:  Each country and capital should be on a new line. 
    France, Paris
    Japan, Tokyo
    Brazil, Brasília
    List 3 countries and their capitals
    Output 1:  1. 2. 3.
    1.  United States  Washington D.C. 2.  Japan  Tokyo 3.  Australia 



```python
terminate_process(server_process)
```

### Dynamic LoRA loading

Instead of specifying all adapters during server startup via `--lora-paths`. You can also load & unload LoRA adapters dynamically via the `/load_lora_adapter` and `/unload_lora_adapter` API.

When using dynamic LoRA loading, it's recommended to explicitly specify both `--max-lora-rank` and `--lora-target-modules` at startup. For backward compatibility, SGLang will infer these values from `--lora-paths` if they are not explicitly provided. However, in that case, you would have to ensure that all dynamically loaded adapters share the same shape (rank and target modules) as those in the initial `--lora-paths` or are strictly "smaller".


```python
lora0 = "Nutanix/Meta-Llama-3.1-8B-Instruct_SFT_lora_4_alpha_16_humaneval_raw_json"  # rank - 4, target modules - q_proj, k_proj, v_proj, o_proj, gate_proj
lora1 = "algoprog/fact-generation-llama-3.1-8b-instruct-lora"  # rank - 64, target modules - q_proj, k_proj, v_proj, o_proj, gate_proj, up_proj, down_proj
lora0_new = "philschmid/code-llama-3-1-8b-text-to-sql-lora"  # rank - 256, target modules - q_proj, k_proj, v_proj, o_proj, gate_proj, up_proj, down_proj


# The `--target-lora-modules` param below is technically not needed, as the server will infer it from lora0 which already has all the target modules specified.
# We are adding it here just to demonstrate usage.
server_process, port = launch_server_cmd("""
    python3 -m sglang.launch_server --model-path meta-llama/Meta-Llama-3.1-8B-Instruct \
    --enable-lora \
    --cuda-graph-max-bs 2 \
    --max-loras-per-batch 2 \
    --max-lora-rank 256
    --lora-target-modules all
    --log-level warning
    """)

url = f"http://127.0.0.1:{port}"
wait_for_server(url, process=server_process)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Multi-thread loading shards:  25% Completed | 1/4 [00:00<00:02,  1.32it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:01<00:01,  1.25it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:01<00:00,  1.76it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.60it/s]Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.54it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)


    2026-04-04 05:00:38,203 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 05:00:38] Unexpected error during package walk: cutlass.cute.experimental



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


Load adapter lora0


```python
response = requests.post(
    url + "/load_lora_adapter",
    json={
        "lora_name": "lora0",
        "lora_path": lora0,
    },
)

if response.status_code == 200:
    print("LoRA adapter loaded successfully.", response.json())
else:
    print("Failed to load LoRA adapter.", response.json())
```

    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 69042.04it/s]
    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 77672.30it/s]
    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00, 344.56it/s]
    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)
    LoRA adapter loaded successfully. {'rid': None, 'http_worker_ipc': None, 'success': True, 'error_message': '', 'loaded_adapters': {'lora0': 'Nutanix/Meta-Llama-3.1-8B-Instruct_SFT_lora_4_alpha_16_humaneval_raw_json'}}


Load adapter lora1:


```python
response = requests.post(
    url + "/load_lora_adapter",
    json={
        "lora_name": "lora1",
        "lora_path": lora1,
    },
)

if response.status_code == 200:
    print("LoRA adapter loaded successfully.", response.json())
else:
    print("Failed to load LoRA adapter.", response.json())
```

    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 9939.11it/s]


    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 62836.01it/s]
    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00, 116.29it/s]


    LoRA adapter loaded successfully. {'rid': None, 'http_worker_ipc': None, 'success': True, 'error_message': '', 'loaded_adapters': {'lora0': 'Nutanix/Meta-Llama-3.1-8B-Instruct_SFT_lora_4_alpha_16_humaneval_raw_json', 'lora1': 'algoprog/fact-generation-llama-3.1-8b-instruct-lora'}}


Check inference output:


```python
url = f"http://127.0.0.1:{port}"
json_data = {
    "text": [
        "List 3 countries and their capitals.",
        "List 3 countries and their capitals.",
    ],
    "sampling_params": {"max_new_tokens": 32, "temperature": 0},
    # The first input uses lora0, and the second input uses lora1
    "lora_path": ["lora0", "lora1"],
}
response = requests.post(
    url + "/generate",
    json=json_data,
)
print(f"Output from lora0: \n{response.json()[0]['text']}\n")
print(f"Output from lora1 (updated): \n{response.json()[1]['text']}\n")
```

    Output from lora0: 
     1. 2. 3.
    1.  United States  Washington D.C. 2.  Japan  Tokyo 3.  Australia 
    
    Output from lora1 (updated): 
     Each country and capital should be on a new line. 
    France, Paris
    Japan, Tokyo
    Brazil, Brasília
    List 3 countries and their capitals
    


Unload lora0 and replace it with a different adapter:


```python
response = requests.post(
    url + "/unload_lora_adapter",
    json={
        "lora_name": "lora0",
    },
)

response = requests.post(
    url + "/load_lora_adapter",
    json={
        "lora_name": "lora0",
        "lora_path": lora0_new,
    },
)

if response.status_code == 200:
    print("LoRA adapter loaded successfully.", response.json())
else:
    print("Failed to load LoRA adapter.", response.json())
```

    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 80659.69it/s]
    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 23797.47it/s]
    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00, 73.81it/s]


    LoRA adapter loaded successfully. {'rid': None, 'http_worker_ipc': None, 'success': True, 'error_message': '', 'loaded_adapters': {'lora1': 'algoprog/fact-generation-llama-3.1-8b-instruct-lora', 'lora0': 'philschmid/code-llama-3-1-8b-text-to-sql-lora'}}


Check output again:


```python
url = f"http://127.0.0.1:{port}"
json_data = {
    "text": [
        "List 3 countries and their capitals.",
        "List 3 countries and their capitals.",
    ],
    "sampling_params": {"max_new_tokens": 32, "temperature": 0},
    # The first input uses lora0, and the second input uses lora1
    "lora_path": ["lora0", "lora1"],
}
response = requests.post(
    url + "/generate",
    json=json_data,
)
print(f"Output from lora0: \n{response.json()[0]['text']}\n")
print(f"Output from lora1 (updated): \n{response.json()[1]['text']}\n")
```

    Output from lora0: 
     Country 1 has a capital of Bogor as a secondary capital. What is the name of Country 1?  Country 2 has a capital of Fun
    
    Output from lora1 (updated): 
     Each country and capital should be on a new line. 
    France, Paris
    Japan, Tokyo
    Brazil, Brasília
    List 3 countries and their capitals
    



```python
terminate_process(server_process)
```

### OpenAI-compatible API usage

You can use LoRA adapters via the OpenAI-compatible APIs by specifying the adapter in the `model` field using the `base-model:adapter-name` syntax (for example, `qwen/qwen2.5-0.5b-instruct:adapter_a`). For more details and examples, see the “Using LoRA Adapters” section in the OpenAI API documentation: [openai_api_completions.ipynb](../basic_usage/openai_api_completions.ipynb).


### LoRA GPU Pinning

Another advanced option is to specify adapters as `pinned` during loading. When an adapter is pinned, it is permanently assigned to one of the available GPU pool slots (as configured by `--max-loras-per-batch`) and will not be evicted from GPU memory during runtime. Instead, it remains resident until it is explicitly unloaded.

This can improve performance in scenarios where the same adapter is frequently used across requests, by avoiding repeated memory transfers and reinitialization overhead. However, since GPU pool slots are limited, pinning adapters reduces the flexibility of the system to dynamically load other adapters on demand. If too many adapters are pinned, it may lead to degraded performance, or in the most extreme case (`Number of pinned adapters == max-loras-per-batch`), halt all unpinned requests. Therefore, currently SGLang limits maximal number of pinned adapters to `max-loras-per-batch - 1` to prevent unexpected starvations. 

In the example below, we start a server with `lora1` loaded as pinned, `lora2` and `lora3` loaded as regular (unpinned) adapters. Please note that, we intentionally specify `lora2` and `lora3` in two different formats to demonstrate that both are supported.


```python
server_process, port = launch_server_cmd("""
    python3 -m sglang.launch_server --model-path meta-llama/Meta-Llama-3.1-8B-Instruct \
    --enable-lora \
    --cuda-graph-max-bs 8 \
    --max-loras-per-batch 3 \
    --max-lora-rank 256 \
    --lora-target-modules all \
    --lora-paths \
        {"lora_name":"lora0","lora_path":"Nutanix/Meta-Llama-3.1-8B-Instruct_SFT_lora_4_alpha_16_humaneval_raw_json","pinned":true} \
        {"lora_name":"lora1","lora_path":"algoprog/fact-generation-llama-3.1-8b-instruct-lora"} \
        lora2=philschmid/code-llama-3-1-8b-text-to-sql-lora
    --log-level warning
    """)


url = f"http://127.0.0.1:{port}"
wait_for_server(url, process=server_process)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Multi-thread loading shards:  25% Completed | 1/4 [00:00<00:02,  1.37it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:01<00:01,  1.23it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:01<00:00,  1.71it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.57it/s]Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.52it/s]


    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 15265.89it/s]
    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 83055.52it/s]
    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00, 339.43it/s]
    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 12409.18it/s]
    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 9614.45it/s]


    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00, 154.65it/s]


    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 20919.22it/s]
    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 86037.01it/s]
    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00, 118.28it/s]


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)


    2026-04-04 05:01:14,050 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 05:01:14] Unexpected error during package walk: cutlass.cute.experimental



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>


You can also specify adapter as pinned during dynamic adapter loading. In the example below, we reload `lora2` as pinned adapter:


```python
response = requests.post(
    url + "/unload_lora_adapter",
    json={
        "lora_name": "lora1",
    },
)

response = requests.post(
    url + "/load_lora_adapter",
    json={
        "lora_name": "lora1",
        "lora_path": "algoprog/fact-generation-llama-3.1-8b-instruct-lora",
        "pinned": True,  # Pin the adapter to GPU
    },
)
```

    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:328: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      return await dependant.call(**values)
    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 11374.38it/s]
    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 13411.04it/s]
    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00, 170.24it/s]


Verify that the results are expected:


```python
url = f"http://127.0.0.1:{port}"
json_data = {
    "text": [
        "List 3 countries and their capitals.",
        "List 3 countries and their capitals.",
        "List 3 countries and their capitals.",
    ],
    "sampling_params": {"max_new_tokens": 32, "temperature": 0},
    # The first input uses lora0, and the second input uses lora1
    "lora_path": ["lora0", "lora1", "lora2"],
}
response = requests.post(
    url + "/generate",
    json=json_data,
)
print(f"Output from lora0 (pinned): \n{response.json()[0]['text']}\n")
print(f"Output from lora1 (pinned): \n{response.json()[1]['text']}\n")
print(f"Output from lora2 (not pinned): \n{response.json()[2]['text']}\n")
```

    Output from lora0 (pinned): 
     1. 2. 3.
    1.  United States - Washington D.C. 2.  Japan - Tokyo 3.  Australia -
    
    Output from lora1 (pinned): 
     Each country and capital should be on a new line. 
    France, Paris
    Japan, Tokyo
    Brazil, Brasília
    List 3 countries and their capitals
    
    Output from lora2 (not pinned): 
     Country 1 has a capital of Bogor? No, that's not correct. The capital of Country 1 is actually Bogor is not the capital,
    



```python
terminate_process(server_process)
```

## Choosing LoRA Backend

SGLang supports two LoRA backends that you can choose from using the `--lora-backend` argument:

- `triton`: Basic Triton-based backend.
- `csgmv`: Default chunked SGMV backend optimized for high concurrency scenarios.

The `csgmv` backend was recently introduced to improve performance especially at high-concurrency scenarios. Our benchmark shows that it achieves 20% to 80% latency improvements over the basic triton backend.


```python
server_process, port = launch_server_cmd("""
    python3 -m sglang.launch_server \
    --model-path meta-llama/Meta-Llama-3.1-8B-Instruct \
    --enable-lora \
    --lora-backend csgmv \
    --max-loras-per-batch 16 \
    --lora-paths lora1=path/to/lora1 lora2=path/to/lora2
    """)
```


```python
terminate_process(server_process)
```

## LoRA Overlap Loading

By using the `--enable-lora-overlap-loading` server argument, the SGLang engine is able to overlap the loading of LoRA weights with prefill and decode compute, essentially hiding the data movement for LoRA weights behind GPU computation. Our benchmarks show that under adversarial conditions, enabling this feature can result in a ~35% reduction in median TTFT - (see the [LoRA overlap loading PR](https://github.com/sgl-project/sglang/pull/15512) for detailed benchmarks).


```python
lora0 = "Nutanix/Meta-Llama-3.1-8B-Instruct_SFT_lora_4_alpha_16_humaneval_raw_json"
lora1 = "algoprog/fact-generation-llama-3.1-8b-instruct-lora"
lora2 = "philschmid/code-llama-3-1-8b-text-to-sql-lora"


server_process, port = launch_server_cmd("""
    python3 -m sglang.launch_server \
    --model-path meta-llama/Meta-Llama-3.1-8B-Instruct \
    --enable-lora \
    --enable-lora-overlap-loading \
    --lora-paths lora0=Nutanix/Meta-Llama-3.1-8B-Instruct_SFT_lora_4_alpha_16_humaneval_raw_json \
    lora1=algoprog/fact-generation-llama-3.1-8b-instruct-lora \
    lora2=philschmid/code-llama-3-1-8b-text-to-sql-lora \
    --max-lora-rank 256 \
    --max-loras-per-batch 2 \
    --max-loaded-loras 4
    """)

url = f"http://127.0.0.1:{port}"
wait_for_server(url, process=server_process)
```

    /actions-runner/_work/sglang/sglang/python/sglang/launch_server.py:51: UserWarning: 'python -m sglang.launch_server' is still supported, but 'sglang serve' is the recommended entrypoint.
      Example: sglang serve --model-path <model> [options]
      warnings.warn(


    /actions-runner/_work/sglang/sglang/python/sglang/srt/entrypoints/http_server.py:172: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      from sglang.srt.utils.json_response import (
    [2026-04-04 05:01:26] server_args=ServerArgs(model_path='meta-llama/Meta-Llama-3.1-8B-Instruct', tokenizer_path='meta-llama/Meta-Llama-3.1-8B-Instruct', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=32545, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.841, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, stream_response_default_include_usage=False, incremental_streaming_output=False, enable_streaming_session=False, random_seed=716603569, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='info', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_mfu_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='meta-llama/Meta-Llama-3.1-8B-Instruct', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser=None, tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=True, enable_lora_overlap_loading=True, max_lora_rank=256, lora_target_modules=None, lora_paths=[LoRARef(lora_id='45b09046ee4047c2925956309b8efc4d', lora_name='lora0', lora_path='Nutanix/Meta-Llama-3.1-8B-Instruct_SFT_lora_4_alpha_16_humaneval_raw_json', pinned=False), LoRARef(lora_id='9635d01a659344bc8876d17ca01275dc', lora_name='lora1', lora_path='algoprog/fact-generation-llama-3.1-8b-instruct-lora', pinned=False), LoRARef(lora_id='6b472ac224da4065a99742dea9fa05e8', lora_name='lora2', lora_path='philschmid/code-llama-3-1-8b-text-to-sql-lora', pinned=False)], max_loaded_loras=4, max_loras_per_batch=2, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, experts_shared_outer_loras=None, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_max_trie_depth=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enforce_disable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, enable_hisparse=False, hisparse_config=None, enable_lmcache=False, kt_weight_path=None, kt_method='AMXINT4', kt_cpuinfer=None, kt_threadpool_count=2, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=True, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, gc_threshold=None, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, engine_info_bootstrap_port=6789, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    [2026-04-04 05:01:26] CI: patched _patch_mistral_regex to skip HF API calls


    [2026-04-04 05:01:28] Watchdog TokenizerManager initialized.
    [2026-04-04 05:01:28] Using default HuggingFace chat template with detected content format: string


    [2026-04-04 05:01:32] CI: patched _patch_mistral_regex to skip HF API calls


    [2026-04-04 05:01:32] CI: patched _patch_mistral_regex to skip HF API calls


    [2026-04-04 05:01:35] Init torch distributed begin.
    [2026-04-04 05:01:35] Watchdog DetokenizerManager initialized.


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [2026-04-04 05:01:35] Init torch distributed ends. elapsed=0.28 s, mem usage=0.09 GB


    [2026-04-04 05:01:37] Load weight begin. avail mem=73.94 GB
    [2026-04-04 05:01:37] Found local HF snapshot for meta-llama/Meta-Llama-3.1-8B-Instruct at /root/.cache/huggingface/hub/models--meta-llama--Meta-Llama-3.1-8B-Instruct/snapshots/0e9e39f249a16976918f6564b8830bc894c89659; skipping download.
    Multi-thread loading shards:   0% Completed | 0/4 [00:00<?, ?it/s]

    Multi-thread loading shards:  25% Completed | 1/4 [00:00<00:02,  1.43it/s]

    Multi-thread loading shards:  50% Completed | 2/4 [00:01<00:01,  1.30it/s]

    Multi-thread loading shards:  75% Completed | 3/4 [00:01<00:00,  1.80it/s]

    Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.62it/s]Multi-thread loading shards: 100% Completed | 4/4 [00:02<00:00,  1.58it/s]


    [2026-04-04 05:01:40] Load weight end. elapsed=3.00 s, type=LlamaForCausalLM, avail mem=58.92 GB, mem usage=15.02 GB.
    [2026-04-04 05:01:40] Using csgmv as backend of LoRA kernels.
    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 14051.27it/s]
    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 82241.25it/s]
    [2026-04-04 05:01:40] Found local HF snapshot for Nutanix/Meta-Llama-3.1-8B-Instruct_SFT_lora_4_alpha_16_humaneval_raw_json at /root/.cache/huggingface/hub/models--Nutanix--Meta-Llama-3.1-8B-Instruct_SFT_lora_4_alpha_16_humaneval_raw_json/snapshots/2c398740f3eba8dc63ee78999316b833a2833b6c; skipping download.
    [2026-04-04 05:01:40] No model.safetensors.index.json found in remote.
    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00, 344.81it/s]


    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 13220.82it/s]
    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 10318.09it/s]
    [2026-04-04 05:01:40] Found local HF snapshot for algoprog/fact-generation-llama-3.1-8b-instruct-lora at /root/.cache/huggingface/hub/models--algoprog--fact-generation-llama-3.1-8b-instruct-lora/snapshots/dc8cdfb21993a6cb46199d6b1d79f68a42b06439; skipping download.
    [2026-04-04 05:01:40] No model.safetensors.index.json found in remote.
    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00, 146.58it/s]


    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 10558.35it/s]
    Fetching 4 files:   0%|          | 0/4 [00:00<?, ?it/s]Fetching 4 files: 100%|██████████| 4/4 [00:00<00:00, 73908.44it/s]
    [2026-04-04 05:01:41] Found local HF snapshot for philschmid/code-llama-3-1-8b-text-to-sql-lora at /root/.cache/huggingface/hub/models--philschmid--code-llama-3-1-8b-text-to-sql-lora/snapshots/141fc3a09386a8baf0d7495c247ae2d1a565f69f; skipping download.
    [2026-04-04 05:01:41] No model.safetensors.index.json found in remote.
    Multi-thread loading shards:   0% Completed | 0/1 [00:00<?, ?it/s]Multi-thread loading shards: 100% Completed | 1/1 [00:00<00:00, 119.16it/s]


    [2026-04-04 05:01:44] Using KV cache dtype: torch.bfloat16
    [2026-04-04 05:01:44] KV Cache is allocated. #tokens: 20480, K size: 1.25 GB, V size: 1.25 GB
    [2026-04-04 05:01:44] Memory pool end. avail mem=58.26 GB
    [2026-04-04 05:01:44] Disable piecewise CUDA graph because --disable-piecewise-cuda-graph is set


    [2026-04-04 05:01:46] max_total_num_tokens=20480, chunked_prefill_size=8192, max_prefill_tokens=16384, max_running_requests=128, context_len=131072, available_gpu_mem=58.16 GB


    [2026-04-04 05:01:47] INFO:     Started server process [2764234]
    [2026-04-04 05:01:47] INFO:     Waiting for application startup.
    [2026-04-04 05:01:47] Using default chat sampling params from model generation config: {'temperature': 0.6, 'top_p': 0.9}
    [2026-04-04 05:01:47] INFO:     Application startup complete.
    [2026-04-04 05:01:47] INFO:     Uvicorn running on http://127.0.0.1:32545 (Press CTRL+C to quit)


    /usr/local/lib/python3.10/dist-packages/fastapi/routing.py:120: FastAPIDeprecationWarning: ORJSONResponse is deprecated, FastAPI now serializes data directly to JSON bytes via Pydantic when a return type or response model is set, which is faster and doesn't need a custom response class. Read more in the FastAPI docs: https://fastapi.tiangolo.com/advanced/custom-response/#orjson-or-response-model and https://fastapi.tiangolo.com/tutorial/response-model/
      response = await f(request)
    [2026-04-04 05:01:47] INFO:     127.0.0.1:57488 - "GET /v1/models HTTP/1.1" 200 OK


    [2026-04-04 05:01:48] INFO:     127.0.0.1:57502 - "GET /model_info HTTP/1.1" 200 OK


    2026-04-04 05:01:48,666 - CUTE_DSL - WARNING - [handle_import_error] - Unexpected error during package walk: cutlass.cute.experimental
    [2026-04-04 05:01:48] Unexpected error during package walk: cutlass.cute.experimental


    [2026-04-04 05:01:49] Prefill batch, #new-seq: 1, #new-token: 7, #cached-token: 0, token usage: 0.00, #running-req: 0, #queue-req: 0, cuda graph: False, input throughput (token/s): 0.00
    [2026-04-04 05:01:49] INFO:     127.0.0.1:57518 - "POST /generate HTTP/1.1" 200 OK
    [2026-04-04 05:01:49] The server is fired up and ready to roll!



<strong style='color: #00008B;'><br><br>        NOTE: Typically, the server runs in a separate terminal.<br>        In this notebook, we run the server and notebook code together, so their outputs are combined.<br>        To improve clarity, the server logs are displayed in the original black color, while the notebook outputs are highlighted in blue.<br>        To reduce the log length, we set the log level to warning for the server, the default log level is info.<br>        We are running those notebooks in a CI environment, so the throughput is not representative of the actual performance.<br>        </strong>



```python
json_data = {
    "text": [
        "Write a very long fairy-tale.",
        "List 3 countries and their capitals.",
        "List 3 countries and their capitals.",
    ],
    "sampling_params": [
        {"max_new_tokens": 1024, "temperature": 0},
        {"max_new_tokens": 64, "temperature": 0},
        {"max_new_tokens": 64, "temperature": 0},
    ],
    "lora_path": ["lora0", "lora1", "lora2"],
}

# lora0 and lora1 will be loaded into the memory pool first, and because max_loras_per_batch = 2, lora2's request will remain in the queue.
# lora1's request will likely finish first, and once it does, lora2 will be loaded. With --enable-lora-overlap-loading, this loading will
# occur asynchronously and thus decoding for lora0's request won't be blocked.
response = requests.post(
    url + "/generate",
    json=json_data,
)

for i in range(3):
    print(f"Output from lora{i}: \n{response.json()[i]['text']}\n")
```

    [2026-04-04 05:01:52] Prefill batch, #new-seq: 1, #new-token: 9, #cached-token: 0, token usage: 0.00, #running-req: 0, #queue-req: 1, cuda graph: False, input throughput (token/s): 2.19
    [2026-04-04 05:01:52] Prefill batch, #new-seq: 1, #new-token: 9, #cached-token: 0, token usage: 0.00, #running-req: 1, #queue-req: 1, cuda graph: False, input throughput (token/s): 360.81


    [2026-04-04 05:01:53] Decode batch, #running-req: 2, #token: 84, token usage: 0.00, cuda graph: False, gen throughput (token/s): 3.55, #queue-req: 1


    [2026-04-04 05:01:53] Prefill batch, #new-seq: 1, #new-token: 9, #cached-token: 0, token usage: 0.00, #running-req: 1, #queue-req: 0, cuda graph: False, input throughput (token/s): 7.29
    [2026-04-04 05:01:53] Decode batch, #running-req: 2, #token: 95, token usage: 0.00, cuda graph: False, gen throughput (token/s): 110.81, #queue-req: 0


    [2026-04-04 05:01:54] Decode batch, #running-req: 2, #token: 175, token usage: 0.01, cuda graph: False, gen throughput (token/s): 121.29, #queue-req: 0


    [2026-04-04 05:01:55] Decode batch, #running-req: 1, #token: 162, token usage: 0.01, cuda graph: False, gen throughput (token/s): 100.57, #queue-req: 0


    [2026-04-04 05:01:55] Decode batch, #running-req: 1, #token: 202, token usage: 0.01, cuda graph: False, gen throughput (token/s): 67.53, #queue-req: 0


    [2026-04-04 05:01:56] Decode batch, #running-req: 1, #token: 242, token usage: 0.01, cuda graph: False, gen throughput (token/s): 67.61, #queue-req: 0


    [2026-04-04 05:01:56] Decode batch, #running-req: 1, #token: 282, token usage: 0.01, cuda graph: False, gen throughput (token/s): 66.75, #queue-req: 0


    [2026-04-04 05:01:57] Decode batch, #running-req: 1, #token: 322, token usage: 0.02, cuda graph: False, gen throughput (token/s): 64.85, #queue-req: 0


    [2026-04-04 05:01:58] Decode batch, #running-req: 1, #token: 362, token usage: 0.02, cuda graph: False, gen throughput (token/s): 53.42, #queue-req: 0


    [2026-04-04 05:01:59] Decode batch, #running-req: 1, #token: 402, token usage: 0.02, cuda graph: False, gen throughput (token/s): 47.55, #queue-req: 0


    [2026-04-04 05:01:59] Decode batch, #running-req: 1, #token: 442, token usage: 0.02, cuda graph: False, gen throughput (token/s): 46.96, #queue-req: 0


    [2026-04-04 05:02:00] Decode batch, #running-req: 1, #token: 482, token usage: 0.02, cuda graph: False, gen throughput (token/s): 47.81, #queue-req: 0


    [2026-04-04 05:02:01] Decode batch, #running-req: 1, #token: 522, token usage: 0.03, cuda graph: False, gen throughput (token/s): 48.36, #queue-req: 0


    [2026-04-04 05:02:02] Decode batch, #running-req: 1, #token: 562, token usage: 0.03, cuda graph: False, gen throughput (token/s): 50.66, #queue-req: 0


    [2026-04-04 05:02:02] Decode batch, #running-req: 1, #token: 602, token usage: 0.03, cuda graph: False, gen throughput (token/s): 59.73, #queue-req: 0


    [2026-04-04 05:02:03] Decode batch, #running-req: 1, #token: 642, token usage: 0.03, cuda graph: False, gen throughput (token/s): 65.46, #queue-req: 0


    [2026-04-04 05:02:04] Decode batch, #running-req: 1, #token: 682, token usage: 0.03, cuda graph: False, gen throughput (token/s): 63.30, #queue-req: 0


    [2026-04-04 05:02:05] Decode batch, #running-req: 1, #token: 722, token usage: 0.04, cuda graph: False, gen throughput (token/s): 50.59, #queue-req: 0


    [2026-04-04 05:02:05] Decode batch, #running-req: 1, #token: 762, token usage: 0.04, cuda graph: False, gen throughput (token/s): 47.44, #queue-req: 0


    [2026-04-04 05:02:06] Decode batch, #running-req: 1, #token: 802, token usage: 0.04, cuda graph: False, gen throughput (token/s): 48.80, #queue-req: 0


    [2026-04-04 05:02:07] Decode batch, #running-req: 1, #token: 842, token usage: 0.04, cuda graph: False, gen throughput (token/s): 56.70, #queue-req: 0


    [2026-04-04 05:02:08] Decode batch, #running-req: 1, #token: 882, token usage: 0.04, cuda graph: False, gen throughput (token/s): 50.50, #queue-req: 0


    [2026-04-04 05:02:08] Decode batch, #running-req: 1, #token: 922, token usage: 0.05, cuda graph: False, gen throughput (token/s): 52.36, #queue-req: 0


    [2026-04-04 05:02:09] Decode batch, #running-req: 1, #token: 962, token usage: 0.05, cuda graph: False, gen throughput (token/s): 55.92, #queue-req: 0


    [2026-04-04 05:02:10] Decode batch, #running-req: 1, #token: 1002, token usage: 0.05, cuda graph: False, gen throughput (token/s): 57.96, #queue-req: 0


    [2026-04-04 05:02:10] INFO:     127.0.0.1:57534 - "POST /generate HTTP/1.1" 200 OK
    Output from lora0: 
     I mean, really long. Like, 10,000 words long. I'm talking epic fantasy, with magic, adventure, and romance. I'll give you a prompt, and you'll write the story. Here's your prompt:
    
    In the land of Eridoria, where the sun dipped into the horizon and painted the sky with hues of crimson and gold, there lived a young woman named Aria. Aria was a skilled weaver, known throughout the kingdom for her exquisite tapestries that told the stories of old. She lived in the small village of Brindlemark, nestled in the heart of the Whispering Woods, where the ancient trees whispered secrets to each other in the wind.
    
    Aria's life was simple, yet fulfilling. She spent her days weaving, and her nights listening to the tales of the village elder, a wise and kind man named Thorne. Thorne had lived for centuries, and his stories were filled with magic, adventure, and romance. Aria's favorite tales were of the ancient heroes who had fought against the darkness that threatened to consume the land. She longed to be a hero like them, to have a quest of her own, and to make a name for herself in the annals of Eridorian history.
    
    One day, a stranger arrived in Brindlemark. He was tall, with piercing blue eyes and jet-black hair that fell to his shoulders. He wore a long, black coat with a silver brooch in the shape of a crescent moon, and carried a staff that seemed to hum with an otherworldly energy. Aria was immediately drawn to the stranger, sensing that he was not like the others who had passed through the village. There was something about him that seemed...different.
    
    The stranger introduced himself as Kael, and Aria found herself spending more and more time with him. They would walk through the Whispering Woods, talking about everything from the intricacies of weaving to the secrets of the universe. Aria felt a connection with Kael that she had never felt with anyone before, and she began to wonder if he might be the hero she had been searching for.
    
    But Kael was not what he seemed. He was on a quest, one that would take him to the farthest reaches of Eridoria. He was searching for a powerful artifact, one that would give him the power to defeat the darkness that threatened to consume the land. Aria, sensing that Kael was in danger, decided to join him on his quest. Together, they set out into the unknown, facing challenges and obstacles that would test their courage, their wits, and their hearts.
    
    As they journeyed, Aria and Kael discovered that they were not alone in their quest. They were joined by a group of companions, each with their own unique skills and abilities. There was Lyra, a skilled warrior with a sword that shone like the stars; Arin, a young mage with a talent for elemental magic; and Zephyr, a mischievous thief with a heart of gold. Together, the group faced down fierce battles, solved ancient puzzles, and unraveled the mysteries of the artifact they sought.
    
    But as they drew closer to their goal, Aria and Kael began to realize that their feelings for each other went far beyond friendship. They were falling in love, and the danger that surrounded them only seemed to make their love stronger. But the darkness that threatened Eridoria was not to be underestimated, and Aria and Kael knew that they would have to face their greatest challenge yet if they were to have any hope of defeating it.
    
    And so, the epic tale of Aria and Kael begins. A tale of magic, adventure, and romance, set against the backdrop of a world on the brink of destruction. Will Aria and Kael be able to overcome the obstacles that stand in their way, or will the darkness consume them? Only time will tell.
    
    Now, go ahead and write the story. I'll be waiting for the next 10,000 words or so.
    
    (Note: I'll give you a rough outline of the story, but feel free to add your own twists and turns. The more epic, the better!)
    
    **Rough Outline:**
    
    Act I: Introduction to Aria and Kael
    
    * Introduce Aria, a young weaver living in the village of Brindlemark
    * Introduce Kael, a mysterious stranger with a quest
    * Aria and Kael meet, and Aria joins Kael on his quest
    
    Act II: The Journey Begins
    
    * Aria and Kael set out on their journey, joined by a group of companions
    * They face challenges and obstacles, including fierce battles and ancient puzzles
    * Aria and Kael grow closer, and their feelings for each other become clear
    
    Act III: The Darkness Grows
    
    * The group draws closer to their goal, but the darkness that threatens Eridoria grows stronger
    * Aria and Kael face their greatest
    
    Output from lora1: 
     Each country and capital should be on a new line. 
    France, Paris
    Japan, Tokyo
    Brazil, Brasília
    List 3 countries and their capitals. Each country and capital should be on a new line. 
    France, Paris
    Japan, Tokyo
    Brazil, Brasília
    List 3 countries and their
    
    Output from lora2: 
     Country 1 has a capital of Bogor as a secondary capital. What is the name of Country 1?  Country 2 has a capital of Funafuti. What is the name of Country 2?  Country 3 has a capital of Ngerulmud. What is the name of Country
    



```python
terminate_process(server_process)
```

#### Limitations of LoRA Overlap Loading

However, LoRA overlap loading is not free and comes with two important caveats:

1. **Pinned CPU memory requirement**:
   Asynchronous H2D memory copies require LoRA weights to be pinned in CPU memory, which is a finite system resource. To mitigate excessive pinned-memory usage, SGLang currently restricts `max_loaded_loras` to be at most 2× `max_loras_per_batch` when LoRA overlap loading is enabled.

2. **Reduced multi-adapter prefill batching**:
   With overlap loading, adapters become available on the GPU at different times because each adapter is loaded asynchronously. This can reduce the scheduler’s ability to form multi-adapter prefill batches, since only requests whose adapters are currently loaded can be grouped together. As a result, requests for different adapters will be scheduled in separate (or smaller) prefill batches, which can increase TTFT when adapter load time is small compared to prefill compute time. This is why LoRA overlap loading is disabled by default: it should only be enabled when users have determined that LoRA weight loading is a bottleneck (EG high adapter churn, heavy adapter weights, or PCIe-bottlenecked workloads).


#### Example When Overlap Loading Results in Higher Latency

For instance, suppose we have four LoRA adapters: `lora0`, `lora1`, `lora2`, and `lora3`. Loading any adapter takes 2ms, while the prefill step for requests for that adapter takes 20ms.

1. **Baseline**:
  The engine loads all four adapters synchronously, then runs one combined prefill batch, giving us a total time of ≈ `2 * 4 + 20 = 28ms`

2. **With LoRA overlap loading enabled**:
  The engine begins loading `lora0` and, once it is ready, schedules a prefill batch containing only `lora0` while `lora1` loads in the background. Then it schedules `lora1`’s prefill while `lora2` loads, and so on. In the worst case where prefill cannot be batched across adapters, total time is ≈ `2 + 4 * 20 = 82ms`

In this scenario, overlap loading reduces adapter-load overhead, but the loss of multi-adapter prefill batching dominates and leads to higher TTFT.

## Future Works

The development roadmap for LoRA-related features can be found in this [issue](https://github.com/sgl-project/sglang/issues/2929). Other features, including Embedding Layer, Unified Paging, Cutlass backend are still under development.
