#### About the human.rdfs schema

### Q1. human.rdfs

### Q2. http://www.inria.fr/2007/09/11/humans.rdfs is the namespace associated by xml:base

### Q3. Look at the XML structure of this file and locate different syntactic properties: the different possible uses of the markup (ex: opening tag and closing, single tag), the use of namespaces for qualified names, the use of entities, etc

### Q4. Locate the use of the terms of the RDF (S) language: Class, Property, label, how, range, domain, subClassOf, subPropertyOf, etc

### Q5. All classes can have the age property

### Q6. Refer to Graph.png

#### Query the schema

### Q1. Find all classes
```
prefix rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> prefix rdfs: <http://www.w3.org/2000/01/rdf-schema#>

select ?x where {
    ?x a rdfs:Class
}
```

### Q2. Find all the links subClassOf
```
select ?x where {
    ?x rdfs:subClassOf ?y
}
```

### Q3. Find the definitions and translations of "shoe size"
```
select ?l ?c where {
    ?t rdfs:label shoesize "shoe.size"@en .
    ?t rdfs:label ?l .
    ?t rdfs:comment ?c
}
```

#### Query data augmented by an RDFS schema

### Q1. Load schema (.rdfs)
The links are appeared between Man & Person, Woman & Person

### Q2.
```
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#>
select * where {
    ?x a h:Male
{ ?x h:hasSpouse ?y .}
UNION
{ ?y h:hasSpouse ?x .}
}
```
The property `hasFather` has range `Male`, so if X `hasFather` Y, Y becomes Male.

### Q3.
We get 7 answers (including only 2 people, Eve and Laura)
Laura is a `Person` and `Animal` because `Lecturer` is a `subClass` of `Person`.
Jack is defined as `Man`, which is a `subClass` of `Male` and `Person`

### Q4.
```
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#>
select * where {
    ?x h:hasAncestor ?y
}
```
`hasFather` and `hasMother` are `subProperties` of `hasParent`, which is `subProperty` of `hasAncestor`

### Q5.
```
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#>

select * where {
{ ?x h:hasSpouse ?y .}
UNION
{ ?y h:hasSpouse ?x .}
UNION
{ ?x h:hasChild ?z .}
UNION
{ ?z h:hasParent ?x .}
}
```
