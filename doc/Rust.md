# Rust

## ドキュメント

公式が一番参考になるのでぐぐる必要はない

- https://doc.rust-jp.rs/book/second-edition/
- https://doc.rust-jp.rs/rust-by-example-ja/
- https://doc.rust-lang.org/beta/std/index.html
- https://sinkuu.github.io/api-guidelines/
- https://cheats.rs/
- https://lib.rs/ (crate.ioよりも洗練されてて事実上の標準ライブラリがわかるようになっていてよい)

命名規約

https://sinkuu.github.io/api-guidelines/naming.html#%E5%A4%A7%E6%96%87%E5%AD%97%E5%B0%8F%E6%96%87%E5%AD%97%E3%81%AE%E4%BD%BF%E3%81%84%E5%88%86%E3%81%91%E3%81%8Crfc430%E3%81%AB%E5%BE%93%E3%81%A3%E3%81%A6%E3%81%84%E3%82%8B-c-case


Cookbook

- https://rust-lang-nursery.github.io/rust-cookbook/intro.html

## 必須ツール

- rls(LSP)
- clippy
- rustfmt

https://www.rust-lang.org/tools

## 代表的なプロダクト

- https://github.com/rust-lang/cargo
- https://github.com/BurntSushi/ripgrep
- https://github.com/jwilm/alacritty
- https://github.com/ogham/exa
- https://github.com/sharkdp/fd
- https://github.com/sharkdp/bat
- https://github.com/xi-editor/xi-editor

## 基本

### 文字列

#### formatの指定の仕方

https://qiita.com/YusukeHosonuma/items/13142ab1518ccab425f4

#### formatでn文字ぴったりにする

7文字制限だと

``` rust
format!("{:0>7.7}", string);
```

### 関数の引数の入出力

入力は`&str`(がんばるなら<S: Into<String>>(s: S)がいい)、出力は`String`がよい気がする

### 型変換

https://qiita.com/legokichi/items/0f1c592d46a9aaf9a0ea

### リファクタリング

https://doc.rust-jp.rs/book/second-edition/ch12-03-improving-error-handling-and-modularity.html

### ファイル分割

https://keens.github.io/blog/2018/12/08/rustnomoju_runotsukaikata_2018_editionhan/

### OptionとResultの使い方

https://qiita.com/tatsuya6502/items/cd41599291e2e5f38a4a

### イテレータ

https://qiita.com/lo48576/items/34887794c146042aebf1

### 非同期プログラミング

https://tech-blog.optim.co.jp/entry/2019/11/08/163000#Rust%E3%81%AE%E9%9D%9E%E5%90%8C%E6%9C%9F%E3%83%97%E3%83%AD%E3%82%B0%E3%83%A9%E3%83%9F%E3%83%B3%E3%82%B0%E3%81%AE%E7%89%B9%E5%BE%B4

rust-jp slackのtatsuya6502さんのコメントより

> スケジューラ（または実行モデル）の分類について、上のOPTiMさんの記事ではM:Nモデルを「スレッドプール」と「ワークスティーリング」の2つに分けています。しかし実際にはスレッドプールと呼んでいるものがさらに2つに分かれて、しかも性能特性が逆なので少し注意が必要です。
> https://tokio.rs/blog/2019-10-scheduler/ の記事の解説に合わせて3つに分類すると以下のようになります。（記事の図を見ると違いがわかりやすいです）
> 1. One queue, many processors
>   - 単一の実行キューを複数プロセッサで共有するタイプ
>   - 全てのタスクが公平にスケジューリングされる
>   - 実行キューが常に複数プロセッサ（複数のOSスレッド）からアクセスされるため、タスク切り替えのオーバーヘッドが高い。（非同期タスクではタスク切り替えの頻度が非常に高く、性能への影響が大きい）
>   - 実装がシンプル（ランタイムがバグりにくい）
> 2. Many processors, each with their own run queue
>   - 個々のプロセッサがそれ専用の実行キューを持つタイプ
>   - タスクのスケジューリングにばらつきがでる（不公平）
>   - 実行キューが単一のプロセッサ（OSスレッド）からしかアクセスされないため、タスク切り替えのオーバーヘッドが低い
>   - 実装がシンプル（ランタイムがバグりにくい）
> 3. Work-stealing
>   - 性能は2番に近く、2番の欠点である不公平なスケジューリングの解決を図ったもの
>   - 個々のプロセッサがそれ専用の実行キューを持つが、プロセッサが暇になると他のプロセッサの実行キューからタスクを奪うタイプ
>   - 全てのタスクがほぼ公平にスケジューリングされる
>   - ほとんどの場合、実行キューが単一のプロセッサ（OSスレッド）からアクセスされるため、タスク切り替えのオーバーヘッドが低い
>   - 実装が複雑（ランタイムがバグりやすい）



