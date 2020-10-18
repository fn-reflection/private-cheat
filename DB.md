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

