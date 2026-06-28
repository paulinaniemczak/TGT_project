
#### Wstęp

Celem tej części metodologii jest odtworzenie populacyjnych map metylacji dla kopalnych kohort walidacyjnych o jednoznacznym trybie życia, czyli dla grup *zbieracko-łowieckich (HG)* oraz *rolniczych (AGR)*, oraz dla populacji niejednoznacznych, między innymi Çatalhöyük. 

Mapy te dostarczają wartości $m^j$ wykorzystywanych później w teście klasyfikacyjnym. Ponieważ pojedyncze próbkimają zbyt niskie pokrycie do rekonstrukcji indywidualnej [@rohland2022three; @davidson2025optimized], mapy budujemy na poziomie populacji, łącząc odczyty wielu osobników z tej samej kohorty. Mapy posłużą potem do walidacji metody.

> Choć Çatalhöyük jest zwykle klasyfikowane jako osiedle rolnicze (AGR), jest przejściowe / niejednoznaczne pod względem strategii bytowania.


#### Procedura

Proces przetwarzania danych obejmuje ekstrakcję sygnału deaminacji, filtrowanie artefaktów, nieliniowe dopasowanie histogramów oraz wygładzanie sygnału metodą okien przesuwnych zgodnie z podejściem zaimplementowanym w pakietach referencyjnych [@barouch2024reconstructing; @mathov2025roam]. Poniżej opisano kolejne kroki potoku — od doboru kohort, przez przygotowanie danych sekwencyjnych i właściwą rekonstrukcję metylacji, po detekcję regionów różnicowo zmetylowanych — zachowując analogiczną strukturę względem części dotyczącej populacji współczesnych.

##### Przygotowanie bibliotek aDNA: Ekstrakcja i konwersja
Procedura laboratoryjna przygotowania materiału kostnego oraz izolacji kopalnego DNA (aDNA) opiera się na protokole, który został opisany w [@rohland2018extraction], co pozwala na zminimalizowanie ryzyka kontaminacji współczesnym materiałem biologicznym. Proces ten obejmuje następujące kroki:

1. **Dekontaminacja powierzchniowa i pobranie próby:** W kroku tym z zewnętrznej warstwy tkanki kostnej zostaje usunięte egzogenne DNA. Następnie z warstwy o najwyższej gęstości osteocytów wywierca się od 50 do 100 mg drobnego proszku kostnego, co pozwala maksymalnie zwiększyć stężenie endogennego aDNA.
2. **Liza i trawienie enzymatyczne:** Uzyskany proszek kostny zostaje zawieszony w buforze ekstrakcyjnym zawierającym EDTA oraz Proteinazę K. Pozwala to na rozpuszczenie matrycy kości i uwolnienie uwięzionych w niej cząsteczek kwasów nukleinowych.
3. **Wiązanie i oczyszczanie na kolumnach krzemionkowych:** W kroku tym zostaje wyemitowane czyste aDNA.
4. **Przygotowanie bibliotek sekwencyjnych (Konwersja):** Wyizolowane aDNA zostaje poddane procedurze ligacji specyficznych adapterów sekwencyjnych w formacie single-stranded (ssDNA) lub double-stranded (dsDNA) [@rohland2018extraction]. Proces konwersji przygotowuje uszkodzone cząsteczki do wydajnej amplifikacji PCR, nadając im unikalne indeksy identyfikujące próbkę.


##### Wzbogacona hybrydyzacja in-solution (Twist Ancient DNA)
W kopalnych próbach kostnych endogenne aDNA stanowi zazwyczaj jedynie niewielki ułamek całkowitego wyekstrahowanego DNA ($\mathrm{wyekstrahowane\ DNA} \leq 10\%$) [@carpenter2013pulling; @rohland2022three], w którym dominują cząsteczki pochodzenia środowiskowego, takie jak DNA bakterii i grzybów glebowych [@carpenter2013pulling; @adler2013sequencing]. Aby rozwiązać problem niskiej zawartości matrycy, usunąć zanieczyszczenia mikrobiologiczne i uzyskać czyste dane o wysokiej specyficzności, w procesie tym zastosujemy technologię wzbogacania ukierunkowanego (*target enrichment*) w układzie *in-solution capture* przy użyciu dedykowanej platformy Twist Ancient DNA [@rohland2022three]. Metoda ta pozwala na wybiórcze wyłapanie ludzkich cząsteczek, co w przypadku bardzo słabo zachowanych próbek zwiększa frakcję DNA endogennego wielokrotnie w stosunku do sekwencjonowania bezpośredniego (shotgun), drastycznie zmniejszając tło sekwencyjne generowane przez organizmy egzogenne [@rohland2022three].

Fizykochemia tego procesu oraz mechanizm selekcji opierają się na następujących założeniach:
- **Fizyczna struktura sond i komplementarność:** Stosujemy sondy RNA, których sekwencja jest ściśle komplementarna do docelowych, ludzkich regionów genomowych [@davidson2025optimized]. W kolejnym etapie, po denaturacji cieplnej biblioteki DNA, sondy hybrydyzują z komplementarnymi cząsteczkami aDNA. Kompleksy sonda-aDNA są następnie fizycznie wyłapywane z roztworu, natomiast nieskomplementowane DNA środowiskowe zostaje całkowicie wymyte. Należy podkreślić, że technologia ta eliminuje kontaminację mikrobiologiczną, lecz nie usuwa zanieczyszczeń współczesnym DNA ludzkim; ich detekcja zachodzi na drodze bioinformatycznej.
- **Wpływ degradacji aDNA na długość odczytów:** Po zmapowaniu odczytów do genomu referencyjnego, uzyskuje się fragmenty pokrywające pozycję docelową (SNP) wraz z jej bezpośrednim otoczeniem, pokrywające region o długości maksymalnie około 80-100 bp wokół pozycji docelowej [@rohland2022three].

