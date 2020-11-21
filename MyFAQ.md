# MyFAQ

## docker-compose up -dしてもportが既に抑えられている場合
```
sudo lsof -i:5432 # port5432の用途を調べる。必要ないならサービスを止めるかkillする。
brew services stop postgresql # macosでpostgresを止める。
```
