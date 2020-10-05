

# よく使うスニペット

| Description                                 | Command or method                                            |
| ------------------------------------------- | ------------------------------------------------------------ |
| タイムゾーン指定してrails動かす             | TZ="JST-9" bundle exec rails s                               |
| delayedjobを動かす                          | bundle exec rake jobs:work                                   |
| ポート3001で別サーバーを立ち上げる          | TZ="JST-9"; bin/rails s -p 3001                              |
| ruby godを使う                              | bundle exec god -c config/scoring.god                        |
| ruby godを止める                            | bundle exec god stop                                         |
| バッチタスクをdevelopmentの環境変数で動かす | bundle exec rails runner -e development Tasks::TaskName.execute |
| マイグレーション                            | bundle exec rails generate migration AddRequiredColumnToUsers |
| Rollbarを差し込む                           | Rollbar.info('deprecated action `UsersController#question` called') |
| ブランチ名のprefixのリスト                  | feat fix refact test misc                                    |
| コミット名に使うワードリスト                | feat fix refact test docs style perf chore [ci skip]         |
| DBシードの読み込み                          | bundle exec rails db:seeds                                   |
| デバッガーを仕込む                          | binding.pry                                                  |
|                                             |                                                              |
## feature spec関連

- featurespecのシンタックスシュガー：feature=describe, scenario=it, given[!]=let[!], background=before
- labelの他にid、name、placeholderを指定子としてDOMがセレクトできる。

| Description                                                  | Method                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 高速テスト開発？                                             | <code>RAILS_ENV=test bundle exec rails s -p 3002<br>Capybara.run_server = false <br> Capybara.app_host = 'http://localhost:3002'</code> |
| ブラウザでのスナップショット                                 | save_and_open_page                                           |
| ブラウザで表示する(chrome)                                   | Chromeのheadlessオプション(spec/rails_helper.rb)をOFF<br>scenario 'A' js: true #(chromeを有効化)<br/>binding.pry |
| describe                                                     | feature                                                      |
| it                                                           | scenario                                                     |
| let[!]                                                       | given[!]                                                     |
| before                                                       | background                                                   |
| ページへ移動                                                 | visit path                                                   |
| submitと書かれたボタンを押す                                 | click_button 'submit'                                        |
| .your_selectクラスを持つDOMは以下の'はい'と書かれたボタンを押す | within '.your_select' {choose 'はい' }                       |
| nextと書かれたアンカーを押す                                 | click_link 'next'                                            |
| button\|anchorと書かれたボタンまたはアンカーを押す           | click_on 'button \| anchor'                                  |
| Aで指定できる入力欄にBを書く                                 | fill_in 'A', with: 'B'                                       |
| Bで指定できるselectからAを選択する                           | select 'A', from: 'B'                                        |
| activeで指定できるチェックボックスにチェック                 | check 'active'                                               |
| activeで指定できるチェックボックスにチェックをとる           | uncheck 'active'                                             |
| ラジオボックスからAを選ぶ                                    | choose 'A'                                                   |
| #valueにtokenをセット                                        | find('#value').set('token')                                  |
| #hidden_value(visible:false)にtokenをセット                  | find('#hidden_value', visible: false).set('token')           |
| pageのhtmlに'コンテンツ'という文字列を持つ                   | expect(page).to have_content 'コンテンツ'                    |
| pageのh1に'title'という文字列を持つ                          | expect(page).to have_selector 'h1', text: 'title'            |
| pageのh1#idに'^title$'でマッチする文字列を持つ               | expect(page).to have_selector 'h1#id', text: '^title$'       |
| submitと書かれたボタンがある                                 | expect(page).to have_button 'submit'                         |
| nextと書かれたアンカーがある                                 | expect(page).to have_link 'next'                             |
| '.errors'クラスを持つDOM要素がある                           | expect(page).to have_css '.errors'                           |
|                                                              |                                                              |

## headless-chromeのremote-debugging-port機能を使ったデバッグ

この方法は色々と試してみたがあまり推奨できない。基本的な手順と推奨できない理由(振る舞い)について述べる。

### remote-debugging-portの有効化

```ruby
options.add_argument('--remote-debugging-port=9222') #port 9222の利用が推奨されている
```

localhost:9222からアクセスできるようになる。

Circleciの場合は以下のような感じでsshトンネルを使うことでlocalhost:9222からアクセスできる。

```sh
ssh -L 9222:localhost:9222 -p CIRCLECI_PORT CIRCLECI_IPADDRESS
```

### windowサイズの変更

結果が見辛い時はこれで設定し直す。

 ```ruby
Capybara.current_session.driver.browser.manage.window.resize_to(1280, 720)
 ```

### 推奨できない理由

localhost:9222から特定のウィンドウ画面に一度アクセスしてしまうと、binding.pry(のnとか)が正常に動作しなくなる。制御をとってしまっているような振る舞いになるためbinding.pryでサーバーを制御しながら、動作の振る舞いを確認するということがうまくできない？

要調査：chromeのremote-debuggingはwebsocketで実現されているようで、一度アクセスするとwebsocketのコネクションが立ち上がるのが原因っぽい？コネクションのclose処理とかで色々つまづいてるような振る舞いを示す。

### セッションが残留する場合とその対処

rspecを終了させようとしてもなかなか終了せず強制終了させるしかなくなることがある。

rspecを強制終了するとteardown処理が正常に動作せずセッション(とheadless chromeプロセス)が残る。その結果port9222を残留したプロセスが持ち続けるため、新しくテストを始めてもport 9222が利用できなくなる。(この場合はps auxしてchromeのプロセスIDを調べkillすることで対処可能ではある。

## VNCを使ったリモートデバッグ

### 参考文献(基本的にこれの焼き直しです。)

https://circleci.com/docs/ja/2.0/browser-testing/#vnc-%E3%81%8B%E3%82%89%E3%81%AE%E3%83%96%E3%83%A9%E3%82%A6%E3%82%B6%E3%83%BC%E6%93%8D%E4%BD%9C

### circleciサーバーへの接続と準備

circleciの"Rerun job with ssh"を使って、circleciのサーバーのポートとIPアドレスを調べる。

下記でcircleciのvncサーバーのポートをclientのlocalhost:5902にバインドする。(なお5901はサーバーのdisplay番号1に対応しており。display番号10の描画状態をみたい場合は5910に置き換える。)

```sh
ssh -p CIRCLECI_PORT CIRCLECI_IPADDRESS -L 5902:localhost:5901
```

サーバー側にVNCサーバーとテキストエディタなどをインストールし、VNCサーバーを起動する。

```sh
sudo apt update
sudo apt install vnc4server metacity vim
vncserver -geometry 1280x1024 -depth 24
```

Display番号を1に設定する。(port 5901に対応)

```sh
export DISPLAY=:1.0
```

この段階でクライアント(mac)のvnc viewerからlocalhost:5902にアクセスすると見えるはず。(firefoxとか立ち上げてみてください。)

vimでrails_helperを立ち上げ、headlessオプションを一時的にコメントアウトする。

```sh
vim spec/rails_helper.rb
```

```ruby
# options.add_argument('headless') # ヘッドレスモードをonにするオプション
```

binding.pryを差し込んだりしてテストの挙動をデバッグしてください。(ただしjs:trueにしないとchromeは立ち上がらないので注意)

