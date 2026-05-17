# TSV2KG Editor

**A lightweight TSV-based RDF/KG visualization and editing tool**

[日本語版 README](README.ja.md)

TSV2KG Editor is a small browser-based tool for creating RDF/KG data from tab-separated triples. You can paste TSV data, visualize it as an interactive graph, edit node and edge labels/colors, and export the result as RDF/Turtle.

## Features

- Create RDF/KG data from simple TSV input.
- Visualize triples as an interactive graph.
- Edit node and edge labels by double-clicking.
- Change node and edge colors with Ctrl + double-click.
- Resize the graph, TSV input, and RDF output areas.
- Generate Turtle with configurable `base-IRI`, `property-IRI`, and `PREFIX` declarations.
- Optionally generate IRIs, output `rdfs:label`, and output `rdf:Property` declarations.
- Suggest English property names for Japanese predicates and confirm them in a mapping table.
- Send generated triples to a SPARQL Update endpoint.

## Getting Started

Open `index.html` in a web browser.

No build step is required. The tool is a static HTML/CSS/JavaScript application.

The graph view uses [vis-network](https://visjs.github.io/vis-network/) loaded from a CDN.

If published with GitHub Pages, the app can run directly from the repository root.

## TSV Format

Basic input format:

```tsv
subject	predicate	object
```

Optional color columns:

```tsv
subject	predicate	object	subjectColor	objectColor	edgeColor
```

Example:

```tsv
Alice	friend	Bob	#90CAF9	#90CAF9	#64B5F6
Alice	affiliation	Knowledge Representation Lab	#90CAF9	#B0BEC5	#78909C
Knowledge Representation Lab	professorInCharge	Prof. Smith	#B0BEC5	#CE93D8	#AB47BC
Alice	researchTheme	Knowledge Graphs	#90CAF9	#A5D6A7	#66BB6A
Prof. Smith	inCharge	Knowledge Representation	#CE93D8	#FFCC80	#F57C00
```

## RDF Generation Options

- **Generate IRIs automatically**
  - Subjects and objects use serial numbers based on `base-IRI`.
  - Predicates use translated or confirmed property names based on `property-IRI`.
  - If disabled, subjects/objects use `base-IRI + original text`, and predicates use `property-IRI + original text`.
- **Output rdfs:label**
  - Outputs `rdfs:label` triples for resources and properties.
- **Output rdf:Property**
  - Outputs `rdf:type rdf:Property` triples for predicates.

## Examples

Example files are in the `examples/` directory.

- [`person-relationship-en.tsv`](examples/person-relationship-en.tsv): TSV input for a small fictional person relationship graph in English.
- [`person-relationship-en.ttl`](examples/person-relationship-en.ttl): Example Turtle output generated from the English TSV.
- [`person-relationship-sparql-en.md`](examples/person-relationship-sparql-en.md): Basic SPARQL queries for the English RDF example.

Japanese examples are also available:

- [`person-relationship.tsv`](examples/person-relationship.tsv)
- [`person-relationship.ttl`](examples/person-relationship.ttl)
- [`person-relationship-sparql.md`](examples/person-relationship-sparql.md)

## Notes

- The Japanese predicate translation feature sends predicate text to Google Translate through a public translation endpoint.
- If you do not want to send predicate text externally, disable automatic IRI generation or enter property names manually in the predicate mapping table.

## License

Apache License 2.0. See [`LICENSE`](LICENSE).
