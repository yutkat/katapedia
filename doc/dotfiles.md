# dotfiles

https://qiita.com/yutakatay/items/c6c7584d9795799ee164


## zsh

### aliasの設定時の注意

#### 不要なコマンドが起動時に呼ばれてしまう

`alias ls="ls -l $(find /)"`
などのようにでサブシェル実行しない

起動時に呼ばれてしまい起動が遅くなる

`alias ls='ls -l $(find /)'`
シングルクオートなら起動時に実行されないですむ

https://stackoverflow.com/questions/47211413/zsh-alias-with-command-parameter

### zinit

#### サンプル設定

- https://github.com/zdharma/zinit-configs/blob/51c2be99224ab3f5d25e0b707f1f1da81136937d/NICHOLAS85/.zshrc
- https://github.com/chhschou/dotfiles/blob/7fd3683e7da62bf0eed9ad1898ba237ea0fccbca/.zsh/zplugin/cmdline_prog.zsh
- https://github.com/zdharma/zinit-configs/blob/3218f7f9c601d035a1ca732d2ae8b09aa72ffdb5/psprint/zshrc.zsh

