[Terug naar het PDR](../../PersonalDevelopmentReport.md)

# Case-study schaalbaarheid van databases

## Inhoudsopgave

## Aanleiding

Wanneer er veel gelijktijdige gebruikers zijn zullen mijn services mooi meeschalen dankzij de kubernetes omgeving, maar hoe zit dit met mijn databases?
De huidige structuur:

- Profile service: SQL database met users en followers
- Chat service: SQL database met chatberichten per userid met de username
- Post service: SQL database met posts en een user id & username, comments en likes
- Feed service: SQL database met followers per userId

De aanleiding van het onderzoek is dat ik wil vaststellen of mijn huidige structuur de juiste is. Als dit het geval is wil ik vastellen hoe mijn databases mee schalen met de requests en anders wat dan wel de juiste databasestructuur is.

## Deelvragen

Om tot de juiste antwoorden te komen, is het onderzoek onderverdeeld in de volgende deelvragen:

1. Welke database structuren zijn er?
2. Op welke manieren zijn databases te schalen?

## Resultaten

### 1. Welke database structuren zijn er?

Databases kunnen we onderverdelen in twee categoriën: Relationele databases en niet-relationele databases

#### Relationele databases (SQL)

Relationele databases zijn het meest bekend en worden nog steeds het meest gebruikt. Relationele databases zijn in de jaren 70 ontstaan om data op te slaan aan de hand van een database-schema wat het mogelijk maakt om deze data weer te geven in de vorm van tabellen. Een relationele database kun je dus zien als een grote collectie van tabellen met elk zijn eigen database-schema dat de attributen en data types bevat. Alle relationele databases bieden de standaard create/read/update/delete (CRUD) functionaliteit wat aangeroepen kan worden met Structured Query Language (SQL) statements.

In de tabellen krijgt elke entry een unieke `key`, deze wordt gebruikt om specifieke kolommen of rijen te identificeren en bieden snelle toegang tot deze entries.

Data integriteit is een belangrijke kernwaarde van relationele databases, en door middel van constraints wordt er gezorgd dat de data in de tabellen betrouwbaar en accuraat is. Er zijn veel varianten van relationele databases en dit zijn de meest populaire:

- Oracle: Oracle Database is een multi-model database systeem gemaakt door Oracle
- MySQL: Een open-source relationele database gebaseerd op SQL. Draait op alle platformen
- Microsoft SQL Server: Database dat veel business intelligence en analytics ondersteund in de IT wereld
- PostgreSQL: Object-relationele database met een nadruk op uitbreidbaarheid en standaardisering
- DB2: Ontworpen om data efficiënt op te slaan, analyseren en op te halen

Wat zijn de voordelen van een relationele database:

- Relationele databases zijn erg volwassen en doorontwikkeld.
- SQL standaarden zijn goed gedefinieerd en geaccepteerd
- Alle relationele databases werken volgens de `ACID` regels wat inhoudt dat ze de requirements van `Atomic` (de mate waarin gegarandeerd kan worden dat een transactie ofwel geheel wordt uitgevoerd of geheel nietig is), `Consistency` (Een transactie creëert of een nieuwe geldige staat, of herstelt de voorgaande staat), `Isolated` (transacties worden geïsoleerd van elkaar uitgevoerd), `Durable` (Een voltooide transactie kan niet later ongeldig gemaakt worden).

Nadelen van relationele databases:

- Ze werken niet goed, of totaal niet, met ongestructureerde / semi-gestructureerde data dankzij de database-schema's en gedwongen types.
- De tabellen worden niet per se een-op-een gemapped naar objecten of classes die dezelfde data representeren
- Om het migreren van de ene database naar de andere te laten slagen, moeten de database-schema's en types identiek zijn in de doel database.
- Kunnen niet horizontaal geschaald worden, alleen verticaal (wat duur is)

#### Niet-relationele databases (NoSQL)

