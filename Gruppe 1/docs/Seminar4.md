# Seminar 4
Erlend Langørgen  
21 september 2017  



## Forutsetninger for OLS

Dere vil se disse forutsetningene formulert på litt forskjellige måter i ulike metodetekster, men alle bygger på de samme matematiske formuleringene. Noen ganger vil dere også se at forutsetningene om restledd er utelatt, fordi det antas at man bruker en eller annen form for robuste standardfeil som passer til data, andre ganger vil dere kunne se en antagelse om at kurtosen ikke er uendelig stor. Noen vil kategorisere ingen innflytelsesrike observasjoner og ikke perfekt multikolinearitet som antagelser, mens andre vil kategorisere det som problemer. Jeg følger Field og Cristophersens forelesning her (tettere enn Skog):

1. Ingen utelatt variabelskjevhet
2. Lineær sammenheng mellom variablene
3. Ingen autokorrelasjon/Uavhengige observasjoner
4. Normalfordelte residualer
5. Homoskedastiske residualer
6. Ingen perfekt multikollinearitet




### Ingen utelatt variabelskjevhet
Hva innebærer denne antagelsen? 

* Dersom vi vil tolke alle variablene i modellen vår substansielt, må alle variabler som påvirker vår avhengige variabel, og som er korrelert med en uavhengig variabel inkluderes i modellen.
* Dersom vi vil tolke en uavhengig variabel, kan vi tenke på de resterende variablene som kontrollvariabler, som er korrelert med uavhengig variabel og påvirker avhengig variabel.

**Merk:** korrelasjon er lineær sammenheng mellom to variabler, ikke et årsaksforhold. Så lenge to variabler påvirker den avhengige variabelen, og de er korrelert (selv om de ikke påvirker hverandre på noe vis), får vi utelatt variabelskjevhet dersom vi ikke kontrollerer for den andre variabelen. 

Denne antagelsen kan vi ikke teste. Det finnes imidlertid metoder for å estimere effekten av utelatte variabler med ulike egenskaper. Denne formen for robusthetstesting kalles *sensivity analysis*

### Lineær sammenheng mellom variablene
Metoden vi bruker for å regne ut lineær regresjon tar blant annet utgangspunkt i kovarians mellom uavhengige variabler og avhengige variabler. I likhet med korrelasjon, er kovarians et mål på lineær sammenheng mellom to variabler. Derfor forutsetter lineær regresjon en lineær sammenheng mellom uavhengig av avhengig variabel. Brudd på denne forutsetningen kan potensielt gi svært missvisende resultater, f.eks. ved å gjøre en U-formet sammenheng om til *ingen lineær sammenheng*.

**Huskregel:** Hver gang vi opphøyer en uavhengig variabel, tillater vi en ekstra *sving* i sammenhengen mellom den avhengige og uavhengige variabelen. 

Dersom hypotesen vår er at det er en positiv sammenheng mellom to variabler, står vi fritt til å legge til andregradsledd og tredjegradsledd, osv, fordi vi ikke påstår at sammenhengen er perfekt lineær, bare at den er positiv. Dette er det vanligste. Vi står dermed fritt til å slenge inn andregrads og tredjegradsledd. Vær imidlertid forsiktig med å opphøye en uavhengig variabel for mye. Da står man i fare for **overfitting**, dvs. å finne en svært spesifikk sammenheng i datasettet ditt, som du ikke finner dersom du samler inn samme type data på nytt. 

I noen tilfeller er hypotesen vår er mer spesifikk, for eksempel at en sammenheng er U-formet (konveks), da må vi teste om: 

1. Vi får en U-formet sammenheng når vi legger inn et annengradsledd.
2. Om regresjonen med et andregradsledd passer til data.


Det finnes flere måter å teste linearitetsantagelsen på. Field gjør en grafisk test, ved å plotte residualene til den avhengige variabelen mot residualene til den uavhengige variabelen vi er interessert i. Jeg viser en annen test som gjør samme nytten, men som har noen fordeler.

