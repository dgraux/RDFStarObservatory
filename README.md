RDF\* Observatory
=================

> Reporting on RDF\* (and SPARQL\*) engines' behaviours&hellip;

Description
-----------

### Context: RDF\* &amp; SPARQL\*

In 2014, Hartig and Thompson proposed [RDF\*](https://arxiv.org/pdf/1406.3399.pdf): a syntax
to express statements of statements for [RDF](https://www.w3.org/TR/rdf11-primer/) data
without involving tedious reification methods. Furthermore, following this RDF extension, they
also suggested an [extension](http://www.diva-portal.org/smash/get/diva2:1141963/FULLTEXT01.pdf)
based on [SPARQL](https://www.w3.org/TR/sparql11-query/) for querying these new datasets.
Recently, Hartig shared an [extension](https://blog.liu.se/olafhartig/documents/sparql-update/)
for the SPARQL UPDATE fragment.

### Announces and achievements from the community

Driven by the propositions from Hartig and Thompson, multiple efforts have been realized by the
Semantic Web community so far:

1. First of all, Hartig developed [a set of tools](https://github.com/RDFstar/RDFstarTools)
based on top of Jena.
2. [Blazegraph](https://github.com/blazegraph/database/wiki/Reification_Done_Right).
3. [AnzoGraph](https://docs.cambridgesemantics.com/anzograph/userdoc/lpgs.htm).
4. In December 2019, [Stardog](https://www.stardog.com/blog/property-graphs-meet-stardog/).
5. [GraphDB](http://graphdb.ontotext.com/documentation/9.2/free/devhub/rdf-sparql-star.html#id1).
6. In May 2020, RDF4J (former Sesame) [announced](https://rdf4j.org/documentation/programming/rdfstar/)
that version 3.2 provides an experimental support.
7. Two weeks later in May 2020, Apache Jena [released](https://jena.apache.org/documentation/rdfstar/)
its version 3.15.0 which now supports RDF\*.

### Toward an Observatory

As presented above, several popular RDF/SPARQL solutions provide a support for the
aforementioned extensions. __However__, as there is yet no standard approaches, engines have
a high degree of freedom. Based on that observation, we wanted to review the current solutions
developed and shed light on their possible differences. In a nutshell, our goals were: first
to observe the existing landscape, developping methods and protocols to compare the solutions
together; and second to discuss the results and share our findings with the hope they would
help the community.

As a first study, we designed [a simple experimental protocol](./CountingStars.md) in order
to investigate the internal representations that RDF\* compliant engines have.

License
-------

This project is shared under the terms of the __Apache License v2.0__ [[here](./LICENSE)].

Contact
-------

[Fabrizio Orlandi](https://badmotor.github.io/)  
[Damien Graux](https://dgraux.github.io/)  
[Declan O'Sullivan](https://www.tcd.ie/research/profiles/?profile=osulldps)  

Affiliated to the [ADAPT Centre](https://www.adaptcentre.ie/)
from the [Trinity College Dublin](https://www.tcd.ie/) in Ireland.

Research partially conducted as part of an [EDGE Marie Sklodowska-Curie](https://edge-research.eu/) fellowship, grant agreement No. 713567.
