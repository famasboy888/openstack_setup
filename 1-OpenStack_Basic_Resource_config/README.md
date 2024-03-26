# OpenStack Basic Provision

## Create Instance Image
Ubuntu image: [image](https://cloud-images.ubuntu.com/jammy/current/jammy-server-cloudimg-amd64.img)

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
Append following
**net.ipv4.ip_forward=1**
**net.ipv4.conf.ens33.proxy_arp = 1**
