# The Gram Documentation

## Versiebeheer

| Versie | Datum      | Wijzigingen                                      |
| ------ | ---------- | ------------------------------------------------ |
| v0.1   | 27-03-2020 | Eerste opzet github pages                        |
| v0.2   | 27-03-2020 | Jira link toegevoegd                             |
| v0.3   | 10-04-2020 | Analyse document toegevoegd                      |
| v0.4   | 13-06-2020 | Case study DDD & onderzoek websockets toegevoegd |

---

## Inhoudsopgave

- [The Gram Documentation](#the-gram-documentation)
  - [Versiebeheer](#versiebeheer)
  - [Inhoudsopgave](#inhoudsopgave)
  - [Inleiding](#inleiding)
  - [Git repositories](#git-repositories)
  - [Documentatie](#documentatie)
    - [Analyse document](#analyse-document)
    - [Software Architectuur Document (SAD)](#software-architectuur-document-sad)
    - [Personal Development Report (PDR)](#personal-development-report-pdr)
    - [Research](#research)
      - [Websockets](#websockets)
      - [Emerging trends case study](#emerging-trends-case-study)

## Inleiding

Op deze centrale plek zal alle documentatie omtrent mijn persoonlijke project "The Gram" waar ik in semester 6 aan het Fontys aan werk. Het is een op instagram gebaseerde applicatie met een SwiftUI client en een .Net Core 3.1 backend. Er wordt gebruik gemaakt van een microservice architectuur welke gedeployed wordt in een kubernets cluster.

De progressie van het project valt te volgen via deze [link](https://s62-1.atlassian.net/jira/software/projects/THEGRAM/boards/5/backlog) naar Jira.

## Git repositories

In dit hoofdstuk is een overzicht te vinden van alle gerelateerde git repositories:

- [Client](https://github.com/TommyGoossens/the-gram-client)
- [Post service](https://github.com/TommyGoossens/the-gram-post)
- [Feed service](https://github.com/TommyGoossens/the-gram-feed)
- [Profile service](https://github.com/TommyGoossens/the-gram-profile)
- [Chat service](https://github.com/TommyGoossens/the-gram-chat)
- [Event bus](https://github.com/TommyGoossens/the-gram-eventbus)
- [API Gateway](https://github.com/TommyGoossens/the-gram-gateway)

## Documentatie

In onderstaande hoofdstukken is alle documentatie terug te vinden die tot dit project behoort.

### Analyse document

In het analyse document staan de functional en non-functional requirements beschreven. Daarnaast zijn ook de schetsen van de iOS applicatie hier te vinden. Dit document geeft een inzicht in de functionaliteit van het systeem.

Het document is hier te vinden [AnalyseDocument](AnalysisDocument.md)

### Software Architectuur Document (SAD)

In het SAD zijn de C1, C2, C3 en C4 modellen te vinden. Het document biedt inzicht in hoe de structuur van mijn applicatie eruit ziet en welke technologie keuzes er gemaakt zijn.

- C1 Systeem Context: Geeft een globale indruk van welke systemen er gebruikt worden
- C2 Containers & Technologieën: Duikt dieper in op het systeem en toont aan welke sub onderdelen er in de applicatie zitten
- C3 Componenten Diagram: Geeft de componenten per container weer
- C4 Code: Per component een class & activity diagram

Het document is hier te vinden: [SAD](SoftwareArchitectuurDocument.md)

### Personal Development Report (PDR)

In het PDR staan mijn ontwikkelingspunten en zal ik aangeven op welk niveau ik zit, wat mij tot dit niveau gebracht heeft en gemaakte keuzes beargumenteren. Het is een levend document en zal dus van tijd tot tijd geupdate worden.

Het document is hier te vinden: [PDR](PersonalDevelopmentReport.md)

### Research

#### Websockets

Ik heb onderzoek gedaan naar de schaalbaarheid van websockets. Het onderzoek is via onderstaande link te lezen.
[Onderzoek schaalbaarheid websockets](onderzoek/websockets/context_based_research_websockets.md)

#### Emerging trends case study

Voor het onderdeel "Emerging trends" heb ik een case study uitgevoerd voor het onderwerp Domain Driven Design
[De case study is hier te lezen](onderzoek/emerging_trends/case_study_ddd.md)
