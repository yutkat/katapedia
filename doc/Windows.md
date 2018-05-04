# Windows

## ツール

### インストールしているツール

* Everything
高速検索ソフト

http://www.voidtools.com/

* WinSCP
http://winscp.net/eng/download.php

* TeraTerm
http://sourceforge.jp/projects/ttssh2/

* SakuraEditor
http://sakura-editor.sourceforge.net/

* WinMerge
http://winmerge.org/

* LhaForge
圧縮解凍ソフト

* Paint.NET
使いやすいペイントツール

http://www.getpaint.net/

* clibor
http://www.forest.impress.co.jp/library/software/clibor/

~~CLCL~~ 開発終了しているみたいなので

http://www.nakka.com/soft/clcl/

* Orchis
ランチャー

http://www.eonet.ne.jp/~gorota/

* [Autohotkey](Autohotkey.md)
キーマップカスタマイズソフト

http://www.autohotkey.com/

* マウ筋
http://www.vector.co.jp/soft/win95/util/se264166.html

* tclock
http://k-takata.bbs.coocan.jp/?m=listthread&t_id=56

* ~~[Console2_NYAOS](Console2_NYAOS.md)~~ 折り返しと設定がめんどいのでConEmuに乗り換え
* ConEmu
http://kenpg.bitbucket.org/blog/201506/07.html

日本語表示がカーソル位置とずれるのでsettings->Main->Monospaceのチェックを外して、Save Settingsすること。

windowsコンソールの拡張

* [Haroopad](Haroopad.md)
markdownエディタ

http://pad.haroopress.com/

* CClenaer
一応入れておこう

http://filehippo.com/jp/download_ccleaner/


#### 社内規定的にアンインストールしたもの

* AutoHotKey
* LhaForge
* Everything
* CCleaner

### AutoHotKey代替ツール

ZeniSynth, yamy, keyhac


### Teraterm

#### カーソルの形状が変化しない

Additional Settings ダイアログの Control sequence タブに有る Cursor control sequence を on にする必要があります。

https://ttssh2.osdn.jp/manual/ja/usage/tips/vim.html



#### TERM環境変数の設定を変更する

TERATERM.INIの以下のように設定する

;TermType=xterm

TermType=xterm-256color



#### ウィンドウ色

文字 R:188 G:188 B:188

背景 R:28 G:28 B:28



#### ✔等の特殊文字が表示できない

UTF-8の拡張に対応しておらず、UTF-8の3バイトまでしか対応していない


http://ttssh2.osdn.jp/manual/ja/usage/unicode.html


### TortoiseSVN

#### アイコンが変わらない

右クリックで出てくる設定メニューより、「アイコンオーバーレイ」メニュー、「ドライブの種類」で「ネットワークドライブ」にチェックを入れる


### Sublime Text

### brackets

proxyを設定しても拡張機能マネージャが繋がらない。。。


### サクラエディタ

#### マークダウンの設定


1. http://sakura.qp.land.to/?Customize%2F%C5%EA%B9%C6%2F74
から.rkwと.colをダウンロードする

1. C:Program Filessakurakeyword に.rkwと.colをいれる
1. タイプ別設定一覧の17あたりの空いている場所を設定変更する
1. スクリーンタブ
名前：Markdown,拡張子：md,markdown

1. カラー インポート .col ファイル
1. 正規表現キーワード 正規表現キーワードを使用するにチェック
インポート .rkwファイル

1. 閉じる
1. プラグイン導入。ダウンロードする
https://github.com/ngyuki/sakuraeditor-plugin-markdown

1. 共通設定->プラグインZIPプラグインを導入


## WSH

### VBScriptとJScriptの違い

WSH＋VBSが情報が多いのでこちらがオススメ。

HTML内の値を直接扱いたいのならJSのほうが、JavaScript感覚なので使いやすいかも。


VBS

* エラー処理。 VBScriptは、例外処理のためのOn Error Resume Nextステートメントを持っています。サーバー スクリプトを書くときには、スクリプトは無人状態で実行されるので、エラー処理が特に重要となります。
* フォーマッティング。 VBScriptには、日付、数値、および金額データを簡単にフォーマットできる関数が含まれています。


JScript

* 動的な実行
* オブジェクト指向

https://msdn.microsoft.com/ja-jp/library/cc482765.aspx


## Tips

#### スタートメニューに最近使ったフォルダを入れたい

C:Windowsexplorer.exeをドラッグアンドドロップすればいける


#### エクスプローラーの検索でファイル名で検索したい

