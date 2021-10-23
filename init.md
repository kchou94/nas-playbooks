# 系统安装后初始化操作

```bash
su

mv /etc/apt/sources.list /etc/apt/sources.list.bk

cat << EOF >> /etc/apt/sources.list
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-free

deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-free

deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bullseye-security main contrib non-free
# deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security bullseye-security main contrib non-free
EOF

apt update && apt upgrade -y && apt install sudo curl zfsutils-linux -y

apt install firmware-realtek -y

gpasswd -a kchou sudo

sync && sudo reboot

ls -l /dev/disk/by-id

/sbin/modprobe zfs

sudo zpool create -o ashift=12 tank raidz \
scsi-0QEMU_QEMU_HARDDISK_drive-scsi1 \
scsi-0QEMU_QEMU_HARDDISK_drive-scsi2 \
scsi-0QEMU_QEMU_HARDDISK_drive-scsi3 \
scsi-0QEMU_QEMU_HARDDISK_drive-scsi4 \
scsi-0QEMU_QEMU_HARDDISK_drive-scsi5

sudo zpool create -o ashift=12 tank raidz ata-WDC_WD10EFRX-68FYTN0_WD-WCC4J0SEF254 ata-WDC_WD10EFRX-68FYTN0_WD-WCC4J7VF4NUS ata-WDC_WD10EFRX-68FYTN0_WD-WCC4J0TDU34X ata-WDC_WD10EFRX-68FYTN0_WD-WCC4J0SEFN9A ata-WDC_WD10EFRX-68FYTN0_WD-WCC4J0TDUPT3

sudo zpool set autoexpand=on tank
sudo zfs set atime=off tank
sudo zfs set compression=lz4 tank

sudo zfs create tank/downloads
sudo zfs create tank/docker
sudo zfs create tank/syncthing

sudo zfs create tank/downloads/media
sudo zfs set recordsize=1M tank/downloads/media
sudo zfs set compression=off tank/downloads/media
sudo zfs set exec=off tank/downloads/media

docker network create traefik

```

```
sops --encrypt --in-place inventory/host_vars/host.sops.yaml
sops inventory/host_vars/host.sops.yaml
```
