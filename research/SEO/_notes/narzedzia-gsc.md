# Google Search Console API — notatka researchowa

Kontekst: budowa lokalnych narzędzi CLI/API dla agenta SEO (Claude Code) pracującego nad mentzen.pl (WordPress, prawo/podatki/księgowość B2B, rynek PL).
Data researchu: 2026-08-28. Fakty z cytowaniami; rzeczy niepewne oznaczone `[do weryfikacji]`.

---

## 1. Co daje

Search Console API to jedyne źródło danych o tym, **na jakie zapytania Google faktycznie pokazuje serwis** — czego nie ma w żadnym crawlerze ani w Analytics. Cztery zasoby:

### 1.1 Search Analytics (`searchanalytics.query`)

Ruch z wyszukiwarki z podziałem na wymiary. Metryki: `clicks`, `impressions`, `ctr`, `position`.
Źródło: <https://developers.google.com/webmaster-tools/v1/searchanalytics/query>

Pola requestu:

| Pole | Opis |
| --- | --- |
| `startDate`, `endDate` | wymagane, `YYYY-MM-DD`, czas pacyficzny (UTC-7/-8), inclusive |
| `dimensions[]` | `QUERY`, `PAGE`, `COUNTRY`, `DEVICE`, `SEARCH_APPEARANCE`, `DATE`, `HOUR` |
| `type` | `web`, `image`, `video`, `news`, `googleNews`, `discover` |
| `dimensionFilterGroups[]` | operatory: `equals`, `notEquals`, `contains`, `notContains`, `includingRegex`, `excludingRegex` |
| `aggregationType` | `auto`, `byPage`, `byProperty`, `byNewsShowcasePanel` |
| `rowLimit` | 1–25 000, domyślnie 1 000 |
| `startRow` | offset od 0 (paginacja) |
| `dataState` | `final`, `all` (świeże, niekompletne dane), `hourly_all` |

Odpowiedź: wiersze z `keys[]` (wartości wymiarów w kolejności `dimensions`) + `clicks`, `impressions`, `ctr`, `position`, plus `responseAggregationType`.

Ważne zachowanie API: **zwraca "top" wyniki, nie komplet** — wiersze sortowane malejąco po klikach, przy remisie kolejność arbitralna; dni bez danych są pomijane (żeby wiedzieć, które dni mają dane, zapytaj bez filtrów z wymiarem `DATE`). Źródło: <https://developers.google.com/webmaster-tools/v1/searchanalytics>

**Dane godzinowe** (kwiecień 2025): wymiar `HOUR` + `dataState: "HOURLY_ALL"`, do 10 dni wstecz, znaczniki czasu w formacie `YYYY-MM-DDThh:mm:ss-07:00` (czas pacyficzny). `HOURLY_ALL` sygnalizuje, że dane godzinowe mogą być częściowe.
Źródła: <https://developers.google.com/search/blog/2025/04/san-hourly-data>, <https://searchengineland.com/google-search-analytics-api-gains-hourly-break-down-for-past-10-days-454167>

### 1.2 URL Inspection (`urlInspection.index.inspect`)

Stan konkretnego URL w indeksie Google — to samo co przycisk „Sprawdź URL" w GSC, ale programowo.
Źródło: <https://developers.google.com/webmaster-tools/v1/urlInspection.index/inspect>

Request: `inspectionUrl` (wymagane, musi leżeć pod `siteUrl`), `siteUrl` (wymagane; property typu URL-prefix wymaga końcowego ukośnika), `languageCode` (opcjonalne, BCP-47, domyślnie `en-US` — tłumaczy komunikaty o problemach, więc `pl` da polskie opisy).

