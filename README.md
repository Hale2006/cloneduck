# Microduck RL — How to Run

Fork of [pollen-robotics/microduck_rl](https://github.com/pollen-robotics/microduck_rl) — PPO walking policy for the Microduck bipedal robot, trained in MuJoCo via [mjlab](https://github.com/mujocolab/mjlab).

## Requirements
- Windows or Linux
- NVIDIA GPU with CUDA
- [uv](https://docs.astral.sh/uv/) package manager

## Install
```powershell
git clone <your-fork-url>
cd microduck_rl
uv sync
```

**Windows only:** PyPI ships a CPU-only PyTorch build for Windows by default. Verify first:
```powershell
.venv\Scripts\Activate.ps1
python -c "import torch; print(torch.__version__); print(torch.cuda.is_available())"
```
If it prints `False`, reinstall the correct CUDA build for your GPU:
```powershell
# Ampere-class GPUs (RTX A2000, RTX 30-series...)
uv pip install torch --index-url https://download.pytorch.org/whl/cu124 --force-reinstall --no-deps

# Blackwell-class GPUs (RTX 50-series, sm_120)
uv pip install torch --index-url https://download.pytorch.org/whl/cu128 --force-reinstall --no-deps
```
From here on, always activate the venv and call commands directly (`train`, `play`, `python ...`) — avoid `uv run`, which re-syncs the environment and reverts this fix.

## Train
```powershell
.venv\Scripts\Activate.ps1
$env:WANDB_MODE="offline"
train Mjlab-Velocity-Flat-MicroDuck --env.scene.num-envs 1024
```
Adjust `--env.scene.num-envs` to your GPU's VRAM: 512–1024 on 4GB, 2048–4096 on 8GB+.

## View a trained checkpoint
```powershell
play Mjlab-Velocity-Flat-MicroDuck --checkpoint-file "imported_checkpoint\model_15500.pt"
```
Opens a `viser` web viewer at `http://localhost:8080`. To export a short clip instead:
```powershell
play Mjlab-Velocity-Flat-MicroDuck --checkpoint-file "imported_checkpoint\model_15500.pt" --video True --video-length 600
```
## Video Submission

[🎥 Watch the Project Video]((https://drive.google.com/file/d/1LTqDaGlrZoGJ_njyOPqeICAB_Xhej9aN/view?usp=sharing)
git add README.md
git commit -m "Add project video link"
git push
