## Get the most frequent employers

* The property _P108 employer_ is quite frequent and adds an interesting relation to organisations that we will use for graph analysis



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

## The 30 most frequent employers

* We observe that at the top of the list are agencies that resell their information to other media. 
* In addition, another "foundation" of war journalism are the large national audiovisual groups. 
* It is also possible to note a large presence of the French press: is this a construction bias of Wikidata or a long French tradition of "great reporters"? 

|-----------------------------------------|--------------------------------------------------|-----|
| employer                                | employerLabel                                    | eff |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q40469   | Associated Press                                 |  13 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q9531    | British Broadcasting Corporation                 |   9 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q130879  | Reuters                                          |   8 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q216047  | Le Figaro                                        |   7 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q493845  | United Press International                       |   7 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q54829   | RTVE                                             |   6 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q13717   | Libération                                       |   6 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q188413  | Newsweek                                         |   5 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q9684    | The New York Times                               |   5 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q1044328 | Le Nouvel Obs                                    |   5 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q12461   | Le Monde                                         |   4 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q770596  | L'Express                                        |   4 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q525894  | France 2                                         |   4 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q532494  | New York Herald Tribune                          |   4 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q463198  | Life                                             |   4 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q610190  | Daily Express                                    |   4 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q223799  | TASS                                             |   3 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q1274521 | Agencia EFE                                      |   3 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q678095  | Paris Match                                      |   3 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q40464   | Agence France-Presse                             |   3 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q1210241 | ITN                                              |   3 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q485500  | Radio Free Europe/Radio Liberty                  |   3 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q1065    | United Nations                                   |   3 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q50008   | The Times                                        |   3 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q649695  | Islamic Republic News Agency                     |   2 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q43297   | Time                                             |   2 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q193755  | United States Geological Survey                  |   2 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q144488  | University of Warsaw                             |   2 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q4314796 | National Academy of Visual Arts and Architecture |   2 |
|-----------------------------------------|--------------------------------------------------|-----|
| http://www.wikidata.org/entity/Q1427385 | Tygodnik Powszechny                              |   2 |
|-----------------------------------------|--------------------------------------------------|-----|
