# Linux

## [zsh](zsh.md)

## [Linux Command](Linux_Command.md)

## [Linux Distribution](Linux_Distribution.md)

## [omniauthによるアカウント統合](omniauthによるアカウント統合.md)

## Systemd

https://speakerdeck.com/moriwaka/systemd-intro

## デスクトップマネージャ

### sway

#### waylandかx11か調べる

`echo $XDG_SESSION_TYPE`

#### タッチパッドとかマウスとかの速度を変更する

sway inputをconfigに追加する
https://www.mankier.com/5/sway-input#

名前はswaymsg -t get_inputsで取得できる

```
input "0:1336:USB_OPTICAL_MOUSE" {        
  pointer_accel 0.7
}
```


### i3

#### キーマップ

親コンテナへ移動する: mod+a

#### TroubleShooting

#### mod+v,mod+hで作った親コンテナが削除できない

mod+shift+up等で移動させれば自動的に削除される

https://unix.stackexchange.com/questions/173754/how-to-move-a-window-up-to-the-level-of-its-parent-window-in-i3wm

##### i3-save-treeでエラーがでる

```
Can't locate AnyEvent/I3.pm in @INC (you may need to install the AnyEvent::I3 module) (@INC contains: /usr/lib/perl5/5.26/site_perl /usr/share/perl5/site_perl /usr/lib/perl5/5.26/vendor_perl /usr/share/perl5/vendor_perl /usr/lib/perl5/5.26/core_perl /usr/share/perl5/core_perl) at /usr/bin/i3-save-tree line 19.
BEGIN failed--compilation aborted at /usr/bin/i3-save-tree line 19.
```

`sudo pacman -S perl-anyevent-i3`
で解決した


## Port

* redmine
80 (VMWare Portforwarding 8080->80)


* jenkins
8080


* gerrit
8081



## GNOME

* gnome3パネルに追加
win alt right click


## ツール

### urxvt

#### コピーペースト

CTRL-ALT-C と CTRL-ALT-V でコピーアンドペーストができます。

https://wiki.archlinux.jp/index.php/Rxvt-unicode



### Kandan

"参考ページ":http://ganeza.hatenablog.com/entry/2014/03/17/Kandan%E3%81%A7%E3%83%81%E3%83%A3%E3%83%83%E3%83%88%E3%82%B5%E3%83%BC%E3%83%90%E7%AB%8B%E3%81%A6%E3%81%A6%E3%81%BF%E3%81%9F


Gemfile

`gem 'therubyracer'`



bundle install --without development test


#### アバターの設定変更

vi config/kandan_settings.yml

external_avatar_max_size: 1024000

external_avatar_formats: [".jpeg", ".jpg", ".png", ".gif", ""]


#### プリコンパイル設定

vi config/environments/production.rb

config.serve_static_assets = true


`sudo bundle exec rake assets:precompile RAILS_ENV=production`


#### メニューダイアログが開かない

RAILS_ENV=productionを付けてprecompile、thinの実行を行う。


### WordPress

簡単だった


http://kawatama.net/web/wordpress/548


### porg

pacoの後継PJ


http://porg.sourceforge.net/


gtkmm-3.0がインストールされていないので


~~~
./configure --disable-grop
~~~

か


~~~
sudo aptitude install libgtkmm-3.0-dev
./configure
~~~

を実行してmakeする


~~~
make
sudo make install
sudo make logme
~~~

@porg -a@でインストール確認


### [xrdp](xrdp.md)

### リモートデスクトップ

今まではrdesktopを使っていたが

`ERROR: CredSSP: Initialize failed, do you have correct kerberos tgt initialized`
https://github.com/rdesktop/rdesktop/issues/28
というエラーで使えなかったためfreerdpに乗り換え

インストール
`sudo pacman -S freerdp`

`xfreerdp /u:<username>  /f +fonts +compression  +smart-sizing -clipboard  /v:<ip> -grab-keyboard`


#### 認証エラーになるとき

会社アカウントとかで認証エラーになるとき

add option
`/gt:http /sec:tls`

https://github.com/FreeRDP/FreeRDP/issues/4781#issuecomment-474383962

### mysql

* mysqlは1000byte以上のデータを登録できない

