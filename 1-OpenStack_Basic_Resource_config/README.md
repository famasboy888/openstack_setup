# OpenStack Basic Provision

## Create Instance Image
Cirros image: [image](https://drive.google.com/file/d/1_2QpOGFHIb_rEivhUSJwBmeRxPC6RG01/view?usp=drive_link)

Debian 12: [image](https://drive.google.com/file/d/1dsbccaAoTJTFIGeIt6sS4J7BZI2S9roA/view?usp=drive_link)

```bash
openstack image create --disk-format qcow2 --container-format bare \
  --public --file /var/tmp/jammy-server-cloudimg-amd64.img ubuntu
```

## Create Security group
- Add SSH port
- Add ICMP port for ping
- Add HTTP port (optional)
Ex:
```bash
openstack security group rule create <security-group-id> --protocol tcp --dst-port 22 --remote-ip 0.0.0.0/0
```

## Create Key Pair
Download and keep key pair safe

## Create Instance Flavor
```bash
openstack flavor create --public kube.mini --id auto \
    --ram 4096 --disk 5 --vcpus 4 --rxtx-factor 1
```

## Networking Stuffs:
```bash
sudo bash
sudo dhclient -r && sudo dhclient -v
sudo ip addr add 192.168.2.225/27 brd + dev br-ex
echo 1 > /proc/sys/net/ipv4/ip_forward
echo 1 > /proc/sys/net/ipv4/conf/ens33/proxy_arp
iptables -t nat -A POSTROUTING -o ens33 -j MASQUERADE
iptables-save
```
and
```bash
sudo nano /etc/sysctl.conf
```
Append following:

**net.ipv4.ip_forward=1**

**net.ipv4.conf.ens33.proxy_arp = 1**

#### For creating persistent IP Addr (Ubuntu)
```bash
sudo nano /etc/netplan/my_ip.yaml
```

```bash
network:
 version: 2
 ethernets:
   br-ex:
     dhcp4: no
     addresses: [192.168.2.225/27]
     gateway4: 192.168.2.35
     nameservers:
         addresses: [8.8.8.8,1.1.1.1]
```

```bash
sudo netplan try
```

#### For creating persistent IP Addr Add (Debian)

Create your script. For example, **/usr/local/bin/your_script.sh**. Ensure the script is executable with the command **chmod +x /usr/local/bin/your_script.sh**.
```bash
#!/bin/bash

sudo ip addr add 192.168.2.225/27 brd + dev br-ex
sudo ip link set br-ex up
```
Use sudo nano /etc/systemd/system/your_service.service to create and open a new service file.
```bash
sudo nano /etc/systemd/system/your_service.service
```
```bash
[Unit]
Description=Add IP to br-ex

[Service]
ExecStartPre=/bin/sleep 30
ExecStart=/usr/local/bin/my_brex.sh

[Install]
WantedBy=multi-user.target
```
Enable and start the service. 

Run **sudo systemctl enable your_service.service** to enable your service, and **sudo systemctl start your_service.service** to start it.

#### For creating persistent Static IP Addr (Debian)
```bash
# The primary network interface
#allow-hotplug ens33
#iface ens33 inet dhcp
auto ens33
iface ens33  inet static
 address 192.168.2.172
 netmask 255.255.255.0
 gateway 192.168.2.1
 dns-nameservers 1.1.1.1 8.8.8.8
```

Then try rebooting.
