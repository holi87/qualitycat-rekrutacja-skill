---
name: qc-rekru
description: "Rekrutacje techniczne SII — z PDF CV w bieżącym folderze generuje per-kandydata folder z questions.md (PL) + interview-plan.html (interaktywny checkpoint). Trigger: /qc-rekru (wszystkie PDFy), /qc-rekru <imie|nazwisko> (jeden), /qc-rekru-add <tresc> (dodaje pytanie do questions.md w bieżącym folderze kandydata), /qc-rekru-feedback <kandydat> (generuje feedback.md z notes/interview-notes.md)."
trigger: /qc-rekru
---

# /qc-rekru — przygotowanie rozmowy rekrutacyjnej

Skill służy do prowadzenia rekrutacji technicznych (domyślnie: **Java Test Automation Engineer / SII**). Pracuje w bieżącym folderze. Skanuje PDFy CV, generuje pakiet rozmowy per kandydat.

## Komendy

```
/qc-rekru                       # skan cwd dla *.pdf, dla każdego → folder ImieNazwisko/
/qc-rekru <imie|nazwisko>       # tylko dla pasującego PDFa (po nazwie pliku lub po treści CV)
/qc-rekru-add <treść pytania>   # dodaje pytanie do questions.md w cwd (pyta o grupę)
/qc-rekru-feedback <kandydat>   # generuje feedback.md z notes/interview-notes.md
/qc-rekru-list                  # lista kandydatów w cwd (status: brak / questions / notes / feedback)
```

## Flagi opcjonalne

```
--role "Manual + Auto 50/50"    # zmień rolę z domyślnej (Java TAE)
--time 75                       # zmień timeboxing (domyślnie 75 min)
--no-livecoding                 # pomiń livecoding (krótka rozmowa screening)
--lang en                       # wygeneruj po angielsku (domyślnie PL)
```

## Workflow per kandydat

### Krok 1: skan PDF
- `ls *.pdf` w cwd
- Dla każdego PDF: **Read tool** żeby wyciągnąć dane z CV (imię, nazwisko, lata doświadczenia, stack, projekty, certyfikaty, język angielski, edukacja).
- Nazwa pliku PDF często to UUID — **nie poleguj na nazwie pliku**, czytaj zawartość.

### Krok 2: dopasowanie roli
- Default: **Java Test Automation Engineer**.
- Jeśli user dał flagę `--role` → użyj jej.
- Jeśli CV jednoznacznie wskazuje inny profil (np. wyłącznie manual, 9+ lat manual + zerowa auto) → **zapytaj usera** czy nie zmienić roli.

### Krok 3: utworzenie struktury
```
ImieNazwisko/
├── ImieNazwisko.pdf      ← move/rename z UUID
├── notes/                 ← pusty, na notatki Grzegorza w trakcie rozmowy
├── questions.md           ← PL, pełny plan + livecoding + kryteria
└── interview-plan.html    ← interaktywny checkpoint (radio OK/NOK/NA + komentarze + localStorage)
```

### Krok 4: questions.md
Wygeneruj po polsku. **Personalizuj na podstawie CV** — używaj nazw projektów, technologii, liczb z CV. Struktura:

