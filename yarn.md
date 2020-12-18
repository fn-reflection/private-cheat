# yarn 

## yarnとは

yarnはnode.jsなどを主としたソフトウェアを作るときにパッケージ(例えば他の人が作成した外部プログラム)を利用する上で便利なツールである。このようなツールはパッケージマネージャーとも呼ばれていて、非常に類似したものとしてはnpmがあるし、他の言語ではpip(python)、bundle(ruby)、maven(java)などが、OSではbrew(mac)やapt,dnf,yum,pacman(linux)などのツールがパッケージマネージャーとして利用されている。

node.jsはjavascriptの実行環境の一つであり、サーバーアプリケーションに使う他、webブラウザで動作するjavascriptを構築するためのツールがあるため、webフロントエンド開発にも使用されることが多い。

所感：npmもyarnも使い方はほとんど変わらないので、迷っている場合はどちらでも良い。

## yarnの基本

package.jsonというファイルをもとに利用するパッケージの名称を指定したりバージョンを(範囲)指定する。yarnを使ってパッケージを取得すると、yarn.lockというファイルが作られそこにパッケージのバージョンの正確な値などが記録される。

パッケージを取得するとnode_modulesというディレクトリに保存され、パッケージ取得をやり直したい場合はnode_modulesディレクトリを消せば元の状態に戻る。

## package.jsonの例

※下の例では<code>//</code>以降に解説を記述するがpackage.jsonは普通のjsonファイルでありこのような記法は許容されないので、実際に利用するときは解説を除去すること

他の人の作ったパッケージを利用するだけならdependencies系が特に重要。パッケージを公開する場合はバージョンやライセンスなども重要になる。

```jsonc
{
  "name": "my-new-project", // このプロジェクトの名前
  "version": "1.0.0", // このプロジェクトのバージョン
  "description": "説明用", // プロジェクトの説明
  "main": "index.js", // エントリーポイント
  "repository": { // このプロジェクトが管理されているリポジトリ(例えばgithub)
    "url": "https://example.com/your-username/my-new-project",
    "type": "git"
  },
  "author": "Your Name <you@example.com>", // 名前とemail
  "license": "MIT", // このプロジェクトに適用するライセンス
  "scripts": {
    "build": "babel src -d lib", // yarn buildを実行するとbabel src -d libコマンドが実行される
    "test": "jest"　// yarn testを実行するとjestコマンドが実行される
  }
  "dependencies": { // 使用するパッケージリスト
    "vue": "^2.6.12"
  },
  "devDependencies": { // 開発時にのみ使用するパッケージリスト(具体的には環境変数NODE_ENV=productionにセットされた場合などは無視される)
    "vue-test-utils": "^1.0.0-beta.11"
  },
  "peerDependencies": { // このプロジェクトを利用するプロジェクトに使用させるパッケージリスト(例えばeslintやreactなどのプラグインを作成するときなどに使うらしい)
    "eslint": "^2.3.19"
  },
  "optionalDependencies": { // 必須ではないパッケージリスト、取得できなくてもエラーにならない。
    "watchman": "^2.3.19"
  },
  "bundledDependencies": { // このプロジェクトをモジュールとして公開するときにモジュールとして一緒に保持するパッケージリスト
    "vue"
  },
  
}

```

## パッケージハンドリング

```sh
yarn init # yarnプロジェクトのベースになるpackage.jsonを対話形式で作成する
yarn # package.jsonの情報に基づきパッケージをnode_modulesディレクトリに取得する。
yarn install # yarnと同じ
yarn install --check-files # パッケージが確かに取得されていることを確認する。
yarn install --force # node_modulesに強制ダウンロード(これするよりは普通にnode_module消してyarnした方が簡単かも)
yarn install --production # dependenciesのパッケージのみ取得する。(devDependenciesのとかは取得しない。)
yarn install --prod # yarn install --productionと同じ
yarn add package@version #パッケージを緩くバージョン指定してdependenciesに追加(1.2.4なら1.2.4以上2.0.0未満の最新バージョン)
yarn add package@version --exact # パッケージを厳格にバージョン指定してdependenciesに追加
yarn add package # = yarn add package@latest 最新バージョンを追加
yarn add package --dev #　devDependenciesに追加
yarn add package -D #　yarn add package --devと同じ
yarn add package --peer # peerDependenciesに追加
yarn add package --optional # optionalDependenciesに追加
yarn global add package # プロジェクトではなくユーザー環境にパッケージを取得する
yarn global dir # ユーザー環境にパッケージを取得する場合、その保存パスを表示する
yarn upgrade package@version # パッケージを指定したバージョンに更新する
yarn upgrade package # パッケージを最新バージョンに更新する
yarn remove package # パッケージをdependenciesから削除する
yarn why package # パッケージが取得されている理由を検索する
yarn config set cache-folder <path> # パッケージのキャッシュディレクトリを変更する、ebsとかを登録すると捗る？
```

## スクリプトの実行とか

```sh
yarn run # 実行可能なスクリプトコマンドを列挙する
yarn run env # スクリプト実行時の環境変数を列挙する
```

## バージョン指定の記法

```sh
'~3.1.4' # 3.1.4以上 3.2.0未満
'~3.1' # 3.1.0以上 3.2.0未満
'~3' # 3.0.0以上 4.0.0未満
'2.x' # メジャーバージョンが2である任意のバージョン
'3.1.x' # 3.1から始まる任意のバージョン
'=4.6.6' # バージョン 4.6.6 のみ
'=3.1.4-beta.2' # ベータ版も指定できる
'>0.4.2' # 0.4.2 を超える任意のバージョン
'>=2.7.1'	# 2.7.1 以上の任意のバージョン
'<2.0.0' # 2.0.0 未満の任意のバージョン
'<=3.1.4' #	3.1.4 以下の任意のバージョン
">=2.0.0 <3.1.4" #  2.0.0 以上 3.14未満の任意のバージョン
'2.0.0 - 3.1.4' #  2.0.0 以上 3.14以下の任意のバージョン
'*' # 任意のバージョン
```

## yarn vs npm

yarnの公式ドキュメントより抜粋

| npm (v5)                                | Yarn                            |
| :-------------------------------------- | :------------------------------ |
| `npm install`                           | `yarn install`                  |
| ない                                    | `yarn install --flat`           |
| ない                                    | `yarn install --har`            |
| `npm install --no-package-lock`         | `yarn install --no-lockfile`    |
| ない                                    | `yarn install --pure-lockfile`  |
| `npm install [package]`                 | `yarn add [package]`            |
| `npm install [package] --save-dev`      | `yarn add [package] --dev`      |
| ない                                    | `yarn add [package] --peer`     |
| `npm install [package] --save-optional` | `yarn add [package] --optional` |
| `npm install [package] --save-exact`    | `yarn add [package] --exact`    |
| ない                                    | `yarn add [package] --tilde`    |
| `npm install [package] --global`        | `yarn global add [package]`     |
| `npm update --global`                   | `yarn global upgrade`           |
| `npm rebuild`                           | `yarn install --force`          |
| `npm uninstall [package]`               | `yarn remove [package]`         |
| `npm cache clean`                       | `yarn cache clean [package]`    |
| `rm -rf node_modules && npm install`    | `yarn upgrade`                  |
