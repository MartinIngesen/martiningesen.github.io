#Man trenger bare 16 minutter for å åpne en låst telefon
###Med ny teknologi kan din 4 sifrede kode være en lett match for hackere

Mobilen har blitt veldig viktig i vår hverdag.

MDSec rapporterte nylig<sup>1</sup> om en teknologi som blir benyttet i markeder for telefonreparasjon, som gjør det mulig å finne ut av hvilken som helst 4-sifret opplåsningskode på en iPhone.<sup>2</sup> Videre undersøkelser viser også at det finnes verktøy for å gjøre dette på HTC og Samsung-telefoner som bruker Android<sup>2</sup> (hvorvidt disse fungerer vet jeg ikke). Produktene sendes fra Kina, og kan bestilles for så lite som 1400 kroner.

Ved å koble telefonen til en såkalt "IP Box" via Lightningporten (der man lader telefonen) så vil IP Boxen prøve alle mulige pinverdier fra 0000 til 9999, helt til den finner en match. Denne formen for angrep kalles et bruteforce-angrep. Det bemerkelsesverdige her, er at dette fungerer selv om telefonen har funksjonen "slett alt innhold etter 10 feil". Dette fungerer ved at boksen agressivt kutter strømmen til enheten rett etter at vi får vite om koden var riktig eller ikke, men rett før minnet i telefonen blir oppdatert med antallet ganger du har prøvd og feilet.

Det som vanligvis skjer hvis man taster feil kode blir dermed;

1. Man taster inn kode
2. Man får beskjed om at koden er feil
3. iPhonen oppdaterer en teller i minne, som inneholder antallet ganger man har prøvd og feilet.

Imens med en enhet som IP Box:

1. IP Box taster inn kode
2. IP Box får beskjed om at koden er feil
3. IP Box kutter strømmen slik at iPhonen ikke rekker å oppdatere telleren
4. Rinse and repeat

Å prøve én kombinasjon tar ifølge MDSec 40 sekunder, så hvis man skulle prøvd alle de mulige kombinasjonene fra 0000 til 9999, så vil det ta **max** (10000*40)400 000 sekunder, eller (400 000/60/60=) ~111 timer å utføre dette angrepet. Det er selvfølgelig lenge, men hvis man har tilgang til telefonen over en ubegrenset periode, så er ikke det noe problem.

### Analyseangrep

Det er dog muligheter for å gjøre dette angrepet enda mer effektivt. I en artikkel skrevet av Nick Berry<sup>5</sup> forteller han at det er 11% sannsynlighet for at pinkoden er 1234. Logisk nok. Neste på listen med over 6% er 1111.

Berry har scannet og statistikkført over 3 400 000 pinkoder, og har laget en sortert liste over alle mulige pinkoder.

Hvis man benytter seg av slike data i angrepet mot telefonen, så vil det utifra grafene til Berry, ta cirka ~28 timer å gjette seg til passordet. Sett opp imot 111 timer, som vi startet med.

### Smudge Attack

Hvis vi vil ta dette enda et steg videre, så er det en forskningsrapport skrevet av noen forskere ved University of Pennylvania som beskriver et såkalt "smudge attack".<sup>4</sup> Dette angrepet har grunnlag i at når vi taster inn koden på en skjermen, etterlater fingerene våres små flekker. Disse flekkene kan man enten se med det blotte øye, eller ta bilder fra forskjellige vinkler og med forskjellig lyssetting. Rapporten tar for seg Android sin mønsterkode, hvor man drar fingeren fra forskjellige punkter, og dermed lager en unik "kode". Rapporten forteller oss at de ved 92% av tilfellene kunne delvis identifisere mønsterets retning og rekkefølge. i 68% av tilfellene var det fullstendig mulig å gjengi mønsteret.

Når man vet hvilke 4 siffer som koden består av, har man !4 (4 fakultet) antall mulige tallkombinasjoner. Det vil si (4*3*2*1 =) 24 muligheter. Hvis man prøver hver av disse, og hver enkelt tar 40 sekunder, så blir det 960 sekunder, eller **16** minutter.

Med andre ord kan telefonen du tror er godt sikret med pinkode, bli åpnet av en person med det rette utstyret på litt over et kvarter!

### Hvordan skal du beskytte deg?

Skru av "simple pin-code" på telefonen. Benytt et lengre passord, som helst inneholder både bokstaver og tall. iPhone sin TouchID vil ikke ha noe si, fordi når du skrur på en iPhone, så blir du tvunget til å skrive inn koden din, før man i det heletatt får lov til å benytte seg av TouchID.

#### Referanser

[1] [http://blog.mdsec.co.uk/2015/03/bruteforcing-ios-screenlock.html](http://blog.mdsec.co.uk/2015/03/bruteforcing-ios-screenlock.html)<br>
[2] [http://www.ipmart.com/main/product/IP,Box,For,iPhone,Packaged,with,4,cables,,391543.php?prod=391543](http://www.ipmart.com/main/product/IP,Box,For,iPhone,Packaged,with,4,cables,,391543.php?prod=391543)<br>
[3] [http://www.ipmart.com/main/product/MFC,Dongle,393044.php?prod=393044&ref=mini_bn_pt_mfc_dongle](http://www.ipmart.com/main/product/MFC,Dongle,393044.php?prod=393044&ref=mini_bn_pt_mfc_dongle)<br>
[4] [https://www.usenix.org/legacy/event/woot10/tech/full_papers/Aviv.pdf](https://www.usenix.org/legacy/event/woot10/tech/full_papers/Aviv.pdf)<br>
[5] [http://www.datagenetics.com/blog/september32012/](http://www.datagenetics.com/blog/september32012/)<br>
[2] []()<br>
[2] []()<br>