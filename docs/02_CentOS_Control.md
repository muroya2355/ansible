

# VMWare Tools のインストール

## 必要ライブラリのインストール

```
yum install -y gcc perl wget git unzip patch
rpm -ivh ftp://ftp.riken.jp/Linux/cern/centos/7/updates/x86_64/Packages/kernel-devel-$(uname -r).el7.x86_64.rpm
```

## VMWare Tools のインストール

https://docs.vmware.com/jp/VMware-Workstation-Player-for-Windows/15.0/com.vmware.player.win.using.doc/GUID-08BB9465-D40A-4E16-9E15-8C016CC8166F.html

```
[Player] > [管理] > [VMware Tools のインストール] 
mkdir -pv /mnt/cdrom
mount /dev/cdrom /mnt/cdrom
cd /tmp
tar zxpf /mnt/cdrom/VMwareTools-x.x.x-yyyy.tar.gz
umount /dev/cdrom 
cd vmware-tools-distrib
sudo ./vmware-install.pl
```

```
cd /tmp
git clone https://github.com/rasa/vmware-tools-patches.git
cd vmware-tools-patches
./download-tools.sh
./untar-and-patch.sh
./compile.sh
```