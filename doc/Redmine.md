# Redmine

## プラグイン

* dmsf
http://www.r-labs.org/projects/r-labs/wiki/%E3%83%97%E3%83%A9%E3%82%B0%E3%82%A4%E3%83%B3DMSF%E3%81%AE%E6%A4%9C%E7%B4%A2%E3%82%A8%E3%83%B3%E3%82%B8%E3%83%B3%E3%82%92Hyper_Estraier%E3%81%B8%E6%9B%BF%E3%81%88%E3%82%8B

http://blog.scimpr.com/2012/08/14/redmine2-0%E3%81%AEdmsf%E3%83%95%E3%82%A1%E3%82%A4%E3%83%AB%E3%82%92hyper-estraier%E3%81%A7%E5%85%A8%E6%96%87%E6%A4%9C%E7%B4%A2%EF%BD%9Epdf%E6%A4%9C%E7%B4%A2%E7%B7%A8/


* 用語集
https://github.com/chiastolite/redmine_glossary.git

CSV読み取り部分にバグがある。今後直す予定。とりあえず、csvアウトプットの形に整形して、ヘッダ行を削除して、一行目をコメントアウトチェック外せば運用回避可能。なお、説明(13列)は絶対に必要な模様


## 使いやすくする設定

public/themes/farend_fancy/stylesheet/application.css


~~~

fieldset.preview {
margin-left: -30px;
padding-left: 30px;
padding-right: -40px;
}

div.wiki pre {
/* farend_basic: preで横スクロールバーを表示させずに折り返す */;
border-radius: 3px;
white-space: -moz-pre-wrap; /* Mozilla */
white-space: -pre-wrap; /* Opera 4-6 */
white-space: -o-pre-wrap; /* Opera 7 */
white-space: pre-wrap; /* CSS3 */
word-wrap: break-word; /* IE 5.5+ */
}

div.wiki {
margin-left:30px;
}

div.wiki p {
line-height: 165%;
}

div.wiki li {
margin-bottom: 4px;
}

div.wiki h1 {
margin-left:-30px;
}

div.wiki h2 {
margin-left:-20px;
}

div.wiki h3 {
margin-left:-10px;
}

div.wiki h4 {
margin-left:0px;
}
~~~

### Redmine導入失敗ケース

* はじめから完全な入力を求めルールを重視しすぎる、メンバーがついてこない”自己満足リーダーケース”
* 最低限の規律をつくらずに、プロジェクトごと放置される”結局みんなメモ帳でタスク管理に戻るのねケース”
* 高性能のツールさえ使えばうまくいくと考えて導入しはじめた”夢と希望にあふれるだけのケース”
* 「うちは独自だから」といって個別に立てたのに使わない”私はあなたと違うのよケース”

### ルール


* 作業を行うことは必ずチケット を書くこと

* 一日一回（業務終了時）は 必ず更新 すること

*  必ず期日 を記入すること

*  ゴール を明確にすること

*  一元管理 を徹底すること

* 作業チケットを承認したら、 ステータスを進行中 とすること

* バグチケットの クローズはPL が行うこと
　　　→80%にて担当者作業完了


* バグチケットは必ず説明（発生状況、手順等）を記述すること。

* クローズする際にはもう一度ゴールを確認すること。

* チケットの開始日時は(締切-予定日数*2)とする。（開始日を作業開始日とするとガントチャートが見にくくなるため）
※ただし、見直し必要チケット（メモの類）は開始日から作業日時を設定すること。






### 拡張

* [RedmineDドライブへの保存](RedmineDドライブへの保存.md)

* [Redmineプラグイン](Redmineプラグイン.md)

* [Redmineアップデート](Redmineアップデート.md)

* [Redmine文字化け](Redmine文字化け.md)

* [Redmineメール通知](Redmineメール通知.md)

* [html,pdfに直リンクする（ダウンロードしない）方法](html,pdfに直リンクする（ダウンロードしない）方法.md)

## TroubleShooting

### Checklistsプラグインで他の人からチェックリストが見えない

ロールと権限の チェックリストの参照  チェックリストへ済印  チェックリストの編集にチェックを入れればよい




### ChecklistsプラグインでIncorrect string value: 'xE3x81xAExE3x83x91...' for column 'subject' at row 1: INSERT INTO `checklists` (`subject`, `issue_id`, `created_at`,`updated_at`) VALUES ('SPICEのパッケージリスト。', 1027, '2017-02-17 09:50:25.602079', '2017-02-17 09:50:25.602079')):

~~~
mysql -u redmine -p

use redmine
alter table checklist_template_categories default character set = utf8 collate = utf8_general_ci
alter table checklist_template_categories convert to character set utf8 collate utf8_general_ci;
alter table checklist_templates default character set = utf8 collate = utf8_general_ci;
alter table checklist_templates convert to character set utf8 collate utf8_general_ci;
alter table checklists default character set = utf8 collate = utf8_general_ci;
alter table checklists convert to character set utf8 collate utf8_general_ci;
~~~


### Mysql2::Error: Field 'position' doesn't have a default value: INSERT INTO `issues`のエラーが出た


BacklogのプラグインがpositionにNotNULL制約をしているため。

sqlで以下を発行


`ALTER TABLE issues MODIFY position INT(11);`