Waar relationele databases erg gestructureerd zijn en duidelijk gedefinieerd moet worden welke data types er in welke kolom komen te staan, is dat bij een NoSQL database niet het geval. NoSQL databases maken het mogelijk om ongestructureerde en semi-gestructureerde data op te slaan en deze te manipuleren.

Er zijn een aantal types NoSQL databases:

##### Key-Value stores

Simpele database management systemen welke alleen de key-value pairs opslaan en basic functionaliteit bieden om deze values op basis van keys op te halen. Deze simpliciteit zorgt ervoor dat data wat niet bijzonder complex is, supersnel opgehaald kan worden.

Aanbieders: Redis en Amazon DynamoDB

##### Wide Column Stores

Database-schema-agnostische systemen dat gebruikers data op laat slaan in `column families` of tabellen, een soort multi-dimensionale key-value store.

Dit type is ontworpen met schaalbaarheid als doel. Ze maken het mogelijk om petabytes aan data te onderhouden verdeeld of duizenden servers. Alhoewel ze technisch gezien geen database-schema hebben, bieden ze een SQL-achtige taal genaamd CQL om data manipulatie gemakkelijk te maken.

Aanbieders: Cassandra, Scylla en HBase

##### Document Stores

Database-schema-vrije system welke data opslaan in de vorm van JSON documenten. De manier van opslag lijkt op de key-value en wide column stores, maar bij document stores is de naam van het document de key en alles wat er in het document zit de value.

In een document store is er geen uniforme structuur en kan veel verschillende value types bevatten welke meerdere lagen diep kunnen gaan. Deze flexibiliteit maken document stores erg geschikt om semi-gestructureerde data op te slaan welke over meerdere systemen verspreid zijn.

Aanbieders: MongoDB en Couchbase

##### Graph Databases

Graph databases representeren data als een netwerk van gerelateerde nodes of objecten om zo data visualisaties en analytics mogelijk te maken. Een node of een object bevat `free-form` data wat verbonden is met relaties en gegroepeerd aan de hand van labels. Graph-Oriented database management systemen zijn ontworpen met de nadruk op het illustreren van de verbindingen tussen data punten.

Dat maakt dit type erg geschikt om relaties tussen data punten te analyseren, bijvoorbeeld om fraude te voorkomen of Facebook's vrienden graph.

Aanbieders: Neo4J en Datastax Enterprise Graph

##### Search Engines

Slaan data op met database-schema-vrije JSON documenten. Lijken op document stores, maar met een grotere nadruk op het toegankelijk maken van gestructureerde en semi-gestructureerde data via text-based searches.

Aanbieders: Elasticsearch, Splunk en Solr

Voordelen:

- Bieden een grote flexibiliteit dankzij de database-schema-vrije manier van data opslag
- NoSQL schalen makkelijk horizontaal bij en hebben een hogere fouttolerantie. Ze blijven dus goed werken wanneer er een storing in een component is
- Data kan makkelijk over meerdere nodes worden gedistribueerd. Om beschikbaarheid en partitie tolerantie te verbeteren kan er voor `eventual consistency` gekozen worden

Nadelen:

- NoSQL databases zijn op minder grote schaal toegepast en minder volwassen dan de SQL varianten.
- Er is geen standaard voor alle NoSQL types.

### 2. Op welke manieren zijn databases te schalen?

Wanneer honderden, duizenden of zelfs miljoenen gebruikers tegelijk hun berichten / reacties plaatsen of chat berichten versturen, wil je garanderen dat alle verstuurde data opgeslagen correct opgeslagen wordt, zonder dat gebruikers moeten wachten op de acties van anderen. Je wil niet hebben dat gebruiker 1 geen reactie kan plaatsen, omdat de database bezig is met het verwerken van een chat bericht. Hoe kan je dit probleem tegengaan?

Er zijn een aantal ontwikkel patronen die hierbij kunnen helpen.

#### 1. Query optimization & Connection Pool

