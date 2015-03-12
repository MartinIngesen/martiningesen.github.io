---
layout: post
title:  "Moderne biler er en trussel mot sikkerheten din"
subtitle: "Eller, hvorfor du (kanskje) er tryggere i en '96 Saab enn i en Tesla.*"
excerpt: "Dagens biler blir i større og større grad fulle av elektronikk. Et avansert og sammensatt nettverk av sensorer, brytere, motorer og mikrokontrollere sørger for å gi deg alle funksjonene en moderne bil kan tilby. Fra å styre så enkle ting som blinklys og spylevæske, til automatisk parkeringsassistent, gass, brems og klimaanlegg.
<br>
<br>
Det villeste av alt; Teknologien bak er faktisk ganske enkel."
date:   2015-03-12 22:09:02
categories: sikkerhet biler
---

Alt er koblet sammen via et nettverk kalt en CAN bus (Controller Area Network), og fungerer ved at alle sensorene, mikrokontrollere et cetera, er koblet sammen på én linje, og kan sende og motta signaler til og fra hverandre. Hver slik sensor kalles en CAN node, eller en ECU (Electronic Control Unit). Sammen blir dette bilens interne nervesystem.

### Nervesystemet

Siden dette tydeligvis er et veldig viktig system, så er det jo åpenbart at det er godt sikret. Ikke sant?
Nei, tro om igjen. CAN bus systemet er bygget på tanken om at sikkerhet skal implementeres av applikasjonene, altså koden på den enkelte ECUen. Selve CAN bus systemet i seg selv er så lavprotokoll at slikt ikke er påtenkt i ISO-spesifikasjonen engang. Dette betyr at man i teorien for eksempel kan sende en melding til bremsen fra blinklys-spaken, såfremst de vet hvordan man kommuniserer sammen.

