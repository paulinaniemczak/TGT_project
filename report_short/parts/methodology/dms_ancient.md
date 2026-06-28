
#### Wstęp

Rekonstrukcja stanów metylacji dla populacji antycznych opiera się na specyficznych właściwościach degradacji post-mortem, umożliwiając binarne określenie statusu epigenetycznego zachowanych loci [@briggs2010removal; @gokhman2014reconstructing]. Ze względu na unikalną architekturę danych uzyskiwanych z paneli wzbogacanych, potok analityczny nie dąży do wyznaczenia ciągłych wartości ilościowych, lecz ma na celu precyzyjne określenie, czy dany region genomowy był metylowany[@barouch2024reconstructing; @mathov2025roam]. Uzyskane w ten sposób profile epigenetyczne starożytnych próbek podlegają bezpośredniemu mapowaniu i porównaniu z referencyjnymi obszarami DMS zidentyfikowanymi u populacji współczesnych, co pozwala na uchwycenie specyficznych zmian ewolucyjnych związanych z trybem życia.

W celu porównania skuteczności i czułości potoku interpretacji epigenetycznej, cała procedura analityczna zostaje równolegle przeprowadzona zarówno dla autentycznych próbek kopalnych, jak i dla syntetycznych zbiorów danych. Przetwarzanie obu tych kohort (prawdziwej i symulowanej) przebiega w sposób identyczny na poziomie plików BAM oczyszczonych z duplikatów sekwencyjnych.

#### Procedura

##### Ekstrakcja sygnału metylacji (wskaźnik C → T)
Wejściowy sygnał metylacji odzyskuje się bezpośrednio ze specyficznego wzorca deaminacji, będącego dominującym procesem chemicznej degradacji starożytnego DNA [@briggs2010removal]. Podczas uszkodzeń  post-mortem, niemetylowana cytozyna deaminuje do uracylu, natomiast metylowana 5-metylocytozyna deaminuje do tyminy [@briggs2010removal]. Na etapie przygotowania bibliotek stosujemy traktowanie enzymem USER, więc fragmenty zawierające uracyl (pochodzący z pozycji niemetylowanych) zostają wykluczone z sekwencjonowania [@mathov2025roam]. W rezultacie, odczytane tyminy w pozycjach CpG reprezentują wyłącznie pozycje pierwotnie metylowane przed śmiercią osobnika, które uniknęły działania enzymu [@mathov2025roam]. Wyższy wskaźnik bezpośrednio odpowiada wyższemu poziomowi metylacji przedśmiertnej [@gokhman2014reconstructing; @barouch2024reconstructing].

##### Filtrowanie pozycji CpG
Z dalszej analizy usuwa się pozycje niewiarygodne dla matematycznej rekonstrukcji mapy epigenetycznej [@barouch2024reconstructing; @mathov2025roam]:
- **Pozycje o anomalnie wysokim pokryciu (Extreme Coverage)** — loci wykazujące nienaturalnie wysoką głębokość pokrycia odczytami. W procesie tym próg odcięcia wyznaczany jest automatycznie, co pozwala wyeliminować fałszywy sygnał pochodzący ze złego mapowania w regionach duplikacji segmentalnych i powtórzeń genomu [@mathov2025roam].
- **Domniemane mutacje C → T** — pozycje, w których wskaźnik C → T przekracza sztywny próg (np. $> 0{,}25$), co może indykować mutację genetyczną, a nie modyfikację epigenetyczną [@mathov2025roam]. 

##### Estymacja tempa deaminacji
Globalne lub lokalne tempo deaminacji estymuje się na podstawie pozycji CpG.

##### Pulowanie osobników i rekonstrukcja mapy populacyjnej
Ponieważ pojedyncze próbki wzbogacane panelem docelowym mają często zbyt niskie indywidualne pokrycie dla samodzielnej rekonstrukcji, binarną mapę stanów buduje się na poziomie całej populacji poprzez pulowanie (łączenie) odczytów wielu osobników [@barouch2024reconstructing]:
- **Łączenie naiwne** — polega na prostym sumowaniu liczników C i T w każdej pozycji CpG bezpośrednio ze wszystkich osobników wchodzących w skład danej kohorty [@barouch2024reconstructing].
- **Mapowanie wskaźnika $\mathrm{C} \rightarrow \mathrm{T}$ na metylację** - realizowane metodą dopasowywania histogramów (*histogram matching*), polegającą na nieliniowym dopasowaniu uzyskanych wskaźników $\mathrm{C} \rightarrow \mathrm{T}$ do referencyjnego histogramu metylacji ludzkiej tkanki kostnej [@mathov2025roam].
- **Wygładzanie w przesuwnym oknie** - proces rekonstrukcji prowadzi się w zdefiniowanym oknie $W$ obejmującym kolejne pozycje CpG [@mathov2025roam].


##### Kontrola efektywnego pokrycia
Wiarygodna i stabilna rekonstrukcja stanów metylacji na poziomie populacyjnym wymaga zachowania **efektywnego pokrycia na poziomie $\ge 12$** [@barouch2024reconstructing]. Efektywne pokrycie kohorty można oszacować matematycznie z góry za pomocą modelu regresji liniowej, przyjmując jako zmienne liczbę pulowanych osobników oraz ich średnią liczbę unikalnych trafień w autosomalne loci SNP ($R^2 = 0{,}99$ w modelach referencyjnych) [@barouch2024reconstructing].

##### Mapowanie i porównanie profili metylacji z referencją współczesną
Uzyskane populacyjne mapy stanów metylacji dla kohort kopalnych (zarówno dla danych autentycznych, jak i zbiorów symulowanych) zostają poddane procedurze nakładania przestrzennego z ilościowymi matrycami referencyjnymi zidentyfikowanymi u ludności współczesnej [@barouch2024reconstructing]. Algorytm mapuje binarne stany metylacji kopalnych pozycji CpG bezpośrednio na współrzędne genomiczne uniwersalnych miejsc różnicowo metylowanych ($\text{DMS}$), dla współczesnych populacji łowców-zbieraczy oraz rolników [@mathov2025roam]. Pozwala to na precyzyjne określenie statusu epigenetycznego starożytnych kohort w oknach funkcjonalnych i identyfikację, które cechy metylacji specyficzne dla danego trybu życia były już utrwalone w badanych okresach historycznych [@barouch2024reconstructing].

##### Walidacja biologiczna i ewolucyjna map
Poprawność merytoryczną zrekonstruowanych map starożytnych weryfikuje się poprzez kontrolę obecności uniwersalnych cech biologicznych: głębokiej hipometylacji wysp CpG oraz promotorów genów metabolizmu podstawowego (*housekeeping genes*), podwyższonej metylacji elementów powtarzalnych, a także najwyższej korelacji ze współczesnymi referencyjnymi metylomami komórek tkanki kostnej [@barouch2024reconstructing]. Zwalidowane pod kątem ewolucyjnym i strukturalnym profile metylacji służą jako niezależny układ odniesienia, umożliwiający ocenę stopnia ciągłości epigenetycznej oraz kierunku zmian funkcjonalnych w genomie ludzkim na przestrzeni dziejów, bez ryzyka zaburzenia wyników przez pośmiertną degradację próby [@barouch2024reconstructing; @mathov2025roam].