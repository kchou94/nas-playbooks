# 系统安装后初始化操作

```bash
su

mv /etc/apt/sources.list /etc/apt/sources.list.bk

cat << EOF >> /etc/apt/sources.list
#------------------------------------------------------------------------------#
#                   OFFICIAL DEBIAN REPOS
#------------------------------------------------------------------------------#

###### Debian Main Repos
deb http://ftp.cn.debian.org/debian/ testing main contrib non-free
deb-src http://ftp.cn.debian.org/debian/ testing main contrib non-free

deb http://ftp.cn.debian.org/debian/ testing-updates main contrib non-free
deb-src http://ftp.cn.debian.org/debian/ testing-updates main contrib non-free

deb http://security.debian.org/ testing-security main
deb-src http://security.debian.org/ testing-security main
EOF

apt update && apt upgrade -y && apt install neovim sudo tmux curl zfsutils-linux -y

gpasswd -a kchou sudo

ls -l /dev/disk/by-id

/sbin/modprobe zfs

sudo zpool create -o ashift=12 tank raidz \
scsi-0QEMU_QEMU_HARDDISK_drive-scsi1 \
scsi-0QEMU_QEMU_HARDDISK_drive-scsi2 \
scsi-0QEMU_QEMU_HARDDISK_drive-scsi3 \
scsi-0QEMU_QEMU_HARDDISK_drive-scsi4 \
scsi-0QEMU_QEMU_HARDDISK_drive-scsi5

sudo zpool set autoexpand=on tank
sudo zfs set atime=off tank
sudo zfs set compression=lz4 tank

sudo zfs create tank/downloads
sudo zfs create tank/docker

```

