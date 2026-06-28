
#### Wstęp

Celem tej części metodologii jest odtworzenie populacyjnych map metylacji dla kopalnych kohort walidacyjnych o jednoznacznym trybie życia, czyli dla grup *zbieracko-łowieckich (HG)* oraz *rolniczych (AGR)*, oraz dla populacji nie jednoznacznych, między innymi Çatalhöyük. 

<!-- Zmieniłem nawiasy na wtrącenia -->

Mapy te dostarczają wartości $m^j$ wykorzystywanych później w teście klasyfikacyjnym. Ponieważ pojedyncze próbki, uzyskane metodą "Twist Ancient DNA" mają zbyt niskie pokrycie do rekonstrukcji indywidualnej [@rohland2022three; @davidson2025optimized], mapy budujemy na poziomie populacji, łącząc odczyty wielu osobników z tej samej kohorty. Mapy posłużą potem do walidacji metody.

<!-- Zmieniłem metode na "Twist Ancient DNA", w artykułach które wysłałem jest no najbardziej optymalna metoda - szczegóły niżej:
Fragment: 
"We find that the “Twist Ancient DNA” assay provides robust enrichment
of approximately 1.2M target SNPs without introducing allelic bias that may interfere
with downstream population genetics analyses." - @ davidson2025optimized

"The Twist Ancient DNA assay was the most efficient of the
three in our experiments, capturing sequences overlapping almost
all targeted positions with relatively high homogeneity, achieving
higher coverage, and having the least allelic bias, making it most
easily coanalyzable with shotgun data at nearly all targeted SNPs.
Like Arbor Complete, the Twist Ancient DNA assay is commercially available. We have introduced a filter that tags the SNPs most affected by the bias in 1240k enrichment and that provides
confidence that Twist data will be robustly coanalyzable with the
great majority of ancient human DNA data generated to date."
- @rohland2022three

 -->

> Choć Çatalhöyük jest zwykle klasyfikowane jako osiedle rolnicze (AGR), jest przejściowe / niejednoznaczne pod względem strategii bytowania.


#### Procedura

<!-- Naniosłem znacznie zmiany, dodałem całą częśc z protokołem opisujacą pobieranie takich danych z kości z 2018 -->

Proces przetwarzania danych obejmuje ekstrakcję sygnału deaminacji, filtrowanie artefaktów, nieliniowe dopasowanie histogramów oraz wygładzanie sygnału metodą okien przesuwnych zgodnie z podejściem zaimplementowanym w pakietach referencyjnych [@barouch2024reconstructing; @mathov2025roam]. Poniżej opisano kolejne kroki potoku — od doboru kohort, przez przygotowanie danych sekwencyjnych i właściwą rekonstrukcję metylacji, po detekcję regionów różnicowo zmetylowanych — zachowując analogiczną strukturę względem części dotyczącej populacji współczesnych.


<!-- #TODO - fragment z opisem kohort doamy do części z kohortami, tam gdzie excel jest -->


<!-- CZĘŚĆ: To jest fragment z pobraniem materiału -->

##### Przygotowanie bibliotek aDNA: Ekstrakcja i konwersja
Procedura laboratoryjna przygotowania materiału kostnego oraz izolacji kopalnego DNA (aDNA) opiera się na protokole, który został opisany w [@rohland2018extraction], co pozwala na zminimalizowanie ryzyka kontaminacji współczesnym materiałem biologicznym. Proces ten obejmuje następujące kroki:

1. **Dekontaminacja powierzchniowa i pobranie próby:** W kroku tym zewnętrzna warstwa tkanki kostnej (części skalistej kości skroniowej lub zębów) zostaje mechanicznie oczyszczona za pomocą wiertła stomatologicznego lub poddana krótkiemu naświetlaniu promieniowaniem UV-C. Działanie to ma na celu usunięcie egzogennego DNA. Następnie z warstwy o najwyższej gęstości osteocytów wywierca się od 50 do 100 mg drobnego proszku kostnego, co pozwala maksymalnie zwiększyć stężenie endogennego aDNA.
<!-- fragment - `Procedure` - `Sample preparation`:
"Remove the surface of the bone or tooth sample with a disposable drill bit or another abrasive
disposable device or tool. Drill into the cleaned area using a fresh disposable drill bit and collect up
to 50 mg of sample powder into a 2.0-ml LoBind tube. If using a homogenizer or mortar and
pestle, grind the cleaned sample to fine powder and collect up to 50 mg into a 2.0-ml LoBind tube.
When extracting DNA from a sediment sample, weigh out up to 50 mg of material into a
2.0-ml LoBind tube."-->
2. **Liza i trawienie enzymatyczne:** W kroku tym uzyskany proszek kostny zostaje zawieszony w buforze ekstrakcyjnym zawierającym kwas etylenodiaminotetraoctowy (EDTA) oraz Proteinazę K. Zastosowanie EDTA pozwala na rozpuszczenie hydroksyapatytowej matrycy kości i uwolnienie uwięzionych w niej cząsteczek kwasów nukleinowych, natomiast Proteinaza K jako enzym proteolityczny trawi białka komórkowe, w tym kolagen i nukleazy. Inkubacja prowadzona w temperaturze $56^\circ\text{C}$ zapewnia całkowite uwolnienie uwięzionego DNA.
<!-- fragment - `Procedure` - `Sample lysis`:
"2 Add 1 ml of lysis buffer to each 50-mg sample. Include extraction negative controls by adding 1 ml
of lysis buffer to empty tubes; include at least one negative control per 11 samples. Optionally,
replace one sample with a positive control (Box 1).
3 Suspend the sample powder by vortexing for 10 s. Seal the tube with Parafilm and incubate
overnight (15–24 h) at 37 °C in an incubator under constant rotation on a tube rotator at 18 r.p.m.
4 Centrifuge the tubes for 2 min at 16,400 g in a table-top centrifuge to separate the lysate from
undigested sample material, and transfer the lysate to a fresh tube or directly to the binding buffer
(Step 5)."-->
3. **Wiązanie i oczyszczanie na kolumnach krzemionkowych:** W kroku tym zawiesina zostaje odwirowana, a supernatant wymieszany z buforem wiążącym o wysokim stężeniu soli chaotropowych (chlorowodorku guanidyny). Sól chaotropowa niszczy otoczkę hydratacyjną DNA, co umożliwia jego selektywne i silne związanie z membraną krzemionkową w mikrokolumnach. Po serii etapów przemywania buforami na bazie etanolu w celu usunięcia inhibitorów polimerazy, czyste aDNA zostaje wyemitowane przy użyciu niskosolnego buforu TE.
<!-- 
fragment - `Experimental design` -`Choice of purification matrix`
fragment - `Procedure` - `DNA purification`:
"Nie ma konkretnego kroku to streszczone jest, znaleźć w tekście"-->
4. **Przygotowanie bibliotek sekwencyjnych (Konwersja):** W kroku tym wyizolowane aDNA zostaje poddane procedurze ligacji specyficznych adapterów sekwencyjnych w formacie single-stranded (ssDNA) lub double-stranded (dsDNA) zgodnie z metodologią optymalizacji dla silnie zdegradowanych cząsteczek [@rohland2018extraction]. Ze względu na silną degradację post-mortem, cząsteczki te charakteryzują się obecnością pęknięć jednoniciowych oraz deaminacją cytozyny do uracylu na końcach fragmentów [@briggs2010removal]. Proces konwersji przygotowuje te uszkodzone cząsteczki do wydajnej amplifikacji PCR, nadając im unikalne indeksy identyfikujące próbkę.
<!-- fragment `Experimental design` - `Selection of binding buffer`:
Binding buffer G is thus preferred
only for samples from unfavorable climatic conditions, for which the DNA is expected to be
particularly degraded, or if yields need to be maximized, for example, if only minuscule amounts of
sample material are available. -->



