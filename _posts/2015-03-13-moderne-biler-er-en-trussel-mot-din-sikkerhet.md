---
layout: post
title:  "Moderne biler er en trussel mot din sikkerhet"
subtitle: "Eller: Hvorfor du (kanskje) er tryggere i en 96 Saab enn i en Tesla.*"
excerpt: "Dagens biler fylles av mer og mer elektronikk. Et avansert og sammensatt nettverk av sensorer, brytere, motorer og mikrokontrollere danner alle funksjonene en moderne bil har å by på. Elektronikken styrer alt ifra blinklys og spylervæske, til automatisk parkeringsassistent, gass, brems og klimaanlegg.
<br>
<br>
Det villeste av alt: Teknologien bak er egentlig ganske enkel."
date:   2015-03-13 01:46:00
categories: sikkerhet biler
---

Alt er koblet sammen via et nettverk kalt en CAN bus (Controller Area Network), og fungerer ved at alle sensorene, mikrokontrollere etc, er koplet sammen på én linje, og kan sende og motta signaler - til og fra hverandre. Hver slik sensor kalles en CAN node, eller en ECU (Electronic Control Unit). Sammen utgjør disse bilens interne nervesystem.

### Nervesystemet

Siden dette åpenbart er et veldig viktig system i bilen din, skulle du tro det er godt sikret - ikke sant?
Tro om igjen. CAN bus systemet er bygd på tanken om at sikkerhet skal implementeres av applikasjonene, altså: Koden på den enkelte ECUen. Selve CAN bus systemet i seg selv er så lavprotokoll at slikt ikke er tenkt på i ISO-spesifikasjonen engang. Dette betyr at man i teorien kan sende en melding til bremsene ved hjelp av blinklys-spaken, såfremt de vet hvordan de kommuniserer sammen.

