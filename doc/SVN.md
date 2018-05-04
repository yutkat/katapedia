# SVN

## TortoiseSVN

### 使い方

1. まず、リポジトリを作る。適当な空フォルダで"Create Repository here"をクリック。
1. "Create folder structure"をクリックし、trunk,tags,branchを作成する。
1. 次に、チェックアウトする。適当な空のフォルダにて"SVN Checkout"を実行。
1. URL of repositoryに先ほど作ったリポジトリ（フォルダパスはWindows形式のままでいい模様。file://は必要。）、Checkout directoryにチェックアウトするフォルダを入れ、実行する。
1. リポジトリの中身が落ちてくる。

### Windows不要ファイルのignore設定

設定->General->Subversion->Global ignore pattern


`*.o **.lo *.la *.al .libs *.so *.so.[0-9]** *.a **.pyc *.pyo __pycache__ *.rej *~ #*# .#** .*.swp .DS_Store .project .metadata Thumbs.db *.tpl.php old bak`


## 文書管理をSVNで、Redmineで管理する方法

1. WindowsにTortoiseSVNをインストールする
1. リポジトリを作成する（このとき、日本語が入っているリポジトリではだめ）。リポジトリは共有できる場所におく（部内共有サーバ等）。
1. Redmineサーバから共有サーバをマウントする。
`sudo mount -t cifs://uri  -o user=username,workgroup=WORKGROUP  /mnt/tmp`

マウントした後のディレクトリに日本語が入らないようマウントポイントを工夫すること

1. Redmineのリポジトリ設定からマウントしたポイントを参照するようにする

## フック

### パラメータ

フック時には以下のようなパラメータが入ってくる


~~~
/path/to/repository 3 2-2
~~~

$1にリポジトリ名

$2にリビジョン

$3にトランザクション名


更新ファイルの情報を取得したい場合


`svnlook changed -r $REV $REPOS`


を実行すると


~~~
A sample2.txt
A sample3.txt
A sample.txt
~~~

のような形式で格納されている


## Tips

### WindowsでSVNのcheckoutを自動化

~~~

Dim repos_dir, to_dir

'①コマンドライン引数の情報を保存
Set objParm = Wscript.Arguments

'②取得したコマンドライン引数が2つ未満のときはエラー
If objParm.Count < 2 Then
WScript.echo "コマンドライン引数が足りません"

'③取得したコマンドライン引数2つより多いときもエラー
ElseIf objParm.Count > 2 Then
WScript.echo "コマンドライン引数が多いです"

Else
'④取得したコマンドライン引数が2つのときはOKなので
'コマンドライン引数の内容を取得し表示する
repos_dir = objParm(0)
to_dir = objParm(1)
WScript.echo "コマンドライン引数1=" & repos_dir
WScript.echo "コマンドライン引数2=" & to_dir
End If


cmd = "svn info " & to_dir
Return = ExecCmd(cmd)

If Return = 1 Then
cmd = "svn checkout " & repos_dir & " " & to_dir
Return = SelectExecSvn(cmd)
Else
cmd = "svn update " & to_dir
Return = SelectExecSvn(cmd)
End If

MsgBox "svn " & Return & "!!"


Function SelectExecSvn(cmd)
Dim msg
Return = ExecCmd(cmd)
If Return = 0 Then
msg = "checkout success!!"
Else
msg = "checkout failed !!"
End If
SelectExecSvn = msg
End Function

Function ExecCmd(cmd)
Set WshShell = WScript.CreateObject("WScript.Shell")
ExecCmd = WshShell.Run(cmd, 0, true)
End Function

~~~



### astahをTortoiseSVN管理する

[差分の高度な設定]ダイアログを開き、[追加]


拡張子もしくはMIMEタイプ： .asta

外部プログラム： "%ASTAH_HOME%astah-commandw.exe" -diff %base %mine


## TroubleShooting

### Expected FS format between '1' and '6'; found format '7' とかでてチェックアウトできない

SVNのクライアントとサーバでバージョンが違うことが原因

バージョンをあげるか、サーバのバージョンを落とす


~~~
svnadmin dump :repo_name > aaa.dump
svnadmin load :new_repo_name < aaa.dump
~~~




### TortoiseSVNでアイコンが更新されない

1. 右クリックしてSettingsを選ぶ。
1. ウィンドウが開くので、Icon Overlaysを選び、Status cacheをNoneにする。ウィンドウを閉じる。
1. エクスプローラーの表示を更新すると、アイコンが更新されている。
1. もう一度Settingsを開き、Status cacheの設定を元に戻しておく。
1. エクスプローラーの表示を更新すると、アイコンの表示が直る。