Odpowiedź `UrlInspectionResult` (<https://developers.google.com/webmaster-tools/v1/urlInspection.index/UrlInspectionResult>):

- `inspectionResultLink` — link do wyniku w UI GSC
- `indexStatusResult`: `verdict`, `coverageState`, `robotsTxtState`, `indexingState`, `lastCrawlTime`, `pageFetchState`, `googleCanonical`, `userCanonical`, `sitemap[]`, `referringUrls[]`, `crawledAs`
- `ampResult`, `mobileUsabilityResult` (`verdict` + `issues[]`), `richResultsResult` (`verdict` + `detectedItems[]`)

Enumy warte zapamiętania przy pisaniu narzędzia:

- `Verdict`: `VERDICT_UNSPECIFIED`, `PASS`, `PARTIAL`, `FAIL`, `NEUTRAL`
- `RobotsTxtState`: `ALLOWED`, `DISALLOWED` (+ `_UNSPECIFIED`)
- `IndexingState`: `INDEXING_ALLOWED`, `BLOCKED_BY_META_TAG`, `BLOCKED_BY_HTTP_HEADER`, `BLOCKED_BY_ROBOTS_TXT` (+ `_UNSPECIFIED`)
- `PageFetchState`: `SUCCESSFUL`, `SOFT_404`, `BLOCKED_ROBOTS_TXT`, `NOT_FOUND`, `ACCESS_DENIED`, `SERVER_ERROR`, `REDIRECT_ERROR`, `ACCESS_FORBIDDEN`, `BLOCKED_4XX`, `INTERNAL_CRAWL_ERROR`, `INVALID_URL` (+ `_UNSPECIFIED`)

**Ograniczenie:** API zwraca wyłącznie stan wersji z indeksu Google. Nie ma odpowiednika „Testuj adres URL na żywo" — nie sprawdzisz przez API świeżo wdrożonej strony. Cytat z dokumentacji: „presently only the status of the version in the Google index is available; you cannot test the indexability of a live URL".

### 1.3 Sitemaps

Metody: `list`, `get`, `submit`, `delete`.
Źródło: <https://developers.google.com/webmaster-tools/v1/sitemaps>

Zasób Sitemap zawiera m.in.: `path`, `lastSubmitted`, `lastDownloaded` (RFC 3339), `isPending`, `errors`, `warnings`, `contents[]` (typ + `submitted`/`indexed` dla web, image, video, news, mobile, androidApp, iosApp). Typy: `atomFeed`, `rssFeed`, `sitemap`, `patternSitemap`, `urlList`, `notSitemap`.

### 1.4 Sites

`list`, `get`, `add`, `delete` — zarządzanie właściwościami na koncie.
Źródło: <https://developers.google.com/webmaster-tools/v1/api_reference_index>

### 1.5 Czego API **nie** daje

- Brak testu „na żywo" URL-a (patrz wyżej).
- Brak żądania ponownego zaindeksowania. **Indexing API to inne API i nie nadaje się do zwykłych stron** — obsługuje wyłącznie strony ze structured data `JobPosting` albo `BroadcastEvent` w `VideoObject`, domyślna kwota 200/dzień. Cytat: „The Indexing API can only be used to crawl pages with either JobPosting or BroadcastEvent embedded in a VideoObject". Źródło: <https://developers.google.com/search/apis/indexing-api/v3/quickstart>
  → Dla mentzen.pl (blog prawno-podatkowy) Indexing API odpada. `[do weryfikacji]` czy w międzyczasie Google nie rozszerzyło zakresu.
- Brak raportów Core Web Vitals / linków / Page Experience przez API (są tylko w UI). `[do weryfikacji]`

---

## 2. Dostęp i auth

### 2.1 Endpointy

Dwie różne bazy URL — łatwo o pomyłkę:

- Search Analytics, Sitemaps, Sites: `POST/GET https://searchconsole.googleapis.com/webmasters/v3/sites/{siteUrl}/...`
- URL Inspection: `POST https://searchconsole.googleapis.com/v1/urlInspection/index:inspect`

Źródło: <https://developers.google.com/webmaster-tools/v1/urlInspection.index/inspect> oraz <https://developers.google.com/webmaster-tools/v1/searchanalytics/query>

`siteUrl` w ścieżce musi być URL-encoded. Dwa formaty property:

- URL-prefix: `https://www.mentzen.pl/` (z ukośnikiem na końcu)
- Domain property: `sc-domain:mentzen.pl`

### 2.2 Zakresy OAuth

Źródło: <https://developers.google.com/webmaster-tools/v1/how-tos/authorizing>

- `https://www.googleapis.com/auth/webmasters` — odczyt i zapis
- `https://www.googleapis.com/auth/webmasters.readonly` — tylko odczyt

Search Analytics i URL Inspection działają na obu. `sitemaps.submit/delete` i `sites.add/delete` wymagają pełnego `webmasters`.
Rekomendacja dla agenta: **domyślnie `webmasters.readonly`**; pełny zakres tylko w osobnym, świadomie odpalanym narzędziu do sitemap.

### 2.3 OAuth user vs service account

**OAuth (installed app / desktop flow)** — użytkownik loguje się raz przeglądarką, refresh token ląduje na dysku. Zaleta: działa natychmiast z uprawnieniami, które konto już ma w GSC. Wada: token wymaga odświeżania, a aplikacja w trybie „Testing" ma refresh tokeny krótkotrwałe (7 dni) — dla stałego narzędzia trzeba opublikować aplikację w OAuth consent screen. `[do weryfikacji: aktualny okres wygasania dla trybu Testing]`

**Service account** — działa i jest wygodniejszy dla headless/cron, ale wymaga jednego dodatkowego kroku: **e-mail konta usługi (`client_email` z JSON-a) trzeba dodać w Search Console jako właściciela delegowanego** (Ustawienia → Użytkownicy i uprawnienia → Dodaj użytkownika). „Delegowany właściciel" to ktoś, komu zweryfikowany właściciel nadał status właściciela bez tokenu weryfikacyjnego.
Źródła: <https://developers.google.com/search/apis/indexing-api/v3/prereqs>, <https://support.google.com/webmasters/answer/7687615>, <https://support.google.com/webmasters/thread/87959428/how-to-enable-a-service-account-to-access-search-console-api-of-a-specific-web-page>

Uwaga praktyczna: samo `gcloud auth application-default login` **nie wystarcza** — Application Default Credentials nie dają uprawnień do GSC bez powiązania konta z property. Zgłoszony problem: <https://github.com/googleapis/google-api-nodejs-client/issues/730>

**Rekomendacja dla tego projektu:** service account + JSON w pliku poza repo, uprawnienie „Pełny" (nie „Właściciel") jeśli wystarczy odczyt. Nie wymaga interakcji przeglądarkowej, przeżywa restarty, nadaje się do crona.