De eerste oplossing is om veelvoorkomende niet-dynamische data in de cache op te slaan. Maar naast deze application-layer cache kun je niks aan de latency problemen doen die dynamische data opvragen. Dus is het mogelijk om overtollige kolommen (welke vaak in `where` of `join on` queries voorkomen) toe toevoegen aan tabellen die vaak aangesproken worden om de-normalisatie te creëeren. Dit zorgt er voor dat er minder join queries nodig zijn, grote queries opgebroken kunnen worden in meerdere kleinere queries en dat de resultaten van deze queries in de application layer worden opgeteld.

Een andere optimalisatie is het tweaken van database connecties. Dankzij een connection pool kunnen er database connecties gecached worden. Elke netwerkconnectie is namelijk kostbaar, dus met een connection pool kun je deze connecties optimaliseren. Dankzij connection pool libraries is het ook mogelijk om deze cache over meerdere threads te delen, maar niet over instanties.

Op basis van 20-50 requests per minuut zullen deze optimalisaties tussen de 20% en 50% van de latency problemen verhelpen.

#### 2. Vertical Scaling

Verticaal schalen is het vergroten van de capaciteiten van de machine waarop de applicatie draait. Dit wordt gedaan door het RAM geheugen en opslag geheugen te vergroten en een betere CPU te installeren.

Verticaal schalen gebeurt door een nieuwe betere machine als `slave` in te stellen van de bestaande machine `de master`. Een `master slave` configuratie wordt toegepast en de data replicatie begint. Wanneer het repliceren van de data voltooid is, dient de nieuwe machine naar master gepromoot te worden en kan de oudere machine offline.

Dit is niet de beste manier om te schalen, aangezien het ontzettend duur is en je zal snel tegen de limieten aanlopen daar je niet onbeperkt veel RAM of betere CPU's kan installeren

#### 3. Command Query Responsibility Segregation (CQRS)

Doordat over het algemeen een write transactie zwaarder is dan een read transactie, is CQRS een goede manier om de latency te verbeteren. Er kunnen `slave` machines bijgeplaatst worden welke dezelfde data bevatten als de `master` dankzij de master-slave replicatie. Alle read transacties (de Q uit CQRS) worden naar de slaves gestuurd en de write transacties (de C uit CQRS) naar de master. Er kan een kleine vertraging in de replicatie zitten, maar voor mijn business case van Posts / Comments / Likes / Chat messages is dat prima.

#### 4. Multi Master Replication

Wanneer de master meer write performance nodig heeft, is het mogelijk om wat in te leveren in de read performance door ook de write requests naar een slave te sturen.

In een `multi-master` configuratie werken alle machines zowel als een master als een slave. Je kan de configuratie zien als een cirkel `A->B->C->D->A`. B repliceert data van A, C van B enz. Tijdens het schrijven kan je elke node aanspreken, en bij het lezen wordt de query naar alle nodes gestuurd. Degene die het eerst de query ontvangt stuurt de response. Alle nodes hebben hetzelfde database-schema, tabellen, index etc. Er moet gezorgd worden dat er geen id overlapping is, anders zouden de nodes voor hetzelfde id andere data kunnen terugsturen.

#### 5. Partitioning

Wanneer je erachter komt dat een bepaalde tabel veel meer verkeer krijgt dan andere tabellen is het mogelijk om deze tabel te scheiden van de rest. Dit is geen probleem als deze tabel geen / weinig referentie(s) heeft naar andere tabellen. Dit noemen ze partitionering. Verschillende databases kunnen andere data hosten op basis van andere functionaliteit, wanneer de resultaten in de application-layer samengevoegd kunnen worden. Door deze techniek toe te passen kan je je focussen op het schalen van de functionaliteit met hoge read/write requests. Aangezien de application-layer er voor moet zorgen dat de data samengevoegd wordt, kost dit wel meer code aanpassingen.

#### 6. Horizontaal schalen

Waar je bij verticaal schalen de capaciteit van de machine vergroot, plaats je bij horizontaal schalen juist machines bij. Elke machine bevat hetzelfde database-schema en dezelfde set aan tabellen. Elke machine houdt een deel van de data bij.

