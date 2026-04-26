## Get the most frequent occupations

* In the case of this population, we observe that the main occupations are the same that the ones used to define the population. The information appears to be less relevant to answer research questions.

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>

SELECT ?object ?objectLabel (COUNT(*) as ?eff)
WHERE
    {
    ### subquery adding the distinct clause
        {
        SELECT DISTINCT ?item
        WHERE {
        ?item wdt:P31 wd:Q5; 
              wdt:P569 ?birthDate.
        BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2001)# Any instance of a human.
              {?item wdt:P106 wd:Q164236}  # war journalist
            UNION
            {?item wdt:P101 wd:Q17042980}     # war journalism
            UNION
            {?item wdt:P106 wd:Q11496048}  # war photographer
            UNION
            {?item wdt:P101 wd:Q605789}     # war photography
            }
        } 
	
        ### The property P106 associates occupations to persons
        # we call here the target variable ?object 
        # in order to more easily reuse the query. 
        # ?occupation would be also a good name for the variable
        ?item wdt:P106 ?object.
        ?object rdfs:label ?objectLabel.
        FILTER(LANG(?objectLabel) = 'en')
}  
GROUP BY ?object ?objectLabel 
ORDER BY DESC(?eff)
LIMIT 10
```

### Most frequent occupations

|------------------------------------------|-------------------|-----|
| object                                   | objectLabel       | eff |
|------------------------------------------|-------------------|-----|
| http://www.wikidata.org/entity/Q164236   | war correspondent | 784 |
|------------------------------------------|-------------------|-----|
| http://www.wikidata.org/entity/Q1930187  | journalist        | 686 |
|------------------------------------------|-------------------|-----|
| http://www.wikidata.org/entity/Q36180    | writer            | 334 |
|------------------------------------------|-------------------|-----|
| http://www.wikidata.org/entity/Q11496048 | war photographer  | 324 |
|------------------------------------------|-------------------|-----|
| http://www.wikidata.org/entity/Q33231    | photographer      | 291 |
|------------------------------------------|-------------------|-----|
| http://www.wikidata.org/entity/Q957729   | photojournalist   | 170 |
|------------------------------------------|-------------------|-----|
| http://www.wikidata.org/entity/Q49757    | poet              |  67 |
|------------------------------------------|-------------------|-----|
| http://www.wikidata.org/entity/Q28389    | screenwriter      |  60 |
|------------------------------------------|-------------------|-----|
| http://www.wikidata.org/entity/Q6625963  | novelist          |  50 |
|------------------------------------------|-------------------|-----|
| http://www.wikidata.org/entity/Q2526255  | film director     |  46 |
|------------------------------------------|-------------------|-----|