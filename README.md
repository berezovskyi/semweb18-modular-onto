# Research Publication ontology (story 2)


## Story no. 2 Queries

> **NB!** Make sure to enable OWL2 reasoning in Blazegraph. Run `CLEAR ALL` *SPARQL UPDATE* query before loading the ontology if needed.

> **NB!** When running queries on the merged ontology, use the `https://rawgit.com/berezovskyi/semweb18-modular-onto/master/researchpub_s02-merged_s03.ttl` as the `rpub` namespace prefix URI.

**CQ1** *What Conference does a given Proceeding belong?*:

```sparql
PREFIX rpub: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/researchpub_s02.ttl#>

SELECT * {
  rpub:ESWC_Proceeding rpub:conference ?sConference .
}

LIMIT 10001
```

**CQ2** *How many Volumes does a given Proceeding have?*:

```sparql
PREFIX rpub: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/researchpub_s02.ttl#>

SELECT (COUNT(?sVolume) as ?volumenCount) {
  rpub:ESWC_Proceeding rpub:proceeding_volume ?sVolume .
}
```

**CQ3** *Which publisher published a given Proceeding?*:

```sparql
PREFIX rpub: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/researchpub_s02-merged_s03.owl#>
PREFIX s3: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/story3.owl#>

SELECT (COUNT(?sVolume) as ?volumenCount) {
#    ?s a rpub:Proceeding .
    ?s rpub:proceeding_volume ?sVolume .
}
```

> **NB! ** The affiliation of an author has the problem similar to that of a Role ontology design pattern, as the affiliation of the author can from an article to an articles due to changes of employer or switch to another project. **These concerns are not addressed in this ontology module**.

**CQ4** *Which university does a given author come from?*:

```sparql
PREFIX rpub: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/researchpub_s02.ttl#>

SELECT ?sUniversity {
  rpub:Heiko_Paulheim rpub:affiliation ?sAffiliation .
  ?sAffiliation rpub:affiliation ?sUniversity .
}

LIMIT 1001
```

**CQ5** *What is the name of a paper authored by a given author in a given proceeding?*:

```sparql
PREFIX rpub: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/researchpub_s02.ttl#>

SELECT DISTINCT ?label ?sPaper {
  ?sPaper a rpub:Paper .
  ?sPaper rpub:author rpub:Heiko_Paulheim .
  ?sPaper rpub:proceeding_volume ?sVolume .
  ?sProceeding rpub:proceeding_volume ?sVolume .
  ?sPaper rdfs:label ?label .
}

LIMIT 1001
```

**CQ6** *Which papers are in a proceeding volume?*:

```sparql
PREFIX rpub: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/researchpub_s02.ttl#>

SELECT DISTINCT ?label ?sPaper {
  ?sPaper rpub:proceeding_volume rpub:ESWCProceedingVolume1 .
  ?sPaper rdfs:label ?label .
}

LIMIT 1001
```


## Ontology merging

The original plan was to verify the correctness of the ontologies by re-running the queries again. But there were 2 problems:

- the queries for the Story 2 were not instance-free
- the queries for the Story 3 were not ready the merging of the ontologies began

Therefore, we decided to do two things:

- rewrite the queries for the Story 2 to be instance-free
- export all the inferences made by a reasoner into a [separate file](merged_inferences.owl) and go over them manually, checking the sanity of the reasoner output on the newly merged ontology


## Instance-free queries for the Story no. 2

**CQ1** *What Proceedings exist for each Conference?*

```sparql
PREFIX rpub: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/researchpub_s02-merged_s03.owl#>
PREFIX s3: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/story3.owl#>

SELECT * {
  ?s a rpub:Proceeding .
}
```

**CQ2** *What Volumes does a each Proceeding have?*:

```sparql
PREFIX rpub: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/researchpub_s02.ttl#>

SELECT (COUNT(?sVolume) as ?volumenCount) {
  rpub:ESWC_Proceeding rpub:proceeding_volume ?sVolume .
}
```

**CQ3** *Which publisher published a given Proceeding?*:

```sparql
PREFIX rpub: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/researchpub_s02.ttl#>

SELECT ?sPublisher {
  rpub:ESWC_Proceeding rpub:publisher ?sPublisher .
}
```

> **NB!** The affiliation of an author has the problem similar to that of a Role ontology design pattern, as the affiliation of the author can from an article to an articles due to changes of employer or switch to another project. **These concerns are not addressed in this ontology module**.

**CQ4** *Which university does a given author come from?*:

```sparql
PREFIX rpub: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/researchpub_s02.ttl#>

PREFIX rpub: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/researchpub_s02-merged_s03.owl#>
PREFIX s3: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/story3.owl#>

SELECT ?researcher ?university {
  ?uniAff a rpub:University_Affiliation .
  ?researcher rpub:affiliation ?uniAff .
  ?uniAff rpub:affiliation ?university .
}
```

**CQ5** *What is the name of a paper authored by a given author in a given proceeding?*:

```sparql
PREFIX rpub: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/researchpub_s02.ttl#>

SELECT DISTINCT ?label ?author {
  ?sPaper a rpub:Paper .
  ?sPaper rpub:author ?author .
  ?sPaper rpub:proceeding_volume ?sVolume .
  ?sProceeding rpub:proceeding_volume ?sVolume .
  ?sPaper rdfs:label ?label .
}
```

**CQ6** *Which papers are in a proceeding volume?*:

```sparql
PREFIX rpub: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/researchpub_s02.ttl#>

SELECT DISTINCT ?label ?volume {
  ?sPaper rpub:proceeding_volume ?volume .
  ?sPaper rdfs:label ?label .
}
```

## Inference analysis

---

Initially, the Turtle format was used for export, but the Protege v5.2.0 was not showing those ontologies as part of the workspace, while it did for the ontologies saved in RDF/XML format with the `*.owl` extension.
