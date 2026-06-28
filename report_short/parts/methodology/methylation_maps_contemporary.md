#### Wstęp

Celem tej części metodologii jest wygenerowanie genomowych map metylacji DNA dla współczesnych kohort referencyjnych o jednoznacznym trybie życia: populacji łowiecko-zbierackich (HG) oraz rolniczych (AGR). Mapy te stanowią sygnaturę odniesienia do wyodrębnienia komponentu epigenetycznego związanego z trybem życia oraz punkt kalibracyjny dla map rekonstruowanych z materiału kopalnego. Zgodnie z założeniem rozszerzenia analizy poza pojedynczą parę populacji afrykańskich [@fagny2015epigenomic], profilowanie obejmuje wiele niezależnych par HG/AGR z różnych kontynentów i stref klimatycznych, co zwiększa moc statystyczną i pozwala oddzielić uniwersalny efekt trybu życia od lokalnych uwarunkowań genetycznych, demograficznych i środowiskowych.

> Uwaga metodologiczna: w odróżnieniu od Fagny i in. [@fagny2015epigenomic] gdzie dane pochodzą z mikromacierzy (Illumina 450K, wartości $\beta$), aby uzyskać rozdzielczość pojedynczego CpG w skali całego genomu, wybrano enzymatyczne sekwencjonowanie metylacji (EM-seq), zapewniające profilowanie całogenomowe [@vaisvila2021emseq].


#### Procedura

##### Pozyskanie materiału i ekstrakcja DNA

Materiałem referencyjnym jest współczesna tkanka kostna (kość zbita lub część skalista kości skroniowej — *petrous bone*) ze współczesnego materiału szkieletowego/sekcyjnego. Wybór tkanki kostnej, a nie krwi obwodowej zastosowanej w oryginalnej analizie referencyjnej [@fagny2015epigenomic], wynika z silnej tkankowej specyficzności metylacji DNA: mapy współczesne muszą pochodzić z tej samej tkanki co kopalne aDNA (materiał szkieletowy), aby porównanie HG vs. AGR nie było obciążone różnicą typu tkanki. Ze względu na heterogeniczność komórkową tkanki kostnej (osteoblasty, osteocyty, osteoklasty, domieszka szpiku) równolegle szacuje się skład komórkowy próbki do późniejszej korekty. Wyizolowane DNA kontroluje się pod kątem ilości i integralności, a masę wejściową dobiera zgodnie z protokołem przygotowania bibliotek EM-seq [@vaisvila2021emseq].


##### Konwersja enzymatyczna i przygotowanie bibliotek (EM-seq)

EM-seq rozróżnia cytozyny metylowane i niemetylowane w dwóch krokach enzymatycznych zamiast chemicznej konwersji dwusiarczynowej [@vaisvila2021emseq]. Najpierw enzymy z rodziny TET (TET2) wraz z utleniaczem chronią 5-metylocytozynę (5mC) i 5-hydroksymetylocytozynę przed deaminacją, następnie deaminaza APOBEC deaminuje wyłącznie niemodyfikowane cytozyny do uracyli. Po amplifikacji i sekwencjonowaniu pozycje niemetylowane odczytywane są jako tymina (T), a metylowane pozostają cytozyną (C) — sygnał analogiczny do metody dwusiarczynowej, lecz przy łagodniejszej obróbce DNA i wyższej wydajności mapowania oraz pokrycia [@vaisvila2021emseq].


##### Mapowanie i budowa macierzy zliczeń CpG

Surowe odczyty poddaje się kontroli jakości oraz przycinaniu adapterów i zasad o niskiej jakości (`FastQC`, `fastp`), a następnie mapuje do ludzkiego genomu referencyjnego (GRCh38) programem uwzględniającym konwersję (`Bismark` lub `BWA-Meth`) w ramach potoku `nf-core/methylseq` [@krueger2011bismark; @pedersen2014bwameth; @ewels2020nfcore]. Wybór GRCh38 zapewnia spójność współrzędnych z mapami kopalnymi. Pliki BAM sortuje się, indeksuje i oczyszcza z duplikatów PCR (`samtools`), a zbiorcze raporty jakości agreguje `MultiQC` [@ewels2016multiqc]. Sygnał metylacji ekstrahuje się na poziomie pojedynczych pozycji CpG (`MethylDackel` lub moduł `Bismark`), uzyskując liczbę odczytów metylowanych i niemetylowanych [@krueger2011bismark]. Komplementarne pomiary z obu nici łączy się (*strand merging*), a pozycje o pokryciu poniżej progu odrzuca. Wynikiem jest macierz zliczeń CpG (poziom metylacji wraz z pokryciem) stanowiąca właściwą mapę metylacji.


##### Filtrowanie pozycji i kontrola jakości

Z macierzy usuwa się pozycje pokrywające się ze znanymi polimorfizmami pojedynczego nukleotydu (maskowanie SNP) oraz regiony problematyczne (*blacklist*), ponieważ wariant genetyczny w miejscu CpG może imitować zmianę stanu metylacji; wpływ pochodzenia genetycznego kontroluje się przez niezależne genotypowanie osobników [@fagny2015epigenomic]. Następnie koryguje się dwie kluczowe zmienne zakłócające — heterogeniczność składu komórkowego (proporcje typów komórek tkanki kostnej) oraz wiek osobnika [@fagny2015epigenomic]. Przed przekazaniem map do detekcji DMS przeprowadza się eksploracyjną kontrolę jakości (rozkład pokrycia i globalnych poziomów metylacji, PCA/MDS, grupowanie próbek, efekty wsadowe) oraz weryfikuje wydajność konwersji enzymatycznej (kontrola lambda/pUC19); próbki odstające lub o niewystarczającej jakości są odrzucane [@vaisvila2021emseq].
