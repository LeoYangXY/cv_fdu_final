```markdown
# CV_FDU_FINAL 实验流程记录

## NeRF 实验流程

### 训练
```bash
python run_nerf.py --config configs/custom.txt
```

### 测试
```bash
python run_nerf.py --config configs/custom.txt --render_only --render_factor 0
```

### 渲染
```bash
python run_nerf.py --config configs/custom.txt --render_only --render_test
```

---

## Instant-NGP 实验流程

### 数据准备
1. 拍摄视频并使用ffmpeg提取所有帧
2. 使用Python脚本处理得到train/test数据集
3. 使用colmap处理train数据集

### 训练准备
```bash
# 转换sparse为transforms.json
python scripts/colmap2nerf.py \
    --text ./raw_data/sparse_train_txt/ \
    --images ./data/nerf/custom/images/ \
    --colmap_camera_model OPENCV \
    --out ./data/nerf/custom/transforms.json
```

### 开始训练
```bash
python scripts/run.py \
  --scene data/nerf/custom \
  --n_steps 35000 \
  --save_snapshot ./results/custom.msgpack \
  --save_mesh ./results/custom.ply \
  --marching_cubes_res 512 \
  --sharpen 0.7 \
  --exposure 0.2
```

### 测试
```bash
python scripts/run.py \
  --scene data/nerf/custom/ \
  --mode nerf \
  --load_snapshot results/custom.msgpack \
  --screenshot_transforms data/nerf/custom/transforms.json \
  --screenshot_dir outputs/custom/screenshot \
  --width 2048 \
  --height 2048 \
  --n_steps 0
```

### 视频合成
```bash
ffmpeg -framerate 10 -i "D:\Users\HUAWEI\Desktop\CV\instant-ngp\pic\frame_%04d.jpg" -c:v libx264 -pix_fmt yuv420p -crf 18 "D:\Users\HUAWEI\Desktop\output.mp4"
```

---

## 3D Gaussian Splatting 实验流程

### 训练
```bash
python train.py -s ./data/custom/3d_gauss
```

### 渲染
```bash
python render.py -m ./output/ba231925-f -s ./data/custom/3d_gauss
```
```