- **Charakterystyka funkcjonalna celów genomowych:** Wybrane sondy zostały zaprojektowane tak, aby celować w kluczowe pod względem ewolucyjnym i funkcjonalnym obszary ludzkiego genomu [@rohland2022three]. Struktura paneli obejmuje następujące komponenty:
  1. *Komponent populacyjny (Rdzeń 1240K)* Kluczowe dla rekonstrukcji historycznych migracji, demografii oraz pokrewieństwa [@lazaridis2022genetic; @mittnik2019kinship]. 
  2. *Regiony kodujące i regulatorowe (Zawartość fenotypowa)* Lokusy powiązane bezpośrednio z ekspresją cech fizycznych, wyselekcjonowane z baz[@mathieson2015genome]. Pozwala to na bezpośrednią analizę funkcjonalnych zmian adaptacyjnych w sekwencjach kodujących białka oraz w regionach promotorowych i enhancerowych.
  2. *Regiony konserwatywne i epigenetyczne* Obszary HAR (*Human Accelerated Regions*) oraz pozycje metylacyjne służące do analizy epigenetycznego starzenia się tkanek i rekonstrukcji starożytnych metylomów [@horvath2013dna; @narasimhan2023epigenetic], co koresponduje z modelami epigenetycznymi opracowanymi dla populacji kopalnych i współczesnych [@barouch2024reconstructing; @fagny2015epigenomic; @gokhman2014reconstructing].


##### Wstępne przetwarzanie odczytów i mapowanie do genomu referencyjnego

Surowe odczyty kopalnych bibliotek poddaje się kontroli jakości i przycinaniu adapterów (`FastQC`, `fastp`), a następnie mapuje do ludzkiego genomu referencyjnego (GRCh38). Należy uwzględnić, że odczyty aDNA są **bardzo krótkie**, co utrudnia jednoznaczne mapowanie, ponieważ krótkie odczyty częściej dopasowują się wieloznacznie, szczególnie w regionach powtarzalnych. Z tego powodu w analizie aDNA standardem jest program mapujący `bwa aln`/`samse` (BWA-backtrack) [@li2009fast], który lepiej radzi sobie z krótkimi i uszkodzonymi odczytami niż `bwa mem` (zaprojektowany dla odczytów > $\approx70$ pz) [@meyer2012high]. Dodatkowo odrzuca się odczyty krótsze niż ~30–35 pz, stosuje filtr jakości mapowania (`MAPQ`) oraz rozluźnia parametry dopasowania lub przeskalowuje oceny jakości poszczególnych zasad (np. programem `mapDamage`), aby uwzględnić niedopasowania wynikające z deaminacji na końcach fragmentów [@schubert2012improving]. Zastosowanie sond panelu Twist łagodzi ten problem, ponieważ sondy celują w unikatowe, dobrze zdefiniowane loci wokół SNP (~100 pz), do których nawet krótkie odczyty mapują się względnie jednoznacznie. Powstałe pliki BAM sortuje się, indeksuje oraz oczyszcza z duplikatów PCR przy użyciu `samtools`, a wynik można skontrolować wizualnie w przeglądarce **IGV** [@mathov2025roam; @hanghoj2016fast]. Wyniki te są zapisywane w postaci plików `bam`, `bai`.


##### Globalna ocena jakości i autentykacja próbek kopalnych
Po przeprowadzeniu sekwencjonowania wysokoprzepustowego (NGS) i wstępnego zmapowania, surowe dane w formacie BAM zostają poddane rygorystycznej procedurze filtracji całogenomowej w celu odrzucenia osobników o niskiej jakości lub wykazujących krytyczny poziom współczesnych zanieczyszczeń:

1. **Weryfikacja frakcji endogennej:** W kroku tym ocenia się ogólny stopień zachowania próby poprzez wyznaczenie stosunku odczytów mapujących się do ludzkiego genomu referencyjnego (GRCh38) do całkowitej liczby uzyskanych sekwencji [@hanghoj2016fast]. Próbki charakteryzujące się zbyt niską frakcją odczytów endogennych, wynikającą z przewagi tła środowiskowego (DNA mikroorganizmów glebowych), zostają bezwzględnie odrzucone z dalszych etapów obliczeniowych [@carpenter2013pulling; @adler2013sequencing].
2. **Kwantyfikacja kontaminacji współczesnym DNA ludzkim:** W kroku tym szacuje się poziom zanieczyszczenia współczesnym ludzkim DNA[@gokhman2014reconstructing; @barouch2024reconstructing]. W przypadku osobników płci męskiej analizuje się poziom heterozygotyczności na chromosomie X, natomiast dla całej kohorty bada się polimorfizm DNA mitochondrialnego. Dane te są procesowane za pomocą pakietów przeznaczonych do weryfikacji czystości starożytnych genomów [@mathov2025roam]. Próbki, w których oszacowany poziom kontaminacji współczesnym ludzkim DNA przekracza próg tolerancji ($>1-3\%$), zostają usunięte z ostatecznej kohorty.
