# qc-rekru — Claude Code skill dla rekrutacji technicznych

Skill do prowadzenia rozmów rekrutacyjnych (domyślnie: **Java Test Automation Engineer**). Z PDFów CV w bieżącym folderze generuje per-kandydata pakiet rozmowy: spersonalizowane `questions.md` (PL) + interaktywny `interview-plan.html` z checkpointami (radio OK/NOK/NA, komentarze, localStorage, eksport JSON).

## Instalacja

```bash
git clone <repo-url> ~/.claude/skills/qc-rekru
```

Restart Claude Code. Skill widoczny przez `/qc-rekru`.

## Komendy

| Komenda | Co robi |
|---------|---------|
| `/qc-rekru` | Skan `*.pdf` w cwd → folder `ImieNazwisko/` per kandydat z `questions.md` + `interview-plan.html` + `notes/` |
| `/qc-rekru <imie\|nazwisko>` | Jak wyżej, ale tylko dla PDFa pasującego po nazwie/treści |
| `/qc-rekru-add <treść>` | Dodaje pytanie do `questions.md` w cwd, pyta o grupę. Dorzuca też kartę w `interview-plan.html` jeśli istnieje |
| `/qc-rekru-feedback <kandydat>` | Generuje `feedback.md` (formalny EN) z `notes/interview-notes.md` |
| `/qc-rekru-list` | Tabela statusu kandydatów w cwd (PDF/questions/notes/feedback) |

## Flagi opcjonalne

```
--role "Manual + Auto 50/50"    # zmień rolę z domyślnej
--time 75                       # zmień timeboxing (default 75 min)
--no-livecoding                 # pomiń livecoding (screening)
--lang en                       # generuj po angielsku
```

## Struktura outputu per kandydat

```
ImieNazwisko/
├── ImieNazwisko.pdf       # PDF (przeniesiony z UUID-name)
├── notes/                  # puste, na notatki Grzegorza
├── questions.md            # PL plan rozmowy (70-300 linii)
└── interview-plan.html     # interaktywny checkpoint (samodzielny, localStorage)
```

## Co jest w środku skilla

| Plik | Rola |
|------|------|
| `SKILL.md` | Instrukcje + workflow + reguły personalizacji |
| `example-questions.md` | Wzorcowy `questions.md` z wszystkimi sekcjami |
| `interview-plan-template.html` | Czysty template HTML z `{{placeholderami}}` |
| `livecoding-bank.md` | 13 zadań livecoding (J/M/S Java, C#, TS, manual) |

## Livecoding bank

- **J-1** LegoSet + Stream API (junior)
- **J-2** Page Object + JUnit + Selenium login
- **J-3** REST Assured CRUD
- **M-1** REST Assured + Builder Pattern (banking)
- **M-2** Selenium PO + dynamic content (Cases list)
- **S-1** TestConfiguration Builder
- **S-2** WebDriver Factory + Strategy Pattern
- **S-3** Framework architecture (whiteboard)
- **C#-1, C#-2** — .NET ekwiwalenty
- **TS-1, TS-2** — Playwright / WDIO
- **MAN-1..3** — manual test case design (banking, e-com, mobile)

## Personalizacja

- Bogate CV (>3 projekty, liczby, certs) → cytuj projekty, używaj konkretów z CV
- Biedne CV (junior, ogólnikowe) → fallback do `question-bank.md` (jeśli istnieje w cwd), ale osadź w kontekście CV
- Konflikt CV ↔ rola (np. głównie manual w CV vs `Java TAE`) → skill **pyta** zanim wygeneruje

## Bezpieczeństwo

- PDFy + feedbacki = dane wrażliwe kandydatów
- Skill **nigdy nie commituje automatycznie**
- Komitowanie ręczne, do prywatnego repo

## Wymagania

- Claude Code (skill mechanism)
- `Read` (PDF parsing), `Write`, `Edit`, `Bash`, `AskUserQuestion`

## Autor

Grzegorz Holak — rekrutacje techniczne SII (Java Test Automation Engineer + warianty).
