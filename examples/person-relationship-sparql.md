# `person-relationship.ttl` 用SPARQLクエリ例

このファイルは、`person-relationship.tsv` から生成したサンプルRDF `person-relationship.ttl` に対する基本的なSPARQLクエリ例です。

元になったTSVデータは、以下のリンクからTSV2KG Editorで直接読み込み、グラフ表示できます。

[サンプルTSVをTSV2KG Editorで開く](https://oecu-kozaki-lab.github.io/TSV2KG-Editor/?tsv=examples/person-relationship.tsv&view=1)

## PREFIX

各クエリの先頭に以下のPREFIXを付けて実行します。

```sparql
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ex:   <http://example.com/>
PREFIX prop: <http://example.com/prop/>
```

## 1. すべてのトリプルを取得する

RDFデータに含まれる主語・述語・目的語の組を確認します。

```sparql
SELECT ?s ?p ?o
WHERE {
  ?s ?p ?o .
}
LIMIT 50
```

## 2. リソースとラベルの一覧を取得する

`rdfs:label` を使って、IRIと表示名の対応を確認します。

```sparql
SELECT ?x ?label
WHERE {
  ?x rdfs:label ?label .
}
ORDER BY ?x
```

## 3. 主語と述語を指定して目的語を取得する

`ex:1` の指導教員を取得します。

```sparql
SELECT ?teacher ?teacherLabel
WHERE {
  ex:1 prop:supervisor ?teacher .
  ?teacher rdfs:label ?teacherLabel .
}
```

## 4. 利用可能なプロパティ一覧を取得する

このRDFデータで定義されているプロパティを確認します。

```sparql
SELECT ?property ?propertyLabel
WHERE {
  ?property rdf:type rdf:Property .
  OPTIONAL {
    ?property rdfs:label ?propertyLabel .
  }
}
ORDER BY ?property
```

## 5. ラベルでリソースを探してから関係を取得する

IRIを直接知らない場合、まずラベル `"太郎"@ja` を持つリソースを探し、そのリソースの指導教員を取得します。

```sparql
SELECT ?student ?teacher ?teacherLabel
WHERE {
  ?student rdfs:label "太郎"@ja .
  ?student prop:supervisor ?teacher .
  ?teacher rdfs:label ?teacherLabel .
}
```

## 6. 述語と目的語を指定して主語を取得する

`ex:7` を指導教員にもつ学生を取得します。

```sparql
SELECT ?student ?studentLabel
WHERE {
  ?student prop:supervisor ex:7 .
  ?student rdfs:label ?studentLabel .
}
```

## 7. ある人物の複数の情報を取得する

`"太郎"@ja` の所属、研究テーマ、興味をまとめて取得します。

```sparql
SELECT ?labLabel ?themeLabel ?interestLabel
WHERE {
  ?person rdfs:label "太郎"@ja .

  ?person prop:affiliation ?lab .
  ?person prop:researchTheme ?theme .
  ?person prop:interest ?interest .

  ?lab rdfs:label ?labLabel .
  ?theme rdfs:label ?themeLabel .
  ?interest rdfs:label ?interestLabel .
}
```

## 8. 所属ごとに人物を取得する

誰がどの研究室・組織に所属しているかを取得します。

```sparql
SELECT ?personLabel ?labLabel
WHERE {
  ?person prop:affiliation ?lab .
  ?person rdfs:label ?personLabel .
  ?lab rdfs:label ?labLabel .
}
ORDER BY ?labLabel ?personLabel
```

## 9. 同じ研究室に所属する人物の組を取得する

`研究室メンバー` のような関係を直接持たせなくても、`所属` から同じ研究室の人物を検索できます。

```sparql
SELECT ?person1Label ?person2Label ?labLabel
WHERE {
  ?person1 prop:affiliation ?lab .
  ?person2 prop:affiliation ?lab .

  FILTER(?person1 != ?person2)

  ?person1 rdfs:label ?person1Label .
  ?person2 rdfs:label ?person2Label .
  ?lab rdfs:label ?labLabel .
}
ORDER BY ?labLabel ?person1Label ?person2Label
```

## 10. 研究室と担当教授を取得する

研究室と、その担当教授を取得します。

```sparql
SELECT ?labLabel ?professorLabel
WHERE {
  ?lab prop:professorInCharge ?professor .
  ?lab rdfs:label ?labLabel .
  ?professor rdfs:label ?professorLabel .
}
```

## 11. 講義、担当教員、履修学生を取得する

講義を担当している教員と、その講義を履修している学生を取得します。

```sparql
SELECT ?courseLabel ?teacherLabel ?studentLabel
WHERE {
  ?teacher prop:inCharge ?course .
  ?student prop:courseCompletion ?course .

  ?course rdfs:label ?courseLabel .
  ?teacher rdfs:label ?teacherLabel .
  ?student rdfs:label ?studentLabel .
}
ORDER BY ?courseLabel ?studentLabel
```

## 12. OPTIONALを使う

`OPTIONAL` を使うと、指定した情報が存在する場合だけ追加で取得できます。

```sparql
SELECT ?personLabel ?themeLabel ?interestLabel
WHERE {
  ?person prop:researchTheme ?theme .
  ?person rdfs:label ?personLabel .
  ?theme rdfs:label ?themeLabel .

  OPTIONAL {
    ?person prop:interest ?interest .
    ?interest rdfs:label ?interestLabel .
  }
}
ORDER BY ?personLabel
```

## 13. 所属ごとの人数を数える

`COUNT` と `GROUP BY` を使って、所属ごとの人数を集計します。

```sparql
SELECT ?labLabel (COUNT(?person) AS ?count)
WHERE {
  ?person prop:affiliation ?lab .
  ?lab rdfs:label ?labLabel .
}
GROUP BY ?labLabel
ORDER BY DESC(?count)
```
