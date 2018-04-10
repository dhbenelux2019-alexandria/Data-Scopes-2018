# Opdracht 1: Greppen in TvG Data

+ [Introductie](#intro)
+ [Data downloaden](#data)
+ [Research focus: wat ga ik onderzoeken?](#focus)
+ [Reguliere expressies](#regex)
    + [Oefenen met reguliere expressies](#regex-train)
+ [Grep: zoeken in data met behulp van reguliere expressies](#grep)
    + [Command Line: interactie via commando's](#grep-command-line)
    + [Zoeken naar specifieke woorden en varianten](#grep-words)
    + [Ketens van commando's: pipelines, sorteren en tellen](#grep-pipelines)
    + [Distributies en dispersie van specifieke woorden](#grep-words-distributions)
    + [Transliteratie: eenvoudige datatransformaties voor normalisatie](#grep-tr)
    + [Zoeken met woordensets](#grep-word-sets)
    + [Zoeken met karaktersets](#grep-character-sets)

<a name="intro"></a>
## Introductie

Met deze opdracht ontwikkel je de volgende vaardigheden:

+ grip krijgen op heterogene data met grote variatie
+ identificeren en duiden van patronen
+ gestructureerd omgaan met ongestructureerde data

De TvG Dataset is groot en beperkt gestructureerd. Er liggen talloze inzichten verborgen in de 121 jaargangen, maar de bestaande structuur geeft weinig handvatten om die inzichten naar voren te halen. Hoe kun je van deze brondata een data scope creeeren die verschillende perspectieven op het TvG corpus biedt waarmee je op een transparante manier tot nieuwe inzichten komt?

TO DO

+ keuzes in modelleren
+ definieren data assen
+ bijhouden van data interacties om tot data scope te komen

<a name="data"></a>
## Data downloaden

+ Waar staat mijn data?
+ Hoe kan ik met command line tools bij mijn data?
    + commando's worden uitgevoerd vanuit een working directory (folder)
    + je kunt je working directory veranderen ('cd' for change directory)
    + je kunt  aangeven waar een command line tool de data kan vinden t.o.v. je working directory

Download de [TvG dataset]. *Voor Windows gebruikers die Git Bash hebben geinstalleerd, sla de dataset op in eennieuwe directory in de directory waar Git Bash is geinstalleerd. Anders kun je vanuit de command line niet bij de data.*

<a name="focus"></a>
## Research focus: wat ga ik onderzoeken?

_Wat is mijn onderzoeksvraag of thema?_

Je kunt van te voren vaststellen wat je wilt onderzoeken, maar de ervaring leert dat je dit tijdens het onderzoeksproces nog regelmatig zal herzien. Initiele aannames en verwachtingen blijken vaak niet goed genoeg op het bestaande materiaal aan te sluiten, waardoor je een onderzoeksvraag wilt aanpassen of over wilt stappen naar een compleet andere vraag. 

<a name="focus-selection"></a>
### Selectie

_Welke informatie uit TvG is daarbij relevant?_

_Welke jaargangen, pagina's zeggen iets over mijn onderzoeksvraag/thema?_

<a name="focus-modelling"></a>
### Modelleren

_Hoe vertaal ik onderzoeksvragen en methoden naar data interacties?_

Dit is een cruciale vraag en vergt continue reflectie op het proces. Voortschreidend inzicht tijdens het proces van data bevragen, bewerken en analyseren leidt tot aanpassingen in eerdere stappen. 

_Hoe representeer ik relevante informatie in mijn brondata?_

Bij het vertalen van je onderzoeksvraag naar data interacties om die vraag te adresseren, moet je keuzes maken over wat voor soort informatie je uit de data wilt halen, hoe je dat kunt doen en wat de consequenties van je keuzes is:

- Wat zijn de informatie-eenheden die je uit de data wilt halen?
- Waar legt jouw keuze de focus op en wat verdwijnt daarbij mogelijk naar de achtergrond? Wat mis je mogelijk?
- Welke alternatieve keuzes had je kunnen maken, en hoe zou dat tot andere focus en achtergrond leiden?

<a name="regex"></a>
## Reguliere expressies

*Een* manier om informatie uit data te halen is het zoeken naar specifieke of generieke patronen d.m.v. *reguliere expressies*. 

<a name="regex-train"></a>
### Oefenen met reguliere expressies

Open een nieuwe tab in je browser en ga naar [https://regex101.com/](https://regex101.com/).

<a name="grep"></a>
## Grep: zoeken in data met behulp van reguliere expressies

`grep` is een UNIX command line tool waarmee je kunt zoeken in data-bestanden naar regels die *matchen* met een reguliere expressie (**G**lobally search a **r**egular **e**xpression and **p**rint).

Eerst wat uitleg over de *command line* en hoe je navigeert naar verschillende mappen of directories op je harde schijf en andere aangesloten opslagmedia.

**Let op: de oefeningen worden al heel snel heel complex. Dit is puur om te laten zien wat mogelijk is, en waarom dit nuttig kan zijn. De syntax is cryptisch en exotisch en vergt veel oefening om het eigen te maken. Als je het nuttig vindt kun je er dus meer tijd in steken en wordt de systematiek al snel duidelijk. Als je hier niet zelf mee aan de slag wil, is het in ieder geval nuttig om te zien hoe het werkt, wat er mogelijk is, en waar potentiele problemen in de data een rol spelen.**

<a name="grep-command-line"></a>
### Command line: interactie via commando's

De command line is een alternatieve manier om te interacteren met de computer. I.p.v. opdrachten geven met de muis, geef je via de command line opdrachten d.m.v. commando's. Alhoewel dit voor veel mensen wennen is aan ongebruikelijke notatie en de onverbiddelijke precisie die vereist is, heeft het ook een aantal voordelen:

+ complexe commando's: het is mogelijk om commando's samen te stellen zodat in 1 regel meerdere opdrachten worden gegeven die achter elkaar worden uitgevoerd.
+ herhaalbaarheid: uitgevoerde opdrachten worden bewaard waardoor je makkelijk eerdere commando's nog een keer kunt uitvoeren. Dit is vooral handig voor complexe commando's of wanneer je een opdracht wilt herhalen met een kleine wijziging.
+ automatisering: je kunt de commando's bewaren in een bestand, zodat je op een later tijdstip kunt zien wat je eerder gedaan hebt. Daarnaast kun je lijsten van commando's in een bestand ook weer achter elkaar laten uitvoeren door het bestand `executable` te maken.

Voor transparant werken met data is het belangrijk en waardevol om uitgevoerde opdrachten bij te houden en te kunnen delen (ook met jezelf op een later tijdstip).

Om onderstaande oefeningen te doen is het handig te navigeren naar de directory waar je de TvG data hebt opgeslagen. **De directory-structuur is een zogenaamde boom structuur, met een enkele stam (root) en een of meer vertakingen. In UNIX omgevingen is `/` het symbool voor de *root* directory, `/home` is de *home* directory binnen de *root* directory. Directories binnen andere directories (sub-directories) zijn vertakingen binnen vertakingen. Zo is je persoonlijke directory die overkomt met je gebruikersnaam een subdirectory van `/home`. Je kunt o.a. navigeren tussen directories via de vertakingen.**

+ navigeren tussen directories:
    + `pwd` (print working directory): waar ben ik? Het `pwd` commando laat zien in welke directory je de commando's uitvoert. 
    + `cd` (change directory): verander mijn *working directory* (*wd*). Je kunt je *wd* wijzigen met `cd`. Met `cd ~` verander je de *wd* naar je zgn. *home directory*, ofwel je persoonlijke directory (`/home/<gebruikersnaam>`). Als je daarbinnen een directory `data` hebt staan, kun je van daaruit navigeren naar de `data` directory met `cd data`. Met `cd ..` ga je een niveau omhoog in de directory-structuur
    + `ls` list: wat zit er in deze directory (e.g. mijn *working directory*)?
+ relatieve paden
    + `ls tvg/`: wat zit er in de subdirectory `tvg`? (dit gaat er vanuit dat de huidige *working directory* een subdirectory `tvg` heeft. Als dat niet het geval is dan leidt dit commando tot een error `no such file or directory`).
    + `ls ../`: wat zit er in de directory boven mijn huidige *working directory*?


<a name="grep-words"></a>
### Zoeken naar specifieke woorden en varianten

Navigeer naar de directory waar je de TvG data hebt opgeslagen. Controleer dat je op de juiste plek bent door `ls` te typen. Als je de directories `tvg_1` etc. ziet staan ben je in de juiste directory. Zo niet, type dan `pwd` en vergelijk je huidige *working directory* met waar de data staat. 

Op de juiste plek aangekomen, kun je nu `grep` uitvoeren om naar patronen in de data te zoeken. Als eerste oefening, zoek voorkomens van het woord *politiek* in alle bestanden van de 111de editie:

```bash
grep "politiek" tvg_111/*.txt
```

Je krijgt een lijst te zien van alle regels (lines) van de verschillende bestanden waarin de tekststring *politiek* voorkomt. Aan het begin van elke regel toont `grep` de naam van het bestand waarbinnen de regel gevonden is (een *match*). 

Dit is ietwat onoverzichtelijk. Je kunt `grep` opdracht geven om het deel van de regel dat *matched* met je patroon in kleur weer te geven, zodat je sneller ziet wat de *match* is. Dit kan met de parameter `--color` of `--colour`:

```bash
grep --color "politiek" tvg_111/*.txt
```

Je ziet nu dat `grep` ook regels vindt waarin *politiek* een onderdeel is van een woord, zoals *politieke* en *bouwpolitiek*. Je kunt de parameter `-w` gebruiken om aan te geven dat `grep` alleen moet matchen met hele woorden (e.g. alleen als *politiek* het hele woord is). *Opmerking: het is gebruikelijk om alle parameters direct achter het `grep` commando te plaatsen, zodat je makkelijk kunt zien welke parameters gebruikt zijn. In de opdrachten hier beneden zul je zien dat er veel verschillende parameters zijn, en je vaak wil je er meerdere combineren. Dan is het handig om ze te groeperen, aan het begin of aan het eind van het commando.*

```bash
grep --color -w "politiek" tvg_111/*.txt
```

Je vraagt je wellicht af hoe `grep` weet wat hele woorden zijn. Dat wordt duidelijk na een aantal opdrachten hier beneden. Standaard maakt `grep` een onderscheid tussen hoofdletters en kleine letters. Als je wilt zoeken zonder hoofdlettergevoeligheid, kun je de `-i` parameter gebruiken:

```bash
grep --color -w -i "politiek" tvg_111/*.txt
```

Om reguliere expressies te gebruiken kun je de parameter `-E` toevoegen (voor *extended regular expression*). Hiermee interpreteert `grep` het opgegeven patroon als een reguliere expressie i.p.v. een letterlijke string. Eerst wordt het zoekpatroon uitgebreid naar alle woorden die beginnen met *politiek*, eventueel gevolgd worden door 1 of meer letters (aangegeven met `\w*`.) De `\w` staat voor alles wat alfanumerisch is (letters en cijfers), de `*` staat voor nul, een of meer keren herhaald:

```bash
grep -E --color -w -i "politiek\w*" tvg_111/*.txt
```

Dit is vergelijkbaar met zoeken a.d.h.v. een zgn. *wildcard*, e.g. *politiek*\*. Het voordeel van reguliere expressies is dat je meer controle hebt over wat er op de plaats van het *wildcard* mag staan. Als je de `*` vervangt door een `+`, matched `grep` alleen met woorden die *beginnen* met *politiek* en gevolgd worden door tenminste een maar mogelijk meer alfanumerische karakters:

```bash
grep -E --color -w -i "politiek\w+" tvg_111/*.txt
```

De kracht van reguliere expressies wordt hieronder steeds duidelijker gemaakt. Je kunt bijvoorbeeld ook preciezer aangeven hoeveel extra karakters er moeten volgen, met behulp van accolades. E.g `\w{2,15}` betekent minimaal 2 en maximaal 15 extra alfanumerische karakters:

```bash
grep -E --color -w -i "politiek\w{2,15}" tvg_111/*.txt
```

Je kunt ook alleen een minimum aangeven (e.g. `{2,}`) of alleen een maximum (e.g. `{,15}`). 

Wat met *wildcards* vaak niet mogelijk is, is ze plaatsen aan het begin van een woord. E.g. om te zoeken naar alle woorden die *eindigen* op *politiek* kun je de expressie `\w+` ervoor plaatsen:

```bash
grep -E --color -w -i "\w*politiek" tvg_111/*.txt
```

Hiermee zie je voorkomens van verschillende vormen van politiek. Uiteraard kun je ook zoeken naar alle woorden die *politiek* bevatten aan het begin, midden of einde van een woord:

```bash
grep -E --color -w -i "\w*politiek\w*" tvg_111/*.txt
```

Je kunt nog veel complexere patronen definieren met reguliere expressies. Verderop staan oefeningen voor het zoeken naar namen, jaartallen en datums. Een andere mogelijkheid is het zoeken naar bijv. woorden die voorafgaan aan een woord dat matched met *politiek*. Door `\w+ ` te plaatsen voor `\w*politiek\w*` krijg je nu ook het voorafgaande woord te zien:

```bash
grep -E --color -w -i "\w+ \w*politiek\w*" tvg_111/*.txt
```

Ook twee woorden vooraf is mogelijk:

```bash
grep -E --color -w -i "\w+ \w+ \w*politiek\w*" tvg_111/*.txt
```

Een generiekere manier is om meerdere herhalingen te definieren. Je kunt een subpatroon definieren door het te plaatsen binnen parentheses, e.g. `(\w+ )`. Voor dit subpatroon kun je aangeven of het bijvoorbeeld nul of een keer mag voorkomen (`(\w+ )?`), een of meer keer (`(\w+ )+`), nul, een of meer keer (`(\w+ )*`), precies vijf keer (`(\w+ ){5}`) of maximaal drie keer (`(\w+ ){,3}`):

```bash
grep -E --color -w -i "(\w+ ){,3}\w*politiek\w*" tvg_111/*.txt
```

<a name="grep-pipelines"></a>
### Ketens van commando's: pipelines, sorteren en tellen

Met flexibele patronen wordt de lijst matches al snel erg lang en gevarieerd. Om beter zicht te krijgen op de verschillende matchende patronen, kun je `grep` ook opdracht geven om niet de hele regels, maar alleen de matchende delen te tonen, met de parameter `-o`:

```bash
grep -E --color -o -w -i "\w*politiek\w*" tvg_111/*.txt
```

Voor exploratie van een grote dataset kan het handig zijn om de gevonden patronen te sorteren en tellen, zodat je een beeld krijgt welke patronen veel en welke weining voorkomen. Voordat je dat doet is het handig om de output van `grep` nog zo aan te passen dat de bestandsnamen niet meer getoond worden, zodat echt alleen de matchende patronen getoond worden. Daarna is sorteren en tellen simpel. Je kunt de bestandnamen laten verbergen met de `-h` parameter:

```bash
grep -E -h -o -w -i "\w*politiek\w*" tvg_111/*.txt
```

UNIX biedt een eenvouig maar zeer krachtig mechanisme om de output van het ene commando direct door te sturen als input voor het volgende commando, d.m.v. zogenaamde *pipes*. Als je het `|` symbool plaatst aan het eind van het `grep` commando, wordt de output van `grep`, e.g. alle matchende patronen, doorgestuurd naar een volgend commando.

Je kunt bijvoorbeeld het `wc` (*word, line, character and bye count*) commando gebruiken om te tellen hoeveel matches er zijn:

```bash
grep -E -h -o -w -i "\w*politiek\w*" tvg_111/*.txt | wc -l
```

De `-l` parameter telt het aantal regels (lines). Met `-w` telt `wc` het aantal woorden, met `-m` het aantal karakers.

Het `sort` commando sorteert input en toont de gesorteerde output weer op het scherm. Standaard sorteert `sort` alfabetisch in oplopende volgorde, maar je kunt ook numerisch sorteren en in aflopende volgorde. Dit ga net als met `grep` a.d.h.v. parameters. Voor nu volstaat dat standaard sortering:

```bash
grep -E -h -o -w -i "\w*politiek\w*" tvg_111/*.txt | sort
```

De meeste matches zijn voor de woorden *politiek* en *politieke*. Om een lijst van de unieke patronen te zien, kun je de output van `sort` doorsturen naar het commando `uniq` die bij opeenvolgende regels met exact dezelfde inhoud alleen de eerste laat zien:

```bash
grep -E -h -o -w -i "\w*politiek\w*" tvg_111/*.txt | sort | uniq
```

Met wederom een parameter kun je `uniq` laten tellen hoe vaak elk patroon voorkomt, e.g. `-c` voor count:

```bash
grep -E -h -o -w -i "\w*politiek\w*" tvg_111/*.txt | sort | uniq -c
```

Deze lijst kun je ook weer sorteren:

```bash
grep -E -h -o -w -i "\w*politiek\w*" tvg_111/*.txt | sort | uniq -c | sort
```

<a name="grep-words-distributions"></a>
### Distributies en dispersie van specifieke woorden


TO DO:

+ uitzoomen naar hele TvG dataset: grep -r ./
+ de context van termen in TvG:
    + breng contexttermen in kaart per editie, vergelijk over edities
    + breng contexttermen in kaart per decennium, vergelijk over decennia
+ verspreiding van termen binnen een editie
+ verspreiding van termen over edities

<a name="grep-tr"></a>
### Transliteratie: eenvoudige datatransformaties voor normalisatie

Je ziet in de lijst matches dat sommige woorden zowel met als zonder hoofdletter voorkomen, zoals *verbroederingspolitiek* en *Verbroederingspolitiek*. Met het `tr` commando (voor *translate characters*) kun je algemene patronen definieren om individuele karakters te veranderen of verwijderen. Bijvoorbeeld, de hoofdletters vervangen door de corresponderende kleine letters. De set hoofdletters kun je aangeven met `[:upper:]`, de kleine met `[:lower:]`. Door dit `tr` commando uit te voeren alvorens het sorteren en tellen, worden variaties in hoofdlettergebruik samengevoegd tot een variant met uitsluitend kleine letters, zodat alle voorkomens van *verbroederingspolitiek* samen geteld worden:

```bash
grep -E -h -o -w -i "\w*politiek\w*" tvg_111/*.txt | tr '[:upper:]' '[:lower:]' | sort | uniq -c | sort
```

Je kunt de reguliere expressie verder uitbreiden om bijvoorbeeld het woord voorafgaand aan *politiek* te zien. Door `\w+ ` te plaatsen voor `politiek` zoek je naar het patroon *<woord> politiek*

```bash
grep -E -h -o -w -i "\w+ \w*politiek\w*" tvg_111/*.txt | tr '[:upper:]' '[:lower:]' | sort | uniq -c | sort
```

Om te kijken wat typische woorden zijn die voorafgaan aan *politiek* kun je met `tr` de spaties vervangen door een nieuwe regel, en vervolgens met `grep` de woorden die matchen met *politiek* wegfilteren. In dit geval roep je `grep` dus twee keer aan, eerst om voorkomens van *politiek* en voorafgaande woorden te matchen, en na wat normalisatie-stappen nogmaals om te concentreren op de voorafgaande woorden:

```bash
grep -E -h -o -w -i "\w+ \w*politiek\w*" tvg_111/*.txt | tr '[:upper:]' '[:lower:]' | tr ' ' '\n' | grep -v "politiek" | sort | uniq -c | sort
```

<a name="grep-word-sets"></a>
### Zoeken naar woordensets

Je kunt ook patronen definieren voor bijvoorbeeld sets van woorden die kwa betekenis dicht bij elkaar liggen, zoals *overheid*, *regering* en *kabinet*:

```bash
grep -E -o -w -i "\w*(overheid|regering|kabinet)\w*" tvg_111/*.txt | sort | uniq -c
```

In oudere teksten kom je vaak dubbele klinkers tegen, zoals in *regeering*. Als je zoekt in TvG editie 49, kun je ook *regeering* toevoegen:

```bash
grep -E -o -w -i "\w*(overheid|regeering|regering|kabinet)\w*" tvg_49/*.txt | sort | uniq -c
```

De alternatieven *regering* en *regeering* kun je ook korter noteren door de variatie *ee* en *e* aan te geven:

```bash
grep -E -o -w -i "\w*(overheid|reg(ee|e)ring|kabinet)\w*" tvg_49/*.txt | sort | uniq -c
```

<a name="grep-chararter-sets"></a>
### Zoeken met karaktersets

Je kunt ook sets van karakters opgeven als zoekpatroon. Bijvoorbeeld, `[a-z]` geeft aan *alle karakters van a tot en met z*, of `[a-p]` voor *alle karakters van a tot en met p*. Dit is hoofdlettergevoelig (tenzij je met `grep` de optie `-i` gebruikt voor *case-insensitive*), dus `[a-z]` is anders dan `[A-Z]`. Cijfers kunnen ook in sets gedefinieerd worden: `[0-9]` voor *0 tot en met 9* of `[2-4]` voor *2 tot en met 4*. Het is ook mogelijk om sets te maken met combinaties van kleine en hoofdletters en cijfers, e.g. `[a-gD-J3-7]` of `D-J3-7a-g]` (de volgorde van de subsets maakt niet uit).

zoeken naar losse hoofdletters:

```bash
grep -E -h --color -w "[A-Z]" tvg_111/*.txt
```

Zoeken naar woorden bestaande uit 2 of 3 hoofdletters:

```bash
grep -E -h --color -w "[A-Z]{2,3}" tvg_111/*.txt
```

Zoeken naar woorden bestaande 3 of meer hoofdletters:

```bash
grep -E -h --color -w "[A-Z]{3,}" tvg_111/*.txt
```

Zoeken naar woorden bestaande 3 of meer hoofdletters in de set *A tot en met G*:

```bash
grep -E -h --color -w "[A-G]{3,}" tvg_111/*.txt
```

Zoeken naar woorden bestaande uit 2 of 3 hoofdletters afgewisseld met punten:

```bash
grep -E -h --color -w "([A-Z]\.){2,3}" tvg_111/*.txt
```

Dit zouden afkortingen kunnen zijn, maar lijken ook wel de voorletters van namen. Hier komen we zo op terug. Eerst nog wat andere voorbeelden van karaktersets proberen.

Zoeken naar getallen met tenminste 3 cijfers:

```bash
grep -E -h --color -w "[0-9]{3,}" tvg_111/*.txt
```

Zoeken naar jaartallen:

```bash
grep -E -h --color -w "1[0-9]{3}" tvg_111/*.txt
```

Zoeken naar combinatiesets van hoofdletters en getallen:

```bash
grep -E -h --color -w "[A-G0-9]{3,}" tvg_111/*.txt
```

### Zoeken naar namen

Zoeken naar woorden die beginnen met een hoofdletter:

```bash
grep -E -h --color -w "[A-Z]\w+" tvg_111/*.txt
```

Het resultaat levert allerlei namen op, maar ook de eerste woorden van zinnen, die immers ook met een hoofdletter beginnen.

Er zijn ook dubbele namen met bijvoorbeeld een verbindingsteken. Je kunt dit ook opnemen in de reguliere expressie en zoeken naar woorden die beginnen met een hoofdletter en een combinatie van letters en verbindingstekens mogen bevatten:

```bash
grep -E -h --color -w "[A-Z](\w|-)+" tvg_111/*.txt
```

Dit levert ook samengestelde namen op, zoals Noord-Nederland. Je kunt `--color` vervangen door `-o` om alleen de matchende delen te tonen, zodat je een wat overzichtelijkere lijst krijgt:

```bash
grep -E -h -o -w "[A-Z](\w|-)+" tvg_111/*.txt
```

Zoeken naar een hoofdletter gevolgd door een punt, een spatie en dan een woord beginnend met een hoofdletter:

```bash
grep -E -h -o -w "[A-Z]\. [A-Z](\w|-)+" tvg_111/*.txt
```

Dit levert een lijst van persoonsnamen op. Waarschijnlijk zijn dit veelal auteurs van de verschillende artikelen, mededelingen en besproken boeken.

Soms word een voornaam afgekort tot twee letters, zoals *Th*. Het volgende patroon is voor zowel voornamen afgekort met een enkele hoofdletter als voor voornamen afgekort met een hoofdletter en een kleine letter:

```bash
grep -E -h -o -w "[A-Z][a-z]?\. [A-Z](\w|-)+" tvg_111/*.txt
```

Deze namen kun je sorteren en tellen met een combinatie van `sort` en `uniq`:

```bash
grep -E -h -o -w "[A-Z][a-z]?\. [A-Z](\w|-)+" tvg_111/*.txt | sort | uniq -c | sort
```

We zien nu "F. Van" als meest frequente naam. Dat is waarschijnlijk een incomplete naam. 

```bash
grep -E -h -o -w "[A-Z][a-z]?\.( [A-Z](\w|-)+)+" tvg_111/*.txt | sort | uniq -c | sort
```

Bovendien zijn er waarschijnlijk ook veel namen waarbij "van" met kleine letters is geschreven. Andere tussenwoorden die veel voorkomen zijn "de" "der" "den":

```bash
grep -E -h -o -w "[A-Z][a-z]?\.( van| de| der| den)*( [A-Z](\w|-)+)+" tvg_111/*.txt | sort | uniq -c | sort
```

Waarschijnlijk zijn er ook namen met meerdere voorletters:

```bash
grep -E -h -o -w "([A-Z][a-z]?\.)+( van| de| der| den)*( [A-Z](\w|-)+)+" tvg_111/*.txt | sort | uniq -c | sort
```

Als we willen zijn op welke pagina's die namen voorkomen, kunnen we de optie "-h" weghalen, zodat de bestandsnamen geprint worden:

```bash
grep -E -o -w "([A-Z][a-z]?\.)+( van| de| der| den)*( [A-Z](\w|-)+)+" tvg_111/*.txt
```

### Zoekresultaten opslaan in een bestand

Resultaten schrijven naar een bestand:

```bash
grep -E -o -w "([A-Z][a-z]?\.)+( van| de| der| den)*( [A-Z](\w|-)+)+" tvg_111/*.txt > persoonsnamen-tvg_111.txt
```

```bash
grep -E -o -w "([A-Z][a-z]?\.)+( van| de| der| den)*( [A-Z](\w|-)+)+" tvg_11[0-9]/*.txt | sed -E 's/.txt:/,/' | sed -E 's/tvg_[0-9]+\///' | sed -E 's/_page/,page/' > tvg_persoonsnamen.txt
```

gebruik sed om patronen te transformeren (e.g. markeren a la XML):



```bash
grep -E --colour -w "([A-Z](\.|\w+))+( van| de| der| den)*( [A-Z](\w|-)+)+" tvg_111/*.txt
```

```bash
grep -E -o -w "politiek\w+" tvg_111/*.txt
```


```bash
grep -E -h --color -w "neutraliteitspolitiek" tvg_*/*.txt | tr -d '[:punct:]' | tr '[:upper:]' '[:lower:]' | tr ' ' '\n' | sort | uniq -c | sort
```


