# Offline Engine API

SGLang provides a direct inference engine without the need for an HTTP server, especially for use cases where additional HTTP server adds unnecessary complexity or overhead. Here are two general use cases:

- Offline Batch Inference
- Custom Server on Top of the Engine

This document focuses on the offline batch inference, demonstrating four different inference modes:

- Non-streaming synchronous generation
- Streaming synchronous generation
- Non-streaming asynchronous generation
- Streaming asynchronous generation

Additionally, you can easily build a custom server on top of the SGLang offline engine. A detailed example working in a python script can be found in [custom_server](https://github.com/sgl-project/sglang/blob/main/examples/runtime/engine/custom_server.py).



## Nest Asyncio
Note that if you want to use **Offline Engine** in ipython or some other nested loop code, you need to add the following code:
```python
import nest_asyncio

nest_asyncio.apply()

```

## Advanced Usage

The engine supports [vlm inference](https://github.com/sgl-project/sglang/blob/main/examples/runtime/engine/offline_batch_inference_vlm.py) as well as [extracting hidden states](https://github.com/sgl-project/sglang/blob/main/examples/runtime/hidden_states). 

Please see [the examples](https://github.com/sgl-project/sglang/tree/main/examples/runtime/engine) for further use cases.

## Offline Batch Inference

SGLang offline engine supports batch inference with efficient scheduling.


```python
# launch the offline engine
import asyncio

import sglang as sgl
import sglang.test.doc_patch  # noqa: F401
from sglang.utils import async_stream_and_merge, stream_and_merge

llm = sgl.Engine(model_path="qwen/qwen2.5-0.5b-instruct")
```

    [2026-03-23 02:28:28] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-23 02:28:28] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-23 02:28:28] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-23 02:28:31] WARNING model_config.py:1098: Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-23 02:28:31] INFO server_args.py:2239: Attention backend not specified. Use fa3 backend by default.


    [2026-03-23 02:28:31] INFO server_args.py:3522: Set soft_watchdog_timeout since in CI


    [2026-03-23 02:28:31] INFO engine.py:177: server_args=ServerArgs(model_path='qwen/qwen2.5-0.5b-instruct', tokenizer_path='qwen/qwen2.5-0.5b-instruct', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=30000, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.903, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=699259604, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='error', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='qwen/qwen2.5-0.5b-instruct', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser=None, tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_match_window_size=1, speculative_ngram_max_match_window_size=12, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_branch_length=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', disable_hicache_numa_detect=False, hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, hierarchical_sparse_attention_extra_config=None, enable_lmcache=False, kt_weight_path=None, kt_method=None, kt_cpuinfer=None, kt_threadpool_count=None, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4, 8, 12, 16, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96, 104, 112, 120, 128, 136, 144, 152, 160, 168, 176, 184, 192, 200, 208, 216, 224, 232, 240, 248, 256], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, mm_max_concurrent_calls=32, mm_per_request_timeout=10.0, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.70it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  7.69it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:41,  2.84s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:41,  2.84s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:41,  2.84s/it]Compiling num tokens (num_tokens=7168):   5%|▌         | 3/58 [00:02<00:42,  1.29it/s]Compiling num tokens (num_tokens=6656):   5%|▌         | 3/58 [00:02<00:42,  1.29it/s]Compiling num tokens (num_tokens=6144):   5%|▌         | 3/58 [00:02<00:42,  1.29it/s]Compiling num tokens (num_tokens=5632):   5%|▌         | 3/58 [00:02<00:42,  1.29it/s]Compiling num tokens (num_tokens=5120):   5%|▌         | 3/58 [00:03<00:42,  1.29it/s]

    Compiling num tokens (num_tokens=5120):  12%|█▏        | 7/58 [00:03<00:13,  3.75it/s]Compiling num tokens (num_tokens=4608):  12%|█▏        | 7/58 [00:03<00:13,  3.75it/s]Compiling num tokens (num_tokens=4096):  12%|█▏        | 7/58 [00:03<00:13,  3.75it/s]Compiling num tokens (num_tokens=3840):  12%|█▏        | 7/58 [00:03<00:13,  3.75it/s]Compiling num tokens (num_tokens=3584):  12%|█▏        | 7/58 [00:03<00:13,  3.75it/s]Compiling num tokens (num_tokens=3328):  12%|█▏        | 7/58 [00:03<00:13,  3.75it/s]Compiling num tokens (num_tokens=3072):  12%|█▏        | 7/58 [00:03<00:13,  3.75it/s]Compiling num tokens (num_tokens=3072):  22%|██▏       | 13/58 [00:03<00:05,  8.31it/s]Compiling num tokens (num_tokens=2816):  22%|██▏       | 13/58 [00:03<00:05,  8.31it/s]Compiling num tokens (num_tokens=2560):  22%|██▏       | 13/58 [00:03<00:05,  8.31it/s]Compiling num tokens (num_tokens=2304):  22%|██▏       | 13/58 [00:03<00:05,  8.31it/s]Compiling num tokens (num_tokens=2048):  22%|██▏       | 13/58 [00:03<00:05,  8.31it/s]Compiling num tokens (num_tokens=1792):  22%|██▏       | 13/58 [00:03<00:05,  8.31it/s]Compiling num tokens (num_tokens=1536):  22%|██▏       | 13/58 [00:03<00:05,  8.31it/s]

    Compiling num tokens (num_tokens=1280):  22%|██▏       | 13/58 [00:03<00:05,  8.31it/s]Compiling num tokens (num_tokens=1280):  34%|███▍      | 20/58 [00:03<00:02, 14.61it/s]Compiling num tokens (num_tokens=1024):  34%|███▍      | 20/58 [00:03<00:02, 14.61it/s]Compiling num tokens (num_tokens=960):  34%|███▍      | 20/58 [00:03<00:02, 14.61it/s] Compiling num tokens (num_tokens=896):  34%|███▍      | 20/58 [00:03<00:02, 14.61it/s]Compiling num tokens (num_tokens=832):  34%|███▍      | 20/58 [00:03<00:02, 14.61it/s]Compiling num tokens (num_tokens=768):  34%|███▍      | 20/58 [00:03<00:02, 14.61it/s]Compiling num tokens (num_tokens=704):  34%|███▍      | 20/58 [00:03<00:02, 14.61it/s]Compiling num tokens (num_tokens=640):  34%|███▍      | 20/58 [00:03<00:02, 14.61it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 21.29it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 21.29it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 21.29it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 21.29it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 21.29it/s]

    Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 21.29it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 21.29it/s]Compiling num tokens (num_tokens=352):  47%|████▋     | 27/58 [00:03<00:01, 21.29it/s]Compiling num tokens (num_tokens=352):  59%|█████▊    | 34/58 [00:03<00:00, 28.22it/s]Compiling num tokens (num_tokens=320):  59%|█████▊    | 34/58 [00:03<00:00, 28.22it/s]Compiling num tokens (num_tokens=288):  59%|█████▊    | 34/58 [00:03<00:00, 28.22it/s]Compiling num tokens (num_tokens=256):  59%|█████▊    | 34/58 [00:03<00:00, 28.22it/s]Compiling num tokens (num_tokens=240):  59%|█████▊    | 34/58 [00:03<00:00, 28.22it/s]Compiling num tokens (num_tokens=224):  59%|█████▊    | 34/58 [00:03<00:00, 28.22it/s]Compiling num tokens (num_tokens=208):  59%|█████▊    | 34/58 [00:03<00:00, 28.22it/s]Compiling num tokens (num_tokens=208):  69%|██████▉   | 40/58 [00:03<00:00, 32.48it/s]Compiling num tokens (num_tokens=192):  69%|██████▉   | 40/58 [00:03<00:00, 32.48it/s]Compiling num tokens (num_tokens=176):  69%|██████▉   | 40/58 [00:03<00:00, 32.48it/s]

    Compiling num tokens (num_tokens=160):  69%|██████▉   | 40/58 [00:03<00:00, 32.48it/s]Compiling num tokens (num_tokens=144):  69%|██████▉   | 40/58 [00:03<00:00, 32.48it/s]Compiling num tokens (num_tokens=128):  69%|██████▉   | 40/58 [00:03<00:00, 32.48it/s]Compiling num tokens (num_tokens=112):  69%|██████▉   | 40/58 [00:03<00:00, 32.48it/s]Compiling num tokens (num_tokens=112):  79%|███████▉  | 46/58 [00:03<00:00, 36.86it/s]Compiling num tokens (num_tokens=96):  79%|███████▉  | 46/58 [00:03<00:00, 36.86it/s] Compiling num tokens (num_tokens=80):  79%|███████▉  | 46/58 [00:03<00:00, 36.86it/s]Compiling num tokens (num_tokens=64):  79%|███████▉  | 46/58 [00:03<00:00, 36.86it/s]Compiling num tokens (num_tokens=48):  79%|███████▉  | 46/58 [00:03<00:00, 36.86it/s]Compiling num tokens (num_tokens=32):  79%|███████▉  | 46/58 [00:03<00:00, 36.86it/s]Compiling num tokens (num_tokens=28):  79%|███████▉  | 46/58 [00:03<00:00, 36.86it/s]Compiling num tokens (num_tokens=28):  90%|████████▉ | 52/58 [00:03<00:00, 40.79it/s]Compiling num tokens (num_tokens=24):  90%|████████▉ | 52/58 [00:03<00:00, 40.79it/s]

    Compiling num tokens (num_tokens=20):  90%|████████▉ | 52/58 [00:03<00:00, 40.79it/s]Compiling num tokens (num_tokens=16):  90%|████████▉ | 52/58 [00:03<00:00, 40.79it/s]Compiling num tokens (num_tokens=12):  90%|████████▉ | 52/58 [00:03<00:00, 40.79it/s]Compiling num tokens (num_tokens=8):  90%|████████▉ | 52/58 [00:03<00:00, 40.79it/s] Compiling num tokens (num_tokens=4):  90%|████████▉ | 52/58 [00:03<00:00, 40.79it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 43.45it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 14.59it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=117.56 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=117.56 GB):   2%|▏         | 1/58 [00:00<00:10,  5.43it/s]Capturing num tokens (num_tokens=7680 avail_mem=117.31 GB):   2%|▏         | 1/58 [00:00<00:10,  5.43it/s]

    Capturing num tokens (num_tokens=7680 avail_mem=117.31 GB):   3%|▎         | 2/58 [00:00<00:09,  5.81it/s]Capturing num tokens (num_tokens=7168 avail_mem=117.32 GB):   3%|▎         | 2/58 [00:00<00:09,  5.81it/s]Capturing num tokens (num_tokens=7168 avail_mem=117.32 GB):   5%|▌         | 3/58 [00:00<00:09,  5.93it/s]Capturing num tokens (num_tokens=6656 avail_mem=117.33 GB):   5%|▌         | 3/58 [00:00<00:09,  5.93it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=117.33 GB):   7%|▋         | 4/58 [00:00<00:08,  6.24it/s]Capturing num tokens (num_tokens=6144 avail_mem=117.52 GB):   7%|▋         | 4/58 [00:00<00:08,  6.24it/s]Capturing num tokens (num_tokens=6144 avail_mem=117.52 GB):   9%|▊         | 5/58 [00:00<00:08,  6.44it/s]Capturing num tokens (num_tokens=5632 avail_mem=117.51 GB):   9%|▊         | 5/58 [00:00<00:08,  6.44it/s]

    Capturing num tokens (num_tokens=5632 avail_mem=117.51 GB):  10%|█         | 6/58 [00:00<00:07,  7.23it/s]Capturing num tokens (num_tokens=5120 avail_mem=117.51 GB):  10%|█         | 6/58 [00:00<00:07,  7.23it/s]Capturing num tokens (num_tokens=5120 avail_mem=117.51 GB):  12%|█▏        | 7/58 [00:01<00:06,  7.36it/s]Capturing num tokens (num_tokens=4608 avail_mem=117.50 GB):  12%|█▏        | 7/58 [00:01<00:06,  7.36it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=117.50 GB):  14%|█▍        | 8/58 [00:01<00:06,  7.76it/s]Capturing num tokens (num_tokens=4096 avail_mem=117.50 GB):  14%|█▍        | 8/58 [00:01<00:06,  7.76it/s]Capturing num tokens (num_tokens=4096 avail_mem=117.50 GB):  16%|█▌        | 9/58 [00:01<00:06,  8.08it/s]Capturing num tokens (num_tokens=3840 avail_mem=117.49 GB):  16%|█▌        | 9/58 [00:01<00:06,  8.08it/s]

    Capturing num tokens (num_tokens=3840 avail_mem=117.49 GB):  17%|█▋        | 10/58 [00:01<00:05,  8.24it/s]Capturing num tokens (num_tokens=3584 avail_mem=117.48 GB):  17%|█▋        | 10/58 [00:01<00:05,  8.24it/s]Capturing num tokens (num_tokens=3328 avail_mem=117.48 GB):  17%|█▋        | 10/58 [00:01<00:05,  8.24it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=117.48 GB):  21%|██        | 12/58 [00:01<00:05,  8.92it/s]Capturing num tokens (num_tokens=3072 avail_mem=117.47 GB):  21%|██        | 12/58 [00:01<00:05,  8.92it/s]Capturing num tokens (num_tokens=2816 avail_mem=117.47 GB):  21%|██        | 12/58 [00:01<00:05,  8.92it/s]Capturing num tokens (num_tokens=2816 avail_mem=117.47 GB):  24%|██▍       | 14/58 [00:01<00:04,  9.54it/s]Capturing num tokens (num_tokens=2560 avail_mem=117.46 GB):  24%|██▍       | 14/58 [00:01<00:04,  9.54it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=117.45 GB):  24%|██▍       | 14/58 [00:01<00:04,  9.54it/s]Capturing num tokens (num_tokens=2304 avail_mem=117.45 GB):  28%|██▊       | 16/58 [00:01<00:04,  9.85it/s]Capturing num tokens (num_tokens=2048 avail_mem=117.44 GB):  28%|██▊       | 16/58 [00:01<00:04,  9.85it/s]Capturing num tokens (num_tokens=1792 avail_mem=117.42 GB):  28%|██▊       | 16/58 [00:02<00:04,  9.85it/s]

    Capturing num tokens (num_tokens=1792 avail_mem=117.42 GB):  31%|███       | 18/58 [00:02<00:03, 10.19it/s]Capturing num tokens (num_tokens=1536 avail_mem=117.43 GB):  31%|███       | 18/58 [00:02<00:03, 10.19it/s]Capturing num tokens (num_tokens=1280 avail_mem=117.43 GB):  31%|███       | 18/58 [00:02<00:03, 10.19it/s]Capturing num tokens (num_tokens=1280 avail_mem=117.43 GB):  34%|███▍      | 20/58 [00:02<00:03, 10.68it/s]Capturing num tokens (num_tokens=1024 avail_mem=117.40 GB):  34%|███▍      | 20/58 [00:02<00:03, 10.68it/s]

    Capturing num tokens (num_tokens=960 avail_mem=117.41 GB):  34%|███▍      | 20/58 [00:02<00:03, 10.68it/s] Capturing num tokens (num_tokens=960 avail_mem=117.41 GB):  38%|███▊      | 22/58 [00:02<00:03, 11.18it/s]Capturing num tokens (num_tokens=896 avail_mem=117.41 GB):  38%|███▊      | 22/58 [00:02<00:03, 11.18it/s]Capturing num tokens (num_tokens=832 avail_mem=117.40 GB):  38%|███▊      | 22/58 [00:02<00:03, 11.18it/s]

    Capturing num tokens (num_tokens=832 avail_mem=117.40 GB):  41%|████▏     | 24/58 [00:02<00:03, 11.30it/s]Capturing num tokens (num_tokens=768 avail_mem=117.39 GB):  41%|████▏     | 24/58 [00:02<00:03, 11.30it/s]Capturing num tokens (num_tokens=704 avail_mem=117.38 GB):  41%|████▏     | 24/58 [00:02<00:03, 11.30it/s]Capturing num tokens (num_tokens=704 avail_mem=117.38 GB):  45%|████▍     | 26/58 [00:02<00:02, 12.07it/s]Capturing num tokens (num_tokens=640 avail_mem=117.39 GB):  45%|████▍     | 26/58 [00:02<00:02, 12.07it/s]

    Capturing num tokens (num_tokens=576 avail_mem=117.39 GB):  45%|████▍     | 26/58 [00:02<00:02, 12.07it/s]Capturing num tokens (num_tokens=576 avail_mem=117.39 GB):  48%|████▊     | 28/58 [00:02<00:02, 12.67it/s]Capturing num tokens (num_tokens=512 avail_mem=117.38 GB):  48%|████▊     | 28/58 [00:02<00:02, 12.67it/s]Capturing num tokens (num_tokens=480 avail_mem=117.39 GB):  48%|████▊     | 28/58 [00:02<00:02, 12.67it/s]Capturing num tokens (num_tokens=480 avail_mem=117.39 GB):  52%|█████▏    | 30/58 [00:03<00:02, 13.94it/s]Capturing num tokens (num_tokens=448 avail_mem=117.38 GB):  52%|█████▏    | 30/58 [00:03<00:02, 13.94it/s]

    Capturing num tokens (num_tokens=416 avail_mem=117.38 GB):  52%|█████▏    | 30/58 [00:03<00:02, 13.94it/s]Capturing num tokens (num_tokens=416 avail_mem=117.38 GB):  55%|█████▌    | 32/58 [00:03<00:01, 15.11it/s]Capturing num tokens (num_tokens=384 avail_mem=117.37 GB):  55%|█████▌    | 32/58 [00:03<00:01, 15.11it/s]Capturing num tokens (num_tokens=352 avail_mem=117.36 GB):  55%|█████▌    | 32/58 [00:03<00:01, 15.11it/s]Capturing num tokens (num_tokens=352 avail_mem=117.36 GB):  59%|█████▊    | 34/58 [00:03<00:01, 16.08it/s]Capturing num tokens (num_tokens=320 avail_mem=117.36 GB):  59%|█████▊    | 34/58 [00:03<00:01, 16.08it/s]

    Capturing num tokens (num_tokens=288 avail_mem=117.35 GB):  59%|█████▊    | 34/58 [00:03<00:01, 16.08it/s]Capturing num tokens (num_tokens=256 avail_mem=117.35 GB):  59%|█████▊    | 34/58 [00:03<00:01, 16.08it/s]Capturing num tokens (num_tokens=256 avail_mem=117.35 GB):  64%|██████▍   | 37/58 [00:03<00:01, 18.48it/s]Capturing num tokens (num_tokens=240 avail_mem=117.34 GB):  64%|██████▍   | 37/58 [00:03<00:01, 18.48it/s]Capturing num tokens (num_tokens=224 avail_mem=117.34 GB):  64%|██████▍   | 37/58 [00:03<00:01, 18.48it/s]Capturing num tokens (num_tokens=208 avail_mem=117.33 GB):  64%|██████▍   | 37/58 [00:03<00:01, 18.48it/s]Capturing num tokens (num_tokens=208 avail_mem=117.33 GB):  69%|██████▉   | 40/58 [00:03<00:00, 20.96it/s]Capturing num tokens (num_tokens=192 avail_mem=117.33 GB):  69%|██████▉   | 40/58 [00:03<00:00, 20.96it/s]

    Capturing num tokens (num_tokens=176 avail_mem=117.30 GB):  69%|██████▉   | 40/58 [00:03<00:00, 20.96it/s]Capturing num tokens (num_tokens=160 avail_mem=117.29 GB):  69%|██████▉   | 40/58 [00:03<00:00, 20.96it/s]Capturing num tokens (num_tokens=144 avail_mem=117.28 GB):  69%|██████▉   | 40/58 [00:03<00:00, 20.96it/s]Capturing num tokens (num_tokens=144 avail_mem=117.28 GB):  76%|███████▌  | 44/58 [00:03<00:00, 23.71it/s]Capturing num tokens (num_tokens=128 avail_mem=117.29 GB):  76%|███████▌  | 44/58 [00:03<00:00, 23.71it/s]Capturing num tokens (num_tokens=112 avail_mem=117.29 GB):  76%|███████▌  | 44/58 [00:03<00:00, 23.71it/s]Capturing num tokens (num_tokens=96 avail_mem=117.28 GB):  76%|███████▌  | 44/58 [00:03<00:00, 23.71it/s] 

    Capturing num tokens (num_tokens=96 avail_mem=117.28 GB):  81%|████████  | 47/58 [00:03<00:00, 25.16it/s]Capturing num tokens (num_tokens=80 avail_mem=117.27 GB):  81%|████████  | 47/58 [00:03<00:00, 25.16it/s]Capturing num tokens (num_tokens=64 avail_mem=117.26 GB):  81%|████████  | 47/58 [00:03<00:00, 25.16it/s]Capturing num tokens (num_tokens=48 avail_mem=117.26 GB):  81%|████████  | 47/58 [00:03<00:00, 25.16it/s]Capturing num tokens (num_tokens=48 avail_mem=117.26 GB):  86%|████████▌ | 50/58 [00:03<00:00, 26.16it/s]Capturing num tokens (num_tokens=32 avail_mem=117.25 GB):  86%|████████▌ | 50/58 [00:03<00:00, 26.16it/s]Capturing num tokens (num_tokens=28 avail_mem=117.24 GB):  86%|████████▌ | 50/58 [00:03<00:00, 26.16it/s]Capturing num tokens (num_tokens=24 avail_mem=117.24 GB):  86%|████████▌ | 50/58 [00:03<00:00, 26.16it/s]Capturing num tokens (num_tokens=20 avail_mem=117.23 GB):  86%|████████▌ | 50/58 [00:03<00:00, 26.16it/s]

    Capturing num tokens (num_tokens=20 avail_mem=117.23 GB):  93%|█████████▎| 54/58 [00:03<00:00, 29.76it/s]Capturing num tokens (num_tokens=16 avail_mem=117.23 GB):  93%|█████████▎| 54/58 [00:03<00:00, 29.76it/s]Capturing num tokens (num_tokens=12 avail_mem=117.22 GB):  93%|█████████▎| 54/58 [00:03<00:00, 29.76it/s]Capturing num tokens (num_tokens=8 avail_mem=117.21 GB):  93%|█████████▎| 54/58 [00:03<00:00, 29.76it/s] Capturing num tokens (num_tokens=4 avail_mem=117.21 GB):  93%|█████████▎| 54/58 [00:04<00:00, 29.76it/s]Capturing num tokens (num_tokens=4 avail_mem=117.21 GB): 100%|██████████| 58/58 [00:04<00:00, 14.40it/s]


