# railsでよく使うコマンド

## バッチタスクをdevelopmentの環境変数で動かす

```sh
bundle exec rails runner -e development Tasks::TaskName.execute
```