### jenkins

* redmineと共存させるため以下の設定を追加
* httpd.conf
ProxyPass /jenkins http://127.0.0.1:8080/jenkins

ProxyPassReverse /jenkins http://127.0.0.1:8080/jenkins

RedirectMatch permanent ^/jenkins$ /jenkins/

* /etc/default/jenkins
JENKINS_ARGSの末尾に --prefix=/jenkins を追加


### gerrit

* gerritインストール
http://www.kyamada.com/50

に従う

※データベースはデフォルト(h2)のものとした。


### nginx

* nginxのファイル上限を設定する
/etc/nginx/sites-available/default


許可したいlocationに

client_max_body_size 10M

を追加


### [GitLab](GitLab.md)

## ウィンドウマネージャ

i3, awesome, xmonad



---


### fcitx

#### i3wmでfcitx自動起動

~/.xprofile

に

fcitx-autostart

を書き込む



#### キャンセル後IMEが無効になる

これがいい

http://qiita.com/iberianpig@github/items/f43afb9d5ec0e2509695




あまりよくない

↓こっちは

fcitxでのIME切り替えを行わないようにしてすべてmozcでやるようにする


1. fcitxでmozc以外のキーボードを削除

2. mozcで入力前にCtrl+Space IMEを無効化を設定


ただし起動時に日本語入力がデフォルトとなってしまう


## ビルド

### configure

わかりやすいブログ

https://engineering.otobank.co.jp/2015/02/19/autotools/



### [Makefile](Makefile.md)

## sshでノンパスワード設定

ssh-keygen -t rsa

ssh-copyid -i .ssh/id_rsa.pub xxx@xxx


※ ノーパスワードにならない場合高確率でディレクトリ権限がおかしい

* Home Directory: drwxr-x--- (750)
* .ssh Directory: drwx------ (700)
* authorized_keys file: ~~rw------~~ (600)

## ssh時のメッセージ変更

/etc/update-motd.dに（番号）-機能 のファイル（シェル）を作成する。

---

## フォント

### fontconfig

https://manpages.ubuntu.com/manpages/disco/man5/fonts-conf.5.html

/etc/fonts/conf.d

---

## 外部デバイス

### udevでのキーマップ変更

- https://yulistic.gitlab.io/2017/12/linux-keymapping-with-udev-hwdb/
- https://wiki.archlinux.jp/index.php/%E3%82%B9%E3%82%AD%E3%83%A3%E3%83%B3%E3%82%B3%E3%83%BC%E3%83%89%E3%82%92%E3%82%AD%E3%83%BC%E3%82%B3%E3%83%BC%E3%83%89%E3%81%AB%E3%83%9E%E3%83%83%E3%83%97
- https://github.com/FreeRDP/FreeRDP/issues/505#issuecomment-148165369

keycode
/usr/include/linux/input-event-codes.h

### udev関連でよく使うコマンド

設定ファイルがある： `cd /usr/lib/udev/`
デバイス一覧的なやつ:`sudo evtest`
現在値：`sudo udevadm info xxx`
設定反映：`sudo systemd-hwdb update && sudo udevadm trigger`


### ディレクトリ接続時に自動接続

`/etc/udev/rules.d/95-monitor-hotplug.rules`

`SUBSYSTEM=="drm", ACTION=="change", RUN+="/usr/local/bin/detect_displays.sh"`

---

## X（GUI）をリモートで使う

### SSH X11Forward(遅い)

/etc/ssh/sshd_configに
`X11UseLocalhost no`
を追加

`$ ssh -X user@remote-pc-ip`

`$ echo $DISPLAY`
`xx.xx.xx.xx:10.0`とかが入っているはず

`$ xev`

### xhost(セキュリティ的に弱い)

* ローカル側（画面表示側）
xhost +remote-pc-ip
(IP（ホスト名）でアクセスを制限する xhost +だと全アクセスになるが危険。デバッグ時に使うのはOK)

* リモート側（画面を表示したい側）
export DISPLAY="xx.xx.xx.xx:0"

### xauth

* ローカル側（画面表示側）
xauth list
`192.168.1.10  MIT-MAGIC-COOKIE-1  xxxxxxxxxxxxxxxxxxxxxxxxx`をコピー

