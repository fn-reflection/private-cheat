# DB master

主要RDBであるmysqlとpostgresの差異＋主要なNoSQLであるelasticsearch、そして個人的に良いと思っているDWHシステムのclickhouseについてコマンドやコンフィグを比較してみる。(少しずつ埋めていく。)

## コンフィグ

|                                | mysql                                                  | Postgres | elastic search | clickhouse                                                   |
| ------------------------------ | ------------------------------------------------------ | -------- | -------------- | ------------------------------------------------------------ |
| 設定ファイルのデフォルトの場所 | linux: /etc/my.cnf<br>win: C:\ProgramData\mysql\my.cnf |          |                | /etc/clickhouse-server.config.xml<br>/etc/clickhouse-server.users.xml |
| スレッドキャッシュ             | thread_cache_size                                      |          |                |                                                              |
| 最大接続数                     | max_connections                                        |          |                |                                                              |
| 通信バッファ                   | net_buffer_length                                      |          |                |                                                              |

## admin

|                  | mysql                                 | postgreSQL                            | Elastic search | Clickhouse      |
| ---------------- | ------------------------------------- | ------------------------------------- | -------------- | --------------- |
| DBの作成         | create database if not exists DBNAME; | create database if not exists DBNAME; |                |                 |
| DBの確認         | show databases;                       | \l OR select * from pg_database;      |                | show databases; |
| ユーザーの確認   | select * from mysql.user              |                                       |                |                 |
| current DBの選択 | use DBNAME;                           | \c DBNAME;                            |                |                 |
| テーブル一覧     |                                       | \dt;                                  |                |                 |

## DDL, DML

|          | mysql   | postgreSQL                 | Elastic search | Clickhouse |
| -------- | ------- | -------------------------- | -------------- | ---------- |
| 実行計画 | explain | explain<br>explain analyze |                | Explain    |

## function

|                                                     | mysql | postgreSQL                           | Clickhouse                          | elastic search |
| --------------------------------------------------- | ----- | ------------------------------------ | ----------------------------------- | -------------- |
| キャスト                                            |       |                                      | CAST(p AS String)                   |                |
| 平均                                                |       | avg(value)                           | avg(value)                          |                |
| 最大・最小                                          |       | max,min(value)                       |                                     |                |
| 標準偏差[標本標準偏差]                              |       |                                      | stdpop\[stdSamp\]                   |                |
| ピアソンの相関係数[安定計算]                        |       |                                      | corr\[corrStable\](x,y)             |                |
| 現在のtimestamp                                     |       | current_timestamp                    | now()                               |                |
| 現在のtimestamp(utc)                                |       | current_timestamp AT TIME ZONE 'etc' | now('UTC')                          |                |
| 3時間を表す                                         |       | interval '3 hours'                   | interval '3 hours'                  |                |
| 分単位へのダウンサンプリング                        |       | date_trunc('minute', time)           | toStartOfMinute(time)               |                |
| datetimeから月を抽出する                            |       | EXTRACT(MONTH FROM time)             | EXTRACT(MONTH FROM time)            |                |
| GROUP BYした結果を(SORTして)最初[最後]のvalueをとる |       |                                      | argMin\[argMax\](value, sortColumn) |                |





## EXPLAIN

### MYSQL

| description                      | Meaning                                                      |
| -------------------------------- | ------------------------------------------------------------ |
| select_type                      | SELECTのタイプ like PRIMARY DERIVED SUBQUERY　DEPENDENT_SUBQUERY |
| select_type=PRIMARY              | 外部クエリ                                                   |
| select_type=SUBQUERY             | サブクエリ。行評価ごとにSQL飛ぶので遅い                      |
| select_type=DEPENDENT_SUBQUERY   | 相関サブクエリ。行評価ごとにサブクエリが実行される(かなり遅い) |
| select_type=UNCACHEABLE_SUBQUERY | 行評価ごとにサブクエリが実行される(かなり遅い)               |
| select_type=DERIVED              | FROMでサブクエリを使用・サブクエリ→外部クエリの順で実行される。(まあまあ早い) |
| Table                            | テーブル                                                     |
| Partitions                       | テーブルパーティション                                       |
| type                             | テーブルのアクセスタイプ like const ALL ref eq_ref range     |
| type=const                       | UNIQUE indexによるルックアップ、速い                         |
| type=eq_ref                      | UNIQUE indexによるJOIN、速い                                 |
| type=ref                         | indexによる等価検索、まあまあ速い                            |
| type=range                       | indexによる範囲検索、まあまあ速い                            |
| type=index                       | インデックスをフルスキャン、遅い                             |
| type=ALL                         | テーブルをフルスキャンする、かなり遅い                       |
| Possible_keys                    | 利用可能なキー like PRIMARY auto_key0 index_...              |
| キー                             | 実際に使用するキー like PRIMARY auto_key0 index_...          |
| Key_len                          | インデックスキーの長さ(バイト数)、長いとスキャン効率が悪い   |
| ref                              | 比較カラム(constの場合は定数と比較する)                      |
| rows                             | スキャン行数(見積値)<br> DERIVEDなテーブルについては実際にanalyzeされるので正確な値<br> Using Whereなどする場合は、実際に返される値はこれより小さい |
| Filtered                         |                                                              |
| Extra                            | like Using index; Using temporary; Using file sort; Using join buffer (Block Nested Loop); Using index for group-by |
| Using where                      | インデックスだけでクエリ(where)を解決できない(ので少し遅い)  |
| Using index                      | インデックスだけでクエリを解決できる                         |
| Using filesort                   | クイックソートを実行している(ので遅い)                       |
| Using temporary                  | クエリにテンポラリテーブルが必要(なので遅い)                 |
|                                  |                                                              |



### postgres

| description | meaning |
| ----------- | ------- |
|             |         |

