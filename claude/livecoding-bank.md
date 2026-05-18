# Livecoding bank — zadania per poziom + stack

Dobieraj zadanie do **CV kandydata**, nie do typu roli. Cel: kandydat ma 15-25 min pisać kod który widziałby w realnej pracy.

---

## Java — Junior / Mid

### J-1: LegoSet + Stream API (15-18 min)

**Polecenie:** Klasa `LegoSet(id, name, pieces, theme)`. Lista 5 obiektów. Napisz:
1. Sortowanie po `pieces` malejąco
2. Filtrowanie tylko `theme == "Star Wars"`
3. Suma wszystkich `pieces` w temacie "Star Wars"
4. Mapa `theme → List<LegoSet>` (`Collectors.groupingBy`)
5. Unit test JUnit 5 dla punktu 3

**Co sprawdzasz:**
- znajomość Stream API (filter/map/collect)
- Lambda vs method reference
- `Collectors.groupingBy`/`summingInt`
- czy używa `for` vs Stream
- czy potrafi napisać test JUnit (assertion z message)

**Red flags:** for-loop pętle zamiast Stream, mutowanie collection w pętli, brak testu.

---

### J-2: Page Object + JUnit + Selenium login (15 min)

**Polecenie:** Strona `/login` (username, password, submit). Po poprawnym → `/dashboard` z "Welcome, <user>". Po błędnym → "Invalid credentials". Napisz:
1. `LoginPage` (lokatory + `loginAs(user, pass)`)
2. `DashboardPage` (getter welcome message)
3. `LoginTest` z 2 testami (happy + negative)

Wymagania: WebDriverManager/Selenium Manager, explicit wait, `@AfterEach quit()`, data-testid > id > CSS.

**Co sprawdzasz:**
- struktura POP (lokatory ukryte, akcje publiczne, fluent return)
- waity (NIE Thread.sleep)
- cleanup driver
- nazewnictwo testów `shouldXWhenY`
- czy bierze negative path z własnej inicjatywy

---

### J-3: REST Assured CRUD (18 min)

**Polecenie:** API `/users` z CRUD. Napisz 4 testy: POST create → GET by id → PUT update → DELETE → GET 404.

Wymagania: REST Assured + JUnit 5, JSON Schema validation dla GET, body builder lub POJO.

**Co sprawdzasz:**
- `given().when().then()` struktura
- extract().path() dla chaining
- schema validation
- czy używa env vars dla URL/credentials

---

## Java — Mid / Senior

### M-1: REST Assured + Builder Pattern (banking) (15-18 min)

**Polecenie:** API kont inwestycyjnych. `POST /auth/token`, `POST /accounts`, `GET /accounts/{id}`, `POST /accounts/{id}/deposit`. Napisz:
1. `AccountRequest` Builder
2. `AccountsClient` (service layer z token caching, RequestSpec reuse)
3. JUnit 5: auth → create $1000 → deposit $500 → balance=1500
4. Min 2 negative (401, 400 invalid currency, 400 negative deposit)

**Co sprawdzasz:**
- Builder Pattern (immutable, defaults, fluent)
- Service/Client layer (token caching, spec reuse)
- BigDecimal dla pieniędzy (NIE double)
- env vars dla credentials
- schema validation

---

### M-2: Selenium PO + dynamic content (Cases list) (18 min)

**Polecenie:** Lista spraw z search po customerId, tabela wyników, klik wiersza → modal szczegółów. Napisz PO listy + modala + test który: szuka "C-1001", asercja 2 wiersze, otwarcie pierwszego, sprawdzenie `Status=NEW`. Plus negative: pusty search → error.

**Co sprawdzasz:**
- PO components (jak rozłoży listy/modale)
- `data-testid` strategia
- czy zwraca `next page` z akcji
- czy myśli o synchronizacji (search → results loaded)

---

## Java — Senior

### S-1: TestConfiguration Builder (15 min)

**Polecenie:** Klasa `TestConfiguration` z polami: `baseUrl`, `browser`, `headless`, `timeout`, `userPool`, `retryCount`, `screenshotsDir`. Napisz Builder Pattern z:
- immutable instance
- sensible defaults
- validacja (`timeout > 0`)
- możliwość kopiowania z modyfikacją (`config.toBuilder()...`)
- unit testy dla Buildera

**Co sprawdzasz:**
- Effective Java item 2 (Builder z wieloma polami)
- immutability (final fields, no setters)
- walidacja w `build()`
- `toBuilder()` (copy with modification)
- testy obejmujące happy + walidacja + default

---

### S-2: WebDriver Factory + Strategy Pattern (18 min)

