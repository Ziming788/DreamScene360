# DreamScene360 CUDA Compatibility Guide

## Issue Summary

The DreamScene360 repository requires CUDA 12.4, but your system has CUDA 12.1. This version mismatch causes specific CUDA operations to fail with errors like:
- "CUDA error: operation not supported"
- Various other CUDA-related runtime errors

## Solution

We've provided two ways to run the training script:

1. **CPU-Only Mode** (`train_cpu_only.py`): A fully CPU-based implementation that bypasses all CUDA operations.
2. **Hybrid Mode** (original `train.py` with fallbacks): Attempts to use CUDA where possible, falling back to CPU when operations are not supported.

## How to Use

### Using the Wrapper Script (Recommended)

We've created a wrapper script that handles running either mode with proper logging:

```bash
python run_training.py -s data/ocean -m output/ocean --mode cpu --self_refinement --api_key YOUR_API_KEY
```

Options:
- `-s, --source_path`: Path to source data (required)
- `-m, --model_path`: Path to output directory (required)
- `--mode`: `cpu` (fully CPU mode) or `hybrid` (attempt CUDA with fallbacks). Default: `cpu`
- `--iterations`: Number of training iterations. Default: 10000
- `--self_refinement`: Enable self-refinement (optional flag)
- `--api_key`: API key for self-refinement (required if using self-refinement)
- `--num_prompt`: Number of prompts. Default: 3
- `--max_rounds`: Maximum rounds. Default: 3
- `--log_file`: Path to log file. Default: `training_{mode}.log`

### Running Scripts Directly

#### CPU-Only Mode:
```bash
python train_cpu_only.py -s data/ocean -m output/ocean --self_refinement --api_key YOUR_API_KEY
```

#### Hybrid Mode (Original with fallbacks):
```bash
python train.py -s data/ocean -m output/ocean --self_refinement --api_key YOUR_API_KEY
```

## Performance Considerations

- **CPU-Only Mode**: Significantly slower but more stable.
- **Hybrid Mode**: Potentially faster but may encounter unexpected errors.

## Monitoring and Troubleshooting

1. Monitor training logs:
   ```bash
   tail -f training_cpu.log
   ```

2. If a process crashes, check for active processes:
   ```bash
   ps aux | grep train.py
   ```

3. To stop training:
   ```bash
   kill <PID>
   ```

## Notes on CPU Training

- CPU training will be much slower than GPU-accelerated training.
- You might want to reduce the number of iterations for testing.
- Some advanced features may have reduced quality when running on CPU.

## Further Assistance

If you continue to experience issues, consider:
1. Upgrading your CUDA installation to 12.4
2. Modifying specific operations in the codebase to be compatible with CUDA 12.1
3. Running on a different machine with CUDA 12.4 support 