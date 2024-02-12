# install docker 
https://linuxconfig.org/how-to-install-docker-on-ubuntu-20-04-lts-focal-fossa  

**in case** ```docker run hello-world``` **still gives permission denied**  
```
sudo groupadd docker  
sudo gpasswd -a $USER docker  
newgrp docker
```

# install nvidia docker

### Add the package repositories
```
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```
### Test nvidia-smi with the latest official CUDA image
```
docker run --gpus all 12.3.1-base-ubuntu20.04 nvidia-smi
```

# Dockerfile

### Build docker - 2 options:
```
./build_docker.sh
```
##### or
```
docker build -t keras-gpu-moli:latest -f keras-gpu-moli.dockerfile .
```

### If you want to use GPU -> set docker default runtime
1. Create ```/etc/docker/daemon.json```
2. Edit ```/etc/docker/daemon.json``` to:  
```json
{  
  "default-runtime": "nvidia",  
    "runtimes": {  
      "nvidia": {  
        "path": "/usr/bin/nvidia-container-runtime",  
        "runtimeArgs": []  
      }  
  }  
}  
```

3. If no ```/usr/bin/nvidia-container-runtime``` is present, you might have to install nvidia-container-runtime  
https://github.com/NVIDIA/nvidia-container-runtime