* リモート側（画面を表示したい側）

xauth add 192.168.1.10  MIT-MAGIC-COOKIE-1  xxxxxxxxxxxxxxxxxxxxxxxxx
export DISPLAY="xx.xx.xx.xx:0"

- http://luozengbin.github.io/blog/2014-06-21-%5B%E3%83%A1%E3%83%A2%5D%E3%83%AA%E3%83%A2%E3%83%BC%E3%83%88x%E3%81%AE%E6%8E%A5%E7%B6%9A%E6%96%B9%E6%B3%95.html
- http://www.ep.sci.hokudai.ac.jp/~inex/y2007/0125/x.html

---


## セキュリティ

### ロック

`/etc/systemd/system/i3lock@username.service`

```
[Unit]
Description=i3lock
Before=sleep.target

[Service]
User=%I
Type=forking
Environment=DISPLAY=:0
ExecStart=/usr/bin/i3lock -c 282C34

[Install]
WantedBy=sleep.target
```

---

## プリンタ

### プリンタ設定

http://localhost:631


---

## タッチパッドとかの入力デバイスの設定

よくsynapticsを使う設定（synclient）が書いてあるが古い模様

xinputを使ったほうがいい。

`xinput list`でデバイス一覧
`xinput list-prop デバイス名`で設定一覧
`xinput set-prop デバイス名 設定名 値`で設定変更

ができる

###  Coordinate Transformation Matrixの設定

https://wiki.archlinux.org/index.php/Talk:Calibrating_Touchscreen
https://en.wikipedia.org/wiki/Transformation_matrix#/media/File:2D_affine_transformation_matrix.svg

### タッチパッドの有効エリアをudevで設定する

`sudo touchpad-edge-detector 1936x1057 /dev/input/event15`

https://wayland.freedesktop.org/libinput/doc/latest/absolute-coordinate-ranges.html#absolute-coordinate-ranges-fix

---


## Hack

### xinitrcとxprofileの違い

1. startx

2. ~/.xinitrcを見る

3. DEが立ち上がる

4. WMが立ち上がろうとする

5. ~/.xprofileを見る

6. WMが立ち上がる

7. 見慣れた光景を得る



http://nosada.hatenablog.com/entry/2014/03/11/232330


### 修飾キーとキーマッピングの対応

arrow-rightで試してみる


