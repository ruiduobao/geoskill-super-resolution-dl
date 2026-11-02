---
name: geoskill-super-resolution-dl
description: '基于 SRCNN（Dong 2014）卷积神经网络的 2x/3x/4x 影像超分辨率，在 CUDA GPU 上训练与推理，输出高分辨率栅格与 PSNR/SSIM 评估'
---

# 深度学习超分辨率 | Deep Learning Super-Resolution

Upscales low-resolution remote sensing imagery to high resolution while enhancing detail, outputting the super-resolved raster together with quality metrics (PSNR/SSIM).

**Core model**: **SRCNN** (Super-Resolution Convolutional Neural Network, Dong et al. 2014, ECCV/TPAMI) — a 3-layer fully convolutional network (9×9 / 5×5 / 5×5 kernels, 1→64→32→1 channels, ReLU) that takes the bicubic-upsampled result as input and learns the HR − bicubic residual. The network architecture, training objective (pixel-wise MSE) and inference pipeline match the original SRCNN.

All training/inference runs on a **CUDA GPU** (torch >= 2.x). Pre-trained weights ``srcnn_weights.pt`` ship with the skill (trained on synthetic ground truth); if the weights are missing, they are automatically trained on the GPU at first run and cached to disk.

Synthetic mode runs the self-consistent "high-resolution ground truth → downsampling → super-resolution" experiment, directly quantifying restoration quality and comparing against the bicubic baseline with ``--psnr_bicubic_only``.

## Dependencies / 依赖

```bash
pip install numpy rasterio scipy torch  # torch requires CUDA build for GPU inference
```

## Usage / 使用方法

### Example 1 (synthetic data, offline)

```bash
python geoskill-super-resolution-dl.py --bbox 116.0 39.0 117.0 40.0 --synthetic --output-dir ./out
```

### Example 2: 3x upscaling of a real low-resolution image

```bash
python geoskill-super-resolution-dl.py --input low.tif --scale 3 --amount 0.7 --output-dir ./out
```

## Output / 输出

| File | Format | Description |
|---|---|---|
| `super_resolved.tif` | GeoTIFF | Super-resolved high-resolution raster |
| `quality_metrics.json` | JSON | PSNR/SSIM and comparison against the pure bicubic baseline |
| `output-manifest.json` | JSON | Run manifest (input/output/QA/exit code) |

## Data Source / 数据源 / Source

A local single-band GeoTIFF, or `--synthetic` (downsample the high-resolution ground truth to obtain the low-resolution input; the ground truth is retained for evaluation).

## Privacy / 隐私声明 / Privacy

- Runs offline by default; `--synthetic` mode requires no network at all.
- All processing is done locally; no user data is uploaded.

## License / License

MIT

---

> 中文完整版：需要时再读 [references/SKILL.zh-CN.md](references/SKILL.zh-CN.md)，不要默认全量加载。
