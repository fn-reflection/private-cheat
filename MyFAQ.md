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
clickhouse-server --config-file /etc/clickhouse-server/config.xml # 起動
clickhouse-client -m # --multilineモードで起動
clickhouse-client --query='INSERT INTO table VALUES' < data.txt
clickhouse-client --query='INSERT INTO table FORMAT TabSeparated' < data.tsv
mvn install -DskipTests=true # ビルドしてインストール
mvn package -DskipTests=true # targetファイル下にビルド
SELECT * from jdbc('jdbc:postgresql://10.10.10.11:5432/m?user= &password= ','public','board')
```
## JDBC driverが見つからない場合、pom.xmlを編集してdependency追加。
```
<!-- https://mvnrepository.com/artifact/org.postgresql/postgresql -->
<dependency>
    <groupId>org.postgresql</groupId>
    <artifactId>postgresql</artifactId>
    <version>42.2.18</version>
</dependency>
```

## postgresql `でエラー
mysqlでは`id`など`で挟む記法があるが、postgresでは許可されていないので`をとる。

## 
```
clickhouse-server --config-file /etc/clickhouse-server/config.xml
java -jar clickhouse-jdbc-bridge-2.0.0-SNAPSHOT.jar
superset run -h 0.0.0.0 -p 8088 --with-threads --reload --debugger
```

## postgres cstore_fdwのインストール (pg_configのディレクトリにパスを通す)

```
git clone https://github.com/citusdata/cstore_fdw.git
cd cstore_fdw
PATH=/usr/bin/:$PATH make
sudo PATH=/usr/bin/:$PATH make install
```

## livLLVM.soがない
```
sudo pacman -S llvm10-libs
```

## superset(docker)からhost側のpostgresqlにアクセスできない。
結論 pg_hba.confの許可が不足していた。**172**.0.0.1/8あたりのipを許可しないといけない。127ではない。
(セキュアにするにはdockerのIP固定化などで許可するIPを絞り込む。)

## superset(docker compose)でうまく動かない。
```
docker-compose down -v --remove-orphans 
docker-compose -f docker-compose-non-dev.yml  up --force-recreate --build
```


## vscodeのremote sshがうまく接続できない。
サーバ側で
```
ps aux | grep vscode
```
して引っかかるプロセスをキルする
