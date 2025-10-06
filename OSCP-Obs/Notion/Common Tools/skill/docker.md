```
sudo docker run --rm -it -v "$PWD":/work -w /work ubuntu:18.04 
```
将目录从主机系统挂载到容器中，以便于数据共享。