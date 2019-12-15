# Linux Distribution

## [Debian系,RedHat系の違い](Debian系RedHat系の違い.md)


## Ubuntu/Debian

### Ubuntu14.04にRictyフォントを追加

参照

http://ktakeda47.blogspot.jp/2014/06/ricty-323ubuntu-1404.html


Ubuntu14.04ではフォントの格納場所が~/.local/share/fontsになっていることに注意！



## CentOS/RHEL

### CentOS7のミニマルインストールにGUIを追加する方法

1. Install CentOS-7 - Minimal (First entry point in list)

2. yum groupinstall "X Window System"

3. yum install gnome-classic-session gnome-terminal nautilus-open-terminal control-center liberation-mono-fonts

4. unlink /etc/systemd/system/default.target

5. ln -sf /lib/systemd/system/graphical.target /etc/systemd/system/default.target

6. reboot


### CentOSのminimalインストールでネットワーク設定

1. /etc/rc.d/init.d/network stop
1. vi /etc/sysconfig/network-scripts/ifcfg-eth0
ONBOOT="yes"

1. /etc/rc.d/init.d/network start

### CentOS7でag（Silver searcher）のインストール

rpm -Uvh http://download.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm

yum install the_silver_searcher


#### CentOS7で日本語入力

`sudo yum -y install ibus-kkc vlgothic-*`


activity->Region & Language


Input SourcesにJapaneseを追加


※input methodの切り替えはデフォルト[super+space]となっている。キーボードショートカットから変更できる

### yum

#### どこのリポジトリからインストールしたか確認する

`yum info`

#### いつインストールしたかなどの詳細情報を調べる

`rpm -qi`


## Arch

### yayでImprove “invalid character ‘<’ looking for beginning of value” errorsが出る

サーバーがメンテナンス中の可能性が高い。少し待てば接続できるようになるかと。


### pacman,yaourt

https://wiki.archlinux.jp/index.php/Pacman_%E6%AF%94%E8%BC%83%E8%A1%A8


#### yaourtでインストール済みのAURパッケージを調べる

`yaourt -Qm`

#### ファイル名からパッケージを探す

`pkgfile makepkg`

#### ファイルを保有するパッケージを調べる

`-Qo <file>`

#### インストール前のパッケージに含まれているファイル一覧を取得する

`pacman -Fl <package>` 実行前に`pacman -Fy`が必要
`pkgfile -l <package>` 実行前に `pkgfile -u`が必要


#### インストールした日時順に表示する

`expac --timefmt=%s '%l\t%n' | sort -n`


## Alpine

### apkでパッケージを探す

https://pkgs.alpinelinux.org/packages


### FAQ

#### pacman -Flでパッケージが見つからない

sudo pacman -Fl lib32-glibc
警告: database file for 'core' does not exist
警告: database file for 'extra' does not exist
警告: database file for 'community' does not exist
警告: database file for 'archlinuxfr' does not exist
error: package 'lib32-glibc' was not found

`sudo pacman -Fy`でデータベースを取得する


