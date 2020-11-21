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
