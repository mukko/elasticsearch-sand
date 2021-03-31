# elasticsearch-sand

elasticsearchをdockerでサクッとお試ししたいやつ  
メモ含みます  

## ファイル

- docker-compose.yml
- Dockerfile
  - 日本語の形態素解析のため、 `kuromoji` をインストールするために作成

## 起動時の注意

macでは問題ないとのこと。  
windowsでは管理者権限でPowerShellを管理者権限で開いて、  

```
wsl -d docker-desktop
sysctl -w vm.max_map_count=262144
```

上記を実行したのち、 `docker-compose up -d` で起動する。  

linuxでは `sysctl -w vm.max_map_count=26144` を実行したのちに起動する。  

## 起動確認

`curl -XGET http://localhost:9200/`  でエラーになっていない様子のjsonなレスポンスが返ってくれば成功。  

`curl -X GET "localhost:9200/_cat/health?v&pretty"`  クラスタのステータス取得できる。  

## 基本的な動作などの参考文献

以下のQiita記事がわかりやすいと思いました。  
https://qiita.com/kiyokiyo_kzsby/items/344fb2e9aead158a5545

indexを作ってdocumentを登録するのは基本的に変わらない流れのようす。  