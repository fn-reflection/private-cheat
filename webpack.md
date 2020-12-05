# webpack

## webpackとは

Webpackはnode.jsなどを主としたソフトウェアを作るときに使う1種のコンパイラのようなものである。主目的は、小分けにしたプログラムファイル(モジュール)をまとめて(バンドルして)一つの大きなプログラムに統合(ビルド)することで、このようなツールをモジュールバンドラと呼ぶこともある。

その他の重要な目的として、言語を変換して出力する機能がある。ブラウザは基本的にhtml,css,javascriptしか理解できないが、scss(cssに機能を追加した言語)やtypescript(javascriptに機能を追加した言語)などを開発段階で利用したい場合がある。このときwebpackがバンドル中に言語を変換(トランスパイル)しブラウザが解釈できる形式で出力してくれるため使われる。InternetExplorer11に対応するために最新のjavascriptから昔のjavascriptに変換することもありbabelというトランスパイラはよく使われてきた。

またバンドル時にファイルサイズを小さくする(minify)機能もあり、これによりネットワーク負荷の軽減や読み込み遅延の低減などの最適化を自動化することができる。

こういった複雑なツールを調べるときは、必ず公式ドキュメント(https://webpack.js.org/concepts/)を参照する。ここでは適当にかいつまんで説明する。

## webpackで用いる用語

| 用語               | 意味                                                         |
| ------------------ | ------------------------------------------------------------ |
| エントリーポイント | 使用しているモジュールをwebpackがリストアップするためのスタート地点<br>(プログラムの依存関係はDAGというグラフ構造で記述でき、その例においてはルートノードに相当する)<br>プログラムの実行文の開始地点(main関数)をエントリーポイントと呼ぶことがあるが、これとは異なる概念ということに注意。<br>webpackのエントリーポイントは複数あっても問題ない。その場合はチャンクが作られる。アプリケーションコードとサードパーティIvendor)のコードを分割するなどが考えられる。<br>optimization.splitChunksオプションで制御できる。 |
| ローダー           | ローダーはtypescriptやcssなど.jsや.json以外の形式のファイルをモジュールに変換して利用できるようにする。<br>(なおimport cssはwebpack特有の記法である。)<br>configではtestプロパティでローダーを適用するファイルを正規表現(例えば/\\.txt$/)で抽出し、useプロパティで使うローダーを指定する<br>ローダーの評価順序は右から左、下から上なので、例えばscssローダーはcssのローダーの下にかく。 |
| チャンク           | モジュールをバンドルする上での塊、複数のエントリーポイントがある場合は複数のチャンクがある。 |
|                    |                                                              |





## webpackの基本

webpack.config.jsにてビルドの手順を記述する。これが難しいとされている。

## 暗黙的な振る舞い

webpackの--configオプションで指定しない限りwebpack.config.jsがコンフィグファイルとして使われる。

コンフィグファイルなどで指定しない限り、src/index.jsをエントリーポイント(依存性解決のスタート地点、依存性グラフのトップノードとも言える)とし、dist/main.jsにバンドルした結果を出力する。

## webpack.config.js

https://webpack.js.org/configuration/ ここにある奴を和訳し要約した。高度なオプションは原典を見るべし

