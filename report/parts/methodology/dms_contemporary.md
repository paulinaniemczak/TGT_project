
#### Wstęp

Dotychczasowa charakterystyka różnic epigenetycznych między populacjami łowiecko-zbierackimi ($\text{HG}$) a rolniczymi ($\text{AGR}$) opiera się przede wszystkim na analizie jednej pary populacji z jednego regionu — afrykańskich łowców-zbieraczy lasu deszczowego oraz sąsiadujących z nimi rolników [@fagny2015epigenomic]. Oparcie referencyjnych sygnatur metylacji na pojedynczej parze populacji wiąże się z dwoma istotnymi ograniczeniami.

Po pierwsze, mała liczba niezależnych populacji ogranicza moc statystyczną i wiarygodność identyfikowanych miejsc różnicowo metylowanych ($\text{DMS}$). Po drugie, różnice obserwowane między jedną parą $\text{HG}/\text{AGR}$ mogą być zaburzane przez pochodzenie genetyczne, lokalną historię demograficzną oraz specyfikę środowiska, w tym klimat lasu równikowego [@fagny2015epigenomic]. W takim układzie trudno rozstrzygnąć, czy wykryty sygnał odzwierciedla rzeczywisty efekt trybu życia, czy raczej lokalną różnicę typu „populacja A vs. populacja B”.

Rozwiązaniem tego problemu jest włączenie wielu niezależnych par $\text{HG}/\text{AGR}$ pochodzących z różnych kontynentów i stref klimatycznych. Sygnatury epigenetyczne powtarzalne w wielu takich porównaniach z dużo większym prawdopodobieństwem odzwierciedlają uniwersalny komponent związany z trybem życia, a nie lokalne uwarunkowania genetyczne, demograficzne lub środowiskowe.


#### Procedura

##### Identyfikacja miejsc różnicowo zmetylowanych (DMS)

<!-- DOSTOSOWANIE do map EM-seq. -->
Określenie miejsc o zróżnicowanym stopniu metylacji między populacjami opiera się na modelowaniu liczby odczytów metylowanych i niemetylowanych w każdej pozycji CpG, uzyskanych z populacyjnych map EM-seq. W przeciwieństwie do oryginalnej analizy mikromacierzowej, w której stosowano model liniowy ciągłych wartości M z empirycznym wygładzeniem Bayesa (pakiet `limma`) [@fagny2015epigenomic], dane sekwencyjne modelujemy testem opartym na rozkładzie beta-dwumianowym, uwzględniającym zmienność biologiczną oraz pokrycie (pakiet `DSS`) [@feng2014dss]. Dla każdej pozycji CpG porównujemy średni poziom metylacji między dwiema populacjami (HG vs. AGR) testem Walda, gdzie zmienną objaśnianą jest udział odczytów metylowanych w całkowitym pokryciu pozycji [@feng2014dss]. Wpływ zmiennych zakłócających to jest: płci, wieku oraz proporcji typów komórek, które w analizie referencyjnej wprowadzano jako współzmienne modelu liniowego [@fagny2015epigenomic], ograniczamy na etapie poprzedzającym test poprzez dopasowanie i stratyfikację próbek. Uzyskane wartości P koryguje się metodą Benjamini-Hochberga, a pozycje o skorygowanej wartości $P < 0{,}01$ uznaje się za różnicowo metylowane [@feng2014dss].

Aby zdefiniować amplitudę zmian i wyodrębnić DMS, stosujemy rygorystyczne kryteria obejmujące skorygowaną wartość $P < 0{,}01$ oraz różnicę w średnim poziomie metylacji między dwiema populacjami ($\Delta\beta$) wynoszącą ponad 10% [@fagny2015epigenomic]. Poziom metylacji w tej analizie określa się jako stosunek liczby odczytów metylowanych do całkowitej liczby odczytów (pokrycia) w danej pozycji CpG, reprezentujący wartość beta ($\beta$) [@feng2014dss]. W procesie tym wyodrębniamy nakładające się fragmenty między różnymi zestawami DMS i obliczamy wartości P mierzące prawdopodobieństwo uzyskania tych nakładających się fragmentów przez przypadek, stosując ponowne próbkowanie (resampling) [@fagny2015epigenomic]. Ponieważ poziomy metylacji DNA w docelowych miejscach są silnie skorelowane w obrębie regionów o określonej wielkości wynoszącej około 2000 bp, dla każdej listy DMS losowo pobieramy ponownie taką samą liczbę spośród wszystkich miejsc, biorąc pod uwagę faktyczną odległość chromosomalną między poszczególnymi DMS [@fagny2015epigenomic].

<!-- KOMENTARZ @maxi7524: dodałem format latex i dodałem fragmenty trochęzmieniłem format, żeby był bardziej czytelny -->

##### Analiza funkcjonalna i ontologia genów

W tym kroku wyodrębniamy geny o zróżnicowanym stopniu metylacji, definiowane jako geny zawierające co najmniej jeden DMS [@fagny2015epigenomic]. Analizę nadreprezentacji kategorii ontologii genów (GO) wśród tych genów przeprowadzamy pakietem `goseq` [@young2010goseq], zgodnie ze strategią analizy referencyjnej [@fagny2015epigenomic]. W odróżnieniu od oryginalnej analizy mikromacierzowej (ważenie liczbą sond) jako miarę obciążenia detekcji stosujemy liczbę pozycji CpG o wystarczającym pokryciu przypadającą na gen, co koryguje większą szansę wykrycia DMS w genach o wyższej gęstości CpG [@young2010goseq].

Funkcja ważenia prawdopodobieństwa ogranicza to obciążenie w obrębie testowanego tła, które stanowią wyłącznie geny posiadające pozycje CpG o wystarczającym pokryciu. Daną kategorię GO uznaje się za istotnie wzbogaconą w genach zawierających DMS, jeśli obliczona dla niej wartość P, skorygowana o współczynnik fałszywych odkryć (FDR) po wszystkich kategoriach, wynosi maksymalnie 0,05 [@fagny2015epigenomic].

<!-- KOMENTARZ @maxi7524: fragment z populacją antyczną przeniosłem do `report/parts/methodology/dms_ancient.md` -->
