# cv_fdu_final

NeRF:

train：python run_nerf.py --config configs/custom.txt
test：python run_nerf.py --config configs/custom.txt --render_only --render_factor 0
渲染：python run_nerf.py --config configs/custom.txt --render_only --render_test

=======================================================================================================================================

instant-NGP：

拍摄视频，使用ffmpeg得到所有帧
python脚本处理得到train，test
colmap处理train数据集
上传云平台进行训练：
准备images：从train中复制图像

把sparse转化为transforms.json
python scripts/colmap2nerf.py \
    --text ./raw_data/sparse_train_txt/ \
    --images ./data/nerf/custom/images/ \
    --colmap_camera_model OPENCV \
    --out ./data/nerf/custom/transforms.json
    
开始训练：
python scripts/run.py \
  --scene data/nerf/custom \
  --n_steps 35000 \
  --save_snapshot ./results/custom.msgpack \
  --save_mesh ./results/custom.ply \
  --marching_cubes_res 512 \
  --sharpen 0.7 \
  --exposure 0.2  
  
进行测试：使用transforms和images，就是全量数据
python scripts/run.py --scene data/nerf/custom/ --mode nerf --load_snapshot results/custom.msgpack --screenshot_transforms data/nerf/custom/transforms.json --screenshot_dir outputs/custom/screenshot --width 2048 --height 2048 --n_steps 0

重新合成视频：
ffmpeg -framerate 10 -i "D:\Users\HUAWEI\Desktop\CV\instant-ngp\pic\frame_%04d.jpg" -c:v libx264 -pix_fmt yuv420p -crf 18 "D:\Users\HUAWEI\Desktop\output.mp4"

=======================================================================================================================================

3D Gaussian Splatting：
训练：
python train.py \
  -s ./data/custom/3d_gauss
  
渲染：
python render.py -m ./output/ba231925-f -s ./data/custom/3d_gauss
