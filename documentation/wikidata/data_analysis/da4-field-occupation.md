## Inspect the most frequent fields of work

* In the case of this population, we observe that although the main fields of work are war-journalism and war-photography, there are many other fiels and it would be interesting to inspect specificities related to them.

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
        ?person_uri wdt:P31 wd:Q5; 
              wdt:P569 ?birthDate.
        BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2001)# Any instance of a human.
            {?person_uri wdt:P106 wd:Q164236}  # war journalist
            UNION
            {?person_uri wdt:P101 wd:Q17042980}     # war journalism
            UNION
            {?person_uri wdt:P106 wd:Q11496048}  # war photographer
            UNION
            {?person_uri wdt:P101 wd:Q605789}     # war photography

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

## Get the most frequent fields of work

* War journalism is a powerful literary material, and many of these reporters have dual careers as novelists or poets.

| object                                    | objectLabel                           | eff   |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q184485    | performing arts                       | 28896 |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q11190     | medicine                              | 26041 |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q309       | history                               | 24159 |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q8242      | literature                            | 15908 |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q11030     | journalism                            | 15187 |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q482       | poetry                                | 14675 |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q638       | music                                 | 13520 |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q11629     | painting                              | 13246 |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q113209507 | creative and professional writing     | 10708 |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q36649     | visual arts                           | 10404 |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q7748      | law                                   |  9670 |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q395       | mathematics                           |  9617 |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q50637     | art history                           |  9307 |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q5891      | philosophy                            |  9135 |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q7163      | politics                              |  8511 |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q8134      | economics                             |  8421 |
|-------------------------------------------|---------------------------------------|-------|
| http://www.wikidata.org/entity/Q21550668  | illustration                          |  8092 |

&nbsp;

### Query to get the data and import them into the database

```sparql

PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
SELECT DISTINCT ?person_uri ?field_uri ?field_label
WHERE
    {
    ### subquery adding the distinct clause
        {
        SELECT DISTINCT ?person_uri
        WHERE {
        ?person_uri wdt:P31 wd:Q5; 
              wdt:P569 ?birthDate.
        BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2001)# Any instance of a human.
           {?person_uri wdt:P106 wd:Q164236}  # war journalist
            UNION
            {?person_uri wdt:P101 wd:Q17042980}     # war journalism
            UNION
            {?person_uri wdt:P106 wd:Q11496048}  # war photographer
            UNION
            {?person_uri wdt:P101 wd:Q605789}     # war photography
        }
        } 
        ### The property P101 associates fields of work to persons
        ?person_uri wdt:P101 ?field_uri.
        ?field_uri rdfs:label ?field_label.
        FILTER(LANG(?field_label) = 'en')
}  

```
### Create a new table

* Download the result of this query as a CSV file
* Import it as a new table into the SQLite database
  * rename the column names during the import process 
* Inspect the imported data using the SQL scripts in the file [] 

## Occupations

```sparql
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>

SELECT ?person_uri ?occupation_label ?occupation_uri
WHERE
    {
    ### subquery adding the distinct clause
        {
        SELECT DISTINCT ?person_uri
        WHERE {
        ?person_uri wdt:P31 wd:Q5; 
              wdt:P569 ?birthDate.
        BIND(REPLACE(str(?birthDate), "(.*)([0-9]{4})(.*)", "$2") AS ?year)
        FILTER(xsd:integer(?year) > 1780 && xsd:integer(?year) < 2001)# Any instance of a human.
            {?person_uri wdt:P106 wd:Q164236}  # war journalist
            UNION
            {?person_uri wdt:P101 wd:Q17042980}     # war journalism
            UNION
            {?person_uri wdt:P106 wd:Q11496048}  # war photographer
            UNION
            {?person_uri wdt:P101 wd:Q605789}     # war photography
            }
        } 
    
        ### The property P106 associates occupations to persons
        ?person_uri wdt:P106 ?occupation_uri.
        ?occupation_uri rdfs:label ?occupation_label.
        FILTER(LANG(?occupation_label) = 'en')
}  
ORDER BY ?person_uri ?occupation_uri
```

