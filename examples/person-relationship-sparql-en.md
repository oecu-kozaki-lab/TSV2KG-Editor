# SPARQL Examples for `person-relationship-en.ttl`

You can open the source TSV directly in TSV2KG Editor and display it as a graph:

[Open the sample TSV in TSV2KG Editor](https://oecu-kozaki-lab.github.io/TSV2KG-Editor/?tsv=examples/person-relationship-en.tsv&view=1)

## Prefixes

```sparql
PREFIX rdf:  <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX ex:   <http://example.com/>
PREFIX prop: <http://example.com/prop/>
```

## 1. List triples

```sparql
SELECT ?s ?p ?o
WHERE {
  ?s ?p ?o .
}
LIMIT 50
```

## 2. List resources and labels

```sparql
SELECT ?x ?label
WHERE {
  ?x rdfs:label ?label .
}
ORDER BY ?x
```

## 3. Get Alice's supervisor

```sparql
SELECT ?teacher ?teacherLabel
WHERE {
  ex:Alice prop:supervisor ?teacher .
  ?teacher rdfs:label ?teacherLabel .
}
```

## 4. List available properties

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

## 5. Find a resource by label, then query its relations

```sparql
SELECT ?student ?teacher ?teacherLabel
WHERE {
  ?student rdfs:label "Alice"@en .
  ?student prop:supervisor ?teacher .
  ?teacher rdfs:label ?teacherLabel .
}
```

## 6. List people by affiliation

```sparql
SELECT ?personLabel ?labLabel
WHERE {
  ?person prop:affiliation ?lab .
  ?person rdfs:label ?personLabel .
  ?lab rdfs:label ?labLabel .
}
ORDER BY ?labLabel ?personLabel
```

## 7. Find people in the same lab

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

## 8. Count people by affiliation

```sparql
SELECT ?labLabel (COUNT(?person) AS ?count)
WHERE {
  ?person prop:affiliation ?lab .
  ?lab rdfs:label ?labLabel .
}
GROUP BY ?labLabel
ORDER BY DESC(?count)
```
