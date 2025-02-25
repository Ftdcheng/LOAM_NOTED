# kitti2rosbag

这个jupter notebook可以把kitti里的点云转换成rosbag

## 使用方法

1. 创建虚拟环境
```bash
python -m venv k2b
```
2. 添加ROS python相关的包进入虚拟环境
```bash
cd k2b 
bash use_ros_in_venv.sh
. k2b/bin/activate
```
3. 安装依赖
```bash
sudo apt install ros-${ROS-DISTRO}-rosbag
pip install -r requirements.txt
```
4. 使用jupyter notebook