<!-- CZĘŚĆ: To jest fragment z opisem hybrydyzacji -->

##### Wzbogacona hybrydyzacja in-solution (Twist Ancient DNA)
W kopalnych próbach kostnych endogenne aDNA stanowi zazwyczaj jedynie niewielki ułamek całkowitego wyekstrahowanego DNA ($\mathrm{wyekstrahowane\ DNA} \leq 10\%$) [@carpenter2013pulling; @rohland2022three], w którym dominują cząsteczki pochodzenia środowiskowego, takie jak DNA bakterii i grzybów glebowych [@carpenter2013pulling; @adler2013sequencing]. Aby rozwiązać problem niskiej zawartości matrycy, usunąć zanieczyszczenia mikrobiologiczne i uzyskać czyste dane o wysokiej specyficzności, w procesie tym zastosujemy technologię wzbogacania ukierunkowanego (*target enrichment*) w układzie *in-solution capture* przy użyciu dedykowanej platformy Twist Ancient DNA [@rohland2022three]. Metoda ta pozwala na wybiórcze wyłapanie ludzkich cząsteczek, co w przypadku bardzo słabo zachowanych próbek zwiększa frakcję DNA endogennego wielokrotnie w stosunku do sekwencjonowania bezpośredniego (shotgun), drastycznie zmniejszając tło sekwencyjne generowane przez organizmy egzogenne [@rohland2022three].
<!-- fragment (rohland2022three) - intruction:
"As an illustration of the power of in-solution enrichment,
consider studying an ancient individual at a set of about 600,000
single-nucleotide polymorphism (SNP) positions that have been
genotyped in diverse modern human populations. Only one in
about 100 ancient DNA sequences mapping to the human genome
will overlap these positions. If a DNA library is only 1% human,
the proportion of sequences that will be informative for analysis
will only be about one in 10,000. Thus, if about 400 million
DNA sequences are read from a library, which is a typical number
used to produce a ∼30× whole-human genome from modern DNA,
at most about 40,000 informative SNPs will be retrieved. In contrast, 25 million sequences from the same ancient DNA library after in-solution enrichment can provide coverage on nearly all
targeted SNP positions by multiple unique molecules, allowing accurate inferences about population history at much lower cost." -->