**Polecenie:** Napisz `WebDriverFactory.create(BrowserType, boolean headless, Map<String,Object> opts)` która zwraca skonfigurowany WebDriver dla Chrome/Firefox/Safari/Edge. Wymagania:
- Strategy lub Factory Method Pattern
- ChromeOptions/FirefoxOptions/etc encapsulated per browser
- Selenium Manager (nie hardkoduj path)
- thread-safe (jeśli używane w parallel)
- unit testy z mockami (Mockito) lub real driver (jeśli framework dostępny)

**Co sprawdzasz:**
- design pattern znajomość (Strategy vs Factory)
- enkapsulacja per-browser specifics
- thread safety awareness (ThreadLocal driver)
- testowalność (DI, mock-able)

---

### S-3: Framework architecture (whiteboard, 18 min)

**Polecenie:** Whiteboard. Zaprojektuj framework dla 800+ testów UI + 500 API. Pokaż warstwy, jak izolujesz dane testowe, jak handlujesz multi-environment, jak puszczasz parallel, gdzie konfiguracja, gdzie reporting. Nie pisz kodu — diagram + uzasadnienie.

**Co sprawdzasz:**
- warstwowość (driver/client → page/service → business actions → tests)
- DI awareness (Spring/Guice/Manual)
- test data isolation (per-tenant/schema)
- parallel safety
- reporting integration
- ownership (kto utrzymuje testy?)

---

## C# / .NET

### C#-1: Selenium + NUnit (Java J-2 ekwiwalent, 15 min)

Jak J-2 ale w C# z NUnit. Sprawdzasz: `[SetUp]`/`[TearDown]`, FluentAssertions znajomość, `By.CssSelector`, async nie dotyczy (Selenium sync).

### C#-2: RestSharp / HttpClient (15 min)

CRUD na API w C#. Sprawdzasz: `HttpClient` vs `RestSharp`, `JsonSerializer` (System.Text.Json vs Newtonsoft), async/await.

---

## TypeScript / JS — Playwright / WDIO

### TS-1: Playwright + Page Object (15 min)

**Polecenie:** Playwright Test, login flow. Napisz: PO klasa, fixture z `test.extend`, 2 testy. Wymagania: `page.locator` z auto-wait, `expect(locator).toBeVisible()`, brak manual wait, parallel safe.

**Co sprawdzasz:**
- Playwright idioms (auto-wait, locator vs $)
- fixtures
- `expect.poll` jeśli potrzebny
- baseURL w config

### TS-2: WDIO + commands (15 min)

WDIO test class, custom commands (`addCommand`), services config. Sprawdzasz: sync vs async API (WDIO v8+), config awareness.

---

## Manual livecoding — Test case design

### MAN-1: Banking transfer test cases (10 min)

**Polecenie:** Feature transfer środków (kwota PLN, konto źródłowe, IBAN docelowy, tytuł max 140, data). Napisz 8-10 manualnych test case'ów (tytuł, preconds, kroki, expected). Wymagania:
- Min 2 boundary (kwota, długość tytułu)
- Min 2 negative
- Min 1 banking edge (idempotency, weekend, IBAN checksum, XSS)

**Co sprawdzasz:**
- techniki (boundary, equivalence, decision table)
- banking domain awareness
- idempotency z własnej głowy (czy złapie double-submit)
- struktura case'a (czy realnie copy-paste do TestLink)

### MAN-2: E-commerce checkout test cases (10 min)

Polecenie analogiczne, ale checkout (koszyk → adres → płatność → potwierdzenie). Sprawdzasz inne edge cases: inventory race condition, payment timeout, discount code expired during checkout, currency conversion.

### MAN-3: Mobile app session timeout (10 min)

Test cases dla aplikacji mobilnej z auth: background/foreground, push notification, deeplink z expired token, biometric fallback. Sprawdzasz: mobile-specific awareness (app lifecycle, permissions, connectivity).

---

## Bonus: pytania dodatkowe podczas livecoding (follow-up)

Zadaj **przed końcem**, niezależnie od poziomu:

1. **Skalowanie:** "Co jeśli ten test musi być 1 z 500? Jak puścisz parallel?"
2. **Data testowe:** "Skąd weźmiesz dane — fixture, baza, faker?"
3. **CI:** "Co dodasz do CI pipeline poza odpaleniem testu?"
4. **Failure mode:** "Co zrobisz jeśli ten test zaczyna flakować raz na 20 runów?"
5. **Refactor:** "Gdyby UI zmieniło się tak że pojawia się dodatkowy step — co zmieniasz w kodzie?"

Cel: sprawdzić myślenie poza happy path implementacji.
