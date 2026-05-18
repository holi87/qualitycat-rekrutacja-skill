# /qc-rekru — generuj pakiety rozmów rekrutacyjnych z PDF CV

Pracujesz w bieżącym folderze (cwd). Cel: wygenerować per-kandydata pakiet rozmowy technicznej (domyślnie Java Test Automation Engineer dla SII).

## Argument

- Brak argumentu → przetwórz **wszystkie** `*.pdf` w cwd
- Argument `<imie|nazwisko>` → przetwórz **tylko** ten PDF (dopasowanie po nazwie pliku lub treści CV)

## Flagi (opcjonalne)

```
--role "<nazwa roli>"           # domyślnie "Java Test Automation Engineer"
--time <minuty>                 # domyślnie 75
--no-livecoding                 # pomiń livecoding
--lang en                       # generuj po angielsku (domyślnie PL)
```

## Workflow per PDF

### 1. Czytaj CV
- Wyciągnij: imię, nazwisko, lata doświadczenia, stack główny, projekty (z nazwami!), certyfikaty, język angielski, edukacja
- **Nazwa pliku PDF często to UUID** — nie poleguj na nazwie, czytaj zawartość
- Jeśli CV nie pozwala wyciągnąć imienia/nazwiska → zapytaj usera

### 2. Dopasuj rolę
- Default: Java Test Automation Engineer
- Jeśli `--role` → użyj
- Jeśli CV jednoznacznie wskazuje inny profil (np. wyłącznie manual przez 9 lat, zerowa auto) → **zapytaj** usera czy nie zmienić roli przed generowaniem

### 3. Utwórz strukturę
```
ImieNazwisko/
├── ImieNazwisko.pdf       # mv z UUID → tutaj
├── notes/                  # mkdir pusty
├── questions.md            # generuj (sekcja 4)
└── interview-plan.html     # generuj (sekcja 5)
```

Jeśli folder `ImieNazwisko/` już istnieje → **zapytaj** czy nadpisać.

### 4. Generuj `questions.md` (PL)

Czytaj wzorzec z `<repo>/claude/example-questions.md`. Personalizuj:

- Cytuj **konkretne projekty z CV** w pytaniach (np. "Mówisz w CV o Trustee Suite — jak..." )
- Używaj liczb z CV (np. "85% coverage, 800+ TC")
- Dobierz głębokość pytań do poziomu (junior → fundamentals; senior → trade-offy, architektura)
- Tagi `[must]` / `[bonus]` na pytaniach krytycznych vs nice-to-have
- Sekcja `*Oczekiwane:*` pod każdym pytaniem (3-5 punktów co kandydat powinien powiedzieć)

**Timeboxing (75 min default):**

| Blok | Czas |
|------|------|
| Intro + projekt z CV | 5 min |
| GUI Testing | 10-12 min |
| API Testing | 10-12 min |
| SQL / dane testowe | 6-8 min |
| ISTQB / teoria | 6-8 min |
| Dodatkowe z CV (CI/CD, BDD, AI) | 6-8 min |
| Livecoding | 15-20 min |
| Pytania kandydata + podsumowanie | 4-5 min |

**Livecoding** — wybierz z `<repo>/claude/livecoding-bank.md`:
- Junior Java → J-1, J-2, J-3
- Mid Java → M-1, M-2
- Senior Java → S-1, S-2, S-3
- C# → C#-1, C#-2
- TypeScript / JS → TS-1, TS-2
- Manual-heavy → MAN-1, MAN-2, MAN-3

50/50 manual+auto rola → **dwa** livecodingi: jeden MAN, jeden auto.

Końcówka questions.md: bullet listy "Czerwone flagi" + "Zielone flagi".

### 5. Generuj `interview-plan.html`

Czytaj template z `<repo>/claude/interview-plan-template.html`. Podmień placeholdery:
- `{{CANDIDATE_NAME}}` → "Imie Nazwisko"
- `{{POSITION}}` → rola
- `{{PROFILE_SUMMARY}}` → 1-2 zdania z CV
- `{{STORAGE_KEY}}` → `imie-nazwisko-interview` (kebab-case)
- `{{EXPORT_PREFIX}}` → `imie-nazwisko`
- `{{INTRO_QUESTION}}`, `{{GUI_QUESTION_*}}`, itd. — treść z questions.md

Każde pytanie z questions.md → osobna `<div class="question-card">` z:
- `<h4>` tytuł + tag `<span class="must-tag">must</span>` / `<span class="bonus-tag">bonus</span>` jeśli pasuje
- `<blockquote>` treść pytania
- `<div class="sub-question"><strong>Oczekiwane:</strong> ...</div>`
- `<div class="rating-row">` z radio OK/NOK/NA (default NA)
- `<textarea data-comment="...">` na notatki

Livecoding sekcja: pełna treść (polecenie + przykładowe rozwiązanie w `<pre><code>` + tabela kryteriów green/red flag).

50/50 manual+auto rola → dodaj klasy `.manual`/`.auto` do kart pytań i osobne nagłówki sekcji.

### 6. Weryfikacja
```bash
ls ImieNazwisko/                  # 4 itemy: PDF, notes/, questions.md, interview-plan.html
wc -l ImieNazwisko/questions.md   # >100 linii
wc -l ImieNazwisko/interview-plan.html  # >400 linii
```

PDF UUID-name usunięty z cwd (przeniesiony).

### 7. Raport

Krótko (caveman): jeden kandydat = 2-3 linie:
- `<ImieNazwisko>: <rola> | <stack> | livecoding <kod>`
- Co spersonalizowane (projekty cytowane z CV)
- Co wymaga uwagi (junior auto / brak ISTQB / etc)

## Edge cases

- Kilka PDFów z tą samą osobą → zapytaj którą wersję CV wziąć
- PDF bez czytelnego imienia (skan obrazu bez OCR) → zapytaj usera o imię/nazwisko
- Konflikt rola ↔ CV → zapytaj zanim wygenerujesz

## Output style

- Komunikacja: caveman (krótko, technicznie)
- Tool calls: nie spamuj — najpierw plan, potem batch operations
- Brak `git commit` automatycznego — to dane wrażliwe kandydatów
