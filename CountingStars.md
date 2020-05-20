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

* [Stardog](https://www.stardog.com/) _version 7.1.2_ which is a commercial enterprise
Knowledge Graph platform
* [Blazegraph](https://blazegraph.com/) _version 2.1.5_ which is a &lsquo;&lsquo;
high-performance &rsquo;&rsquo; graph database supporting Blueprints and RDF/SPARQL
APIs.
* [ExecuteSPARQLStar](https://github.com/RDFstar/RDFstarTools) _version 0.0.1-SNAPSHOT_ from
RDFStar repository, which can only query, on-the-fly, data stored in an RDF\* file (_e.g._
using Turtle\*)

Observations
------------

We split our experiments in two heats. First, we loaded only the first two triples of the
presented dataset above and ask Q1 and Q2. Second, we loaded the complete dataset and ran the
three queries. These two heats enable observations when engines are dealing with simple
RDF\* statements and then with a nested statement _i.e._ a statement of a statement of
statement.

#### With only `:s1 :p1 :o1` and `<<:s2 :p2 :o2>> :d2 :e2`

Stardog, Blazegraph and ExecuteSPARQLStar returned that 3 entities were created during the
loading phase. Then, we ran Q1 and Q2. The obtained results, for both the engines and the
ExecuteSPARQLStar were:

|Query|Results for all the engines|
|:---:|:-----:|
|Q1|:s1 :p1 :o1,<br>:s2 :p2 :o2,<br>\{:s2 :p2 :o2\} :d2 :e2|
|Q2|:s2 :p2 :o2 :d2 :e2|

This implies that all systems consider the RDF-Graph of an RDF\* statement as a subject and,
in case of Q1, counted 3 triples (which corresponds to the naïve counting idea we had in
the introduction above).

#### With the complete dataset

For the second round of experimentation we loaded all the 4 statements of
`./testSuits/CountingStars/data.nt` and used the 3 queries.

|Query|Stardog|Blazegraph &amp; ExecuteSPARQLStar|
|:---:|:-----:|:--------------------------------:|
|Q1|s1 p1 o1, s2 p2 o2,<br>s3 p3 o3, s4 p4 o4,<br>\{s2 p2 o2\} d2 e2,<br>\{s4 p4 o4\} d4 e4,<br>\{s3 d3 e3\} t3 u3,<br>s3 d3 e3 |s1 p1 o1, s2 p2 o2,<br>s3 p3 o3, s4 p4 o4,<br>\{s2 p2 o2\} d2 e2,<br>\{s4 p4 o4\} d4 e4,<br>\{s3 p3 o3\} d3 e3,<br>\{\{s3 p3 o3\} d3 e3\} t3 u3|
|Q2|s2 p2 o2 d2 e2,<br>s4 p4 o4 d4 e4,<br>s3 d3 e3 t3 u3 |s2 p2 o2 d2 e2,<br>s4 p4 o4 d4 e4,<br>s3 p3 o3 d3 e3,<br>\{s3 p3 o3\} d3 e3 t3 u3 |
|Q3|ø|:s3 :p3 :o3 :d3 :e3 :t3 :u3|

We observed that both Blazegraph and ExecuteSPARQLStar presented the same behaviour as the
one observed in the previous test. Inversely, Stardog couldn't deal correctly with nested
RDF\* statements. It is visible that Stardog &lsquo;&lsquo;flattens&rsquo;&rsquo; one level
of the encapsulation as Q3 does not offer any result and `:s3 :d3 :e3` is present as a
result of Q1, instead of the expected `{:s3 :p3 :o3} :d3 :e3`.

#### Additional remarks

Despite the simplicity of the experiments, we find multiple syntax anomalies:

1. Stardog needs its own syntax based on curly-brackets (the RDF\* angular-brackets often
raise some exceptions);
2. Blazegraph cannot deal with spaces at some places and raises errors when `Select *` is
used (all the variables need to be specifically listed);
3. the ExecuteSPARQLStar tool doesn't return the subject column when there is an RDF* triple
at the subject place in the clauses. (Similarly to Blazegraph, instead of using `Select *`,
users need to specify all the variables in the `Select` clause.)

Citation
--------

This study has been published at ESWC 2020 and can be cited using the following BibTeX code:

    @article{orlandi2020countingstars,
        title={How many stars do you see in this constellation?},
        author={Fabrizio Orlandi and Damien Graux and Declan O'Sullivan},
        journal={ESWC (Poster track)},
        year={2020}
    }