### Non-streaming Synchronous Generation


```python
prompts = [
    "Hello, my name is",
    "The president of the United States is",
    "The capital of France is",
    "The future of AI is",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95}

outputs = llm.generate(prompts, sampling_params)
for prompt, output in zip(prompts, outputs):
    print("===============================")
    print(f"Prompt: {prompt}\nGenerated text: {output['text']}")
```

    ===============================
    Prompt: Hello, my name is
    Generated text:  Shiri and I am a 17-year-old boy. I came to this website to learn more about the differences between people. 1. What does the term "person" mean to you? 2. What does "human" mean to you? 3. What do you think of the idea of "human" as it relates to people? 4. What is your opinion on the idea of "human" in the context of people? 5. Is the idea of "human" a problem or a solution?
    5 Answers
    1
    There are many terms that are often used in the context of people.
    ===============================
    Prompt: The president of the United States is
    Generated text:  elected by _________. A. the President of the Senate B. the President of the House of Representatives C. the Speaker of the House of Representatives D. the leader of the Democratic Party
    
    The correct answer is A. The President of the Senate. The president is elected by the U. S. Senate, which is a bicameral legislature. The House of Representatives elects the Speaker of the House of Representatives. The Democratic Party is the party that controls the House of Representatives. The President of the United States is the head of state and is elected by the people of the United States. The President of the United States is also
    ===============================
    Prompt: The capital of France is
    Generated text:  ____
    A. Paris
    B. Brussels
    C. London
    D. New York
    Answer:
    
    A
    
    Which of the following statements about the capital of France is incorrect?
    A. Paris is the capital of France.
    B. The capital of France is not Paris.
    C. The capital of France is the seat of the President of the Republic.
    D. The capital of France is the seat of the President of the Republic and the President of the General National Assembly of France.
    Answer:
    
    C
    
    In the 1920s, the socialist government enacted a series of laws, which included ____.
    A. The General Pro
    ===============================
    Prompt: The future of AI is
    Generated text:  increasingly dependent on the capabilities of its underlying technology. Quantum computing is not only a promising technology for accelerating AI and machine learning, but also has the potential to deliver significant benefits for AI researchers, including faster computation and even faster AI models. However, quantum computing also has the potential to have a negative impact on the AI community, which may result in the collapse of the machine learning industry. If we want to harness the potential of quantum computing, we need to take into consideration the potential risks and mitigate them. In addition, we should also be aware of the legal and ethical implications of quantum computing, which may affect the AI community’s ability to


