# Research Publication ontology (story 2)

## Queries

> **NB!** Make sure to enable OWL2 reasoning in Blazegraph.

Determine the conference where a given proceeding was published:

```sparql
PREFIX rpub: <https://rawgit.com/berezovskyi/semweb18-modular-onto/master/researchpub_s02.ttl#>

SELECT * {
  rpub:ESWC_Proceeding rpub:conference ?sConference .
}

LIMIT 1001
```