### 2.4 Wymagania wstępne w Google Cloud

Włączyć **Google Search Console API** (`searchconsole.googleapis.com`) w projekcie GCP.
Źródło: <https://developers.google.com/webmaster-tools/v1/prereqs>

---

## 3. Limity i koszty

### 3.1 Koszt

**Samo Search Console API jest bezpłatne.** Płatny jest wyłącznie BigQuery, jeśli zdecydujesz się na bulk export (sekcja 3.4).

### 3.2 Limity zapytań

Źródło: <https://developers.google.com/webmaster-tools/limits>

| Zasób | Per-site | Per-user | Per-project |
| --- | --- | --- | --- |
| Search Analytics | 1 200 QPM | 1 200 QPM | 40 000 QPM; 30 000 000 QPD |
| URL Inspection | **600 QPM; 2 000 QPD** | — | 15 000 QPM; 10 000 000 QPD |
| Pozostałe (sitemaps, sites) | — | 20 QPS; 200 QPM | 100 000 000 QPD |

Dodatkowo Search Analytics ma **„load quota"** — limit obciążenia, nie liczby zapytań: krótkoterminowy w oknach 10-minutowych (przy przekroczeniu odczekać 15 minut) i długoterminowy w oknach 1-dniowych (przy przekroczeniu zmniejszyć liczbę zapytań grupowanych/filtrowanych po `page` lub `query`). Dokumentacja zaznacza, że większość użytkowników go nie dotknie.

**Praktyczna konsekwencja dla agenta:** twardy sufit to `2 000 inspekcji URL na dobę na property`. Przy audycie kilkuset artykułów bloga to działa, ale narzędzie musi mieć własny licznik dzienny i cache wyników, bo agent potrafi w pętli wywołać to samo.

### 3.3 Limity danych (to jest ważniejsze niż QPS)