Fizykochemia tego procesu oraz mechanizm selekcji opierają się na następujących założeniach:
- **Fizyczna struktura sond i komplementarność:** W procesie tym zastosujemy biotynowane, jednoniciowe sondy RNA o długości 80 par zasad (bp), których sekwencja jest ściśle komplementarna do docelowych, ludzkich regionów genomowych [@davidson2025optimized]. Po denaturacji cieplnej biblioteki DNA, sondy hybrydyzują z komplementarnymi cząsteczkami aDNA. Kompleksy sonda-aDNA są następnie fizycznie wyłapywane z roztworu za pomocą kulek magnetycznych opłaszczonych streptawidyną, natomiast nieskomplementowane DNA środowiskowe zostaje całkowicie wymyte. Należy podkreślić, że technologia ta eliminuje kontaminację mikrobiologiczną, lecz nie usuwa zanieczyszczeń współczesnym DNA ludzkim ze względu na identyczność sekwencji; ich detekcja zachodzi na drodze bioinformatycznej.
- **Wpływ degradacji aDNA na długość odczytów:** Naturalna, pośmiertna fragmentacja, wynikająca z chemicznej hydrolizy wiązań fosfodiestrowych oraz aktywności nukleaz, powoduje, że cząsteczki aDNA są wysoce zdegradowane i ultratrótkie, a ich średnia długość w bibliotece często spada poniżej 80 bp [@rohland2018extraction]. Ponieważ sondy Twist mają długość 80 bp, w przeciwieństwie do 52 bp stosowanych w klasycznym panelu 1240K [@rohland2022three]. Zapewniają one wyższą stabilność termodynamiczną i wydajność hybrydyzacji z ultrakrótkimi fragmentami matrycy. Pozwala to na efektywne wiązanie cząsteczek, których fizyczna długość w roztworze uniemożliwia stabilne mapowanie przy użyciu krótszych sond. W rezultacie, po zmapowaniu odczytów do genomu referencyjnego, uzyskuje się fragmenty pokrywające pozycję docelową (SNP) wraz z jej bezpośrednim otoczeniem, pokrywające region o długości maksymalnie około 80-100 bp wokół pozycji docelowej [@rohland2022three].
<!-- fragment: to jest Figure 1 plot A -->
- **Charakterystyka funkcjonalna celów genomowych:** Sondy panelu Twist zostały zaprojektowane tak, aby celować w kluczowe pod względem ewolucyjnym i funkcjonalnym obszary ludzkiego genomu [@rohland2022three]. Struktura paneli obejmuje następujące komponenty:
  1. *Komponent populacyjny (Rdzeń 1240K):* Pozycje polimorficzne (SNP) na chromosomach autosomalnych oraz chromosomie X, które opisali Haak i in. (2015) oraz Olalde i in. (2018) [@haak2015arrival; @olalde2018beaker; @mathieson2015genome], kluczowe dla rekonstrukcji historycznych migracji, demografii oraz pokrewieństwa [@lazaridis2022genetic; @mittnik2019kinship]. 
  <!-- Tutaj wpisałem autorów osobno, można to wywalić i dac same artykuł€ -->
  1. *Regiony kodujące i regulatorowe (Zawartość fenotypowa):* Lokusy powiązane bezpośrednio z ekspresją cech fizycznych, wyselekcjonowane z baz takich jak ClinVar, GWAS czy PolyFun [@mathieson2015genome]. Pozwala to na bezpośrednią analizę funkcjonalnych zmian adaptacyjnych w sekwencjach kodujących białka oraz w regionach promotorowych i enhancerowych.
  2. *Regiony konserwatywne i epigenetyczne:* Obszary HAR (*Human Accelerated Regions*) oraz pozycje metylacyjne służące do analizy epigenetycznego starzenia się tkanek i rekonstrukcji starożytnych metylomów (Zegary Horvatha i Narasimhana) [@horvath2013dna; @narasimhan2023epigenetic], co koresponduje z modelami epigenetycznymi opracowanymi dla populacji kopalnych i współczesnych [@barouch2024reconstructing; @fagny2015epigenomic; @gokhman2014reconstructing].


<!-- CZĘŚĆ: Tutaj jest fragment z oddaniem enzymu USER -->

##### Biochemiczne generowanie asymetrii sygnału (Traktowanie enzymem USER)
W celu umożliwienia matematycznej rekonstrukcji przedśmiertnych wzorców metylacji, procedurę przygotowania bibliotek po etapie wzbogacenia Twist uzupełnia się o kontrolowane traktowanie kwasów nukleinowych enzymem USER (*Uracil-Specific Excision Reagent*) [@mathov2025roam]. Naturalna, pośmiertna deaminacja uszkadza kopalne DNA w dwojaki sposób: niemetylowane cytozyny przekształcają się w uracyle, podczas gdy metylowane 5-metylocytozyny (5mC) ulegają konwersji do tymin [@briggs2010removal]. Dodatek enzymu USER przed etapem amplifikacji PCR skutkuje selektywnym niszczeniem i wycinaniem cząsteczek uracylu (pochodzących z loci niemetylowanych), uniemożliwiając ich dalszą replikację i sekwencjonowanie [@briggs2010removal; @mathov2025roam]. Cząsteczki tyminy (pochodzące z pozycji metylowanych) pozostają nienaruszone przez enzym, dzięki czemu pomyślnie przechodzą proces amplifikacji, generując w odczytach sekwencyjnych asymetrię przejść $\text{C} \to \text{T}$ ściśle skorelowaną z pierwotnym stanem epigenetycznym [@mathov2025roam].

<!-- CZĘŚĆ: Naniosłem małe poprawki -->

##### Wstępne przetwarzanie odczytów i mapowanie do genomu referencyjnego

