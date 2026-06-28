
#### Wstęp

Dotychczasowa charakterystyka różnic epigenetycznych między populacjami ($\text{HG}$) a ($\text{AGR}$) opiera się przede wszystkim na analizie jednej pary populacji z jednego regionu — afrykańskich łowców-zbieraczy lasu deszczowego oraz sąsiadujących z nimi rolników [@fagny2015epigenomic]. Oparcie referencyjnych sygnatur metylacji na pojedynczej parze populacji wiąże się z dwoma istotnymi ograniczeniami.

Po pierwsze, mała liczba niezależnych populacji ogranicza moc statystyczną i wiarygodność identyfikowanych miejsc różnicowo metylowanych ($\text{DMS}$). Po drugie, różnice obserwowane między jedną parą $\text{HG}/\text{AGR}$ mogą być zaburzane przez pochodzenie genetyczne, lokalną historię demograficzną oraz specyfikę środowiska, w tym klimat lasu równikowego [@fagny2015epigenomic]. W takim układzie trudno rozstrzygnąć, czy wykryty sygnał odzwierciedla rzeczywisty efekt trybu życia, czy raczej lokalną różnicę typu „populacja A vs. populacja B”.

Rozwiązaniem tego problemu jest włączenie wielu niezależnych par $\text{HG}/\text{AGR}$ pochodzących z różnych kontynentów i stref klimatycznych. Sygnatury epigenetyczne powtarzalne w wielu takich porównaniach z dużo większym prawdopodobieństwem odzwierciedlają uniwersalny komponent związany z trybem życia, a nie lokalne uwarunkowania genetyczne, demograficzne lub środowiskowe.


#### Procedura

##### Identyfikacja miejsc różnicowo zmetylowanych (DMS)
Określenie miejsc o zróżnicowanym stopniu metylacji między populacjami zostaje zmodyfikowane statystycznie poprzez dopasowanie modelu regresji liniowej dla każdego loci CpG [@fagny2015epigenomic]. Modelowanie opiera się na analizie ciągłych wartości M.

Aby zdefiniować amplitudę zmian i wyodrębnić DMS, stosujemy rygorystyczne kryteria obejmujące skorygowaną wartość $P < 0{,}01$ oraz różnicę w średnim poziomie metylacji między dwiema populacjami ($\Delta\beta$)[@fagny2015epigenomic]. Poziom metylacji w tej analizie określa się jako stosunek intensywności sygnału z sondy metylowanej do intensywności ogólnej [@fagny2015epigenomic].

##### Analiza funkcjonalna i ontologia genów

W tym kroku wyodrębniamy wszystkie geny o zróżnicowanym stopniu metylacji, definiowane jako geny posiadające co najmniej jeden zidentyfikowany DMS [@fagny2015epigenomic].

<!-- #TODO @paulinaniemczak: ta część będzie do poprawy ALE to wymaga części marcina ze współćzesnym mapowaniem, tego nie mogę na ten momnet porpawić. Wydaje mi się że powinniśmy mieć pełne tło.  -->
Ponieważ nie wszystkie geny są reprezentowane przez , zestaw referencyjny (tło analizy) składa się wyłącznie z genów, dla których dostępne są poprawne dane z sond mikromacierzowych [@fagny2015epigenomic]. Zbiory DMS uznaje się za istotnie wzbogacone w danej kategorii funkcjonalnej, jeśli wartość P skorygowana o współczynnik fałszywych odkryć (FDR) wynosi maksymalnie 0,05 [@fagny2015epigenomic].

<!-- KOMENTARZ @maxi7524: fragment z populacją antyczną przeniosłem do `report/parts/methodology/dms_ancient.md` -->
