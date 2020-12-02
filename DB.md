# DB master

|                          | mysql                                 | postgreSQL                              | Elastic search | influx DB                     |
| ------------------------ | ------------------------------------- | --------------------------------------- | -------------- | ----------------------------- |
| 前回入力時のエラーの確認 | show errors;                          |                                         |                |                               |
| 前回入力時の警告の確認   | show warnings;s                       |                                         |                |                               |
| DBの作成                 | create database DBNAME;               | create database DBNAME;                 |                |                               |
| DBの作成2                | create database if not exists DBNAME; |                                         |                |                               |
| DBの作成3                |                                       | create database DBNAME encoding 'utf8'; |                |                               |
| DBの確認                 | show databases;                       | \l OR select * from pg_database;        |                | show databases;               |
| ユーザーの確認           | select * from mysql.user              |                                         |                |                               |
| current DBの選択         | use DBNAME;                           | \c DBNAME;                              |                |                               |
| テーブル一覧             |                                       | \dt;                                    |                |                               |
| 実行計画                 | explain QUERY;                        |                                         |                |                               |
| 設定ファイル             | [/etc/,C:\ProgramData\mysql\\]my.cnf  |                                         |                | `/etc/influxdb/influxdb.conf` |
| case-sensitive           | lower_case_table_names                |                                         |                |                               |
| スレッドキャッシュ       | thread_cache_size                     |                                         |                |                               |
| 最大接続数               | max_connections                       |                                         |                |                               |
| 通信バッファ(デフォルト) | net_buffer_length                     |                                         |                |                               |
| 使用CPU数                |                                       |                                         |                | GOMAXPROCS                    |
| 開発用レポートの停止     |                                       |                                         |                | reporting-disabled=false      |
|                          |                                       |                                         |                |                               |
|                          |                                       |                                         |                |                               |
|                          |                                       |                                         |                |                               |

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

