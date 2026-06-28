#### Wstęp

Celem tej części metodologii jest wygenerowanie genomowych map metylacji DNA dla współczesnych kohort referencyjnych o jednoznacznym trybie życia - populacji łowiecko-zbierackich (HG) oraz rolniczych (AGR). Mapy te stanowią sygnaturę odniesienia, na podstawie której w dalszej części wyodrębniany jest komponent epigenetyczny związany z trybem życia, oraz punkt kalibracyjny dla map rekonstruowanych z materiału kopalnego.

Zgodnie z założeniem rozszerzenia analizy poza pojedynczą parę populacji afrykańskich [@fagny2015epigenomic], profilowanie obejmuje wiele niezależnych par HG/AGR z różnych kontynentów i stref klimatycznych, co zwiększa moc statystyczną oraz pozwala oddzielić uniwersalny efekt trybu życia od lokalnych uwarunkowań genetycznych, demograficznych i środowiskowych.

> Uwaga metodologiczna: oryginalne dane Fagny i in. [@fagny2015epigenomic] pochodzą z macierzy mikromacierzowej (Illumina 450K, wartości $\beta$). W niniejszej pracy, aby uzyskać rozdzielczość pojedynczego CpG w skali całego genomu oraz dane w postaci zliczeń odczytów zgodne z testami różnicowymi opartymi na modelach zliczeniowych (sekcja DMS), wybrano technikę sekwencyjną — **enzymatyczne sekwencjonowanie metylacji (EM-seq)**. Macierz 450K pokrywa jedynie ułamek pozycji CpG genomu, podczas gdy EM-seq zapewnia profilowanie całogenomowe [@vaisvila2021emseq].


#### Procedura

Proces obejmuje pozyskanie materiału i ekstrakcję DNA, enzymatyczną konwersję i przygotowanie bibliotek (EM-seq), wstępne przetwarzanie i mapowanie odczytów, ekstrakcję sygnału metylacji i budowę macierzy zliczeń CpG, a następnie filtrowanie oraz eksploracyjną kontrolę jakości. \


##### Pozyskanie materiału i ekstrakcja DNA

Materiał referencyjny stanowi współczesna tkanka kostna (preferencyjnie kość zbita lub część skalista kości skroniowej — *petrous bone*), pozyskiwana ze współczesnego materiału szkieletowego/sekcyjnego. Wybór tkanki kostnej, a nie krwi obwodowej zastosowanej w oryginalnej analizie referencyjnej [@fagny2015epigenomic], jest podyktowany silną tkankową specyficznością metylacji DNA: mapy współczesne muszą pochodzić z tej samej tkanki co kopalne aDNA (materiał szkieletowy), aby porównanie HG vs. AGR nie było obciążone różnicą typu tkanki. Ze względu na heterogeniczność komórkową tkanki kostnej (m.in. osteoblasty, osteocyty, osteoklasty oraz domieszka komórek szpiku) równolegle szacuje się skład komórkowy próbki, co umożliwia późniejszą korektę tej heterogeniczności. Wyizolowane DNA genomowe poddaje się kontroli ilości i integralności, a wymaganą masę wejściową dobiera się zgodnie z protokołem przygotowania bibliotek EM-seq, który pozwala na pracę z niewielkimi ilościami materiału [@vaisvila2021emseq].


##### Konwersja enzymatyczna i przygotowanie bibliotek (EM-seq)

Do rozróżnienia cytozyn metylowanych i niemetylowanych stosujemy enzymatyczne sekwencjonowanie metylacji (EM-seq), w którym konwersja zachodzi w dwóch krokach enzymatycznych zamiast chemicznej konwersji dwusiarczynowej [@vaisvila2021emseq]. W pierwszym kroku enzymy z rodziny TET (TET2) wraz z utleniaczem przekształcają 5-metylocytozynę (5mC) i 5-hydroksymetylocytozynę do form chroniących te zasady przed deaminacją. W drugim kroku deaminaza APOBEC deaminuje wyłącznie niemodyfikowane cytozyny do uracyli. W rezultacie, po amplifikacji i sekwencjonowaniu, pozycje niemetylowane odczytywane są jako tymina (T), a pozycje metylowane pozostają cytozyną (C), generując sygnał metylacji analogiczny do uzyskiwanego metodą dwusiarczynową, lecz przy łagodniejszej obróbce DNA i wyższej wydajności mapowania oraz pokrycia [@vaisvila2021emseq].


