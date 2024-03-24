# openstack_setup

## Initial steps
```bash
sudo apt update && sudo apt upgrade -y

sudo useradd -s /bin/bash -d /opt/stack -m stack

sudo chmod +x /opt/stack

echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack

sudo -u stack -i

sudo apt-get install git screen nano -y

wget https://github.com/k-dev1234/libthai-data_0.1.29-1build1_all/raw/main/libthai-data_0.1.29-1build1_all.deb && \

sudo dpkg -i libthai-data_0.1.29-1build1_all.deb
```

## Install Python build dependencies:
```bash
sudo apt install git python3-dev libffi-dev gcc libssl-dev -y
```
