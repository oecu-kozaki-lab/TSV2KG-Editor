# TSV2KG Editor

**TSVベースの軽量なRDF/KG可視化・編集ツール**

[English README](README.md)

TSV2KG Editor は、TSV形式で入力した主語・述語・目的語の関係からRDF/KGを作成するためのブラウザベースの小さなツールです。TSVデータを貼り付け、グラフとして可視化し、ノードやエッジのラベル・色を編集し、RDF/Turtleとして出力できます。

## オンラインで試す

GitHub Pagesで公開している版を以下から実行できます。

https://oecu-kozaki-lab.github.io/TSV2KG-Editor/

## 主な機能

- シンプルなTSV入力からRDF/KGデータを作成
- トリプルをインタラクティブなグラフとして可視化
- ダブルクリックによるノード・エッジラベルの編集
- Ctrl + ダブルクリックによるノード・エッジ色の変更
- グラフ表示領域、TSV入力欄、RDF出力欄のサイズ変更
- `base-IRI`、`property-IRI`、`PREFIX` を指定したTurtle生成
- IRI自動生成、`rdfs:label` 出力、`rdf:Property` 出力の切り替え
- 日本語述語から英語プロパティ名候補を生成し、対応表で確認・修正
- 生成したトリプルのSPARQL Updateエンドポイントへの送信

## 使い方

ブラウザで `index.html` を開きます。

ビルドは不要です。HTML/CSS/JavaScriptのみで動作する静的Webアプリです。

グラフ表示には、CDNから読み込む [vis-network](https://visjs.github.io/vis-network/) を利用しています。

GitHub Pagesで公開する場合も、リポジトリのルートを公開元にすればそのまま動作します。

## TSV形式

基本形式:

```tsv
主語	述語	目的語
```

色指定を含む形式:

```tsv
主語	述語	目的語	主語色	目的語色	エッジ色
```

例:

```tsv
太郎	友人	花子	#90CAF9	#90CAF9	#64B5F6
太郎	所属	知識情報研究室	#90CAF9	#B0BEC5	#78909C
知識情報研究室	担当教授	佐藤教授	#B0BEC5	#CE93D8	#AB47BC
太郎	研究テーマ	知識グラフ	#90CAF9	#A5D6A7	#66BB6A
佐藤教授	担当	知識表現特論	#CE93D8	#FFCC80	#F57C00
```

## RDF生成オプション

- **IRIを自動生成**
  - ONの場合、主語・目的語は `base-IRI` に基づく連番IRIになります。
  - 述語は `property-IRI` に基づき、翻訳候補または対応表で確認したプロパティ名から生成されます。
  - OFFの場合、主語・目的語は `base-IRI + 元文字列`、述語は `property-IRI + 元文字列` で生成されます。
- **rdfs:labelを出力**
  - リソースやプロパティに `rdfs:label` を出力します。
- **rdf:Propertyを出力**
  - 述語に `rdf:type rdf:Property` を出力します。

## サンプル

サンプルファイルは `examples/` フォルダにあります。

- [`person-relationship.tsv`](examples/person-relationship.tsv): 架空の人物相関図のTSV入力例
- [`person-relationship.ttl`](examples/person-relationship.ttl): TSVから生成したTurtle例
- [`person-relationship-sparql.md`](examples/person-relationship-sparql.md): 生成したRDFに対する基本的なSPARQLクエリ例

## 注意

- 日本語述語の翻訳候補生成では、述語文字列を外部のGoogle Translate系エンドポイントへ送信します。
- 外部送信を避けたい場合は、IRI自動生成をOFFにするか、述語変換表へ手動でプロパティ名を入力してください。

## ライセンス

Apache License 2.0です。詳細は [`LICENSE`](LICENSE) を参照してください。