1. Header: pozycja, data (TBD), profil kandydata (1-2 zdania z CV)
2. Timeboxing table (domyślnie 75 min)
3. Sekcje per blok (z czasem, tagami `[must]` / `[bonus]`, oczekiwane odpowiedzi)
4. **Livecoding** — dobierz do stacku z CV (Java→Selenium+REST Assured; C#→NUnit+Selenium; mid Java→Page Object + JUnit5; senior→Builder/Factory pattern, framework design; manual-heavy→test case design + mini auto)
5. Czerwone flagi / zielone flagi

Wzorzec dobierania pytań patrz `example-questions.md` w tym folderze skilla. Pełne przykłady livecoding patrz `livecoding-bank.md`.

### Krok 5: interview-plan.html
Skopiuj template `interview-plan-template.html` z folderu skilla. Podmień placeholdery (`{{CANDIDATE_NAME}}`, `{{POSITION}}`, `{{PROFILE_SUMMARY}}`, `{{STORAGE_KEY}}`, `{{EXPORT_PREFIX}}`, sekcje pytań). Każde pytanie z questions.md → osobna karta `.question-card` z radio OK/NOK/NA + textarea. Livecoding sekcja: pełne polecenie + przykładowe rozwiązanie referencyjne + kryteria green/red flag.

**STORAGE_KEY** = `imie-nazwisko-interview` (kebab-case), **EXPORT_PREFIX** = `imie-nazwisko`.

Jeśli rola = 50/50 manual+auto → dodaj **dwa livecodingi** (manual test case design + mini auto) i tagi MANUAL/AUTO w kartach pytań.

### Krok 6: weryfikacja
- `ls ImieNazwisko/` — wszystkie 4 itemy obecne
- `wc -l ImieNazwisko/questions.md` — >100 linii
- `wc -l ImieNazwisko/interview-plan.html` — >400 linii
- PDF UUID-name usunięty z cwd (przeniesiony)

## /qc-rekru-add — dodawanie pytania

1. Sprawdź czy cwd zawiera `questions.md`. Jeśli nie → poproś usera o `cd` do folderu kandydata.
2. Sparsuj nagłówki H2 (`## 1. Intro`, `## 2. GUI...`) — zbierz listę grup.
3. **AskUserQuestion** z opcjami grup + "Nowa grupa".
4. Dodaj pytanie pod wybraną sekcją w formacie:
   ```
   ### <tytuł generowany z treści>
   <treść pytania>

   *Oczekiwane:* <jeśli możesz wywnioskować z kontekstu CV>
   ```
5. Jeśli istnieje `interview-plan.html` w tym folderze — **też dodaj** kartę pytania (parsuj kontekst, generuj `data-qid` + radio + textarea, wstaw przed `<h2>` następnej sekcji).
6. Krótki feedback: "Dodano do grupy X."

## /qc-rekru-feedback — generowanie feedbacku

Workflow opisany w `~/Desktop/GenAI/rekrutacje/CLAUDE.md` sekcja "Generowanie feedbacku":
1. Czytaj `<kandydat>/notes/interview-notes.md` (PL, format `java: komentarz\napi: komentarz\n...`)
2. Czytaj `<kandydat>/<kandydat>.pdf` dla kontekstu
3. Generuj `<kandydat>/feedback.md` — **formalny angielski**, struktura z CLAUDE.md projektu (Java/API/GUI/Theory/Livecoding/Soft + Overall Assessment).
4. Reguły konwersji PL→EN:
   - "ok"/"dobrze" → "Demonstrated solid understanding of..."
   - "słabo"/"nie wiedział" → "Showed limited familiarity with..."
   - "super"/"świetnie" → "Exhibited strong expertise in..."
5. Dodaj kontekst z CV (lata, projekty).
6. Score 1-5 + Recommendation + "Would I want to work with this person?" + Project type.

## /qc-rekru-list

Skan cwd dla folderów `ImieNazwisko/` (heurystyka: PascalCase folder z `.pdf` w środku). Tabela:

```
Kandydat              | PDF | questions | notes | feedback
WeronikaMela          | ✓   | ✓         | -     | -
DamianOlszewski       | ✓   | ✓         | ✓     | ✓
```

## Wzorce stylu

### questions.md
- Sekcje H2 numerowane (`## 1. Intro`, `## 2. GUI Testing`...)
- Pytania jako H3
- Tagi `[must]` (czerwony) / `[bonus]` (fioletowy)
- Każde pytanie z sekcją `*Oczekiwane:*` (3-5 punktów co kandydat powinien powiedzieć)
- Livecoding ma: Polecenie, Przykładowe rozwiązanie (Java/C#/JS pełne, nie szkielet), Kryteria oceny (tabela green/red flag), Follow-up
- Końcówka: "Czerwone flagi" + "Zielone flagi" jako bullet list

### interview-plan.html
- Self-contained (CSS inline, JS inline, **bez** zewnętrznych zależności)
- localStorage z unikalnym STORAGE_KEY
- Eksport JSON (przycisk w toolbar) z timestampem + nazwą kandydata
- Karty `.question-card` z radio + textarea
- Sumarycznym licznikiem OK/NOK/NA u góry
- Print-friendly (`@media print`)
- Responsive (`@media max-width: 760px`)
- 75 min default, ale dopasuj sekcje pod CV (senior → mniej teorii, więcej framework design; junior → mniej skomplikowany livecoding)

## Personalizacja vs question-bank

Jeśli CV bogate (>3 projekty, konkretne liczby, certs) → **personalizuj** pytania (cytuj projekty, używaj liczb).
Jeśli CV biedne (junior, brak liczb, ogólnikowe) → fallback do `question-bank.md` (skoryguj na rolę), ale i tak osadź w kontekście CV.

## Ostrzeżenia

- **Nigdy nie commituj** PDFów ani feedbacków automatycznie. To dane wrażliwe kandydatów. User commituje ręcznie.
- Jeśli cwd już zawiera folder o tej samej nazwie → **zapytaj** czy nadpisać.
- Jeśli kilka PDFów ma tę samą osobę (np. dwie wersje CV) → zapytaj którą wziąć.
- PDFy UUID-name **przenieś** do folderu kandydata, nie zostawiaj śmiecia w cwd.

## Pliki referencyjne w skillu

- `example-questions.md` — przykładowy questions.md (pełny, mid-level Java TAE) jako baseline
- `interview-plan-template.html` — czysty template HTML z placeholderami `{{...}}`
- `livecoding-bank.md` — bank zadań livecoding (Junior/Mid/Senior Java; C#; manual test case design)
