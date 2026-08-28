# Google Analytics 4 — Data API v1

Notatka researchowa pod budowę własnego CLI/API dla agenta SEO (mentzen.pl).
Data researchu: 2026-08-28. Wszystko poniżej z oficjalnej dokumentacji Google, chyba że zaznaczono inaczej.

---

## 1. Co daje

Data API v1 to programowy dostęp do danych raportowych właściwości GA4 — te same liczby, które widać w interfejsie GA4, tylko w formie JSON/gRPC.
Źródło: <https://developers.google.com/analytics/devguides/reporting/data/v1>

Metody (kanał **v1beta** = stabilny):

| Metoda | Do czego |
|---|---|
| `runReport` | podstawowy raport: wymiary × metryki × zakres dat |
| `batchRunReports` | kilka raportów w jednym wywołaniu |
| `runPivotReport` / `batchRunPivotReports` | raporty przestawne |
| `runRealtimeReport` | ostatnie 30 minut (60 minut dla Analytics 360) |
| `getMetadata` | lista dostępnych wymiarów i metryk, w tym własnych definicji |
| `checkCompatibility` | sprawdzenie, czy dana kombinacja wymiarów/metryk jest dozwolona |
| `audienceExports` | eksport list użytkowników z audiencji |

Kanał **v1alpha** (może się zmienić bez zachowania wstecznej zgodności):
- `runFunnelReport` — raporty lejkowe.
- `reportTasks` — raporty asynchroniczne dla dużych zapytań (wprowadzone 2024-05-08).
- od 2026-04-23: raport „Conversion performance" przez `runReport` w v1alpha, z polem `ConversionSpec` (wybór `conversionActions` i modelu atrybucji `DATA_DRIVEN` / `LAST_CLICK`) oraz polem `section` w `ResponseMetaData` (`SECTION_REPORT` vs `SECTION_ADVERTISING`).
Źródło: <https://developers.google.com/analytics/devguides/reporting/data/v1/changelog>

**Czego GA4 NIE da.** Nie ma tu fraz wyszukiwania, wyświetleń w SERP ani średniej pozycji — to domena Search Console. GA4 odpowiada na pytanie „co ruch organiczny zrobił po wejściu na stronę", GSC na „skąd i na co przyszedł". Do pełnego obrazu SEO agent potrzebuje obu, sklejonych po URL-u landing page'a.

### Endpointy REST
- Data API: `https://analyticsdata.googleapis.com/v1beta/properties/{PROPERTY_ID}:runReport`
- Metadata: `GET https://analyticsdata.googleapis.com/v1beta/properties/{PROPERTY_ID}/metadata` — property ID `0` zwraca wymiary i metryki wspólne dla wszystkich właściwości (bez własnych definicji).
  Źródło: <https://developers.google.com/analytics/devguides/reporting/data/v1/rest/v1beta/properties/getMetadata>
- Admin API (do wykrycia property ID): `GET https://analyticsadmin.googleapis.com/v1beta/accountSummaries` — zwraca konta i podpięte do nich właściwości z ID i nazwami; `pageSize` domyślnie 50, maks. 200.
  Źródło: <https://developers.google.com/analytics/devguides/config/admin/v1/rest/v1beta/accountSummaries/list>

### Wymiary i metryki istotne dla SEO
Nazwy API (dokładnie tak trafiają do requestu). Źródło: <https://developers.google.com/analytics/devguides/reporting/data/v1/api-schema>

Wymiary:
- `landingPage` — ścieżka strony przy pierwszej odsłonie w sesji.
- `landingPagePlusQueryString` — to samo z query stringiem.
- `pagePath`, `pageTitle`, `hostName` (subdomena + domena).
- `sessionDefaultChannelGroup` — grupa kanałów dla sesji; wartość `Organic Search` = ruch organiczny.
- `sessionSource`, `sessionMedium`, `sessionSourceMedium`.
- `firstUserDefaultChannelGroup` — kanał, który pozyskał użytkownika po raz pierwszy.
- `defaultChannelGroup` — enum: `Direct`, `Organic Search`, `Paid Social`, ...
- `country`, `deviceCategory` (Desktop / Tablet / Mobile).
- `isKeyEvent` — zastąpił `isConversionEvent`.