### テストコード

単体テストコードはソースコードの中に書く。modでくくる

``` rust
#[cfg(test)]
mod tests {
    use super::add_two;

    #[test]
    fn it_works() {
        assert_eq!(4, add_two(2));
    }
}
```

結合テストは`tests/`ディレクトリに格納する

#### テストデータの置き場

- rlsはtests/fixtures/に置いてる模様
- ripgrepはtests/data
- testdata派もいるみたい

## Cargo

### Cargoで使える環境変数

- RUSTUP_HOME: ~/.rustup
- CARGO_HOME: ~/.cargo
- OUT_DIR: ./target/debug/build/xxx-hashyyy/out

https://doc.rust-lang.org/cargo/reference/environment-variables.html

## テスト

### Tips

#### FixtureTest

FixtureTestは2019/07/12現在サポートされていない。

https://internals.rust-lang.org/t/pre-rfc-lightweight-test-fixtures/6605


## デバッグ

### lldb

#### 型を表示する

`type lookup xxx`

## 仕様

### 借用と参照と関数の引数の関係

https://qiita.com/yz2cm/items/9a8337c368cf055b4255#%E3%81%BE%E3%81%A8%E3%82%81%E3%82%8B%E3%81%A8

### 後置のif

ない模様

https://github.com/rust-lang/rfcs/issues/1362

## CI

### CIでやること

https://github.com/sagiegurari/cargo-make

## よく使う構文

### 現在時刻取得

`Utc::now().naive_utc()`

### static変数

```
let x: &'static str = "Hello, world.";
static Foo: i32 = 5;
let x: &'static i32 = &FOO;
```

## よく使うcrate

### clap

引数処理

#### 特定の値だけを許容してenumに突っ込みたい

possible_valuesを使ってfrom_strで変換するのがよさげ

https://yuk1tyd.hatenablog.com/entry/2018/03/21/225533

## 並列/並行処理

### スレッド

https://totem3.hatenablog.jp/entry/2017/05/10/210000


## Tips

### OsStringからfailtureに変換する

the trait `std::error::Error` is not implemented for `std::ffi::OsString`

``` rust
pathbuf.
    .into_os_string()
    .into_string()
    .map_err(|x| format_err!("Home env not found {:?}", x))?)
```


### boolをResult型に変換する

 boolinatorを使うのがよさそう

https://stackoverflow.com/questions/54841351/how-do-i-idiomatically-convert-a-bool-to-an-option-or-result-in-rust

### 文字列から改行を削除する

```rust
fn trim_newline(s: &mut String) {
    while s.ends_with('\n') || s.ends_with('\r') {
        s.pop();
    }
}
```

```rust
let len = input.trim_end_matches(&['\r', '\n'][..]).len();
input.truncate(len);
```

https://blog.v-gar.de/2019/04/rust-remove-trailing-newline-after-input/

### 関数の引数でたくさんの型を許容したい

- &Tとmut &Tを受け付けたい: Borrowを使う 
https://doc.rust-jp.rs/the-rust-programming-language-ja/1.6/book/borrow-and-asref.html
- Pathっぽいものを受け付けたい：AsRef<Path>を使う
- &strとStringを受け入れたい： Into<String> or AsRef<str>
https://exoskeleton.dev/proglang/rust/rust-8.md#String%E3%81%A8Into%3CString%3E


### 非同期で関数をループ実行

``` rust
fn run_async<P: AsRef<Path>>(
    input_files: &Vec<P>,
) -> Result<Vec<PathBuf>, Error> {
    let v = Arc::new(Mutex::new(vec![]));
    thread::scope(|s| -> Result<(), Error> {
        for input_file_ in input_files {
            let input_file = input_file_.as_ref().clone();
            let v = Arc::clone(&v);
            s.spawn(move |_| -> Result<(), Error> {
                let output_file = format!(
                    "out_{}",
                    input_file
                        .file_stem()
                        .ok_or(format_err!("Could not convert to filename"))?
                        .to_str()
                        .ok_or(format_err!("Could not convert to string"))?
                );
                let mut v1 = v.lock().unwrap();
                match run_app(&input_file, &output_file) {
                    Ok(p) => v1.push(p),
                    Err(e) => warn!(
                        "Error Occured!: {}. Skip: {}",
                        e.to_string(),
                        &output_file
                    ),
                }
                Ok(())
            });
            std::thread::sleep(std::time::Duration::from_secs(0));
        }
        Ok(())
    })
    .map_err(|_| format_err!("Thread execution error"))?
    .map_err(|e| format_err!("Thread execution error: {}", e))?;

    let ret = v
        .lock()
        .map_err(|e| format_err!("lock error: {}", e))?
        .to_vec();
    Ok(ret)
}
```

### std::threadで? operatorを使いたい

