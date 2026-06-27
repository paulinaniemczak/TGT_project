
#### Wstęp

Dotychczasowa charakterystyka różnic epigenetycznych między populacjami łowiecko-zbierackimi ($\text{HG}$) a rolniczymi ($\text{AGR}$) opiera się przede wszystkim na analizie jednej pary populacji z jednego regionu — afrykańskich łowców-zbieraczy lasu deszczowego oraz sąsiadujących z nimi rolników [@fagny2015epigenomic]. Oparcie referencyjnych sygnatur metylacji na pojedynczej parze populacji wiąże się z dwoma istotnymi ograniczeniami.

Po pierwsze, mała liczba niezależnych populacji ogranicza moc statystyczną i wiarygodność identyfikowanych miejsc różnicowo metylowanych ($\text{DMS}$). Po drugie, różnice obserwowane między jedną parą $\text{HG}/\text{AGR}$ mogą być zaburzane przez pochodzenie genetyczne, lokalną historię demograficzną oraz specyfikę środowiska, w tym klimat lasu równikowego. W takim układzie trudno rozstrzygnąć, czy wykryty sygnał odzwierciedla rzeczywisty efekt trybu życia, czy raczej lokalną różnicę typu „populacja A vs. populacja B”.

Rozwiązaniem tego problemu jest włączenie wielu niezależnych par $\text{HG}/\text{AGR}$ pochodzących z różnych kontynentów i stref klimatycznych. Sygnatury epigenetyczne powtarzalne w wielu takich porównaniach z dużo większym prawdopodobieństwem będą odzwierciedlały uniwersalny komponent związany z trybem życia, a nie lokalne uwarunkowania genetyczne, demograficzne lub środowiskowe.

> #TODO_6 - Marcin - pewnie będą potrzebne jeszcze propozycje konkretnych populacji tutaj.

#### Procedura

##### Część I - metodologia do badania DNA z populacji dzisiejszych

1. **Pozyskanie próbek**

   - **Tkanka**: materiał kostny pozyskany w trakcie przeprowadzania procedur medycznych
   - **Liczebność**: 1-2% osób na populację
   - **Kryteria włączenia**: dorośli, znany wiek i płeć, brak bliskiego pokrewieństwa, bez ostrej infekcji w chwili poboru
   - **Etyka**: świadoma zgoda i zasady suwerenności danych społeczności (CARE/OCAP) — warunek konieczny
   - **Metadane**: wiek, płeć, region, dieta/tryb życia — potrzebne jako współzmienne w modelu DMS


2. **Ekstrakcja i kontrola jakości DNA**

   - Izolacja genomowego DNA protokołem mogącym być również zastosowanym do izolacji DNA antycznego (kolumienki krzemionkowe z buforem zawierającym EDTA i proteinazę K [@rohland2007extraction])
   - Kontrola ilości (fluorymetria) i integralności (elektroforeza); minimalny wkład zwykle $\ge 250\text{–}500$ ng na próbkę


3. **Konwersja dwusiarczynowa (bisulfite conversion)**

   - niemetylowana cytozyna $\rightarrow$ uracyl $\rightarrow$ (po PCR) tymina
   - metylowana 5mC pozostaje cytozyną
   - Dzięki temu stan metylacji zamienia się w różnicę sekwencji C/T. Stosować zestaw o wysokiej wydajności konwersji ($\geq; \sim 99\%$); zalecane powtórzenia techniczne do oceny powtarzalności.


4. **Analiza danych o metylacji DNA**

   Próbki poddamy hybrydyzacji przy użyciu mikromacierzy HumanMethylation450, dla próbek i powtórzeń technicznych. Następnie wyeliminujemy sondy, które mogą ulec hybrydyzacji krzyżowej (te znajdujące się na chromosomach X i Y oraz sondy zawierające SNP lub powiązane z CpG zawierającymi SNP, których częstotliwość przekracza 1% w co najmniej jednej z badanych populacji). Po tym procesie filtrowania poziom metylacji zostanie obliczony na podstawie surowych danych, korzystając z pakietu R Bioconductor `lumi`. Należy wybrać, czy lepiej nada się wartość M, czy beta, sprawdzając, która z nich da lepszą czułość [@du2010comparison]. Wybrana wartość zostanie skorygowana pod kątem tła i odchylenia za pomocą `lumi` i znormalizowana kwantylowo. Różnice techniczne zostaną skorygowane przez normalizację wybranej wartości w ramach macierzy przy użyciu podzbiorów kwantylowych za pomocą pakietu R Bioconductor `minifi60`. Analizą PCA wykryjemy batch-effect, do skorygowania czego wykorzystamy funkcję `ComBat` z pakietu sva bioconductor.


