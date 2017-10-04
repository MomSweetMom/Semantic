### Q1. Declare hasSpouse and hasFriend are symmetrical Properties
```
<owl:SymmetricProperty rdf:ID="hasSpouse" />
<owl:SymmetricProperty rdf:ID="hasFriend" />
```

### Q2. Declare hasAncestor as transitive property.
```
<owl:TransitiveProperty rd:ID="hasAncestor" />
```

### Q3. Declare that hasChild is the inverse hasParent property.
```
<rdf:Property rdf:ID="hasChild">
    <owl:inverseOf rdf:resource="#hasParent" />
</rdf:Property>
```

### Q4. Declare the disconnection between Male and Female.
```
<owl:Class rdf:ID="Male">
    <owl:disjointWith rdf:resource="#Female" />
</owl:Class>
```

### Q5. Declare that the class Professor is the intersection of the class Lecturer and Researcher class.
```
<owl:Class rdf:ID="Professor" />
    <owl:intersectionOf rdf:parseType="Collection">
        <owl:Class rdf:about="#Lecturer" />
        <owl:Class rdf:about="#Researcher" />
    </owl:unionOf>
</owl:Class>
```

### Q6. Declare that the Academic class is the union of classes Lecturer and Researcher.
```
<owl:Class rdf:ID="Academic" />
    <owl:intersectionOf rdf:parseType="Collection">
        <owl:Class rdf:about="#Lecturer" />
            <owl:Class rdf:about="#Researcher" />
    </owl:unionOf>
</owl:Class>
```

### Q7. Use two restrictions to declare that any person married to a man is a woman and vice versa.
```
<owl:Class rdf:ID="Male">
    <owl:equivalentClass>
        <owl:Restriction>
            <owl:hasSpouse rdf:resource="#Woman">
        </owl:Restriction>
    </owl:equivalentClass>
</owl:Class>

<owl:Class rdf:ID="Female">
    <owl:equivalentClass>
        <owl:Restriction>
            <owl:hasSpouse rdf:resource="#Man">
        </owl:Restriction>
    </owl:equivalentClass>
</owl:Class>

```

### Q8. Use a restriction to declare that any person must have a parent who is a woman.
```
<owl:Class rdf:ID="Person">
    <owl:equivalentClass>
        <owl:Restriction>
            <owl:hasParent rdf:resource="#Woman">
        </owl:Restriction>
    </owl:equivalentClass>
</owl:Class>
```
