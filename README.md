# Deep Learning Super-Resolution (geoskill-super-resolution-dl)

> 2x/3x/4x image super-resolution based on the SRCNN (Dong 2014) convolutional neural network, with training and inference on CUDA GPU; outputs high-resolution rasters and PSNR/SSIM evaluation

---

## 1. Overview

Upscales low-resolution remote sensing imagery to high resolution while enhancing detail, outputting the super-resolved raster and quality assessment metrics (PSNR/SSIM). **Core model**: **SRCNN** (Super-Resolution Convolutional Neural Network, Dong et al. 2014, ECCV/TPAMI) — a 3-layer fully convolutional network (9×9 / 5×5 / 5×5 kernels, 1→64→32→1 channels, ReLU) that takes the bicubic upsampled result as input and learns the HR − bicubic residual; the network architecture, training objective (pixel-wise MSE) and inference pipeline are consistent with the original SRCNN. All training/inference runs on a **CUDA GPU** (torch >= 2.x); the skill ships with pre-trained weights ``srcnn_weights.pt`` (trained on synthetic ground truth); if the weights are missing, they are automatically trained on the GPU at first run and cached to disk. Synthetic mode executes a self-consistent "high-resolution ground truth → downsampling → super-resolution" experiment that directly quantifies restoration quality and compares it against the ``--psnr_bicubic_only`` bicubic baseline.

## 3. Quick Start

```bash
pip install -r requirements.txt
python geoskill-super-resolution-dl.py --bbox 116 39 117 40 --synthetic --output-dir ./out
```

## 4. CLI Parameters

Run `python <skill>.py --help` for the full list. Common parameters:

| Parameter | Type | Description |
|---|---|---|
| `--bbox` | `float[4]` | WGS84 bounding box `min_lon min_lat max_lon max_lat` |
| `--input` | `path` | Local input file (GeoJSON/GeoTIFF/etc.) |
| `--output-dir` | `path` | Output directory (default `./output`) |
| `--synthetic` | `flag` | Use synthetic data instead of real input |
| `--quiet` | `flag` | Suppress non-essential stdout |

## 5. Input / Output

| File | Format | Description |
|---|---|---|
| `super_resolved.tif` | GeoTIFF | Super-resolved high-resolution raster |
| `quality_metrics.json` | JSON | PSNR/SSIM and comparison against the pure bicubic baseline |
| `output-manifest.json` | JSON | Run manifest (input/output/QA/exit code) |

## 6. Technical Principle

(see SKILL.md for details)

## 8. License

MIT License. See [`LICENSE`](./LICENSE) for full text.

---

> 中文完整版：[references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)