Surowe odczyty kopalnych bibliotek poddaje się kontroli jakości i przycinaniu adapterów (`FastQC`, `fastp`), a następnie mapuje do ludzkiego genomu referencyjnego (GRCh38). Należy uwzględnić, że odczyty aDNA są **bardzo krótkie** (zwykle $\approx30–70$ pz wskutek pośmiertnej fragmentacji), co utrudnia jednoznaczne mapowanie, ponieważ krótkie odczyty częściej dopasowują się wieloznacznie, szczególnie w regionach powtarzalnych. Z tego powodu w analizie aDNA standardem jest program mapujący `bwa aln`/`samse` (BWA-backtrack) [@li2009fast], który lepiej radzi sobie z krótkimi i uszkodzonymi odczytami niż `bwa mem` (zaprojektowany dla odczytów > $\approx70$ pz) [@meyer2012high]. Dodatkowo odrzuca się odczyty krótsze niż ~30–35 pz, stosuje filtr jakości mapowania (`MAPQ`) oraz rozluźnia parametry dopasowania lub przeskalowuje oceny jakości poszczególnych zasad (np. programem `mapDamage`), aby uwzględnić niedopasowania wynikające z deaminacji na końcach fragmentów [@schubert2012improving]. Zastosowanie sond panelu Twist łagodzi ten problem, ponieważ sondy celują w unikatowe, dobrze zdefiniowane loci wokół SNP (~100 pz), do których nawet krótkie odczyty mapują się względnie jednoznacznie. Powstałe pliki BAM sortuje się, indeksuje oraz oczyszcza z duplikatów PCR przy użyciu `samtools`, a wynik można skontrolować wizualnie w przeglądarce **IGV** [@mathov2025roam; @hanghoj2016fast]. Wyniki te są zapisywane w postaci plików `bam`, `bai`.

<!-- Te informacje śą w wtych artykułach w suplementach -->
<!-- fragment: A high coverage genome sequence from an archaic Denisovan individual: suplement strona 12 -->


<!-- CZĘŚĆ: Fragment poniżej został przeniesiony do `report/parts/methodology/dms_ancient.md` tutaj diodąłem część odpowiedzialną za filtering -->


##### Globalna ocena jakości i autentykacja próbek kopalnych
Po przeprowadzeniu sekwencjonowania wysokoprzepustowego (NGS) i wstępnego zmapowania, surowe dane w formacie BAM zostają poddane rygorystycznej procedurze filtracji całogenomowej w celu odrzucenia osobników o niskiej jakości lub wykazujących krytyczny poziom współczesnych zanieczyszczeń:

1. **Weryfikacja frakcji endogennej:** W kroku tym ocenia się ogólny stopień zachowania próby poprzez wyznaczenie stosunku odczytów mapujących się do ludzkiego genomu referencyjnego (GRCh38) do całkowitej liczby uzyskanych sekwencji [@hanghoj2016fast]. Próbki charakteryzujące się zbyt niską frakcją odczytów endogennych, wynikającą z przewagi tła środowiskowego (DNA mikroorganizmów glebowych), zostają bezwzględnie odrzucone z dalszych etapów obliczeniowych [@carpenter2013pulling; @adler2013sequencing].
2. **Kwantyfikacja kontaminacji współczesnym DNA ludzkim:** W kroku tym szacuje się poziom zanieczyszczenia współczesnym ludzkim DNA[@gokhman2014reconstructing; @barouch2024reconstructing]. W przypadku osobników płci męskiej analizuje się poziom heterozygotyczności na chromosomie X, natomiast dla całej kohorty bada się polimorfizm DNA mitochondrialnego. Dane te są procesowane za pomocą pakietów przeznaczonych do weryfikacji czystości starożytnych genomów [@mathov2025roam]. Próbki, w których oszacowany poziom kontaminacji współczesnym ludzkim DNA przekracza próg tolerancji ($>1-3\%$), zostają usunięte z ostatecznej kohorty.


