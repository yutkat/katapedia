# xrdp

* xrdp のPATHがおかしいとき
startwm.sh を/etc/X11/xinit/Xsessionにシンボリックリンクさせる

* xrdp 日本語・英語切り替えの要
km-e0210411.ini

* lxdeにする
`sudo apt-get install lxde xvfb`

~~~
sudo vi /etc/xrdp/startwm.sh
lxsession
~~~

## i3を使う

startwm.sh

```
export LANG="ja_JP.UTF-8"
xrdb -remove && xrdb -merge $HOME/.Xresources
exec i3
```

## セッションの意味

・VNC — VNCサーバーをバックエンドとして使う
・X11RDP — 自前で構築した X11 サーバーをバックエンドとして使う
・xorgxrdp — ドライバ経由でシステムの X11 サーバーを使う

## archlinux

### インストール

`yaourt -S xrdp-git`
`yaourt -S xorgxrdp-git`

`sudo vi /etc/xrdp/startwm.sh`
デスクトップセッションを
eg. `exec mate-session`
に変更

### 接続

Xvncじゃないとなぜかエラーになる
-> xorgxrdpがないからだった


```
xrdp_wm_log_msg: connection problem, giving up
X server for display 10 startup timeout
another Xserver might already be active on display 10 - see log
```

Xorgでログインする際はログイン済みでないとだめな模様
[20171218-11:54:25] [DEBUG] Closed socket 19 (AF_UNIX) が出る

## 強制的に英語キーボード

最近のxrdp（たぶん0.9くらいから）はクライアントの言語環境でキーボードタイプを自動識別している模様。
日本人なのに英字キーボードを使っている人には不自由なので強制的に英字キーボードにする

xrdp_keyboard.ini
のdefault_rdp_layouts, default_layouts_mapのus以外をコメントアウト


## CentOS7でxrdp

~~~
rpm -Uvh https://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-2.noarch.rpm
rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-1.el7.nux.noarch.rpm
yum -y install xrdp tigervnc-server
systemctl start xrdp.service
systemctl enable xrdp.service
firewall-cmd --permanent --zone=public --add-port=3389/tcp
firewall-cmd --reload
~~~

~~~
yum --enablerepo=epel -y groups install "MATE Desktop"
~~~

~~~
# vi /etc/xrdp/startwm.sh

SESSIONS="mate-session gnome-session blackbox fluxbox startxfce4 startkde xterm"

...

export LANG=ja_JP.UTF-8

if [ "$LANG" = "ja_JP.UTF-8" ]; thenexport XMODIFIERS=@im=SCIM
export GTK_IM_MODULE=scim
scim -d &
fi
~~~

~~~
sudo echo "mate-session" > ~/.Xclients
sudo chmod +x ~/.Xclients
~~~

## TroubleShooting

### thinclient_drivesにアクセスできませんのエラーがでる

`sudo umount ~/thinclient_drives`

https://github.com/neutrinolabs/xrdp/issues/720



