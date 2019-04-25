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

### リファクタリング

https://doc.rust-jp.rs/book/second-edition/ch12-03-improving-error-handling-and-modularity.html

### ファイル分割

https://keens.github.io/blog/2018/12/08/rustnomoju_runotsukaikata_2018_editionhan/

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

## CI

### CIでやること

https://github.com/sagiegurari/cargo-make



## Tips

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