- **Retencja 16 miesięcy.** Dane starsze są trwale usuwane — ani API, ani UI ich nie zwróci. API pozwala odpytywać zakres do ok. 486 dni. Źródła: <https://www.lumar.io/blog/industry-news/google-update-search-console-api-now-includes-16-months-of-data/>, <https://seotesting.com/google-search-console/data-limitations/> `[do weryfikacji — to źródła wtórne; oficjalna dokumentacja Google mówi o 16 miesiącach w kontekście raportu Skuteczność]`
- **25 000 wierszy na jedno żądanie** (`rowLimit`), paginacja przez `startRow`.
- **Ok. 50 000 wierszy na dobę na site na search type** — sufit tego, ile w ogóle da się wyciągnąć z Search Analytics za dany dzień; wyżej nie wyjdziesz nawet paginacją. Zwracane są „top" wiersze po klikach. Źródła wtórne: <https://weld.app/blog/google-search-console-50k-limit>, <https://www.analyticsedge.com/blog/download-over-25000-rows-from-google-search-console-api/> `[do weryfikacji — nie znalazłem tej liczby w oficjalnej dokumentacji; oficjalna strona limitów jej nie podaje]`
- **Zapytania zanonimizowane.** Google pomija zapytania, których nie zadało więcej niż kilkadziesiąt osób w okresie 2–3 miesięcy. Nie ma dla nich wiersza ani w raporcie, ani w API — więc **suma kliknięć z wierszy nie zgadza się z totalem z wykresu**. Anonimizowane zapytania są wliczone w totale, dopóki nie nałożysz filtra; po nałożeniu filtra na `query` znikają również z totalu. Praktycznie: `suma(zawiera X) + suma(nie zawiera X) ≠ total`. Oficjalne omówienie: <https://developers.google.com/search/blog/2022/10/performance-data-deep-dive> `[do weryfikacji — treści artykułu nie udało mi się pobrać, opis pochodzi z omówień wtórnych; przeczytać przed budową raportów porównawczych]`

To jest kluczowe dla mentzen.pl: **wąskie, niszowe frazy prawno-podatkowe B2B to dokładnie ten typ zapytań, który wpada w anonimizację.** Raportowanie „ile fraz mamy w TOP10" będzie systematycznie zaniżone, a agent nie powinien wyciągać wniosków z różnicy total − suma wierszy bez ostrzeżenia.

### 3.4 Bulk data export do BigQuery

Codzienny eksport wszystkich danych o skuteczności do BigQuery, **z wyjątkiem zapytań zanonimizowanych** (te nadal nie są dostępne — eksport nie omija anonimizacji, ale omija limit 50 tys. wierszy/dzień i limit 25 tys. na request).
Źródło: <https://support.google.com/webmasters/answer/12918484>

