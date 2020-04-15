# Software Architectuur Document

#### Tommy Goossens - The Gram

![App Icon](images/Appicon.jpg)

##### V0.4

### Versiebeheer

| Versie | Datum      | Wijzigingen                  |
| ------ | ---------- | ---------------------------- |
| v0.1   | 27-03-2020 | Eerste opzet                 |
| v0.2   | 28-03-2020 | C1, C2 & C3 modellen         |
| v0.3   | 29-03-2020 | Omgezet naar markdown        |
| v0.4   | 15-04-2020 | Added C4 & updated C1 and C3 |

### Verspreidingen

| Versie | Datum | Ontvangers |
| ------ | ----- | ---------- |
|        |       |            |

---

### Inhoudsopgave

- [Software Architectuur Document](#software-architectuur-document)
      - [Tommy Goossens - The Gram](#tommy-goossens---the-gram)
        - [V0.4](#v04)
    - [Versiebeheer](#versiebeheer)
    - [Verspreidingen](#verspreidingen)
    - [Inhoudsopgave](#inhoudsopgave)
    - [Inleiding](#inleiding)
      - [Doel](#doel)
      - [Scope](#scope)
    - [System Context C1](#system-context-c1)
    - [C2 Containers en Technologieën](#c2-containers-en-technologieën)
    - [C3 Componenten Diagram](#c3-componenten-diagram)
      - [Feed & Post services](#feed--post-services)
      - [Profile & Chat services](#profile--chat-services)
    - [C4 Componenten en Activiteitendiagram](#c4-componenten-en-activiteitendiagram)
    - [Communicatie](#communicatie)

### Inleiding

In dit document vallen de technische details over de applicatie The Gram te lezen. The Gram is een op instagram gebaseerde iOS applicatie welke in Swift 5 geschreven wordt. De backend waar de applicatie mee communiceert is geschreven met het .Net Core 3.1 framework. Er wordt gebruik gemaakt van een microservice architectuur wat in een kubernetes omgeving gedeployed zal worden.

#### Doel

Het doel van dit document is om meer inzicht te geven in hoe The Gram in componenten opgedeeld en opgebouwd wordt. Ook valt te lezen hoe de communicatie tussen de services onderling en tussen de mobiele applicatie en de backend zal verlopen.

#### Scope

Tot de scope van dit document horen de UML modellen voor de context, containers, componenten en code. Het model van de code zal zich richten op de backend code.

### System Context C1

In het onderstaande model is het globale systeem beschreven. Twee soorten actoren zijn mogelijk: een gastgebruiker en een geregistreerde gebruiker. De gast gebruiker registreert zichzelf door middel van het Firebase Authenticatie systeem. Nadat de gastgebruiker geregistreerd is heeft deze toegang tot het software systeem van The Gram.

![C1 Model](<images/C1 System Context Diagram.png>)

### C2 Containers en Technologieën

In onderstaand model wordt het systeem uitvergroot en valt af te lezen dat er vier services draaien, naast de event bus en gateway. De API gateway wordt gerealiseerd met behulp van Ocelot. De event bus zal werken met het RabbitMQ protocol.

![C2 Model](<images/C2 Container diagram.png>)

### C3 Componenten Diagram

In dit hoofdstuk staan de modellen van de verschillende componenten die samen de complete applicatie vormen.

#### Feed & Post services

Beide services zijn gemaakt met het .Net Core framework en beschikken over een rest interface. Rest calls gaan via de gateway naar de desbetreffende controller.

De feed controller haalt alle relevante posts op voor de ingelogde gebruiker. Alleen de posts van de van de gebruikers die gevolgd worden, worden getoond.

De post controller houdt zich bezig met het verwerken van nieuwe posts, geplaatste reacties en de likes.

![C3 Feed & Post](images/C3%20Componenten%20Diagram%20Feed%20en%20Post.png)

#### Profile & Chat services

Wederom services gemaakt met het .Net Core framework en bieden ook een rest interface aan. De chat service heeft ook nog een websocket controller om de realtime chat mogelijk te maken.

De profile controller biedt de mogelijkheid om de gebruiker zijn profiel te laten wijzigen, vrienden toe te voegen en gegevens te bekijken.

De chat controller haalt de bericht geschiedenis op en de WS controller zorgt voor het real-time afleveren van berichten aan de client.

![C3 Profile & Chat services](images/C3%20Componenten%20Diagram%20Profile%20en%20Chat.png)

### C4 Componenten en Activiteitendiagram

Onderstaand het klassendiagram voor de globale applicatie. Iedere service zal zijn eigen user object bijhouden.

![C4 Class diagram](images/C4%20Class%20Diagram.png)

### Communicatie
