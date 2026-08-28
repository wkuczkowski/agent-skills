# Narzędzia techniczne dla agenta SEO (mentzen.pl)

Research: 2026-08-28. Obszar: PageSpeed Insights API, CrUX API, Lighthouse CI, walidacja schema.org, crawlery open-source/CLI, IndexNow, Bing Webmaster API, WordPress REST API.

Konwencja: fakty z URL-em źródła. Rzeczy nieudokumentowane oficjalnie oznaczone `[do weryfikacji]`.

## Stan faktyczny mentzen.pl (sprawdzone na żywo 2026-08-28)

Zanim cokolwiek budować, warto wiedzieć, z czym agent będzie rozmawiał. Sprawdzone `curl`-em na produkcji:

- WordPress z **Yoast SEO v28.3** (znacznik w `<head>`), motyw **Divi**.
- `https://mentzen.pl/wp-json/` odpowiada 200. Nazwa: „Kancelaria Mentzen". Namespace'y: `wp/v2`, `yoast/v1`, `redirection/v1`, `wordfence/v1`, `wordfence-login-security/v1`, `contact-form-7/v1`, `wpcf7r/v1`, `cptui/v1`, `divi/v1`, `omgf/v1`, `mlk-km/v1`, `mentzen-blog/v2`, `mentzen/v1`, `oembed/1.0`, `llar/v1`, `wp-site-health/v1`, `wp-block-editor/v1`, `wp-abilities/v1`.
- `GET /wp-json/wp/v2/posts?per_page=1` zwraca 200 bez autoryzacji; obiekt posta zawiera m.in. `yoast_head`, `yoast_head_json`, `meta`, `categories`, `tags`, `slug`, `modified`.
- `https://mentzen.pl/sitemap_index.xml` działa (Yoast). Zawiera co najmniej: `post-sitemap.xml`, `page-sitemap.xml`, `praktyka-sitemap.xml`, `interpretacje-sitemap.xml`. Czyli są własne typy postów (CPT UI) — `praktyka`, `interpretacje`.
- **`https://mentzen.pl/robots.txt` ma 24 linie i zero realnych dyrektyw.** Same komentarze (boilerplate Cloudflare „content signals" + klauzula o art. 4 dyrektywy 2019/790). Brak `User-agent`, brak `Disallow`, brak `Allow`, **brak linii `Sitemap:`**. To gotowy, tani task dla agenta na start.
- Obecność `wordfence-login-security/v1` sugeruje 2FA na logowaniu. Application Passwords zwykle działają obok 2FA, ale to trzeba potwierdzić w konfiguracji Wordfence `[do weryfikacji]`.
- Czy site ma plik klucza IndexNow — nie da się stwierdzić bez znajomości klucza (sprawdziłem zmyśloną nazwę, serwer oddał 404, co niczego nie dowodzi).

---

## 1) Co daje

### PageSpeed Insights API (PSI)
Zwraca pełny raport Lighthouse dla pojedynczego URL-a (dane laboratoryjne: performance, accessibility, best-practices, SEO) plus, historycznie, dane polowe CrUX. Google zapowiada wycofanie danych CrUX z tego API i kieruje po nie do CrUX API. Źródło: https://developers.google.com/speed/docs/insights/v5/get-started

Kluczowe pola odpowiedzi: `lighthouseResult` (audyty, metryki, score) oraz `loadingExperience` / `originLoadingExperience` (CrUX na poziomie URL i originu).

### CrUX API
Dane polowe (realni użytkownicy Chrome) dla URL-a albo originu. Metryki: `cumulative_layout_shift`, `first_contentful_paint`, `interaction_to_next_paint`, `largest_contentful_paint`, `experimental_time_to_first_byte`, `largest_contentful_paint_resource_type`, rozbicie LCP na fazy (`largest_contentful_paint_image_time_to_first_byte`, `_resource_load_delay`, `_resource_load_duration`, `_element_render_delay`), `navigation_types`, `round_trip_time`. Źródło: https://developer.chrome.com/docs/crux/api

Dane to średnia krocząca z 28 dni, opóźnienie ok. 2 dni, aktualizacja codziennie ok. 04:00 UTC.

**CrUX History API** — ten sam zestaw metryk, ale jako szereg czasowy: 40 tygodni wstecz (ok. 10 miesięcy), jeden okres zbiorczy na tydzień, domyślnie 25 wpisów, parametr `collectionPeriodCount` w zakresie 1–40. Aktualizacja w poniedziałki ok. 04:00 UTC. Kolejne okresy nachodzą na siebie (28-dniowe okna przesuwane o tydzień), więc różnica tydzień do tygodnia jest wygładzona. Źródło: https://developer.chrome.com/docs/crux/history-api

To jest właściwe narzędzie do pytania „czy CWV mentzen.pl poprawiły się po zmianie X", bo pokazuje trend, a nie punkt.

### Lighthouse CLI / Lighthouse CI / Unlighthouse
Trzy różne rzeczy:

- **`lighthouse`** (npm) — jednorazowy audyt jednego URL-a lokalnie, z własną instancją Chrome. Wymaga Node 22+. Źródło: https://github.com/GoogleChrome/lighthouse
- **`@lhci/cli`** (Lighthouse CI) — uruchamianie Lighthouse na commit/deploy, asercje progów i upload raportów. „your build system will be running Lighthouse on your project's URLs on every commit, automatically asserting that important Lighthouse audits pass, and uploading the reports for manual inspection". Źródło: https://github.com/GoogleChrome/lighthouse-ci/blob/main/docs/getting-started.md
- **Unlighthouse** — Lighthouse na *całej* witrynie równolegle, z auto-odkrywaniem URL-i z robots.txt, sitemap.xml i linków wewnętrznych. Sampling tras dynamicznych, dashboard, MIT, Node 22+. Źródło: https://unlighthouse.dev/

Dla mentzen.pl (setki artykułów) Unlighthouse jest bliżej potrzeby niż LHCI, bo problem to „które z 800 podstron mają słaby LCP", a nie „czy ten commit pogorszył wydajność".

### Walidacja schema.org
Tu jest zła wiadomość i trzeba ją powiedzieć wprost.

- **Rich Results Test (search.google.com/test/rich-results) nie ma publicznego API.** Nie znalazłem żadnej oficjalnej dokumentacji API dla tego narzędzia.
- **validator.schema.org też nie ma publicznego API.** Walidator wyciąga JSON-LD 1.0, RDFa 1.1 i Microdata oraz wskazuje błędy składni; jest oparty na dawnym Google Structured Data Testing Tool i dostarczany przez Google jako usługa dla społeczności schema.org. Źródła: https://schema.org/docs/validator.html oraz https://github.com/schemaorg/schemaorg/blob/main/docs/validator.md — żadne z nich nie opisuje endpointu API. Istnieje otwarta dyskusja „Public API or IFrame for validator.schema.org" (schemaorg/schemaorg#3261), co potwierdza, że API nie ma.

Wniosek dla architektury: walidację schema trzeba zrobić **lokalnie** (parsowanie JSON-LD + własne reguły), a Rich Results Test traktować jako ręczny krok weryfikacyjny dla człowieka. Istnieją komercyjne obejścia (aktory Apify, RapidAPI) scrapujące walidator Google — nie polecam ich w projekcie kancelarii: to zależność od cudzego scrapera na czyjejś infrastrukturze, z niepewną legalnością i trwałością.

### Crawlery
- **advertools** (Python, na Scrapy) — funkcja `crawl()`. Wyciąga automatycznie: title, meta description, viewport, charset, canonical, Open Graph, Twitter cards, JSON-LD, nagłówki h1–h6, treść, alt obrazków, linki z anchorami i `nofollow` (osobno nav/header/footer), kody HTTP, nagłówki request/response, łańcuchy przekierowań, rozmiar strony, latency, głębokość, IP, timestamp. Wyjście: jsonlines (`.jl`), dopisywane strumieniowo (nie trzyma wszystkiego w pamięci). Wartości wielokrotne łączone `@@`. Tryb discovery (`follow_links=True`, respektuje robots.txt) i tryb listy. Źródło: https://advertools.readthedocs.io/en/master/advertools.spider.html
  Poza `crawl()` biblioteka ma pobieranie i parsowanie robots.txt oraz sitemap do DataFrame'ów. Źródło: https://advertools.readthedocs.io/en/master/readme.html
- **Screaming Frog SEO Spider CLI** — `--headless`, `--crawl [URL]`, `--config [plik]`, `--export-tabs`, `--save-crawl`, `--bulk-export`, `--export-format` (CSV/Excel/Google Sheets), `--overwrite`, `--timestamped-output`, `--output-folder`. Na Linuksie start przez `screamingfrogseospider`. Źródło: https://www.screamingfrog.co.uk/seo-spider/user-guide/general/
  Tryb headless **wymaga licencji**; licencję wpisuje się w `~/ScreamingFrogSEOSpider/licence.txt` (login w pierwszej linii, klucz w drugiej) `[do weryfikacji — ścieżka pliku z materiałów społecznościowych, nie z oficjalnego user guide]`.
- Inne open-source wymieniane w 2026 (LibreCrawl, SiteOne Crawler, crawlie, Apache Nutch, Crawljax) — nie weryfikowałem ich jakości ani utrzymania; **przed użyciem czegokolwiek z tej listy zastosuj zasadę z CLAUDE.md o sprawdzaniu pakietu** `[do weryfikacji]`.

Rekomendacja: advertools jako baza. Jest to Scrapy z gotowym schematem ekstrakcji SEO, wynik ląduje w DataFrame, agent może na nim robić dowolne zapytania.

### IndexNow
Protokół pingowania wyszukiwarek o zmianie treści. Aktualna lista uczestników z https://www.indexnow.org/searchengines.json (pobrane 2026-08-28):

```
bing, yandex, seznam, naver, yep, internetarchive, amazonbot
```

**Google nie uczestniczy w IndexNow.** Dla polskiego B2B prawno-podatkowego oznacza to, że IndexNow wpływa na Bing/Copilot i (marginalnie) resztę, ale nie na główne źródło ruchu. Wartość rośnie o tyle, że Bing zasila odpowiedzi Copilota — czyli widoczność w asystentach AI.

Google ma osobne **Indexing API**, ale jest bezużyteczne dla bloga: „The Indexing API can only be used to crawl pages with either `JobPosting` or `BroadcastEvent` embedded in a `VideoObject`", domyślny limit 200 zapytań. Źródło: https://developers.google.com/search/apis/indexing-api/v3/quickstart. Agent nie powinien próbować go używać do artykułów.

### Bing Webmaster Tools API
Dane wyszukiwania i zarządzanie witryną w Bing programowo: `GetQueryStats`, `GetRankAndTrafficStats`, `GetUrlTrafficInfo`, `SubmitUrlBatch` i inne. Źródła: https://learn.microsoft.com/en-us/bingwebmaster/getting-started i https://learn.microsoft.com/en-us/bingwebmaster/api-protocols

To jedyne darmowe źródło danych o zapytaniach spoza Google Search Console. Przy tematyce prawno-podatkowej udział Binga w PL jest niski, ale dane bywają użyteczne jako kontrola krzyżowa dla GSC.

### WordPress REST API
Odczyt i zapis treści bez dostępu do wp-admin. Endpointy postów: `GET /wp/v2/posts`, `POST /wp/v2/posts`, `GET /wp/v2/posts/<id>`, `POST /wp/v2/posts/<id>`, `DELETE /wp/v2/posts/<id>`. Argumenty przy tworzeniu/aktualizacji: `title`, `content`, `status` (publish/draft/pending/future/private), `slug`, `author`, `excerpt`, `categories`, `tags`, `meta`, `featured_media`, `comment_status`, `ping_status`. Listowanie filtruje przez `search`, `author`, zakresy dat, `orderby`, taksonomie; `context` steruje zestawem pól. Źródło: https://developer.wordpress.org/rest-api/reference/posts/

**Pułapka Yoast:** `yoast_head_json` jest **tylko do odczytu**. POST na to pole nic nie zrobi (cicho zignoruje). Żeby agent mógł zapisywać tytuł SEO i meta description, trzeba w motywie/wtyczce zarejestrować odpowiednie meta kluczem `register_post_meta(..., show_in_rest => true)`, wtedy zapis idzie przez standardowe pole `meta` w `/wp/v2/posts/<id>`. Źródło: https://developer.yoast.com/customization/apis/rest-api/ + potwierdzenia w materiałach społecznościowych `[do weryfikacji — dokładne nazwy kluczy meta Yoast, np. `_yoast_wpseo_title`, `_yoast_wpseo_metadesc`, sprawdzić na instancji]`.

---

## 2) Dostęp i auth

| Narzędzie | Endpoint | Auth |
|---|---|---|
| PageSpeed Insights | `GET https://www.googleapis.com/pagespeedonline/v5/runPagespeed` | Klucz API w `key=` — opcjonalny, ale „a key is recommended for frequent, automated queries" |
| CrUX | `POST https://chromeuxreport.googleapis.com/v1/records:queryRecord?key=API_KEY` | Klucz API Google Cloud, obowiązkowy |
| CrUX History | `POST https://chromeuxreport.googleapis.com/v1/records:queryHistoryRecord?key=API_KEY` | jw. |
| Lighthouse / LHCI / Unlighthouse | lokalnie | brak (LHCI server opcjonalnie własny) |
| Walidacja schema | — | brak publicznego API |
| IndexNow | `GET https://<searchengine>/indexnow?url=...&key=...` lub `POST /indexnow` (JSON) | plik klucza na domenie |
| Bing Webmaster | `https://ssl.bing.com/webmaster/api.svc/json/<Metoda>?apikey=KLUCZ` | klucz API generowany w BWT (per użytkownik, nie per witryna) albo OAuth 2.0 |
| WordPress REST | `https://mentzen.pl/wp-json/wp/v2/...` | Application Password (Basic Auth RFC 7617 po HTTPS) |

Szczegóły wymagające precyzji:

**PSI, parametry zapytania:** `url` (wymagany), `key`, `category` z wartościami `accessibility` / `best-practices` / `performance` / `seo`, `locale`, `strategy` z wartościami `desktop` (domyślna) / `mobile`, `utm_campaign`, `utm_source`. Źródło: https://developers.google.com/speed/docs/insights/v5/reference/pagespeedapi/runpagespeed
Parametr `category` można podać wielokrotnie, żeby dostać kilka kategorii w jednym przebiegu.

**CrUX, ciało zapytania:** dokładnie jedno z `origin` albo `url`; opcjonalnie `formFactor` (`DESKTOP` / `PHONE` / `TABLET`) i `metrics` (tablica nazw; pominięcie zwraca wszystkie).

**IndexNow, klucz:** 8–128 znaków szesnastkowych (a–z, A–Z, 0–9, myślniki), plik tekstowy UTF-8. Dwa warianty hostowania: `https://mentzen.pl/{klucz}.txt` w katalogu głównym, albo dowolna lokalizacja na domenie wskazana parametrem `keyLocation` — w tym drugim wariancie klucz autoryzuje wyłącznie URL-e z katalogu pliku i podkatalogów.

**IndexNow, POST zbiorczy:**
```json
{
  "host": "mentzen.pl",
  "key": "twoj-klucz",
  "keyLocation": "https://mentzen.pl/twoj-klucz.txt",
  "urlList": ["https://mentzen.pl/a/", "https://mentzen.pl/b/"]
}
```
Kody odpowiedzi: `200` przyjęte, `202` przyjęte, klucz w trakcie weryfikacji, `400` zły format, `403` nieprawidłowy klucz lub brak pliku, `422` URL nie pasuje do hosta lub niezgodny schemat, `429` za dużo żądań. Źródło: https://www.indexnow.org/documentation

**Bing Webmaster, format błędu:** HTTP 400 z ciałem `{"ErrorCode":3,"Message":"InvalidApiKey"}`. Źródło: https://learn.microsoft.com/en-us/bingwebmaster/getting-started

**WordPress Application Passwords:** od WordPress 5.6, generowane w wp-admin → Users → Edit User. Format wywołania:
```
curl --user "USERNAME:APPLICATION_PASSWORD" https://mentzen.pl/wp-json/wp/v2/users?context=edit
```
Dokumentacja WP wskazuje Application Passwords jako preferowaną metodę dla żądań spoza WordPressa. Cookie auth działa tylko wewnątrz WP dla zalogowanego użytkownika (z nonce `wp_rest` w nagłówku `X-WP-Nonce`). Wtyczka Basic Authentication jest wyłącznie do dev/testów: „should only be used for development and testing i.e. not in a production environment". Źródło: https://developer.wordpress.org/rest-api/using-the-rest-api/authentication/

---

## 3) Limity i koszty

| Narzędzie | Limit | Koszt |
|---|---|---|
| PageSpeed Insights | 25 000 zapytań/dobę, ok. 240 zapytań/min `[do weryfikacji]` | 0 zł, brak płatnego tieru |
| CrUX + CrUX History | **150 zapytań/min na projekt Google Cloud** (oficjalne) | 0 zł, „offered without charge", limitu nie da się podnieść |
| Lighthouse / LHCI / Unlighthouse | CPU i czas lokalnie | 0 zł (MIT) |
| IndexNow | do **10 000 URL-i na jeden POST**; każda wyszukiwarka ma własne limity | 0 zł |
| Bing Webmaster API | `SubmitUrlBatch` maks. 500 URL-i na batch `[do weryfikacji]`; dzienny limit zgłoszeń URL zależny od witryny (widoczny w BWT) | 0 zł |
| Screaming Frog | free do 500 URL-i na crawl; headless wymaga licencji | £199/rok `[do weryfikacji ceny na 2026]` |
| advertools / Scrapy | brak | 0 zł (MIT) |
| WordPress REST | limity własnego serwera / Cloudflare | 0 zł |

Uwagi:

- **Limit CrUX 150 qpm jest twardy i dzielony między CrUX API i History API.** To jedyny limit, o który agent realnie się otrze przy skanie całej witryny. Przy 800 URL-ach i dwóch form factorach to ~1600 zapytań — trzeba throttlingu (np. 2 zapytania/s daje bezpieczny zapas).
- **Limit PSI nie jest podany w oficjalnej dokumentacji.** Wartość 25 000/dobę i 240/min pochodzi ze źródeł wtórnych (m.in. DebugBear, grupa dyskusyjna pagespeed-insights-discuss). Rzeczywisty limit jest widoczny w Google Cloud Console jako metryka „Queries per day" dla usługi `pagespeedonline.googleapis.com`. **Sprawdź tam po założeniu projektu.** `[do weryfikacji]`
- PSI jest wolne (kilka do kilkunastu sekund na URL), więc przy skanie całego bloga i tak wąskim gardłem jest czas, nie limit.
- Google planuje usunąć dane CrUX z odpowiedzi PSI. Nie buduj logiki na polu `loadingExperience` z PSI — bierz dane polowe z CrUX API. Źródło: https://developers.google.com/speed/docs/insights/v5/get-started

---

## 4) Biblioteki i przykłady integracji

### Python (uv)

**PSI i CrUX — bez biblioteki.** To zwykły REST, `httpx` w zupełności wystarcza. Zewnętrzna zależność tylko dodaje powierzchnię ryzyka.

```python
# PSI
import httpx
r = httpx.get(
    "https://www.googleapis.com/pagespeedonline/v5/runPagespeed",
    params=[("url", "https://mentzen.pl/"), ("strategy", "mobile"),
            ("category", "performance"), ("category", "seo"),
            ("key", KEY)],
    timeout=120,
)
score = r.json()["lighthouseResult"]["categories"]["performance"]["score"]

# CrUX History
h = httpx.post(
    "https://chromeuxreport.googleapis.com/v1/records:queryHistoryRecord",
    params={"key": KEY},
    json={"origin": "https://mentzen.pl", "formFactor": "PHONE",
          "metrics": ["largest_contentful_paint", "interaction_to_next_paint",
                      "cumulative_layout_shift"],
          "collectionPeriodCount": 40},
    timeout=60,
)
```

**advertools** — `pip install advertools` (u nas: `uv add advertools`):

```python
import advertools as adv
import pandas as pd

adv.crawl(
    url_list=["https://mentzen.pl/"],
    output_file="crawl.jl",
    follow_links=True,
    allowed_domains=["mentzen.pl"],
    custom_settings={
        "CONCURRENT_REQUESTS_PER_DOMAIN": 2,
        "DOWNLOAD_DELAY": 0.5,
        "USER_AGENT": "MentzenSEOBot/1.0 (+kontakt)",
        "CLOSESPIDER_PAGECOUNT": 2000,
    },
)
df = pd.read_json("crawl.jl", lines=True)

# sitemap i robots bez crawlu
sm = adv.sitemap_to_df("https://mentzen.pl/sitemap_index.xml")
```
Źródło: https://advertools.readthedocs.io/en/master/advertools.spider.html

**Bing Webmaster** — `bing-webmaster-tools` (PyPI, autor MERJ), wrapper na API. `[do weryfikacji — przed instalacją przejść procedurę sprawdzania pakietu z CLAUDE.md: właściciel, data wydania, pobrania, CVE]`. Alternatywa: `httpx` bezpośrednio, API jest proste.

**IndexNow** — nie ma potrzeby biblioteki, to jeden POST JSON.

**Walidacja schema** — parsowanie JSON-LD z `extruct` (Python, wyciąga JSON-LD, Microdata, RDFa, OpenGraph) i własne reguły dla typów istotnych dla kancelarii `[do weryfikacji — sprawdzić utrzymanie extruct]`. Alternatywnie advertools już wyciąga JSON-LD w kolumnach crawlu, więc walidacja może być czystą warstwą reguł na DataFrame.

### Node (pnpm)

- `lighthouse` — CLI i moduł Node:
  ```bash
  pnpm add -g lighthouse
  lighthouse https://mentzen.pl/ --output json --output-path ./report.json --chrome-flags="--headless"
  ```
  Źródło: https://github.com/GoogleChrome/lighthouse
- `@lhci/cli` — `npm install -g @lhci/cli@0.15.x`, potem `lhci autorun`. Konfiguracja w `lighthouserc.js`:
  ```javascript
  module.exports = {
    ci: {
      upload: { target: 'temporary-public-storage' },
    },
  };
  ```
  Asercje startują od presetu `'lighthouse:recommended'`. Alternatywa dla `temporary-public-storage` (publiczne, kilkudniowe) to własny LHCI server — u nas na `127.0.0.1` zgodnie z zasadami sieciowymi. Źródło: https://github.com/GoogleChrome/lighthouse-ci/blob/main/docs/getting-started.md
- `unlighthouse`:
  ```bash
  npx unlighthouse --site mentzen.pl
  ```
  Dashboard domyślnie na `localhost:5678`. Źródło: https://unlighthouse.dev/
- `psi` (GoogleChromeLabs) — wrapper na PSI v5 z raportowaniem, opcje `--key`, `--strategy`, `--format`, `--locale`, `--threshold`. Źródło: https://github.com/GoogleChromeLabs/psi. Przy prostocie PSI raczej zbędny.

### WordPress

```bash
# odczyt: publiczny
curl -s "https://mentzen.pl/wp-json/wp/v2/posts?per_page=100&page=1&_fields=id,slug,link,title,modified,yoast_head_json"

# zapis: Application Password
curl -X POST "https://mentzen.pl/wp-json/wp/v2/posts/123" \
  --user "user:xxxx xxxx xxxx xxxx xxxx xxxx" \
  -H "Content-Type: application/json" \
  -d '{"status":"draft","meta":{"_yoast_wpseo_metadesc":"..."}}'
```
Zapis do `meta` zadziała dopiero po zarejestrowaniu kluczy przez `register_post_meta(..., 'show_in_rest' => true)` po stronie WordPressa.

Alternatywa dla REST: **WP-CLI** przez SSH. Daje pełny dostęp (`wp post list`, `wp post meta update`, `wp rewrite`), omija problem read-only Yoast, ale wymaga shella na produkcji. Rozsądny podział: REST do odczytu i szkiców, WP-CLI do operacji masowych na lokalnej kopii `[do weryfikacji — czy hosting daje SSH]`.

---

## 5) Zastosowanie dla agenta SEO — konkretne workflow

### W1. Nocny audyt techniczny całej witryny
1. `adv.sitemap_to_df("https://mentzen.pl/sitemap_index.xml")` → lista kanonicznych URL-i z `lastmod`.
2. `adv.crawl(follow_links=True)` → pełny obraz: statusy, canonicale, title/meta description, h1, linkowanie wewnętrzne, JSON-LD, łańcuchy przekierowań.
3. Diff względem wczorajszego `.jl`: nowe 404, nowe `noindex`, zniknięte canonicale, tytuły dłuższe niż próg, duplikaty title/meta description, strony osierocone (w sitemapie, bez linków wewnętrznych).
4. Raport tylko z deltami. Agent nie powinien co noc powtarzać tej samej listy 200 znanych problemów.

### W2. Core Web Vitals: pole vs laboratorium
1. CrUX API na origin + na 20 najważniejszych szablonów (strona główna, kategoria, artykuł, strona usługi, kontakt) w `PHONE` i `DESKTOP`.
2. Dla URL-i z p75 LCP powyżej progu — PSI z `category=performance`, żeby dostać konkretne audyty (`render-blocking-resources`, `unused-css-rules`, `uses-responsive-images`).
3. CrUX History co poniedziałek na origin → wykres 40 tygodni, sprawdzenie czy zmiany na stronie faktycznie ruszyły metrykę.
4. Ważne rozróżnienie w raporcie: CrUX to średnia z 28 dni, więc efekt wdrożenia widać dopiero po ok. 4 tygodniach pełnego okna. Agent nie powinien ogłaszać sukcesu po trzech dniach.

**Uwaga o Divi:** motyw Divi jest znany z ciężkiego CSS/JS. Jeśli LCP jest problemem, to prawdopodobnie problem szablonu, nie pojedynczych artykułów — agent powinien grupować wyniki po typie strony, nie raportować 800 osobnych ustaleń.

### W3. Walidacja danych strukturalnych bez API Google
1. Z crawlu advertools wyciągnij kolumnę JSON-LD dla wszystkich stron.
2. Lokalne reguły: czy artykuły mają `Article`/`BlogPosting` z `author`, `datePublished`, `dateModified`, `headline`; czy strony usług mają `LegalService` / `Organization`; czy `FAQPage` nie jest wstawiony tam, gdzie nie ma FAQ; czy `@id` są spójne między encjami.
3. Wykryte problemy → lista URL-i do ręcznego sprawdzenia w Rich Results Test przez człowieka. **Agent nie może zastąpić tego kroku**, bo Google nie daje API.
4. Dla branży prawno-podatkowej istotne są `Person` z `jobTitle` dla autorów (E-E-A-T) i `sameAs` do profili zawodowych.

### W4. Zgłaszanie zmian do IndexNow po publikacji
1. Poll `GET /wp-json/wp/v2/posts?orderby=modified&order=desc&per_page=20` co godzinę (albo hook po stronie WP).
2. Nowe/zmienione URL-e → POST na `https://api.indexnow.org/indexnow` albo bezpośrednio na `https://www.bing.com/indexnow`.
3. Logowanie kodów odpowiedzi: `202` to normalne, `403` oznacza problem z plikiem klucza, `429` wymaga backoffu.
4. **Nie obiecuj efektu w Google.** Google nie jest w IndexNow. To działanie pod Bing/Copilot.

Prostsza alternatywa: oficjalna wtyczka **IndexNow** od Microsoftu (wordpress.org/plugins/indexnow) generuje i hostuje klucz sama oraz zgłasza URL-e przy publikacji/aktualizacji/usunięciu. Jeśli celem jest tylko ping, wtyczka jest tańsza w utrzymaniu niż własny poller. Własne narzędzie ma sens, gdy agent ma zgłaszać selektywnie (np. tylko po istotnej zmianie treści, nie po poprawce literówki).

### W5. Bing jako druga para oczu na zapytania
`GetQueryStats` i `GetRankAndTrafficStats` dla mentzen.pl → zestawienie z danymi GSC. Rozbieżności w frazach bywają sygnałem: fraza mocna w Bingu, słaba w Google, przy podobnej intencji, sugeruje problem z konkurencją, nie z treścią. Realna wartość zależy od wolumenu — przy niskim udziale Binga w PL dane mogą być zbyt rzadkie. Zweryfikować po pierwszym pobraniu.

### W6. Agent jako edytor treści przez REST
- **Odczyt:** pełna baza artykułów z `yoast_head_json` (tytuł SEO, meta description, canonical, OG, schema Yoasta) bez autoryzacji. To wystarcza do audytu treści i planowania.
- **Zapis:** wyłącznie jako `status: "draft"` albo do pola `meta`, nigdy bezpośrednia publikacja. Kancelaria publikuje treści o skutkach prawnych; człowiek musi zaakceptować.
- **Rozsądny zakres zapisu dla agenta:** meta description, tytuł SEO, slug (ostrożnie — zmiana slugu wymaga przekierowania), linkowanie wewnętrzne w treści.
- **Poza zakresem:** merytoryka podatkowa. Agent proponuje, prawnik zatwierdza.
- Namespace `redirection/v1` sugeruje wtyczkę Redirection, która ma własne REST API do zarządzania przekierowaniami. Gdyby agent zmieniał slugi, tędy mógłby dodawać 301. `[do weryfikacji — dokładne endpointy redirection/v1, nie zdążyłem sprawdzić dokumentacji]`

### W7. Zadanie na start: robots.txt
`https://mentzen.pl/robots.txt` nie zawiera ani jednej dyrektywy — tylko komentarze content-signals. Brakuje przede wszystkim linii:
```
Sitemap: https://mentzen.pl/sitemap_index.xml
```
To nie jest krytyczne (Google i tak zna sitemapę z GSC), ale jest darmowe i porządkuje sygnały dla crawlerów spoza Google, w tym botów AI. Przy okazji warto zdecydować, czy content-signals mają zostać w obecnej formie, skoro nie towarzyszą im żadne reguły `User-agent`.

---

## 6) Wymagana konfiguracja po stronie użytkownika

### Google Cloud (jeden projekt na wszystko)
1. Utwórz projekt w https://console.cloud.google.com/
2. Włącz **PageSpeed Insights API** (`pagespeedonline.googleapis.com`).
3. Włącz **Chrome UX Report API** (`chromeuxreport.googleapis.com`).
4. Wygeneruj klucz API (Credentials). Ten sam klucz obsłuży PSI i CrUX.
5. **Ogranicz klucz** do tych dwóch API (restrykcja API). Ograniczenie po adresie IP nie ma tu sensu przy laptopie w sieci firmowej.
6. Sprawdź w Cloud Console → IAM & Admin → Quotas realny limit „Queries per day" dla `pagespeedonline.googleapis.com` i zapisz go w notatce projektu.

### Bing Webmaster Tools
1. Zweryfikuj mentzen.pl w https://www.bing.com/webmasters (jeśli nie jest).
2. Wygeneruj klucz API w ustawieniach BWT. Klucz jest **per użytkownik**, nie per witryna — obsłuży wszystkie zweryfikowane domeny.
3. Zdecyduj: klucz API czy OAuth 2.0. Do skryptu lokalnego klucz jest prostszy.

### IndexNow
Dwie ścieżki, wybierz jedną:
- **Wtyczka:** zainstaluj oficjalną wtyczkę IndexNow od Microsoftu. Klucz generuje i hostuje sama. Zero pracy dla agenta.
- **Własne narzędzie:** wygeneruj klucz (8–128 znaków hex), umieść plik `{klucz}.txt` w katalogu głównym mentzen.pl (zawartość pliku = sam klucz), sprawdź że `https://mentzen.pl/{klucz}.txt` zwraca 200 i `text/plain`. Uwaga na Cloudflare — może wymagać reguły, żeby nie serwować pliku przez cache z złym Content-Type.

### WordPress
1. Utwórz **osobne konto** dla agenta w mentzen.pl (nie używaj konta administratora).
2. Nadaj mu minimalną rolę wystarczającą do zadań — Editor jeśli ma edytować szkice, Author jeśli tylko własne, Subscriber + capability read jeśli tylko odczyt. Rola Administrator nie jest potrzebna.
3. Wygeneruj **Application Password** dla tego konta (Users → Edit User → Application Passwords). Zapisz w menedżerze haseł, nie w repo.
4. Sprawdź, czy Wordfence Login Security nie blokuje Application Passwords przy włączonym 2FA.
5. Jeśli agent ma zapisywać meta Yoasta: dodaj snippet PHP z `register_post_meta` i `show_in_rest => true` dla kluczy Yoasta. To zmiana w kodzie motywu potomnego lub własnej wtyczce — do wykonania przez człowieka.
6. Rozważ testy na lokalnej kopii (docker compose w `local-mentzen`, localhost:8080) zanim cokolwiek pójdzie na produkcję. Uwaga: Application Passwords domyślnie wymagają HTTPS, na `http://localhost:8080` trzeba filtra `wp_is_application_passwords_available` `[do weryfikacji]`.

### Screaming Frog (opcjonalnie)
Potrzebny tylko jeśli chcesz tryb headless. Licencja £199/rok `[do weryfikacji ceny]`, klucz w `~/ScreamingFrogSEOSpider/licence.txt`. **Zanim kupisz — sprawdź, czy advertools nie wystarczy.** Dla agenta pracującego na danych w DataFrame advertools jest wygodniejszy, a różnica to głównie renderowanie JS i gotowe integracje z GSC/GA.

### Środowisko lokalne
- Serwery deweloperskie (LHCI server, dashboard Unlighthouse na 5678) wiąż na `127.0.0.1` zgodnie z zasadami z CLAUDE.md.
- Node 22+ dla Lighthouse i Unlighthouse.
- Python przez `uv`, Node przez `pnpm`.
- Każdą nową zależność (advertools, extruct, bing-webmaster-tools) przepuść przez procedurę sprawdzania pakietu opisaną w CLAUDE.md.

### Sekrety
Do trzymania: klucz Google Cloud, klucz Bing Webmaster, klucz IndexNow, Application Password WordPressa. Cztery sekrety, jeden `.env` poza repo, `.gitignore` sprawdzony przed pierwszym commitem.

---

## Czego tu nie ma (luki do domknięcia)

- **Rich Results Test bez API** to realne ograniczenie. Jeśli walidacja schema ma być zautomatyzowana end-to-end, jedyna uczciwa odpowiedź brzmi: nie da się oficjalnie. Do rozważenia: raport Search Console o wzbogaconych wynikach (Enhancement reports) przez GSC API jako namiastka — pokazuje błędy schema wykryte przez Google, choć z opóźnieniem i tylko dla stron zaindeksowanych. To obszar innego researchu (GSC).
- Endpointy `redirection/v1` — niesprawdzone.
- Dokładne nazwy kluczy meta Yoasta w tej instalacji — do sprawdzenia zapytaniem do bazy lub `wp post meta list`.
- Realny udział Binga w ruchu mentzen.pl — bez tego trudno ocenić, czy Bing Webmaster API i IndexNow to priorytet, czy dodatek.
