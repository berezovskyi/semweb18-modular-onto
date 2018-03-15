# Research Publication ontology (story 2)


## Queries

> **NB!** Make sure to enable OWL2 reasoning in Blazegraph. Run `CLEAR ALL` *SPARQL UPDATE* query before loading the ontology if needed.

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

LIMIT 10001
```

**CQ3** *Which publisher published a given Proceeding?*:

```sparql
PREFIX rpub: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/researchpub_s02.ttl#>

SELECT ?sPublisher {
  rpub:ESWC_Proceeding rpub:publisher ?sPublisher .
}

LIMIT 1001
```

> **NB!** The affiliation of an author has the problem similar to that of a Role ontology design pattern, as the affiliation of the author can from an article to an articles due to changes of employer or switch to another project. **These concerns are not addressed in this ontology module**.

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