### Streaming Synchronous Generation


```python
prompts = [
    "Write a short, neutral self-introduction for a fictional character. Hello, my name is",
    "Provide a concise factual statement about France’s capital city. The capital of France is",
    "Explain possible future trends in artificial intelligence. The future of AI is",
]

sampling_params = {
    "temperature": 0.2,
    "top_p": 0.9,
}

print("\n=== Testing synchronous streaming generation with overlap removal ===\n")

for prompt in prompts:
    print(f"Prompt: {prompt}")
    merged_output = stream_and_merge(llm, prompt, sampling_params)
    print("Generated text:", merged_output)
    print()
```

    
    === Testing synchronous streaming generation with overlap removal ===
    
    Prompt: Write a short, neutral self-introduction for a fictional character. Hello, my name is


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm passionate about [job title] and [job title]. I'm always looking for ways to improve my skills and knowledge, and I'm always eager to learn new things. What's your favorite hobby or activity? I'm a [job title] at [company name], and I'm always looking for ways to improve my skills and knowledge. I'm always eager to learn new things,
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, which is known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a bustling city with a rich cultural heritage and is a major tourist destination. It is also known for its fashion industry and its role in the French Revolution. The city is home to many famous French artists, writers, and musicians, and it is a major center for the arts and culture in France. Paris is a city of contrasts, with its modern architecture and historical landmarks blending together to
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the technology's direction and impact on society. Here are some of the most likely trends:
    
    1. Increased focus on ethical considerations: As AI becomes more integrated into our daily lives, there will be a growing emphasis on ethical considerations. This includes issues such as bias, transparency, accountability, and privacy. AI developers will need to prioritize these concerns in their designs and ensure that their systems are designed to be fair, transparent, and accountable.
    
    2. Integration with human decision-making: AI is likely to become more integrated with human decision-making in the future. This could involve the
    


