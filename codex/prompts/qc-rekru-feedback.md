# /qc-rekru-feedback — generuj feedback EN z notatek PL

Argument: `<kandydat>` (nazwa folderu, np. `WeronikaMela`).

## Workflow

### 1. Walidacja
- Sprawdź `<kandydat>/notes/interview-notes.md` — musi istnieć
- Jeśli brak → "Brak notatek w `<kandydat>/notes/interview-notes.md`. Najpierw przeprowadź rozmowę i zapisz notatki."

### 2. Czytaj kontekst
- `<kandydat>/notes/interview-notes.md` — notatki PL (format: `java: komentarz\napi: komentarz\nogolne: komentarz\n...`)
- `<kandydat>/<kandydat>.pdf` — CV dla kontekstu (lata, projekty, stack)
- `<kandydat>/questions.md` — opcjonalnie, żeby wiedzieć co było pytane

### 3. Konwersja PL → EN

Reguły konwersji:
- "ok" / "dobrze" → "Demonstrated solid understanding of..."
- "słabo" / "nie wiedział" → "Showed limited familiarity with..."
- "super" / "świetnie" → "Exhibited strong expertise in..."
- "nie pamięta" / "nie używał" → "Indicated limited hands-on experience with..."
- "wymaga douczenia" → "Would benefit from additional learning in..."
- Krótkie notatki rozwiń do pełnych zdań
- Zawsze dodawaj kontekst z CV (lata, projekty)
- Ton: formalny, profesjonalny, neutralny (nie ozdabiaj nadmiernie)

### 4. Generuj `<kandydat>/feedback.md`

Struktura:

```markdown
# Technical Interview Feedback - <Imię Nazwisko>

## Date: <today YYYY-MM-DD>
## Position: <rola z questions.md>

### Java / Programming Skills
<przerobione notatki z sekcji java:>

### API Testing
<przerobione notatki z sekcji api:>

### GUI Testing
<przerobione notatki z sekcji gui:>

### Testing Theory / ISTQB
<przerobione notatki z sekcji teoria:>

### Livecoding Exercise
<przerobione notatki z sekcji livecoding:>

### Soft Skills / Communication
<przerobione notatki z sekcji soft/ogolne:>

---

## Overall Assessment

- **Score:** <X/5>
- **Recommendation:** <Strongly Recommend / Recommend / Recommend with Reservations / Do Not Recommend>
- **Would I want to work with this person?** <Yes/No + reasoning>
- **Recommended project type:** <typ projektu + uzasadnienie>

## Summary
<2-3 zdania podsumowujące>
```

### 5. Mapowanie skali notatek → Score + Recommendation

Heurystyka (dopasuj do tonu notatek):

| Notatki dominujące | Score | Recommendation |
|---|---|---|
| "super" / "świetnie" / konkretne osiągnięcia | 5/5 | Strongly Recommend |
| "ok" / "solidnie" / brak red flags | 4/5 | Recommend |
| "mieszane" / "tak sobie" / kilka luk | 3/5 | Recommend with Reservations |
| "słabo" / "nie wiedział" / wiele luk | 2/5 | Do Not Recommend |
| "kompletnie nie pasuje" | 1/5 | Do Not Recommend |

User może w notatkach napisać "score: X" lub "recommendation: Y" → użyj tego bezpośrednio.

### 6. Raport

Jedna linia: "feedback.md utworzony w <kandydat>/. Score: X/5, Recommendation: <Y>."

## Edge cases

- Notatki bardzo lakoniczne ("java: ok") → wygeneruj minimalny feedback z `TODO: rozwiń podczas review` notatkami
- Notatki w mieszanym PL/EN → konwertuj PL na EN, EN zostaw
- Brak sekcji java/api/etc w notatkach → pomiń tę sekcję w feedback lub wpisz "Not directly assessed during this interview"
- feedback.md już istnieje → zapytaj czy nadpisać czy stworzyć `feedback-v2.md`

## Bezpieczeństwo

- **Nie commituj** feedback.md automatycznie — to dane wrażliwe
- Nie wrzucaj treści feedbacku do logów / telemetrii / publicznych miejsc
