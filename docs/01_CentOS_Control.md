# 仮想マシンの作成・起動

# CentOS のインストール

# ネットワーク設定

# VMWare Tools のインストール

## 必要ライブラリのインストール

yum install -y gcc perl wget git unzip patch
rpm -ivh ftp://ftp.riken.jp/Linux/cern/centos/7/updates/x86_64/Packages/kernel-devel-3.10.0-862.el7.x86_64.rpm
ls

## VMWare Tools のインストール
mkdir -pv /mnt/cdrom
mount /dev/cdrom /mnt/cdrom
cd /tmp
tar zxpf /mnt/cdrom/VMwareTools-x.x.x-yyyy.tar.gz
umount /dev/cdrom 
cd /tmp
git clone https://github.com/rasa/vmware-tools-patches.git
cd vmware-tools-patches
./download-tools.sh
./untar-and-patch.sh
./compile.sh

# 共有フォルダの設定

## samba設定
yum -y install samba samba-common
mkdir -pv /home/share
chown nobody:nobody /home/share
chmod -R 0777 /home/share
ls -ld /home/share
vi /etc/samba/smb.conf

[global]
        unix charset = UTF-8
        dos charset = CP932
        
        hosts allow = 127.0.0.1 192.168.182.0/24
        hosts deny = all

        guest account = nobody
        map to guest = Bad User

[share]
        comment = Home Directories
        path = /home/share
        writable = yes
        guest ok = yes

systemctl start smb
systemctl start nmb
systemctl enable smb
systemctl enable nmb

systemctl stop firewalld.service
--firewall-cmd --permanent --zone=public --add-service=samba
--firewall-cmd --reload

-- chcon -t samba_share_t /home/share
-- sudo setsebool -P samba_enable_home_dirs on
-- sudo setsebool -P samba_export_all_rw on

echo OK > /home/share/test.txt
chmod 777 /home/share/test.txt

## NFS 基本設定
yum -y install nfs-utils
yum -y install rpcbind

vi /etc/exports
/home/share 192.168.182.0/24(rw)

vi /etc/hosts.allow
portmap : 192.168.182.0/24 : allow

-- vi /etc/idmapd.conf

systemctl start rpcbind
systemctl start nfs-server
systemctl start nfs-lock
systemctl start nfs-idmap
systemctl enable nfs-server

systemctl stop firewalld.service
--systemctl start firewalld.service
--firewall-cmd --state
--firewall-cmd --list-all
--firewall-cmd --permanent --zone=public --add-service=nfs
--firewall-cmd --reload


## NASクライアント設定

### CentOS
less /proc/filesystems | grep nfs

firewall-cmd --permanent --zone=public --add-service=nfs
firewall-cmd --reload

showmount -a 192.168.182.122

mkdir -pv /mnt/share
mount -t nfs 192.168.182.122:/home/share /mnt/share

### Windows
エクスプローラで
\\192.168.182.122\share
にアクセス

### ESXi