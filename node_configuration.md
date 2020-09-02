# Configure Node

## Configure allow root ssh login
```bash
vim /etc/ssh/sshd_config
change "PermitRootLogin yes"
passwd root
service sshd restart
```
## login without passsword
```bash
ssh-copy-id root@gpunode-1-4
```

## Pastas compartilhadas
No nó
```bash
apt-get update
apt-get install build-essential nfs-common htop
```
Edit /etc/fstab with shared folders in cluster
```bash
vim /etc/fstab
mkdir /share
mkdir /share_beta
mkdir /share_gamma
mkdir /share_alpha
mount -a
```

## Install NVIDIA
```bash
wget https://developer.download.nvidia.com/compute/cuda/11.0.3/local_installers/cuda_11.0.3_450.51.06_linux.run
sudo sh cuda_11.0.3_450.51.06_linux.run
````

## Install DOCKER
```bash
apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
	
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
apt update
```

## Show docker versions
```bash
apt-cache madison docker-ce
apt-get install docker-ce=5:18.09.2~3-0~ubuntu-xenial docker-ce-cli=5:18.09.2~3-0~ubuntu-xenial containerd.io
```

## Install nis client
https://www.server-world.info/en/note?os=Ubuntu_16.04&p=nis&f=2

## Install Torque 
No gpucluster
```bash
scp torque-6.1.2.tar.gz gpunode-1-4:/root/
```
No nó
```bash
apt-get update
apt-get install -y libxml2-dev
apt-get install -y zlib1g-dev
apt-get install libboost-all-dev
apt install libssl-dev
apt install libhwloc-dev

./configure --with-debug --enable-nvidia-gpus --with-nvml-lib=/usr/lib64 \
--with-nvml-include=/usr/local/cuda-11.0/targets/x86_64-linux/include \
--with-hwloc-path=/usr
make -j4
make install
````

## Torque Configuration
edit /var/spool/torque/server_name
gpucluster.ica.ele.puc-rio.br

## Iptables
iptables configuration -> gpunode-1-1:/root/iptables.txt
iptables-restore < iptables.txt
service pbs_mom restart

### No gpucluster:
edit /var/spool/torque/serv_priv/nodes
adicionar o nó

qterm
pbs_server

## Install NVIDIA-DOCKER2
```bash
sudo systemctl start docker && sudo systemctl enable docker
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
sudo apt-get install -y nvidia-docker2
sudo systemctl restart docker
```

### Add user to docker group
vim /etc/group
adicionar os usuarios no grupo docker
docker:x:999:....

# help: find files in folders
find /usr -type f -name nvml.h