system.filename:~="xxx"


http://windows.microsoft.com/ja-jp/windows7/advanced-tips-for-searching-in-windows


#### 特定のサイトの保護ビューを無効にする

インターネットオプションをクリックします。

　→「セキュリティ」タブをクリックします。

　　→「信頼済みサイト」をクリックし、その下のサイトをクリックします。

　　　→「このゾーンのサイトにはすべてサーバーの確認(https)を必要とする(S)」のチェックをはずします。

　　　　→「このWebサイトをゾーンに追加する」に対象のFQDNまたはIPアドレスを入力し、追加をクリックします。


できなければ、Excel/Wordから→オプション→セキュリティセンター→信頼できる場所

で

プライベートネットワーク上にある信頼できる場所を許可するをチェックし

ユーザ指定の場所に追加する。（サブフォルダも許可するにチェックもする）



#### Windowsにてパフォーマンス監視を行う

CPU,メモリ,ディスクI/Oなどを調べる


スタートメニュー->プログラムとファイルの検索->「perfmon」

あとは、モニターしたいイベントを記録する。

※バイナリデータに保存されるため、可視化するにはもう一手順必要




#### Windows7の検索結果から一つ上のフォルダに移動できない

検索結果から右クリック->「フォルダーの場所を開く」（下の方にある）を選択する。

新しいウィンドウで開くでは戻れなくなるので注意！


新しいウィンドウで開いてしまったあとは、ctrl+右クリックでパスとしてコピーを選択し、アドレスバーに貼り付ける。


#### パフォーマンス改善

|  項目|  適用|  備考|
|---|---|---|
|Aeroプレビューを有効にする|○|タスクバーのAeroは便利なため|
|Aeroプレビューを有効にする|○|タスクバーのAeroは便利なため|
|Windows内のアニメーション コントロールと要素|||
|アイコンの代わりに縮小版を表示する|○|画像を探しやすくなるため|
|ウィンドウとボタンに視覚スタイルを使用する|○|クラシックぽくなってださいため|
|ウィンドウの下に影を表示する||ウィンドウの重なり具合はわかりやすくなる|
|ウィンドウを最大化や最小化するときにアニメーションで表示する|||
|コンボ ボックスをスライドして開く|||
|スクリーン フォントの縁を滑らかにする|○|ないとフォントが見るに堪えない状況に|
|タスク バーとスタート メニューでアニメーションを表示する|||
|タスクバーの縮小版のプレビューを保存する|○|Aeroを使う場合入れといたほうがタスクバーの処理が高速になる|
|デスクトップ コンポジションを有効にする|○|入れないとAeroが切れる|
|デスクトップのアイコン名に影を付ける|○|チェックしないと同じ白い部分にアイコンを置くとファイル名が見えなくなるため|
|ドラッグ中にウィンドウの内容を表示する|||
|ヒントをフェードまたはスライドで表示する|||
|マウス ポインターの下に影を表示する|||
|メニューをフェードまたはスライドして表示する|||
|メニュー項目をクリック後にフェードアウトする|||
|リストボックスを滑らかにスクロールする|||
|透明感を有効にする|||
|半透明の［選択］ツールを表示する|||

http://news.mynavi.jp/special/2001/windowsxp-sp/03/030.html


#### Windows Searchの設定変更で低負荷化

インデックスオプションのインデックスが作成された場所を変更する。


1. コントロールパネル
1. インデックスのオプション
1. 変更
1. チェックボックスを外す。（Usersとスタートメニューは外れなくなっている）
1. 詳細設定
1. インデックスの再構築

#### タスクバーでのマウスの挙動

タスクバーのウィンドウアイコンをホイールクリックすると新しいウィンドウが生まれる。

逆にポップアップした画面でホイールクリックするとウィンドウが閉じる。



#### リモートデスクトップでデュアルディスプレイ

`mstsc /span`


#### スペースを含むフルパス　Windowsをスペースなしに変換

dir /xでいちいち調べる


#### Excelシートタブの簡単移動

Excelでシートが多くなりすぎた際には、左下の◀▶のところで右クリックで一覧を表示できる


#### infocage FRM ARGUSの無効化

ICF_AutoCapsule_disabledフォルダに格納する


#### ここでコマンドプロンプトを開く

shift+右クリックで右クリックメニューに表示されるようになる


#### ファイルフルパスをコピー

ファイル選択後、shift+右クリックで右クリックメニューにパスとしてコピーが表示されるようになる。



