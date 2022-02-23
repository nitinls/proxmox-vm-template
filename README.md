# proxmox-vm-template

https://www.rajaseelan.com/posts/proxmox-create-vm-template-ubuntu/


Create a Proxmox VM Template for Ubuntu 20.04
Sun, Oct 3, 2021
Some necessary details for creating a Virtual Machine Template for Ubuntu 20.04 in Proxmox 7.0.
I wanted a quick machine I could spin up to start installing various Kubernetes Flavours.
Here is what I setup:

Perform a custom install.
/boot - 1 GiB
/     - Everything else
Upon successfull install, remove swap.
# /etc/fstab
# /swap.img  # comment out line

# disable swap
sudo swapoff -a 
sudo rm -vf /swap.img
Install qemu agent
sudo apt install qemu-guest-agent
sudo systemctl enable --now qemu-guest-agent

Proxmox -> Options -> Qemu Guest Agent -> Enabled
Update everything
sudo apt update && sudo apt upgrade -y
Remove apt caches
sudo apt clean
sudo apt autoremove
Remove machine-id
truncate -s 0 /etc/machine-id

# check if machine-id points to that file
sudo ls -l /var/lib/dbus/machine-id
/var/lib/dbus/machine-id -> /etc/machine-id
Clear SSH Host Keys
cd /etc/ssh
rm -vf ssh_host_*
Shutdown -> Hardware
-> remove CDROM
-> Add Cloud-init drive
-> Add Serial Port (Cloud-init Requirement)
Convert Machine to template.
