# Docker安裝與使用Docker執行Tensorflow

# 安裝Docker
環境：ubuntu 18.04

1開啟終端機
 
2輸入下面指令安裝 Docker：
    
    sudo sh -c "$(curl -fsSL https://get.docker.com)"
    
    sudo usermod -aG docker danny



說明：
第一行用 docker 官方提供的 script 快速安裝
第二行則是將現有的使用者加入 docker 群組，否則會沒有權限操作 docker 指令
請依自已的使用者名稱更換掉danny

3登出或重開機

4驗證安裝是否成功可採下列其中之一方法
   
   開啟終端機，輸入 ：

    docker run hello-world
    
 出現 "Hello from Docker!" 表示安裝成功
 
 也可以輸入：
 
    docker -v
    
出現 版本訊息 表示安裝成功

也可以輸入：

    docker info

會出現系統資訊畫面，包括CPU數，記憶體大小以及目前有的docker映像檔數等等

也可以輸入：

     docker

會列出docker指令表     


# 安裝Nvdia-docker 

環境：Linux(EX： ubuntu 18.04）、Nvidia 顯卡

Docker是在GPU上運行TensorFlow的最簡單方法，因為主機只需安裝NVIDIA®驅動程序（無需安裝NVIDIA® CUDA®工具包）。

1.檢查GPU 是否可用，輸入：

    lspci | grep -i nvidia
    
2.安裝nvidia-docker、啟動支持NVIDIA® GPU的Docker容器。依序輸入下列數行指令：

    distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
    curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
    curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
    sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
    sudo systemctl restart docker
    
測試是否成功輸入：

    docker run --gpus all nvidia/cuda:9.0-base nvidia-smi
    
    docker pull tensorflow/tensorflow:1.13.1-gpu-jupyter
    
# 下載TensorFlow Docker 映像 

    docker pull tensorflow/tensorflow                     
    docker pull tensorflow/tensorflow:devel-gpu          
    docker pull tensorflow/tensorflow:latest-gpu-jupyter 
以上三行指令分別為：# latest stable release ， # nightly dev release w/ GPU support  ， # latest release w/ GPU support and Jupyter

    docker pull tensorflow/tensorflow:1.13.1-gpu-jupyter-py3
 [重要]下載tensorflow指定1.13.1版，指定gpu版本，指定有jupyter, 指定為python3(否則是2.7)

    docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu-py3
 以上指令才會用py3, 否則是2.7
 
使用Docker執行Tensorflow-GPU    
    
3.下載並運行支持GPU 的TensorFlow 映像（需要幾分鐘的時間），輸入下列二行：

    docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu \
    python -c "import tensorflow as tf; tf.enable_eager_execution(); print(tf.reduce_sum(tf.random_normal([1000, 1000])))"
    
    
    docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu \
   python -c "import tensorflow as tf; tf.enable_eager_execution(); print(tf.reduce_sum(tf.random_normal([1000, 1000])))"
   
   docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu \
   python -c "import tensorflow as tf; print(tf.reduce_sum(tf.random_normal([1000, 1000])))"

使用Docker執行Tensorflow-GPU

列出本機的所有 image 文件。
docker image ls


docker pull tensorflow/tensorflow:1.13.1-gpu-jupyter
sudo systemctl hibernate