```javascript
const path = require('path');
module.exports = {
  mode: "production", // "production"|"development"|"none"　デフォルトはproduction
  entry: "./app/entry", // デフォルトは./src バンドルのスタート地点、配列も許可される
  output: {
    path:path.resolve(__dirname, "dist"), // バンドルした結果の出力ディレクトリを絶対パスで指定する
    filename: "[name].js", // ファイルの名前 [name]:entryのチャンク名、[contenthash]:ハッシュなど動的にファイル名を決定することもできる
    publicPath: "/assets/", // string
    // the url to the output directory resolved relative to the HTML page
    library: { // There is also an old syntax for this available (click to show)
      type: "umd", // universal module definition
      // the type of the exported library
      name: "MyLibrary", // string | string[]
      // the name of the exported library

      /* Advanced output.library configuration (click to show) */
    },
    uniqueName: "my-application", // (defaults to package.json "name")
    // unique name for this build to avoid conflicts with other builds in the same HTML
    name: "my-config",
    // name of the configuration, shown in output
    /* Advanced output configuration (click to show) */
    /* Expert output configuration 1 (on own risk) */
    /* Expert output configuration 2 (on own risk) */
  },
  module: {
    // configuration regarding modules
    rules: [
      // rules for modules (configure loaders, parser options, etc.)
      {
        // Conditions:
        test: /\.jsx?$/,
        include: [
          path.resolve(__dirname, "app")
        ],
        exclude: [
          path.resolve(__dirname, "app/demo-files")
        ],
        // these are matching conditions, each accepting a regular expression or string
        // test and include have the same behavior, both must be matched
        // exclude must not be matched (takes preferrence over test and include)
        // Best practices:
        // - Use RegExp only in test and for filename matching
        // - Use arrays of absolute paths in include and exclude to match the full path
        // - Try to avoid exclude and prefer include
        // Each condition can also receive an object with "and", "or" or "not" properties
        // which are an array of conditions.
        issuer: /\.css$/,
        issuer: path.resolve(__dirname, "app"),
        issuer: { and: [ /\.css$/, path.resolve(__dirname, "app") ] },
        issuer: { or: [ /\.css$/, path.resolve(__dirname, "app") ] },
        issuer: { not: [ /\.css$/ ] },
        issuer: [ /\.css$/, path.resolve(__dirname, "app") ], // like "or"
        // conditions for the issuer (the origin of the import)
        /* Advanced conditions (click to show) */

        // Actions:
        loader: "babel-loader",
        // the loader which should be applied, it'll be resolved relative to the context
        options: {
          presets: ["es2015"]
        },
        // options for the loader
        use: [
          // apply multiple loaders and options instead
          "htmllint-loader",
          {
            loader: "html-loader",
            options: {
              // ...
            }
          }
        ]
        type: "javascript/auto",
        // specifies the module type
        /* Advanced actions (click to show) */
      },
      {
        oneOf: [
          // ... (rules)
        ]
        // only use one of these nested rules
      },
      {
        // ... (conditions)
        rules: [
          // ... (rules)
        ]
        // use all of these nested rules (combine with conditions to be useful)
      },
    ], //他にもコンフィグがある
  },
  resolve: { // モジュールの名前解決のコンフィグ(ここで記述されたルールはローダーの名前解決には使用されない)
    modules: ["node_modules",path.resolve(__dirname, "app")], // モジュールをimportするときの基点となるパス(いわゆるクラスパス)
    extensions: [".js", ".json", ".jsx", ".css"], // 使用されている拡張子
    alias: {
      "module": "new-module", // エイリアス 例えばimport 'module'をimport 'new-module'に読み替える
      "only-module$": "new-module", // この場合はonly-moduleがnew-moduleになるが、module配下のパスについては、エイリアシングされない(トップレベルのみがエイリアシングされる)
      "module": path.resolve(__dirname, "app/third/module.js"),
      "module": path.resolve(__dirname, "app/third"),
      [path.resolve(__dirname, "app/module.js")]: path.resolve(__dirname, "app/alternative-module.js"),
    }, //他にもコンフィグがある
  },
  performance: {
    hints: "warning", // ファイルサイズが↓を超えるとwarningを発する
    maxAssetSize: 200000, // アセットの最大サイズ(byte)
    maxEntrypointSize: 400000, // エントリーポイントの最大サイズ(byte)
    assetFilter: function(assetFilename) { // アセットを判別するフィルタ関数
      return assetFilename.endsWith('.css') || assetFilename.endsWith('.js');
    }
  },
  devtool: "source-map", 
    // このキーが無しだとビルド最速で最適化されていてモジュール解決されている(バンドルされたコード) 
    // source-mapだとビルド最遅だが元のコードが残っている
    // eval-cheap-source-mapなどいろいろある cheap=ビルドコストが安くビルドがそこそこ早い
  　// evalは開発段階むけでモジュールをevalで解決する。ビルドやや速いがline no.がずれたりsourcemapがなかったりする。
    // eval[-cheap]-source-mapなどいろいろある cheap=ビルドコストが安くビルドがそこそこ早い
    // source-mapは本番でのデバッグに使えるかもしれないがエラーリポートのみに使用し公開はしないこと
  context: __dirname, // webpackのホームディレクトリを絶対パスで指定 entry module.rulesなどの相対パス解決の基点になる
  target: "web", // web:ブラウザ環境、 node:node.js環境に向けてモジュールを作る。デフォルトはweb
  externals: ["react", /^@angular/],
  // Don't follow/bundle these modules, but request them at runtime from the environment
  externalsType: "var", // (defaults to output.library.type)
  // Type of externals, when not specified inline in externals
  externalsPresets: { /* ... */ },
  // presets of externals
  ignoreWarnings: [/warning/],
  stats: "errors-only",
  stats: {
    // lets you precisely control what bundle information gets displayed
    preset: "errors-only", // A stats preset

    env: true, // include value of --env in the output
    outputPath: true, // include absolute output path in the output
    publicPath: true, // include public path in the output
    assets: true, // show list of assets in output
    entrypoints: true, // show entrypoints list
    chunkGroups: true, // show named chunk group list
    chunks: true, // show list of chunks in output
    modules: true, // show list of modules in output
    children: true　// show stats for child compilations
    logging: true,　// show logging in output
    loggingDebug: /webpack/, // show debug type logging for some loggers
    loggingTrace: true, // show stack traces for warnings and errors in logging output
    warnings: true　// show warnings
    errors: true, // show errors
    errorDetails: true,　// show details for errors
    errorStack: true,　// show internal stack trace for errors
    moduleTrace: true,　// show module trace for errors
    // (why was causing module referenced)
    builtAt: true,　// show timestamp in summary
    errorsCount: true,　// show errors count in summary
    warningsCount: true,　// show warnings count in summary
    timings: true,　// show build timing in summary
    version: true,　// show webpack version in summary
    hash: true, // show build hash in summary
  },
  devServer: {
    proxy: { // proxy URLs to backend development server
      '/api': 'http://localhost:3000'
    },
    contentBase: path.join(__dirname, 'public'), // boolean | string | array, static file location
    compress: true, // enable gzip compression
    historyApiFallback: true, // true for index.html upon 404, object for multiple paths
    hot: true, // hot module replacement. Depends on HotModuleReplacementPlugin
    https: false, // true for self-signed, object for cert authority
    noInfo: true, // only errors & warns on hot reload
    // ...
  },
  experiments: {
    asyncWebAssembly: true, // WebAssembly as async module (Proposal)
    syncWebAssembly: true, // WebAssembly as sync module (deprecated)
    outputModule: true, // Allow to output ESM
    topLevelAwait: true, // Allow to use await on module evaluation (Proposal)
  }
  plugins: [
    // 使用するプラグインを列挙する
  ],
  optimization: {
    chunkIds: "size",　// method of generating ids for chunks
    moduleIds: "size", // method of generating ids for modules
    mangleExports: "size",　// rename export names to shorter names
    minimize: true, // ファイルサイズを最小化するか
    minimizer: [new CssMinimizer(), "..."], // 最小化に使用するモジュールなど
    splitChunks: {
      cacheGroups: {
        "my-name": { test: /\.sass$/, type: "css/mini-extract" }
      },
      fallbackCacheGroup: { /* Advanced (click to show) */ }
    }
  },
```

## modeの詳説

| Mode        | 効果                                                         |
| ----------- | ------------------------------------------------------------ |
| production  | DefinePluginの<code>process.env.NODE_ENV=production</code>にセットしモジュールとチャンクに対する便利な名前を有効にする？<br>devtool : 'eval' // sourcemapが有効になる?<br>performance.hints: false // assetsやentrypointのファイルサイズが指定サイズを超過しても警告しない<br>output.pathinfo:true //バンドル結果にrequire pathの情報を記録する。 |
| Development | DefinePluginの<code>process.env.NODE_ENV=development</code>にセットしモジュールとチャンクと`FlagDependencyUsagePlugin`, `FlagIncludedChunksPlugin`, `ModuleConcatenationPlugin`, `NoEmitOnErrorsPlugin` and `TerserPlugin`に対するmangledな名前を有効にする？ |
| None        | 最適化オプションを使用しない                                 |







## 単一ファイルにまとめる(バンドルする)のはなぜ？

