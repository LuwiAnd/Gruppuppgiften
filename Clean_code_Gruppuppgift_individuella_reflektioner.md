# Gruppuppgift - individuella reflektioner - Ludwig Andersson

***Vad har du lärt dig om microservices-arkitektur och dess fördelar och utmaningar? Diskutera de specifika fördelarna och svårigheterna du upplevde med att bygga fristående tjänster som kommunicerar genom lösare kopplingar.*** 

Fördelarna med microservices är att man kan deploya dem oberoende av varandra och att om en av dem går ner, så kan de andra fortsätta att fungera. En tydlig fördel är att varje tjänst har ett avgränsat ansvar. I vår lösning ansvarar UserProfiler enbart för användarprofiler, medan UserMessenger hanterar konversationer och meddelanden. Detta gör koden lättare att vidareutveckla, eftersom vi kan arbeta med olika microservice:ar och deploy:a den microservice vi jobbat med utan att det stör den som jobbar i en annan microservice. Om vi hade fortsatt att utveckla vår app till ett stort projekt, så hade det underlättat mycket att vi skapat microservices, eftersom en utvecklare inte behöver ha full koll på alla service:ar, utan kan lära sig om en microservice och kunna vidareutvecklar den utan att behöva sätta sig in i exempelvis en stor databas som har mängder av tabeller som är orelaterade till det han eller hon ska utveckla.

Att vi dessutom satt upp en API-gateway har dessutom gett oss förutsättningar att sätta upp viktig funktionalitet för att skydda oss mot DDOS-attacker med hjälp av rate limiting och för att ha en gemensam identity provider för att hantera autorisering och autentisering av användare.

Svårigheterna med microservice:ar är att det kan öka komplexiteten för små projekt som vårt och det hade varit enklare att sätta upp en gateway för flera projekt inom samma solution om vi istället hade utvecklat projektet som en monolit.




***Hur har användandet av design patterns och SOLID-principerna påverkat din kodstruktur och systemets förvaltningsbarhet?  Reflektera över specifika design patterns eller SOLID-principer du har använt. Hur har de bidragit till att förbättra kodens struktur, förvaltningsbarhet och läsbarhet?***

Vi har använt clean code architecture för att sätta upp våra microservice:ar, så att varje projekt inom deras respektive solution motsvarar ett lager/layer från den arkitekturtypen. Detta följer även Single Responsibility Principle, eftersom varje lager har ett specifikt ansvar (vi brukar ju prata om att varje klass bara ska ha en anledning att ändras, men jag tänker att denna princip kan tillämpas på ett helt projekt/lager). Detta gör att affärslogiken i core kan hållas intakt även om vi skulle ändra något som påverkar infrastructure- eller presentationslagret.

Vi har satt upp service:ar för att slimma koden i controllers och i presentationen genom att flytta logiken till service:arna istället. 

Vi har satt upp repository pattern för att separera databashantering från övrig logik, vilket underlättar om vi skulle vilja byta databasen eller byta ut EF Core mot något annat paket.

Vi har lagt in Dependency Injections på flera ställen i våra olika Program.cs, vilket följer Dependency Inversion Principle.

Vi har använt DTO-pattern för att inte läcka information om databasstrukturen. Detta gör även att lösenord aldrig skickas tillbaka till frontend och vi minskar risken att hackare ska kunna snappa upp dem.

Tillsammans har dessa principer och patterns lett till att vår kod är mer modulär och lättare att läsa/förstå, och därmed är vår kod enklare att testa, förvalta och skala upp. Detta minskar även risken för att en teknisk skuld byggs upp över tid, då man behöver lägga mindre tid på att förstå kod i små, avgränsade program, och då enklare kan uppdatera koden i enlighet med clean code redan från början, utan att göra någon "fulfix" även om man är stressad.