[Infosecinstitute](http://resources.infosecinstitute.com/car-hacking-safety-without-security/) skriver at moderne biler har opp mot 50 slike ECUer. Noe som betyr at hvis man klarer å få tilgang til én av 50, så kan man styre alle de andre 49 via den ene. Opp mot 50 potensielle angrepsflater for en hacker.

### Fysisk tilgang

Én ting er fysisk tilgang. Man kan på mange måter sammenlikne en bil og en datamaskin i den forstand, "har noen fått fysisk tilgang, så har du blitt kompromitert". Til en pris av snaue 250 kroner, kan man selv modifisere en ECU til å lese og sende kommandoer. Dette viste Alberto Garcia Illera og Javier Vazquez Vidal i sitt foredrag under DEF_CON 21 med tittelen [Dude WTF In My Car](https://www.defcon.org/images/defcon-21/dc-21-presentations/Illera-Vidal/DEFCON-21-Illera-Vidal-Dude-WTF-in-My-Car-Updated.pdf). Chris Valasek og Charlie Miller har også gjort masse research på emnet på oppdrag fra DARPA. Valasek holdt et [detaljert foredrag](https://www.youtube.com/watch?v=737_GtDmfH4) på Duo Tech Talk i januar 2014.

### Fjerntilgang

Fjerntilgang. Se for deg scenarioet at en person kan sitte på en strand i Australia, og styre bilen din ut i grøften ved kun ett tastetrykk? Det er kanskje ikke så umulig som du skulle ønske. Fordi hva er det mange biler har i dag som muligens kan være sårbare for fjernangrep? Jo, hva med "Ring bilen varm"-funksjonen? Hva med den innebygde telefonen som tillater deg å ringe og motta samtaler i bilen (*Telematic Control Unit, TCU*)? Hva med nødkommunikasjonsenheten (*Emergency Communication System, ECS*) i bilen som ringer 113 hvis du har vært i en ulykke? Eller for et litt mindre fjernt angrep, hva med blåtann-tilkoplingen bilen din tilbyr for å spille av musikk fra mobilen?

Alle disse er potensielle angrepsflater som bilen din tilbyr. Du bruker kanskje ingen av dem, men de er som oftest skrudd på som standard, og de er kanskje ikke mulig å skru av uten å fysisk kutte ledninger (eller flashe ECUen).

### "So this is something that a hacker could...? heheheheee"

Amerikanske 60 Minutes gjorde nylig en reportasje med DARPA og deres softwareenhetssjef Dan Kaufman, der de illustrerte et fjernangrep hvor de ringer opp bilens ECS og spiller av de rette tonene. Dette fører til en buffer underrun som de utnytter til å omprogrammere ECSen, slik at de får fullstendig fjerntilgang. [Videoen kan sees her.](https://www.youtube.com/watch?v=7E1WsdODxu0)

Bilen, en 9 generasjon Chevrolet Impala, er bare én av mange som dette angrepet kan fungere på. Disse CAN bus systemene er hyllevare, kjøpt i kvanta av ulike bilfabrikanter. Dette betyr at samme systemer eksisterer på tvers av både modeller og merker. Til sammenlikning er dette litt som fragmenteringen vi ser i OS-versjoner på datamaskiner eller telefoner, der hver versjon har sine sikkerhetsproblemer. Overnevnte angrep fungerer ikke på alle mulige biler fordi de har ulike systemer, men hvis hvert system har opp imot 50 ECUer, så betyr det at man har mange andre steder å lete etter svakheter.

60 Minutes er selvfølgelig ikke de første som har rapportert om dette. Foruten en haug av granskning utført av forskere, har også Motherboard skrevet flere artikler<sup>1,2,3</sup> om emnet, samt en [video (How to hack a car)](https://www.youtube.com/watch?v=3jstaBeXgAs) som er innholdsmessig ganske lik 60 Minutes sin.

<sup>1</sup>[We Drove a Car While It Was Being Hacked](http://motherboard.vice.com/read/we-drove-a-car-while-it-was-being-hacked)

<sup>2</sup>[How Easily Can a Moving Car Be Hacked?](http://motherboard.vice.com/blog/how-easily-can-a-moving-car-be-hacked)

<sup>3</sup>[Hacking a Car Shouldn't Be as Easy as Hacking a Computer](http://motherboard.vice.com/read/hacking-a-car-shouldnt-be-as-easy-as-hacking-a-computer)

## Men, hvordan har dette innvirkning på meg?

Først og fremst har vi det helt elementære; Du kan bli skadet, i verstefall drept. Du kan også skade andre, uten selv å være i kontroll av kjøretøyet du sitter i.

Videre har vi passiv påvirkning, noen kan dundre inn i deg i 120 uten at du eller motpart egentlig har skyld i det.

Begge de overnevnte bærer med seg én ting; Terrorisme. Hvis ingen tørr å kjøre bilene sine lenger, og de som gjør det blir smelt inn i nærmeste fjellvegg, vil dette spre og skape stor frykt.

Fra noe veldig ille til noe som kanskje ikke er så forferdelig; Noen kan fjernopplåse bilen din, og dra sin vei. Dette er kanskje det mest normale for enkelte hackere, nemlig de med økonomiske insentiver. Biler har alle slags sikkerhetsmekanismer for å unngå å bli stjelt, men hvis man kan omprogrammere disse mekanismene, så har man kommet like langt.

En siste ting er tanken om personvern. Moderne biler er som oftest utstyrt med satelittnavigasjon, noe som muliggjør overvåkning av hvor du (eller i alle fall bilen) befinner seg til enhver tid. Bilen har også gjerne handsfree innebygget. Noe som betyr at man kan overhøre alt som blir sagt. Dette føles kanskje ikke så krenkende for de som kjører til og fra jobb hver dag og kun synger til radioen, men for personer med "viktige" stillinger kan dette være fatalt. Selv når man bare sitter og snakker for seg selv kan bilen være delaktig i industrispionasje.

## Bør jeg føle meg utrygg?

Tjanei. Per dags dato har du nok ikke så mye å frykte, men det er verdt å tenke over som et kriterie neste gang du skal investere i ny bil. Sjekk hvilke tilkoblingsmuligheter den har, søk opp om det finnes offentlige rapporter på sikkerhetshull i modellen du vurderer sjekk produsentens forhold til sikkerhet og om de tar det på alvor. Det er kundene som legger lista for hva produsentene lager.

## Hvordan kan man løse dette?

1. Man kan [saksøke](http://www.computerworld.com/article/2895057/lawsuit-seeks-damages-against-automakers-and-their-hackable-cars.html).

Men for de som ikke ønsker å gå "The American Way", så kunne biler trengt et nytt system for internkommunikasjon. En helt ny ISO-standard som sikkerhet som en grunnpilar fra begynnelsen. Det er sagt så mange ganger før, men sikkerhet er ikke noe man bare kan legge til når et produkt er "ferdig". Det må inkorporeres allerede fra begynnelsen av.

For de som allerede sitter med en bil som de vet har en haug av fancy funksjoner, så er det ikke så alt for mye man får gjort. Man kan prøve å deaktiver mesteparten av funksjonalitetene, men som oftest er dette veldig vanskelig.


Eller så man man vel bare... kjøpe en '96 Saab?*


<br>
<br>
<small>\* Bilmerker og årstall valgt på slump. Poenget står fortsatt.</small>