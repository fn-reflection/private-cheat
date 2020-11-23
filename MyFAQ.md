# MyFAQ

## docker-compose up -dしてもportが既に利用されている場合
```
sudo lsof -i:5432 # port5432の用途を調べる。必要ないならサービスを止めるかkillする。
brew services stop postgresql # 5432を使用していたpostgresを止める。(macosの場合の記述)
```

## dockerの環境を整理する。
```
docker system prune # ダングリングなリソースを消し去るらしい
```

## clickhouse
```
sudo systemctl start clickhouse-server.service # 起動
clickhouse-client -m # --multilineモードで起動
clickhouse-client --query='INSERT INTO table VALUES' < data.txt
clickhouse-client --query='INSERT INTO table FORMAT TabSeparated' < data.tsv
```
