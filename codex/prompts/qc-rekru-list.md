# /qc-rekru-list — lista kandydatów ze statusem

## Workflow

### 1. Skan cwd
Heurystyka: folder PascalCase (`ImieNazwisko/`) zawierający `*.pdf` z tą samą nazwą bazową (`ImieNazwisko.pdf`).

### 2. Per kandydat sprawdź obecność
- `<kandydat>/<kandydat>.pdf` → ✓ jeśli istnieje
- `<kandydat>/questions.md` → ✓ jeśli istnieje, `-` jeśli nie
- `<kandydat>/notes/interview-notes.md` → ✓ jeśli istnieje i ma >0 bajtów, `-` jeśli pusty
- `<kandydat>/feedback.md` → ✓ jeśli istnieje, `-` jeśli nie

### 3. Output

Tabela markdown:

```
| Kandydat              | PDF | questions | notes | feedback | Status         |
|-----------------------|-----|-----------|-------|----------|----------------|
| WeronikaMela          | ✓   | ✓         | -     | -        | Przygotowane   |
| DamianOlszewski       | ✓   | ✓         | ✓     | ✓        | Domknięte      |
| GabrielStarle         | ✓   | ✓         | ✓     | -        | Po rozmowie    |
```

Kolumna **Status**:
- Brak questions.md → "Do przygotowania"
- questions.md + brak notes → "Przygotowane (czeka rozmowa)"
- notes + brak feedback → "Po rozmowie (do feedbacku)"
- feedback.md → "Domknięte"

### 4. Sumaryczne liczby
Pod tabelą:

```
Łącznie: X kandydatów
  Do przygotowania: Y
  Przygotowane:     Z
  Po rozmowie:      W
  Domknięte:        V
```

### 5. Sortowanie
Domyślnie: alfabetycznie po nazwisku. Jeśli user dał flagę `--sort status` → sortuj wg statusu (Domknięte na dole, Do przygotowania na górze).

## Edge cases

- Brak kandydatów w cwd → "Brak kandydatów w bieżącym folderze. `/qc-rekru` żeby utworzyć z PDFów."
- Folder pasuje do wzorca ale bez PDFa → pokaż z PDF=✗ i Status="Niepełny"
- PDFy luźno w cwd (jeszcze nie przetworzone) → osobna sekcja "Nieprzetworzone PDFy: N (uruchom `/qc-rekru`)"