#### スリープ時にネットワークから切断されないようにする

Intel Network Adapter Driver for Windows 7(Intel PROSet)というアプリをインストールする


http://dogrungames.seesaa.net/article/263483916.html


### ショートカット

**エクスプローラ**

alt+d : アドレスバーに移動



---


## ○Windowsでコマンドプロンプトを使いやすく

[Console2+NYAOS](Console2+NYAOS.md)



### ○ 日本語のレイアウト崩れを修正

両方とも適当なフォルダに解凍して、Console2を起動する。

Console2のメニューバーの「View」→「Console Window」

開いた「Console Window」のタイトルバーあたりで右クリックして、「プロパティ」を開く。

フォントタブを選択し、MSゴシックを選択してOKして「Console Window」を閉じる。その時Console2も一緒に終了するのであせらない。


## Outlook

Outlookをカレンダーで起動する


C:Program FilesMicrosoft OfficeOffice14OUTLOOK.EXE  /select outlook:calendar /recycle


---


## Windowsチューニング

### エクスプローラーの起動が遅い

フォルダーとデスクトップの項目の説明をポップアップで表示するのチェックを外す

と軽快に動作するようになる。



## Windowsカスタマイズ（レジストリ変更）

### タスクバーの表示を常にリスト表示にする

[HKEY_CURRENT_USERSoftwareMicrosoftWindowsCurrentVersionExplorerTaskband]

"NumThumbnails"=dword:00000000


### タスクバーでポップアップするツールチップが遅い

現状改善する方法はない模様。

http://answers.microsoft.com/en-us/windows/forum/windows_7-desktop/setting-to-speed-up-taskbar-tooltip-display/c881c535-35f6-4075-9b91-7a40b9582d2d


### ツールチップの表示時間の変更

HKEY_CURRENT_USERControl PanelMouse

MouseHoverTime


http://www.sevenforums.com/tutorials/1884-mouse-hover-time-change.html


### コピー時のコピー命名方法の変更

1. 管理者権限で「regedit」を実行。
1. レジストリエディターが起動したら、HKEY_CURRENT_USER  Software  Microsoft  Windows  CurrentVersion  Explorerキーを開く。
1. 新たにNamingTemplatesキーを作成。
1. 文字列値「CopyNameTemplate」を作成し、データ値を「Copy()-%s」に変更。
1. ［F5］キーを押して変更内容をシステムに反映させてから、レジストリエディターを終了。
1. リブートする。


"参考ページ":http://terrygeek.wordpress.com/2012/04/28/windows7-%E3%81%A7%E3%82%B3%E3%83%94%E3%83%BC%E3%81%97%E3%81%9F%E6%99%82%E3%81%AE%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E5%90%8D%E3%81%AE%E9%A0%AD%E3%82%92%E3%80%8C%E3%82%B3%E3%83%94%E3%83%BC-%E3%80%9C/


### へのショートカットを非表示にする

HKEY_CURRENT_USER  Software  Microsoft  Windows  CurrentVersion  Explorer

link

1E 00 00 00

->

00 00 00 00


### AltTabでウィンドウ表示を高速化

HKEY_CURRENT_USERSoftwareMicrosoftWindowsCurrentVersionExplorerAltTab

を追加

その中にDWORDキー、名前：LivePreview_ms 値：1で入れると1ms後にウィンドウが表示される。


### Aeroプレビューでデスクトッププレビューを表示するまでの待機時間

HKEY_CURRENT_USERSoftwareMicrosoftWindowsCurrentVersionExplorerAdvanced

DWORD値

DesktopLivePreviewHoverTime


### ライブサムネイルでマウスポインタを合わせたウインドウをAeroプレビューで表示するまでの待機時間

HKEY_CURRENT_USERSoftwareMicrosoftWindowsCurrentVersionExplorerAdvanced

DWORD値

ThumbnailLivePreviewHoverTime

0


### ライブサムネイルを表示するまでの待機時間

HKEY_CURRENT_USERSoftwareMicrosoftWindowsCurrentVersionExplorerAdvanced

DWORD値

ExtendedUIHoverTime

0


デフォルト 400

0だと短い？当たっただけで表示される。100くらいにしてみる。


### スタートメニュー表示

HKEY_CURRENT_USERControl PanelDesktop

文字列値

MenuShowDelay

0



### Excelを別ウィンドウで開く

HKEY_CLASSES_ROOTExcel.Sheet.12shell


HKEY_CLASSES_ROOTExcel.Sheet.8shell



