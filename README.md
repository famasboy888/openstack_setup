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

## Create cinder volume
*Go and add an external drive and attach to VMM
### List Disks
```bash
lsblk
```
### Create Physical Volume and Volume Group for Cinder using the newly attached disk
```bash
sudo pvcreate /dev/sdb
sudo vgcreate cinder-volumes /dev/sdb
sudo vgs
```

## Install Python build dependencies:
```bash
sudo apt install git python3-dev libffi-dev gcc libssl-dev -y

sudo apt install python3-venv -y
```

## Create a virtual environment, change owner and activate it:
```bash
sudo python3 -m venv /path/to/venv

sudo chown -R stack:stack /path/to/venv

source /path/to/venv/bin/activate
```
