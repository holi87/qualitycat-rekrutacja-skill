# qc-rekru — rekrutacje techniczne (Codex)

Workflow do prowadzenia rozmów rekrutacyjnych technicznych (domyślnie: **Java Test Automation Engineer / SII**).

Codex pracuje w bieżącym folderze (`pwd`). Skanuje PDFy CV, generuje pakiet rozmowy per kandydat.

## Slash commands

| Komenda | Co robi |
|---------|---------|
| `/qc-rekru` | Skan `*.pdf` w cwd → folder `ImieNazwisko/` per kandydat |
| `/qc-rekru <imie\|nazwisko>` | Tylko pasujący PDF |
| `/qc-rekru-add <treść>` | Dodaje pytanie do `questions.md` w cwd, pyta o grupę |
| `/qc-rekru-feedback <kandydat>` | Generuje `feedback.md` z `notes/interview-notes.md` |
| `/qc-rekru-list` | Tabela statusu kandydatów |

Definicje promptów w `prompts/` obok tego pliku.

## Output per kandydat

```
ImieNazwisko/
├── ImieNazwisko.pdf       # PDF (przeniesiony z UUID)
├── notes/                  # puste, na notatki Grzegorza
├── questions.md            # PL plan rozmowy
└── interview-plan.html     # interaktywny checkpoint (localStorage, JSON export)
```

## Reguły personalizacji

- Bogate CV (>3 projekty, liczby, certs) → cytuj projekty, używaj konkretów z CV
- Biedne CV (junior, ogólnikowe) → fallback do `question-bank.md` jeśli istnieje
- Konflikt CV ↔ rola (np. głównie manual w CV vs `Java TAE`) → **zapytaj usera** zanim wygenerujesz

## Pliki referencyjne

Współdzielone z wariantem Claude (folder rówieśniczy `../claude/`):

- `../claude/example-questions.md` — wzorcowy questions.md
- `../claude/interview-plan-template.html` — czysty template HTML z `{{placeholderami}}`
- `../claude/livecoding-bank.md` — 13 zadań livecoding

W Codex po klonie repo: czytaj te pliki bezpośrednio z `<repo>/claude/`.

## Flagi opcjonalne

```
--role "Manual + Auto 50/50"    # zmień rolę z domyślnej
--time 75                       # zmień timeboxing (default 75 min)
--no-livecoding                 # pomiń livecoding (screening)
--lang en                       # generuj po angielsku
```

## Bezpieczeństwo

- PDFy + feedbacki = **dane wrażliwe** kandydatów
- Codex **nigdy nie commituje automatycznie** wyników rozmów
- User commituje ręcznie do prywatnego repo
- Konflikt ścieżki (folder kandydata już istnieje) → zapytaj zanim nadpiszesz

## Wzorzec stylu

### questions.md
- Sekcje H2 numerowane (`## 1. Intro`, `## 2. GUI Testing`...)
- Pytania jako H3
- Tagi `[must]` (czerwony) / `[bonus]` (fioletowy)
- Każde pytanie z sekcją `*Oczekiwane:*`
- Livecoding: Polecenie + Przykładowe rozwiązanie (pełny kod) + Kryteria green/red flag + Follow-up
- Końcówka: "Czerwone flagi" + "Zielone flagi"

### interview-plan.html
- Self-contained (CSS + JS inline, brak zewnętrznych zależności)
- localStorage z unikalnym STORAGE_KEY (`imie-nazwisko-interview`)
- Eksport JSON (timestamp + nazwa kandydata)
- Karty `.question-card` z radio OK/NOK/NA + textarea
- Sumaryczny licznik OK/NOK/NA
- Print-friendly + responsive

## Zasady ogólne Codex

- Komunikacja: krótko, technicznie, bez fluffu (caveman mode jeśli włączony)
- Dokumentacja bibliotek: użyj Context7 przed decyzją o API
- Język interview-plan/questions: **polski** (chyba że flaga `--lang en`)
- Feedback: **angielski** formalny (workflow dla SII)