http://plaza.rakuten.co.jp/mscrtf/diary/201212310000/


## TroubleShooting

#### TortoiseSVNでアイコンオーバーレイが表示されない

* アイコンキャッシュの再構成
"C:Program FilesTortoiseSVNbinTortoiseProc.exe" /command:rebuildiconcache

* TortoiseSVN 設定から「Icon Overlays」Drive Typeにすべてチェックを入れる
* TortoiseSVN 設定から「Overlay Handlers」でstart registry editorからレジストリに含まれるキーのうちTortoiseSVNの優先度を上げる
* start registry editorのキーを11個にする
* インストーラーから修復をする
* ネットワークドライブだとうまく認識できない場合がある。直接ではなくネットワークドライブとして設定する
* Icon Overlays status cache Shell
https://tortoisesvn.net/faq.html#ovlsubst


#### 別のプログラムがこのフォルダーまたはファイルを開いているので・・・の原因と解決方法

http://www.se-support.com/helpdesk/folderrename.html


#### エクスプローラーを詳細表示だと空白部分がないため、貼り付けや新規作成がやりにくい

~~解決策はない模様~~

http://answers.microsoft.com/ja-jp/windows/forum/windows_7-files/%E8%B3%AA%E5%95%8Fwindows-7/2c4171bb-8b00-4d08-90c6-9f1e1780d8ef


あった！


![](/img/explorer.jpg)

赤い部分で新規作成＆貼り付けできる！

青い部分にドロップしても貼り付けられる。




#### アイコンが表示されなくなった


1. 「C:/ユーザー/ユーザー名/AppData/Local/IconCache.db」を削除
1. 「IconCache.db」があったフォルダ内で右クリックし「新規作成」で「テキストドキュメント」を作成
1. 作成された「新しいテキスト ドキュメント.txt」のファイル名を「IconCache.db」にする
1.  「IconCache.db」を右クリックして「プロパティ」を開く
1.  「プロパティ」の「属性」の「読み取り専用」にチェックして OK
1. マシンを再起動

http://szdy.info/wp/2009/11/22/872/



#### フォルダのリネーム、削除ができない

**事象**

エクスプローラーによってファイルが開かれているため、操作を完了できません

Windowsで「Thumbs.db」ファイルなどを削除しようとすると、「エクスプローラによってファイルが開かれているため、操作を完了できません。ファイルを閉じてから再実行してください。」と表示されることがあります。


**解決**

Windowsエクスプローラの［整理］－［フォルダーと検索のオプション］メニューを選択し、［フォルダー オプション］ダイアログを開く。［フォルダー オプション］ダイアログの［表示］タブを開き、「詳細設定」の［常にアイコンを表示し、縮小版は表示しない］のチェックを入れれば、Thumbs.dbによる縮小画像の表示がオフになり、フォルダの名前の変更や削除が可能になる。


**参考**

http://www.atmarkit.co.jp/ait/articles/1205/25/news122.html


## サポート

マイクロソフトのサポート期限を検索できるサイト

http://support2.microsoft.com/lifecycle/search/?ln=ja


## FAQ

### Windows7にてデュアルディスプレイに別々の壁紙を設定したい

I can run one desktop background image spanned across both screens out of the box, with no extra software installed. The trick is to find an image that matches the resolution of both screens together. In my case, with 17" monitors both at 1280x1024, I need an image that is 2560x1024.

Now go to Control Panel>Apperance and Personalization>Personalization>Desktop Background and select the image. Then set the picture position to "Tile". Your background image should now be spanned across both screens.


### Q.スタートメニュー等でポップアップが表示されない

1. When the "Folder Options" multi-tabbed dialog box appears, click the "View" tab.

2. Scroll down and check or uncheck "Show pop-up description for folders and desktop items" as desired.


### Q.タスクバーの同一グループのウィンドウの位置を入れ替える

A. デフォルトではできない。以下のツールを入れると可能な模様。

http://jutememo.blogspot.jp/2014/03/windows-7.html


### Q. Windows標準検索機能でWord,Excelの本文検索が実行できない。

A. ネットワークドライブにあるため。Windows標準検索はインデックスされていると、ファイル名＋本文となるが、インデックスされてないとファイル名のみの検索となる。そのうち暗号化されるので、暗号化がかからないところに置かないとまた検索できなくなる。


### Q. Office標準のPDF変換ができない！

A. armsでロックされているのが原因。ロック解除は管理職しかできない。プリンタドライバを使用したpdf化ならできるので、それを使用したほうがよい。

