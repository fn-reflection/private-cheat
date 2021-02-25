# Rust

あんまりまとまっていないです。個人的には所有権とかマーカートレイトとかトレイトオブジェクトとかそれら×非同期があんまりわかっていない。マクロも設計がよくわからない。そのほかはthe bookとrustnomiconをザックリ読めばわかるはず。言いたいことはわかるが、確実な知識になってない奴 Pin Sync Send dyn Sized? 

他の言語を経験していないとasyncだとか、ムーブセマンティクスとかトレイトとかイテレータとか挫折ポイント多め。型設計は有力ライブラリや拡張を読みながら勉強することにする。

## 学習リソース

[the book](https://doc.rust-jp.rs/book-ja/) (非常に読みやすい、完読)

[rust by examples](https://doc.rust-jp.rs/rust-by-example-ja/) (こっちの方が入りやすそう)

[rustnomicon](https://doc.rust-jp.rs/rust-nomicon-ja/) (たまにみる)

[rust api guidelines](https://sinkuu.github.io/api-guidelines/) (読んでないけど重要そう)

[async book](https://async-book-ja.netlify.app/01_getting_started/01_chapter.html]) (非同期)

[macro book](https://danielkeep.github.io/tlborm/book/README.html) (マクロ)

## 学習リソース2

https://qiita.com/k5n/items/758111b12740600cc58f

https://qiita.com/nirasan/items/cf05ac6d5a1ae17ae36f







rustupの基本設定

```sh
~/.rustup # ツールチェインやメタデータのインストール先　RUSTUP_HOMEで変更できる
~/.cargo # cargoのホームディレクトリ CARGO_HOMEで変更できる
~/.cargo/bin # cargo, rustc, rustupのバイナリ保存先
```

## rustup
```sh
rustup update # 最新版へ移行
rustup self uninstall # アンインストール
```

## rustc
```sh
rustc source_file # コンパイル
rustc --explain 308 # E308エラーの詳細ドキュメントをみる
```

## cargo

```sh
cargo new hello_cargo --bin #プロジェクトの作成 --bin:実行可能ファイル、ライブラリではない
--vcs # vcsの指定 git, hg, pijul, fossil, none
--lib # ライブラリの作成
--name # ライブラリの名前(指定しないとディレクトリ名)
cargo build # ビルドする(デバッグビルド、最適化なし)
cargo build --release # ビルドする(リリースビルド、最適化あり)
cargo run # cargo build＋実行する
cargo check # コンパイルできるか確認する
cargo update # Cargo.tomlに指定したバージョニングに基づいてクレートをアップデートする
cargo doc --open # ローカルにある依存のドキュメントをビルドしブラウザで見せる
cargo test # テストを実行
-- --test-threads=1 # 1スレッドで実行
-- --nocapture # テスト成功時も標準出力をキャプチャしない。
cargo test one_hundred # 特定のテストのみ実行
cargo test abc # abcを名前に含むテストのみ実行
cargo test module_name # module_nameを名前に含むテストのみ実行 
cargo test -- --ignored # [ignored]が定義されているテストのみ実行
cargo test --test integration_test # integration_testというファイルのテストのみ実行

```

## rustの開発ツール

```sh
rustfmt # フォーマッタ
rust-analyzer # vscodeのプラグイン 標準rlsより強力 
clippy # linter あまり使ってない
lldb # Codelldb (vscode) デバッガ
rust-lldb ./target/debug/[project] # debugプロファイルの実行ファイルをデバッグ
```

## rust-lldbのコマンド

```sh
br set -f main.rs -l 13 # ブレイクポイントを行数指定
c # continue
n # next
po [variable] # print object
```

## rustのモジュールシステム

|                          |                                                              |
| ------------------------ | ------------------------------------------------------------ |
| パス                     | 要素（例えば構造体や関数やモジュール）に名前をつける         |
| 絶対パス                 | クレート名 or crateという文字列がルートパスになる            |
| 相対パス                 | self, super, モジュール内識別子を使い相対パスを作れる。      |
| モジュール               | パスの構成、スコープ、公開制御を決める                       |
| 変数名・関数名の命名規則 | ライブラリクレートとバイナリ(実行可能)クレートがある<br>木構造をしたモジュール群 |
| クレートルート           | コンパイラの開始点<br>クレートのルートモジュールを作るソース<br>バイナリクレートルートのデフォルト：src/main.rs<br>ライブラリクレートルートのデフォルト：src/lib.rs<br>バイナリクレートが複数ある場合のデフォルト：src/bin |
| パッケージ               | クレートをビルド・テスト・共有できるCargoの機能<br>パッケージはCargo.tomlを持つ<br>クレートを1つ以上持つ<br>ライブラリクレートを0または1個持つ(それ以上は不可) |
|                          |                                                              |



## rustの慣習

|                          |                                        |
| ------------------------ | -------------------------------------- |
| println!の!は何か        | マクロを表している(つまり関数ではない) |
| use無しで使える型は？    | std:preludeに存在するいくつかの型のみ  |
| 変数名・関数名の命名規則 | スネークケース(var_name, func_name)    |
|                          |                                        |

## # rustのデータ型

下記表の型はスタック領域に格納される。

|              |                                                              |
| ------------ | ------------------------------------------------------------ |
| 符号あり整数 | I8, i16, i32(これが基準型・最速), i64, isize(アーキテクチャに依存) |
| 符号なし整数 | u8, u16, u32, u64, usize                                     |
| 浮動小数点数 | f32, f64(これが基準型)                                       |
| 論理値型     | bool(trueまたはfalse)                                        |
| 文字型       | char(charのリテラルは''で囲う、ユニコード)                   |
| タプル       | (I32, f64, u8) のように宣言する                              |
| 配列         | 同一型・伸縮しない・スタック領域に確保される                 |

##  rustの構文

```rust
use std::io // std::ioをioとして呼び出せる
let mut guess = String::new(); // ミュータブルなString型変数guess 型は右辺から推定される
String::new() // ::は関連関数(いわゆる静的メソッド)の参照に用いる
std::io::stdin() // std::io::Stdinオブジェクトを返す関数
std::io::stdin().read_line(&mut guess) // stdinの入力をguessに入れてio::Resultを返す
&x // xのimmutableな参照
&mut x // xのmutableな参照
io::Result // ioに特化したResult型、enumでありOKやerrなど
io::Result.expect("read_line ERROR") // io.ResultがErrなら(OSエラーが原因とみなして)引数に渡したメッセージを表示してクラッシュする、OKの場合バイト値を返す。.expectを指定しないとResult値を使用していないことを示すエラーが出る。エラーハンドリングが正しいが、ここではクラッシュさせる。
println!("x = {} and y = {}", x, y); // x,yを{}に入れて表示する。
use rand::Rng // Rngトレイトは乱数生成器の実装するメソッドを定義していて、トレイとがスコープにないとメソッドを使用できない
rand::thread_rng().gen_range(1, 101); // thread_rng():スレッドローカルな乱数生成器を返す
match guess.cmp(&secret_number) {
    Ordering::Less => println!("Too small!"),
    Ordering::Greater => println!("Too big!"),
    Ordering::Equal => println!("You win!"),
} // match式 Orderingはenum型でパターンマッチングが網羅されている

let guess: u32 = match guess.trim().parse() {
    Ok(num) => num,
    Err(_) => continue,
}; // Err(_):_は包括値、つまり失敗した場合どんな入力だったとしてもcontinue

mod front_of_house {
    mod hosting {
        fn add_to_waitlist() {}
        fn seat_at_table() {}
    }

    mod serving {
        fn take_order() {}
        fn serve_order() {}
        fn take_payment() {}
    }
}
// module構造
// crate - front_of_house - hosting - add_to_waitlist
//                                  - seat_at_table
//                        - serving - take_order
//                                  - serve_order
//                                  - take_payment
```

スマートポインタは、`Deref`と`Drop`トレイトを実装している。

スマートポインタは普通、構造体を使用して実装されています。

- ヒープに値を確保する`Box<T>`
- 複数の所有権を可能にする参照カウント型の`Rc<T>`
- `RefCell<T>`を通してアクセスされ、コンパイル時ではなく実行時に借用規則を強制する型の`Ref<T>`と`RefMut<T>`

便利スニペット

```rust
pub fn print_typeof<T>(_: T) { println!("{}", std::any::type_name::<T>());}
```

##  エラーハンドリング

ユーザーによるカスタム定義型にはError (std;:error::Error)トレイトを実装する必要がある。thiserrorにはderiveマクロとかが定義されているのでこれを使う。

関数の引数に具体的な型を指定すると、他のエラー型を?で返すことはできない。汎用性のあるエラー型、およびちゃんとエラーコンテキストを処理するためにanyhowを使う。

```rust
// 汎用性ある返り値定義 anyhow::ErrorでもOK
fn hoge(filename: &str) -> Result<(), Box<dyn Error + Send + Sync + 'static>> { }
return Err(anyhow!("Missing attribute: {}", missing)); // anyhow::Errorを簡単に作るマクロ
ensure!(user == 0, "only user 0 is allowed");　// 条件を満たさなければエラーを早期リターン、いい。
// let a = val?; のセマンティクス
let a = match val {
    Ok(v) => v,
    Err(e) => return Err(From::from(e))
}
```

## 非同期処理

- 実行ランタイムが必要
- OSのプリエンプションがないのでコンテキストスイッチが安価
- blockingな処理はランタイム全体のパフォーマンスを下げるので、例えばtokioではblocking_spawn!を使い明示的にブロッキングスレッド送りにする。async_stdだと検出機能がランタイムに入っている。

## rust macrobookの簡略化

fibonacciの実装が必要以上にゴテゴテしてるので削り取る

```
use std::mem::swap;
#[derive(Debug)]
struct Recurrence { mem: [u64; 2], pos: usize }
impl Iterator for Recurrence {
    type Item = u64;
    fn next(&mut self) -> Option<u64> {
        if self.pos < 2 {
            let next = self.mem[self.pos];
            self.pos += 1;
            Some(next)
        } else {
            let next_val = &self.mem[1] + &self.mem[0];
            let mut swap_tmp = next_val;
            for i in (0..2).rev() { swap(&mut swap_tmp, &mut self.mem[i]); }
            Some(next_val)
        }
    }
}
fn main() {
    let fib = Recurrence { mem: [0, 1], pos: 0 };
    for e in fib.take(10) { println!("{}", e) }
}
```

