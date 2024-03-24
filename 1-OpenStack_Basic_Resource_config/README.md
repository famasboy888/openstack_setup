# OpenStack Basic Provision

## Create Instance Image
```bash
openstack image create --disk-format qcow2 --container-format bare \
  --public --file /var/tmp/jammy-server-cloudimg-amd64.img ubuntu
```
