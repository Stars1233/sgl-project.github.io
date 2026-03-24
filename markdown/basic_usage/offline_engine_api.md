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

    [2026-03-24 04:33:19] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-24 04:33:19] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-24 04:33:19] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-24 04:33:22] INFO server_args.py:2240: Attention backend not specified. Use fa3 backend by default.


    [2026-03-24 04:33:22] INFO server_args.py:3523: Set soft_watchdog_timeout since in CI


    [2026-03-24 04:33:22] INFO engine.py:177: server_args=ServerArgs(model_path='qwen/qwen2.5-0.5b-instruct', tokenizer_path='qwen/qwen2.5-0.5b-instruct', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=30000, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.903, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=874015730, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='error', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='qwen/qwen2.5-0.5b-instruct', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser=None, tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_match_window_size=1, speculative_ngram_max_match_window_size=12, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_max_trie_depth=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', disable_hicache_numa_detect=False, hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, enable_hisparse=False, hisparse_config=None, enable_lmcache=False, kt_weight_path=None, kt_method=None, kt_cpuinfer=None, kt_threadpool_count=None, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4, 8, 12, 16, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96, 104, 112, 120, 128, 136, 144, 152, 160, 168, 176, 184, 192, 200, 208, 216, 224, 232, 240, 248, 256], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, mm_max_concurrent_calls=32, mm_per_request_timeout=10.0, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-24 04:33:31.624 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 04:33:31] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 04:33:31.624 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 04:33:31] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 04:33:31.624 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 04:33:31] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 04:33:31.624 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 04:33:31] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 04:33:31.624 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 04:33:31] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]

    Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.48it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  2.48it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:42,  2.85s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:42,  2.85s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:42,  2.85s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:42,  2.85s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:30,  1.76it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:30,  1.76it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:30,  1.76it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:30,  1.76it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:03<00:30,  1.76it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:03<00:30,  1.76it/s]

    Compiling num tokens (num_tokens=4096):  16%|█▌        | 9/58 [00:03<00:10,  4.83it/s]Compiling num tokens (num_tokens=3840):  16%|█▌        | 9/58 [00:03<00:10,  4.83it/s]Compiling num tokens (num_tokens=3584):  16%|█▌        | 9/58 [00:03<00:10,  4.83it/s]Compiling num tokens (num_tokens=3328):  16%|█▌        | 9/58 [00:03<00:10,  4.83it/s]Compiling num tokens (num_tokens=3072):  16%|█▌        | 9/58 [00:03<00:10,  4.83it/s]Compiling num tokens (num_tokens=2816):  16%|█▌        | 9/58 [00:03<00:10,  4.83it/s]Compiling num tokens (num_tokens=2560):  16%|█▌        | 9/58 [00:03<00:10,  4.83it/s]Compiling num tokens (num_tokens=2304):  16%|█▌        | 9/58 [00:03<00:10,  4.83it/s]Compiling num tokens (num_tokens=2304):  28%|██▊       | 16/58 [00:03<00:04, 10.22it/s]Compiling num tokens (num_tokens=2048):  28%|██▊       | 16/58 [00:03<00:04, 10.22it/s]Compiling num tokens (num_tokens=1792):  28%|██▊       | 16/58 [00:03<00:04, 10.22it/s]Compiling num tokens (num_tokens=1536):  28%|██▊       | 16/58 [00:03<00:04, 10.22it/s]Compiling num tokens (num_tokens=1280):  28%|██▊       | 16/58 [00:03<00:04, 10.22it/s]Compiling num tokens (num_tokens=1024):  28%|██▊       | 16/58 [00:03<00:04, 10.22it/s]Compiling num tokens (num_tokens=960):  28%|██▊       | 16/58 [00:03<00:04, 10.22it/s] 

    Compiling num tokens (num_tokens=960):  38%|███▊      | 22/58 [00:03<00:02, 15.47it/s]Compiling num tokens (num_tokens=896):  38%|███▊      | 22/58 [00:03<00:02, 15.47it/s]Compiling num tokens (num_tokens=832):  38%|███▊      | 22/58 [00:03<00:02, 15.47it/s]Compiling num tokens (num_tokens=768):  38%|███▊      | 22/58 [00:03<00:02, 15.47it/s]Compiling num tokens (num_tokens=704):  38%|███▊      | 22/58 [00:03<00:02, 15.47it/s]Compiling num tokens (num_tokens=640):  38%|███▊      | 22/58 [00:03<00:02, 15.47it/s]Compiling num tokens (num_tokens=576):  38%|███▊      | 22/58 [00:03<00:02, 15.47it/s]Compiling num tokens (num_tokens=512):  38%|███▊      | 22/58 [00:03<00:02, 15.47it/s]Compiling num tokens (num_tokens=512):  50%|█████     | 29/58 [00:03<00:01, 22.34it/s]Compiling num tokens (num_tokens=480):  50%|█████     | 29/58 [00:03<00:01, 22.34it/s]Compiling num tokens (num_tokens=448):  50%|█████     | 29/58 [00:03<00:01, 22.34it/s]Compiling num tokens (num_tokens=416):  50%|█████     | 29/58 [00:03<00:01, 22.34it/s]Compiling num tokens (num_tokens=384):  50%|█████     | 29/58 [00:03<00:01, 22.34it/s]Compiling num tokens (num_tokens=352):  50%|█████     | 29/58 [00:03<00:01, 22.34it/s]Compiling num tokens (num_tokens=320):  50%|█████     | 29/58 [00:03<00:01, 22.34it/s]

    Compiling num tokens (num_tokens=288):  50%|█████     | 29/58 [00:03<00:01, 22.34it/s]Compiling num tokens (num_tokens=288):  62%|██████▏   | 36/58 [00:03<00:00, 29.26it/s]Compiling num tokens (num_tokens=256):  62%|██████▏   | 36/58 [00:03<00:00, 29.26it/s]Compiling num tokens (num_tokens=240):  62%|██████▏   | 36/58 [00:03<00:00, 29.26it/s]Compiling num tokens (num_tokens=224):  62%|██████▏   | 36/58 [00:03<00:00, 29.26it/s]Compiling num tokens (num_tokens=208):  62%|██████▏   | 36/58 [00:03<00:00, 29.26it/s]Compiling num tokens (num_tokens=192):  62%|██████▏   | 36/58 [00:03<00:00, 29.26it/s]Compiling num tokens (num_tokens=176):  62%|██████▏   | 36/58 [00:03<00:00, 29.26it/s]Compiling num tokens (num_tokens=176):  72%|███████▏  | 42/58 [00:03<00:00, 33.85it/s]Compiling num tokens (num_tokens=160):  72%|███████▏  | 42/58 [00:03<00:00, 33.85it/s]Compiling num tokens (num_tokens=144):  72%|███████▏  | 42/58 [00:03<00:00, 33.85it/s]Compiling num tokens (num_tokens=128):  72%|███████▏  | 42/58 [00:03<00:00, 33.85it/s]Compiling num tokens (num_tokens=112):  72%|███████▏  | 42/58 [00:03<00:00, 33.85it/s]

    Compiling num tokens (num_tokens=96):  72%|███████▏  | 42/58 [00:03<00:00, 33.85it/s] Compiling num tokens (num_tokens=80):  72%|███████▏  | 42/58 [00:03<00:00, 33.85it/s]Compiling num tokens (num_tokens=80):  83%|████████▎ | 48/58 [00:03<00:00, 37.09it/s]Compiling num tokens (num_tokens=64):  83%|████████▎ | 48/58 [00:03<00:00, 37.09it/s]Compiling num tokens (num_tokens=48):  83%|████████▎ | 48/58 [00:03<00:00, 37.09it/s]Compiling num tokens (num_tokens=32):  83%|████████▎ | 48/58 [00:03<00:00, 37.09it/s]Compiling num tokens (num_tokens=28):  83%|████████▎ | 48/58 [00:03<00:00, 37.09it/s]Compiling num tokens (num_tokens=24):  83%|████████▎ | 48/58 [00:03<00:00, 37.09it/s]Compiling num tokens (num_tokens=20):  83%|████████▎ | 48/58 [00:03<00:00, 37.09it/s]Compiling num tokens (num_tokens=16):  83%|████████▎ | 48/58 [00:03<00:00, 37.09it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:03<00:00, 43.92it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:03<00:00, 43.92it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:03<00:00, 43.92it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:03<00:00, 43.92it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 15.04it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=120.33 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.65it/s]Capturing num tokens (num_tokens=7168 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.65it/s]Capturing num tokens (num_tokens=6656 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.65it/s]Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   3%|▎         | 2/58 [00:00<00:03, 18.65it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=120.30 GB):   9%|▊         | 5/58 [00:00<00:02, 22.13it/s]Capturing num tokens (num_tokens=5632 avail_mem=120.29 GB):   9%|▊         | 5/58 [00:00<00:02, 22.13it/s]Capturing num tokens (num_tokens=5120 avail_mem=120.33 GB):   9%|▊         | 5/58 [00:00<00:02, 22.13it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.62 GB):   9%|▊         | 5/58 [00:00<00:02, 22.13it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.62 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.14it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.01 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.14it/s]Capturing num tokens (num_tokens=3840 avail_mem=119.01 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.14it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.00 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.14it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.00 GB):  14%|█▍        | 8/58 [00:00<00:01, 25.14it/s]

    Capturing num tokens (num_tokens=3328 avail_mem=119.00 GB):  21%|██        | 12/58 [00:00<00:01, 30.12it/s]Capturing num tokens (num_tokens=3072 avail_mem=119.00 GB):  21%|██        | 12/58 [00:00<00:01, 30.12it/s]Capturing num tokens (num_tokens=2816 avail_mem=119.00 GB):  21%|██        | 12/58 [00:00<00:01, 30.12it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.99 GB):  21%|██        | 12/58 [00:00<00:01, 30.12it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.99 GB):  21%|██        | 12/58 [00:00<00:01, 30.12it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.98 GB):  21%|██        | 12/58 [00:00<00:01, 30.12it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.98 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.04it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.98 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.04it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.97 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.04it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.97 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.04it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.95 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.04it/s]

    Capturing num tokens (num_tokens=960 avail_mem=118.96 GB):  29%|██▉       | 17/58 [00:00<00:01, 35.04it/s] Capturing num tokens (num_tokens=960 avail_mem=118.96 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.91it/s]Capturing num tokens (num_tokens=896 avail_mem=118.96 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.91it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.91it/s]Capturing num tokens (num_tokens=768 avail_mem=118.95 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.91it/s]Capturing num tokens (num_tokens=704 avail_mem=118.95 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.91it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  38%|███▊      | 22/58 [00:00<00:00, 37.91it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.21it/s]Capturing num tokens (num_tokens=576 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.21it/s]Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.21it/s]Capturing num tokens (num_tokens=480 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.21it/s]Capturing num tokens (num_tokens=448 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.21it/s]

    Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  47%|████▋     | 27/58 [00:00<00:00, 40.21it/s]Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.70it/s]Capturing num tokens (num_tokens=384 avail_mem=118.94 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.70it/s]Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.70it/s]Capturing num tokens (num_tokens=320 avail_mem=118.94 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.70it/s]Capturing num tokens (num_tokens=288 avail_mem=118.93 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.70it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  55%|█████▌    | 32/58 [00:00<00:00, 41.70it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=224 avail_mem=118.92 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.57it/s]

    Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  64%|██████▍   | 37/58 [00:01<00:00, 42.57it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.47it/s]Capturing num tokens (num_tokens=160 avail_mem=118.91 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.47it/s]Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.47it/s]Capturing num tokens (num_tokens=128 avail_mem=118.91 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.47it/s]Capturing num tokens (num_tokens=112 avail_mem=118.90 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.47it/s]Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  72%|███████▏  | 42/58 [00:01<00:00, 43.47it/s] Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  81%|████████  | 47/58 [00:01<00:00, 43.81it/s]Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  81%|████████  | 47/58 [00:01<00:00, 43.81it/s]Capturing num tokens (num_tokens=64 avail_mem=118.89 GB):  81%|████████  | 47/58 [00:01<00:00, 43.81it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  81%|████████  | 47/58 [00:01<00:00, 43.81it/s]

    Capturing num tokens (num_tokens=32 avail_mem=118.88 GB):  81%|████████  | 47/58 [00:01<00:00, 43.81it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  81%|████████  | 47/58 [00:01<00:00, 43.81it/s]Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.11it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.11it/s]Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.11it/s]Capturing num tokens (num_tokens=16 avail_mem=118.87 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.11it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.11it/s]Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  90%|████████▉ | 52/58 [00:01<00:00, 44.11it/s] Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  98%|█████████▊| 57/58 [00:01<00:00, 44.66it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB):  98%|█████████▊| 57/58 [00:01<00:00, 44.66it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:01<00:00, 39.48it/s]


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
    Generated text:  Michael and I'm an application developer who specializes in creating custom software. I have 5 years of experience in creating custom software, and I have worked on a wide variety of software projects, including web applications, mobile apps, and desktop applications. I'm proficient in various programming languages such as Java, C#, and Python. I also have experience with web technologies such as HTML, CSS, and JavaScript. My goal is to create high-quality software that meets the needs of my clients and improves their productivity.
    What are some specific skills and technologies that Michael has mastered?
    Michael has several specific skills and technologies that he has mastered, including proficiency with
    ===============================
    Prompt: The president of the United States is
    Generated text:  a lifelong office. There are only a few exceptions where the president is elected for one term. He or she must be at least 35 years of age to be eligible for election. There are no restrictions on the person’s political affiliation and there are no restrictions on the person’s views or religion. However, any one of the 50 states may not appoint a president. The candidate must have at least 100,000 votes cast in the state where they win the election. The president-elect (presidential nominee) must have at least 100,000 votes cast in the state where
    ===============================
    Prompt: The capital of France is
    Generated text:  located in which city?
    A. Lyon
    B. Paris
    C. Lyon
    D. Paris
    Answer: B
    
    What is the name of the place where the Ancestral Temple of the Heavenly King is located?
    A. Sanxingdui
    B. Yumen
    C. Daxing
    D. Jiayuguan
    Answer: D
    
    Which of the following entities is the repository for important information such as government regulations, policies, and management regulations?
    A. National Archives
    B. State Council
    C. Municipal People's Government
    D. Civil Affairs Department
    Answer: A
    
    Where is the President of
    ===============================
    Prompt: The future of AI is
    Generated text:  bright, but to achieve it, we must understand how to adapt to it.
    
    What is the purpose of the "AI is bright" sentence? The purpose of the sentence "The future of AI is bright, but to achieve it, we must understand how to adapt to it" is to convey that AI is currently a promising field, but that it is important for us to understand how to adapt to it in order to maximize its potential.
    
    To explain this to a middle school student: Imagine you have a toy box full of fun toys. The toy box is like AI, and the toys are like the new technology or ideas. The sentence


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about your career and interests. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about your career and interests. What can you tell me about yourself? [Name] is a [job title] at [company name]. I'm excited to meet you and learn more about your career and interests. What can you tell me about yourself? [Name] is a [job title] at [company name]. I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French National Library. Paris is a cultural and economic hub, known for its diverse cuisine, fashion, and art scene. It is also home to the world's largest city, with over 10 million people living in the city. Paris is a popular tourist destination, known for its romantic ambiance, historical sites, and cultural events. It is also a major transportation hub, with the Eiffel Tower serving as a landmark for
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some of the most likely trends that could shape the future of AI:
    
    1. Increased automation: As AI becomes more advanced, it is likely to become more capable of performing tasks that were previously done by humans. This could lead to a significant increase in automation, which could have a wide range of implications for employment and the economy.
    
    2. AI ethics and privacy: As AI becomes more integrated into our daily lives, there will be a need to address the ethical and privacy concerns associated with it. This
    


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
    Generated text:  ________ and I'm a software engineer with over ___ years of experience in this field. I am an expert in the development of software applications that handle the user's data and provide them with the desired functionalities. I am constantly learning and evolving my skills to stay ahead of the latest developments in the industry. I have a keen sense of problem-solving and am confident in my ability to deliver high-quality software that meets user needs. I believe in using the latest technologies and tools to create efficient and reliable software applications. I am always willing to explore new ways to improve my skills and contribute to the growth of the industry. Thank you. 
    
    I
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris, known for its elegant architecture, world-renowned museums, and vibrant culture. It is also famous for its iconic Eiffel Tower and its annual City of Lights festival, which brings millions of visitors to the city every year. Paris is a beautiful, historic city with a rich history and continues to be a major center of culture, art, and commerce. 
    
    French fries are a popular dish in Paris, and there are many excellent restaurants to try. The city is also a popular tourist destination, with many attractions, museums, and historical sites to explore. Visitors can enjoy the city’s culture, food, and lively atmosphere in a
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  expected to be highly dynamic and unpredictable, with a wide range of possible trends and developments that can impact the field in significant ways. Here are some of the most likely future trends in AI:
    
    1. Increased integration with human intelligence: One of the most promising areas of AI research is the integration of AI with human intelligence. Researchers are exploring ways to develop AI systems that can learn from human behavior and make decisions based on context and knowledge.
    
    2. Development of advanced cognitive architectures: AI systems are becoming more sophisticated and are being designed with more complex architectures and algorithms. These systems are expected to become more capable of handling complex tasks and making accurate predictions


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

    Your

     Name

    ],

     and

     I

    'm

     a

     [

    type

     of

     role

    ,

     job

    ,

     etc

    .

    ].

     I

    'm

     currently

     employed

     by

     [

    Your

     Company

    /

    Ent

    ire

     Organization

    ].

     My

     specialty

     is

     [

    Your

     specialty

    ].

     I

    've

     worked

     at

     [

    Your

     Company

    /

    Ent

    ire

     Organization

    ]

     for

     [

    x

     years

    ]

     and

     I

    'm

     looking

     to

     take

     on

     a

     new

     role

     at

     [

    New

     Company

    /

    Ent

    ire

     Organization

    ].

     I

    'm

     confident

     in

     my

     abilities

     and

     eager

     to

     contribute

     to

     the

     team

    .

     [

    Your

     Name

    ]

     would

     love

     to

     hear

     about

     what

     you

    're

     looking

     for

     in

     a

     new

     role

     and

     how

     I

     can

     help

     you

    .

     If

     you

     have

     any

     questions

    ,

     please

     don

    't

     hesitate

     to

     reach

     out

    .

     Welcome

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    .

     It

     is

     the

     city

     that

     serves

     as

     the

     political

    ,

     economic

    ,

     and

     cultural

     center

     of

     France

    .

     Its

     history

    ,

     architecture

    ,

     and

     culture

     have

     made

     it

     one

     of

     the

     most

     visited

     cities

     in

     the

     world

    .

     The

     city

     has

     a

     rich

     heritage

     dating

     back

     to

     the

     ancient

     Romans

     and

     its

     landmarks

     include

     the

     E

    iff

    el

     Tower

    ,

     Notre

    -D

    ame

     Cathedral

    ,

     and

     the

     Lou

    vre

     Museum

    .

     Paris

     is

     also

     home

     to

     many

     renowned

     museums

    ,

     such

     as

     the

     Mus

    ée

     d

    '

    Or

    say

     and

     the

     Lou

    vre

    .

     The

     city

     is

     also

     known

     for

     its

     fashion

     industry

    ,

     with

     several

     famous

     fashion

     houses

     like

     Ch

    anel

    ,

     Gu

    cci

    ,

     and

     D

    ior

     being

     headquartered

     there

    .

     Finally

    ,

     Paris

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     highly

     uncertain

    ,

     but

     there

     are

     several

     key

     trends

     that

     are

     likely

     to

     shape

     its

     evolution

     in

     the

     years

     to

     come

    .

     Here

     are

     some

     potential

     areas

     of

     development

     and

     change

    :
    


    1

    .

     AI

     will

     continue

     to

     become

     more

     sophisticated

     and

     capable

    .

     As

     AI

     technologies

     continue

     to

     advance

    ,

     we

     will

     see

     more

     complex

     algorithms

    ,

     greater

     accuracy

    ,

     and

     more

     sophisticated

     decision

    -making

     processes

    .
    


    2

    .

     AI

     will

     become

     more

     pervasive

     in

     our

     daily

     lives

    .

     We

     will

     see

     more

     applications

     of

     AI

     in

     various

     fields

    ,

     such

     as

     healthcare

    ,

     finance

    ,

     and

     transportation

    ,

     as

     AI

     algorithms

     become

     more

     robust

     and

     efficient

    .
    


    3

    .

     AI

     will

     continue

     to

     be

     used

     for

     surveillance

     and

     security

     purposes

    .

     As

     AI

     becomes

     more

     advanced

    



```python
llm.shutdown()
```
