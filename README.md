```markdown

最详细的流程在Instant-NGP中描述了，其余的均展示训练，测试渲染的主要步骤

---

## NeRF

```bash
### Training
python run_nerf.py --config configs/custom.txt

### Testing
python run_nerf.py --config configs/custom.txt --render_only --render_factor 0

### Rendering
python run_nerf.py --config configs/custom.txt --render_only --render_test
---

## Instant-NGP

### 数据准备流程
1. **视频采集**
   - 使用设备拍摄视频并提取帧序列
   - 使用 `ffmpeg` 提取所有帧图像

2. **数据集划分**
   ```bash
   python [处理脚本]  # 将帧序列划分为 train/test 集
   ```

3. **COLMAP 处理**
   ```bash
   colmap [命令]  # 处理 train 数据集生成稀疏重建
   ```

4. **转换 COLMAP 到 NeRF 格式**
   ```bash
   python scripts/colmap2nerf.py \
       --text ./raw_data/sparse_train_txt/ \
       --images ./data/nerf/custom/images/ \
       --colmap_camera_model OPENCV \
       --out ./data/nerf/custom/transforms.json
   ```

---

### 云平台训练
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

### 测试推理
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
ffmpeg -framerate 10 -i "D:\Users\HUAWEI\Desktop\CV\instant-ngp\pic\frame_%04d.jpg" \
  -c:v libx264 -pix_fmt yuv420p -crf 18 "D:\Users\HUAWEI\Desktop\output.mp4"
```

---

## 3D Gaussian Splatting

### 训练
```bash
python train.py -s ./data/custom/3d_gauss
```

### 渲染
```bash
python render.py -m ./output/ba231925-f -s ./data/custom/3d_gauss
```

---

> ⚠️ 注意事项：
> - Windows 路径需使用双引号包裹（如 `ffmpeg` 命令）
> - 实际路径需根据项目结构替换（如 `./data/nerf/custom/`）
> - 所有参数（如 `--n_steps`）可根据硬件条件调整
```

---