[Infosecinstitute](http://resources.infosecinstitute.com/car-hacking-safety-without-security/) skriver at moderne biler kan ha omtrent 50 slike ECUer. Får man tilgang til bare én av 50, kan man styre de øvrige 49 gjennom den ene. Enkelt fortalt: de utgjør 50 potensielle angrepsflater for en hvilken som helst hacker.

### Fysisk tilgang

Vi kan på mange måter sammenlikne en bil med en datamaskin i følgende forstand, "har noen fått fysisk tilgang, så har du blitt kompromittert".

Det viser seg at for snaue 250 kroner, kan hvem som helst få en ECU til å lese og sende kommandoer. Dette viste Alberto Garcia Illera og Javier Vazquez Vidal i sitt foredrag under DEF_CON 21 (tittelen på foredraget var "[Dude WTF In My Car](https://www.defcon.org/images/defcon-21/dc-21-presentations/Illera-Vidal/DEFCON-21-Illera-Vidal-Dude-WTF-in-My-Car-Updated.pdf)" om du er interessert). Chris Valasek og Charlie Miller har også gjort grundig research om emnet, på oppdrag fra DARPA (Defense Advanced Research Projects Agency). Valasek holdt også [detaljert foredrag](https://www.youtube.com/watch?v=737_GtDmfH4) på Duo Tech Talk i januar 2014. Dette er bare et lite utvalg av forskere som har vist at bilers ECUer kan hackes.

### Fjerntilgang

Se for deg følgende: En person kan sitter på en strand i Australia. Sola skinner, og han koser seg med et bilspill på iPhonen sin. Bare at det ikke er et spill. I stedet har han hacket bilen din, og er i ferd med å styre den ut i grøften - med deg og ungene i den.

Dette scenariet er kanskje ikke så utenkelig som du skulle ønske. For hva er det mange biler i dag har - som muligens kan være sårbare for fjernangrep? Høres "ring bilen varm"-funksjonen kjent ut? Hva med den innebygde telefonen som tillater deg å ringe og motta samtaler  (*Telematic Control Unit, TCU*)? Hva med nødkommunikasjonsenheten (*Emergency Communication System, ECS*) som ringer 113 hvis du havner i en ulykke? Eller for et litt mindre fjernt angrep, hva med blåtann-tilkoplingen bilen din tilbyr for å spille av musikk fra mobilen?

Alle disse funksjonene er potensielle angrepsflater som bilen din tilbyr. Du bruker kanskje ingen av dem, men de er som regel skrudd på som standard, og de er ofte ikke mulig å deaktivere uten fysisk å kutte ledninger (eller flashe ECUen - for dem som kan sånt).

Forskning utført i 2010-11 av forskere ved University of Washington og University of California viser eksempler på nettopp dette. Der har de kommet frem til at fjerntilgang til bilen er mulig via en rekke funksjoner. Deriblant fjernreparasjonsfunksjoner, CD-spilleren, blåtanntilkopling, radio. Videre har de også funnet ut at trådløse kommunikasjonskanaler åpner for å styre en bil på avstand. Artiklene kan leses her: [Experimental Security Analysis of a Modern Automobile](http://www.autosec.org/pubs/cars-oakland2010.pdf), [Comprehensive Experimental Analyses of Automotive Attack Surfaces](http://www.autosec.org/pubs/cars-usenixsec2011.pdf)

### "So this is something that a hacker could...? [heheheheee](https://www.youtube.com/watch?v=7E1WsdODxu0)"

Amerikanske 60 Minutes gjorde nylig en reportasje med DARPA og deres softwareenhetssjef Dan Kaufman, der de illustrerte et fjernangrep ved å ringe bilens ECS, og spille av de rette tonene. Dette fører til en buffer underrun som de utnytter til å omprogrammere ECSen, slik at de får fullstendig fjerntilgang. [Videoen kan sees her.](https://www.youtube.com/watch?v=7E1WsdODxu0)

Bilen, en 9. generasjons Chevrolet Impala, er bare én av mange som dette angrepet kan fungere på. CAN bus systemene er hyllevare, kjøpt i kvanta av ulike bilfabrikanter. Dette betyr at de samme systemene eksisterer på tvers av både modeller og merker. Til sammenlikning er dette litt som fragmenteringen vi ser i OS-versjoner på datamaskiner eller telefoner, der hver versjon har sine unike sikkerhetsproblemer. Overnevnte angrep fungerer ikke på alle biler, siden også de har ulike systemer, men hvis hvert system har opp mot 50 ECUer, betyr det at man har en rekke andre steder å lete etter svakheter.

60 Minutes er selvfølgelig ikke de første som har rapportert om denne typen svakhet. Foruten en haug granskning utført av forskere, har også Motherboard skrevet flere artikler** om emnet, samt laget en [video (How to hack a car)](https://www.youtube.com/watch?v=3jstaBeXgAs) som er innholdsmessig ganske lik 60 Minutes sin.

## Men hvordan påvirker dette meg?

Først og fremst: Du kan bli skadet, i verstefall drept. Du kan også skade andre - uten selv å ha kontroll på kjøretøyet du sitter bak rattet på.

Videre har vi passiv påvirkning: Noen kan dundre inn i deg i 120km/t uten at du, eller motparten, egentlig har skyld i det.

Begge de overnevnte har én ting til felles; De kan brukes til terrorisme. Hvis ingen tør å kjøre bilene sine lenger, og de som gjør det blir smelt inn i nærmeste fjellvegg, vil frykten raskt gjennomsyre både E6, E18 og hver minste lille fylkesvei.

Så: Fra noe veldig ille til noe som kanskje ikke er så forferdelig: Noen kan låse opp bilen din på avstand, og dra sin vei. Dette er kanskje det mest realistiske scenariet, og gjennomføres av hackere med økonomiske hensikter. Biler har alle slags sikkerhetsmekanismer for å unngå å bli stjålet, men hvis man enkelt kan omprogrammere disse mekanismene er du like langt.

En siste ting å tenke på er personvernet. Moderne biler er som oftest utstyrt med satellittnavigasjon, som muliggjør overvåkning av hvor du (eller i alle fall bilen din) befinner deg til enhver tid. Bilen har også gjerne innebygd hands-free. Noe som tillater  at andre kan overhøre alt som blir sagt. Dette føles kanskje ikke så krenkende for de som kjører til og fra jobb hver dag og kun synger med til radioen, men for personer med sensitive stillinger kan dette være fatalt. Selv når du bare sitter og snakker med deg selv kan bilen være en viktig brikke i industrispionasje.

## Bør jeg føle meg utrygg?

Tja. Per dags dato har du nok ikke så mye å frykte, men det er verdt å ha digital sikkerhet  som et kriteria neste gang du skal investere i ny bil. Sjekk hvilke tilkoblingsmuligheter den har, søk opp om det finnes offentlige rapporter på sikkerhetshull i modellen du vurderer, sjekk produsentens forhold til digital sikkerhet og om de tar det på alvor. Det er kundene som legger lista for hva produsentene lager.

## Hvordan kan man løse dette?

Én måte er å [saksøke](http://www.computerworld.com/article/2895057/lawsuit-seeks-damages-against-automakers-and-their-hackable-cars.html) bilfabrikantene som bygger usikre biler, men hvorvidt man kommer noen vei med det er usikkert. Det handler om insentiver. Kundene ønsker seg en trygg og energibesparende bil: Og det er akkurat det de får. Ingen sier noe om den underliggende sikkerheten som bare blir viktigere jo flere sensorer du dytter inn i bilen.

Biler trenger et nytt system for internkommunikasjon. En helt ny ISO-standard som har sikkerhet som en grunnpilar fra begynnelsen. Det er sagt mange ganger før, men sikkerhet er ikke noe som bare kan legges til når et produkt er "ferdig". Det må inkorporeres allerede fra dag én.

For dem som allerede sitter med en bil de vet har en haug fancy funksjoner, er det ikke så alt for mye man får gjort. Du kan prøve å deaktiver mesteparten av funksjonene, men som oftest er dette tilnærmet umulig.


Eller så kan du vel bare... kjøpe en 96 Saab?*


<br>
<small>\* Bilmerker og årstall valgt på slump. Poenget kommer uansett gjennom.</small>

<small>\*\*[We Drove a Car While It Was Being Hacked](http://motherboard.vice.com/read/we-drove-a-car-while-it-was-being-hacked)
| [How Easily Can a Moving Car Be Hacked?](http://motherboard.vice.com/blog/how-easily-can-a-moving-car-be-hacked)
| [Hacking a Car Shouldn't Be as Easy as Hacking a Computer](http://motherboard.vice.com/read/hacking-a-car-shouldnt-be-as-easy-as-hacking-a-computer)</small>
