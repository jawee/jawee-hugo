---
title: "Setup Vm From Template"
date: 2020-12-18T18:38:13+01:00
draft: false
---

# Setup a new VM from a template

## Increase disk
from https://www.dlford.io/docker-basics-how-to-home-lab-part-8/

Resize Disk in Proxmox

`sudo parted /dev/sda`

Type `p` then `fix`. Then `q`to exit.

`sudo fdisk /dev/sda`
Type `p`, then `d` and delete partition 3. Then press `n` and create a new partition, press enter three times. 
You do not want to remove the LVM2_member signature. 
End with `wq` to write the changes to the disk and quit fdisk.

Then run `sudo pvresize /dev/sda3` followed by `sudo lvresize -l +100%FREE /dev/ubuntu-vg/ubuntu-lv`

`sudo resize2fs /dev/ubuntu-vg/ubuntu-lv` to extend the filesystem to the new size.

## Script to run
`sudo bash -c "bash <(wget -qO- https://raw.githubusercontent.com/dlford/ubuntu-vm-boilerplate/master/run.sh)"`

Script can be seen in full here
```bash
#!/bin/bash

source /etc/lsb-release

if [[ "$DISTRIB_ID" -ne "Ubuntu" ]]; then
  echo "No action taken..."
  echo "Are you sure this is an Ubuntu system?"
  exit 1
fi

if [[ $EUID -ne 0 ]]; then
  echo "No action taken..."
  echo "This script must be run as root"
  exit 1
fi

read -p "Enter desired hostname: " newHostname

echo "Generating new Machine ID"
rm -f /var/lib/dbus/machine-id
rm -f /etc/machine-id
dbus-uuidgen --ensure=/etc/machine-id
ln -s /etc/machine-id /var/lib/dbus/

echo "Generating SSH server keys"
ssh-keygen -f /etc/ssh/ssh_host_rsa_key -N '' -t rsa -y
ssh-keygen -f /etc/ssh/ssh_host_dsa_key -N '' -t dsa -y
ssh-keygen -f /etc/ssh/ssh_host_ecdsa_key -N '' -t ecdsa -b 521 -y

echo "Setting Hostname"
sed -i "s/$HOSTNAME/$newHostname/g" /etc/hosts
sed -i "s/$HOSTNAME/$newHostname/g" /etc/hostname
hostnamectl set-hostname $newHostname

echo "Done!"

read -p "Would you like to reboot the system now? [Y/N]: " confirm &&
  [[ $confirm == [yY] || $confirm == [yY][eE][sS] ]] || exit 1

reboot
exit 0
```
