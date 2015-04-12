---
layout: post
title:  "The Gathering Hacking Competition Writeup"
subtitle: "Et sammendrag av hvordan jeg og en kamerat tok førsteplass i hackekonkurransen på The Gathering."
excerpt: "Under årets The Gathering 2015 arrangerte Universitetet i Oslo i samarbeid med The Gathering en hackekonkurranse. Jeg og Nikolai endte opp i samme team, og takket være iherdig innsats vant vi førsteplass! Her er vårt sammendrag av konkurransen og de ulike oppgavene."
date:   2015-04-13 00:00:00
categories: hacking writeup
---

##AFK

###QR

Oppgaven sier at vi skal finne en QR-kode, og gir oss et bilde som hint. Det er trygt å anta at bildet inneholder informasjon om hvor vi skal gå for å finne QR-koden. Jeg kastet bildet rett inn i en online [EXIF-viewer](http://regex.info/exif.cgi) og fikk opp et kart rett utenfor Vikingskipet! På med skoa!

En liten detalj når vi fant QR-koden; Vi scannet den og fikk en viss [YouTube-video](https://www.youtube.com/watch?v=dQw4w9WgXcQ) i ansiktet. Det tok litt tid før vi forstod at dette i realiteten var token til oppgaven.

###Lockpicking 1

Med litt dårlige låser i år, så ble konkurransen kanskje litt hardere enn før, men jeg var tidlig ute og fikk prøvd meg frem på litt forskjellige låser. Dermed traff jeg én som var ganske grei og poppet opp på kort tid.

###Lockpicking 2

Jeg tok denne samtidig som jeg tok Lockpicking 1, og hadde tilsvarende fremgangsmåte for å finne en lås som var grei å dirke.

###Funky funge

På UiO-standen var det en plakat av en kryptisk "firkant" som vi måtte finne ut hva var. Jeg tok et bilde og gikk (les; løp) tilbake til plassen min for å starte Googlingen. Jeg fikk tidlig nys om at dette kunne være et esoterisk programmeringsspråk. [Wikipedia artikkelen](http://en.wikipedia.org/wiki/Esoteric_programming_language) var her til stor hjelp. Koden på plakaten innehold det som så ut som mange piler, så jeg scannet Wikipedia-artikkelen etter noe som kunne likne. Der fant jeg språket "Befunge" som virket 'right down my alley'. Et kjapt Google-søk til, og jeg fant en online [Befunge-kompilator](http://www.tutorialspoint.com/compile_befunge_online.php). Ved å plotte inn koden, kompilere og kjøre programmet, så ble løsningen på oppgaven "TG_OiU"!

###Hidden in plain sight

Forferdelig oppgave der vi skulle finne en kode på en PC-skjerm som hadde fått det polariserende filteret fjernet (ergo, skjermen var nesten helt hvit). Med litt finurlig bruk av et plastglass var det alikevel mulig å såvidt tyde token. (Neste år tar jeg med polariserte solbriller...)

##Crypto

###IBM

Ved å besøke siden oppgitt, ble man møtte av denne rare teksten:

> â…ƒ¤™‰£¨@£ˆ™–¤‡ˆ@–‚¢¤™‰£¨@†£¦Z@ã–’…•z@ÅÂÃÄÉÃ~…•ƒ™¨—£‰–•%

Etter mye om og men så var det klart at dette måtte omkodes på én eller annen måte, og mye Google-fu fikk meg på sporet av noen gamle IBM-encodings.
Jeg fant nettsiden [string-functions.com](http://www.string-functions.com/encodedecode.aspx) som var veldig behjelpelig med å konvertere for meg. Etter litt justering av innstillingene, så fikk jeg (cirka) dette som output:

> CSCeCcCuCrCiCtCyC BtBhBrBoBuBgBhB SoSbSsSuSrSiStSyS SfStSwS!S CTCoCkCeCnC:C CECBCCCDCICCC=CeBnBcBrByBpBtBiBoBnB

Hvis man stirrer litt lenge på den, så ser man at man kan fjerne annenhver karakter og få:

>Security through obsurity ftw! Token: EBCDIC=encryption

Yay!

Dette var ikke den *egentlige* løsningen på oppgaven, da den riktige encodingen *egentlig* er EBCDIC (Extended Binary Coded Decimal Interchange Code), noe som ville gjort konverteringen *enda* enklere. Menmen.

###Authenticate

/todo

###Key trouble

Vi ble gitt en offentlig nøkkel og en chiffertekst som vi måtte dekryptere.

Vi antok raskt at det var snakk om en svak privat nøkkel, og kom fort frem til at den mulige fremgangsmåten var et såkalt [Wiener's attack](http://en.wikipedia.org/wiki/Wiener's_attack).

Ved å benytte et Python-script fant vi eksponenten og modulusen fra den offentlige nøkkelen. På denne måte var det bare å sette de sammen til en private nøkkel, slik at vi kunne dekryptere meldingen og få token.

##Web


###First web

/todo

###Warmup

/todo

###Crumble

/todo

###¿Hello?

/todo

###Secure zone

/todo

###Privileged

/todo

##Reverse Engineering

### easy 1

/todo

### easy 2

/todo

### Still easy?

/todo

### Secrets

/todo

## Remote

###Good O'l Windows XP

/todo Metasploit'ed

###A bloggers best friend is her pc

/todo Metasploit'ed

###Coolest shell ever

/todo

##Forensics

### Data Recovery

Her fikk vi tildelt filen fs.image. Det var bare å kjøre ```$ strings fs.image``` i terminalen, og helt nederst i output fant vi token.