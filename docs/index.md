Software Architectuur Document
Tommy Goossens - The Gram

V0.1Versiebeheer

VerspreidingenInhoudsopgave
Inhoudsopgave	1
Inleiding	2
Doel	2
System Context C1	3
C2 Containers en Technologieën	4
C3 Componenten Diagram	5
Feed & Post services	5
Beide services zijn gemaakt met het .Net Core framework en beschikken over een rest interface. Rest calls gaan via de gateway naar de desbetreffende controller.	5
De feed controller haalt alle relevante posts op voor de ingelogde gebruiker. Alleen de posts van de van de gebruikers die gevolgd worden, worden getoond.	5
De post controller houdt zich bezig met het verwerken van nieuwe posts, geplaatste reacties en de likes.	5
Profile & Chat services	6
Wederom services gemaakt met het .Net Core framework en bieden ook een rest interface aan. De chat service heeft ook nog een websocket controller om de realtime chat mogelijk te maken.	6
De profile controller biedt de mogelijkheid om de gebruiker zijn profiel te laten wijzigen, vrienden toe te voegen en gegevens te bekijken.	6
De chat controller haalt de bericht geschiedenis op en de WS controller zorgt voor het real-time afleveren van berichten aan de client.	6
C4 Componenten en Activiteitendiagram	7
Communicatie	8
Inleiding
In dit document vallen de technische details over de applicatie The Gram te lezen. The Gram is een op instagram gebaseerde iOS applicatie welke in Swift 5 geschreven wordt. De backend waar de applicatie mee communiceert is geschreven met het .Net Core 3.1 framework. Er wordt gebruik gemaakt van een microservice architectuur wat in een kubernetes omgeving gedeployed zal worden.

Doel
Het doel van dit document is om meer inzicht te geven in hoe The Gram in componenten opgedeeld en opgebouwd wordt. Ook valt te lezen hoe de communicatie tussen de services onderling en tussen de mobiele applicatie en de backend zal verlopen.
Scope
Tot de scope van dit document horen de UML modellen voor de context, containers, componenten en code. Het model van de code zal zich richten op de backend code.System Context C1
In het onderstaande model is het globale systeem beschreven. Twee soorten actoren zijn mogelijk: een gastgebruiker en een geregistreerde gebruiker. De gast gebruiker registreert zichzelf door middel van het Firebase Authenticatie systeem. Nadat de gastgebruiker geregistreerd is heeft deze toegang tot het software systeem van The Gram. C2 Containers en Technologieën
In onderstaand model wordt het systeem uitvergroot en valt af te lezen dat er vier services draaien, naast de event bus en gateway. De API gateway wordt gerealiseerd met behulp van Ocelot. De event bus zal werken met het RabbitMQ protocol.
C3 Componenten Diagram
In dit hoofdstuk staan de modellen van de verschillende componenten die samen de complete applicatie vormen.

Feed & Post services
Beide services zijn gemaakt met het .Net Core framework en beschikken over een rest interface. Rest calls gaan via de gateway naar de desbetreffende controller. 

De feed controller haalt alle relevante posts op voor de ingelogde gebruiker. Alleen de posts van de van de gebruikers die gevolgd worden, worden getoond.

De post controller houdt zich bezig met het verwerken van nieuwe posts, geplaatste reacties en de likes. Profile & Chat services

Wederom services gemaakt met het .Net Core framework en bieden ook een rest interface aan. De chat service heeft ook nog een websocket controller om de realtime chat mogelijk te maken.

De profile controller biedt de mogelijkheid om de gebruiker zijn profiel te laten wijzigen, vrienden toe te voegen en gegevens te bekijken.

De chat controller haalt de bericht geschiedenis op en de WS controller zorgt voor het real-time afleveren van berichten aan de client.C4 Componenten en ActiviteitendiagramCommunicatie
