# Console2+NYAOS


console2にてEdit->Settings->ShellにNyaosのパスを追加


### ・コマンド履歴

nyaosのインストールディレクトリにて

~~~
type nul > .history
~~~
履歴ファイルを作成する。


次に、同じディレクトリの_nyaファイルを開き末尾に

~~~
option savehist C:Program Filesnyaos-3.3.8_3-win.history
option histfilesize 1000
~~~

を追加する。

