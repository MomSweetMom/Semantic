### Q1.
```
CONSTRUCT { ?s ?p ?o } WHERE { ?s ?p ?o }
```

### Q2.
```
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#>
select * where { ?x a ?t . filter(strstarts(?t, h:)) }
```
The query finds all the link 3-triples which has the 3rd triple start with h, a predefined namespace.
We have in total 21 results.
```
prefix john:<http://www.inria.fr/2007/09/11/humans.rdfs-instances#John>
select * where { ?x a ?t . filter(strstarts(?x, john:)) }
```
John is a person


### Q3.
```
select ?property ?object where {
<http://www.inria.fr/2007/09/11/humans.rdfs-instances#John> ?property ?object .
 }
```

### Q4.
```
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#>
select * where { ?x h:hasSpouse ?y }
```
The query find any Person who has property hasSpouse

### Q5.
0. Find all the property
```
select distinct ?p where {
   ?x ?p ?o
}
```

1. All the person with their shoes size
```
select ?person ?object where {
	?person <http://www.inria.fr/2007/09/11/humans.rdfs#shoesize> ?object
}
```

2. All the person and if available, their shose size
```
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#>
select distinct ?person ?shoesize where {
    ?person ?property ?object .
    filter(strstarts(?object, h:)) .
    OPTIONAL {
    	?person <http://www.inria.fr/2007/09/11/humans.rdfs#shoesize> ?shoesize
    }
}
```

3. All person whose shoesize is greater than 8 or whose shirt size is greater than 12
```
select ?person where {
	?person <http://www.inria.fr/2007/09/11/humans.rdfs#shoesize> ?shoesize .
	?person <http://www.inria.fr/2007/09/11/humans.rdfs#shirtsize> ?shirtsize .
	filter (?shoesize > 8 && ?shirtsize >12)
}
```
It's William

### Q6.
```
select ?person ?spouse ?child where {
 	?person <http://www.inria.fr/2007/09/11/humans.rdfs#hasChild> ?child .
	OPTIONAL {
	?person <http://www.inria.fr/2007/09/11/humans.rdfs#hasSpouse> ?spouse
 	}
}
```
We can see the couple Gaston and Flora has 3 childs
Query for men who have no children
```
prefix per: <http://www.inria.fr/2007/09/11/humans.rdfs-instances#>
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#>
select distinct ?person where {
  {?person a h:Man}
  minus {?person h:hasChild ?o}
}
```

### Q7.
1. People who are not an adult: There are 6 of them
```
prefix per: <http://www.inria.fr/2007/09/11/humans.rdfs-instances#>
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#>
select ?person ?o where {
	?person h:age ?o .
	filter(?o > 18)
}
```

2. Ask whether Mark is an adult
```
prefix per: <http://www.inria.fr/2007/09/11/humans.rdfs-instances#>
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#>
ASK {
	per:Mark h:age ?o .
	filter(?o > 18)
}
```

3. Indicate age of each person: even or not
(I have no idea how to implement modulo operation in sparql, even after refering to W3C document)
```
prefix human: <http://www.inria.fr/2007/09/11/humans.rdfs-instances#>
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#>
select ?person ?o ?status where {
    ?person h:age ?o .
    BIND( exists
        {  ?person h:age ?o
 	          . filter(?o mode 2 = 0)
	    } as ?status)
}
```

### Q8.
Construct a symmetric hasFriend
```
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#>
construct {	?person h:hasFriend ?friend .
}
where {	{?person h:hasFriend ?friend}
        UNION
        {?person ^h:hasFriend ?friend}
     }
```
Using insert query
```
prefix human: <http://www.inria.fr/2007/09/11/humans.rdfs-instances#>
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#>
insert
{
	?person h:hasFriend ?o
}
where {
	?person ^h:hasFriend ?o
}
```


### Q9.
Visualization
```
construct {?person ?spouse ?child} where {
    ?person <http://www.inria.fr/2007/09/11/humans.rdfs#hasChild> ?child .
    OPTIONAL {
    ?person <http://www.inria.fr/2007/09/11/humans.rdfs#hasSpouse> ?spouse
    }
}
```

### Q10.
human_modified.rdf created with my name Trung
```
prefix human: <http://www.inria.fr/2007/09/11/humans.rdfs-instances#>
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#>
select * where {
   human:Trung ?p ?o .
}
```

### Q11.
1. Person with the same shoesize
```
prefix human: <http://www.inria.fr/2007/09/11/humans.rdfs-instances#>
prefix h: <http://www.inria.fr/2007/09/11/humans.rdfs#>

select (group_concat (?person; separator =';') as ?p) ?o where {
	?person h:shirtsize ?o .
} group by ?o
```
