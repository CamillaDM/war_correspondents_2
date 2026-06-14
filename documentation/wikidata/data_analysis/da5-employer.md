## Get the most frequent employers

* The property _P108 employer_ is quite frequent and adds an interesting relation to organisations that we will use for graph analysis
* See [this document](../explore-employer.md) for a distribution of the most frequent employers


## Get the relations to the employers
```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
SELECT ?employer ?employerLabel (COUNT(*) as ?eff)
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

      ?item wdt:P108 ?employer.
        ?employer rdfs:label ?employerLabel.
        FILTER(LANG(?employerLabel) = 'en')
       
}  
GROUP BY ?employer ?employerLabel 
ORDER BY DESC(?eff)
LIMIT 30
```
* execute the query on Wikidata or [QLever](https://qlever.dev/wikidata)
* download the result as a CSV file
* import the file as a 'import_person_employer' table into the database using DBeaver

## Get the list of the organisations

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
SELECT DISTINCT ?organisation_uri ?organisation_label 
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
           {?item wdt:P106 wd:Q164236}  # war journalist
            UNION
            {?item wdt:P101 wd:Q17042980}  # war journalism
            UNION
            {?item wdt:P106 wd:Q11496048}  # war photographer
            UNION
            {?item wdt:P101 wd:Q605789}     # war photography
            }
        } 
        
        ?person_uri wdt:P108 ?organisation_uri.
        ?organisation_uri rdfs:label ?organisation_label.
        FILTER(LANG(?organisation_label) = 'en')
}  
# LIMIT 30
```
* execute the query on Wikidata or [QLever](https://qlever.dev/wikidata)
* download the result as a CSV file
* import the file as an 'import_organisation' table into the database using DBeaver
  

## Get the list of the organisations' classes

```
PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#>
PREFIX xsd: <http://www.w3.org/2001/XMLSchema#>
PREFIX wd: <http://www.wikidata.org/entity/>
PREFIX wdt: <http://www.wikidata.org/prop/direct/>
SELECT DISTINCT ?organisation_uri  ?organisation_class_uri ?organisation_class_label
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
            {?item wdt:P106 wd:Q164236}  # war journalist
            UNION
            {?item wdt:P101 wd:Q17042980}     # war journalism
            UNION
            {?item wdt:P106 wd:Q11496048}  # war photographer
            UNION
            {?item wdt:P101 wd:Q605789}     # war photography
            }
        } 
        
        ?person_uri wdt:P108 ?organisation_uri.
        ?organisation_uri wdt:P31 ?organisation_class_uri.
        ?organisation_class_uri rdfs:label ?organisation_class_label.
        FILTER(LANG(?organisation_class_label) = 'en')

}  
LIMIT 30
```
* execute the query on Wikidata or [QLever](https://qlever.dev/wikidata)
* download the result as a CSV file: import_organisations_and_classes.csv
* import the file as an 'import_organisations_classes' table into the database using DBeaver