### Non-streaming Asynchronous Generation


```python
prompts = [
    "Write a short, neutral self-introduction for a fictional character. Hello, my name is",
    "Provide a concise factual statement about France’s capital city. The capital of France is",
    "Explain possible future trends in artificial intelligence. The future of AI is",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95}

print("\n=== Testing asynchronous batch generation ===")


async def main():
    outputs = await llm.async_generate(prompts, sampling_params)

    for prompt, output in zip(prompts, outputs):
        print(f"\nPrompt: {prompt}")
        print(f"Generated text: {output['text']}")


asyncio.run(main())
```

    
    === Testing asynchronous batch generation ===


    
    Prompt: Write a short, neutral self-introduction for a fictional character. Hello, my name is
    Generated text:  [Name], and I'm an [Occupation] from [Location]. I'm currently [Current Position] [Current Company], [Name] is my online presence and I'm here to share my experiences, insights, and opinions on [Topic/Issue]. How can I be of assistance? Please let me know your name and the topic you would like to discuss. I'm here to listen and provide any relevant information you need. Let's get started! #Hello #SelfIntroduction #OnCampus. #OnlineOpinions. #CareerAdvice. #CommunityConnect. #OnlineCommunity. #OnlineDiscussions. #Community. #
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris. It is the largest city in France and the fifth largest city in the world by population. The city is known for its rich history, famous landmarks, and unique cultural traditions. Paris is a major center for art, fashion, music, and gastronomy, and is home to many world-renowned museums, theaters, and concert halls. It is also a hub for business, education, and technology. Paris is a cultural and educational center that has played a significant role in shaping France and the world. In general, the capital is a lively and vibrant city with a strong sense of French identity and culture. It is known for its
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  bright and promising, and it is likely to continue to evolve in many exciting ways. Here are some potential trends to watch out for in the next few years:
    
    1. More personalized experiences: As AI continues to develop, we can expect to see more personalized experiences in our daily lives. AI systems will be able to learn from our behavior and preferences, allowing us to tailor our interactions with the world to our individual needs and preferences.
    
    2. Greater integration with natural language processing: As AI becomes more integrated into our daily lives, we can expect to see more natural language processing in its applications. This will allow AI systems to understand and respond to


