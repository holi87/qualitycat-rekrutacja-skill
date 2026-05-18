# /qc-rekru-add — dodaj pytanie do questions.md

Argument: treść pytania (1+ słowo).

## Workflow

### 1. Sprawdź lokalizację
- cwd musi zawierać `questions.md`
- Jeśli nie → wyświetl: "Brak questions.md w bieżącym folderze. `cd <ImieNazwisko>/` i spróbuj ponownie."

### 2. Sparsuj sekcje
- Czytaj `questions.md`
- Wyciągnij nagłówki H2 (`## 1. Intro`, `## 2. GUI Testing`, ...)
- Buduj listę grup

### 3. Zapytaj usera o grupę
Pokaż numerowaną listę grup + opcję "Nowa grupa":

```
Do jakiej grupy dodać pytanie?
  1. Intro
  2. GUI Testing
  3. API Testing
  4. SQL / dane testowe
  5. ISTQB / teoria testowania
  6. Dodatkowe z CV
  7. Livecoding
  n. Nowa grupa (podaj nazwę)
```

Czekaj na wybór.

### 4. Wygeneruj tytuł pytania
Z treści argumentu wygeneruj krótki tytuł (3-6 słów) jako H3. Np.:
- treść: "Jak handlujesz tokeny które wygasają w trakcie testu" → tytuł: "Token expiry w testach"
- treść: "Co to jest Page Object Pattern" → tytuł: "Page Object Pattern"

### 5. Dodaj do questions.md
Wstaw przed nagłówkiem następnej sekcji (lub na końcu wybranej sekcji):

```markdown
### <tytuł>
<treść pytania z argumentu>

*Oczekiwane:* <wygeneruj 2-4 punkty oczekiwanej odpowiedzi z kontekstu CV+grupy, lub `TODO: uzupełnij` jeśli niejasne>
```

### 6. Dodaj kartę do interview-plan.html (jeśli istnieje)

Jeśli `interview-plan.html` w cwd:

- Wygeneruj unikalny `data-qid` (np. `gui-N+1`, `api-N+1`)
- Wstaw przed `<h2>` następnej sekcji w odpowiednim H2 bloku:

```html
<div class="question-card" data-qid="<qid>">
  <h4><tytuł></h4>
  <blockquote><treść></blockquote>
  <div class="sub-question"><strong>Oczekiwane:</strong> <punkty></div>
  <div class="rating-row" data-name="<qid>">
    <strong>Ocena:</strong>
    <label><input type="radio" name="<qid>" value="OK"><span class="label-ok">OK</span></label>
    <label><input type="radio" name="<qid>" value="NOK"><span class="label-nok">NOK</span></label>
    <label><input type="radio" name="<qid>" value="NA" checked><span class="label-na">NA</span></label>
  </div>
  <div class="comment-label">Komentarz</div>
  <textarea data-comment="<qid>" placeholder="Notatki..."></textarea>
</div>
```

### 7. Raport
Jedna linia: "Dodano do grupy <nazwa>: <tytuł>. questions.md + interview-plan.html updated."

## Edge cases

- Pytanie pasuje do wielu grup → user wybiera jedną (nie duplikuj)
- Tytuł grupy zawiera znaki specjalne → escape przy parsowaniu
- HTML nie ma jeszcze sekcji dla nowej grupy → dodaj `<h2>` przed kartą pytania
- Pytanie już istnieje (duplikat treści) → ostrzeż usera zanim dodasz