5. **Określenie miejsc o zróżnicowanym stopniu metylacji - DMS**

   Określenie tych miejsc między populacjami zostanie zmodyfikowane statystycznie poprzez dopasowanie modelu regresji liniowej dla każdego miejsca (wartości M: populacja x płeć x wiek x proporcje typów komórek x błąd) oraz zastosujemy wygładzenie empiryczne Bayesa do odchylenia standardowego (przy użyciu pakietu `limma` w bibliotece R bioconductor). Miejsca o skorygowanej wartości P poniżej 0,01 według Benjamini i Hochberga uznaje się za różnicowo metylowane. 

   Aby zdefiniować amplitudę DMS, zastosujemy następujące kryteria: skorygowaną wartość P poniżej 0,01 według Benjamini i Hochberga oraz różnicę w średnim poziomie metylacji między dwiema populacjami wynoszącą ponad 2, 5 lub 10%. W tego typu analizie poziom metylacji określa się jako stosunek intensywności sygnału z sondy metylowanej do intensywności ogólnej, czyli wartość beta. Wyodrębnimy nakładające się fragmenty między różnymi zestawami DMS i obliczymy wartości P mierzące prawdopodobieństwo uzyskania tych nakładających się fragmentów przez przypadek, stosując ponowne próbkowanie. Poziomy metylacji DNA w docelowych miejscach są silnie skorelowane w obrębie regionów n o określonej wielkości (około 2000 pz). W związku z tym dla każdej listy DMS losowo pobierzemy ponownie taką samą liczbę spośród wszystkich miejsc, biorąc pod uwagę odległość między DMS.


6. **Funkcje biologiczne genów o zróżnicowanym stopniu metylacji**

   Wyodrębnimy wszystkie geny o zróżnicowanym stopniu metylacji, czyli geny posiadające co najmniej jeden DMS. Do tego celu wykorzystamy pakiet `goseq` z biblioteki R Bioconductor oraz do przeprowadzenia analizy nadreprezentacji kategorii ontologii genów wśród genów o zróżnicowanym stopniu metylacji. Liczbę sond odpowiadających każdemu genowi wprowadzimy do funkcji ważenia prawdopodobieństwa pakietu `goseq`. Ponieważ nie wszystkie geny genomu są reprezentowane na chipie Illumina HumanMethy450 BeadChip, zestaw referencyjny w analizie nadreprezentacji będzie składał się z genów, dla których dostępne są dane. Zbiory DMS będą istotnie wzbogacone w danej kategorii, jeśli wartość P skorygowana o FDR wyniesie maksymalnie 0,05.

> #TODO - Paulina - dopisanie dokładne metodologi do konca (metodologia tych badań które byly wykorzystane do autochtonów afrykańskich)z
> #TODO - Paulina to można rozibć na pojedyncze fragmenty:
<!-- > 
> ##### Konwersja dwusiarczynowa (Bisulfite Conversion)
> ##### Przetwarzanie i normalizacja danych mikromacierzowych
> ##### Identyfikacja miejsc różnicowo zmetylowanych (DMS)
> ##### Analiza funkcjonalna i ontologia genów -->


##### Część II - metodologia do badania DNA z populacji antycznych

**Kontrola jakości**

   Dla porównania skuteczności, zostanie dodatkowo przeprowadzona analiza wykorzystująca sztucznie zanieczyszczone i pofragmentowane sekwencje współczesne, symulujące aDNA. 

   Przygotowane dane zostaną poddane ocenie jakości przy wykorzystaniu programu FastQC. Ewentualne filtrowanie i przycinanie sekwencji o zbyt niskiej jakości zostanie przeprowadzone wykorzystując program fastp. Odczyty zostaną zmapowane do genomu referencyjnego człowieka (GRCh37/hg19) programem BWA. Obce sekwencje usunięte zostaną narzędzeim BlobTools. 
