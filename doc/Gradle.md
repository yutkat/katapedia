# Gradle

## 基本操作

### タスク一覧確認

`gradle -q task --all`

### コンポーネントのタイプとかを表示

`gradle components`



## FAQ

### withTypeのタイプを見たい

`gradle help --task junitPlatformTest`


### C/C++でアウトプットされるバイナリ名を動的に変更したい


``` groovy
    new File('src/main/c/').eachDir() {
        "${it.name}"(NativeExecutableSpec) {
            sources {
                c {
                    source {
                        srcDirs "src/main/c/${it.name}"
                        include "**/*.c"
                        lib library: "Aaa"
                        lib library: "Bbb"
                    }
                    exportedHeaders { srcDirs "src/main/c/include" }
                    exportedHeaders { srcDirs "src/main/c/include" }
                }
            }
        }
    }
```


