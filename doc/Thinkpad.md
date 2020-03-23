
# Thinkpad

## Linux　

### 入れたパッケージ

`sudo pacman -S ripgrep mlocate xf86-input-synaptics xorg-xinput tpacpi-bat acpi_call urxvt-perls`

`sudo pacman -S tlp acpi`

`sudo systemctl enable tlp`

`sudo yay -S i3-easyfocus-git clipmenu urxvt-resize-font-git`

`sudo pacman -S rustup go clang`



### バッテリー

### バッテリーを長持ちさせる設定

https://wiki.archlinux.jp/index.php/Tp_smapi


### Troubleshooting

#### トラックポイントの速度変更

なぜかsystemd-hwdbのPOINTINGSTICK_SENSITIVITYではうまくいかない
たぶん名前が悪い？TPPS/2 Elan TrackPoint。IBMならうまくいくみたい。

xinputはうまくいく。
Waylandでも/etc/libinput/xxx.quirksならうまくいった

#### システム暗号化

dm-encryptを使ってcryptsetupしてシステムを暗号化したが、起動時にパスワードプロンプトが表示されない

/etc/mkinitcpio.conf のMODULESにi915を追加したら表示されるようになる 

https://wiki.archlinux.org/index.php/Lenovo_ThinkPad_X1_Carbon_(Gen_6)
