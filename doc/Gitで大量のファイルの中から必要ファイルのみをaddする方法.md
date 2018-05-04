# Gitで大量のファイルの中から必要ファイルのみをaddする方法

1. コミットしたいものリストAを作成する。

2. 以下のシェルをリストAに対して実行し、ディレクトリのリストBを作成する。


~~~
#!/bin/bash

path=`dirname $1`
if [ ${path:0:1} == "/" ];then
cnt=${tmp:1}
else
cnt=`dirname $1`
fi


echo $path

MAX_LOOP=`echo -n $cnt | sed -e 's@[^/]@@g' | wc -c`

i=0

while [ $i -le $MAX_LOOP ];do
echo ${path%/*}
path=`echo -n ${path%/*}`
i=`expr $i + 1`
done

~~~

3. 以下のシェルをリストBに対して実行し、.gitignoreに追加するリストCを作成する。


~~~
#!/bin/bash

NOTIGNORE=`echo $1 | awk '{print "!/"$1"/"}'`
IGNORE=`echo $1 | awk '{print "/"$1"/*"}'`

echo $NOTIGNORE
echo $IGNORE
~~~

4. リストAの上にリストCをくっつける。



~~~
###### all ignore ######
/*
/.*

# リストCを貼り付ける

# リストAを貼り付ける !/を先頭につけること

!.gitignore

~~~