### Streaming Asynchronous Generation


```python
prompts = [
    "Write a short, neutral self-introduction for a fictional character. Hello, my name is",
    "Provide a concise factual statement about France’s capital city. The capital of France is",
    "Explain possible future trends in artificial intelligence. The future of AI is",
]

sampling_params = {"temperature": 0.8, "top_p": 0.95}

print("\n=== Testing asynchronous streaming generation (no repeats) ===")


async def main():
    for prompt in prompts:
        print(f"\nPrompt: {prompt}")
        print("Generated text: ", end="", flush=True)

        # Replace direct calls to async_generate with our custom overlap-aware version
        async for cleaned_chunk in async_stream_and_merge(llm, prompt, sampling_params):
            print(cleaned_chunk, end="", flush=True)

        print()  # New line after each prompt


asyncio.run(main())
```

    
    === Testing asynchronous streaming generation (no repeats) ===
    
    Prompt: Write a short, neutral self-introduction for a fictional character. Hello, my name is
    Generated text: 

     [

    Name

    ],

     and

     I

    'm

     a

    /an

     [

    Occup

    ation

    ].

     I

    've

     always

     loved

     [

    Occup

    ation

    ]

     since

     I

     was

     a

     child

    ,

     and

     I

    've

     always

     been

     fascinated

     by

     the

     world

     around

     me

    .

     I

    've

     traveled

     the

     world

     and

     studied

     history

     and

     literature

    ,

     and

     I

    've

     become

     very

     knowledgeable

     about

     the

     subject

    .

     I

    'm

     passionate

     about

     sharing

     my

     knowledge

     with

     others

     and

     helping

     them

     to

     understand

     and

     appreciate

     the

     beauty

     of

     [

    Occup

    ation

    ].

     I

     love

     to

     read

    ,

     write

    ,

     and

     travel

    ,

     and

     I

     hope

     to

     use

     my

     experiences

     and

     knowledge

     to

     help

     others

     on

     their

     journeys

    .

     I

    'm

     excited

     to

     learn

     more

     about

     [

    Occup

    ation

    ],

     and

     I

    'm

     here

     to

     help

     anyone

     who

    's

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     known

     for

     its

     rich

     history

     and

     beautiful

     architecture

    ,

     including

     the

     iconic

     E

    iff

    el

     Tower

    .

     
    


    The

     French

     capital

     has

     a

     rich

     and

     fascinating

     history

     dating

     back

     to

     the

     Roman

     period

    ,

     and

     is

     now

     the

     largest

     city

     in

     Europe

    .

     It

     is

     home

     to

     the

     iconic

     Notre

     Dame

     Cathedral

    ,

     as

     well

     as

     many

     other

     historical

     sites

     and

     attractions

    .
    


    Paris

     is

     a

     vibrant

     and

     multicultural

     city

     with

     a

     rich

     culture

    ,

     including

     the

     famous

     E

    iff

    el

     Tower

    ,

     the

     Lou

    vre

     Museum

    ,

     and

     the

     many

     Paris

    ian

     restaurants

     and

     cafes

    .

     It

     is

     also

     a

     hub

     for

     education

     and

     culture

    ,

     with

     numerous

     museums

     and

     galleries

    ,

     as

     well

     as

     numerous

     universities

     and

     cultural

     institutions

    .
    


    The

     French

     capital

     is

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     be

     shaped

     by

     several

     key

     trends

    ,

     including

    :
    


     

     

    1

    .

     Increased

     use

     of

     AI

     in

     healthcare

    :

     AI

     is

     increasingly

     being

     used

     in

     healthcare

     to

     improve

     diagnosis

    ,

     treatment

    ,

     and

     patient

     care

    .

     This

     could

     lead

     to

     more

     accurate

     diagnoses

    ,

     personalized

     treatment

     plans

    ,

     and

     better

     patient

     outcomes

    .


     

     

    2

    .

     Autonomous

     vehicles

    :

     The

     rapid

     development

     of

     AI

    -driven

     autonomous

     vehicles

     is

     expected

     to

     become

     more

     common

     in

     the

     future

    .

     These

     vehicles

     could

     be

     used

     for

     both

     transportation

     and

     delivery

    ,

     and

     could

     reduce

     traffic

     congestion

     and

     improve

     safety

    .


     

     

    3

    .

     AI

     in

     manufacturing

    :

     AI

     is

     being

     used

     in

     manufacturing

     to

     improve

     efficiency

    ,

     reduce

     waste

    ,

     and

     improve

     product

     quality

    .

     This

    



```python
llm.shutdown()
```
