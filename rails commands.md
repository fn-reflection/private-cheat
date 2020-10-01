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

| Description                                        | Method                                                       |
| -------------------------------------------------- | ------------------------------------------------------------ |
| ブラウザでのスナップショット                       | save_and_open_page                                           |
| ブラウザで表示する(chrome)                         | Chromeのheadlessオプション(spec/rails_helper.rb)をOFF<br>binding.pry |
| describe                                           | feature                                                      |
| it                                                 | scenario                                                     |
| let[!]                                             | given[!]                                                     |
| before                                             | background                                                   |
| ページへ移動                                       | visit path                                                   |
| submitと書かれたボタンを押す                       | click_button 'submit'                                        |
| nextと書かれたアンカーを押す                       | click_link 'next'                                            |
| button\|anchorと書かれたボタンまたはアンカーを押す | click_on 'button \| anchor'                                  |
| Aで指定できる入力欄にBを書く                       | fill_in 'A', with: 'B'                                       |
| Bで指定できるselectからAを選択する                 | select 'A', from: 'B'                                        |
| activeで指定できるチェックボックスにチェック       | check 'active'                                               |
| activeで指定できるチェックボックスにチェックをとる | uncheck 'active'                                             |
| ラジオボックスからAを選ぶ                          | choose 'A'                                                   |
| #valueにtokenをセット                              | find('#value').set('token')                                  |
| #hidden_value(visible:false)にtokenをセット        | find('#hidden_value', visible: false).set('token')           |
| pageのhtmlに'コンテンツ'という文字列を持つ         | expect(page).to have_content 'コンテンツ'                    |
| pageのh1に'title'という文字列を持つ                | expect(page).to have_selector 'h1', text: 'title'            |
| pageのh1#idに'^title$'でマッチする文字列を持つ     | expect(page).to have_selector 'h1#id', text: '^title$'       |
| submitと書かれたボタンがある                       | expect(page).to have_button 'submit'                         |
| nextと書かれたアンカーがある                       | expect(page).to have_link 'next'                             |
| expect(page).to have_css '.field_with_errors'      | expect(page).to have_css '.errors'                           |

 