**Konfiguracja** (<https://support.google.com/webmasters/answer/12917675>):

1. Projekt GCP z włączonym **billingiem** i włączonym BigQuery API.
2. W IAM dodać konto usługi Google: `search-console-data-export@system.gserviceaccount.com` z rolami **BigQuery Job User** + **BigQuery Data Editor**.
3. W Search Console: Ustawienia → Zbiorczy eksport danych → podać **ID projektu** (nie numer), nazwę datasetu (domyślnie `searchconsole`), lokalizację datasetu.
4. Pierwszy eksport do **48 godzin** po konfiguracji. Lokalizacji datasetu praktycznie nie da się później zmienić.

**Uwaga kosztowa z dokumentacji:** ustawić czas wygaśnięcia partycji na **min. 14 dni**, bo inaczej „data will be accumulated forever for your project" i rachunek rośnie w nieskończoność.

**Tabele** (<https://support.google.com/webmasters/answer/12917991>):

| Tabela | Zawartość |
| --- | --- |
| `searchdata_site_impression` | dane zagregowane na poziomie property: `data_date`, `site_url`, `query`, `is_anonymized_query`, `country` (ISO-3166-1 alpha-3), `search_type` (web/image/video/news/discover/googleNews), `device`, `impressions`, `clicks`, `sum_top_position` |
| `searchdata_url_impression` | to samo + `url`, `is_anonymized_discover`, `sum_position` zamiast `sum_top_position`, oraz kilkadziesiąt booleanów typu wyglądu w wynikach (`is_amp_top_stories`, `is_job_listing`, `is_video`, `is_organic`…) |
| `ExportLog` | log eksportów: `agenda`, `namespace`, `data_date`, `epoch_version` (rośnie, gdy dane wymagają korekty), `publish_time` |

**Pułapka przy liczeniu pozycji:** `sum_position` to suma pozycji liczonych **od zera**. Średnia pozycja (1-based, jak w UI) = `SUM(sum_position)/SUM(impressions) + 1`. Źródło: <https://support.google.com/webmasters/answer/12917991>, omówienie: <https://trevorfox.com/2024/07/google-search-console-tables-in-bigquery/>

Eksport odbywa się raz dziennie, o niestałej porze. Błędy przejściowe są ponawiane automatycznie, nieprzejściowe wymagają interwencji przed kolejnym eksportem.

**Koszty BigQuery:** obowiązuje darmowy tier (składowanie + przetwarzanie zapytań), powyżej niego opłaty za storage i za skanowane dane. Konkretne stawki: <https://cloud.google.com/bigquery/pricing> `[do weryfikacji — nie udało mi się pobrać aktualnych liczb; sprawdzić przed uruchomieniem]`. Dla pojedynczego serwisu wielkości mentzen.pl wolumen danych jest mały i przy sensownej partycji koszt powinien mieścić się w darmowym tierze `[do weryfikacji]`.

**Czy warto dla mentzen.pl?** Dwa argumenty za: (1) obchodzi limit 50 tys. wierszy/dzień, (2) buduje archiwum przeżywające 16-miesięczną retencję — po roku masz historię, której nikt inny już nie ma. Argument przeciw: wymaga billingu i nadzoru nad kosztami. Sensowne rozwiązanie pośrednie: **własny snapshot do SQLite/Parquet z API**, codzienny cron, bez BigQuery i bez billingu — dla jednego serwisu w zupełności wystarcza.

---

## 4. Biblioteki i przykłady integracji

### 4.1 Python

**Oficjalna: `google-api-python-client` + `google-auth`** — to jest domyślny wybór.

```bash
uv add google-api-python-client google-auth google-auth-oauthlib
```

Service account:

```python
from google.oauth2 import service_account
from googleapiclient.discovery import build

SCOPES = ["https://www.googleapis.com/auth/webmasters.readonly"]
creds = service_account.Credentials.from_service_account_file(
    "sa.json", scopes=SCOPES
)
sc = build("searchconsole", "v1", credentials=creds)

resp = sc.searchanalytics().query(
    siteUrl="sc-domain:mentzen.pl",
    body={
        "startDate": "2026-07-01",
        "endDate": "2026-07-31",
        "dimensions": ["query", "page"],
        "type": "web",
        "rowLimit": 25000,
        "startRow": 0,
        "dataState": "final",
    },
).execute()
```

URL Inspection przez ten sam klient: `sc.urlInspection().index().inspect(body={...}).execute()`.

**Wrapper: `searchconsole` (joshcarty/google-searchconsole)** — MIT, upraszcza zakresy dat, filtrowanie i zwraca `pandas.DataFrame`.
Źródła: <https://github.com/joshcarty/google-searchconsole>, <https://pypi.org/project/searchconsole/>

```python
import searchconsole
account = searchconsole.authenticate(client_config="client_secrets.json")
webproperty = account["https://www.mentzen.pl/"]
report = webproperty.query.range("today", days=-7).dimension("query").get()
```

Uwierzytelnianie: OAuth przez `client_config`, z opcją `serialize="credentials.json"` żeby nie klikać zgody za każdym razem. **Dokumentacja nie wspomina o wsparciu dla service accounts** — jeśli wybierzesz SA, ten wrapper prawdopodobnie odpada i zostaje oficjalny klient. `[do weryfikacji]`

Przykład ekstrakcji query×page tym wrapperem: <https://www.jcchouinard.com/searchconsole-api-wrapper-python/>

### 4.2 Node

**Oficjalna: `googleapis`** (<https://www.npmjs.com/package/googleapis>), dokumentacja klasy: <https://googleapis.dev/nodejs/googleapis/latest/searchconsole/classes/Searchconsole.html>

```bash
pnpm add googleapis
```

```js
import { google } from "googleapis";

const auth = new google.auth.JWT({
  keyFile: "sa.json",
  scopes: ["https://www.googleapis.com/auth/webmasters.readonly"],
});
const sc = google.searchconsole({ version: "v1", auth });

const { data } = await sc.searchanalytics.query({
  siteUrl: "sc-domain:mentzen.pl",
  requestBody: { startDate: "2026-07-01", endDate: "2026-07-31", dimensions: ["query"] },
});
```

Poradniki: <https://stateful.com/blog/google-search-console-nodejs>, <https://www.codeconcisely.com/posts/how-to-use-google-search-console-api-in-node-js/>

### 4.3 Gotowe CLI

| Narzędzie | Język | Uwagi |
| --- | --- | --- |
| [awkoy/gsc-cli](https://github.com/awkoy/gsc-cli) | TS/Node ≥20, MIT | `npm i -g @gsc-cli/cli`. Komendy: `sites`, `sitemaps`, `analytics query`, `inspect` (pojedynczo lub **batch po całym sitemapie**), `config`, `auth`, `doctor`. OAuth przez `gsc auth login` **albo** `GOOGLE_APPLICATION_CREDENTIALS`. Jednolity JSON na wyjściu (`ok`/`data`/`error`/`meta`) — jawnie zaprojektowane pod agentów AI. Status: pre-1.0, spodziewane drobne breaking changes. |
| [jpreagan/gsc-cli](https://github.com/jpreagan/gsc-cli) | — | starsze, mniej opisane `[do weryfikacji]` |
| [benedict2310/gsc-cli](https://github.com/benedict2310/gsc-cli) | Go | read-only |
| [ivankristianto/google-search-console-cli](https://github.com/ivankristianto/google-search-console-cli) | Node ≥10 | obsługuje też Indexing API |
| `google-search-console-cli` | Python | `pipx install google-search-console-cli` `[do weryfikacji — nie sprawdziłem repo ani wydawcy]` |

**Wspólny wzorzec:** wszystkie wspierają service account przez `GOOGLE_APPLICATION_CREDENTIALS` i JSON na stdout.

### 4.4 Serwery MCP dla GSC

| Repo | Uwagi |
| --- | --- |
| [ahonn/mcp-server-gsc](https://github.com/ahonn/mcp-server-gsc) | MIT, ~261 gwiazdek — największy. `npm i mcp-server-gsc`. Jedno główne narzędzie `search_analytics` z wymiarami, regexami, do 25 000 wierszy i wykrywaniem „quick wins". Auth: service account przez `GOOGLE_APPLICATION_CREDENTIALS`, e-mail SA dodany jako administrator property. |
| [surendranb/google-search-console-mcp](https://github.com/surendranb/google-search-console-mcp) | MIT, ~38 gwiazdek. `uvx google-search-console-mcp` albo `npx -y @surendranb/google-search-console-mcp`. Szerszy zakres: search analytics, lista site'ów, URL inspection, zarządzanie sitemapami, diagnostyka SEO. Auth: service account. |
| [acamolese/google-search-console-mcp](https://github.com/acamolese/google-search-console-mcp) | MIT, read-only, OAuth, generuje raporty audytowe HTML. |
| [Shin-sibainu/google-search-console-mcp-server](https://github.com/Shin-sibainu/google-search-console-mcp-server) | pod Claude Code / Claude Desktop. |
| [ncosentino/google-search-console-mcp](https://github.com/ncosentino/google-search-console-mcp) | zero-dependency, gotowe binarki natywne (Linux/macOS/Windows). |
| [garethcull/search-console-mcp](https://github.com/garethcull/search-console-mcp) | Flask/Python; **tłumaczy naturalny język na payload API przez Gemini 2.5 Flash** — dodatkowa zależność od zewnętrznego LLM-a, raczej niepotrzebna warstwa dla Claude Code. |
| [sarahpark/google-search-console-mcp](https://github.com/sarahpark/google-search-console-mcp) | porównania okresów, „find opportunities", śledzenie stron. |

**Ocena:** żaden z tych serwerów nie jest oficjalny ani nie ma widocznego backingu instytucjonalnego. Przed użyciem któregokolwiek — zgodnie z zasadą z CLAUDE.md — zlecić subagentowi sprawdzenie pakietu (właściciel, data ostatniego wydania, pobrania, CVE, podobieństwo nazwy). To są narzędzia, którym oddajesz JSON konta usługi z dostępem do danych firmowych.

**Rekomendacja:** dla mentzen.pl napisać **własne cienkie CLI w Pythonie na `google-api-python-client`**, wypisujące JSON, i wywoływać je przez Bash. Powody: (a) pełna kontrola nad tym, co dostaje klucz, (b) możliwość wbudowania cache i licznika kwoty URL Inspection, (c) możliwość zaszycia domenowej logiki (progi, słowniki fraz prawno-podatkowych, mapowanie URL→artykuł WP), której żaden generyczny serwer MCP nie ma. Gotowe MCP dobre na szybki prototyp/porównanie.

---

## 5. Zastosowanie dla agenta SEO (konkretne workflow)

### 5.1 Striking distance — pozycje 5–20

Najwyższy stosunek zysku do wysiłku. Zapytanie: 28 dni, `dimensions: ["query","page"]`, `type: "web"`, filtr `country = pol`, `rowLimit: 25000`. Filtrowanie po stronie narzędzia: `position` w przedziale 4.5–20, `impressions >= 100`, `clicks` niskie.

Wynik: lista „fraza → URL", na której agent odpala się dalej — czyta artykuł z WP, sprawdza, czy fraza występuje w H1/H2/lead, proponuje rozbudowę sekcji. Dla mentzen.pl typowo: „składka zdrowotna ryczałt 2026", „estoński CIT warunki", „KSeF obowiązek" — frazy, gdzie serwis jest na 8–12 miejscu i brakuje jednej dedykowanej sekcji.

### 5.2 Kanibalizacja zapytań

Dla każdego `query` pobrać wszystkie `page` i znaleźć frazy, gdzie ≥2 URL-e zbierają wyświetlenia, a żaden nie dominuje. W blogu prawno-podatkowym to plaga: co roku powstaje nowy artykuł „Składka zdrowotna 2024/2025/2026" i wszystkie konkurują ze sobą.

Agent generuje rekomendację: konsolidacja + 301, albo przekierowanie linkowania wewnętrznego na kanoniczny artykuł. Weryfikacja po wdrożeniu przez porównanie okresów.

### 5.3 Wykrywanie spadków (cron dzienny/tygodniowy)

Snapshot `date × query` i `date × page` do lokalnego SQLite. Porównanie ostatnich 7 dni vs poprzednich 7 (albo rok do roku — istotne przy sezonowości podatkowej: styczeń, kwiecień, grudzień). Alert, gdy URL traci >30% kliknięć lub >3 pozycje przy stabilnych wyświetleniach.

Uwaga: `dataState: "final"` dla porównań, `"all"` tylko gdy potrzeba świeżości i akceptujesz niekompletność. Dane godzinowe (`HOUR` + `HOURLY_ALL`, 10 dni) przydają się wyłącznie do szybkiej reakcji po wdrożeniu — nie do raportowania.

### 5.4 Audyt indeksacji

Wejście: URL-e z sitemapy WP (`sitemaps.list` + `get`, albo bezpośrednio parsowanie XML). Dla każdego — `urlInspection`. Raportuj URL-e z:

- `indexStatusResult.verdict != PASS`
- `robotsTxtState == DISALLOWED`
- `indexingState != INDEXING_ALLOWED`
- `pageFetchState != SUCCESSFUL`
- `googleCanonical != userCanonical` ← najczęstszy realny problem na WP (paginacja, parametry, wersje z/bez `www`)
- `lastCrawlTime` starsze niż X dni przy świeżo zaktualizowanej treści

**Twardy limit: 2 000 URL-i na dobę na property.** Narzędzie musi trzymać licznik i cache (np. nie sprawdzaj ponownie URL-a z `PASS` częściej niż raz na 30 dni), inaczej agent wypali kwotę w jednym audycie i zablokuje sobie resztę dnia.

Dodatkowo `richResultsResult.detectedItems` weryfikuje, czy schema (FAQPage, Article, Organization) faktycznie jest widziana przez Google — istotne dla treści prawnych, gdzie FAQ ma sens.

### 5.5 Luki w treści i briefy

Zapytania z wysokimi `impressions` i `ctr` poniżej mediany dla danej pozycji → problem z title/meta description, nie z treścią. Agent przepisuje title, potem mierzy przez porównanie okresów.

Zapytania, na które serwis wyświetla się przypadkowo (wysokie `impressions`, pozycja 20+, brak dedykowanej strony) → kandydat na nowy artykuł. Dla kancelarii to naturalne wejście: agent zbiera 20 takich fraz, grupuje tematycznie i proponuje plan redakcyjny.

### 5.6 Weryfikacja wdrożeń

Każda zmiana (nowy title, rozbudowa artykułu, konsolidacja) dostaje wpis w lokalnym logu z datą. Po 14 i 28 dniach agent automatycznie porównuje okres przed/po dla dotkniętych URL-i i raportuje, czy zmiana zadziałała. To zamyka pętlę — bez tego agent generuje rekomendacje, których nikt nie weryfikuje.

### 5.7 Sitemapy

`sitemaps.list` → alert, gdy `errors > 0`, `warnings > 0`, albo `lastDownloaded` starsze niż tydzień. Po większej publikacji na WP: `sitemaps.submit` (wymaga zakresu `webmasters`, nie `readonly`).

### 5.8 Zasady dla agenta — do wpisania w skill

1. **Nigdy nie sumuj wierszy jako totalu.** Anonimizowane zapytania nie mają wierszy. Podając „łączne kliknięcia", pobierz je osobnym zapytaniem bez wymiaru `query`.
2. **Domyślnie `dataState: "final"`.** Ostatnie 2–3 dni są niekompletne i wyglądają jak spadek.
3. **Filtruj `country = pol`** — mentzen.pl to rynek polski, ruch zagraniczny zaszumia CTR i pozycje.
4. **`position` to średnia ważona wyświetleniami** — zmiana z 8.0 na 6.0 może oznaczać wejście do TOP5 na jednej frazie albo wypadnięcie długiego ogona. Zawsze patrz razem z `impressions`.
5. **Kwota URL Inspection to 2 000/dobę.** Sprawdź licznik przed batchem.
6. **16 miesięcy retencji** — porównania rok do roku działają tylko przez ~4 miesiące w roku. Stąd sens własnego archiwum (5.3).

---

## 6. Wymagana konfiguracja po stronie użytkownika

1. **Projekt Google Cloud** — utworzyć (lub użyć istniejącego) i włączyć **Google Search Console API** (`searchconsole.googleapis.com`).
   <https://console.cloud.google.com/apis/library/searchconsole.googleapis.com>

2. **Konto usługi** — utworzyć w IAM & Admin → Service Accounts, wygenerować klucz JSON. Zanotować `client_email`.
   Bez ról w GCP — uprawnienia nadaje się po stronie Search Console (krok 3).

3. **Nadanie dostępu w Search Console** — w property mentzen.pl: Ustawienia → Użytkownicy i uprawnienia → Dodaj użytkownika → wkleić `client_email` konta usługi.
   - Poziom **Pełny** wystarcza do odczytu Search Analytics + URL Inspection.
   - Poziom **Właściciel** potrzebny do `sitemaps.submit/delete` i `sites.add/delete`.
   - Ustalić, czy pracujecie na property typu **Domena** (`sc-domain:mentzen.pl`) czy **Prefiks URL** (`https://www.mentzen.pl/`) — format `siteUrl` w każdym wywołaniu API zależy od tej decyzji. Domena obejmuje wszystkie subdomeny i protokoły, więc zwykle lepsza.

4. **Umieszczenie klucza** — plik JSON **poza repozytorium projektu**, np. `~/.config/gsc/mentzen-sa.json`, `chmod 600`. Ścieżka przez zmienną `GOOGLE_APPLICATION_CREDENTIALS` w pliku `.env` (też poza repo, w `.gitignore`).

5. **Weryfikacja dostępu** — najprościej `sites.list`; powinien zwrócić property mentzen.pl z poziomem uprawnień.

6. **Opcjonalnie: bulk export do BigQuery** (tylko jeśli chcecie archiwum ponad 16 miesięcy albo pełne wiersze ponad limit dzienny):
   - włączyć **billing** w projekcie GCP i BigQuery API,
   - w IAM dodać `search-console-data-export@system.gserviceaccount.com` z rolami **BigQuery Job User** i **BigQuery Data Editor**,
   - w Search Console: Ustawienia → Zbiorczy eksport danych → ID projektu, nazwa datasetu, lokalizacja (nieodwracalna),
   - **od razu ustawić partition expiration ≥ 14 dni** i budżet z alertem w GCP,
   - pierwszy eksport do 48 h.

7. **Decyzja o granicy uprawnień** — rekomendacja: dwa osobne konfiguracje. Domyślna dla agenta z zakresem `webmasters.readonly`, druga (zapisowa, do sitemap) uruchamiana świadomie przez użytkownika. Agent nie powinien mieć w domyślnej ścieżce możliwości usunięcia sitemapy albo property.

---

## Otwarte pytania / do weryfikacji

- Limit ~50 000 wierszy/dobę/site/search type — nie znalazłem w oficjalnej dokumentacji Google, wyłącznie w źródłach wtórnych. Zweryfikować empirycznie na produkcyjnym property.
- Treść oficjalnego artykułu Google o filtrowaniu i limitach (`/search/blog/2022/10/performance-data-deep-dive`) — nie udało się pobrać; przeczytać przed budową modułu raportów.
- Aktualne stawki i darmowy tier BigQuery.
- Czy `searchconsole` (joshcarty) obsługuje service accounts.
- Czy Indexing API nadal ogranicza się do JobPosting/BroadcastEvent.
- Okres ważności refresh tokenu dla aplikacji OAuth w trybie „Testing".
- Dostępność przez API raportów Core Web Vitals i linków (prawdopodobnie brak).
