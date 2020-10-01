# elasticsearch

## RDB vs elastic search 用語比較

| **RDB**        | **Elasticsearch**    |
| -------------- | -------------------- |
| データベース   | インデックス         |
| テーブル       | マッピングタイプ     |
| カラム（列）   | フィールド           |
| レコード（行） | 文書（ドキュメント） |

## TIPS

- マッピングタイプはdeprecated
- SQLと同じようにstrictなtypedefもできるが・・・
- Textは全文検索可能
- Keywordはソートや集約のキーとして使える
- 期間を過ぎたら自動的に削除するという機能もある
- _bulk apiで投入するのが効率が良い

## command例

```sh
curl pls:9200/lint -X PUT
```



## 参考文献

https://dev.classmethod.jp/articles/elasticsearch-getting-started-01/

https://www.elastic.co/guide/en/elasticsearch/reference/current/removal-of-types.html

https://blog.web.nifty.com/engineer/1084

https://qiita.com/a4_nghm/items/94ed0640ed8a15ce1592

https://www.slideshare.net/snuffkin/elasticsearch-as-a-distributed-system

https://qiita.com/btamari-gy/items/085f0a98f3ec54d919a0

https://discuss.elastic.co/t/best-practice-index-naming/38007

[https://www.elastic.co/guide/en/elasticsearch/guide/current/time-based.html?q=time%20based](https://www.elastic.co/guide/en/elasticsearch/guide/current/time-based.html?q=time based)

https://www.elastic.co/jp/blog/elasticsearch-as-a-time-series-data-store

https://www.elastic.co/jp/blog/querying-and-aggregating-time-series-data-in-elasticsearch

https://www.elastic.co/guide/en/elasticsearch/reference/master/indices-rollover-index.html

https://qiita.com/shnagai/items/affeac4c7be266f8c459

https://stackoverflow.com/questions/15404225/can-we-do-a-bulk-index-without-specifying-a-document-id-for-elasticsearch