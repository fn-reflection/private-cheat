# investigate

サーバーのリソース状態を確認するためのコマンドをまとめておく。(arch)linuxを中心にまとめる。

## リソース

- cpu
- memory
- storage
- network
- port

## port

```sh
 sudo lsof -i:<port> # portにバインドされているプロセスを見る。
 ss -tnlp | sort -t: -k2 -n # portの使用状態を確認する
```





