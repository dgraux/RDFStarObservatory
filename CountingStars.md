Observing RDF\* engines' internal representations
=================================================

> When counting the stars in a constellation tells about data storage!

Task
----

In order to illustrate this variety of possible representations, let’s consider the simplest
RDF\* dataset: `<< <s> <p> <o> >> <d> <e>`; and let’s try to answer the following question:
&laquo;once stored, what should `SELECT (count(*) as ?c) WHERE{?s ?p ?o}` return?&raquo;.
One possible solution is to consider the reification method where actually __six__ triples are
to be created. Another one is to use the singleton property method which would lead to
__four__ triples created. Finally, a more naïve one could be to answer __two__, as there are
two predicates (`<p>` and `<d>`) in the considered example.

Protocol
--------

To have a glimse at their internal representations, we devised
[an experimental protocol](./testSuits/CountingStars/) involving very few triples and three
simple queries.

### Dataset

The proposed dataset only involves four triples: 1 classic RDF triple, 2 basic RDF\* statements
and 1 nested RDF\* statement.

    @prefix : <http://test.> .
    :s1 :p1 :o1 .
    <<:s2 :p2 :o2>> :d2 :e2 .
    <<:s4 :p4 :o4>> :d4 :e4 .
    << <<:s3 :p3 :o3>> :d3 :e3 >> :t3 :u3 .


### Queries

The three considered queries focus on retrieving all the known data of different _shape_ _i.e._
from regular triples to nested statement of statement of statement.

    Select * Where{ ?s ?p ?o }
    Select * Where{ << ?s ?p ?o >> ?d ?e }
    Select * Where{ <<<<?s ?p ?o>> ?d ?e>> ?t ?u }


Competitors
-----------

As of today, the following engines have been tested:

* [Stardog](https://www.stardog.com/) _version 7.1.2_
* [Blazegraph](https://blazegraph.com/) _version 2.1.5_
* [ExecuteSPARQLStar](https://github.com/RDFstar/RDFstarTools) _version 0.0.1-SNAPSHOT_ from
RDFStar repository

Observations
------------

Citation
--------

This study has been published at ESWC 2020 and can be cited using the following BibTeX code:

    @article{orlandi2020countingstars,
        title={How many stars do you see in this constellation?},
        author={Fabrizio Orlandi and Damien Graux and Declan O'Sullivan},
        journal={ESWC (Poster track)},
        year={2020}
    }