fast returnしたいときのやり方。closureに戻り値を書く

https://stackoverflow.com/questions/56535634/propagating-errors-from-within-a-closure-in-a-thread-in-rust

### ジェネリクスでの型を指定する

turbofish　::<>を使う

``` rust
let _a = AAA::<i64>(0);
```

### 最適化したい

rustcにオプションをつける
`-C opt-level=3 -C debug_assertions=no`

### lines()でreferenceが使えない

by_ref()を使う

https://stackoverflow.com/questions/39935158/cannot-use-moved-bufreader-after-for-loop-with-bufreader-lines

### unit testでプロジェクトルートを取得する

``` rust
let mut project_root = PathBuf::from(env!("CARGO_MANIFEST_DIR"));
```

### build.rsでprintlnする

`cargo build --verbose -vv`で表示できる。表示されないときはファイルを一部更新してもう一度実行してみる。


### リポジトリが変更されたら強制的にbuild.rsを呼び出すようにする方法

https://stackoverflow.com/questions/49077147/how-can-i-force-build-rs-to-run-again-without-cleaning-my-whole-project


### エラーメッセージでNo such file or directoryがどのファイルかわからない

std::fsのエラーメッセージは貧弱なのでex(https://docs.rs/ex/0.1.3/ex/)を使うとよい模様

https://www.reddit.com/r/rust/comments/bsn0zs/get_which_filename_is_not_found_on_rust_file/

### mainで早期リターン

1.26からはmainで?が使えるのでそれを使ったほうがいい。

https://blog.rust-lang.org/2018/05/10/Rust-1.26.html

``` rust
use std::fs;

fn main() -> Result<(), std::io::Error> {
    let _ = fs::File::create("bar.txt");
    let _ = if let Ok(val) = fs::File::open("bar.txt") { val } else { println!("aaaa"); return Ok(())};
    println!("bbbb");
    Ok(())
}
```

https://www.reddit.com/r/rust/comments/6c3gjr/early_return_and_if_let/

### ワイルドカード(glob)でファイルを消す

``` rust
let _ = glob(glob_pattern)
    .expect("Failed to read glob pattern")
    .for_each(|path| fs::remove_file(path.as_ref().unwrap()).unwrap());
```

### 文字列で0パディングする

https://stackoverflow.com/questions/50458144/what-is-the-easiest-way-to-pad-a-string-with-0-to-the-left

### ファイルの更新日時が最新のものを取得する

``` rust
let new_generated = glob(&*glob_pattern)
    .expect("Failed to read glob pattern")
    .max_by_key(|path| {
        fs::metadata(path.as_ref().unwrap())
            .unwrap()
            .modified()
            .unwrap()
    })
    .unwrap()?;
```

### StringをErrorに変換したい(the trait bound `&str: std::error::Error` is not satisfied)

format_err!()マクロを使う

### OptionalをResultに変換したい

ok_orを使う

### info!,debug!をユニットテストでも出力させたい

env_loggerを使って、
```
env::set_var("RUST_LOG", "info");
let _ = env_logger::builder().is_test(true).try_init();
```
をsetupかなにかで毎回実行させる


### coverageを表示したい

``` bash
cat target/coverage/kcov-merged/coverage.json | jq -r '[.percent_covered, .covered_lines, .total_lines] | @sh' | tr -d \' | awk '{ printf "coverage: %s%% (%d / %d)\n", $1, $2, $3 }'
```

cargo makeなら

``` toml
[tasks.coverage-show]
script = [
'''
#!/usr/bin/env bash
cat target/coverage/kcov-merged/coverage.json | jq -r '[.percent_covered, .covered_lines, .total_lines] | @sh' | tr -d \' | awk '{ printf "coverage: %s%% (%d / %d)\n", $1, $2, $3 }'
'''
]
```


### serdeで項目名を変更したい

``` rust
#[serde(rename = "pos[0]")]
pos_0: Option<f64>,
```

をフィールドにつけてリネームする

https://serde.rs/container-attrs.html

### serdeでシリアライズとデシリアライズの名称を変えたい

```rust
struct Joeybloggs {
    #[serde(rename(deserialize = "old", serialize = "new"))]
    field: i32,
}
```

https://github.com/serde-rs/serde/issues/1320

### テストデータの置き場をプロジェクトルートからの相対パスで取得する方法

``` rust
let mut d = PathBuf::from(env!("CARGO_MANIFEST_DIR"));
d.push("resources/test");
```

https://stackoverflow.com/questions/30003921/locating-resources-for-testing-with-cargo


## TroubleShooting

### テストを書いたのに実行されない

{module-name}.rs or mod.rsにpub mod xxxの記述がないため実行ファイルからテストが呼び出せないことが原因。追加すること。



## 参考サイト

http://imoz.jp/note/rust-functions.html

