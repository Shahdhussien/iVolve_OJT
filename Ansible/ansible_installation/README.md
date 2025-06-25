# 🛠️ Lab 4: Ansible Installation

## 🎯 Objectives:

-✅ Install and configure Ansible Automation Platform on the control node.
-📋 Create an inventory file of managed nodes.
-🔑 Generate a new SSH key pair on the control node.
-🚀 Transfer the public SSH key to managed nodes using ssh-copy-id.
-🧪 Run an ad-hoc Ansible command (check disk space) to verify connectivity and functionality.

## Step 1: Install Ansible on Control Node 💻

```
sudo dnf install epel-release -y
sudo dnf install ansible -y
```

## Step 2: Create Inventory File 📂
Edit or create your inventory file /etc/ansible/hosts:

```
[webservers]
managednode   ansible_user=yasmin
```

## Step 3: Generate SSH Key Pair 🔐
Create an SSH key if you don’t have one yet:

```
ssh-keygen
```

## Step 4: Copy SSH Public Key to Managed Nodes ✉️

```
ssh-copy-id yasmin@managednode 
```

## Step 6: Check Disk Space on Managed Nodes 💾

```
ansible webservers -m shell -a "df -h"
```
Output:

```
managednode | CHANGED | rc=0 >>
Filesystem               Size  Used Avail Use% Mounted on
/dev/mapper/cs_nti-root   17G  4.2G   13G  26% /
devtmpfs                 4.0M     0  4.0M   0% /dev
tmpfs                    848M  100K  848M   1% /dev/shm
efivarfs                 256K   31K  226K  13% /sys/firmware/efi/efivars
tmpfs                    340M  7.0M  333M   3% /run
tmpfs                    1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
/dev/nvme0n1p2           960M  316M  645M  33% /boot
/dev/nvme0n1p1           599M   13M  587M   3% /boot/efi
tmpfs                    170M  160K  170M   1% /run/user/1000
/dev/sr0                 7.1G  7.1G     0 100% /run/media/yasmin/CentOS-Stream-10-BaseOS-aarch64
```