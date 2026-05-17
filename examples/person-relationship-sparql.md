# SPARQL Examples for `person-relationship.ttl`

These examples use the sample RDF graph generated from `person-relationship.tsv`.

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

## 3. Get an object by subject and predicate

Get the supervisor of `ex:1`.

```sparql
SELECT ?teacher ?teacherLabel
WHERE {
  ex:1 prop:supervisor ?teacher .
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

## 5. Find a subject by label, then query its relations

```sparql
SELECT ?student ?teacher ?teacherLabel
WHERE {
  ?student rdfs:label "太郎"@ja .
  ?student prop:supervisor ?teacher .
  ?teacher rdfs:label ?teacherLabel .
}
```

## 6. Get subjects by predicate and object

Get students supervised by `ex:7`.

```sparql
SELECT ?student ?studentLabel
WHERE {
  ?student prop:supervisor ex:7 .
  ?student rdfs:label ?studentLabel .
}
```

## 7. Get multiple properties of a resource

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

## 8. List people by affiliation

```sparql
SELECT ?personLabel ?labLabel
WHERE {
  ?person prop:affiliation ?lab .
  ?person rdfs:label ?personLabel .
  ?lab rdfs:label ?labLabel .
}
ORDER BY ?labLabel ?personLabel
```

## 9. Find people in the same lab

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

## 10. List labs and professors in charge

```sparql
SELECT ?labLabel ?professorLabel
WHERE {
  ?lab prop:professorInCharge ?professor .
  ?lab rdfs:label ?labLabel .
  ?professor rdfs:label ?professorLabel .
}
```

## 11. List courses, instructors, and students

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

## 12. Use OPTIONAL

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

## 13. Count people by affiliation

```sparql
SELECT ?labLabel (COUNT(?person) AS ?count)
WHERE {
  ?person prop:affiliation ?lab .
  ?lab rdfs:label ?labLabel .
}
GROUP BY ?labLabel
ORDER BY DESC(?count)
```
