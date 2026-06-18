# Install Server
## 1. Bagi Partisi
```bash
cfdisk /dev/nvme0n1
```
```bash
nvme0n1p6 = 3G EFI FILE SYSTEM
```
## 2. Mengecek Partisi
```bash
lsblk
```
## 3. Menghubungkan ke Wifi
```bash
iwctl
```
## 4. Konfigurasi LUKS
```bash
cryptsetup luksFormat /dev/nvme0n1p7
```
```bash
cryptsetup luksOpen /dev/nvme0n1p7 proc
```
## 5. Konfigurasi LVM
Membuat Physical Volume
``bash
pvcreate /dev/mapper/proc
```
Membuat Volume Grup
```bash
vgcreate saw /dev/mapper/proc
```
