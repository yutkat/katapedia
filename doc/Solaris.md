# Solaris

## Tips

### Solaris9でディスクイメージをマウントするには

`loadm -a imgfile`を実行したあとできた/dev/lofiをマウントする必要がある


### Solaris9でディスクI/Oを調べるためには

`iostat -xnC 1`コマンドを用いるとよい


### ファイルシステムの種類は

ufs


Linuxでマウントするには

`mount -t ufs -r -o ufstype=sun /dev/rdsk/c0t0d0s0 /mnt`


### ディスク全体をバックアップするには

Solarisでは/dev/rdsk/c0t0d0s2がディスク全体にあたる

`dd /dev/rdsk/c0t0d0s2 /tmp/disk.img`



