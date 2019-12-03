# Docker
Docker安裝與使用

# 安裝
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


    
    
   

