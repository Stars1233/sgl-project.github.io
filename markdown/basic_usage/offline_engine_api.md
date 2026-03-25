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

    [2026-03-25 06:06:15] INFO utils.py:148: Note: detected 192 virtual cores but NumExpr set to maximum of 64, check "NUMEXPR_MAX_THREADS" environment variable.


    [2026-03-25 06:06:15] INFO utils.py:151: Note: NumExpr detected 192 cores but "NUMEXPR_MAX_THREADS" not set, so enforcing safe limit of 16.


    [2026-03-25 06:06:15] INFO utils.py:164: NumExpr defaulting to 16 threads.


    [2026-03-25 06:06:18] INFO server_args.py:2246: Attention backend not specified. Use fa3 backend by default.


    [2026-03-25 06:06:19] INFO server_args.py:3535: Set soft_watchdog_timeout since in CI


    [2026-03-25 06:06:19] INFO engine.py:177: server_args=ServerArgs(model_path='qwen/qwen2.5-0.5b-instruct', tokenizer_path='qwen/qwen2.5-0.5b-instruct', tokenizer_mode='auto', tokenizer_worker_num=1, skip_tokenizer_init=False, load_format='auto', model_loader_extra_config='{}', trust_remote_code=False, context_length=None, is_embedding=False, enable_multimodal=None, revision=None, model_impl='auto', host='127.0.0.1', port=30000, fastapi_root_path='', grpc_mode=False, skip_server_warmup=False, warmups=None, nccl_port=None, checkpoint_engine_wait_weights_before_ready=False, ssl_keyfile=None, ssl_certfile=None, ssl_ca_certs=None, ssl_keyfile_password=None, enable_ssl_refresh=False, dtype='auto', quantization=None, quantization_param_path=None, kv_cache_dtype='auto', enable_fp32_lm_head=False, modelopt_quant=None, modelopt_checkpoint_restore_path=None, modelopt_checkpoint_save_path=None, modelopt_export_path=None, quantize_and_serve=False, rl_quant_profile=None, mem_fraction_static=0.903, max_running_requests=128, max_queued_requests=None, max_total_tokens=20480, chunked_prefill_size=8192, enable_dynamic_chunking=False, max_prefill_tokens=16384, prefill_max_requests=None, schedule_policy='fcfs', enable_priority_scheduling=False, disable_priority_preemption=False, default_priority_value=None, abort_on_priority_when_disabled=False, schedule_low_priority_values_first=False, priority_scheduling_preemption_threshold=10, schedule_conservativeness=1.0, page_size=1, swa_full_tokens_ratio=0.8, disable_hybrid_swa_memory=False, radix_eviction_policy='lru', enable_prefill_delayer=False, prefill_delayer_max_delay_passes=30, prefill_delayer_token_usage_low_watermark=None, prefill_delayer_forward_passes_buckets=None, prefill_delayer_wait_seconds_buckets=None, device='cuda', tp_size=1, pp_size=1, pp_max_micro_batch_size=None, pp_async_batch_depth=0, stream_interval=1, incremental_streaming_output=False, enable_streaming_session=False, random_seed=466852139, constrained_json_whitespace_pattern=None, constrained_json_disable_any_whitespace=False, watchdog_timeout=300, soft_watchdog_timeout=300, dist_timeout=None, download_dir=None, model_checksum=None, base_gpu_id=0, gpu_id_step=1, sleep_on_idle=False, use_ray=False, custom_sigquit_handler=None, log_level='error', log_level_http=None, log_requests=False, log_requests_level=2, log_requests_format='text', log_requests_target=None, uvicorn_access_log_exclude_prefixes=[], crash_dump_folder=None, show_time_cost=False, enable_metrics=False, enable_metrics_for_all_schedulers=False, tokenizer_metrics_custom_labels_header='x-custom-labels', tokenizer_metrics_allowed_custom_labels=None, extra_metric_labels=None, bucket_time_to_first_token=None, bucket_inter_token_latency=None, bucket_e2e_request_latency=None, collect_tokens_histogram=False, prompt_tokens_buckets=None, generation_tokens_buckets=None, gc_warning_threshold_secs=0.0, decode_log_interval=40, enable_request_time_stats_logging=False, kv_events_config=None, enable_trace=False, otlp_traces_endpoint='localhost:4317', export_metrics_to_file=False, export_metrics_to_file_dir=None, api_key=None, admin_api_key=None, served_model_name='qwen/qwen2.5-0.5b-instruct', weight_version='default', chat_template=None, hf_chat_template_name=None, completion_template=None, file_storage_path='sglang_storage', enable_cache_report=False, reasoning_parser=None, tool_call_parser=None, tool_server=None, sampling_defaults='model', dp_size=1, load_balance_method='round_robin', attn_cp_size=1, moe_dp_size=1, dist_init_addr=None, nnodes=1, node_rank=0, json_model_override_args='{}', preferred_sampling_params=None, enable_lora=None, enable_lora_overlap_loading=None, max_lora_rank=None, lora_target_modules=None, lora_paths=None, max_loaded_loras=None, max_loras_per_batch=8, lora_eviction_policy='lru', lora_backend='csgmv', max_lora_chunk_size=16, attention_backend='fa3', decode_attention_backend=None, prefill_attention_backend=None, sampling_backend='flashinfer', grammar_backend='xgrammar', mm_attention_backend=None, fp8_gemm_runner_backend='auto', fp4_gemm_runner_backend='auto', nsa_prefill_backend=None, nsa_decode_backend=None, disable_flashinfer_autotune=False, mamba_backend='triton', speculative_algorithm=None, speculative_draft_model_path=None, speculative_draft_model_revision=None, speculative_draft_load_format=None, speculative_num_steps=None, speculative_eagle_topk=None, speculative_num_draft_tokens=None, speculative_accept_threshold_single=1.0, speculative_accept_threshold_acc=1.0, speculative_token_map=None, speculative_attention_mode='prefill', speculative_draft_attention_backend=None, speculative_moe_runner_backend='auto', speculative_moe_a2a_backend=None, speculative_draft_model_quantization=None, speculative_ngram_min_match_window_size=1, speculative_ngram_max_match_window_size=12, speculative_ngram_min_bfs_breadth=1, speculative_ngram_max_bfs_breadth=10, speculative_ngram_match_type='BFS', speculative_ngram_max_trie_depth=18, speculative_ngram_capacity=10000000, enable_multi_layer_eagle=False, ep_size=1, moe_a2a_backend='none', moe_runner_backend='auto', flashinfer_mxfp4_moe_precision='default', enable_flashinfer_allreduce_fusion=False, enable_aiter_allreduce_fusion=False, deepep_mode='auto', ep_num_redundant_experts=0, ep_dispatch_algorithm=None, init_expert_location='trivial', enable_eplb=False, eplb_algorithm='auto', eplb_rebalance_num_iterations=1000, eplb_rebalance_layers_per_chunk=None, eplb_min_rebalancing_utilization_threshold=1.0, expert_distribution_recorder_mode=None, expert_distribution_recorder_buffer_size=1000, enable_expert_distribution_metrics=False, deepep_config=None, moe_dense_tp_size=None, elastic_ep_backend=None, enable_elastic_expert_backup=False, mooncake_ib_device=None, max_mamba_cache_size=None, mamba_ssm_dtype=None, mamba_full_memory_ratio=0.9, mamba_scheduler_strategy='no_buffer', mamba_track_interval=256, linear_attn_backend='triton', linear_attn_decode_backend=None, linear_attn_prefill_backend=None, enable_hierarchical_cache=False, hicache_ratio=2.0, hicache_size=0, hicache_write_policy='write_through', hicache_io_backend='kernel', hicache_mem_layout='layer_first', disable_hicache_numa_detect=False, hicache_storage_backend=None, hicache_storage_prefetch_policy='best_effort', hicache_storage_backend_extra_config=None, enable_hisparse=False, hisparse_config=None, enable_lmcache=False, kt_weight_path=None, kt_method=None, kt_cpuinfer=None, kt_threadpool_count=None, kt_num_gpu_experts=None, kt_max_deferred_experts_per_token=None, dllm_algorithm=None, dllm_algorithm_config=None, enable_double_sparsity=False, ds_channel_config_path=None, ds_heavy_channel_num=32, ds_heavy_token_num=256, ds_heavy_channel_type='qk', ds_sparse_decode_threshold=4096, cpu_offload_gb=0, offload_group_size=-1, offload_num_in_group=1, offload_prefetch_step=1, offload_mode='cpu', multi_item_scoring_delimiter=None, disable_radix_cache=False, cuda_graph_max_bs=4, cuda_graph_bs=[1, 2, 4, 8, 12, 16, 24, 32, 40, 48, 56, 64, 72, 80, 88, 96, 104, 112, 120, 128, 136, 144, 152, 160, 168, 176, 184, 192, 200, 208, 216, 224, 232, 240, 248, 256], disable_cuda_graph=True, disable_cuda_graph_padding=False, enable_profile_cuda_graph=False, enable_cudagraph_gc=False, enable_layerwise_nvtx_marker=False, enable_nccl_nvls=False, enable_symm_mem=False, disable_flashinfer_cutlass_moe_fp4_allgather=False, enable_tokenizer_batch_encode=False, disable_tokenizer_batch_decode=False, disable_outlines_disk_cache=False, disable_custom_all_reduce=False, enable_mscclpp=False, enable_torch_symm_mem=False, pre_warm_nccl=False, disable_overlap_schedule=False, enable_mixed_chunk=False, enable_dp_attention=False, enable_dp_lm_head=False, enable_two_batch_overlap=False, enable_single_batch_overlap=False, tbo_token_distribution_threshold=0.48, enable_torch_compile=False, disable_piecewise_cuda_graph=False, enforce_piecewise_cuda_graph=False, enable_torch_compile_debug_mode=False, torch_compile_max_bs=32, piecewise_cuda_graph_max_tokens=8192, piecewise_cuda_graph_tokens=[4, 8, 12, 16, 20, 24, 28, 32, 48, 64, 80, 96, 112, 128, 144, 160, 176, 192, 208, 224, 240, 256, 288, 320, 352, 384, 416, 448, 480, 512, 576, 640, 704, 768, 832, 896, 960, 1024, 1280, 1536, 1792, 2048, 2304, 2560, 2816, 3072, 3328, 3584, 3840, 4096, 4608, 5120, 5632, 6144, 6656, 7168, 7680, 8192], piecewise_cuda_graph_compiler='eager', torchao_config='', enable_nan_detection=False, enable_p2p_check=False, triton_attention_reduce_in_fp32=False, triton_attention_num_kv_splits=8, triton_attention_split_tile_size=None, num_continuous_decode_steps=1, delete_ckpt_after_loading=False, enable_memory_saver=False, enable_weights_cpu_backup=False, enable_draft_weights_cpu_backup=False, allow_auto_truncate=False, enable_custom_logit_processor=False, flashinfer_mla_disable_ragged=False, disable_shared_experts_fusion=False, disable_chunked_prefix_cache=False, disable_fast_image_processor=False, keep_mm_feature_on_device=False, enable_return_hidden_states=False, enable_return_routed_experts=False, scheduler_recv_interval=1, numa_node=None, enable_deterministic_inference=False, rl_on_policy_target=None, enable_attn_tp_input_scattered=False, enable_nsa_prefill_context_parallel=False, nsa_prefill_cp_mode='round-robin-split', enable_fused_qk_norm_rope=False, enable_precise_embedding_interpolation=False, enable_fused_moe_sum_all_reduce=False, enable_prefill_context_parallel=False, prefill_cp_mode='in-seq-split', enable_dynamic_batch_tokenizer=False, dynamic_batch_tokenizer_batch_size=32, dynamic_batch_tokenizer_batch_timeout=0.002, debug_tensor_dump_output_folder=None, debug_tensor_dump_layers=None, debug_tensor_dump_input_file=None, debug_tensor_dump_inject=False, disaggregation_mode='null', disaggregation_transfer_backend='mooncake', disaggregation_bootstrap_port=8998, disaggregation_ib_device=None, disaggregation_decode_enable_offload_kvcache=False, num_reserved_decode_tokens=512, disaggregation_decode_polling_interval=1, encoder_only=False, language_only=False, encoder_transfer_backend='zmq_to_scheduler', encoder_urls=[], enable_adaptive_dispatch_to_encoder=False, custom_weight_loader=[], weight_loader_disable_mmap=False, remote_instance_weight_loader_seed_instance_ip=None, remote_instance_weight_loader_seed_instance_service_port=None, remote_instance_weight_loader_send_weights_group_ports=None, remote_instance_weight_loader_backend='nccl', remote_instance_weight_loader_start_seed_via_transfer_engine=False, modelexpress_config=None, enable_pdmux=False, pdmux_config_path=None, sm_group_num=8, mm_max_concurrent_calls=32, mm_per_request_timeout=10.0, enable_broadcast_mm_inputs_process=False, enable_prefix_mm_cache=False, mm_enable_dp_encoder=False, mm_process_config={}, limit_mm_data_per_request=None, enable_mm_global_cache=False, decrypted_config_file=None, decrypted_draft_config_file=None, forward_hooks=None)


    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0
    [Gloo] Rank 0 is connected to 0 peer ranks. Expected number of connected peer ranks is : 0


    2026-03-25 06:06:27.724 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 06:06:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 06:06:27.725 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 06:06:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 06:06:27.725 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 06:06:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 06:06:27.725 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 06:06:27] Persistent cache disabled, using in-memory JIT cache
    2026-03-25 06:06:27.725 DEBUG Persistent cache disabled, using in-memory JIT cache
    [2026-03-25 06:06:27] Persistent cache disabled, using in-memory JIT cache


    Loading safetensors checkpoint shards:   0% Completed | 0/1 [00:00<?, ?it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.89it/s]Loading safetensors checkpoint shards: 100% Completed | 1/1 [00:00<00:00,  5.88it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Compiling num tokens (num_tokens=8192):   0%|          | 0/58 [00:00<?, ?it/s]

    Compiling num tokens (num_tokens=8192):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=7680):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=7168):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6656):   2%|▏         | 1/58 [00:02<02:13,  2.35s/it]Compiling num tokens (num_tokens=6656):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=6144):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5632):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=5120):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=4608):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=4096):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]Compiling num tokens (num_tokens=3840):   7%|▋         | 4/58 [00:02<00:25,  2.12it/s]

    Compiling num tokens (num_tokens=3840):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=3584):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=3328):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=3072):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=2816):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=2560):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=2304):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=2048):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=1792):  17%|█▋        | 10/58 [00:02<00:07,  6.45it/s]Compiling num tokens (num_tokens=1792):  31%|███       | 18/58 [00:02<00:02, 13.50it/s]Compiling num tokens (num_tokens=1536):  31%|███       | 18/58 [00:02<00:02, 13.50it/s]Compiling num tokens (num_tokens=1280):  31%|███       | 18/58 [00:02<00:02, 13.50it/s]Compiling num tokens (num_tokens=1024):  31%|███       | 18/58 [00:02<00:02, 13.50it/s]Compiling num tokens (num_tokens=960):  31%|███       | 18/58 [00:02<00:02, 13.50it/s] Compiling num tokens (num_tokens=896):  31%|███       | 18/58 [00:02<00:02, 13.50it/s]Compiling num tokens (num_tokens=832):  31%|███       | 18/58 [00:02<00:02, 13.50it/s]

    Compiling num tokens (num_tokens=768):  31%|███       | 18/58 [00:02<00:02, 13.50it/s]Compiling num tokens (num_tokens=768):  43%|████▎     | 25/58 [00:02<00:01, 20.03it/s]Compiling num tokens (num_tokens=704):  43%|████▎     | 25/58 [00:02<00:01, 20.03it/s]Compiling num tokens (num_tokens=640):  43%|████▎     | 25/58 [00:02<00:01, 20.03it/s]Compiling num tokens (num_tokens=576):  43%|████▎     | 25/58 [00:02<00:01, 20.03it/s]Compiling num tokens (num_tokens=512):  43%|████▎     | 25/58 [00:02<00:01, 20.03it/s]Compiling num tokens (num_tokens=480):  43%|████▎     | 25/58 [00:02<00:01, 20.03it/s]Compiling num tokens (num_tokens=448):  43%|████▎     | 25/58 [00:02<00:01, 20.03it/s]Compiling num tokens (num_tokens=416):  43%|████▎     | 25/58 [00:02<00:01, 20.03it/s]Compiling num tokens (num_tokens=416):  55%|█████▌    | 32/58 [00:02<00:00, 27.00it/s]Compiling num tokens (num_tokens=384):  55%|█████▌    | 32/58 [00:02<00:00, 27.00it/s]Compiling num tokens (num_tokens=352):  55%|█████▌    | 32/58 [00:02<00:00, 27.00it/s]Compiling num tokens (num_tokens=320):  55%|█████▌    | 32/58 [00:02<00:00, 27.00it/s]Compiling num tokens (num_tokens=288):  55%|█████▌    | 32/58 [00:02<00:00, 27.00it/s]Compiling num tokens (num_tokens=256):  55%|█████▌    | 32/58 [00:02<00:00, 27.00it/s]

    Compiling num tokens (num_tokens=240):  55%|█████▌    | 32/58 [00:02<00:00, 27.00it/s]Compiling num tokens (num_tokens=240):  66%|██████▌   | 38/58 [00:02<00:00, 31.97it/s]Compiling num tokens (num_tokens=224):  66%|██████▌   | 38/58 [00:02<00:00, 31.97it/s]Compiling num tokens (num_tokens=208):  66%|██████▌   | 38/58 [00:03<00:00, 31.97it/s]Compiling num tokens (num_tokens=192):  66%|██████▌   | 38/58 [00:03<00:00, 31.97it/s]Compiling num tokens (num_tokens=176):  66%|██████▌   | 38/58 [00:03<00:00, 31.97it/s]Compiling num tokens (num_tokens=160):  66%|██████▌   | 38/58 [00:03<00:00, 31.97it/s]Compiling num tokens (num_tokens=144):  66%|██████▌   | 38/58 [00:03<00:00, 31.97it/s]Compiling num tokens (num_tokens=144):  76%|███████▌  | 44/58 [00:03<00:00, 36.04it/s]Compiling num tokens (num_tokens=128):  76%|███████▌  | 44/58 [00:03<00:00, 36.04it/s]Compiling num tokens (num_tokens=112):  76%|███████▌  | 44/58 [00:03<00:00, 36.04it/s]

    Compiling num tokens (num_tokens=96):  76%|███████▌  | 44/58 [00:03<00:00, 36.04it/s] Compiling num tokens (num_tokens=80):  76%|███████▌  | 44/58 [00:03<00:00, 36.04it/s]Compiling num tokens (num_tokens=64):  76%|███████▌  | 44/58 [00:03<00:00, 36.04it/s]Compiling num tokens (num_tokens=48):  76%|███████▌  | 44/58 [00:03<00:00, 36.04it/s]Compiling num tokens (num_tokens=48):  86%|████████▌ | 50/58 [00:03<00:00, 33.28it/s]Compiling num tokens (num_tokens=32):  86%|████████▌ | 50/58 [00:03<00:00, 33.28it/s]Compiling num tokens (num_tokens=28):  86%|████████▌ | 50/58 [00:03<00:00, 33.28it/s]Compiling num tokens (num_tokens=24):  86%|████████▌ | 50/58 [00:03<00:00, 33.28it/s]

    Compiling num tokens (num_tokens=20):  86%|████████▌ | 50/58 [00:03<00:00, 33.28it/s]Compiling num tokens (num_tokens=16):  86%|████████▌ | 50/58 [00:03<00:00, 33.28it/s]Compiling num tokens (num_tokens=16):  95%|█████████▍| 55/58 [00:03<00:00, 34.60it/s]Compiling num tokens (num_tokens=12):  95%|█████████▍| 55/58 [00:03<00:00, 34.60it/s]Compiling num tokens (num_tokens=8):  95%|█████████▍| 55/58 [00:03<00:00, 34.60it/s] Compiling num tokens (num_tokens=4):  95%|█████████▍| 55/58 [00:03<00:00, 34.60it/s]Compiling num tokens (num_tokens=4): 100%|██████████| 58/58 [00:03<00:00, 16.47it/s]


      0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=131.88 GB):   0%|          | 0/58 [00:00<?, ?it/s]Capturing num tokens (num_tokens=8192 avail_mem=131.88 GB):   2%|▏         | 1/58 [00:00<00:06,  8.75it/s]Capturing num tokens (num_tokens=7680 avail_mem=131.85 GB):   2%|▏         | 1/58 [00:00<00:06,  8.75it/s]Capturing num tokens (num_tokens=7168 avail_mem=131.85 GB):   2%|▏         | 1/58 [00:00<00:06,  8.75it/s]

    Capturing num tokens (num_tokens=7168 avail_mem=131.85 GB):   5%|▌         | 3/58 [00:00<00:04, 12.50it/s]Capturing num tokens (num_tokens=6656 avail_mem=131.84 GB):   5%|▌         | 3/58 [00:00<00:04, 12.50it/s]Capturing num tokens (num_tokens=6144 avail_mem=131.84 GB):   5%|▌         | 3/58 [00:00<00:04, 12.50it/s]Capturing num tokens (num_tokens=5632 avail_mem=131.84 GB):   5%|▌         | 3/58 [00:00<00:04, 12.50it/s]Capturing num tokens (num_tokens=5632 avail_mem=131.84 GB):  10%|█         | 6/58 [00:00<00:03, 16.63it/s]Capturing num tokens (num_tokens=5120 avail_mem=131.84 GB):  10%|█         | 6/58 [00:00<00:03, 16.63it/s]Capturing num tokens (num_tokens=4608 avail_mem=131.83 GB):  10%|█         | 6/58 [00:00<00:03, 16.63it/s]

    Capturing num tokens (num_tokens=4096 avail_mem=131.83 GB):  10%|█         | 6/58 [00:00<00:03, 16.63it/s]Capturing num tokens (num_tokens=4096 avail_mem=131.83 GB):  16%|█▌        | 9/58 [00:00<00:02, 20.43it/s]Capturing num tokens (num_tokens=3840 avail_mem=131.83 GB):  16%|█▌        | 9/58 [00:00<00:02, 20.43it/s]Capturing num tokens (num_tokens=3584 avail_mem=131.82 GB):  16%|█▌        | 9/58 [00:00<00:02, 20.43it/s]Capturing num tokens (num_tokens=3328 avail_mem=131.82 GB):  16%|█▌        | 9/58 [00:00<00:02, 20.43it/s]Capturing num tokens (num_tokens=3072 avail_mem=131.82 GB):  16%|█▌        | 9/58 [00:00<00:02, 20.43it/s]Capturing num tokens (num_tokens=3072 avail_mem=131.82 GB):  22%|██▏       | 13/58 [00:00<00:01, 24.39it/s]Capturing num tokens (num_tokens=2816 avail_mem=131.82 GB):  22%|██▏       | 13/58 [00:00<00:01, 24.39it/s]Capturing num tokens (num_tokens=2560 avail_mem=131.81 GB):  22%|██▏       | 13/58 [00:00<00:01, 24.39it/s]

    Capturing num tokens (num_tokens=2304 avail_mem=131.81 GB):  22%|██▏       | 13/58 [00:00<00:01, 24.39it/s]Capturing num tokens (num_tokens=2048 avail_mem=131.81 GB):  22%|██▏       | 13/58 [00:00<00:01, 24.39it/s]Capturing num tokens (num_tokens=1792 avail_mem=131.80 GB):  22%|██▏       | 13/58 [00:00<00:01, 24.39it/s]Capturing num tokens (num_tokens=1792 avail_mem=131.80 GB):  31%|███       | 18/58 [00:00<00:01, 30.01it/s]Capturing num tokens (num_tokens=1536 avail_mem=131.80 GB):  31%|███       | 18/58 [00:00<00:01, 30.01it/s]Capturing num tokens (num_tokens=1280 avail_mem=131.79 GB):  31%|███       | 18/58 [00:00<00:01, 30.01it/s]Capturing num tokens (num_tokens=1024 avail_mem=131.78 GB):  31%|███       | 18/58 [00:00<00:01, 30.01it/s]Capturing num tokens (num_tokens=960 avail_mem=131.79 GB):  31%|███       | 18/58 [00:00<00:01, 30.01it/s] 

    Capturing num tokens (num_tokens=960 avail_mem=131.79 GB):  38%|███▊      | 22/58 [00:00<00:01, 29.99it/s]Capturing num tokens (num_tokens=896 avail_mem=131.79 GB):  38%|███▊      | 22/58 [00:00<00:01, 29.99it/s]Capturing num tokens (num_tokens=832 avail_mem=131.78 GB):  38%|███▊      | 22/58 [00:00<00:01, 29.99it/s]Capturing num tokens (num_tokens=768 avail_mem=131.78 GB):  38%|███▊      | 22/58 [00:00<00:01, 29.99it/s]Capturing num tokens (num_tokens=704 avail_mem=131.78 GB):  38%|███▊      | 22/58 [00:00<00:01, 29.99it/s]Capturing num tokens (num_tokens=640 avail_mem=131.77 GB):  38%|███▊      | 22/58 [00:00<00:01, 29.99it/s]Capturing num tokens (num_tokens=640 avail_mem=131.77 GB):  47%|████▋     | 27/58 [00:00<00:00, 34.38it/s]Capturing num tokens (num_tokens=576 avail_mem=131.77 GB):  47%|████▋     | 27/58 [00:00<00:00, 34.38it/s]Capturing num tokens (num_tokens=512 avail_mem=131.19 GB):  47%|████▋     | 27/58 [00:01<00:00, 34.38it/s]

    Capturing num tokens (num_tokens=480 avail_mem=131.75 GB):  47%|████▋     | 27/58 [00:01<00:00, 34.38it/s]Capturing num tokens (num_tokens=448 avail_mem=131.28 GB):  47%|████▋     | 27/58 [00:01<00:00, 34.38it/s]Capturing num tokens (num_tokens=448 avail_mem=131.28 GB):  53%|█████▎    | 31/58 [00:01<00:01, 26.91it/s]Capturing num tokens (num_tokens=416 avail_mem=131.74 GB):  53%|█████▎    | 31/58 [00:01<00:01, 26.91it/s]Capturing num tokens (num_tokens=384 avail_mem=131.31 GB):  53%|█████▎    | 31/58 [00:01<00:01, 26.91it/s]

    Capturing num tokens (num_tokens=352 avail_mem=131.74 GB):  53%|█████▎    | 31/58 [00:01<00:01, 26.91it/s]Capturing num tokens (num_tokens=352 avail_mem=131.74 GB):  59%|█████▊    | 34/58 [00:01<00:01, 23.80it/s]Capturing num tokens (num_tokens=320 avail_mem=131.33 GB):  59%|█████▊    | 34/58 [00:01<00:01, 23.80it/s]Capturing num tokens (num_tokens=288 avail_mem=131.72 GB):  59%|█████▊    | 34/58 [00:01<00:01, 23.80it/s]Capturing num tokens (num_tokens=256 avail_mem=131.73 GB):  59%|█████▊    | 34/58 [00:01<00:01, 23.80it/s]

    Capturing num tokens (num_tokens=256 avail_mem=131.73 GB):  64%|██████▍   | 37/58 [00:01<00:00, 22.65it/s]Capturing num tokens (num_tokens=240 avail_mem=131.38 GB):  64%|██████▍   | 37/58 [00:01<00:00, 22.65it/s]Capturing num tokens (num_tokens=224 avail_mem=131.72 GB):  64%|██████▍   | 37/58 [00:01<00:00, 22.65it/s]Capturing num tokens (num_tokens=208 avail_mem=131.40 GB):  64%|██████▍   | 37/58 [00:01<00:00, 22.65it/s]Capturing num tokens (num_tokens=208 avail_mem=131.40 GB):  69%|██████▉   | 40/58 [00:01<00:00, 22.09it/s]Capturing num tokens (num_tokens=192 avail_mem=131.71 GB):  69%|██████▉   | 40/58 [00:01<00:00, 22.09it/s]Capturing num tokens (num_tokens=176 avail_mem=131.71 GB):  69%|██████▉   | 40/58 [00:01<00:00, 22.09it/s]

    Capturing num tokens (num_tokens=160 avail_mem=131.73 GB):  69%|██████▉   | 40/58 [00:01<00:00, 22.09it/s]Capturing num tokens (num_tokens=160 avail_mem=131.73 GB):  74%|███████▍  | 43/58 [00:01<00:00, 22.60it/s]Capturing num tokens (num_tokens=144 avail_mem=131.47 GB):  74%|███████▍  | 43/58 [00:01<00:00, 22.60it/s]Capturing num tokens (num_tokens=128 avail_mem=131.70 GB):  74%|███████▍  | 43/58 [00:01<00:00, 22.60it/s]Capturing num tokens (num_tokens=112 avail_mem=131.69 GB):  74%|███████▍  | 43/58 [00:01<00:00, 22.60it/s]Capturing num tokens (num_tokens=112 avail_mem=131.69 GB):  79%|███████▉  | 46/58 [00:01<00:00, 22.50it/s]Capturing num tokens (num_tokens=96 avail_mem=131.69 GB):  79%|███████▉  | 46/58 [00:01<00:00, 22.50it/s] 

    Capturing num tokens (num_tokens=80 avail_mem=131.68 GB):  79%|███████▉  | 46/58 [00:01<00:00, 22.50it/s]Capturing num tokens (num_tokens=64 avail_mem=131.67 GB):  79%|███████▉  | 46/58 [00:02<00:00, 22.50it/s]Capturing num tokens (num_tokens=64 avail_mem=131.67 GB):  84%|████████▍ | 49/58 [00:02<00:00, 23.03it/s]Capturing num tokens (num_tokens=48 avail_mem=131.51 GB):  84%|████████▍ | 49/58 [00:02<00:00, 23.03it/s]Capturing num tokens (num_tokens=32 avail_mem=131.50 GB):  84%|████████▍ | 49/58 [00:02<00:00, 23.03it/s]Capturing num tokens (num_tokens=28 avail_mem=131.63 GB):  84%|████████▍ | 49/58 [00:02<00:00, 23.03it/s]Capturing num tokens (num_tokens=28 avail_mem=131.63 GB):  90%|████████▉ | 52/58 [00:02<00:00, 24.06it/s]Capturing num tokens (num_tokens=24 avail_mem=131.64 GB):  90%|████████▉ | 52/58 [00:02<00:00, 24.06it/s]

    Capturing num tokens (num_tokens=20 avail_mem=131.64 GB):  90%|████████▉ | 52/58 [00:02<00:00, 24.06it/s]Capturing num tokens (num_tokens=16 avail_mem=131.63 GB):  90%|████████▉ | 52/58 [00:02<00:00, 24.06it/s]Capturing num tokens (num_tokens=12 avail_mem=131.62 GB):  90%|████████▉ | 52/58 [00:02<00:00, 24.06it/s]Capturing num tokens (num_tokens=12 avail_mem=131.62 GB):  97%|█████████▋| 56/58 [00:02<00:00, 26.26it/s]Capturing num tokens (num_tokens=8 avail_mem=131.59 GB):  97%|█████████▋| 56/58 [00:02<00:00, 26.26it/s] Capturing num tokens (num_tokens=4 avail_mem=131.59 GB):  97%|█████████▋| 56/58 [00:02<00:00, 26.26it/s]Capturing num tokens (num_tokens=4 avail_mem=131.59 GB): 100%|██████████| 58/58 [00:02<00:00, 24.49it/s]


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
    Generated text:  Bryan and I am a medical student. I plan to become a Doctor of Medicine. I have a 4 year degree in Pharmacy. I have been working as a Pharmacist for 3 years and I have been using my knowledge to help people. I have taken both anatomy and physiology and have studied in depth and have a passion for research and learning. My interest in patients is always high. I do my best to help my patients understand what is happening in their body and my research has led me to start a blog, called "Bryan's Pharmacy. " I am also interested in connecting with people and can be found on my social
    ===============================
    Prompt: The president of the United States is
    Generated text:  trying to decide whether to go to war with another country. The president is afraid that if he goes to war, the economy of that country will collapse. The president is also a huge sports fan and he wants to see his team win.
    
    The president has gathered 10 different experts to help him make a decision. Each expert has a different opinion on whether or not the president should go to war. The opinions of the 10 experts are as follows:
    
    - Expert 1: Yes
    - Expert 2: No
    - Expert 3: Yes
    - Expert 4: No
    - Expert 5: Yes
    
    ===============================
    Prompt: The capital of France is
    Generated text:  _______.
    
    A: New York  
    B: London  
    C: Paris  
    D: Moscow
    To determine the capital of France, let's review the capital cities of the other options and the information given in the problem:
    
    1. **New York**: This is the capital of the United States.
    2. **London**: This is the capital of the United Kingdom.
    3. **Paris**: This is the capital of France.
    4. **Moscow**: This is the capital of Russia.
    
    Based on this information, the capital of France is Paris. Therefore, the correct answer is:
    
    \boxed{C}
    ===============================
    Prompt: The future of AI is
    Generated text:  not clear, and the rapid development of the field continues to challenge the boundaries of human perception. As AI continues to evolve, it is crucial to understand the implications of these developments on our daily lives and make conscious choices to ensure that we are prepared for the future. In this essay, we will explore the potential impact of AI on various aspects of our lives and explore how we can make informed decisions to ensure that we are prepared for the future of AI. 
    
    One of the most significant impacts of AI is its potential to revolutionize various industries. AI has already transformed the healthcare industry by improving diagnostic accuracy and personalized treatment plans. It has also


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


    Generated text:  [Name] and I'm a [Age] year old [Gender] [Occupation]. I'm a [Occupation] who has always been [Positive or Negative] about [What you do for a living]. I'm [Positive or Negative] about [What you do for a living]. I'm [Positive or Negative] about [What you do for a living]. I'm [Positive or Negative] about [What you do for a living]. I'm [Positive or Negative] about [What you do for a living]. I'm [Positive or Negative] about [What you do for a living]. I'm [Positive
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is


    Generated text:  Paris, the city known for its iconic landmarks such as the Eiffel Tower, Notre-Dame Cathedral, and the Louvre Museum. It is also home to the French Parliament, the French National Library, and the French Academy of Sciences. Paris is a cultural and economic hub, with a rich history dating back to the Roman Empire and a modern city that has undergone significant development over the centuries. It is a popular tourist destination, with millions of visitors annually. Paris is also known for its cuisine, including its famous French fries and its iconic French wine. The city is home to many cultural institutions, including the Louvre Museum, the
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is


    Generated text:  likely to be characterized by several key trends:
    
    1. Increased automation: AI will continue to automate many tasks, from manufacturing to customer service. This will lead to increased efficiency and productivity, but it will also create new jobs and raise concerns about job displacement.
    
    2. AI will become more integrated with other technologies: AI will continue to be integrated with other technologies, such as machine learning, natural language processing, and computer vision. This will lead to new applications and opportunities for AI, but it will also require new skills and knowledge.
    
    3. AI will become more ethical and transparent: As AI becomes more integrated into our daily lives, there will
    


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
    Generated text:  [Name] and I'm a [Age] year old. I'm an experienced [Occupation] with a passion for [Interest or Hobby]. I love spending time with [Person] and traveling to [City]. What brings you to this place now? I am [City]. And I am a [City]. My professional background includes [Number of Years in Industry]. I'm currently [Occupation]. I'm passionate about [My Passion] and I'm looking forward to [Future Goals]. What's your journey like? What's your story? What brought you to this place? What do you hope to achieve here? Let me
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text:  Paris.
    
    Paris is the cultural, political, and economic center of France. It is the largest city in the European Union and has one of the world's most important cultural centers. Its most famous landmarks include the Eiffel Tower, Louvre Museum, Notre-Dame Cathedral, and the Louvre Abu Dhabi. Paris is also known for its fashion industry, cuisine, and French food culture. The city has a diverse population of about 2.3 million people, and it is home to many of the world's top fashion and art institutions. It is known for its beautiful architecture, vibrant nightlife, and importance in the world of finance
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text:  likely to be shaped by several key trends, including:
    
    1. Increased accuracy and precision: As AI systems become more advanced and capable of performing complex tasks, their accuracy and precision will increase. This will lead to more reliable and reliable results in a wide range of applications, from healthcare to finance.
    
    2. Integration with human decision-making: AI systems will become more integrated with human decision-making processes, allowing for more effective and efficient decision-making in a wide range of industries.
    
    3. Greater use of ethical considerations: As AI systems become more sophisticated, there will be a greater need for ethical considerations in their development and use. This will lead to


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

     am

     a

     [

    employment

     type

    ]

     at

     [

    company

     name

    ].

     I

     am

     passionate

     about

     [

    mot

    ivation

    ]

     and

     always

     strive

     to

     exceed

     my

     expectations

    .

     I

     am

     friendly

    ,

     honest

    ,

     and

     always

     eager

     to

     learn

     new

     things

    .

     I

     am

     a

     reliable

    ,

     dependable

     colleague

     who

     takes

     pride

     in

     my

     work

    .

     I

     am

     looking

     forward

     to

     [

    future

     goal

    ]

     and

     am

     excited

     about

     the

     opportunities

     that

     may

     come

     my

     way

    .

     I

     am

     enthusiastic

    ,

     proactive

    ,

     and

     always

     willing

     to

     go

     above

     and

     beyond

     for

     my

     colleagues

    .

     I

     am

     a

     team

     player

     and

     always

     aim

     to

     support

     and

     encourage

     my

     colleagues

    .

     I

     am

     always

     learning

     and

     growing

    ,

     and

     I

     am

     constantly

     striving

     to

     improve

    
    
    Prompt: Provide a concise factual statement about France’s capital city. The capital of France is
    Generated text: 

     Paris

    ,

     commonly

     known

     as

     "

    La

     Ville

     Fl

    ott

    ante

    "

     or

     simply

     "

    Paris

    ",

     and

     the

     largest

     city

     in

     France

     by

     population

    .

     The

     city

     is

     located

     in

     the

     north

    western

     region

     of

     the

     country

     and

     has

     a

     diverse

     population

     of

     over

     

    1

    4

     million

     people

    .

     It

     is

     home

     to

     many

     of

     France

    's

     oldest

     and

     most

     prestigious

     institutions

    ,

     including

     the

     Lou

    vre

     Museum

    ,

     Notre

    -D

    ame

     Cathedral

    ,

     and

     the

     E

    iff

    el

     Tower

    .

     Paris

     is

     also

     a

     hub

     for

     music

    ,

     film

    ,

     and

     fashion

    ,

     and

     is

     considered

     one

     of

     the

     most

     liv

    able

     cities

     in

     the

     world

    .

     Its

     history

     dates

     back

     to

     the

     Roman

     period

    ,

     and

     it

     has

     evolved

     into

     a

     vibrant

     and

     dynamic

    
    
    Prompt: Explain possible future trends in artificial intelligence. The future of AI is
    Generated text: 

     bright

    ,

     with

     many

     possibilities

     and

     potential

     developments

     to

     come

    .

     Here

     are

     some

     possible

     trends

     in

     AI

     that

     are

     likely

     to

     shape

     the

     industry

     in

     the

     coming

     years

    :
    


    1

    .

     Greater

     focus

     on

     ethical

     and

     responsible

     AI

    :

     The

     industry

     will

     continue

     to

     prioritize

     ethical

     and

     responsible

     AI

     practices

    ,

     especially

     when

     it

     comes

     to

     data

     privacy

    ,

     bias

    ,

     and

     transparency

    .

     This

     will

     require

     a

     greater

     emphasis

     on

     ethical

     guidelines

     and

     standards

    ,

     as

     well

     as

     increased

     investment

     in

     research

     and

     development

     to

     address

     these

     issues

    .
    


    2

    .

     Integration

     of

     AI

     with

     emerging

     technologies

    :

     AI

     is

     already

     being

     integrated

     into

     a

     wide

     range

     of

     technologies

    ,

     including

     self

    -driving

     cars

    ,

     robots

    ,

     and

     smart

     homes

    .

     As

     these

     technologies

     become

     more

    



```python
llm.shutdown()
```