Aangezien alle databases dezelfde tabellen bevatten, is het mogelijk om alle gerelateerde data in dezelfde machine wordt opgeslagen. Elke machine houdt zijn eigen replica bij, welke gebruikt kan worden wanneer de machine crasht. Zo'n database noemen ze een `shard`. Een fysieke machine kan meerdere shards bevatten, het is aan de ontwerper om de inrichting te bepalen. `Sharding keys` worden gebruikt om ervoor te zorgen dat elke sharding key naar dezelfde machine refereert. Dus met horizontaal schalen zorgen een hele hoop machines dat ze zelf alle gerelateerde data in dezelfde set tabellen bijhouden, en alle requests voor dezelfde row of resource set worden naar dezelfde database machine gestuurd.

Helaas ondersteunen monolitische databases zoals Oracle, PostgreSQL en MySQL geen automatische sharding. Dus zal je handmatig moeten sharden wat het ontwikkelen ineens een stuk lastiger maakt.

Wanneer er gebruik wordt gemaakt van een NoSQL database is horizontaal schalen een stuk makkelijker, aangezien elk document een self-contained object is. Een document kan dus op compleet andere machines staan zonder zich druk te maken over het joinen van meerdere rijen of tabellen.

#### 7. Data Centre Wise Partition

Alle bovenstaande stappen / patterns werken uitstekend wanneer alle requests vanuit hetzelfde land komen, maar wanneer de requests vanuit een ander land / continent komen zal de latency ook gaan groeien.

Om deze latency in te perken zullen er meerdere data centers komen, die verspreid zijn over de continenten. Er kan bijvoorbeeld een data center in Duitsland komen voor alle requests vanuit Europa, eentje in California voor alle USA requests etc. Cross data center replicatie zorgt ervoor dat het herstellen van een storing makkelijk te overkomen is. Wanneer het data center in Duitsland al zijn data repliceert naar California, en Duitsland valt uit, dan kunnen alle requests doorgestuurd worden naar California. De gebruiker zal wat latency ervaren, maar alles blijft werken.

## Conclusie

Er zijn dus twee types databases: Relationele en niet-relationele databases. Elk type heeft zijn eigen voordelen, maar de belangrijkste verschillen zijn dat een relationele database slecht horizontaal schaalt, maar dat dankzij de ACID regels de data erg betrouwbaar is. Een niet-relationele database schaalt dan horizontaal weer erg goed, maar heeft een hogere fout tolerantie.

Er zijn veel manieren om de latency te verlagen / load te verdelen voor de database. Vooral het partitioneren, verticaal schalen en het partitioneren van data centers zijn het effectiefst wanneer de hoeveelheid requests significante nummers bereiken.

Hoe vertaald dit zich naar mijn applicatie, welke nu dus deze structuur heeft:

- Profile service: SQL database met users en followers
- Chat service: SQL database met chat berichten per userid met de username
- Post service: SQL database met posts en een user id & username, comments en likes
- Feed service: SQL database met followers per userId

Zoals af te lezen is, pas ik momenteel al partitionering toe. Elke service heeft zijn eigen database met relevante informatie. Door middel van een event based architectuur valt data op te halen bij de andere services. Daarnaast pas ik CQRS al toe met het MediatR pattern. Dit zal niet genoeg zijn om duizenden tot miljoenen requests te verwerken, dus zal ik horizontaal moeten schalen en naar andere landen uitbreiden (theoretisch gezien). Zoals gezegd schalen relationele databases niet makkelijk horizontaal en dit kan bij de Post service wel nodig zijn. Zeker als er veel gelijktijdige nieuwe posts, comments en likes binnen komen. Het lijkt er dan op dat het beter is om een NoSQL database te gebruiken voor deze service, waar elke post een document is, welke weer comments en likes bevat.

Dus wat is er nodig om mijn applicatie goed mee te laten schalen met de requests: De database van de post service omzetten naar een NoSQL database (MongoDB) om horizontaal schalen goed mogelijk te maken bij de service die dat het meest nodig gaat hebben.
