#### Wstęp

Dla porównania skuteczności, zostanie dodatkowo przeprowadzona analiza wykorzystująca sztucznie zanieczyszczone i pofragmentowane sekwencje współczesne, symulujące aDNA. Dzięki temu sprawdzimy, czy wybrane przez nas parametry do analizy dają wiarygodne wyniki. Pomoże nam to również ustalić, z jakiego zakresu czasowego mogą pochodzić analizowane w ten sposób szczątki. 

#### Procedura

##### Przygotowanie danych
Całość procesu symulacji zanieczyszczenia danych opiera się na wykorzystaniu opcji dostępnych w narzędziu gargammel [@renaud2017gargammel], które zostało zaprojektowane właśnie do symulowania danych antycznych.

1. Sekwencje ludzkie będą pobrane z bazy NIH [@grc2026human]. 
2. Zostaną one podzielone na fragmenty wielkości 30-80 par zasad. 
3. Dodamy sekwencje egzogenne pochodzenia bakteryjnego, odpowiadające mikroorganizmom charakterystycznym dla tych znajdowanych w szczątkach badanego rejonu geograficznego. 
4. Dodane również zostaną charakterystyczne uszkodzenia zachodzące w procesie degradacji DNA post mortem.

##### Analiza bioinformatyczna

Przygotowane dane zostaną poddane ocenie jakości przy wykorzystaniu programu FastQC. Ewentualne filtrowanie i przycinanie sekwencji o zbyt niskiej jakości zostanie przeprowadzone wykorzystując program fastp. Odczyty zostaną zmapowane do genomu referencyjnego człowieka (GRCh38) programem BWA. Obce sekwencje usunięte zostaną narzędzeim BlobTools. 