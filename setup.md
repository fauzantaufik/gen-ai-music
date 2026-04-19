## AudioCraft Setup (Windows, Conda only)

This guide uses only the Conda method that worked reliably on Windows.

## 1) Prerequisites

1. Miniconda or Anaconda installed
2. NVIDIA driver installed if using GPU

## 2) Create and activate environment

```powershell
conda create -n audiocraft python=3.11 -y
conda activate audiocraft
```

## 3) Install PyTorch (CUDA build — required for GPU)

**Important:** Do NOT install PyTorch from PyPI (`pip install torch`) — it only has the CPU build.
Always use PyTorch's own index URL to get the CUDA build.

### GPU path (CUDA 12.1) — recommended for RTX 3050 / driver CUDA 12.x

```powershell
pip install "torch==2.1.0+cu121" "torchaudio==2.1.0+cu121" "torchvision==0.16.0+cu121" --index-url https://download.pytorch.org/whl/cu121
```

### GPU path (CUDA 11.8) — alternative if cu121 is unavailable

```powershell
pip install "torch==2.1.0+cu118" "torchaudio==2.1.0+cu118" "torchvision==0.16.0+cu118" --index-url https://download.pytorch.org/whl/cu118
```

### CPU-only path (slow — not recommended)

```powershell
pip install "torch==2.1.0" "torchaudio==2.1.0" --index-url https://download.pytorch.org/whl/cpu
```

> **Note:** Your driver's max CUDA version is shown by `nvidia-smi` (top-right corner).
> Any PyTorch CUDA build version ≤ that number will work.

## 4) Install transformers (must be 4.x — compatible with torch 2.1.0)

```powershell
pip install "transformers==4.31.0"
```

> **Warning:** transformers 5.x requires PyTorch >= 2.4, which conflicts with audiocraft's
> requirement of torch==2.1.0. Always pin to 4.x.

## 5) Install FFmpeg + PyAV binaries

```powershell
conda install ffmpeg "av=11.*" -c conda-forge -y
```

## 6) Install AudioCraft + extras

```powershell
pip install -U audiocraft ipywidgets
```

## 7) Verify installation

```powershell
python -c "import audiocraft, torch; print('AudioCraft:', audiocraft.__version__); print('Torch:', torch.__version__); print('CUDA available:', torch.cuda.is_available()); print('GPU:', torch.cuda.get_device_name(0) if torch.cuda.is_available() else 'N/A')"
```

Optional smoke test (downloads musicgen-small model ~600MB):

```powershell
python -c "from audiocraft.models import MusicGen; model = MusicGen.get_pretrained('facebook/musicgen-small'); print('Model loaded')"
```

## 8) Reproducible command sequence (copy/paste)

```powershell
conda create -n audiocraft python=3.11 -y
conda activate audiocraft

# GPU (CUDA 12.1) — recommended:
pip install "torch==2.1.0+cu121" "torchaudio==2.1.0+cu121" "torchvision==0.16.0+cu121" --index-url https://download.pytorch.org/whl/cu121

# transformers 4.x (required — do not upgrade to 5.x):
pip install "transformers==4.31.0"

conda install ffmpeg "av=11.*" -c conda-forge -y
pip install -U audiocraft ipywidgets

python -c "import audiocraft, torch; print(audiocraft.__version__, torch.__version__, torch.cuda.is_available())"
```

## Known issues

| Error | Cause | Fix |
|---|---|---|
| `T5EncoderModel requires PyTorch` | transformers 5.x installed | `pip install "transformers==4.31.0"` |
| `CUDA available: False` | CPU-only torch installed | Reinstall torch with `+cu121` build from pytorch.org index |
| `No module named 'ipywidgets'` | Missing package | `pip install ipywidgets` |
