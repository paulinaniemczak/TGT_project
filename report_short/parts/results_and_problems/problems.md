#### Znaczące różnice w epigenomach pomiędzy współczesnymi ludnościami autochtonicznymi oraz ludnością prehistoryczną

Może dojść do sytuacji, że różnice w DMS pomiędzy współczesnymi ludnościami autochtonicznymi i prehistorycznymi są na tyle duże, że nie znajdziemy żadnego przecięcia. Może to wynikać nie tylko z znaczących różnic środowiskowych, ale także z ewentualnych zanieczyszczeń. Trzeba by wtedy lepiej dobrać populację testową.

#### Różnice tkankowe między materiałem referencyjnym a kopalnym

Pierwszym istotnym problemem jest różnica tkankowa między materiałem referencyjnym a kopalnym. DMS (*differentially methylated site*) identyfikowane we współczesnej krwi byłyby następnie walidowane na DNA pozyskanym z kości, podczas gdy wzorce metylacji są silnie zależne od typu tkanki. Aby ograniczyć to ryzyko, analizę należy zawęzić do pozycji względnie stabilnych międzytkankowo, w tym potencjalnie do metastabilnych epialleli.

<!-- KOMENTARZ: 
To jest dobra uwaga-->

#### Reprezentatywność współczesnych populacji autochtonicznych

Drugie ograniczenie dotyczy reprezentatywności współczesnych populacji autochtonicznych jako modeli populacji prehistorycznych. Dzisiejsze grupy określane jako HG często przeszły kontakt kulturowy, zmiany diety, presję demograficzną oraz ekspozycje środowiskowe odmienne od tych, które charakteryzowały populacje pradziejowe. W konsekwencji nie mogą być traktowane jako wierne odpowiedniki dawnych łowców-zbieraczy, lecz jedynie jako przybliżone populacje referencyjne.

<!-- KOMENTARZ: 
To jest dobra uwaga-->


#### Czynniki konfudujące

Trzecim ryzykiem są konfudery związane z pochodzeniem genetycznym, klimatem, środowiskiem lokalnym oraz efektami technicznymi, takimi jak partia eksperymentalna. Sygnał interpretowany jako „HG vs. AGR” może w rzeczywistości odzwierciedlać strukturę populacyjną, lokalną historię demograficzną albo specyficzne warunki środowiskowe. Mitygacja tego problemu wymaga kontroli współzmiennych, uwzględnienia *cis*-meQTL oraz replikacji w wielu niezależnych parach populacji HG/AGR z różnych regionów geograficznych. (Inna genetyka populacji, a nie tylko epigenetyka)

<!-- KOMENTARZ: 
konfundery .... 
To rozumiem że uwzględnimy w metodologii żeby do tego nie doszło - można to przenieść -->

#### Specyfika starożytnego DNA (aDNA)

Czwarta grupa ograniczeń wynika ze specyfiki starożytnego DNA. Deaminacja cytozyn, niskie pokrycie oraz fragmentacja cząsteczek mogą prowadzić do błędnej estymacji proporcji $C \to T$, a tym samym do fałszywie dodatnich DMR. Minimalnym zabezpieczeniem powinno być zastosowanie progu efektywnego pokrycia, na przykład $\ge 12$, wygładzanie sygnału w oknach genomowych oraz kontrola FDR oparta na permutacjach.

<!-- KOMENTARZ: To rozumiem że uwzględnimy w metodologii żeby do tego nie doszło - można to przenieść -->

#### Kontaminacja współczesnym DNA

Kolejnym problemem jest kontaminacja współczesnym DNA, która może zaniżać lub zniekształcać rekonstruowany sygnał metylacji. Konieczna jest zatem walidacja profilu uszkodzeń, na przykład z użyciem `mapDamage`, oraz zastosowanie filtrów kontaminacji dostosowanych do typu materiału i jakości biblioteki.

<!-- KOMENTARZ: To rozumiem że uwzględnimy w metodologii żeby do tego nie doszło - można to przenieść -->
<!-- KOMENTARZ: Tego nie rozumiem, mówiliśmy już kilkakrotnie że będziemy to pobierać z kości bo inaczej nie ma to sensu -->

#### Wymienność etykiet w testach permutacyjnych

Dodatkowym problemem jest wymienność etykiet w testach permutacyjnych. Jeżeli osobnicy różnią się nie tylko trybem życia, lecz także pochodzeniem, wiekiem próbki, regionem lub jakością zachowania DNA, losowe permutowanie etykiet HG/AGR może prowadzić do obciążonych wartości $p$. Permutacje powinny być zatem wykonywane w ramach bloków kontrolujących strukturę danych, na przykład region, okres chronologiczny, partię eksperymentalną lub poziom pokrycia.

<!-- KOMENTARZ: To rozumiem że uwzględnimy w metodologii żeby do tego nie doszło - można to przenieść -->

#### Brak niezależnego złotego standardu (Ryzyko cyrkularności)

Wreszcie, metoda nie dysponuje niezależnym złotym standardem. Status HG/AGR populacji kopalnych pochodzi z interpretation archeologicznej, czyli z tego samego typu informacji, który metoda miałaby częściowo uzupełniać lub weryfikować. Rodzi to ryzyko cyrkularności. Dlatego klasyfikacja epigenetyczna powinna być traktowana nie jako samodzielny dowód trybu życia, lecz jako dodatkowa warstwa informacji integrowana z archeologią, izotopami stabilnymi, paleogenomiką oraz kontekstem środowiskowym.

<!-- Nie rozumiem co tu jest napisane, jeżeli chodzi o traktotwanie to jest napisane we wstępie o tym -->