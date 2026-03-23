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

    [2026-03-23 04:21:22] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-23 04:21:22] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-23 04:21:22] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-23 04:21:25] WARNING model_config.py:1098: Transformers version 5.3.0 is used for model type qwen2. If you experience issues related to RoPE parameters, they may be due to incompatibilities between Transformers >=5.0.0 and some models. You can try downgrading to transformers==4.57.1 as a workaround.


    [2026-03-23 04:21:25] INFO server_args.py:2239: Attention backend not specified. Use fa3 backend by default.


    [2026-03-23 04:21:25] INFO server_args.py:3522: Set soft_watchdog_timeout since in CI


    [2026-03-23 04:21:25] INFO engine.py:177: server_args=ServerArgs(model_path='qwen/qwen2.5-0.5b-instruct', tokenizer_path='qwen/qwen2.5-0.5b-instruct', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=30000, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.903, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=879560528, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='error', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='qwen/qwen2.5-0.5b-instruct', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser=None, tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_match_window_size=1, speculative_ngram_max_match_window_size=12, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_branch_length=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', disable_hicache_numa_detect=False, hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, hierarchical_sparse_attention_extra_config=None, enable_lmcache=False, kt_weight_path=None, kt_method=None, kt_cpuinfer=None, kt_threadpool_count=None, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4, 8, 12, 16, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96, 104, 112, 120, 128, 136, 144, 152, 160, 168, 176, 184, 192, 200, 208, 216, 224, 232, 240, 248, 256], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, mm_max_concurrent_calls=32, mm_per_request_timeout=10.0, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.10it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.09it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:44,  2.89s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:44,  2.89s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:44,  2.89s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:44,  2.89s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:03<00:31,  1.72it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:03<00:31,  1.72it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:03<00:31,  1.72it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:03<00:31,  1.72it/s]

    Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:03<00:31,  1.72it/s]Compiling num tokens (num_tokens=4608):  14%|█▍        | 8/58 [00:03<00:12,  4.09it/s]Compiling num tokens (num_tokens=4096):  14%|█▍        | 8/58 [00:03<00:12,  4.09it/s]Compiling num tokens (num_tokens=3840):  14%|█▍        | 8/58 [00:03<00:12,  4.09it/s]Compiling num tokens (num_tokens=3584):  14%|█▍        | 8/58 [00:03<00:12,  4.09it/s]Compiling num tokens (num_tokens=3328):  14%|█▍        | 8/58 [00:03<00:12,  4.09it/s]Compiling num tokens (num_tokens=3072):  14%|█▍        | 8/58 [00:03<00:12,  4.09it/s]Compiling num tokens (num_tokens=2816):  14%|█▍        | 8/58 [00:03<00:12,  4.09it/s]Compiling num tokens (num_tokens=2816):  24%|██▍       | 14/58 [00:03<00:05,  8.58it/s]Compiling num tokens (num_tokens=2560):  24%|██▍       | 14/58 [00:03<00:05,  8.58it/s]Compiling num tokens (num_tokens=2304):  24%|██▍       | 14/58 [00:03<00:05,  8.58it/s]Compiling num tokens (num_tokens=2048):  24%|██▍       | 14/58 [00:03<00:05,  8.58it/s]Compiling num tokens (num_tokens=1792):  24%|██▍       | 14/58 [00:03<00:05,  8.58it/s]Compiling num tokens (num_tokens=1536):  24%|██▍       | 14/58 [00:03<00:05,  8.58it/s]

    Compiling num tokens (num_tokens=1280):  24%|██▍       | 14/58 [00:03<00:05,  8.58it/s]Compiling num tokens (num_tokens=1024):  24%|██▍       | 14/58 [00:03<00:05,  8.58it/s]Compiling num tokens (num_tokens=1024):  36%|███▌      | 21/58 [00:03<00:02, 14.71it/s]Compiling num tokens (num_tokens=960):  36%|███▌      | 21/58 [00:03<00:02, 14.71it/s] Compiling num tokens (num_tokens=896):  36%|███▌      | 21/58 [00:03<00:02, 14.71it/s]Compiling num tokens (num_tokens=832):  36%|███▌      | 21/58 [00:03<00:02, 14.71it/s]Compiling num tokens (num_tokens=768):  36%|███▌      | 21/58 [00:03<00:02, 14.71it/s]Compiling num tokens (num_tokens=704):  36%|███▌      | 21/58 [00:03<00:02, 14.71it/s]Compiling num tokens (num_tokens=640):  36%|███▌      | 21/58 [00:03<00:02, 14.71it/s]Compiling num tokens (num_tokens=640):  47%|████▋     | 27/58 [00:03<00:01, 20.42it/s]Compiling num tokens (num_tokens=576):  47%|████▋     | 27/58 [00:03<00:01, 20.42it/s]Compiling num tokens (num_tokens=512):  47%|████▋     | 27/58 [00:03<00:01, 20.42it/s]Compiling num tokens (num_tokens=480):  47%|████▋     | 27/58 [00:03<00:01, 20.42it/s]Compiling num tokens (num_tokens=448):  47%|████▋     | 27/58 [00:03<00:01, 20.42it/s]

    Compiling num tokens (num_tokens=416):  47%|████▋     | 27/58 [00:03<00:01, 20.42it/s]Compiling num tokens (num_tokens=384):  47%|████▋     | 27/58 [00:03<00:01, 20.42it/s]Compiling num tokens (num_tokens=384):  57%|█████▋    | 33/58 [00:03<00:00, 26.48it/s]Compiling num tokens (num_tokens=352):  57%|█████▋    | 33/58 [00:03<00:00, 26.48it/s]Compiling num tokens (num_tokens=320):  57%|█████▋    | 33/58 [00:03<00:00, 26.48it/s]Compiling num tokens (num_tokens=288):  57%|█████▋    | 33/58 [00:03<00:00, 26.48it/s]Compiling num tokens (num_tokens=256):  57%|█████▋    | 33/58 [00:03<00:00, 26.48it/s]Compiling num tokens (num_tokens=240):  57%|█████▋    | 33/58 [00:03<00:00, 26.48it/s]Compiling num tokens (num_tokens=224):  57%|█████▋    | 33/58 [00:03<00:00, 26.48it/s]Compiling num tokens (num_tokens=224):  67%|██████▋   | 39/58 [00:03<00:00, 31.31it/s]Compiling num tokens (num_tokens=208):  67%|██████▋   | 39/58 [00:03<00:00, 31.31it/s]Compiling num tokens (num_tokens=192):  67%|██████▋   | 39/58 [00:03<00:00, 31.31it/s]Compiling num tokens (num_tokens=176):  67%|██████▋   | 39/58 [00:03<00:00, 31.31it/s]

    Compiling num tokens (num_tokens=160):  67%|██████▋   | 39/58 [00:03<00:00, 31.31it/s]Compiling num tokens (num_tokens=144):  67%|██████▋   | 39/58 [00:03<00:00, 31.31it/s]Compiling num tokens (num_tokens=128):  67%|██████▋   | 39/58 [00:03<00:00, 31.31it/s]Compiling num tokens (num_tokens=128):  78%|███████▊  | 45/58 [00:03<00:00, 35.98it/s]Compiling num tokens (num_tokens=112):  78%|███████▊  | 45/58 [00:03<00:00, 35.98it/s]Compiling num tokens (num_tokens=96):  78%|███████▊  | 45/58 [00:03<00:00, 35.98it/s] Compiling num tokens (num_tokens=80):  78%|███████▊  | 45/58 [00:03<00:00, 35.98it/s]Compiling num tokens (num_tokens=64):  78%|███████▊  | 45/58 [00:03<00:00, 35.98it/s]Compiling num tokens (num_tokens=48):  78%|███████▊  | 45/58 [00:03<00:00, 35.98it/s]Compiling num tokens (num_tokens=32):  78%|███████▊  | 45/58 [00:03<00:00, 35.98it/s]Compiling num tokens (num_tokens=32):  88%|████████▊ | 51/58 [00:03<00:00, 39.66it/s]Compiling num tokens (num_tokens=28):  88%|████████▊ | 51/58 [00:03<00:00, 39.66it/s]Compiling num tokens (num_tokens=24):  88%|████████▊ | 51/58 [00:03<00:00, 39.66it/s]

    Compiling num tokens (num_tokens=20):  88%|████████▊ | 51/58 [00:03<00:00, 39.66it/s]Compiling num tokens (num_tokens=16):  88%|████████▊ | 51/58 [00:03<00:00, 39.66it/s]Compiling num tokens (num_tokens=12):  88%|████████▊ | 51/58 [00:03<00:00, 39.66it/s]Compiling num tokens (num_tokens=8):  88%|████████▊ | 51/58 [00:03<00:00, 39.66it/s] Compiling num tokens (num_tokens=4):  88%|████████▊ | 51/58 [00:03<00:00, 39.66it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 14.53it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=118.71 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.68 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=118.68 GB):   3%|▎         | 2/58 [00:00<00:04, 11.88it/s]Capturing num tokens (num_tokens=7168 avail_mem=118.67 GB):   3%|▎         | 2/58 [00:00<00:04, 11.88it/s]

    Capturing num tokens (num_tokens=6656 avail_mem=118.66 GB):   3%|▎         | 2/58 [00:00<00:04, 11.88it/s]Capturing num tokens (num_tokens=6656 avail_mem=118.66 GB):   7%|▋         | 4/58 [00:00<00:04, 12.94it/s]Capturing num tokens (num_tokens=6144 avail_mem=118.66 GB):   7%|▋         | 4/58 [00:00<00:04, 12.94it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.65 GB):   7%|▋         | 4/58 [00:00<00:04, 12.94it/s]Capturing num tokens (num_tokens=5632 avail_mem=118.65 GB):  10%|█         | 6/58 [00:00<00:03, 14.51it/s]Capturing num tokens (num_tokens=5120 avail_mem=118.56 GB):  10%|█         | 6/58 [00:00<00:03, 14.51it/s]

    Capturing num tokens (num_tokens=4608 avail_mem=118.56 GB):  10%|█         | 6/58 [00:00<00:03, 14.51it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.58 GB):  10%|█         | 6/58 [00:00<00:03, 14.51it/s]Capturing num tokens (num_tokens=4096 avail_mem=118.58 GB):  16%|█▌        | 9/58 [00:00<00:02, 17.49it/s]Capturing num tokens (num_tokens=3840 avail_mem=118.65 GB):  16%|█▌        | 9/58 [00:00<00:02, 17.49it/s]Capturing num tokens (num_tokens=3584 avail_mem=118.62 GB):  16%|█▌        | 9/58 [00:00<00:02, 17.49it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.57 GB):  16%|█▌        | 9/58 [00:00<00:02, 17.49it/s]Capturing num tokens (num_tokens=3328 avail_mem=118.57 GB):  21%|██        | 12/58 [00:00<00:02, 20.59it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.59 GB):  21%|██        | 12/58 [00:00<00:02, 20.59it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=118.59 GB):  21%|██        | 12/58 [00:00<00:02, 20.59it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.58 GB):  21%|██        | 12/58 [00:00<00:02, 20.59it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.58 GB):  26%|██▌       | 15/58 [00:00<00:02, 19.80it/s]Capturing num tokens (num_tokens=2304 avail_mem=117.44 GB):  26%|██▌       | 15/58 [00:00<00:02, 19.80it/s]

    Capturing num tokens (num_tokens=2048 avail_mem=117.41 GB):  26%|██▌       | 15/58 [00:00<00:02, 19.80it/s]Capturing num tokens (num_tokens=1792 avail_mem=117.40 GB):  26%|██▌       | 15/58 [00:01<00:02, 19.80it/s]Capturing num tokens (num_tokens=1792 avail_mem=117.40 GB):  31%|███       | 18/58 [00:01<00:02, 14.75it/s]Capturing num tokens (num_tokens=1536 avail_mem=117.40 GB):  31%|███       | 18/58 [00:01<00:02, 14.75it/s]

    Capturing num tokens (num_tokens=1280 avail_mem=117.41 GB):  31%|███       | 18/58 [00:01<00:02, 14.75it/s]Capturing num tokens (num_tokens=1280 avail_mem=117.41 GB):  34%|███▍      | 20/58 [00:01<00:02, 13.77it/s]Capturing num tokens (num_tokens=1024 avail_mem=117.38 GB):  34%|███▍      | 20/58 [00:01<00:02, 13.77it/s]Capturing num tokens (num_tokens=960 avail_mem=117.39 GB):  34%|███▍      | 20/58 [00:01<00:02, 13.77it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=117.39 GB):  38%|███▊      | 22/58 [00:01<00:02, 12.70it/s]Capturing num tokens (num_tokens=896 avail_mem=117.38 GB):  38%|███▊      | 22/58 [00:01<00:02, 12.70it/s]Capturing num tokens (num_tokens=832 avail_mem=117.38 GB):  38%|███▊      | 22/58 [00:01<00:02, 12.70it/s]Capturing num tokens (num_tokens=832 avail_mem=117.38 GB):  41%|████▏     | 24/58 [00:01<00:02, 12.26it/s]Capturing num tokens (num_tokens=768 avail_mem=117.39 GB):  41%|████▏     | 24/58 [00:01<00:02, 12.26it/s]

    Capturing num tokens (num_tokens=704 avail_mem=117.38 GB):  41%|████▏     | 24/58 [00:01<00:02, 12.26it/s]Capturing num tokens (num_tokens=704 avail_mem=117.38 GB):  45%|████▍     | 26/58 [00:01<00:02, 12.08it/s]Capturing num tokens (num_tokens=640 avail_mem=117.35 GB):  45%|████▍     | 26/58 [00:01<00:02, 12.08it/s]Capturing num tokens (num_tokens=576 avail_mem=117.37 GB):  45%|████▍     | 26/58 [00:01<00:02, 12.08it/s]

    Capturing num tokens (num_tokens=576 avail_mem=117.37 GB):  48%|████▊     | 28/58 [00:02<00:02, 11.84it/s]Capturing num tokens (num_tokens=512 avail_mem=117.34 GB):  48%|████▊     | 28/58 [00:02<00:02, 11.84it/s]Capturing num tokens (num_tokens=480 avail_mem=117.37 GB):  48%|████▊     | 28/58 [00:02<00:02, 11.84it/s]Capturing num tokens (num_tokens=480 avail_mem=117.37 GB):  52%|█████▏    | 30/58 [00:02<00:02, 11.53it/s]Capturing num tokens (num_tokens=448 avail_mem=117.37 GB):  52%|█████▏    | 30/58 [00:02<00:02, 11.53it/s]

    Capturing num tokens (num_tokens=416 avail_mem=117.36 GB):  52%|█████▏    | 30/58 [00:02<00:02, 11.53it/s]Capturing num tokens (num_tokens=416 avail_mem=117.36 GB):  55%|█████▌    | 32/58 [00:02<00:02, 11.48it/s]Capturing num tokens (num_tokens=384 avail_mem=117.36 GB):  55%|█████▌    | 32/58 [00:02<00:02, 11.48it/s]Capturing num tokens (num_tokens=352 avail_mem=117.35 GB):  55%|█████▌    | 32/58 [00:02<00:02, 11.48it/s]

    Capturing num tokens (num_tokens=352 avail_mem=117.35 GB):  59%|█████▊    | 34/58 [00:02<00:02, 11.70it/s]Capturing num tokens (num_tokens=320 avail_mem=117.32 GB):  59%|█████▊    | 34/58 [00:02<00:02, 11.70it/s]Capturing num tokens (num_tokens=288 avail_mem=117.33 GB):  59%|█████▊    | 34/58 [00:02<00:02, 11.70it/s]Capturing num tokens (num_tokens=288 avail_mem=117.33 GB):  62%|██████▏   | 36/58 [00:02<00:01, 11.68it/s]Capturing num tokens (num_tokens=256 avail_mem=117.33 GB):  62%|██████▏   | 36/58 [00:02<00:01, 11.68it/s]

    Capturing num tokens (num_tokens=240 avail_mem=117.33 GB):  62%|██████▏   | 36/58 [00:02<00:01, 11.68it/s]Capturing num tokens (num_tokens=240 avail_mem=117.33 GB):  66%|██████▌   | 38/58 [00:02<00:01, 11.65it/s]Capturing num tokens (num_tokens=224 avail_mem=117.32 GB):  66%|██████▌   | 38/58 [00:02<00:01, 11.65it/s]Capturing num tokens (num_tokens=208 avail_mem=117.31 GB):  66%|██████▌   | 38/58 [00:02<00:01, 11.65it/s]

    Capturing num tokens (num_tokens=208 avail_mem=117.31 GB):  69%|██████▉   | 40/58 [00:03<00:01, 11.81it/s]Capturing num tokens (num_tokens=192 avail_mem=117.31 GB):  69%|██████▉   | 40/58 [00:03<00:01, 11.81it/s]Capturing num tokens (num_tokens=176 avail_mem=117.30 GB):  69%|██████▉   | 40/58 [00:03<00:01, 11.81it/s]Capturing num tokens (num_tokens=176 avail_mem=117.30 GB):  72%|███████▏  | 42/58 [00:03<00:01, 11.45it/s]Capturing num tokens (num_tokens=160 avail_mem=117.29 GB):  72%|███████▏  | 42/58 [00:03<00:01, 11.45it/s]

    Capturing num tokens (num_tokens=144 avail_mem=117.28 GB):  72%|███████▏  | 42/58 [00:03<00:01, 11.45it/s]Capturing num tokens (num_tokens=144 avail_mem=117.28 GB):  76%|███████▌  | 44/58 [00:03<00:01, 11.77it/s]Capturing num tokens (num_tokens=128 avail_mem=117.28 GB):  76%|███████▌  | 44/58 [00:03<00:01, 11.77it/s]Capturing num tokens (num_tokens=112 avail_mem=117.27 GB):  76%|███████▌  | 44/58 [00:03<00:01, 11.77it/s]

    Capturing num tokens (num_tokens=112 avail_mem=117.27 GB):  79%|███████▉  | 46/58 [00:03<00:01, 11.95it/s]Capturing num tokens (num_tokens=96 avail_mem=117.26 GB):  79%|███████▉  | 46/58 [00:03<00:01, 11.95it/s] Capturing num tokens (num_tokens=80 avail_mem=117.26 GB):  79%|███████▉  | 46/58 [00:03<00:01, 11.95it/s]Capturing num tokens (num_tokens=80 avail_mem=117.26 GB):  83%|████████▎ | 48/58 [00:03<00:00, 12.64it/s]Capturing num tokens (num_tokens=64 avail_mem=117.25 GB):  83%|████████▎ | 48/58 [00:03<00:00, 12.64it/s]

    Capturing num tokens (num_tokens=48 avail_mem=117.24 GB):  83%|████████▎ | 48/58 [00:03<00:00, 12.64it/s]Capturing num tokens (num_tokens=48 avail_mem=117.24 GB):  86%|████████▌ | 50/58 [00:03<00:00, 13.19it/s]Capturing num tokens (num_tokens=32 avail_mem=117.24 GB):  86%|████████▌ | 50/58 [00:03<00:00, 13.19it/s]Capturing num tokens (num_tokens=28 avail_mem=117.23 GB):  86%|████████▌ | 50/58 [00:03<00:00, 13.19it/s]Capturing num tokens (num_tokens=28 avail_mem=117.23 GB):  90%|████████▉ | 52/58 [00:03<00:00, 13.72it/s]Capturing num tokens (num_tokens=24 avail_mem=117.23 GB):  90%|████████▉ | 52/58 [00:03<00:00, 13.72it/s]

    Capturing num tokens (num_tokens=20 avail_mem=117.23 GB):  90%|████████▉ | 52/58 [00:04<00:00, 13.72it/s]Capturing num tokens (num_tokens=16 avail_mem=117.23 GB):  90%|████████▉ | 52/58 [00:04<00:00, 13.72it/s]Capturing num tokens (num_tokens=16 avail_mem=117.23 GB):  95%|█████████▍| 55/58 [00:04<00:00, 16.62it/s]Capturing num tokens (num_tokens=12 avail_mem=117.22 GB):  95%|█████████▍| 55/58 [00:04<00:00, 16.62it/s]Capturing num tokens (num_tokens=8 avail_mem=117.22 GB):  95%|█████████▍| 55/58 [00:04<00:00, 16.62it/s] Capturing num tokens (num_tokens=4 avail_mem=117.22 GB):  95%|█████████▍| 55/58 [00:04<00:00, 16.62it/s]Capturing num tokens (num_tokens=4 avail_mem=117.22 GB): 100%|██████████| 58/58 [00:04<00:00, 13.84it/s]


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
    Generated text:  Maddy, and I’m a senior at New York University. I love reading, writing, and watching reality TV. I just started writing my first book, and I’m excited to tell the stories of the people who make up my world. I’m interested in how real-life events affect people’s lives and how they affect people’s lives. I’m also a huge animal lover, and I love to participate in community service projects like volunteering at animal shelters and animal welfare organizations. What are some of your favorite hobbies and activities, and what makes you passionate about them?
    As a senior at NYU, I have a variety of hobbies and
    ===============================
    Prompt: The president of the United States is
    Generated text:  an elected position, which means he is elected for a certain term of 5 years. The vice president is also an elected position, which is a different term than the president, elected for 4 years. If each position has a unique election cycle, what is the difference between the term lengths of the president and the vice president? To determine the difference between the term lengths of the president and the vice president, we need to follow these steps:
    
    1. Identify the term length of the president.
    2. Identify the term length of the vice president.
    3. Calculate the difference between the two term lengths.
    
    The term length of the president
    ===============================
    Prompt: The capital of France is
    Generated text:  Paris. The population of Paris is around 2.3 million, the largest in the world. Its heart is the Eiffel Tower. It was built in 1889.
    The Eiffel Tower is the tallest man-made structure in the world. It is 324.5m high. It was originally a clock tower. It was built in 1889.
    The Eiffel Tower is located in the Paris, a city famous for the view. The view was enhanced by the tower during the 1889 World's Fair. The 1889 World's Fair was
    ===============================
    Prompt: The future of AI is
    Generated text:  bright, and the potential to revolutionize industries is immense. The field is constantly evolving, with new technologies emerging every day. The field of AI has a long history and has seen many significant advancements in recent years. AI has already been used in a variety of industries, including healthcare, finance, and transportation. In addition, it is still developing and evolving rapidly, so there is still a lot of room for improvement and innovation. As such, AI is poised for significant growth and impact in the coming years. As technology continues to advance, AI will play an increasingly important role in shaping the future of work, society, and our interactions with


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about your interests and experiences. Let's chat! [Name] [Job Title] [Company Name] [Company Address] [City, State, Zip Code] [Phone Number] [Email Address] [LinkedIn Profile] [Twitter Profile] [Facebook Profile] [GitHub Profile] [Instagram Profile] [Twitter Profile] [Facebook Profile] [GitHub Profile] [Instagram Profile] [LinkedIn Profile] [Twitter Profile] [Facebook Profile] [GitHub Profile] [Instagram Profile] [LinkedIn Profile] [Twitter
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also a major center for French culture, cuisine, and fashion. Paris is a cosmopolitan city with a rich history dating back to the Roman Empire and the French Revolution. It is home to many famous museums, including the Louvre, the Musée d'Orsay, and the Musée Rodin. The city is also known for its vibrant nightlife, with many bars, clubs, and restaurants. Paris is a city of contrasts, with its modern architecture and historical landmarks blending seamlessly into
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by rapid advancements in areas such as machine learning, natural language processing, and computer vision. Some potential future trends include:
    
    1. Increased integration of AI into everyday life: AI is already being integrated into our daily lives through devices such as smartphones, smart homes, and self-driving cars. As AI continues to advance, we can expect to see even more integration into our daily routines.
    
    2. AI becoming more autonomous: As AI becomes more advanced, we can expect to see more autonomous vehicles on the road, with AI taking over tasks such as driving, navigation, and even decision-making.
    
    3. AI becoming more ethical:
    


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
    Generated text:  [Name], and I'm a [job title] at [company name]. My greatest strength is [strength], and I'm always looking for ways to [strength]. I have an interesting personality, and I enjoy [interest] with [person]. I'm passionate about [interest] and I always aim to [improve]. I'm a [state] citizen and I love to [activity]. How can someone tell me more about me?
    Certainly! Here’s a neutral self-introduction tailored to your character:
    
    ---
    
    Good day, everyone! My name is [Name], and I am [Name]. I'm a [job title]
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, also known as “La Ville-Joyeuse,” the most popular city in the country. It is located in the central region of France and is the largest city in both the country and the world. Paris is the cultural, political, and economic center of France. The city is home to iconic landmarks such as Notre-Dame Cathedral, the Eiffel Tower, and the Palace of Versailles. It is also the birthplace of the French Revolution and the inspiration for the phrase “la vie dans l’art” or “life in art.” Paris is known for its bustling streets, vibrant culture, and delicious cuisine.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be shaped by a number of different trends and developments. Here are some possible trends that could shape AI in the coming years:
    
    1. Increased use of AI in healthcare: AI can be used to improve the accuracy of diagnoses, predict the outcome of medical procedures, and assist in the prevention of diseases. This could lead to more effective and efficient healthcare systems, potentially reducing costs and improving patient outcomes.
    
    2. Advancements in natural language processing: AI is already being used to understand and generate natural language, but this trend is expected to continue. AI can be used to build chatbots and virtual assistants that can answer questions and provide information


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

    ]

     and

     I

     am

     [

    Age

    ]

     years

     old

    .

     I

     have

     [

    Description

     of

     your

     profession

     or

     skill

    ]

     and

     [

    Optional

    :

     Education

     or

     hobbies

    ].

     In

     my

     free

     time

    ,

     I

     enjoy

     [

    Your

     Favorite

     Past

    ime

     or

     Hobby

    ].

     I

     am

     excited

     to

     meet

     you

     and

     have

     a

     fun

     chat

    !

     

    📸

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    ✨

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .
    


    Paris

     is

     the

     capital

     city

     of

     France

     and

     is

     the

     largest

     city

     in

     Europe

    .

     It

     is

     known

     for

     its

     historical

     architecture

    ,

     vibrant

     nightlife

    ,

     and

     rich

     cultural

     scene

    .

     The

     city

     is

     located

     on

     the

     Se

    ine

     River

     and

     is

     home

     to

     many

     museums

    ,

     theaters

    ,

     and

     other

     cultural

     attractions

    .

     Paris

     has

     a

     large

     and

     diverse

     population

     of

     over

     

    1

    0

     million

     residents

     and

     is

     known

     for

     its

     culinary

     traditions

    ,

     including

     famous

     dishes

     like

     cro

    iss

    ants

     and

     past

    ries

    .

     The

     city

     is

     also

     home

     to

     many

     international

     organizations

     and

     events

    ,

     making

     it

     a

     popular

     destination

     for

     tourists

     and

     locals

     alike

    .

     
    


    Paris

     is

     also

     known

     for

     its

     annual

     E

    iff

    el

     Tower

     spectacle

    ,

     which

     is

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     bright

    ,

     and

     there

     are

     a

     number

     of

     trends

     that

     are

     likely

     to

     shape

     the

     technology

     in

     the

     coming

     years

    .

     Here

     are

     some

     of

     the

     key

     trends

     that

     experts

     predict

     will

     shape

     AI

     in

     the

     years

     to

     come

    :
    


    1

    .

     Increased

     specialization

     and

     expertise

    :

     AI

     is

     already

     becoming

     more

     sophisticated

    ,

     and

     experts

     will

     be

     required

     to

     specialize

     in

     certain

     areas

     of

     AI

     to

     be

     effective

    .

     This

     will

     result

     in

     a

     more

     specialized

     AI

     market

    ,

     with

     a

     growing

     demand

     for

     AI

     professionals

     in

     fields

     such

     as

     data

     science

    ,

     machine

     learning

    ,

     and

     computer

     vision

    .
    


    2

    .

     Enhanced

     privacy

     and

     security

    :

     With

     the

     increasing

     amount

     of

     personal

     data

     being

     collected

     and

     shared

     online

    ,

     there

     is

     a

     growing

     need

     for

     AI

    



```python
llm.shutdown()
```
