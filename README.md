# elasticsearch-sand

elasticsearch を docker でサクッとお試ししたいやつ  
メモ含みます

## ファイル

- docker-compose.yml
- Dockerfile
  - 日本語の形態素解析のため、 `kuromoji` をインストールするために作成

## 起動時の注意

mac では問題ないとのこと。  
windows では管理者権限で PowerShell を管理者権限で開いて、

```
wsl -d docker-desktop
sysctl -w vm.max_map_count=262144
```

上記を実行したのち、 `docker-compose up -d` で起動する。

linux では `sysctl -w vm.max_map_count=26144` を実行したのちに起動する。

## 起動確認

`curl -XGET http://localhost:9200/` でエラーになっていない様子の json なレスポンスが返ってくれば成功。

`curl -X GET "localhost:9200/_cat/health?v&pretty"` クラスタのステータス取得できる。

## 基本的な動作などの参考文献

以下の Qiita 記事がわかりやすいと思いました。  
https://qiita.com/kiyokiyo_kzsby/items/344fb2e9aead158a5545

index を作って document を登録するのは基本的に変わらない流れのようす。

## データ操作メモ

v7 からは `type` は `_doc` 固定でほとんど問題なさそう？

- インデックス作成
  - PUT `/{index_name}` -H 'Content-Type: application/json' -d @japanese_settings.json
- インデックス確認
  - GET `/index_name`
- ドキュメント作成
  - PUT `/index/type/document_id`
  - /hoge/\_doc/1
  - create.json
- ドキュメント取得
  - GET `/index/type/document_id`
  - 基本的に id 指定で取得する形
  - 全件取得したい場合は後述する検索クエリを利用する

## マッピングメモ

https://www.elastic.co/jp/blog/how-to-implement-japanese-full-text-search-in-elasticsearch

ここを写経しながら、認識しやすいワードに変更した (japanese_settings.json)  
`target_contents` というキーに入れた値が、日本語&bigram で解析されてインデクシングされるようになった
ファイルに記述した書き方だと、ドキュメントが未登録の時しか適用できないかも

PUT `/index` -d japanese_settings.json
PUT `/index/_mapping/{type_name}` {mappings.properties 以下の部分}
