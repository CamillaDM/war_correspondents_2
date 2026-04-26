## Get the most frequent fields of work

* War journalism is a powerful literary material, and many of these reporters have dual careers as novelists or poets.
* The fact that journalism (168) is before war journalism (143) makes sense: war journalism is a specialization. This shows that most of these individuals had a broader journalistic career before or after covering conflicts.
* Are the employees of the Associated Press more "photojournalists", while those of Le Figaro or Le Monde are more interested in "literature" and "opinion journalism"? This would reveal the corporate cultures of these media.

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
	
        ### The property P101 associates fields of work to persons
        ?item wdt:P101 ?object.
        ?object rdfs:label ?objectLabel.
        FILTER(LANG(?objectLabel) = 'en') 
     }
GROUP BY ?object ?objectLabel 
ORDER BY DESC(?eff)
LIMIT 100
```


|-----------|--------------|-----------------------------------|-----|
| 		      ?object      | ?objectLabel                      | ?eff|
|-----------|--------------|-----------------------------------|-----|
| 1	        | Q11030       | journalism                        | 168 |
|-----------|--------------|-----------------------------------|-----|
| 2	        | Q17042980    | war journalism                    | 143 |
|-----------|--------------|-----------------------------------|-----|
| 3	        | Q113209507   | creative and professional writing |  51 |
|-----------|--------------|-----------------------------------|-----|
| 4	        | Q11633       | photography                       |  33 |
|-----------|--------------|-----------------------------------|-----|
| 5	        | Q8242        | literature                        |  25 |
|-----------|--------------|-----------------------------------|-----|
| 6	        | Q156035      | opinion journalism                |  25 |
|-----------|--------------|-----------------------------------|-----|
| 7	        | Q482         | poetry                            |  18 |
|-----------|--------------|-----------------------------------|-----|
| 8	        | Q676         | prose                             |  17 |
|-----------|--------------|-----------------------------------|-----|
| 9	        | Q115160290   | literary activity                 |  16 |
|-----------|--------------|-----------------------------------|-----|
| 10	    | Q506858      | photojournalism                   |  15 |
|-----------|--------------|-----------------------------------|-----|