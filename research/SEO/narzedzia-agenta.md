# Narzędzia i API dla agenta SEO: GSC, GA4, Trends, keyword data, tech, sweep

Research: 2026-08-28. Kontekst: skill SEO (Claude Code) dla mentzen.pl — kancelaria prawo/podatki/księgowość B2B, WordPress (Yoast v28.3, Divi), rynek polski, YMYL. Destylat sześciu notatek researchowych; szczegóły w `_notes/narzedzia-*.md`.

## TL;DR

1. **Fundament to Google Search Console API** — darmowe, jedyne źródło prawdziwych fraz i pozycji mentzen.pl. Auth: service account (JSON poza repo), zakres `webmasters.readonly`. Zbuduj to jako pierwsze ([docs](https://developers.google.com/webmaster-tools/v1/searchanalytics/query), 2026-08-28).
2. **Drugi filar: DataForSEO** — jedno konto pay-as-you-go (min. wpłata $50, starczy na wiele miesięcy) pokrywa SERP-y PL, wolumeny z Keyword Plannera, keyword ideas, backlinki i widoczność w LLM. Realny miesięczny koszt przy jednym serwisie: pojedyncze dolary ([cennik SERP](https://dataforseo.com/pricing/serp/google-organic-serp-api), 2026-08-28).
3. **Buduj własne cienkie CLI w Pythonie (uv), nie polegaj na MCP.** Nazwane komendy z domyślnymi parametrami, JSON na stdout, cache w SQLite, twarde liczniki kwot i budżetu. MCP (oficjalne: Google Analytics, DataForSEO, Senuto) zostaw do eksploracji ad hoc — nie zapisuje historii, a bez historii nie ma odpowiedzi na „co się zmieniło".
4. **GA4 Data API** dokłada zachowanie i konwersje po kliknięciu; klucz łączenia z GSC to URL landing page'a. Bez oznaczonych key events (formularz, telefon) raport konwersji nie ma sensu — to trzeba sprawdzić przed budową.
5. **pytrends jest martwy** (archiwum od 2025-04-17) — do Trends używaj `trendspyg` (darmowy start) albo DataForSEO (produkcyjnie). Oficjalne Trends API to wciąż alfa za formularzem — złożyć wniosek, nie planować pod to architektury ([blog Google](https://developers.google.com/search/blog/2025/07/trends-api), 2026-08-28).
6. **Rich Results Test i validator.schema.org nie mają API** — walidację schema robi się lokalnie (advertools/extruct + własne reguły), Google zostaje krokiem ręcznym dla człowieka.
7. **IndexNow nie działa na Google** (uczestnicy: bing, yandex, seznam, naver, yep, internetarchive, amazonbot — [searchengines.json](https://www.indexnow.org/searchengines.json), 2026-08-28), a Indexing API Google obsługuje tylko JobPosting/BroadcastEvent — dla bloga kancelarii oba nie przyspieszą indeksacji w Google.
8. **Nie kupuj Ahrefs/Semrush wyłącznie dla API** ($249+/mies. + units za dane, które przy tej skali DataForSEO da za kilka dolarów rocznie). Senuto (od ~457 zł/mies. z API, własny serwer MCP) rozważ dopiero, gdy podstawa działa — jego przewagą jest polska baza fraz i klastrowanie polskiej odmiany.
9. **Największe pułapki danych:** anonimizacja zapytań w GSC (niszowe frazy B2B znikają z wierszy — nigdy nie sumuj wierszy jako totalu), sampling/thresholding/`(other)` w GA4, próbkowanie Trends (różnice <5 pkt to szum). Każde CLI musi te sygnały wykrywać i dopisywać ostrzeżenie do wyjścia.
10. **Konfiguracja użytkownika sprowadza się do:** jeden projekt GCP (GSC API + GA4 Data API + PSI + CrUX, service account + klucz API), konto DataForSEO, Application Password w WP dla osobnego konta agenta, opcjonalnie Bing Webmaster Tools. Szczegółowa checklista na końcu.

---

## Zasady architektury — do wpisania w skill

- **Własne CLI zamiast generycznych wrapperów.** Warstwa nad każdym z tych API to kilkaset linii, a klucz z dostępem do danych firmowych nie powinien przechodzić przez niesprawdzony kod społecznościowy. Żaden z siedmiu znalezionych serwerów MCP dla GSC nie ma backingu instytucjonalnego. Każdą zewnętrzną zależność przepuszczaj przez procedurę audytu pakietu z CLAUDE.md.
- **Jedna komenda = jeden nazwany raport** (`gsc striking-distance --last 28d`, `serp-inspect "estoński cit"`), nie surowe `runReport`/`query` z dowolnymi parametrami. Agent z wolnym API zje kwotę albo dostanie próbkowane dane i tego nie zauważy.
- **Cache i historia w SQLite/Parquet są obowiązkowe.** Dane historyczne się nie zmieniają, limity są realne, a wartość leży w diffach: właściwy interfejs dla agenta to nie 200 pozycji, tylko „6 fraz spadło, 2 wypadły z top 10".
- **Twarde liczniki:** kwota URL Inspection (2 000/dobę), tokeny GA4 (`returnPropertyQuota: true` w każdym wywołaniu), budżet dolarowy DataForSEO na przebieg. Przy niskim stanie komenda odmawia, nie próbuje.
- **Granica uprawnień:** domyślna ścieżka agenta read-only (`webmasters.readonly`, `analytics.readonly`, WP tylko odczyt + szkice). Operacje zapisowe (submit sitemapy, publikacja) w osobnych, świadomie odpalanych narzędziach.
- **Sekrety:** wszystkie klucze w `.env` / plikach poza repo (`chmod 600`), nigdy w CLAUDE.md ani kodzie. Klucz Semrush jedzie w query stringu — gdyby kiedyś doszedł, klient nie może logować pełnych URL-i.
- **Nic nie wymaga otwartych portów** — wszystko to lokalne skrypty; ewentualne dashboardy (Unlighthouse, LHCI server) wiąż na `127.0.0.1`.

---

## 1. Google Search Console API — fundament

**Co daje:** Search Analytics (`clicks`/`impressions`/`ctr`/`position` w wymiarach query/page/country/device/date/hour), URL Inspection (stan URL-a w indeksie: verdict, canonical Google vs user, `lastCrawlTime`, rich results), Sitemaps, Sites. Darmowe. ([referencja](https://developers.google.com/webmaster-tools/v1/searchanalytics/query), 2026-08-28)

**Czego nie daje:** testu URL „na żywo" (tylko stan z indeksu), żądania ponownej indeksacji (Indexing API obsługuje wyłącznie JobPosting/BroadcastEvent — [docs](https://developers.google.com/search/apis/indexing-api/v3/quickstart)), raportów CWV i linków [do weryfikacji].

**Auth — rób tak:** service account w projekcie GCP, `client_email` dodany w Search Console jako użytkownik z poziomem „Pełny" (Ustawienia → Użytkownicy i uprawnienia). Działa headless, przeżywa restarty, nadaje się do crona. OAuth desktop flow odpada dla stałego narzędzia (refresh token w trybie „Testing" wygasa po ~7 dniach [do weryfikacji]). Samo `gcloud auth application-default login` **nie wystarcza** — ADC bez powiązania konta z property nie da dostępu ([issue](https://github.com/googleapis/google-api-nodejs-client/issues/730)).

**Limity, które zmieniają projekt narzędzia** ([limits](https://developers.google.com/webmaster-tools/limits), 2026-08-28):

- **URL Inspection: 2 000/dobę na property** — audyt kilkuset artykułów działa, ale CLI musi mieć licznik dzienny i cache (URL z `PASS` nie sprawdzaj częściej niż raz na 30 dni), inaczej agent wypali kwotę w jednej pętli.
- **Retencja 16 miesięcy** — dane starsze przepadają bezpowrotnie; porównania rok-do-roku działają tylko ~4 miesiące w roku. Dlatego dzienny snapshot `date × query` i `date × page` do lokalnego SQLite od pierwszego dnia. BigQuery bulk export ([docs](https://support.google.com/webmasters/answer/12918484)) jest przerostem dla jednego serwisu — wymaga billingu; własny snapshot wystarcza.
- **25 000 wierszy/request** (paginacja `startRow`); sufit ~50 000 wierszy/dobę/site/search type pochodzi ze źródeł wtórnych [do weryfikacji].
- **Anonimizacja zapytań:** frazy zadane przez zbyt mało osób nie mają wierszy, ale są wliczone w totale — więc `suma(zawiera X) + suma(nie zawiera X) ≠ total` ([omówienie Google](https://developers.google.com/search/blog/2022/10/performance-data-deep-dive) [do weryfikacji — treści nie udało się pobrać]). Dla mentzen.pl to kluczowe: **niszowe frazy prawno-podatkowe B2B to dokładnie ten typ zapytań, który wpada w anonimizację** — raport „ile fraz w TOP10" będzie systematycznie zaniżony.

**Reguły dla agenta:** totale pobieraj osobnym zapytaniem bez wymiaru `query`; domyślnie `dataState: "final"` (ostatnie 2–3 dni są niekompletne i wyglądają jak spadek); filtruj `country = pol`; `position` to średnia ważona wyświetleniami — czytaj zawsze razem z `impressions`.

**Implementacja:** `uv add google-api-python-client google-auth`. Wrapper `searchconsole` (joshcarty) dokumentuje wyłącznie OAuth (`client_config` + `serialize`), bez żadnej wzmianki o service accounts — README sprawdzone 2026-08-28 — pomiń.

---

## 2. GA4 Data API — zachowanie po kliknięciu

**Rola:** GSC mówi „skąd i na co przyszli", GA4 — „co zrobili po wejściu". Klucz łączenia: URL landing page'a. Praktyczny sygnał: wysokie impressions + niski CTR (GSC) = popraw title/description; dobry CTR + niski `engagementRate` (GA4) = treść nie dowozi obietnicy z SERP-a. ([przegląd API](https://developers.google.com/analytics/devguides/reporting/data/v1), 2026-08-28)

**Wymiary/metryki dla SEO:** filtr `sessionDefaultChannelGroup == "Organic Search"`; wymiary `landingPagePlusQueryString`, `deviceCategory`, `sessionSourceMedium`; metryki `sessions`, `engagementRate`, `keyEvents`, `sessionKeyEventRate` (od maja 2024 „conversions" → „key events"; stare nazwy zdeprecjonowane — [changelog](https://developers.google.com/analytics/devguides/reporting/data/v1/changelog)). Kilka `dateRanges` w jednym requeście załatwia porównania okresów bez dwóch zapytań.

**Auth:** ten sam wzorzec co GSC — service account, e-mail dodany w GA4 (Administracja → Zarządzanie dostępem) z rolą przeglądającego. Zakres `analytics.readonly`. Property ID (numeryczne, nie „G-XXXX") z `accountSummaries.list` Admin API.

**Limity:** brak opłat; standardowa właściwość ma 200 000 core tokens/dobę i 40 000/h ([quotas](https://developers.google.com/analytics/devguides/reporting/data/v1/quotas)). Koszt tokenowy zapytania jest nieprzewidywalny z góry — dlatego `returnPropertyQuota: true` zawsze i logowanie pozostałego budżetu.

**Pułapki jakości danych** ([reporting-data-expectations](https://developers.google.com/analytics/devguides/reporting/data/v1/reporting-data-expectations)) — CLI musi je wykrywać:

- **Świeżość:** okno raportowe kończ na `2daysAgo` lub wcześniej.
- **Sampling:** sprawdzaj `samplingMetadatas` w odpowiedzi i ostrzegaj — agent, który nie wie, że patrzy na próbkę 12%, wygeneruje pewne siebie i błędne rekomendacje.
- **Wiersz `(other)`:** wymiary o >500 unikalnych wartościach dziennie zwijają rzadkie wartości, a **filtry działają PO utworzeniu `(other)`** — przy dużej liczbie URL-i (blog + interpretacje) pobieraj pełną listę i grupuj lokalnie, nie filtruj po stronie API.
- **Thresholding:** pole `subjectToThresholding` — sprawdzać i raportować.

**Implementacja:** `uv add google-analytics-data` (oficjalny pakiet Google, 0.23.0 z 2026-06-03). Oficjalny MCP `googleanalytics/google-analytics-mcp` (eksperymentalny) — tylko do eksploracji.

---

## 3. Keyword i SERP data — DataForSEO jako centrum, reszta warunkowo

### DataForSEO (rekomendowany)

Pay-as-you-go bez abonamentu, HTTP Basic auth, $1 kredytu na start, min. wpłata $50. Jedno konto pokrywa ([cenniki](https://dataforseo.com/pricing/serp/google-organic-serp-api), 2026-08-28):

| Zasób | Koszt | Zastosowanie |
| --- | --- | --- |
| SERP Google Organic (Standard queue) | $0.0006/SERP (~5 min) | rank tracking, analiza SERP przed pisaniem, PAA |
| SERP Live | $0.002/SERP (~6 s) | pojedyncze `serp-inspect` |
| Keywords Data / Google Ads | $0.06/zadanie do 1000 fraz | wolumeny + `monthly_searches` (12 mies. → sezonowość) |
| Labs `keyword_ideas` / `ranked_keywords` | $0.012/zadanie + $0.00012/element | luki contentowe, przegląd zasięgu domeny |
| Backlinks | $0.024/zadanie + $0.000036/wiersz | miesięczny diff profilu linków |
| LLM Responses | $0.0002/zadanie (Standard) lub $0.0006 (Live) + koszt LLM | widoczność marki w ChatGPT/Claude/Gemini/Perplexity |

Rachunek dla 200 fraz mierzonych co tydzień: **~$0.48/mies.** Pomiar dzienny: ~$3.60. Parametry PL: `location_name: "Poland"`, `language_code: "pl"` (kod `location_code` zweryfikuj przez `/v3/keywords_data/google_ads/locations` — prawdopodobnie 2616, nie 616 [do weryfikacji]). Oficjalny serwer MCP: `npx dataforseo-mcp-server@latest` ([repo](https://github.com/dataforseo/mcp-server-typescript)).

Zasady: nie włączaj `include_clickstream_data` odruchowo (podwaja rachunek); nie używaj trybu Live tam, gdzie kolejka Standard odda wynik w 5 minut; ustaw limit wydatków w panelu.

**Ryzyko prawne:** automatyczne pobieranie SERP-ów Google narusza ToS Google niezależnie od tego, czy robi to własny skrypt, czy dostawca API. SerpApi reklamuje „U.S. Legal Shield" do 2 mln USD ([pricing](https://serpapi.com/pricing)), ale jego skuteczność dla podmiotu z Polski jest niezbadana [do weryfikacji]. Przy kancelarii to decyzja, którą użytkownik musi podjąć świadomie przed wdrożeniem — nie efekt uboczny wyboru narzędzia.

### Czego nie kupować (przy jednym serwisie B2B)

- **Ahrefs / Semrush wyłącznie dla API.** Ahrefs: plan Standard $249/mies. + units, minimum 50 units za każde zapytanie (odpytywanie po jednej frazie jest 50× droższe niż batch — [limits](https://docs.ahrefs.com/en/api/docs/limits-consumption)). Semrush: plan SEO Business (~$500/mies. [do weryfikacji]) to tylko przepustka, units kupuje się osobno. Metryki `traffic_potential`/`parent_topic` Ahrefs są realnie użyteczne, ale nie za tę cenę przy tej skali.
- **Google Ads API (Keyword Planner) jako pierwsza ścieżka.** Najdłuższa konfiguracja w całym zestawie: konto MCC, developer token, wniosek o poziom Basic/Standard — poziom Explorer, nadawany czasem automatycznie po rejestracji, **blokuje `KeywordPlanIdeaService`** ([access levels](https://developers.google.com/google-ads/api/docs/api-policy/access-levels)). Wniosek warto złożyć wcześnie i równolegle, ale startować na DataForSEO.
- **Google Custom Search JSON API** — zamknięte dla nowych klientów, wyłączenie 1 stycznia 2027 ([docs](https://developers.google.com/custom-search/v1/overview)). Nie budować niczego.
- **Osobny SaaS do rank trackingu i platformy AI visibility ($499/mies. Profound itp.)** — duplikują pętlę SERP API → SQLite → diff, którą i tak trzeba mieć.

### Senuto — warunkowo, jako rozszerzenie PL

Jedyny dostawca z bazą budowaną pod polski Google (deklarowane 80 mln fraz PL) i klastrowaniem dostrojonym do polskiej odmiany — dla długiego ogona typu „czy fundacja rodzinna płaci CIT od najmu" spodziewana przewaga nad globalnymi bazami (rozumowanie, nie pomiar — twardych porównań dla polskiego brak). Ma własny zdalny serwer MCP `https://mcp.senuto.com/mcp` (23 narzędzia, reklamowany z Claude Code — [senuto.com/pl/mcp](https://www.senuto.com/pl/mcp/)). Koszt: plan Advanced 457,50 zł/mies. + dodatek MCP 69 zł, albo niższy plan + dodatek API 199 zł [do weryfikacji cen w panelu]. Decyzja budżetowa użytkownika — dokładać dopiero, gdy fundament (GSC + DataForSEO) działa i widać, czego brakuje.

---

## 4. Google Trends — warstwa czasowa, nie wolumenowa

**Stan ekosystemu:** oficjalne API od lipca 2025 wciąż w alfie za formularzem zgłoszeniowym ([apply](https://developers.google.com/search/apis/trends)) — złóż wniosek (opisz konkretny use case: sezonowość fraz podatkowych dla planowania treści), ale nie projektuj pod to. `pytrends` zarchiwizowany 2025-04-17, nie instalować. Realne ścieżki:

- **`trendspyg`** (MIT, aktywny — ostatni push 2026-08-19) — darmowy start. Wymaga Chrome dla ścieżek Explore/CSV; rate limit ~8–10 sesji Explore/15 min, potem 429 na 35+ minut — realny sufit to kilkadziesiąt zapytań Explore dziennie, cache obowiązkowy. Uwaga: młody projekt jednego autora, nazwa myląco podobna do nieutrzymywanego `trendspy` (ostatni commit 2024-12-25) — audyt pakietu przed instalacją, wersja zapięta na sztywno.
- **RSS „Trending now"**: `https://trends.google.com/trending/rss?geo=PL` — bez auth, bezpieczny do pollowania co 15–30 min (zweryfikowane 2026-08-28). Tylko trendy dzienne — użyteczne wyłącznie jako trigger newsjackingowy z filtrem regex branżowym (`podatk|PIT|CIT|VAT|ZUS|KSeF|faktur|...`).
- **DataForSEO Google Trends** ($0.011/task live, maks. 5 fraz/request) — docelowa ścieżka produkcyjna: bez 429, deterministyczna. Roczny koszt dla koszyka 200 fraz: ~$5.30.

**Do czego Trends się nadaje, a do czego nie:** nadaje się do kształtu sezonu i porównań względnych między frazami; **nie nadaje się** do wolumenów (indeks 0–100, nie liczba wyszukiwań), long-taila (niski wolumen = `0` i szum) ani decyzji na podstawie różnic <5 pkt. Te same zapytanie w różne dni zwraca różne wartości — pobieraj serię 3–5 dni z rzędu i pracuj na średniej ([badanie reliability, PMC8186442](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8186442/)). Nigdy nie mieszaj serii z różnymi `timeframe` (normalizacja jest per zapytanie); do łączenia wyników kotwicz stałą frazę referencyjną w każdym zapytaniu.

**Najmocniejsze zastosowanie:** rozróżnienie „spadek bo sezon" od „spadek bo utrata pozycji" — reguła do wpisania na sztywno w skill:

- Trends spada i GSC spada w podobnym tempie → sezon, nie ruszaj strony.
- Trends płaski/rośnie, GSC spada → problem SEO, eskaluj.
- Trends rośnie, GSC płaski → niewykorzystana szansa.

Bez tej reguły agent regularnie diagnozuje sezonowe spadki (branża podatkowa jest brutalnie sezonowa: PIT luty–kwiecień, CIT marzec, zmiany przepisów od stycznia) jako awarię.

Drugie zastosowanie: **mapa sezonowości koszyka 150–300 fraz filarowych** (5-letnie szeregi tygodniowe, profil roczny per fraza → tydzień startu wzrostu) → kalendarz, w którym publikacja/refresh wypada 4–6 tygodni przed startem sezonu. Alternatywnie tańszy przybliżony substytut: `monthly_searches` z DataForSEO Google Ads, które i tak dostajesz przy pobieraniu wolumenów.

---

## 5. Warstwa techniczna

### CWV: CrUX API (pole) + PSI (laboratorium)

- **CrUX API / CrUX History API** — dane realnych użytkowników Chrome; History daje 40 tygodni szeregu (aktualizacja poniedziałki ~04:00 UTC) i to jest właściwe narzędzie do pytania „czy CWV poprawiły się po zmianie X" ([docs](https://developer.chrome.com/docs/crux/history-api), 2026-08-28). Limit twardy: **150 zapytań/min na projekt GCP**, nie do podniesienia — przy skanie ~800 URL-i × 2 form factory potrzebny throttling.
- **PSI API** — pełny Lighthouse dla URL-a, do diagnozy przyczyn (`render-blocking-resources` itd.) dla stron, gdzie CrUX pokazuje problem. Google zapowiada usunięcie danych CrUX z odpowiedzi PSI — **nie buduj logiki na polu `loadingExperience`** ([get-started](https://developers.google.com/speed/docs/insights/v5/get-started)). Limit 25 000/dobę ze źródeł wtórnych [do weryfikacji — sprawdzić w Cloud Console po założeniu projektu].
- **Unlighthouse** (MIT, Node 22+) do przekrojowego „które z 800 podstron mają słaby LCP" — bliższy potrzebie niż Lighthouse CI (który odpowiada na „czy ten commit pogorszył wydajność").
- **Uwaga o Divi:** motyw jest znany z ciężkiego CSS/JS — problemy LCP grupuj po typie szablonu (artykuł/kategoria/usługa), nie raportuj 800 osobnych ustaleń. CrUX to średnia krocząca 28 dni — efekt wdrożenia widać po ~4 tygodniach, agent nie może ogłaszać sukcesu po trzech dniach.

### Crawler: advertools

`uv add advertools` — Scrapy z gotowym schematem ekstrakcji SEO (title, meta, canonical, h1–h6, JSON-LD, linki wewnętrzne z anchorami, statusy, przekierowania), wyjście jsonlines → DataFrame ([docs](https://advertools.readthedocs.io/en/master/advertools.spider.html)). Plus `sitemap_to_df()` i parsowanie robots.txt. Wzorzec: nocny crawl + diff względem wczorajszego — raportuj tylko delty (nowe 404, nowe noindex, zmienione canonicale, duplikaty title), nie tę samą listę 200 znanych problemów co noc. Screaming Frog headless (licencja £199/rok — [cennik](https://www.screamingfrog.co.uk/seo-spider/pricing/), zweryfikowane 2026-08-28) kupować dopiero, gdy advertools nie wystarczy (główna różnica: rendering JS).

### Walidacja schema — tylko lokalnie

Rich Results Test ani validator.schema.org **nie mają publicznego API** (otwarta dyskusja [schemaorg#3261](https://github.com/schemaorg/schemaorg/issues/3261)). Rób: JSON-LD z crawlu advertools + własne reguły (artykuły: `Article`/`BlogPosting` z `author`, `datePublished`, `dateModified`; usługi: `LegalService`/`Organization`; autorzy: `Person` z `jobTitle` i `sameAs` — istotne dla E-E-A-T w YMYL). Wynik = lista URL-i do ręcznego sprawdzenia w Rich Results Test przez człowieka. Nie używaj komercyjnych scraperów walidatora Google (Apify/RapidAPI) — obca infrastruktura, niepewna legalność.

### IndexNow i Bing

- IndexNow = Bing/Copilot (i marginalnie inni), **nie Google**. Wartość: widoczność w asystentach AI zasilanych indeksem Binga. Najtańsza droga: oficjalna wtyczka IndexNow od Microsoftu w WP; własny poller tylko przy selektywnym zgłaszaniu ([dokumentacja](https://www.indexnow.org/documentation)).
- Bing Webmaster Tools API — darmowe `GetQueryStats` i dane o linkach jako kontrola krzyżowa dla GSC; znane zgłoszenia pustych odpowiedzi `GetLinkCounts` ([MS Answers](https://learn.microsoft.com/en-us/answers/questions/5939109/bing-webmaster-tools-api-getlinkcounts-and-geturll)) — przetestować empirycznie, nie zakładać, że działa.

### WordPress REST — interfejs zapisu agenta

Stan faktyczny mentzen.pl (sprawdzone curl-em 2026-08-28): `wp-json` otwarte do odczytu, posty zawierają `yoast_head_json`; sitemap Yoast działa; CPT `praktyka` i `interpretacje`; **robots.txt bez ani jednej dyrektywy** — brak nawet linii `Sitemap:` (tani task na start).

- **Odczyt:** publiczny, bez auth — pełna baza artykułów z meta Yoasta wystarcza do audytu treści.
- **Zapis:** Application Password (WP 5.6+) dla **osobnego konta agenta** z minimalną rolą, nigdy konta admina. Zapis wyłącznie jako `status: "draft"` — kancelaria publikuje treści o skutkach prawnych, człowiek zatwierdza. Sprawdzić, czy Wordfence Login Security nie blokuje Application Passwords przy 2FA [do weryfikacji].
- **Pułapka Yoast:** `yoast_head_json` jest read-only — POST cicho zignoruje. Zapis SEO title/meta description wymaga rejestracji kluczy meta przez `register_post_meta(..., show_in_rest => true)` w motywie potomnym/wtyczce ([Yoast dev](https://developer.yoast.com/customization/apis/rest-api/)) — zmiana w kodzie do wykonania przez człowieka; dokładne nazwy kluczy (`_yoast_wpseo_title`, `_yoast_wpseo_metadesc`) potwierdzić na instancji [do weryfikacji].
- Namespace `redirection/v1` istnieje — przy zmianie slugów agent mógłby tędy dodawać 301 [do weryfikacji endpointów].

---

## 6. Sweep: SERP tracking, AI visibility, backlinki, wzmianki

Rank tracking to nie produkt do kupienia, tylko pętla: lista fraz → SERP API → SQLite → diff. Konkretne workflow (wszystkie na DataForSEO, koszty pomijalne):

- **`serp-snapshot`** (tygodniowo, Standard queue): pozycje mentzen.pl, top 10 konkurencji, obecność AI Overview/featured snippet/PAA; wyjście = tylko diff (zmiany >3 pozycje, wejścia/wyjścia z top 10, nowe AI Overview).
- **`serp-inspect`** (na żądanie, Live): pełne 20 wyników + PAA przed pisaniem tekstu — SERP-y prawno-podatkowe są zdominowane przez poradnikprzedsiebiorcy, infor, ifirma i strony rządowe; bez tego kroku łatwo napisać tekst o złym formacie.
- **`domain-keywords`** (miesięcznie, Labs `ranked_keywords` dla mentzen.pl + 3–5 konkurentów): luki contentowe i frazy na pozycjach 11–20.
- **`ai-visibility`** — dwa strumienie: (a) raport „Generative AI performance" w GSC (AI Overviews + AI Mode, wyświetlenia, limit 1000 wierszy — [pomoc](https://support.google.com/webmasters/answer/16984139); dostępność przez API niepotwierdzona, sprawdzić empirycznie wymiarem `searchAppearance` [do weryfikacji]); (b) stały zestaw 30–50 polskich promptów („jaka kancelaria podatkowa dla spółki z o.o.") przez DataForSEO LLM Responses — metryka: udział promptów ze wzmianką marki + **lista domen cytowanych zamiast nas** (to gotowy plan linkbuildingu). ~640 zadań/mies. kosztuje ułamek najtańszego SaaS-a. Granica: do promptów nigdy nie trafiają treści spraw ani dane klientów.
- **`backlinks-diff`** (miesięcznie): nowe/utracone domeny linkujące, nowe anchory (wykrywanie negatywnego SEO). Profil linków zmienia się w skali tygodni — codzienny zrzut to marnotrawstwo.
- **`brand-mentions`** (tygodniowo): zapytania markowe do endpointu news/search, dedup po URL, wyjście = nowe wzmianki z podziałem linkują/nie linkują (te drugie = najtańsze zadania linkbuildingowe). Social media wymagałyby Brand24 — API [do weryfikacji u dostawcy], decyzja użytkownika (przy marce osobowej to obszar reputacyjny, nie tylko SEO).

**Czego agent nie robi automatycznie:** nie publikuje ani nie modyfikuje produkcji na podstawie spadku pozycji, nie kontaktuje się z podmiotami z listy wzmianek, nie wydaje budżetu API bez twardego limitu na przebieg.

---

## Co budować najpierw — rekomendowana kolejność

Kryterium: wartość na godzinę pracy, zerowy/niski koszt najpierw, zależności danych (diff wymaga historii, więc snapshoty startują wcześnie).

1. **GSC CLI + dzienny snapshot do SQLite** (cron). Raporty: striking-distance (pozycje 5–20, `impressions >= 100`), kanibalizacja (≥2 URL-e na frazę — w blogu z artykułami „Składka zdrowotna 2024/2025/2026" to plaga), wykrywanie spadków 7d vs 7d, weryfikacja wdrożeń po 14/28 dniach. Zero kosztu, dane własne, największa wartość.
2. **Tanie porządki techniczne:** linia `Sitemap:` w robots.txt; crawl advertools + diff nocny; lokalna walidacja schema.
3. **DataForSEO:** `serp-inspect` + `serp-snapshot` + wolumeny z `monthly_searches` (sezonowość gratis) + `domain-keywords`. Równolegle: złożyć wniosek o Google Ads API Basic i o alfę Trends API (oba to dni–tygodnie oczekiwania, nic nie kosztują).
4. **GA4 CLI:** ranking organicznych landing page'ów z konwersjami, złączenie z GSC po URL-u. Warunek wstępny: key events skonfigurowane w GA4.
5. **CWV:** CrUX History tygodniowo na origin + szablony; PSI punktowo do diagnozy.
6. **Trends** (trendspyg → docelowo DataForSEO): reguła sezon-vs-problem wpięta w raport spadków z punktu 1; mapa sezonowości koszyka fraz.
7. **AI visibility, backlinks-diff, brand-mentions, IndexNow/Bing** — dopiero gdy 1–5 działają i mają historię, do której da się je odnieść.

Punkty 1–4 pokrywają większość realnej wartości.

---

## Wymagana konfiguracja po stronie użytkownika

Jeden projekt GCP obsłuży wszystkie API Google. Kolejność wg roadmapy:

| # | Krok | Dla | Czas/koszt |
| --- | --- | --- | --- |
| 1 | Projekt GCP; włączyć `searchconsole.googleapis.com` | GSC | minuty, 0 zł |
| 2 | Service account + klucz JSON → `~/.config/…`, `chmod 600`, `GOOGLE_APPLICATION_CREDENTIALS` w `.env` poza repo | GSC, GA4 | minuty |
| 3 | GSC: Ustawienia → Użytkownicy → dodać `client_email` z poziomem „Pełny"; ustalić typ property (Domain `sc-domain:mentzen.pl` zwykle lepsza — obejmuje subdomeny i protokoły) | GSC | minuty |
| 4 | Konto DataForSEO, login/hasło API z `app.dataforseo.com/api-access`, wpłata $50, limit wydatków w panelu | SERP/keywords/backlinki/LLM | $50 na wiele miesięcy |
| 5 | Świadoma decyzja o ToS Google przy SERP API (sekcja 3) | — | decyzja |
| 6 | Lista 150–300 fraz filarowych (jedyny wsad, którego żadne API nie zastąpi) | tracking, sezonowość | praca wspólna z agentem |
| 7 | Włączyć GA4 Data API + Admin API; dodać e-mail SA w GA4 z rolą przeglądającego; zanotować `GA4_PROPERTY_ID`; zweryfikować key events (formularz, telefon, newsletter) | GA4 | minuty + weryfikacja zdarzeń |
| 8 | Włączyć PSI + CrUX API, klucz API ograniczony do tych dwóch usług; sprawdzić realny limit PSI w Quotas | CWV | minuty |
| 9 | WP: osobne konto agenta (rola Editor max), Application Password; snippet `register_post_meta` dla kluczy Yoasta (zmiana w kodzie); test na lokalnej kopii (`local-mentzen`) przed produkcją | zapis treści | godzina + deploy snippetu |
| 10 | Wnioski równoległe: Google Ads API poziom Basic (permissible use „Researching keywords"), alfa Trends API | Keyword Planner, Trends | dni–tygodnie oczekiwania |
| 11 | Opcjonalnie: Bing Webmaster Tools (weryfikacja + klucz API), wtyczka IndexNow, Senuto (decyzja budżetowa), Brand24 (decyzja o social) | uzupełnienia | 0 zł / abonamenty |

Sekrety docelowo: klucz SA Google, klucz API Google (PSI/CrUX), login+hasło DataForSEO, Application Password WP, opcjonalnie klucz Bing — jeden `.env` poza repo, `.gitignore` sprawdzony przed pierwszym commitem.

---

## Najważniejsze otwarte punkty [do weryfikacji]

- Sufit ~50 000 wierszy/dobę w GSC Search Analytics i treść oficjalnego artykułu o anonimizacji — sprawdzić empirycznie / przeczytać przed budową modułu raportów.
- Dostępność raportu Generative AI przez GSC API (`searchAppearance`) i czy property mentzen.pl już go ma.
- Czy Wordfence przepuszcza Application Passwords przy 2FA; dokładne klucze meta Yoasta; endpointy `redirection/v1`; czy hosting daje SSH (WP-CLI).
- Realny limit PSI w Cloud Console; `location_code` Polski w DataForSEO (2616 vs 616); polskie kody subregionów w Trends (`PL-MZ`…).
- Czy Basic wystarcza do `KeywordPlanIdeaService` w Google Ads API — przeczytać Permissible Use przed wnioskiem.
- Realny udział Binga w ruchu mentzen.pl — od tego zależy priorytet IndexNow/BWT.