##### Wstępne przetwarzanie odczytów i mapowanie do genomu referencyjnego

Surowe odczyty poddaje się kontroli jakości oraz przycinaniu adapterów i zasad o niskiej jakości (`FastQC`, `fastp`), a następnie mapuje do ludzkiego genomu referencyjnego (GRCh38) z wykorzystaniem programu mapującego uwzględniającego konwersję (`Bismark` lub `BWA-Meth`), zorganizowanego w ramach zautomatyzowanego potoku `nf-core/methylseq` [@krueger2011bismark; @pedersen2014bwameth; @ewels2020nfcore]. Wybór genomu GRCh38 zapewnia spójność współrzędnych z mapami kopalnymi oraz z danymi referencyjnymi. Powstałe pliki BAM sortuje się, indeksuje i oczyszcza z duplikatów PCR przy użyciu `samtools`, a wynik można skontrolować wizualnie w przeglądarce **IGV**; zbiorcze raporty jakości z poszczególnych etapów agreguje się narzędziem `MultiQC` [@ewels2016multiqc].

##### Ekstrakcja sygnału metylacji i budowa macierzy zliczeń CpG

Po zmapowaniu sygnał metylacji ekstrahuje się na poziomie pojedynczych pozycji CpG (`MethylDackel` lub moduł ekstrakcji metylacji `Bismark`), uzyskując dla każdej pozycji liczbę odczytów metylowanych i niemetylowanych [@krueger2011bismark]. Komplementarne pomiary z obu nici DNA dla tej samej pozycji CpG łączy się (*strand merging*), a pozycje o pokryciu poniżej przyjętego progu odrzuca się, aby ograniczyć szum wynikający z niskiej liczby odczytów. Wynikiem tego kroku jest macierz zliczeń CpG (poziom metylacji wyrażony jako udział odczytów metylowanych wraz z pokryciem), stanowiąca właściwą mapę metylacji wykorzystywaną w dalszej analizie.


##### Filtrowanie pozycji, maskowanie SNP i korekta zmiennych zakłócających

Aby ograniczyć artefakty, z macierzy usuwa się pozycje pokrywające się ze znanymi polimorfizmami pojedynczego nukleotydu (maskowanie SNP) oraz pozycje w regionach problematycznych (listy wykluczeń, *blacklist*), ponieważ wariant genetyczny w miejscu CpG może imitować zmianę stanu metylacji. W celu kontroli wpływu pochodzenia genetycznego na metylację osobniki genotypuje się niezależnie [@fagny2015epigenomic].

Następnie koryguje się dwie kluczowe zmienne zakłócające: heterogeniczność składu komórkowego (na podstawie oszacowanych proporcji typów komórek tkanki kostnej) oraz wiek osobnika, zgodnie z podejściem zastosowanym dla populacji referencyjnych [@fagny2015epigenomic].


##### Eksploracyjna kontrola jakości map populacyjnych

Przed przekazaniem map do detekcji DMS przeprowadza się eksploracyjną kontrolę jakości: ocenę rozkładu pokrycia, globalnego rozkładu poziomów metylacji, analizę głównych składowych (PCA/MDS), grupowanie próbek oraz ocenę efektów wsadowych (*batch effects*). Dodatkowo, na podstawie kontroli konwersji (lambda/pUC19) weryfikuje się wydajność konwersji enzymatycznej dla każdej biblioteki [@vaisvila2021emseq]. Próbki odstające lub o niewystarczającej jakości (niskie pokrycie, nieprawidłowa wydajność konwersji) są odrzucane z dalszej analizy.