~~~
right: ^[[C
shift-right:^[[1;2C
alt-right:^[[1;3C
shift-alt-right:^[[1;4C
ctrl-right:^[[1;5C
ctrl-shift-right:^[[1;6C
ctrl-alt-right:^[[1;7C
ctrl-shift-alt-right:^[[1;8C
~~~



### initrd.imgを展開する

~~~
Extract:

mv initrd.img /tmp/initrd.img.xz
cd /tmp
xz –format=lzma initrd.img.xz –decompress
mkdir initrd
cd initrd
cpio -ivdum < ../initrd.img

Archive (after you applied your changes):

cd /tmp/initrd
find . | cpio -o -H newc | xz -9 –format=lzma > ../new-initrd.img
~~~

## Troubleshooting

### なんか起動後ちょっとしてノートパソコンの画面の輝度が上がってカーソルがカクつく現象

jounalctlで見張っておくと処理が遅くなったときに以下が出る

```
[  184.156188] audit: type=1130 audit(1575565427.320:53): pid=1 uid=0 auid=4294967295 ses=4294967295 msg='unit=systemd-backlight@backlight:intel_backlight comm="systemd" exe="/usr/lib/systemd/systemd" hostname=? addr=? terminal=? res=success'
```

これが原因なのは間違いなさそう
https://wiki.archlinux.jp/index.php/%E3%83%90%E3%83%83%E3%82%AF%E3%83%A9%E3%82%A4%E3%83%88#xbacklight

マスクすればよさげだが、そうすると以前の輝度に戻る機能がなくなる模様。


### なぜかシャットダウンできない

poweroff とか haltとかでもシャットダウンできないときに強引にシャットダウンする

```
echo s > /proc/sysrq-trigger
echo b > /proc/sysrq-trigger
```


### 音が思った出力先から出ない

思ったサウンドカードから音がでないときの対処

1. サウンドカードの読み込み順(優先順位)を固定する
`/etc/modprobe.d/sound.conf`を編集
2. `cat /proc/asound/modules`で優先順位が高く（数字が低く）なっていることを確認
3. `aplay -l`と`aplay /usr/share/sounds/alsa/Front_Center.wav -D plughw:0,0`を駆使しながら希望のサウンドカードから音がでることを確認
4. `~/.asoundrc`に
```
pcm.!default "plughw:0,0"
ctl.!default "plughw:0,0"
```
という形で音がなった-Dの引数を入力
5. `aplay /usr/share/sounds/alsa/Front_Center.wav -D default`で音がなるかを確認
6. pulseaudioのデフォルトも変更`pacmd list-sinks | grep -e 'name:' -e 'index:'`で現在のデフォルト確認
7. nameの<>部分 eg. name: <alsa_output.pci-0000_00_1f.3.analog-stereo>を使って`pacmd set-default-sink alsa_output.pci-0000_00_1f.3.analog-stereo`を実

chrome/chromiumだけ変えたいならAudioPack(https://chrome.google.com/webstore/detail/audiopick/gfhcppdamigjkficnjnhmnljljhagaha?hl=en)を使うとよい

https://qiita.com/propella/items/4699eda71cd742cba8d3


### CentOSでメディアのyum update時にエラーになる

`http://192.168.1.10/yum_repositories/CentOS-6.7-x86_64-bin-DVD1/repodata/c11b211333eadda7b2e2d0f7fa8ffbf70a1d32d5182babbb43b90427578e2891-primary.sqlite.bz2: [Errno 14] PYCURL ERROR 22 - "The requested URL returned error: 404 Not Found"`
WindowsでDVDのデータをコピーしたことによってパスが途中で切れているのが原因だった。

正しいファイル名にリネームしたら直る

https://bugzilla.redhat.com/show_bug.cgi?id=715415
https://lists.centos.org/pipermail/centos/2014-January/140229.html
http://wp.laziness.mydns.jp/archives/345



### CentOSでyum update時にエラーになる

```
--> Finished Dependency Resolution
Error: Package: matahari-host-0.4.4-11.el6.x86_64 (@anaconda-CentOS-201112091719.x86_64/6.2)
           Requires: libqpidclient.so.5()(64bit)
           Removing: qpid-cpp-client-0.12-6.el6.x86_64 (@anaconda-CentOS-201112091719.x86_64/6.2)
               libqpidclient.so.5()(64bit)
           Updated By: qpid-cpp-client-0.14-22.el6_3.x86_64 (base)
               Not found
```

matahari を削除してから yum update すればOKです。
```
$ sudo yum remove matahari-*
$ sudo yum update
```
mahatari の開発は終了しておりセキュリティ上の問題があるため CentOS 6.5 から削除されたそうです。

### /etc/sudoers.d以下にファイルを追加したのに反映されない

ファイル名に拡張子が付いていると読み込まれない模様



### whichでaliasが表示される

type -aを使う




### ubuntuにてsshが遅い


**debug1: SSH2_MSG_SERVICE_ACCEPT receivedとかで10秒くらいかかる場合**

ログイン先の /etc/ssh/sshd_config に以下を追記してssh再起動で解決。

UseDNS no



**debug1: Next authentication method: gssapi-with-mic**

~/.ssh/config (もしくは /etc/ssh/ssh_config でもいいと思う）に以下を追記して解決。

GSSAPIAuthentication no



http://tegetegekibaru.blogspot.jp/2013/07/ssh.html



### network-scripts/ifcfg-ethxのGATEWAYを設定したのにデフォルトゲートウェイを使ってしまう


network-scripts/ifcfg-ethxのGATEWAYはデフォルトゲートウェイを設定するものの模様。

route-ethxを使うかipv4.gatewayを使用する必要がある。


http://iwsttty.hatenablog.com/entry/2015/03/01/152541



### rm -rfしてしまった！！！

sudo apt-get install extundelete

extundelete --restore-all --after $(date +%s -d '2014-06-25 05:37:44') /dev/sda1


RECOVERED_FILESというディレクトリに復元される


### sedで改行置換

むずいので、trを使うのがよい


### sedで最短マッチ

sedでは@.+?@や@.*?@が使えないため工夫する必要がある。


sed s/<[^>]*>//g test.html


### sedで書き換えた行だけ表示

-nとpを組み合わせる


`sed -n -e "s/^AAA .*/AAA 1/gp" sample.txt`


### ビープ音を消す

`xset -b`


## Trivia

### xlsfontsとfc-listの違い

xlsfonts:X サーバーの設定依存


Fontconfig（フォントコンフィグ、fontconfig）はライブラリであり、システム全体のフォント設定、

カスタマイズやアプリケーションからのアクセスを提供するように設計されている。



http://d.hatena.ne.jp/jeneshicc/20100408/1270731732



### .dディレクトリ


ディレクトリを明示している

/etc/apt/sources.list.d

/etc/apt/sources.list

みたいに設定ファイル名と同じディレクトリを作るときとかに便利かも



### いろいろなカーネル値を知る方法

getconf

getconf LONG_BIT <- 32bitか64bitかわかる

getconf ARG_MAX <- コマンドラインの最大上限




### ターミナルで使用できる色数を調べる

`tput colors`


### よく使う拡張子

~~~
Linux extension    |Windows Equivalent    |Short description
------------------------------------------------------------
.so, .o            | .dll                 | Object that can be loaded in(Like a DLL)
[none], .elf(rare) | .exe, .com(rare)     | Linux executables
.sh                | .bat                 | Shell script
.exe               | .exe                 | Mono application
.deb               | .msi                 | Installer package(Though .deb is much more powerful with native support for dependencies and repos). Note that .deb is actually a .ar archive with a special control file, a special file order, and a different extension.
.tar.gz, .tar, .gz | .zip                 | Compressed files that can contain a program or any other data, like images, documents, etc
.ko                | .sys                 | Drivers and kernel modules are loaded into the Linux kernel and have more hardware access than other programs.
~~~

#### locateにnfsを含める

/etc/updatedb.confのPRUNEFSからnfsを削除する

sudo /usr/bin/updatedb  --netpaths='/suzaku'とかする。


http://blog.livedoor.jp/vavacco/archives/45287.html



#### 実行ファイルの場合

シェルスクリプトは.shをよく使う。

バイナリの場合は.bin

.exeだとアイコンにWindowsマークがつくためLinuxではやめた方がよいかも。

.runはインストーラに多い


http://www.file-extensions.org/filetype/extension/name/program-executable-files

http://pcsupport.about.com/od/tipstricks/a/execfileext.htm

---

## systemd

### 特殊変数一覧

https://www.freedesktop.org/software/systemd/man/systemd.unit.html#Specifiers


---


## Tips

### AsciiとHex文字列を相互変換する方法

https://stackoverflow.com/a/49903434/5720201

### 関数がどこで定義されているか調べる

`type`

https://unix.stackexchange.com/questions/322817/how-to-find-the-file-where-a-bash-function-is-defined


### 複数のスペース（空白）をcutで処理する

`tr -s ' '`を使う

例：
`df -h | grep -w '/' | tr -s ' ' | cut -d " " -f 5`

### CRLFの改行コードを探す

`find . -type f | xargs grep -lzUP '\r\n'`

LFに変換する

`find . -type f | xargs grep -lzUP '\r\n' | xargs -i nkf -Lu --overwrite {}`


### ターミナルで使えるC-特殊キー

- C-@ = C-Space
- ^^
- ^[ = Esc
- ^]
- ^\
- ^? = BS
- ^_


### デフォルトアプリケーションの設定

確認
`xdg-mime  query default text/html`

設定
`xdg-mime  default chromium.desktop text/html`

ファイル
`$HOME/.config/mimeapps.list`


https://wiki.archlinux.jp/index.php/%E3%83%87%E3%83%95%E3%82%A9%E3%83%AB%E3%83%88%E3%82%A2%E3%83%97%E3%83%AA%E3%82%B1%E3%83%BC%E3%82%B7%E3%83%A7%E3%83%B3

### ターミナルの表示を変更する

`echo -ne "\033]0;SOME TITLE HERE\007"`

### 全体バックアップ

`tar cpfl - / | ssh server-user@file-server "dd of=/archive-dir-path/hogehoge.tar"`


### サニタイズを置換する

@sed 's/&nbsp;/ /g; s/&amp;/\&/g; s/&lt;/\</g; s/&gt;/\>/g; s/&quot;/\"/g; s/#'/\'"'"'/g; s/&ldquo;/\"/g; s/&rdquo;/\"/g;'@
https://stackoverflow.com/questions/5929492/bash-script-to-convert-from-html-entities-to-characters

### Xのアプリをssh越しで起動する

$ export DISPLAY=:0.0

$ nohup gedit &

$ exit


Xサーバが起動していないとできない。


### lsの表示順で大文字小文字を無視しない

大文字小文字を別々にしてアルファベット順に並べる方法


`export LC_ALL=C`

`export LC_COLLATE=C`


https://jaydight.wordpress.com/2012/11/10/gnu-ls/



### プロンプト（PROMPT）の色設定

http://misc.flogisoft.com/bash/tip_colors_and_formatting


装飾はBold,Underline,Invertくらいは使ってもよさそう


### /lib/x86_64-linux-gnu

64bitとか32bitとかCPUとかを区別したライブラリの置き場

http://intothevoid.hatenablog.com/entry/20150329/1427630864



### 16進文字を読めるように変換したい

4e4f4e00

`echo 'x4ex4fx4ex00'`

->

NON



### PCのシステム情報を知りたい

`sudo dmidecode`



### Linuxにおけるデバイス検出とドライバーの自動ロード

1. デバイスのホットプラグ、ないしは起動時のバス・プローブでデバイスが検出され、デバイス・エイリアス文字列を引数としたUEVENTが発生する。
1. udevdなどのシステムデーモンがUEVENTを受信し、エイリアス文字列を引数としてmodprobeコマンド（ないしは互換機能である内蔵のkmod load）を実行する。
1. modprobeは/lib/modules//modules.aliasからエイリアスを検索する。エイリアスが見つかった場合は、対応するモジュール名を引数としてmodprobeを実行する。
1. modprobeは/lib/modules//modules.depからモジュール名を検索する。モジュール名が見つかった場合は依存モジュール一覧を調べ、まだロードされていなかった場合は更に親モジュール名を引数としてmodprobeを実行する。
1. 依存モジュールがロード済みとなった段階で、モジュール名に対応するモジュールファイル名をmodules.depから抽出し、insmodコマンド（ないしは互換機能）を実行して該当ファイルを読み込む。

- insmodをでファイル名を直接指定してドライバーロードできるか？

- modprobeでモジュール名を指定してドライバーロードできるか？

- modprobeでデバイスエイリアスを指定してドライバーロードできるか？

- デバイスは検出されているか？（/sys/busなどを利用）

- デバイス検出のUEVENTは発生しているか？（dmesg、udevadmなどを利用）

- 検出されたデバイスのエイリアスはmodules.aliasに登録されているか？


http://techon.nikkeibp.co.jp/atcl/column/15/293600/033000003/?n_cid=nbptec_tecrs&rt=nocnt


### cpでシンボリックリンクごと

コピーする際にシンボリックリンクの扱いについて


-s コピーのかわりにシンボリックリンクを作る。

-L シンボリックリンクを実態としてコピー

-d シンボリックリンクをコピーする



### 使える特殊キーバインド

キーマップの中で削除とかに割り当てられてないキー

```
^]
^@
^_
^^
```

### limit.confのsoftとhardの違い

ulimit とかで使用するときにでてくるsoftとhardの記述。

soft/hardの違いは、softが一般ユーザが変更できる上限値で、hardはrootが変更できる

上限値を意味する。


### Windowsの共有フォルダをマウントする

~~~
sudo yum install samba_client cifs_utils

sudo vi /etc/fstab
//Windows サーバのIP/フォルダ /Linux 側のマウントポイント   cifs　username=WINUSER,password=WINPASSWORD,workgroup=domian,uid=user,gid=group,file_mode=0700,　dir_mode=0700,defaults  0 0
~~~

これではセキュリティ的に弱いので


~~~
credentials=/etc/winpasswd
username=WINUSER
password=WINPASSWORD
domain=domain
~~~

http://www.asahi-net.or.jp/~si7d-htt/tips/auto_mount.html


### 空きメモリを調べるには

CentOS7と6にてやり方が異なるらしい。

CentOS7にはmeminfoにavailableという行があるのでそれを見ること。


http://nopipi.hatenablog.com/entry/2015/09/13/181026



### アクセス制御を行う

/etc/hosts.deny


`ALL : ALL`


/etc/hosts.allow


`ALL : 192.168.1.0/24 192.168.2.0/24 127.0.0.1`



/etc/hosts.allow, /etc/hosts.deny への修正はただちに反映され、inetd, xinetd を再起動する必要はない。


### 文字コードunicodeとhexの変換

ASCII文字等にターミナルで変換する方法


**エンコード**

utf-8 hex

`echo "xE2x96xB2";`

utf-16 hex

`echo "u25C7";`



**デコード**

`echo -n "〶" |xxd -p -u`

or

`echo -n "〶" | od -A n -t x1`


サイトでは

http://www.fileformat.info/info/unicode/char/search.htm

を使えば良い。


### プロセス置換

一時ファイルを作ったりする手間が省ける



http://sechiro.hatenablog.com/entry/2013/08/15/bash%E3%81%AE%E3%83%97%E3%83%AD%E3%82%BB%E3%82%B9%E7%BD%AE%E6%8F%9B%E6%A9%9F%E8%83%BD%E3%82%92%E6%B4%BB%E7%94%A8%E3%81%97%E3%81%A6%E3%80%81%E3%82%B7%E3%82%A7%E3%83%AB%E4%BD%9C%E6%A5%AD%E3%82%84%E3%82%B9


### 行末の空行（trailing whitespace）を削除する

sed -i 's/[ t]*$//'  filename


### bashで覚えておきたいキーバインド（ショートカット）


http://orebibou.com/2015/06/bash%E3%81%A7%E8%A6%9A%E3%81%88%E3%81%A6%E3%81%8A%E3%81%8D%E3%81%9F%E3%81%84%E3%82%B7%E3%83%A7%E3%83%BC%E3%83%88%E3%82%AB%E3%83%83%E3%83%88%E3%82%AD%E3%83%BC%E3%82%AD%E3%83%BC%E3%83%90%E3%82%A4/


http://hogem.hatenablog.com/entry/20090411/1239451878


### CentOS6でvimのクリップボード共有

centosではvim-gnomeがないので別の手段をとる。

まず、vim-X11をインストールする。

すると、gvimが使えるようになる。

あとは、

alias vi='gvim -v'

とすればよい



### 起動中のプロセスのリダイレクト先を変更する

単純には無理っぽい。

gdbを起動してdupすればいける模様。


https://siguniang.wordpress.com/2013/08/29/redirect-a-running-process-output-to-a-file-and-log-out/


### bashのキーバインド

http://rcmdnk.github.io/blog/2013/06/01/computer-bash-linux-mac/

http://d.hatena.ne.jp/hogem/20090411/1239451878


### 隠し(.)ファイルを含めてコピーする

1.

shopt -s dotglob

shopt -u dotglob


2.

cp dirA/{.*,*} dirB



### ddの進捗を見る

kill -USR1 12345

killall -USR1 dd


とかUSR1シグナルを使う



### コマンドラインでセミコロンとパイプの優先順位を変える

`{}` を利用する



http://askubuntu.com/questions/133386/how-to-merge-and-pipe-results-from-two-different-commands-to-single-command


### ログインしているユーザにメッセージを送る

wall 全ユーザにメッセージを送る

write 個別に送る


http://d.hatena.ne.jp/horus531/20110110/1294647041


### コマンドラインから入力画面等を簡単に出す方法

zenityコマンドを使う


### TUIを簡単に使う

dialogコマンドを使う


http://rikukuu.blog27.fc2.com/blog-entry-44.html


### コピーコマンド（cp）でのバックアップを変更する

`export SIMPLE_BACKUP_SUFFIX=`date +_%Y%m%d``



### ターミナルでの一時的な変数宣言

bash/zsh ならコマンドの前に varname=value というのがあると、そのコマンドを実行するときだけ環境変数を設定してくれる。

主に man か configure のときに実行する。



### 自作yumリポジトリ作成

yum install createrepo

createrepo /tmp/Packages


これで、repodataディレクトリが作成される。


~~~
[repos.myrepo]
name=CentOS-$releasever - myyumrepo
baseurl=http://xxx
enable=1
gpcheck=0
~~~



### ssh時のログインメッセージ変更

/etc/update-motd.d


### ディレクトリの意味

libexec: 内部から呼び出される実行ファイル



### コマンド実行確認のデフォルト表現

[y/N] だと大文字、つまりnoがデフォルトとなる


## FAQ

#### xdg-openのデフォルトを変更したい

`xdg-mime query default text/html`
`xdg-settings set default-web-browser chromium.desktop`


#### Windows10が入っているUEFIでLinuxに書き換えると時々起動せず、回復コンソールが立ち上がる

* Windows情報を削除
sudo efibootmgr

sudo efibootmgr -B -b 0000


* Windows設定を削除（しないとefibootmgrで消したものが復活する）
sudo rm /boot/efi/EFI/Boot/bootx64.efi /boot/efi/EFI/Microsoft



http://blog.myskng.xyz/entry/2016/07/04/005126



#### シンボリックリンク切れのファイルを検索したい

`find . -xtype l`



#### リソースが一時的に利用できませんが表示される

ulimit -a

で

-u: processes                  1024

が小さいのが原因

ulimit -u unlimit

とするか

/etc/security/limits.d/90-nproc.conf

or

/etc/security/limits.conf

に

@ *          soft    nproc     unlimited@

と記述する


#### scpしたときとかに「stty: 標準入力: 無効な引数です」が表示される

sttyがリモートで実行されることが原因。


`[ -t 0 ] && stty erase '^?'`

とかすればよい


もしくは

case "$TERM" in

kterm|xterm|sun)

でもよい


#### NFSもlocateで検索したい！

/etc/updatedb.conf


PRUNE_BIND_MOUNTS = "no"

PRUNEFS から nfs や NFS を除外




#### 直前のコマンドのpidを取得する

myCommand & echo $!


http://serverfault.com/questions/205498/how-to-get-pid-of-just-started-process


#### sshでスクリーンショットを取りたい


DISPLAY=:0.0 import -window root /tmp/shot.png


#### ネットワークの設定をGUIを使わずに簡単に実施したい

system-config-network-tui

を使う




#### LVMに新規追加

pvcreate /dev/xxx

あとは、system-config/lvmで管理できる。


#### ターミナルのリセット方法

exec bash/zsh/tcsh


#### 環境変数のリセット方法

env -i bash ~/.bashrc

env - bash ~/.bashrc

bash -l


#### 環境円数すらもリセット

env -i bash --noprofile --norc


#### CentOS7の日本語化

`sudo yum -y install ibus-kkc vlgothic-*`

`sudo localectl set-locale LANG=ja_JP.UTF-8`


http://www.server-world.info/query?os=CentOS_7&p=japanese



#### Firefoxで日本語入力できない

`sudo yum install gtk2-immodule-xim`


#### Firefoxでflash playerを使う

1. flash playerのlinux用tar.gzをダウンロード解凍する
2. cp libflashplayer.so ~/.mozilla/plugins/
 
#### Ubuntuでalt+tabが無効にできない

Open dconf-editor

org → gnome → desktop → wm → keybindings.

Change the value for the correct key (in your case, this should be show-desktop).


#### LinuxでAutoHotKeyのようなキーバインドにするには

xvkbd xbindkeys or xmodmapを使えば一応可能。 -> Xmodmapはやめたほうがいい

使うならsetxkbmapかloadkeys使うかudevのhwdbがいい

setxkbmapの設定は/usr/share/X11/xkb/rules/evdev.lstや/usr/share/X11/xkb/symbols/にある

しかし、以下の点で不具合がでるため使用しない。
・ターミナルのショートカットと衝突する
・Linuxではalt_l,alt_rという認識はしていないため、mod1,mod2として使用する必要がある。そうすると片方のaltはaltとして機能しなくなる（controlはしている模様）

