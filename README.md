# GSQ / CGS Commodity Codes Linkset
This is a Linkset - a special kind of [RDF](https://www.w3.org/RDF/) Dataset that relates objects in one Dataset to objects in another.

This Linkset contains [SKOS mapping properties](https://www.w3.org/TR/skos-reference/#mapping)
between Concepts in the [GSQ Commodity Codes vocabulary](http://linked.data.gov.au/def/gsq-commodity-codes) and Concepts in the [CGI Commodity Codes vocabulary](http://resource.geosciml.org/classifierscheme/cgi/2016.01/commodity-code). The first is Queensland's list and the second is the international geoscience consortium's list.

Only the following SKOS mapping properties are so far used as link predicates: `skos:exactMatch`.

## Reification
This Linkset stores all its data in a special way. Instead of relating Dataset A's Object 1 to Dataset B's Object 2 via relationship X in the usual way that you would make an RDF link (triple) like this:

```
<A.1> <X> <B.2> .
```
where `<A.1>` is the *subject*, `<X>` the *predicate* and `<B.2>` the *object*,  it's related like this:
```
[]
    a rdf:Statement ;
    rdf:subject <A.1> ;
    rdf:predicate <X> ;
    rdf:object <B.2> .
```
What we've done here is *shove* the triple's subject, predicate & object information into a containing object's properties. Nowe we have a `rdf:Statement` object with `rdf:subject`, `rdf:predicate` & `rdf:object` properties. This lets use include information such as when that link (triuple) was created and what method was used, like this:

```
[]
    a rdf:Statement ;
    rdf:subject <A.1> ;
    rdf:predicate <X> ;
    rdf:object <B.2> ;
    loci:hadGenerationMethod <SOME-METHOD> ;
    dct:created "2019-10-08"^^xsd:date .
```
## Un-reifying data
Run this SPARQL query on this RDF data, loaded into a triplestore, to un-reify the data. That is, to *un-shove* the data from the statement objects back into *normal* triples:
```
PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#>
SELECT * 
WHERE {
    [] a rdf:Statement ;
       rdf:subject ?s ;
       rdf:predicate ?p ;
       rdf:object ?o.
}
```
This will turn this:
```
[] a rdf:Statement ;
    rdf:subject gsqcc:clay ;
    rdf:predicate skos:exactMatch ;
    rdf:object cgicc:clay ;    
    loci:hadGenerationMethod method: ;
    dct:created "2019-10-09"^^xsd:date .
```
into this:
```
gsqcc:clay skos:exactMatch cgicc:clay .
```

## Contacts
**Geoscience Information Team**,
Geological Survey of Queensland,
Department of Resources,
Brisbane, QLD, Australia,
<geological_info@resources.qld.gov.au>
