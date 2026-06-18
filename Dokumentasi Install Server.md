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
```bash
pvcreate /dev/mapper/proc
```
Membuat Volume Grup
```bash
vgcreate saw /dev/mapper/proc
```
Membuat Logical Volume
```bash
lvcreate -L 8G saw -n root
lvcreate -L 5G saw -n vars
lvcreate -L 1G saw -n vlog
lvcreate -L 1G saw -n vaud
lvcreate -L 1.5G saw -n vtmp
lvcreate -l50%FREE saw -n home
lvcreate -l100%FREE saw -n podman
```
## 6. Membuat Filesystem
```bash
mkfs.ext4 /dev/proc/root
mkfs.vfat -F32 /dev/nvme0n1p6
mkfs.ext4 /dev/proc/vars
mkfs.ext4 /dev/proc/vlog
mkfs.ext4 /dev/proc/vaud
mkfs.ext4 /dev/proc/vtmp
mkfs.ext4 /dev/proc/home
mkfs.ext4 /dev/proc/podman
```
## 7. Mount Filesystem
```bash
mount /dev/saw/root
mount --mkdir -o uid=0,gid=0,fmask=0077,dmask=0077 /dev/nvme0n1p6 /mnt/boot
mount --mkdir -o rw,nodev,nosuid,relatime /dev/saw/vars /mnt/var
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/saw/vlog /mnt/var/log
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/saw/vaud /mnt/var/log/audit
mount --mkdir -o rw,nodev,nosuid,noexec,relatime /dev/saw/vtmp /mnt/var/tmp
mount --mkdir -o rw,nodev,relatime /dev/saw/home /mnt/home
mount --mkdir -o rw,nodev,nosuid,relatime /dev/saw/podman /mnt/var/lib/containers
```
## 8. Install Package
```bash
pacstrap /mnt base intel-ucode linux-lts linux-lts-headers Linux-firmware mkinitcpio lvm2 git neovim firewalld openssh sudo pacman wget curl grep iwd podman
```
## 9. Genfstab
```bash
genfstab -U /mnt > /mnt/etc/fstab
```
## 10. Mounting RAM
```bash
echo "tmpfs /tmp tmpfs deafults,rw,nodev,nosuid,noexec,relatime,size=512M 0 0" >> /mnt/etc/fstab
cp /etc/systemd/network/* /mnt/etc/systemd/network
arch-chroot /mnt
```
kalau berhasil tulisan root yang berwarna merah akan berubah menjadi putih
## 11. Konfigurasi System
Hostname
```bash
echo (nama computer) > /etc/hostname
```
Timezone
```bash
ln -fs /usr/share/zoneinfo/Asia/Jakarta /etc/localtime
hwclock --systohc
```
```bash
nvim /etc/locale.gen
```
hapus # en_US
Locale
```bash
locale-gen
```
```bash
locale > /etc/locale.conf
```
```bash
nvim /etc/locale.conf
```
## 12. Konfigurasi User
```bash
useradd -m (nama user)
```
```bash
passwd (nama user)
```
```bash
echo "(nama user) ALL=(ALL:ALL) ALL" > /etc/sudoers.d/(nama user)
```
Uji coba
```bash
su (nama user)
```
Jika berhasil nama root akan berubah menjadi nama user
```bash
sudo su
```
Masukkan Password
## 13. Setup Kernel Parameter
``bash
mkdir /etc/cmdline.d
```
## 14. Cek




