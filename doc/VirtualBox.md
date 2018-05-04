# VirtualBox


## Tips

### コマンドラインからの起動

`VBoxManage list vms`

で起動できるVM一覧を表示

起動させる

`VBoxManage startvm YOUR_VM_NAME --type headless`

動いているかどうか確認

`VBoxManage list runningvms`

停止させる

`VBoxManage controlvm YOUR_VM_NAME [ poweroff  | savestate | reset | pause | resume ]`



### NATネットワークとNATの違い

http://changineer.info/server/virtualization/virtualbox.html#_8211-4


### 容量変更

**vdi形式**


1. "C:Program Files\Oracle\VirtualBox\VBoxManage.exe" modifyhd 拡張したい仮想マシンの仮想ディスクのパス --resize 拡張後のディスク容量(MB)
1. OS起動後、gparted等で変更する

**vmdk形式**


1. "C:Program Files\Oracle\VirtualBox\VBoxManage.exe"  clonehd "box-disk1.vmdk" "cloned.vdi" --format vdi
1. "C:Program Files\Oracle\VirtualBox\VBoxManage.exe"  modifyhd "cloned.vdi" --resize 16384    # 単位はMB
1. "C:Program Files\Oracle\VirtualBox\VBoxManage.exe"  clonehd "cloned.vdi" "resized.vmdk" --format vmdk

### VirtualBox Guest Additionsのインストールでエラーとなる

http://punio.org/blog/201408041942VVAW.html

