<h3 align="center">
  <img src="https://d20vrrgs8k4bvw.cloudfront.net/images/courses/logos/logo-color-tensorflow.png" width="300">
</h3>

## Install tensorflow-gpu on a Tesla ec2 in AWS

* #### [Install Docker engine](https://docs.docker.com/engine/install/ubuntu/)
```
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
```
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
```
sudo apt update && sudo apt install docker-ce docker-ce-cli containerd.io
```
****

* #### Install Nvidia-docker
```
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
```
```
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```
```
sudo apt-get update && sudo apt-get install -y nvidia-container-toolkit
sudo systemctl restart docker
```
****

* #### Install Nvidia graphics driver (Tesla)
```
wget http://us.download.nvidia.com/tesla/418.152.00/nvidia-driver-local-repo-ubuntu1804-418.152.00_1.0-1_amd64.deb
sudo apt-key add /var/nvidia-driver-local-repo-ubuntu1804-418.152.00/7fa2af80.pub
sudo dpkg -i nvidia-driver-local-repo-ubuntu1804-418.152.00_1.0-1_amd64.deb
sudo apt-get install -f
```
****

* #### Install Cuda toolkit & cuDNN
```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-ubuntu1804.pin
sudo mv cuda-ubuntu1804.pin /etc/apt/preferences.d/cuda-repository-pin-600
```
```
sudo apt-key adv --fetch-keys  http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
sudo bash -c 'echo "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda.list'
sudo bash -c 'echo "deb http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64 /" > /etc/apt/sources.list.d/cuda_learn.list'
sudo apt update
sudo apt install cuda-10-1
sudo apt-get install --no-install-recommends \
    libcudnn7=7.6.5.32-1+cuda10.1  \
    libcudnn7-dev=7.6.5.32-1+cuda10.1
```
****

* #### Run tensorflow-gpu in a container & access it from a jupyter notebook
```
sudo docker run -it --gpus all -v /home/ubuntu/tf:/tf/ -p 80:8888 tensorflow/tensorflow:2.2.0-gpu-jupyter
```
The notebook will be accessible at `http://{ec2-ip}/?token={token}`