### psych.soでエラーとなる

gem uninstall psych

gem install psych


gemのバージョン違いも対象！（e.g. gem2.0 uninstall psych）


## Tips

### wikiのデータをエクスポート

1. 保存したい Wiki を開く

2. 画面右側の「索引(名前順)」を開く *1

3. 画面右下の「他の形式に出力 HTML」をクリック

4. 任意のファイル名を付けて保存


http://d.hatena.ne.jp/fyts/20090330/redmine


### 複数リポジトリにリンクする方法

doc|r102

code|commit:c6f4d0fd


## FAQ

* Redmine file upload internal errorとなる時
パーミッションが悪い。CREATOR OWNERというユーザをフルコントロールで許可すること


* redmine SQLバックアップ・リストア
~~~
import export backup
(パスワード等はconfig/database.ymlを見ること)

dump
mysqldump.exe --default-character-set=utf8 -u root --password=adminadmin --port=3306  --database bitnami_redmine > dump_utf8.sql

import

mysql.exe -u root --password=adminadmin --database bitnami_redmine < dump_utf8.sql

cd appsredminehtdocs
rake db:migrate RAILS_ENV="production" --trace
rake redmine:plugins:migrate RAILS_ENV=production --trace
rake tmp:cache:clear
rake tmp:sessions:clear
~~~

* Redmineの通知メールの送信者
https://groups.google.com/forum/?fromgroups=#!topic/redmine-users-ja/-QAodFNv3MQ

Mailer (app/models/mailer.rb)


* Redmineのwikiの画像調整
appsredminehtdocspublicstylesheets

application.css

div.wiki img { vertical-align: middle;}

↓

div.wiki img { vertical-align: middle; max-width: 98%}






* 表の中で改行
~~~
/var/lib/redmine/lib/redcloth3.rb
ALLOWED_TAGS = %w(redpre pre code notextile br)
<br>が使えるようになる
~~~

### Sidebar


~~~
h1. Sidebar

{{new_page}}

h3. 最近の更新

{{recent(10)}}

h3. 本日のアクセス数TOP5

{{popularity(5, 1)}}

h3. アクセス数TOP10

{{popularity(10)}}
{{tagcloud}}

h3. ナビゲーション


[Sidebar](Sidebar.md)
[Footer](Footer.md)
~~~


### Footer

~~~
{{comments}}
{{comment_form}}

p>. Updated by {{lastupdated_by}} , {{lastupdated_at}}{{count}}
Access count: {{show_count}} :since 2014-11-06
~~~

## トラッカー分類

|  トラッカー|  説明|
|---|---|
|バグ|バグ修正、PJの重大な問題、作業が止まるクリティカルな障害等|
|バグ|バグ修正、PJの重大な問題、作業が止まるクリティカルな障害等|
|機能|新たな機能の開発、既存の機能を改良等|
|サポート|成果物に直接結びつかない(ソースコードの変更が発生しない)作業|
|課題|PJの課題、懸念点、処置すべき事項の備忘録|
|メモ|メモ、備忘録、アイデア、要望|
|要求分析・要件定義|[機能を細分化]分析、検討|
|設計|[機能を細分化]分析、設計、設計書・仕様書の作成、テスト計画作成、レビュー|
|製造|[機能を細分化]コーディング、テストコード作成、コードレビュー指摘、コードレビュー反映、静的解析|
|テスト|[機能を細分化]試験実施、不具合対応|
|運用保守|[機能を細分化]要望・問い合わせ対応、不具合対応|
|管理|[サポートを細分化]進捗会議、進捗報告、進捗管理等|
|記録・情報整理|[サポートを細分化]ドキュメント作成、情報展開、フィードバック等。主に、マニュアル、報告書の作成のこと。|
|ストーリー|スクラムのストーリー|
|タスク|スクラムのタスク|

## 運用ルール

#### 基本方針

* すべての作業はRedmineに紐づくようにすること
* 作業を行う際には必ずチケット を書くこと
* 1日1回（業務終了時）は 必ず更新 すること
*  一元管理 を徹底すること

#### チケット

*  必ず期日 を記入すること
*  ゴール を明確にすること
* バグチケットの クローズはPL が行うこと
　　　→80%にて担当者作業完了

* バグチケットは必ず説明（発生状況、手順等）を記述すること。
* クローズする際にはもう一度ゴールを確認すること。
* チケットの開始日時は(締切-予定日数*2)とする。（開始日を作業開始日とするとガントチャートが見にくくなるため）
※ただし、見直し必要チケット（メモの類）は開始日から作業日時を設定すること。

* フィードバックチケットはフィードバックステータスにした人がクローズすること。

#### バージョン

* バージョンの終了はそのバージョンの『チケット全クローズ』と『振り返り・フィードバック作業』が必要
* バージョンの延期は理由を必ずニュースに記述すること。


#### Wiki

* Wikiに追記する場合は、@{{new(yyyy-mm-dd)}}@を記載し目立たせること。（yyyy-mm-ddは今日の日付に変更すること。google日本語入力を使っている人は「きょう」で変換すれば入力の手間が省ける）


#### その他

* ニコカレを更新すること。ニコカレにはPJの進捗ではなく体調・精神状態を記述すること