**Viktig:** Dersom dere legger inn andregradsledd eller andre polynomer, husk på å tolke alle leddene for den variabelen sammen. Det er lettest å gjøre dette ved hjelp av plot (eller derivasjon for dem som er glad i matte).

### Uavhengighet/Ingen autokorrelasjon

Denne antagelsen holder dersom vi har et tilfeldig utvalg fra en populasjon, på et tidspunkt. Da vil observasjonene være statistisk uavhengige (alle observasjonene er trukket tilfeldig), og likt distribuert (alle observasjonene er trukket fra samme populasjon). Dersom vi ikke har et slikt utvalg, vil det kunne være sammenhenger mellom observasjoner. Dersom vi f.eks. har data for statsbudsjettet over tid, vil vi trolig se **autokorrelasjon** fra ett år til det neste fordi budsjettet endres inkrementelt. Andre typer avhengighet enn autokorrelasjon er også mulig, som geografisk avhengighet.  

### Normalfordelte residualer:
Residualene fra modellen er normalfordelt, og har gjennomsnitt tilnærmet lik 0. 

### Homoskedastiske residualer:
Variansen til residualene skal være konstante for ulike nivå av uavhengig variabel.

### Ingen perfekt multikolinearitet:
Det skal ikke være en perfekt lineær sammenheng mellom et sett av de uavhengige variablene. Dette fører til at regresjonen ikke lar seg estimere, og skyldes som regel at man har lagt inn dummyvariabler for alle kategorier av en variabel, som en dummy for mann og en for kvinne. Høy multikolinearitet kan også være problematisk, men er ikke en forutsetning for at regresjon ikke skal fungere.


## Regresjonsdiagnostikk i R
Jeg anbefaler `car` pakken til John Fox til regresjonsdiagnostikk. Den gir ikke like vakre figurer som `ggplot`, men er veldig lett å bruke for nybegynnere, og inneholder alle slags funksjoner man trenger for regresjonsdiagnostikk. På sikt kan dere lære dere å konstruere disse plottene selv med `ggplot`. Pass imidlertid på at dere forstår hva plot dere bruker faktisk innebærer (det er lov å spørre om hjelp i statistikk-kanalen vår på **slack**).

I tillegg til å teste antagelsene over (med unntak av antagelse 1), skal vi også se på innflytelsesrike observasjoner, og multikolinearitet. 

### Data - Burnside og Dollar 2000
I dag skal vi se på en ekte artikkel, og gjøre regresjonsdiagnostikk på denne.
Jeg har valgt en artikkel som tidligere var på pensum i fordypningsemnet i statistikk som holdes av Håvard Strand, jeg har altså hatt replikasjon av denne artikkelen som hjemmeoppgave. I tillegg til diagnostikken som vi gjør i seminaret, er det fint å se på deskriptiv statistikk, effektplot, samt diskusjon av data. Jeg skal laste opp et tilleggsnotat som dere kan se nærmere på med deskriptiv statistikk og diskusjon av data til denne artikkelen etterpå. Til sammen utgjør dermed dagens seminar og tilleggsnotatet fine ressurser for dem som er interessert i å gjøre replikasjon i hjemmeoppgaven sin. Dersom noen er interessert, kan jeg også tipse om noen nyttige artikler om replikasjon gjennom **slack**.

### Linearitet
Vi kan bruke funksjonen `ceresPlot()` fra pakken `car` til å teste om sammenhengen mellom en uavhengig og en avhengig variabel er lineær. Denne funksjonen fungerer både for lineær regresjon, og for logistisk regresjon (`glm`). Denne funksjonen fungerer imidlertid ikke for regresjon med samspill, ta kontakt med meg dersom dere vil teste linearitet i en regresjon med samspill, så kan jeg vise en annen metode for det. 

Det denne funksjonen gjør, er å legge sammen residualene fra en regresjon med parameterestimatet til en variabel (på y-aksen), og plotte mot variabelens verdi. Deretter tegnes det en grønn linje som passer data, dersom denne ikke er lineær, kan man prøve en transformasjon eller et polynom. 

