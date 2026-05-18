# qc-rekru — skill rekrutacyjny (Claude Code + Codex)

Skill do prowadzenia rozmów rekrutacyjnych technicznych (domyślnie: **Java Test Automation Engineer**). Z PDFów CV w bieżącym folderze generuje per-kandydata pakiet rozmowy: spersonalizowane `questions.md` (PL) + interaktywny `interview-plan.html` z checkpointami (radio OK/NOK/NA, komentarze, localStorage, eksport JSON).

Dwa warianty w jednym repo:
- `claude/` — skill dla **Claude Code**
- `codex/` — slash commands dla **OpenAI Codex CLI**

Współdzielą template HTML + livecoding bank + example questions (fizycznie w `claude/`, referencowane przez `codex/AGENTS.md`).

## Struktura repo

```
qualitycat-rekrutacja-skill/
├── README.md
├── .gitignore
├── claude/
│   ├── SKILL.md                       # frontmatter + trigger + workflow
│   ├── example-questions.md           # wzorcowy questions.md
│   ├── interview-plan-template.html   # template HTML z placeholderami
│   └── livecoding-bank.md             # 13 zadań livecoding (J/M/S Java, C#, TS, manual)
└── codex/
    ├── AGENTS.md                      # kontekst dla Codex
    └── prompts/
        ├── qc-rekru.md                # /qc-rekru
        ├── qc-rekru-add.md            # /qc-rekru-add
        ├── qc-rekru-feedback.md       # /qc-rekru-feedback
        └── qc-rekru-list.md           # /qc-rekru-list
```

## Instalacja — Claude Code

```bash
# Sklonuj repo
git clone git@github.com:holi87/qualitycat-rekrutacja-skill.git ~/dev/qualitycat-rekrutacja-skill

# Symlink folderu claude/ do katalogu skilli Claude Code
ln -s ~/dev/qualitycat-rekrutacja-skill/claude ~/.claude/skills/qc-rekru
```

Restart Claude Code. Skill widoczny przez `/qc-rekru`.

**Alternatywnie** (jeśli nie chcesz symlinka):
```bash
cp -r ~/dev/qualitycat-rekrutacja-skill/claude ~/.claude/skills/qc-rekru
```

## Instalacja — Codex CLI

```bash
# Sklonuj repo (jeśli jeszcze nie masz)
git clone git@github.com:holi87/qualitycat-rekrutacja-skill.git ~/dev/qualitycat-rekrutacja-skill

# Skopiuj prompty do katalogu Codex
mkdir -p ~/.codex/prompts
cp ~/dev/qualitycat-rekrutacja-skill/codex/prompts/*.md ~/.codex/prompts/

# Opcjonalnie: AGENTS.md w folderze gdzie pracujesz nad rekrutacjami
cp ~/dev/qualitycat-rekrutacja-skill/codex/AGENTS.md ~/Desktop/rekrutacje/AGENTS.md
```

Slash commands `/qc-rekru`, `/qc-rekru-add`, `/qc-rekru-feedback`, `/qc-rekru-list` dostępne w Codex CLI.

## Komendy (działają identycznie w Claude Code i Codex)

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

## Output per kandydat

```
ImieNazwisko/
├── ImieNazwisko.pdf       # PDF (przeniesiony z UUID-name)
├── notes/                  # puste, na notatki w trakcie rozmowy
├── questions.md            # PL plan rozmowy (70-300 linii)
└── interview-plan.html     # interaktywny checkpoint (samodzielny, localStorage)
```

## Livecoding bank

13 zadań w `claude/livecoding-bank.md` (Codex referencuje przez `codex/AGENTS.md`):

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
- Biedne CV (junior, ogólnikowe) → fallback do `question-bank.md` (jeśli istnieje w cwd), osadź w kontekście CV
- Konflikt CV ↔ rola (np. głównie manual w CV vs `Java TAE`) → skill **pyta** zanim wygeneruje

## Bezpieczeństwo

- PDFy + feedbacki = **dane wrażliwe** kandydatów
- Skill **nigdy nie commituje automatycznie**
- Commit ręczny do **prywatnego** repo

## Wymagania

- **Claude Code:** `Read` (PDF parsing), `Write`, `Edit`, `Bash`, `AskUserQuestion`
- **Codex CLI:** PDF parsing tool dostępny, podstawowe operacje plików

## Autor

**Grzegorz Holak — Quality Cat**
