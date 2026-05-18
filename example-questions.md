# <ImieNazwisko> - Interview Questions

**Pozycja:** Java Test Automation Engineer
**Data:** TBD
**Profil:** <1-2 zdania z CV: lata doświadczenia, główny stack, ostatni projekt, certyfikaty, język>.

> **Uwaga:** <opcjonalnie — np. "Senior — pytaj o trade-offy, nie definicje", "Junior — sprawdź fundamentals", "50/50 manual+auto — nie testuj jak senior auto">.

## Timeboxing (75 min)

| Blok | Czas |
|------|------|
| 1. Intro + projekt z CV | 5 min |
| 2. GUI Testing | 12 min |
| 3. API Testing | 12 min |
| 4. SQL / dane testowe | 8 min |
| 5. ISTQB / teoria testowania | 8 min |
| 6. Dodatkowe z CV (CI/CD, BDD, AI) | 8 min |
| 7. Livecoding | 18 min |
| 8. Pytania kandydata + podsumowanie | 4 min |

---

## 1. Intro (5 min)

**Projekt otwierający:** Opowiedz o <konkretnym projekcie z CV> — Twoja rola, zakres automatyzacji, realny impact.

*Cel: baseline. Konkretne liczby, samodzielność, ja vs my, ownership.*

---

## 2. GUI Testing (12 min)

### Page Object Pattern [must]
<Pytanie dostosowane do stacku z CV — Selenium Java / WebdriverIO / Playwright. Jak organizuje POP, co wystawia, co ukrywa.>

*Oczekiwane:* separacja test/UI, page components, brak logiki biznesowej w testach, fluent API, builder dla danych.

### Waity i flaky tests [must]
Implicit vs explicit vs fluent wait. Jak diagnozuje flaky scenariusze?

*Oczekiwane:* explicit wait na warunek, brak `Thread.sleep`, root cause przed retry, świadomość że implicit + explicit się gryzie.

### Lokatory i testability
Aplikacja legacy z dynamicznymi ID. Jak podchodzi?

*Oczekiwane:* hierarchia (data-testid > id > CSS > XPath), rozmowa z dev, unikanie indeksów.

---

## 3. API Testing (12 min)

### HTTP status codes [must]
Różnica 200 / 201 / 204 / 400 / 401 / 403 / 404 / 409 / 422 / 500.

*Oczekiwane:* 401 brak auth, 403 brak uprawnień, 422 walidacja semantyczna, 400 syntactic, 409 konflikt stanu.

### REST Assured / Postman [must]
<Dostosuj do stacku CV. Jak organizuje regresję API?>

*Oczekiwane:* spec reuse (RequestSpecification), env per środowisko, pre-request scripts, Newman w CI, schema validation.

### Negative API testing
Endpoint `POST /resource`. Jakie negatywne scenariusze?

*Oczekiwane:* brak wymaganych pól, invalid format/enum, duplikat, brak/expired token, malformed JSON, za długie wartości.

---

## 4. SQL / dane testowe (8 min)

### Pisanie query [must]
<Realny case z domeny aplikacji z CV. Tabele + zadanie GROUP BY HAVING.>

*Oczekiwane:* poprawny JOIN, WHERE, GROUP BY, HAVING, świadomość NULL, INTERVAL, indeksy.

### Walidacja w bazie vs przez API
Kiedy walidujesz bezpośrednio w SQL zamiast tylko UI/API?

*Oczekiwane:* eventual consistency, audit log, side effects, reconciliation, batch jobs.

---

## 5. ISTQB / teoria testowania (8 min)

### Techniki black-box [must]
Equivalence partitioning + boundary value dla konkretnego pola.

*Oczekiwane:* klasy + granice <min, min, min+1, max-1, max, >max; dodatkowo decimal, negative, scientific notation.

### Defect report
Co MUSI zawierać bug report? Severity vs priority — przykład sev=critical, prio=low.

*Oczekiwane:* tytuł, env, wersja, kroki, expected/actual, dane, logi, repro%, impact; np. crash w admin tool używanym raz/rok.

### Poziomy testów
Wymień poziomy wg ISTQB. Gdzie automatyzacja ma sens, gdzie przesadzona?

*Oczekiwane:* unit/integration/system/acceptance, ROI test pyramid, nie wszystko w E2E.

---

## 6. Dodatkowe z CV (8 min)

<Dostosuj do CV. Np.:>

### CI/CD - <Jenkins/GitLab/Bamboo z CV>
Jak wygląda pipeline od commit do raportu?

*Oczekiwane:* multi-stage, parallel, artifacts, Allure/JUnit reports, fail-fast smoke + nightly full.

### BDD / Cucumber [bonus jeśli w CV]
Plus minus BDD. Kiedy się zwraca, kiedy overkill?

*Oczekiwane:* business readable, ale podwojona maintenance, ROI tylko gdy biznes faktycznie czyta scenariusze.

### English check
<Pytanie po angielsku z domeny — np. "Walk me through the most critical bug you found and how you communicated it.">

*Oczekiwane:* zgodnie z deklaracją z CV (B2/C1). Struktura STAR, precyzja, brak "tłumaczenia z polskiego".

---

## 7. Livecoding (18 min)

### Polecenie
<Wybierz z livecoding-bank.md zadanie pasujące do poziomu i stacku.>

### Przykładowe rozwiązanie referencyjne

```java
// pełny kod referencyjny — nie szkielet, nie pseudokod
```

### Kryteria oceny

| Aspekt | Green flag | Red flag |
|--------|-----------|----------|
| Page Object / struktura | Lokatory w polu, akcje publiczne | Wszystko w teście |
| Waity | Explicit wait | Thread.sleep |
| Lokatory | data-testid, hierarchia | XPath po tekście, indeksy |
| Asercje | Konkretne, z message | `assertTrue(x)` bez kontekstu |
| Negative path | Z własnej inicjatywy | Tylko happy path |
| Nazewnictwo | `shouldXWhenY` | `test1`, `seleniumTest` |
| Cleanup | `@AfterEach quit` | Driver leak |

### Follow-up
- <2-3 pytania doprecyzowujące zależne od podejścia kandydata>

---

## 8. Pytania kandydata + podsumowanie (4 min)

Zwróć uwagę: pyta o stack, proces, zespół, rozwój — czy tylko warunki.

---

## Czerwone flagi do złapania

- "Thread.sleep" w odpowiedzi o waity
- Brak rozumienia różnicy 401/403
- UPDATE na prodzie bez SELECT/transakcji
- POP "wszystko w jednej klasie"
- Hardcoded URL/credentials w livecodingu
- "Nie wiem, ktoś inny robił" — kluczowe w CV

## Zielone flagi

- Konkretne liczby z projektów z CV
- Komunikacja problem → rozwiązanie → wpływ
- Pyta o cel zadania zanim zacznie kodować
- Świadomość kosztu utrzymania
- Uczciwość "tego nie robiłem" zamiast udawania
