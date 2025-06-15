# Extend-Linux-Root-Disk-Using-LVM
Step-by-step guide to expand the root (/) partition in Linux using LVM and a second disk.

# 🧱 Extend Linux Root Partition Using LVM (AlmaLinux + VMware)

## 🔧 Problem

My root partition (`/`) was only 70 GB, while a second 2 TB SSD was connected to the same server 
(running on VMware Workstation Pro on Windows). I needed to **extend the root volume using space from the second SSD**.

## 💻 Environment
- OS: AlmaLinux (RHEL-based)
- Virtualized on: VMware Workstation Pro
- Filesystem: XFS
- Disk Management: LVM (Logical Volume Management)

---

## 📷 Initial Disk Layout

- Root (`/`) = 70 GB → **Too small**
- `/mnt/old_home` = 527 GB
- `/homeb` = 1.8 TB → **Mostly unused space available**

---

## ✅ Goal

Increase the root filesystem (`/`) from **70 GB** → **200+ GB** using the available 2 TB SSD space.

---

## 🗺️ Solution Diagram

> 📌 See the visual workflow below:

![LVM Root Expansion Diagram](./lvm-extension-diagram.png)

---

## 🔨 Step-by-Step Commands

```bash
# 1. Create a new partition on the 2 TB disk
sudo fdisk /dev/nvme0n2

# 2. Create a Physical Volume (PV)
sudo pvcreate /dev/nvme0n2p2

# 3. Extend the Volume Group (VG)
sudo vgextend almalinux /dev/nvme0n2p2

# 4. Extend the Logical Volume (LV)
sudo lvextend -L +150G /dev/almalinux/root

# Or use all available space:
# sudo lvextend -l +100%FREE /dev/almalinux/root

# 5. Resize the filesystem (XFS)
sudo xfs_growfs /
