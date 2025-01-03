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

# 02 Usage

```bash
CUDA_VISIBLE_DEVICES=0 python ./omg_llava/tools/chat_omg_llava.py  ./omg_llava/configs/finetune/omg_llava_7b_finetune_8gpus.py  /download/omg_llava_7b_finetune_8gpus.pth --image ../demo/images/test3.jpg
```

# Debug
- 1、缺少权重文件，config文件配置不对
![图片](https://github.com/user-attachments/assets/e737613e-115b-4869-a27c-273aef0f7780)
- 【解决方案】Hugging face下载权重
- 设置环境变量 + 使用 huggingface-cli download下载
- 下载后如果没有设置local_dir可能会存放在缓存位置，需要自行移动出来
```bash
pip install -U huggingface_hub
export HF_ENDPOINT=https://hf-mirror.com
huggingface-cli download --resume-download zhangtao-whu/OMG-LLaVA --local-dir /download
```

source bin/activate 进入llava-seg-env
docker commit

docker run --name lkp_container -it --gpus all -v  /home/liukunpeng:/workspace -v /data1:/data 10.108.0.3:15080/public/llm-torch2.0.1:0.8-flash-attn-A100 /bin/bash
