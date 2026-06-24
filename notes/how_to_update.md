
### 1. Tworzenie i przejście na własny branch

Aby utworzyć nowy branch lokalny i automatycznie się na niego przełączyć, użyj komendy:

```bash
git checkout -b <nazwa_twojego_brancha>

```

*Alternatywnie w nowszych wersjach Gita:* `git switch -c <nazwa_twojego_brancha>`

---

### 2. Edycja i zapisywanie zmian

Po zmodyfikowaniu plików w swoim środowisku, dodaj je do obszaru przejściowego (staging) i stwórz commit:

```bash
# Sprawdzenie statusu plików
git status

# Dodanie konkretnego pliku lub wszystkich zmian (.)
git add .

# Zapisanie zmian w lokalnym repozytorium
git commit -m "Jasny i zwięzły opis wprowadzonych zmian"

```

---

### 3. Wypchnięcie brancha na serwer (Push)

Przy pierwszym wypchnięciu nowego brancha należy powiązać go z repozytorium zdalnym (`origin`):

```bash
git push -u origin <nazwa_twojego_brancha>

```

Każdy kolejny push na tym branchu wymaga już tylko wywołania: `git push`.

---

### 4. Tworzenie Pull Requesta (PR) i synchronizacja z głównym branchem

1. **Aktualizacja brancha przed scaleniem (Opcjonalnie - unikanie konfliktów):**
* Jeśli w międzyczasie główny branch uległ zmianie, pobierz najnowsze poprawki i scal je ze swoim branchem lokalnym przed ostatecznym zamknięciem PR:


```bash
# pobranie zmian
git checkout main
git pull origin main
git checkout <nazwa_twojego_brancha>
git merge main

# wypchnięcie swoich
git push 

```

2. **Otwarcie Pull Requesta:**
* Przejdź do interfejsu webowego (GitHub/GitLab).
* Kliknij przycisk **"Compare & pull request"**, który pojawi się automatycznie po wypchnięciu brancha.
* Ustaw branch docelowy (target) jako główny branch projektu (najczęściej `main` lub `master`).


3. **Opis zmian**
* Napisać krótko co było zmienione - chodzi o kontekst
