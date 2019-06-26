# Rust

## 公式ドキュメント

これが一番参考になるのでぐぐる必要はない

- https://doc.rust-jp.rs/book/second-edition/
- https://doc.rust-jp.rs/rust-by-example-ja/
- https://doc.rust-lang.org/beta/std/index.html
- https://sinkuu.github.io/api-guidelines/

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


### リファクタリング

https://doc.rust-jp.rs/book/second-edition/ch12-03-improving-error-handling-and-modularity.html

### ファイル分割

https://keens.github.io/blog/2018/12/08/rustnomoju_runotsukaikata_2018_editionhan/

### OptionとResultの使い方

https://qiita.com/tatsuya6502/items/cd41599291e2e5f38a4a

### イテレータ

https://qiita.com/lo48576/items/34887794c146042aebf1


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


## よく使うcrate

### clap

引数処理

#### 特定の値だけを許容してenumに突っ込みたい

possible_valuesを使ってfrom_strで変換するのがよさげ

https://yuk1tyd.hatenablog.com/entry/2018/03/21/225533

## Tips

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