Metryki:
- `totalUsers`, `newUsers`, `activeUsers`, `sessions`.
- `screenPageViews`, `engagedSessions`, `engagementRate`, `bounceRate`, `averageSessionDuration`, `userEngagementDuration`.
- `keyEvents`, `sessionKeyEventRate` — od maja 2024 „conversions" zostało przemianowane na „key events"; stare nazwy (`conversions`, `isConversionEvent`) są zdeprecjonowane.
  Źródło: <https://developers.google.com/analytics/devguides/reporting/data/v1/changelog>

Wymiary i metryki własne (custom definitions) adresuje się prefiksami `customEvent:<nazwa>` i `customUser:<nazwa>`; listę zwraca `getMetadata`.
Źródło: <https://developers.google.com/analytics/devguides/reporting/data/v1/advanced>

### Budowa requestu
Pola `runReport`. Źródło: <https://developers.google.com/analytics/devguides/reporting/data/v1/rest/v1beta/properties/runReport>

- `dimensions[]`, `metrics[]` — co najmniej jedna metryka. Limity liczby wymiarów/metryk w pojedynczym raporcie: [do weryfikacji — dokumentacja przeglądowa mówi o maks. 9 wymiarach, nie potwierdziłem tego w referencji REST].
- `dateRanges[]` — daty absolutne albo względne (`7daysAgo`, `yesterday`, `today`). Kilka zakresów naraz, w odpowiedzi indeksowane od zera (przydatne do porównań rok do roku bez dwóch zapytań).
- `dimensionFilter`, `metricFilter` — `FilterExpression` z `andGroup` / `orGroup` / `notExpression`. `metricFilter` działa po agregacji (odpowiednik SQL-owego HAVING). Od 2024-10-20 istnieje `EmptyFilter` do porównywania z wartością pustą.
- `orderBys[]`, `metricAggregations[]`, `currencyCode`, `cohortSpec`, `comparisons[]`, `keepEmptyRows`.
- `offset`, `limit` — cytat z referencji: *"The API returns a maximum of 250,000 rows per request, no matter how many you ask for. `limit` must be positive."* Domyślnie 10 000 wierszy. Paginacja przez `offset` + pole `rowCount` w odpowiedzi.
- `returnPropertyQuota` — zwraca stan zużycia limitów.

---

## 2. Dostęp i auth

**Wymagany projekt Google Cloud.** Każde żądanie do Data API v1 idzie przez projekt GCP, w którym trzeba włączyć *Google Analytics Data API v1* (a dla wykrywania właściwości także *Google Analytics Admin API*).
Źródło: <https://developers.google.com/analytics/devguides/reporting/data/v1/quotas>

**Zakres OAuth:** `https://www.googleapis.com/auth/analytics.readonly` (do odczytu; alternatywnie szerszy `https://www.googleapis.com/auth/analytics`). Do edycji encji zarządczych — `analytics.edit`, ale agent SEO tego nie potrzebuje.
Źródło: <https://developers.google.com/analytics/devguides/reporting/data/v1/rest/v1beta/properties/getMetadata>

Dwie ścieżki uwierzytelnienia:

**A. Konto serwisowe (rekomendowane dla CLI odpalanego przez agenta).**
1. W projekcie GCP tworzysz service account i pobierasz klucz JSON.
2. Ustawiasz `GOOGLE_APPLICATION_CREDENTIALS=/ścieżka/do/klucza.json` — biblioteki klienckie czytają tę zmienną w domyślnym konstruktorze. Cytat: *"Using a default constructor instructs the client to use the credentials specified in GOOGLE_APPLICATION_CREDENTIALS environment variable."*
3. W GA4: **Administracja → Zarządzanie dostępem** na poziomie usługi → `+` → **Dodaj użytkowników** → wpisujesz adres e-mail konta serwisowego (`...@...iam.gserviceaccount.com`) i nadajesz uprawnienia. Do samego czytania raportów wystarczy rola przeglądającego (Viewer) — [do weryfikacji: strona pomocy 9305788 opisuje procedurę dodawania, ale nie wylicza w tym miejscu ról; warto potwierdzić w „Access and data-restriction management"].
Źródła: <https://developers.google.com/analytics/devguides/reporting/data/v1/quickstart-client-libraries>, <https://support.google.com/analytics/answer/9305788>

**B. ADC z konta użytkownika (do pracy lokalnej / do MCP).**
```bash
gcloud auth application-default login \
  --scopes="https://www.googleapis.com/auth/cloud-platform,https://www.googleapis.com/auth/analytics.readonly"
```
Źródło: <https://developers.google.com/analytics/devguides/reporting/data/v1/quickstart-client-libraries>

**Property ID** to numer właściwości GA4 (nie „G-XXXX" — to Measurement ID). W requestach jako `properties/{PROPERTY_ID}`. Można go wyciągnąć programowo z `accountSummaries.list` Admin API.

---

## 3. Limity i koszty

**Koszt: brak opłat za wywołania Data API.** API jest częścią darmowej platformy GA i nie ma cennika per request; projekt GCP jest wymagany, ale sam Data API nie generuje pozycji na rachunku. [do weryfikacji jako oficjalny cytat — Google nie publikuje strony „pricing" dla tego API; potwierdzenie pochodzi z opracowań zewnętrznych, m.in. <https://www.graphed.com/blog/is-google-analytics-data-api-free>. Realny koszt to limity, nie pieniądze.]

**Limity (właściwość standardowa, czyli mentzen.pl o ile nie ma 360):**

| Limit | Standard | Analytics 360 |
|---|---|---|
| Core tokens / właściwość / dzień | 200 000 | 2 000 000 |
| Core tokens / właściwość / godzinę | 40 000 | 400 000 |
| Core tokens / projekt / właściwość / godzinę | 14 000 | 140 000 |
| Równoległe żądania (Core) | 10 | 50 |
| Błędy serwera / projekt / właściwość / godzinę | 10 | 50 |

Limity Realtime i Funnel są liczone osobno, ale mają te same wartości liczbowe co Core.
Osobno: **120 żądań na godzinę z wymiarami potencjalnie podlegającymi progowaniu** (`potentiallyThresholdedRequestsPerHour`).
Źródła: <https://developers.google.com/analytics/devguides/reporting/data/v1/quotas>, <https://developers.google.com/analytics/devguides/reporting/data/v1/rest/v1beta/PropertyQuota>

**Jak liczone są tokeny.** Cytat: *"The exact token cost for a request is determined at the time of execution, making precise pre-calculation challenging."* Koszt rośnie z: liczbą zwróconych wierszy, liczbą wymiarów i metryk, złożonością filtrów, długością zakresu dat, kardynalnością wymiarów (np. `pagePath`) i wolumenem zdarzeń właściwości.
Limity dzienne resetują się o północy czasu pacyficznego; godzinowe w ciągu godziny, niekoniecznie na pełnej godzinie.

**Monitoring zużycia.** `"returnPropertyQuota": true` w requeście zwraca obiekt `PropertyQuota` z polami `tokensPerDay`, `tokensPerHour`, `tokensPerProjectPerHour`, `concurrentRequests`, `serverErrorsPerProjectPerHour`, `potentiallyThresholdedRequestsPerHour` — każde jako `QuotaStatus` (zużyte / pozostałe). Warto to logować w każdym wywołaniu CLI.

### Pułapki jakości danych
Źródło: <https://developers.google.com/analytics/devguides/reporting/data/v1/reporting-data-expectations>

- **Świeżość.** Google potrzebuje czasu na przetworzenie zdarzeń; raport z UI i to samo zapytanie do API kilka minut później mogą się różnić. Dokumentacja nie podaje konkretnej liczby godzin. [do weryfikacji: powszechnie przyjmuje się 24–48 h do stabilizacji, ale to nie jest oficjalna liczba.] Praktyczny wniosek dla agenta: nie porównuj wczoraj do wczoraj; okno raportowania kończ na `2daysAgo` albo później.
- **Próbkowanie (sampling).** Występuje przy dużych zbiorach i złożonych zapytaniach. Pole `samplingMetadatas` w `ResponseMetaData` mówi, jaki procent zdarzeń wykorzystano: `samplesReadCount / samplingSpaceSize`, jeden wpis na każdy zakres dat, w kolejności z requestu. Właściwości 360 mają wyższe progi (do 1 mld zdarzeń) i mogą wymusić `samplingLevel: UNSAMPLED` w `reportTasks` (zmiana z 2024-10-15).
- **Kardynalność i wiersz `(other)`.** Wymiary o ponad 500 unikalnych wartościach dziennie powodują zwijanie rzadkich wartości do wiersza `(other)`. Kluczowe: **filtry działają PO utworzeniu `(other)`**, więc zawężenie do jednego katalogu URL nie odzyska danych, które już wpadły do `(other)`. Dla serwisu z dużą liczbą URL-i (blog + baza wiedzy) to realne ryzyko przy `landingPagePlusQueryString`.
- **Progowanie (thresholding).** Wiersze z małą liczbą użytkowników są usuwane, żeby nie dało się zidentyfikować osoby. API sygnalizuje to polem `subjectToThresholding` w `ResponseMetaData` — CLI powinno je sprawdzać i ostrzegać.
- **Tożsamość raportowania.** Liczba użytkowników zależy od ustawienia właściwości (Blended / Observed / Device-based), więc liczby mogą się nie zgadzać między właściwościami.

---

## 4. Biblioteki i przykłady integracji

### Python (preferowany stack użytkownika: `uv`)
Pakiet: **`google-analytics-data`**, autor Google LLC (`googleapis-packages@google.com`), maintainerzy `gcloudpypi` / `google_opensource`. Najnowsza wersja **0.23.0 z 2026-06-03**, wymaga Pythona ≥ 3.10 (wspiera 3.14).
Źródło: <https://pypi.org/project/google-analytics-data/>

```bash
uv add google-analytics-data
# plus, jeśli chcesz wykrywać property ID: google-analytics-admin
```

```python
from google.analytics.data_v1beta import BetaAnalyticsDataClient
from google.analytics.data_v1beta.types import (
    DateRange, Dimension, Metric, RunReportRequest,
)

client = BetaAnalyticsDataClient()          # czyta GOOGLE_APPLICATION_CREDENTIALS
request = RunReportRequest(
    property=f"properties/{property_id}",
    dimensions=[Dimension(name="city")],
    metrics=[Metric(name="activeUsers")],
    date_ranges=[DateRange(start_date="2020-03-31", end_date="today")],
)
response = client.run_report(request)
for row in response.rows:
    print(row.dimension_values[0].value, row.metric_values[0].value)
```
Źródło: <https://developers.google.com/analytics/devguides/reporting/data/v1/quickstart-client-libraries>

Klasa `BetaAnalyticsDataClient` ma też `from_service_account_file()` i `from_service_account_info()`, jeśli nie chcesz polegać na zmiennej środowiskowej. Istnieje wariant async (`async_client`).
Źródło: <https://googleapis.dev/python/analyticsdata/latest/data_v1beta/beta_analytics_data.html>

### Node.js (preferowany stack: `pnpm`)
Pakiet: **`@google-analytics/data`**, licencja Apache-2.0, repo `googleapis/google-cloud-node` (katalog `packages/google-analytics-data`), maintainerzy `google-wombot`, `bcoe`. Najnowsza wersja **7.0.0 z 2026-08-05**, wymaga Node ≥ 22, ~403 700 pobrań tygodniowo (dane z registry.npmjs.org, sprawdzone 2026-08-28).

```bash
pnpm add @google-analytics/data
```

```javascript
const {BetaAnalyticsDataClient} = require('@google-analytics/data');
const analyticsDataClient = new BetaAnalyticsDataClient();

async function runReport() {
  const [response] = await analyticsDataClient.runReport({
    property: `properties/${propertyId}`,
    dateRanges: [{startDate: '2020-03-31', endDate: 'today'}],
    dimensions: [{name: 'city'}],
    metrics: [{name: 'activeUsers'}],
  });
  response.rows.forEach((row) => {
    console.log(row.dimensionValues[0], row.metricValues[0]);
  });
}
```
Źródło: <https://developers.google.com/analytics/devguides/reporting/data/v1/quickstart-client-libraries>

### Oficjalny MCP od Google
Repo: **`googleanalytics/google-analytics-mcp`**, utrzymywane przez zespół Google Analytics, licencja Apache 2.0, status **eksperymentalny**.

Narzędzia, które wystawia:
- konto/właściwość: `get_account_summaries`, `get_property_details`, `list_google_ads_links`
- raporty: `run_report`, `run_funnel_report`, `get_custom_dimensions_and_metrics`
- realtime: `run_realtime_report`

Wymaga włączonych obu API (Admin + Data) i ADC ze scope `analytics.readonly`.

```bash
claude mcp add analytics-mcp \
  -e "GOOGLE_APPLICATION_CREDENTIALS=/ścieżka/klucz.json" \
  -e "GOOGLE_PROJECT_ID=twoj-projekt" \
  -- pipx run analytics-mcp
```
Źródło: <https://github.com/googleanalytics/google-analytics-mcp>

**Decyzja MCP vs własne CLI.** MCP Google jest wygodny do eksploracji ad hoc, ale `run_report` przyjmuje dowolne wymiary/metryki, więc agent może zjeść limit tokenów albo dostać próbkowane dane i tego nie zauważyć. Własne CLI z kilkoma nazwanymi raportami (`ga4 organic-landing-pages --last 28d`) jest przewidywalne, cache'owalne i deterministyczne — a to ważniejsze, gdy agent ma z tego robić rekomendacje SEO. Sensowny układ: własne CLI jako podstawa, MCP Google opcjonalnie do zadań eksploracyjnych.

### Społecznościowe CLI (nie weryfikowane)
Znalezione podczas researchu, wszystkie [do weryfikacji przed użyciem — zgodnie z zasadą sprawdzania zależności]:
- <https://github.com/Bin-Huang/google-analytics-cli> — CLI + skills dla agentów AI, deklaruje JSON na stdout.
- <https://github.com/kasdimg/analytics-cli> — GA4 + Search Console w jednym, pozycjonowane jako alternatywa dla MCP.
- <https://github.com/konfirmed/kanmi-ga4-cli> — wyjście table/JSON/markdown/CSV.
- <https://github.com/sulimanbenhalim/ga-cli>, <https://github.com/surendranb/google-analytics-mcp>

Żaden nie ma za sobą organizacji o ustalonej reputacji. Do produkcyjnego użycia lepiej napisać własne, bo warstwa nad `runReport` to kilkaset linii, a klucz JSON do GA4 nie powinien przechodzić przez niesprawdzony kod.

---

## 5. Zastosowanie dla agenta SEO (mentzen.pl)

Wszystkie poniższe raporty filtruje się na `sessionDefaultChannelGroup == "Organic Search"` (albo `sessionMedium == "organic"`, jeśli chcesz pominąć klasyfikację Google).

**W1. Ranking landing page'ów organicznych.**
`landingPagePlusQueryString` × [`sessions`, `activeUsers`, `engagementRate`, `averageSessionDuration`, `keyEvents`], 28 dni vs poprzednie 28 dni (dwa `dateRanges` w jednym requeście). Wynik: co rośnie, co spada, gdzie ruch jest, ale nie konwertuje. To wejście do decyzji „aktualizować / scalić / zostawić".

**W2. Wykrywanie spadków (regresje).**
Ten sam raport miesiąc do miesiąca oraz rok do roku; agent flaguje URL-e ze spadkiem sesji > X% przy jednoczesnym progu minimalnego wolumenu (żeby nie ścigać szumu). Spadek w GA4 + spadek pozycji w GSC = problem SEO; spadek w GA4 przy stabilnej pozycji = problem z CTR albo sezonowość.

**W3. Ruch organiczny wg sekcji serwisu.**
Filtr `landingPage` z operatorem `beginsWith` na `/blog/`, `/uslugi/`, `/kancelaria/` itd. Pokazuje, która sekcja ciągnie ruch B2B, i czy inwestycja w bloga podatkowego przekłada się na wejścia. Uwaga na wiersz `(other)` — przy dużej liczbie URL-i lepiej pobrać pełną listę i grupować lokalnie w Pythonie, niż filtrować po stronie API.

**W4. Konwersje z organica.**
`keyEvents` + `sessionKeyEventRate` w rozbiciu na `landingPage`, plus `eventName` żeby oddzielić formularz kontaktowy od zapisu na newsletter. Dla kancelarii B2B to najważniejszy raport: który artykuł faktycznie generuje zapytania, a nie tylko odsłony. Wymaga, żeby zdarzenia były w GA4 oznaczone jako kluczowe.

**W5. Kanibalizacja i porównanie kanałów.**
`sessionSourceMedium` × `landingPage` — czy dana strona żyje z organica, czy z newslettera i social mediów. Strona z wysokim ruchem, ale zerowym organikiem, to kandydat do optymalizacji on-page.

**W6. Segment mobile vs desktop dla treści prawnych.**
`deviceCategory` × `landingPage` × `engagementRate`. Długie teksty ustawowe czytane głównie na mobile z niskim zaangażowaniem to sygnał problemu z formatowaniem.

**W7. Zestawienie z Search Console.**
Klucz łączenia: URL landing page'a. GSC daje wyświetlenia/kliknięcia/CTR/pozycję i frazy, GA4 dodaje zachowanie i konwersje po kliknięciu. Praktyczny sygnał: wysokie wyświetlenia + niskie CTR (GSC) = popraw title/description; dobry CTR + niski `engagementRate` (GA4) = treść nie dowozi obietnicy z SERP-a.

**W8. Nowe treści — obserwacja startu.**
Raport dzienny dla URL-i opublikowanych w ostatnich 90 dniach; agent widzi, po ilu dniach nowy artykuł łapie ruch organiczny i które wzorce (długość, temat, autor) startują szybciej. Wiąże się z projektem checklisty autorów bloga.

**W9. Kontrola higieny danych.**
CLI przy każdym raporcie sprawdza `samplingMetadatas` i `subjectToThresholding` i dopisuje ostrzeżenie do wyjścia. Agent, który nie wie, że patrzy na próbkę 12% albo na dane po progowaniu, wygeneruje pewne siebie i błędne rekomendacje. To jedna z ważniejszych rzeczy do zaimplementowania.

**W10. Realtime — tylko do weryfikacji wdrożeń.**
`runRealtimeReport` (30 min) przydaje się po deployu, żeby sprawdzić, czy tagowanie działa po zmianach na stronie. Do analizy SEO bezużyteczne.

### Uwagi do projektowania CLI
- Jedna komenda = jeden nazwany raport z sensownymi domyślnymi wartościami; nie wystawiaj agentowi surowego `runReport` z dowolnymi wymiarami.
- Cache na dysku po kluczu (property, raport, zakres dat, filtry). Dane historyczne się nie zmieniają, a limit tokenów jest realny.
- Domyślne okno kończące się na `2daysAgo` lub wcześniej, żeby ominąć niedomknięte dane.
- Wyjście: JSON na stdout (dla agenta) + opcjonalnie CSV/tabela (dla człowieka). Jeden `--format`.
- Paginacja `offset`/`limit` obsłużona wewnątrz, z górnym bezpiecznikiem, żeby agent nie wyciągnął 250 tys. wierszy do kontekstu.
- Zawsze wysyłaj `returnPropertyQuota: true` i loguj pozostały budżet; przy niskim stanie komenda powinna odmówić, a nie próbować.
- Property ID trzymaj w konfigu projektu, nie w argumentach — mniej okazji do pomyłki.

### Dane
Raporty GA4 dla mentzen.pl to zagregowane statystyki ruchu na własnej stronie, nie dane klientów kancelarii. Zwróć jednak uwagę, żeby ewentualne wymiary własne (`customEvent:` / `customUser:`) nie zawierały identyfikatorów osób ani treści formularzy — to trzeba sprawdzić przez `getMetadata` przed pierwszym eksportem.

---

## 6. Wymagana konfiguracja po stronie użytkownika

1. **Projekt Google Cloud** (może być istniejący). Włączyć:
   - Google Analytics Data API v1,
   - Google Analytics Admin API (jeśli CLI ma samo wykrywać property ID albo jeśli używasz MCP Google).
2. **Konto serwisowe** w tym projekcie + klucz JSON. Plik poza repozytorium, uprawnienia `chmod 600`.
3. **Dostęp w GA4:** Administracja → Zarządzanie dostępem (poziom usługi mentzen.pl) → dodaj e-mail konta serwisowego z rolą przeglądającego.
4. **Zmienne środowiskowe** dla CLI:
   - `GOOGLE_APPLICATION_CREDENTIALS` — ścieżka do klucza JSON,
   - `GA4_PROPERTY_ID` — numeryczne ID właściwości mentzen.pl,
   - `GOOGLE_PROJECT_ID` — jeśli używasz MCP Google.
5. **Ustalić, czy właściwość to standard czy Analytics 360** — od tego zależą limity (200 tys. vs 2 mln tokenów dziennie) i dostępność `UNSAMPLED`.
6. **Zweryfikować zdarzenia kluczowe (key events)** w GA4: czy formularz kontaktowy, telefon i zapis na newsletter są oznaczone jako kluczowe. Bez tego raport W4 nie ma sensu.
7. **Zdecydować o MCP:** jeśli tak, `pipx` (albo `uv tool run`) i `claude mcp add analytics-mcp` jak w sekcji 4.
8. Opcjonalnie: **BigQuery export z GA4** jako alternatywa dla przypadków, gdzie sampling i wiersz `(other)` przeszkadzają — export daje surowe zdarzenia bez tych ograniczeń, ale kosztuje po stawkach BigQuery i wymaga osobnej konfiguracji. [do weryfikacji — nie badałem tego w tym researchu.]

---

## Źródła

- GA4 Data API — przegląd: <https://developers.google.com/analytics/devguides/reporting/data/v1>
- Tworzenie raportu (podstawy): <https://developers.google.com/analytics/devguides/reporting/data/v1/basics>
- Zaawansowane przypadki użycia: <https://developers.google.com/analytics/devguides/reporting/data/v1/advanced>
- Limity i przydziały: <https://developers.google.com/analytics/devguides/reporting/data/v1/quotas>
- PropertyQuota: <https://developers.google.com/analytics/devguides/reporting/data/v1/rest/v1beta/PropertyQuota>
- runReport (referencja REST): <https://developers.google.com/analytics/devguides/reporting/data/v1/rest/v1beta/properties/runReport>
- getMetadata: <https://developers.google.com/analytics/devguides/reporting/data/v1/rest/v1beta/properties/getMetadata>
- Schemat wymiarów i metryk: <https://developers.google.com/analytics/devguides/reporting/data/v1/api-schema>
- Oczekiwania wobec danych (sampling, kardynalność, progowanie): <https://developers.google.com/analytics/devguides/reporting/data/v1/reporting-data-expectations>
- Changelog: <https://developers.google.com/analytics/devguides/reporting/data/v1/changelog>
- Quickstart z bibliotekami klienckimi: <https://developers.google.com/analytics/devguides/reporting/data/v1/quickstart-client-libraries>
- Admin API accountSummaries.list: <https://developers.google.com/analytics/devguides/config/admin/v1/rest/v1beta/accountSummaries/list>
- Dodawanie użytkowników w GA4: <https://support.google.com/analytics/answer/9305788>
- Oficjalny MCP Google Analytics: <https://github.com/googleanalytics/google-analytics-mcp>
- PyPI google-analytics-data: <https://pypi.org/project/google-analytics-data/>
- npm @google-analytics/data (metadane z registry.npmjs.org): <https://www.npmjs.com/package/@google-analytics/data>
- Dokumentacja klienta Python: <https://googleapis.dev/python/analyticsdata/latest/data_v1beta/beta_analytics_data.html>
