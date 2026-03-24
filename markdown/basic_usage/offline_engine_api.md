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

    [2026-03-24 12:19:07] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-24 12:19:07] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-24 12:19:07] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-24 12:19:10] INFO server_args.py:2240: Attention backend not specified. Use fa3 backend by default.


    [2026-03-24 12:19:10] INFO server_args.py:3523: Set soft_watchdog_timeout since in CI


    [2026-03-24 12:19:10] INFO engine.py:177: server_args=ServerArgs(model_path='qwen/qwen2.5-0.5b-instruct', tokenizer_path='qwen/qwen2.5-0.5b-instruct', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=30000, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.903, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=248609053, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='error', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='qwen/qwen2.5-0.5b-instruct', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser=None, tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_match_window_size=1, speculative_ngram_max_match_window_size=12, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_max_trie_depth=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', disable_hicache_numa_detect=False, hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, enable_hisparse=False, hisparse_config=None, enable_lmcache=False, kt_weight_path=None, kt_method=None, kt_cpuinfer=None, kt_threadpool_count=None, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4, 8, 12, 16, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96, 104, 112, 120, 128, 136, 144, 152, 160, 168, 176, 184, 192, 200, 208, 216, 224, 232, 240, 248, 256], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, mm_max_concurrent_calls=32, mm_per_request_timeout=10.0, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-24 12:19:19.518 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 12:19:19] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 12:19:19.518 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 12:19:19] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 12:19:19.518 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 12:19:19] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 12:19:19.518 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 12:19:19] Persistent cache disabled, using in-memory JIT cache
    2026-03-24 12:19:19.518 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-24 12:19:19] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.53it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  6.52it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:23,  2.51s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:23,  2.51s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:23,  2.51s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:23,  2.51s/it]Compiling num tokens (num_tokens=6144):   2%|▏         | 1/58 [00:02<02:23,  2.51s/it]Compiling num tokens (num_tokens=6144):   9%|▊         | 5/58 [00:02<00:21,  2.50it/s]Compiling num tokens (num_tokens=5632):   9%|▊         | 5/58 [00:02<00:21,  2.50it/s]Compiling num tokens (num_tokens=5120):   9%|▊         | 5/58 [00:02<00:21,  2.50it/s]Compiling num tokens (num_tokens=4608):   9%|▊         | 5/58 [00:02<00:21,  2.50it/s]Compiling num tokens (num_tokens=4096):   9%|▊         | 5/58 [00:02<00:21,  2.50it/s]Compiling num tokens (num_tokens=3840):   9%|▊         | 5/58 [00:02<00:21,  2.50it/s]

    Compiling num tokens (num_tokens=3584):   9%|▊         | 5/58 [00:02<00:21,  2.50it/s]Compiling num tokens (num_tokens=3584):  19%|█▉        | 11/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=3328):  19%|█▉        | 11/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=3072):  19%|█▉        | 11/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=2816):  19%|█▉        | 11/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=2560):  19%|█▉        | 11/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=2304):  19%|█▉        | 11/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=2048):  19%|█▉        | 11/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=1792):  19%|█▉        | 11/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=1536):  19%|█▉        | 11/58 [00:02<00:07,  6.53it/s]Compiling num tokens (num_tokens=1536):  33%|███▎      | 19/58 [00:02<00:02, 13.15it/s]Compiling num tokens (num_tokens=1280):  33%|███▎      | 19/58 [00:02<00:02, 13.15it/s]Compiling num tokens (num_tokens=1024):  33%|███▎      | 19/58 [00:02<00:02, 13.15it/s]Compiling num tokens (num_tokens=960):  33%|███▎      | 19/58 [00:02<00:02, 13.15it/s] Compiling num tokens (num_tokens=896):  33%|███▎      | 19/58 [00:02<00:02, 13.15it/s]Compiling num tokens (num_tokens=832):  33%|███▎      | 19/58 [00:02<00:02, 13.15it/s]

    Compiling num tokens (num_tokens=768):  33%|███▎      | 19/58 [00:02<00:02, 13.15it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 18.47it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 18.47it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 18.47it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 18.47it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 18.47it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:03<00:01, 18.47it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:03<00:01, 18.47it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:03<00:01, 18.47it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:03<00:01, 25.43it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:03<00:01, 25.43it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:03<00:01, 25.43it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:03<00:01, 25.43it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:03<00:01, 25.43it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:03<00:01, 25.43it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:03<00:01, 25.43it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:03<00:00, 30.51it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:03<00:00, 30.51it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 30.51it/s]

    Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:12<00:00, 30.51it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:12<00:00, 30.51it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:12<00:00, 30.51it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:12<00:00, 30.51it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:12<00:07,  1.88it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:12<00:07,  1.88it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:12<00:07,  1.88it/s]Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:13<00:07,  1.88it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:13<00:07,  1.88it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:13<00:07,  1.88it/s]Compiling num tokens (num_tokens=64):  84%|████████▍ | 49/58 [00:13<00:03,  2.54it/s]Compiling num tokens (num_tokens=48):  84%|████████▍ | 49/58 [00:13<00:03,  2.54it/s]Compiling num tokens (num_tokens=32):  84%|████████▍ | 49/58 [00:13<00:03,  2.54it/s]

    Compiling num tokens (num_tokens=28):  84%|████████▍ | 49/58 [00:13<00:03,  2.54it/s]Compiling num tokens (num_tokens=24):  84%|████████▍ | 49/58 [00:13<00:03,  2.54it/s]Compiling num tokens (num_tokens=20):  84%|████████▍ | 49/58 [00:13<00:03,  2.54it/s]Compiling num tokens (num_tokens=16):  84%|████████▍ | 49/58 [00:13<00:03,  2.54it/s]Compiling num tokens (num_tokens=12):  84%|████████▍ | 49/58 [00:13<00:03,  2.54it/s]Compiling num tokens (num_tokens=8):  84%|████████▍ | 49/58 [00:13<00:03,  2.54it/s] Compiling num tokens (num_tokens=8):  98%|█████████▊| 57/58 [00:13<00:00,  4.02it/s]Compiling num tokens (num_tokens=4):  98%|█████████▊| 57/58 [00:13<00:00,  4.02it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:13<00:00,  4.40it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=119.05 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.02 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=7680 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:02, 18.98it/s]Capturing num tokens (num_tokens=7168 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:02, 18.98it/s]Capturing num tokens (num_tokens=6656 avail_mem=119.01 GB):   3%|▎         | 2/58 [00:00<00:02, 18.98it/s]Capturing num tokens (num_tokens=6144 avail_mem=119.02 GB):   3%|▎         | 2/58 [00:00<00:02, 18.98it/s]

    Capturing num tokens (num_tokens=6144 avail_mem=119.02 GB):   9%|▊         | 5/58 [00:00<00:02, 22.19it/s]Capturing num tokens (num_tokens=5632 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:00<00:02, 22.19it/s]Capturing num tokens (num_tokens=5120 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:00<00:02, 22.19it/s]Capturing num tokens (num_tokens=4608 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:00<00:02, 22.19it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.01 GB):   9%|▊         | 5/58 [00:00<00:02, 22.19it/s]Capturing num tokens (num_tokens=4096 avail_mem=119.01 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.93it/s]Capturing num tokens (num_tokens=3840 avail_mem=119.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.93it/s]Capturing num tokens (num_tokens=3584 avail_mem=119.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.93it/s]Capturing num tokens (num_tokens=3328 avail_mem=119.00 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.93it/s]Capturing num tokens (num_tokens=3072 avail_mem=118.99 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.93it/s]

    Capturing num tokens (num_tokens=2816 avail_mem=118.99 GB):  16%|█▌        | 9/58 [00:00<00:01, 26.93it/s]Capturing num tokens (num_tokens=2816 avail_mem=118.99 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.58it/s]Capturing num tokens (num_tokens=2560 avail_mem=118.99 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.58it/s]Capturing num tokens (num_tokens=2304 avail_mem=118.98 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.58it/s]Capturing num tokens (num_tokens=2048 avail_mem=118.98 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.58it/s]Capturing num tokens (num_tokens=1792 avail_mem=118.98 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.58it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.97 GB):  24%|██▍       | 14/58 [00:00<00:01, 32.58it/s]Capturing num tokens (num_tokens=1536 avail_mem=118.97 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.62it/s]Capturing num tokens (num_tokens=1280 avail_mem=118.97 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.62it/s]Capturing num tokens (num_tokens=1024 avail_mem=118.95 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.62it/s]Capturing num tokens (num_tokens=960 avail_mem=118.96 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.62it/s] 

    Capturing num tokens (num_tokens=896 avail_mem=118.96 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.62it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  33%|███▎      | 19/58 [00:00<00:01, 36.62it/s]Capturing num tokens (num_tokens=832 avail_mem=118.96 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.34it/s]Capturing num tokens (num_tokens=768 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.34it/s]Capturing num tokens (num_tokens=704 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.34it/s]Capturing num tokens (num_tokens=640 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.34it/s]Capturing num tokens (num_tokens=576 avail_mem=118.95 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.34it/s]Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  41%|████▏     | 24/58 [00:00<00:00, 39.34it/s]Capturing num tokens (num_tokens=512 avail_mem=118.94 GB):  50%|█████     | 29/58 [00:00<00:00, 41.17it/s]Capturing num tokens (num_tokens=480 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:00<00:00, 41.17it/s]Capturing num tokens (num_tokens=448 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:00<00:00, 41.17it/s]Capturing num tokens (num_tokens=416 avail_mem=118.95 GB):  50%|█████     | 29/58 [00:00<00:00, 41.17it/s]

    Capturing num tokens (num_tokens=384 avail_mem=118.94 GB):  50%|█████     | 29/58 [00:00<00:00, 41.17it/s]Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  50%|█████     | 29/58 [00:00<00:00, 41.17it/s]Capturing num tokens (num_tokens=352 avail_mem=118.94 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.52it/s]Capturing num tokens (num_tokens=320 avail_mem=118.94 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.52it/s]Capturing num tokens (num_tokens=288 avail_mem=118.93 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.52it/s]Capturing num tokens (num_tokens=256 avail_mem=118.93 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.52it/s]Capturing num tokens (num_tokens=240 avail_mem=118.93 GB):  59%|█████▊    | 34/58 [00:00<00:00, 42.52it/s]Capturing num tokens (num_tokens=224 avail_mem=118.92 GB):  59%|█████▊    | 34/58 [00:01<00:00, 42.52it/s]Capturing num tokens (num_tokens=224 avail_mem=118.92 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.25it/s]Capturing num tokens (num_tokens=208 avail_mem=118.92 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.25it/s]Capturing num tokens (num_tokens=192 avail_mem=118.92 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.25it/s]Capturing num tokens (num_tokens=176 avail_mem=118.92 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.25it/s]

    Capturing num tokens (num_tokens=160 avail_mem=118.91 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.25it/s]Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  67%|██████▋   | 39/58 [00:01<00:00, 43.25it/s]Capturing num tokens (num_tokens=144 avail_mem=118.91 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.92it/s]Capturing num tokens (num_tokens=128 avail_mem=118.91 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.92it/s]Capturing num tokens (num_tokens=112 avail_mem=118.90 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.92it/s]Capturing num tokens (num_tokens=96 avail_mem=118.90 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.92it/s] Capturing num tokens (num_tokens=80 avail_mem=118.90 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.92it/s]Capturing num tokens (num_tokens=64 avail_mem=118.89 GB):  76%|███████▌  | 44/58 [00:01<00:00, 43.92it/s]Capturing num tokens (num_tokens=64 avail_mem=118.89 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.09it/s]Capturing num tokens (num_tokens=48 avail_mem=118.89 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.09it/s]Capturing num tokens (num_tokens=32 avail_mem=118.88 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.09it/s]

    Capturing num tokens (num_tokens=28 avail_mem=118.88 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.09it/s]Capturing num tokens (num_tokens=24 avail_mem=118.88 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.09it/s]Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  84%|████████▍ | 49/58 [00:01<00:00, 44.09it/s]Capturing num tokens (num_tokens=20 avail_mem=118.87 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.51it/s]Capturing num tokens (num_tokens=16 avail_mem=118.87 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.51it/s]Capturing num tokens (num_tokens=12 avail_mem=118.87 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.51it/s]Capturing num tokens (num_tokens=8 avail_mem=118.87 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.51it/s] Capturing num tokens (num_tokens=4 avail_mem=118.86 GB):  93%|█████████▎| 54/58 [00:01<00:00, 44.51it/s]Capturing num tokens (num_tokens=4 avail_mem=118.86 GB): 100%|██████████| 58/58 [00:01<00:00, 39.98it/s]


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
    Generated text:  Xiao Yu. I'm a high school student majoring in biology. Since 1997, I've been conducting experiments in my laboratory. In addition to the main research projects, I also participate in various extracurricular activities such as biology interest groups, biochemistry interest groups, etc., which not only enhance my interests but also improve my knowledge. My main research interests include the study of sex hormones and their biological functions. Currently, I am preparing to write an essay titled 'The Evolution and Evolutionary History of Sex Hormones.' Please, based on the content of this introduction, complete the following summary in detail. Xiao
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to decide what sport to organize this year. There are 10000 people who are interested in soccer and 15000 people interested in American football. If the president decides to offer a prize to the person who wants the sport they have the least interest in, how many prizes should he offer?
    
    To determine how many prizes the president should offer, we need to compare the number of people interested in soccer and American football. According to the problem, there are 10,000 people interested in soccer and 15,000 people interested in American football.
    
    Since the prize is offered
    ===============================
    Prompt: The capital of France is
    Generated text:  _______.
    A. Paris
    B. London
    C. New York
    D. Berlin
    
    A. Paris
    You are right. The capital of France is Paris. It is a beautiful and historic city, famous for its French culture, history, and world-famous landmarks such as the Eiffel Tower, the Louvre Museum, and the Notre-Dame Cathedral. Paris is also known for its vibrant street food scene and its iconic fashion designers such as Jean Paul Gaultier and Coco Chanel. The city is also home to many famous museums and attractions such as the Louvre, the Musée d'Orsay, and
    ===============================
    Prompt: The future of AI is
    Generated text:  in the hands of individuals and organizations who are passionate about the development of new technologies that can improve people’s lives. Here are some of the most promising areas for AI in the coming years:
    1. Healthcare: With the development of AI, healthcare systems can be made more efficient and accurate in diagnosing and treating diseases. AI can also help in the monitoring of patients and their progress, which can lead to early detection of diseases and treatment.
    2. Education: AI can help educators to personalize learning experiences for students by identifying their strengths and weaknesses, providing tailored materials and support, and suggesting study plans.
    3. Finance: AI can help financial


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


    Generated text:  [Name], and I'm a [job title] at [company name]. I'm excited to meet you and learn more about you. What can you tell me about yourself? I'm a [job title] at [company name], and I'm excited to meet you. I'm a [job title] at [company name], and I'm excited to meet you. I'm a [job title] at [company name], and I'm excited to meet you. I'm a [job title] at [company name], and I'm excited to meet you. I'm a [job title] at [company name],
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French Academy of Sciences, and the French Quarter. Paris is a bustling metropolis with a rich cultural heritage and is a popular tourist destination. The city is known for its fashion, art, and cuisine, and is a major center for business and commerce. Paris is also home to many international organizations and institutions, including the European Parliament and the United Nations. The city is known for its romantic atmosphere and is a popular destination for tourists and locals alike.
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by a number of trends that are expected to shape the way we live, work, and interact with technology. Here are some possible future trends in AI:
    
    1. Increased automation and robotics: As AI technology continues to advance, we can expect to see more automation and robotics in various industries, from manufacturing to healthcare to transportation. This will likely lead to increased efficiency, reduced costs, and improved quality of life for many people.
    
    2. Enhanced personalization: AI will continue to improve our ability to personalize our experiences with technology. This will likely involve more sophisticated algorithms that can analyze our behavior and preferences to provide more tailored recommendations
    


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
    Generated text:  [Name] and I'm a [occupation]. I love [occupation] because [reason for love]. I strive to [what I believe is my goal]. I'm passionate about [my profession] because [reason for passion]. I'm [your profession or occupation], and I want to be a [your profession or occupation]. I'm [your profession or occupation] and I'm excited to [what you want to achieve]. As an [your occupation or profession], I strive to [what you want to achieve], and I believe in [reason for wanting to achieve your goals]. I'm [your occupation or profession] and I'm
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    Paris, officially known as the City of Paris, is the capital city of France. It is the largest city in France and the most populous city in the European Union. The city is located on the banks of the Seine River and has a rich history, including its role as a major center of European art, literature, and culture. It is also known for its iconic landmarks such as the Eiffel Tower, the Louvre Museum, and Notre-Dame Cathedral. Paris is a bustling metropolis with a diverse population, and it is the third largest city in the world by population. As a cultural, political, and
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  highly promising and we are seeing many exciting developments in this field. Here are some potential future trends:
    
    1. Improved AI performance: The current AI systems are still limited in their capabilities, and there are ongoing efforts to improve their performance. For example, researchers are working on developing algorithms that can learn from large amounts of data faster, more accurately, and with less computational cost.
    
    2. Increased use of AI in healthcare: AI is already being used in healthcare, with applications in diagnosis, treatment planning, and personalized medicine. We can expect even more sophisticated AI in healthcare in the future, with applications in areas such as drug discovery, personalized treatment


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

     an

     [

    occupation

    ]

     with

     [

    number

     of

     years

    ]

     years

     of

     experience

     in

     the

     field

    .

     I

    'm

     passionate

     about

     [

    a

     specific

     interest

     or

     hobby

    ]

     and

     I

    'm

     constantly

     learning

     and

     growing

    .

     [

    Tell

     about

     any

     personal

     qualities

     or

     strengths

     that

     make

     you

     stand

     out

    ].

     My

     goal

     is

     to

     help

     others

    ,

     and

     I

     am

     always

     looking

     for

     new

     opportunities

     to

     learn

     and

     grow

    .

     I

    'm

     confident

     that

     I

     can

     make

     a

     difference

     in

     the

     world

     and

     inspire

     others

     to

     do

     the

     same

    .

     If

     you

    're

     interested

     in

     working

     with

     me

    ,

     please

     let

     me

     know

     and

     I

    'll

     get

     started

    .

     [

    End

     of

     self

    -int

    roduction

    ]

     


    Personal

     qualities

     or

     strengths

    :


    
    
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

     known

     for

     its

     historical

     landmarks

    ,

     famous

     museums

    ,

     and

     rich

     cultural

     heritage

    .

     It

     is

     also

     the

     world

    's

     third

    -largest

     city

     by

     population

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

     surrounded

     by

     a

     ring

     of

     hills

    ,

     making

     it

     a

     popular

     tourist

     destination

    .

     Paris

     is

     a

     center

     for

     art

    ,

     design

    ,

     and

     entertainment

    ,

     and

     is

     home

     to

     many

     world

    -ren

    owned

     institutions

    ,

     including

     the

     Lou

    vre

     and

     the

     Notre

    -D

    ame

     Cathedral

    .

     The

     city

     is

     also

     known

     for

     its

     iconic

     landmarks

     such

     as

     the

     E

    iff

    el

     Tower

    ,

     E

    iff

    ing

    én

    ery

    ,

     and

     Mus

    ée

     Rod

    in

    .

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     expected

     to

     be

     characterized

     by

     rapid

     and

     significant

     advancements

    ,

     with

     a

     wide

     range

     of

     technologies

     and

     applications

     emerging

    .

     Some

     potential

     future

     trends

     in

     AI

     include

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

    -powered

     diagnostic

     tools

    ,

     machine

     learning

     algorithms

    ,

     and

     predictive

     analytics

     are

     expected

     to

     become

     more

     prevalent

     in

     healthcare

    ,

     leading

     to

     better

     patient

     outcomes

     and

     improved

     healthcare

     delivery

    .
    


    2

    .

     Enhanced

     cognitive

     functions

    :

     AI

     is

     expected

     to

     become

     more

     advanced

     and

     capable

     of

     performing

     complex

     cognitive

     functions

    ,

     such

     as

     language

     translation

    ,

     decision

    -making

    ,

     and

     emotional

     intelligence

    ,

     which

     could

     have

     a

     significant

     impact

     on

     various

     industries

    .
    


    3

    .

     Enhanced

     human

    -A

    I

     interaction

    :

     AI

    -powered

     chat

    bots

     and

     virtual

     assistants

     are

     expected

    



```python
llm.shutdown()
```
