
# Docker安裝與使用Docker執行Tensorflow

# 安裝Docker

@@@@@@@@@@@@@@@@@@(完整說明) https://glints.com/tw/blog/docker-basic-tutorial/ 

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
    
# 下載TensorFlow Docker 映像  （可跳過）
參考：https://www.tensorflow.org/install/docker
範例指令：
    docker pull tensorflow/tensorflow                     
    docker pull tensorflow/tensorflow:devel-gpu          
    docker pull tensorflow/tensorflow:latest-gpu-jupyter 
以上三行指令分別為：# latest stable release ， # nightly dev release w/ GPU support  ， # latest release w/ GPU support and Jupyter

我們採用下面下載指令：

    docker pull tensorflow/tensorflow:1.13.1-gpu-py3-jupyter
指令說明：下載tensorflow指定1.13.1版，指定gpu版本，指定有jupyter, 指定為python3(否則是2.7)

    
使用Docker執行Tensorflow-GPU    
    
# 下載並運行支持GPU 的TensorFlow 映像（需要幾分鐘的時間），輸入下列二行：
若沒有執行上面的「下載TensorFlow Docker 映像」，直接打下面這行指令也可以，能同時下載與執行
注意下面指令有同時引入Nvdia-docker 
    
    docker pull tensorflow/tensorflow:1.13.1-gpu-py3
    
     docker run --gpus all --rm -it  tensorflow/tensorflow:1.13.1-gpu-py3-jupyter
    docker run --gpus all nvidia/cuda:9.0-base --rm -it  tensorflow/tensorflow:1.13.1-gpu-jupyter-py3 
    
    
    docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu \
    python -c "import tensorflow as tf; tf.enable_eager_execution(); print(tf.reduce_sum(tf.random_normal([1000, 1000])))"
    
    
    docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu \
   python -c "import tensorflow as tf; tf.enable_eager_execution(); print(tf.reduce_sum(tf.random_normal([1000, 1000])))"
   
   docker run --gpus all -it --rm tensorflow/tensorflow:latest-gpu \
   python -c "import tensorflow as tf; print(tf.reduce_sum(tf.random_normal([1000, 1000])))"

使用Docker執行Tensorflow-GPU

列出本機的所有 image 文件。
docker image ls

如果確認是跑完就不再使用的，可以在執行時加上 --rm 參數，當容器終止時會自動刪除，很方便
跟容器互動，可以加上 -it


docker pull tensorflow/tensorflow:1.13.1-gpu-jupyter

Hibernate 模式，將記憶體內容寫入硬碟後完全關閉電源，等同 Windows 的休眠模式。
     
     sudo systemctl hibernate
     
     
     docker ps -a
     
     
Remove:

    docker rmi Image


If you want to cleanup docker images and containers

CAUTION: this will flush everything

停止全部containers指令：stop all containers

    docker stop $(docker ps -a -q)

刪除全部containers指令： remove all containers

    docker rm $(docker ps -a -q)

刪除全部image 指令：remove all images

    docker rmi -f $(docker images -a -q)

=======================================no use 

裝 ubuntu
docker pull ubuntu

啟動它
docker run -it ubuntu


保存 docker 的狀態的話

先用下面的指令查看一下它的版本號

1
docker ps -l
docker ps -a   =>>all

list all contantor
docker ps -a


上面這個指令可以幫助你取得 ID

然後就可以使用下面的指令把變動 commit 上去了

docker commit [ID] [CONTAINER] [REPOSITORY[:TAG]]

一個使用實例大概看起來像這樣

1
docker commit XXX any-name/ubuntu

docker start c372268f2810


run containtor 
docker exec -i -t c372268f2810 bash


記錄：
ubuntu 支援 CUDA10, 不支援CUDA 9
網友：tensorflow 1.13.1需要cudnn版本7.4.1。更新您的cudnn，它將起作用。
     


ps - 顯示容器清單
首先是 ps，列出目前執行中的容器。雖然沒有明確說明，猜想是 linux 的 ps 指令，也就是 process status

docker ps
如果想要看到全部，包含已停止的，加上 -a 參數，代表 all 全部

docker ps -a
如果有照著之前執行過 hello-world，就會發現他出現在清單上，而且是 Exit 狀態。




======================開始一步一步建containtor內容======================

apt-get update
apt-get install python3.6



====================================================================
docker pull nvidia/cuda:10.1-cudnn7-runtime-ubuntu18.04

docker run --gpus all nvidia/cuda:10.1-cudnn7-runtime-ubuntu18.04 nvidia-smi
 docker run --gpus all --rm -it  tensorflow/tensorflow:1.13.1-gpu-py3

docker run --gpus all -it --rm tensorflow/tensorflow:1.13.1-gpu-py3 \


docker run --gpus all -it --rm tensorflow/tensorflow:1.13.1-gpu-py3 bash



// 拷貝資料夾到容器中
Copy all files to folder inside container:

docker cp ./src/build/. ContainerName:/app/
above example shows all files inside build folder are copying to app folder inside container

//linux 的object detection api 路徑設定
export PYTHONPATH="${PYTHONPATH}:/home/siquare01/project/ObjectDetection/models:/home/siquare01/project/ObjectDetection/models/research:/home/siquare01/project/ObjectDetection/models/research/slim"


//安裝包
pip install pillow
pip install lxml 
pip install Cython
pip install contextlib2
pip install jupyter
pip install matplotlib
pip install pandas
pip install opencv-python


任意切換tensorflow 版本（自動反安裝舊版加安裝新的版）
pip install --upgrade tensorflow-gpu==1.8.0


docker build -t cuda9ubuntu18 .

DannyXXXX!

檢查tensorflow板本 ：

python3 -c 'import tensorflow as tf; print(tf.__version__)'

# 檢查cuda 版本

    nvcc  --version
    
# 檢查cudnn 版本

    cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2

# 使用NVDIA NGC 下載容器


推薦各位使用Docker + NVIDIA Docker
其實沒有那麼複雜，只要Linux OS和GPU Driver安裝完成之後把Docker裝起來就可以跑了
Docker image來源可以使用NVIDIA GPU Cloud(免費註冊)，主流的那幾個Framework都有提供，每個月都會定期更新，針對GPU計算做最佳化
https://ngc.nvidia.com/catalog/containers?query=&quickFilter=deep-learning&filters=&orderBy=

要先註冊： 超級mmi DannyXXXX!

安裝CLI https://ngc.nvidia.com/setup/installers/cli

下指令

    ngc config set
    docker login nvcr.io
    Username: $oauthtoken
    Password: amc2bXZlbGFsMHR1YjlxNzFrbjZkdDFkZG46ZDljYmIxZDctYjcyNC00Mjc3LWEzOTUtM2JhZWFlZDE0ZGVl
    
上面那串金龠若失效要重新取得，從NGC網站Setup -> API Key

NCG帳密：fXXXXXXXX@  ,   DannyXXXX!

docker pull nvcr.io/nvidia/tensorflow:19.11-tf1-py3

執行image:

docker run --gpus all -it --rm -v /home:/home nvcr.io/nvidia/tensorflow:19.11-tf1-py3

第一個/home是實際電腦中/home，第二個/home是 containtor  中/home

舊版如下：docker pull nvcr.io/nvidia/tensorflow:19.06-py3




=====================uninstall nvidia driver===============================
sudo apt-get remove --purge nvidia*

sudo apt-get remove --purge nvidia*

    sudo apt-get remove --purge nvidia*

w
