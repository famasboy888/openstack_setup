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

```bash
Output:
loop0                       7:0    0 63.9M  1 loop /snap/core20/2105
loop1                       7:1    0   87M  1 loop /snap/lxd/27037
loop2                       7:2    0 40.4M  1 loop /snap/snapd/20671
sda                         8:0    0   40G  0 disk
├─sda1                      8:1    0    1M  0 part
├─sda2                      8:2    0    2G  0 part /boot
└─sda3                      8:3    0   38G  0 part
  └─ubuntu--vg-ubuntu--lv 253:0    0   38G  0 lvm  /
sdb                         8:16   0   20G  0 disk         <============ Check if present
sr0                        11:0    1 1024M  0 rom
```
### Create Physical Volume and Volume Group for Cinder using the newly attached disk
```bash
sudo pvcreate /dev/sdb
sudo vgcreate cinder-volumes /dev/sdb
sudo vgs
```
### Check that it was created
```bash
VG             #PV #LV #SN Attr   VSize   VFree
  cinder-volumes   1   0   0 wz--n- <20.00g <20.00g    <========Successfully created!
  ubuntu-vg        1   1   0 wz--n- <38.00g      0
```
#### Or
```bash
sudo vgdisplay
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

## Ensure the latest version of pip is installed inside the activated Python Environment(venv):
```bash
pip install -U pip
```

## Install Ansible:
```bash
pip install 'ansible-core>=2.15'
```

## Install kolla-ansible and its dependencies using pip:
```bash
pip install git+https://opendev.org/openstack/kolla-ansible@master
```

## Create the /etc/kolla directory:
```bash
sudo mkdir -p /etc/kolla
sudo chown $USER:$USER /etc/kolla
```

## Copy globals.yml and passwords.yml to /etc/kolla directory:
```bash
cp -r /path/to/venv/share/kolla-ansible/etc_examples/kolla/* /etc/kolla
```

## Copy all-in-one inventory file to the current directory:
```bash
cp /path/to/venv/share/kolla-ansible/ansible/inventory/all-in-one .
```

## Install Ansible Galaxy dependencies:
```bash
kolla-ansible install-deps
```

 ## Run random password generator:
 ```bash
kolla-genpwd
```

 ## Edit **/etc/kolla/globals.yml** using nano:
  ```bash
sudo nano /etc/kolla/globals.yml
```

 ## Deployment (Wait for each commands to finish):
   ```bash
kolla-ansible -i ./all-in-one bootstrap-servers

kolla-ansible -i ./all-in-one prechecks

kolla-ansible -i ./all-in-one deploy
```
