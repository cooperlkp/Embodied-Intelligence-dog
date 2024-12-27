# 00 环境要求
- 环境要求
```bash
CUDA==11.8
Ubuntu==20.04(focal)
```
## 0.1 CUDA install
- CUDA安装 https://developer.nvidia.com/cuda-toolkit-archive
- CUDNN版本 https://developer.nvidia.com/rdp/cudnn-archive
## 0.2 Pytorch install
- pytorch版本安装 https://pytorch.org/get-started/locally/
```bash
pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```
## 0.3 Install mmengine
```bash
python -m pip install https://github.com/open-mmlab/mmengine/archive/refs/tags/v0.8.5.zip
```


# 01 文件结构
- ├── eigen-3.3.7
- ├── glfw-3.3.5
- ├── hpp-fcl
- ├── lcm
- ├── mujoco-2.3.5
- ├── qpOASES
- └── src.zip
- 其中src中内含readme
