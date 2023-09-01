---
title: Neo4j & Cypher
tags: 
keywords: rouge, pygments, prettify, color coding,
last_updated: Sept 5, 2022
summary: ""
sidebar: mydoc_sidebar
permalink: Neo4j_2.html
folder: mydoc
---
[link to lab](https://neo4j.com/labs/) |
[Video Link](https://www.youtube.com/watch?v=h7mq1mrQIBQ) |
[Apoc video link](https://www.youtube.com/playlist?list=PL9Hl4pk2FsvXEww23lDX_owoKoqqBQpdq) |
[Edit page here](https://github.com/bhbharat/bhbharat.github.io/edit/gh-pages/pages/mydoc/course-cypher.md)

## Neo4j

**Neo4j version :** 4.7.7

**Java11 location**: C:\Program Files\Eclipse Adoptium\jre-11.0.14.9-hotspot\bin

**Java19 location** : C:\Program Files\jdk-19.0.2\bin

**Neo4j browser:** [http://localhost:7474/browser/](http://localhost:7474/browser/)

**Neo4j password :** admin, P@$$w0rd, neo4j_admin

**Neo4j commands:**

```
bin\neo4j console
bin\neo4j-admin dump --to=import\NER.dump --database=neo4j
bin\neo4j-admin load --from=import\NER.dump --database=neo4j

bin\neo4j install-service
bin\neo4j stop  
bin\neo4j start
bin\neo4j restart
```

1. [windows installation instruction (documentation page)](https://neo4j.com/docs/operations-manual/current/installation/windows/)
2. Add Neo4j plugins:

> A. Neosemantics - download jar file from [here](https://github.com/neo4j-labs/neosemantics/releases) and add to plugins directory ([README](https://github.com/neo4j-labs/neosemantics/blob/4.0/README.md)). Then restart the neo4j.
>
> B. APOC - copy apoc jar file from labs folder to plugin folder. Add following line in the neo4j.conf file :
> dbms.security.procedures.unrestricted=algo.*,apoc.*

3. Configure the loading of csv files:
   Find the neo4j.conf file for your Neo4j installation.

   > Comment this line(By adding # in the start): dbms.directories.import=import
   >
   > Uncomment this line to allow CSV import from file URL: #dbms.security.allow_csv_import_from_file_urls=true
   > Restart Neo4j
   >
4. apoc.conf file:

   apoc.import.file.enabled=true
   apoc.export.file.enabled=true
   apoc.import.file.use_neo4j_config=false
   apoc.export.file.use_neo4j_config=false
5. Neo4j conf file:
   dbms.security.allow_csv_import_from_file_urls=true
   dbms.security.procedures.unrestricted=algo.*,apoc.*,gds.*
   dbms.connector.bolt.listen_address=:7688
   dbms.connector.http.listen_address=:7475

## Using py2neo and useful commands

from py2neo import Graph
graph = Graph("bolt://localhost:7687", auth=("neo4j", "bharat123"))

graph.run("MATCH (n) DETACH DELETE n") # delete the database
graph.run("CALL apoc.schema.assert({},{},true) YIELD label, key RETURN *") # delete the indexes and constraints
graph.run("CALL n10s.graphconfig.init({handleVocabUris: 'MAP'})")  # initiate the graph config
graph.run("CREATE CONSTRAINT n10s_unique_uri ON (r:Resource) ASSERT r.uri IS UNIQUE;") # create a contraint

graph.run("CALL n10s.rdf.import.fetch('file:///C:/Users/if441f/2022_Projects/DSRM/Pre-generated ontologies/amm_22.owl', 'RDF/XML')") # load ontology

graph.run("MATCH (n)<-[r]-(n1) RETURN n,r,n1 LIMIT 10").data() ## load data

graph.run("CALL db.schema.visualization()").data()

graph.run("call db.relationshipTypes() ")

graph.run("call db.labels()")

graph.run("MATCH p=shortestPath((n:Part)-[*..3]->(n1:PartLocation))
WHERE
n.basepartname="structure"
// n.basepartname =~ ".*structure.*"
// AND n1.basepartname=~ ".*"
// AND type(r)="Contains" RETURN p")

## Cypher language (https://neo4j.com/docs/cypher-cheat-sheet/current/)

```
match (n) return n 
match (n:Part) return n 
match (n1:Part {section:41}) - [: Has] -> (n:Notes) return n.text

## Where Clause

match (n:Part) where n.section = 41 and n.page = 416 return n

## Count

match (n:Part)-[r]-(x) return type (r), count(n)

## Order by

match (n1:Part {section:41}) - [: Has] -> (n:Notes) return n1,count(n1) order by n1.section, n1.chapter

# Load CSV

LOAD CSV WITH HEADERS FROM 'https://rawcdn.githack.com/lju-lazarevic/bloom/c347841ba8474c84d2e0eb7186a5fbad036d1ca6/athlete_events.csv' AS line
WITH line WHERE line.Season='Winter'
WITH distinct line.Team as Team, line.Event as Event, line.Year as Year,line.City as City
MATCH (t:Team {name:Team + ' ' + Event + ' ' + Year})
MATCH (g:Games {name:City + ' ' + Year})
MERGE (t)-[:PARTICIPATED_IN]->(g);

## Create constraint

CREATE CONSTRAINT IF NOT EXISTS ON (a:Athlete) ASSERT a.id IS UNIQUE;

```

## OWL Ontology

 Ontologies are  used to capture knowledge about some domain of interest. An ontology describes the concepts in the domain and also the relationships that hold between those concept. OWL is a ontology language which makes it possible to describe concepts  in an unambiguous manner based on set theory and logic.

An OWL ontology consists of  Classes, Properties, and  Individuals.

- Individuals represent objects in the domain  of interest.
- Properties are binary relations between individuals.
- OWL  classes are sets that contain individuals.

## WebVOWL

Documentation - http://vowl.visualdataweb.org/webvowl.html

Video link - https://www.youtube.com/watch?v=JiGRVIQ9rks

online webVOWL - https://service.tib.eu/webvowl/

```python
#convert to json
!java -jar owl2vowl_0.3.7/owl2vowl.jar -file "amm.owl" -output "amm_22.json"
#run WebVOWL and load json
!cd WebVOWL-1.1.6 && serve deploy/
#VOWL is serving at - http://localhost:3000

```

## Owlready2

```python
from owlready2 import *
onto = get_ontology("file://C:/Users/if441f/2022_Projects/DSRM/Pre-generated ontologies/amm_22.owl").load()
onto.base_iri
```

## Protege interface

The Protégé user interface is divided up into a set of major tabs. These tabs  can be seen in the  Window>Tabs  option.

There are many  views that are not in the default version of Protégé that can be added via the  Window>Views  option. The additional  views are divided into various categories such as  Window>Views>Individual views.

- use OntoGraf for visualization
- use indivisual by class tab and indivisual by type view to visualized all indivisuals

**Using a Reasoner (pallet)**
You may notice that one or more of your classes is highlighted  in red as in Figure 4.5. This is because we  haven’t run the reasoner yet so Protégé has not been able to verify that ournew classes have no  inconsistencies.

**Disjoint Classes**
no individual can be an instance of more than one of those classes. In  set theory terminology the intersection of these three classes is the empty set:  owl:Nothing.

**OWL Properties**
OWL Properties represent relationships. There are  three  types of properties, Object properties,  Data  properties  and Annotation properties.

- Object properties are relationships between two individuals.
- Data properties are relations between an individual and a datatype such as
  xsd:string or xsd:dateTime.
- Annotation properties also usually have datatypes as values although they can have object.

**Inverse Properties**

Each object property may have a corresponding inverse property. If some property links individual  a  to  individual  b  then its inverse property will link individual  b  to individual  a.

**OWL Object Property Characteristics**

- **Functional Properties** - If a property is functional, for a given individual, there can be at most  one individual that is related to  the  individual via the property
- **Inverse Functional Properties** - If a property is inverse functional then it means that the  inverse  property is functional.
- **Transitive Properties** - If a property  P  is transitive, and  P  relates individual a toindividual b, and also individual b to  individual c, then we can infer that individual a is related to individual c via property P
- **Symmetric  and Asymmetric  Properties**  - If a property P is symmetric, and the property relates individual a to individual b then individual b is  also  related to individual a via property P.
- **Reflexive and Irreflexive Properties** - A reflexive property is a property that always relates an individual to itself. If a property P is reflexive  then for all individuals  a P will always relate a to a.

## Property restrictions

We will initially use quantifier restrictions.  Quantifier restrictions can be further categorized as  existential  restrictions and  universal  restrictions6.

- **Existential restrictions** describe classes of individuals that participate in at least one relation along  a specified property.  For example, the class of individuals who have at least one (or some)  hasTopping  relation to instances of  VegetableTopping. In OWL the keyword  **some**  is used  to denote existential restrictions.
- **Universal restrictions** describe classes of individuals that for a given property  only  have relations  along a property to individuals that are members of a specific class. For example, the class of  individuals that only have  hasTopping  relations to instances of the class  VegetableTopping.  In OWL they keyword  **only**  is used for universal restrictions.

**Primitive and Defined Classes (Necessary and Sufficient Axioms)**

Necessary  axioms can be read as,  If something is a member of this class then it is necessary to fulfil these  conditions. With necessary axioms  alone (primitive class), we  cannot say that:  If something fulfils these conditions then it  must be a member of this class.
