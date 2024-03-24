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