### Uavhengighet
Man kan teste for autkorrelasjon med Durbin-Watson testen. En funksjon for dette er `durbinWatsonTest()`.

### Normalfordelte residualer:
Vi kan teste for normalfordelte residualer ved å plotte studentiserte residualer fra regresjonen vår mot kvantiler fra den kummulative normalfordelingen. Dette kalles qq-plot, og kan kjøres i R med `qqPlot()`.

**Studentiserte residualer:** Alternativ måte å standardisere på, i beregning av varians for hver enkelt observasjon, fjerner man observasjonen. Formålet med dette er at vi får statistisk uavhengighe mellom teller og nevner, noe som lar oss bruke residualene til statistiske tester.

### Homoskedastiske restledd:
Vi kan teste for heteroskedastisitet ved hjelp av plot av studentiserte residualer mot standardiserte predikerte verdier fra modellen. Dette kan gjøres med `spreadLevelPlot()`, dere kan også se på Martin sitt script fra seminar 3, der han bruker `ggplot()` i stedet.

### Multikolinearitet:
Vi kan teste for multikolinearitet ved hjelp av en vif-test. Funksjonen for dette er `vif()`. Med vif tester vi om det er en sterk lineær sammenheng mellom uavhengige variabler, dersom dette er tilfellet er det gjerne nødvendig med store mengder data for å skille effektene av ulike variabler fra hverandre, men bortsett fra å samle mer data er det ikke så mye vi gjøre dersom vi mener begge variablene må være med i modellen. Field skriver at vif over 10 er problematisk.

### Uteliggere og innflytelsesrike observasjoner
Innflytelsesrike observasjoner (leverage), er observasjoner som "trekker" regresjonslinjen mot seg med stor kraft. Disse observasjonene har gjerne uvanlige kombinasjoner av verdier på de uavhengige variablene, og predikeres gjerne dårlig av modellen. Vi bruker gjerne hatte-verdier som mål på innflytelsesrike observasjoner.  

Uteliggere er observasjoner som predikeres dårlig av modellen. Studentiserte residualer brukes ofte som mål på uteliggere. 

Det er ofte lurt å se nærmere på innflytelsesrike enheter og uteliggere, vi kan bruke `influenceIndexPlot()` til å identifisere slike observasjoner. Spesifiser hvor mange observasjoner du vil ha nummerert med argumentet `id.n = 5`. Deretter kan vi se nærmere på disse observasjonene ved hjelp av indeksering. En form for robusthetstesting er å kjøre regresjonen på nytt uten uteliggere og innflytelsesrike observasjoner, for å sjekke om man får samme resultat. Dersom man ikke gjør dette, er ikke resultatene dine særlig robuste.

Vi kan også se på Cook's distance, som kombinerer informasjon om uteliggere og innflytelsesrike observasjoner. `influenceIndexPlot()` gir oss alle disse målene.


## Logistisk regresjon:
Mange av metodene for diagnostikk som vi har sett på i dag fungerer også for logistisk regresjon. Jeg legger ut et eget notat med diagnostikk for logistisk regresjon som vil være mest relevant til hjemmeoppgaven. Dersom dere synes formen på presentasjonen her er fin, legger jeg opp på samme måte.


## Multinomisk logistisk regresjon:
Her skal jeg bare kort demonstrere hvordan man gjør multinomisk logistisk regresjon. Det finnes mange pakker for å gjøre dette i R, her bruker jeg funksjonen `multinom()` fra pakken `nnet`. For å slippe å laste inn flere datasett, og for å øve på omkoding har jeg laget en ny avhengig variabel med utgangspunkt i datasettet til Burnside og Dollar. Vi skiller mellom land som har negativ vekst, land som har lav vekst (under 1), land som har middels vekst (fra 1 til 3) og land som har høy vekst. Hvordan kan man gjøre denne omkodingen ved hjelp av `ifelse()`?











