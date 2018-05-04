# Redmineプラグイン

redmine プラグインplugin

redmine/htdocsにて


* インストール
rake redmine:plugins:migrate RAILS_ENV=production


* アンインストール
rake redmine:plugins:migrate NAME=redmine_wiki_extensions VERSION=0 RAILS_ENV=production

or

ruby /script/plugin remove {name_of_the_plugin}

