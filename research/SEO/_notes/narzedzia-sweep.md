# Sweep uzupełniający: czego agent SEO potrzebuje poza GSC / GA4 / Trends / keyword API / PSI

Data researchu: 2026-08-28. Kontekst: jeden serwis (mentzen.pl, WordPress), rynek polski, branża prawo/podatki/księgowość B2B, mała skala, narzędzia jako lokalne CLI/API.

Obszary objęte tym sweepem: SERP API, rank tracking, widoczność w AI (AI Overviews / AI Mode / ChatGPT), backlinki, monitoring wzmianek o marce.

Konwencja: fakty mają URL źródła. `[do weryfikacji]` oznacza dane pochodzące z opracowań firm trzecich albo z materiałów marketingowych, których nie potwierdziłem w dokumentacji producenta.

---

## 1. Co daje

### 1.1 Luka względem narzędzi już pokrytych

GSC, GA4, Trends, keyword API i PageSpeed Insights nie odpowiadają na pięć pytań:

| Pytanie | Czego brakuje w GSC/GA4 |
| --- | --- |
| Jak faktycznie wygląda SERP dla mojej frazy? | GSC podaje uśrednioną pozycję własnej strony, nie pokazuje kto jest wyżej, jakie są featured snippety, PAA, reklamy, ani jak wygląda strona wyników |
| Jak zmienia się pozycja na konkretnej frazie dzień po dniu? | GSC uśrednia pozycję po wyświetleniach i miesza urządzenia/lokalizacje; nie nadaje się do śledzenia pojedynczej frazy |
| Czy marka jest cytowana w ChatGPT / Perplexity / Gemini? | GSC pokrywa wyłącznie powierzchnie Google |
| Kto do mnie linkuje i co się zmieniło? | GSC ma raport linków, ale bez metryk jakości, bez historii i z ograniczonym eksportem |
| Kto pisze o marce poza wynikami wyszukiwania? | brak pokrycia |

### 1.2 SERP API (podgląd polskich wyników)

Zwraca ustrukturyzowany JSON z wynikami Google dla zadanej frazy, lokalizacji i języka: wyniki organiczne z pozycjami, reklamy, featured snippet, People Also Ask, local pack, oraz — w części dostawców — blok AI Overview. To jest fundament, na którym można samodzielnie zbudować rank tracking, analizę konkurencji w SERP i wykrywanie zmian w wyglądzie wyników.

Trzej realni kandydaci: DataForSEO, Serper, SerpApi.

### 1.3 Rank tracking

Nie ma potrzeby kupować osobnego narzędzia. Rank tracking to pętla: lista fraz → SERP API → zapis pozycji do bazy → diff względem poprzedniego pomiaru. Cała wartość leży w liście fraz i w tym, co agent zrobi ze zmianą, a nie w samym pobieraniu.

### 1.4 Widoczność w AI

Dwa rozłączne źródła:

- **Google (AI Overviews, AI Mode)** — od 2026 r. Search Console ma dedykowany raport „Generative AI performance". Obejmuje AI Overviews i AI Mode; wyłączone są eksperymenty z Search Labs. Metryka to wyświetlenia; kliknięcia nie są wymienione wśród śledzonych metryk. Wymiary: strony, kraje, daty, urządzenia. Raport jest udostępniany etapami, nie wszystkie właściwości go mają. Obowiązuje limit 1000 wierszy, a najnowsze dane są wstępne. Źródło: https://support.google.com/webmasters/answer/16984139 . Osobny raport istnieje dla Discover: https://support.google.com/webmasters/answer/16983858 . Jest też przełącznik pozwalający wyłączyć treść z funkcji AI: https://support.google.com/webmasters/answer/16908024
  - Ogłoszenie: https://developers.google.com/search/blog/2026/06/gen-ai-performance-reports (nie udało mi się pobrać treści posta; opracowania firm trzecich podają start 3 czerwca 2026, dane od 18 maja 2026, bez uzupełnienia historii, początkowo dla podzbioru witryn z Wielkiej Brytanii — `[do weryfikacji]`, np. https://www.pragma-code.de/en/blog-search-console-generative-ai-performance)
  - Czy raport jest dostępny przez Search Console API — strona pomocy o tym nie wspomina. `[do weryfikacji]`, do sprawdzenia empirycznie wymiarem `searchAppearance` (dokumentacja wymiarów: https://developers.google.com/webmaster-tools/v1/how-tos/all-your-data)
- **Modele poza Google (ChatGPT, Claude, Gemini, Perplexity)** — nikt nie udostępnia własnych danych o cytowaniach. Jedyna droga to odpytywanie modeli zestawem promptów i liczenie, czy marka/domena pada w odpowiedzi. Można to zrobić samodzielnie albo kupić gotowe przez DataForSEO AI Optimization API.

### 1.5 Backlinki

Google Search Console pokazuje linki przychodzące, ale bez metryk autorytetu, bez dat pierwszego wykrycia i bez wygodnego diffowania. Zewnętrzne indeksy linków (DataForSEO, Ahrefs, Moz) dodają metryki domeny, anchory, historię pojawiania się i znikania linków oraz porównanie z konkurencją.

Darmowe uzupełnienie: Bing Webmaster Tools API zwraca dane o linkach dla zweryfikowanej witryny.

### 1.6 Monitoring wzmianek o marce

Wykrywanie, gdzie w polskim internecie pojawia się „Mentzen" poza własnym serwisem: media branżowe, fora, LinkedIn, komentarze. Wartość dla SEO jest pośrednia (okazje linkbuildingowe, sygnały do E-E-A-T, wykrywanie treści cytowanych przez modele), ale przy marce osobowej silnie związanej z osobą publiczną to również obszar reputacyjny, czyli decyzja biznesowa użytkownika, nie tylko techniczna.

---

## 2. Dostęp i auth

### 2.1 DataForSEO

Rekomendowany jako pojedyncze konto pokrywające cztery z pięciu obszarów: SERP, dane o rankingach domeny (Labs), backlinki i widoczność w LLM.

- Auth: HTTP Basic (login + hasło z panelu), poświadczenia z https://app.dataforseo.com/api-access
- Model: pay-as-you-go, bez abonamentu. https://dataforseo.com/apis/serp-api/pricing
- Sandbox do rozwoju bez zużywania środków. https://dataforseo.com/ai-optimization-api
- Silniki: 7 (Google, Bing, YouTube, Yahoo, Baidu, Naver, Seznam) plus warianty Google (Maps, News, Images). Google Organic używa Google Geographical Targeting, czyli wspierane są wszystkie lokalizacje Google — Polska i język polski mieszczą się w tym zakresie. Kody pobiera się z `/v3/serp/google/locations` i `/v3/serp/google/languages`. https://dataforseo.com/apis/serp-api oraz https://docs.dataforseo.com/v3/serp/google/organic/live/advanced/
- Endpoint `/v3/serp/google/organic/live/advanced` przyjmuje `keyword` (do 700 znaków), `location_code`/`location_name`, `language_code`/`language_name`, `device` (desktop|mobile), `depth` (domyślnie 10, maks. 200). Wśród typów elementów odpowiedzi jest `ai_overview`. https://docs.dataforseo.com/v3/serp/google/organic/live/advanced/
- Istnieje osobny endpoint Google AI Mode. https://dataforseo.com/apis/serp-api
- AI Optimization API dzieli się na cztery grupy. https://docs.dataforseo.com/v3/ai_optimization-overview/
  - **LLM Responses** — odpowiedzi ChatGPT, Claude, Gemini, Perplexity w czasie rzeczywistym; każda platforma ma własny endpoint `Models` do wyboru wersji modelu; tryby Standard i Live
  - **LLM Scraper** — wyniki wyszukiwania ChatGPT i Gemini; tryby Standard i Live
  - **AI Keyword Data** — wolumeny i intencje wywiedzione z interakcji z narzędziami AI; wyłącznie tryb Live; wiele lokalizacji i języków
  - **LLM Mentions** — wzmianki o domenie, stronie i marce w modelach; metryki: liczba wzmianek w czatach, źródła, AI search volume; wyłącznie tryb Live; do 2000 wywołań na minutę i maks. 30 równoległych żądań. https://docs.dataforseo.com/v3/ai_optimization-llm_mentions-overview/
- Backlinks API: wszystkie endpointy obsługują tryb Live, średnio do 2 sekund. https://dataforseo.com/pricing/backlinks/backlinks
- Oficjalny serwer MCP: `npx dataforseo-mcp-server@latest`, auth przez OAuth 2.0 (tryb HTTP) albo zmienne `DATAFORSEO_LOGIN` / `DATAFORSEO_PASSWORD` (tryb stdio/CLI). https://github.com/dataforseo/mcp-server-typescript

### 2.2 Serper

- Auth: klucz API w nagłówku
- 2500 darmowych zapytań bez karty. https://serper.dev/
- Endpointy: search, images, news, maps, videos, shopping, scholar, patents, autocomplete. Parametry kraju i języka (`gl`, `hl`). https://serper.dev/
- Deklarowana latencja 1–2 s. https://serper.dev/
- Uwaga: `docs.serper.dev` nie rozwiązuje się w DNS (stan na 2026-08-28), a według opracowań trzecich publiczna strona `serper.dev/pricing` zwraca 404 i cennik jest za rejestracją. `[do weryfikacji]` — dokumentację trzeba obejrzeć po założeniu konta. https://apiserpent.com/blog/serper-pricing-credits-explained

### 2.3 SerpApi

- Auth: klucz API
- Plany miesięczne z gwarantowaną przepustowością godzinową. https://serpapi.com/pricing
- Oferuje „U.S. Legal Shield" do 2 mln USD pokrycia dla scrapowania i parsowania danych wyszukiwarek, warunkowane zgodnością użycia z prawem. https://serpapi.com/pricing
- „ZeroTrace Mode" na wyższych planach: brak przechowywania parametrów zapytań, zapytań i wyników po stronie SerpApi. https://serpapi.com/pricing

### 2.4 Google Custom Search JSON API — odpada

Wygląda na oczywisty oficjalny wybór, ale nie jest dostępny dla nowych projektów: cennik dotyczy wyłącznie istniejących klientów, a usługa zostaje wyłączona 1 stycznia 2027. Nie budować na tym niczego. https://developers.google.com/custom-search/v1/overview

### 2.5 Bing Webmaster Tools API

- Auth: `apikey` jako parametr zapytania; REST pod `https://ssl.bing.com/webmaster/api.svc/json/...`
- `GetLinkCounts(siteUrl, page)` zwraca listę stron witryny mających linki przychodzące wraz z licznikami, stronicowaną (`TotalPages`). Istnieje też `GetUrlLinks`. https://learn.microsoft.com/en-us/dotnet/api/microsoft.bing.webmaster.api.interfaces.iwebmasterapi.getlinkcounts
- Znany problem: zgłoszenia, że `GetLinkCounts` i `GetUrlLinks` zwracają HTTP 200 z pustym wynikiem dla zweryfikowanej witryny. https://learn.microsoft.com/en-us/answers/questions/5939109/bing-webmaster-tools-api-getlinkcounts-and-geturll — traktować jako opcję do przetestowania, nie jako pewne źródło

### 2.6 Ahrefs API v3

- Dostępne dla subskrybentów, klucz API z panelu
- Koszt żądania: `base_cost` = 50 jednostek minimum, plus `per_row_cost` = suma kosztów każdego unikalnego pola występującego w wyniku albo w parametrach `where` / `order_by`. https://docs.ahrefs.com/en/api/docs/limits-consumption
- Liczba jednostek na plan i limit wierszy na żądanie rosną wraz z poziomem planu; dokumentacja odsyła po konkrety do cennika. `https://ahrefs.com/api/pricing` przekierowuje na hub dokumentacji https://docs.ahrefs.com/ , który konkretnych liczb nie podaje
- Ahrefs udostępnia też MCP, Data Studio, Bot Analytics i Ahrefs Connect. https://docs.ahrefs.com/

### 2.7 Moz Links API

Darmowy próg i cennik oparty na liczbie zwróconych wierszy. Konkretów nie potwierdziłem w dokumentacji Moz — patrz sekcja 3. `[do weryfikacji]`

### 2.8 Monitoring wzmianek

- **Brand24** (polski dostawca) — nie udało się pobrać strony API (`https://brand24.com/api/` zwraca 404). Istnienie, zakres i cennik API `[do weryfikacji]` bezpośrednio u dostawcy
- **Senuto** (polski dostawca, dane widoczności dla rynku PL) — według opracowań trzecich API jest dostępne w planach Business/Enterprise, a dokumentacja po kontakcie z działem sprzedaży. `[do weryfikacji]`. https://mateuszkozlowski.pl/blog/skuteczne-pozycjonowanie-stron-w-google-przy-uzyciu-senuto-api/
- **Wariant DIY** — endpointy `news` i `search` w SERP API (Serper ma dedykowany `news`), odpytywane cyklicznie zapytaniem markowym z `gl=pl`, `hl=pl`. To pokrywa część indeksowaną przez Google, nie pokrywa mediów społecznościowych

---

## 3. Limity i koszty

### 3.1 SERP API — porównanie kosztu

| Dostawca | Model | Koszt | Uwagi |
| --- | --- | --- | --- |
| DataForSEO Standard | pay-as-you-go | $0.0006 / SERP | kolejka, wynik ok. 5 min |
| DataForSEO Priority | pay-as-you-go | $0.0012 / SERP | ok. 1 min |
| DataForSEO Live | pay-as-you-go | $0.002 / SERP | ok. 6 s |
| Serper | prepaid credits | ok. $1.00 → $0.30 / 1000 zapytań zależnie od wolumenu `[do weryfikacji]` | 2500 zapytań gratis; 1 kredyt do 10 wyników, 2 kredyty dla 11–100 `[do weryfikacji]` |
| SerpApi Free | abonament | $0 / 250 zapytań mies. | 50 zapytań/h |
| SerpApi Starter | abonament | $25 / 1000 zapytań mies. | 200 zapytań/h |
| SerpApi Developer | abonament | $75 / 5000 zapytań mies. | 1000 zapytań/h |

Źródła: https://dataforseo.com/apis/serp-api/pricing , https://serpapi.com/pricing , https://serper.dev/ , https://apiserpent.com/blog/serper-pricing-credits-explained

DataForSEO: cena obejmuje SERP do 10 wyników, głębsze zapytania kosztują więcej; $1 kredytu przy rejestracji, minimalna wpłata $50, brak abonamentu. https://dataforseo.com/apis/serp-api/pricing

SerpApi: niewykorzystane zapytania nie przechodzą na kolejny miesiąc `[do weryfikacji]`. https://apiserpent.com/blog/serpapi-pricing-explained

### 3.2 Rachunek dla jednego serwisu

Założenie: 200 monitorowanych fraz.

| Scenariusz | SERP-ów mies. | DataForSEO Standard | Serper (~$1/1000) | SerpApi |
| --- | --- | --- | --- | --- |
| Pomiar tygodniowy | 800 | ok. $0.48 | ok. $0.80 | plan $25 |
| Pomiar dzienny | 6000 | ok. $3.60 | ok. $6.00 | plan $75 |
| Dzienny, 500 fraz | 15000 | ok. $9.00 | ok. $15.00 | plan $150 |

Wniosek: przy jednym serwisie koszt samego pobierania SERP jest pomijalny u każdego dostawcy pay-as-you-go. Abonamenty SerpApi mają sens dopiero, gdy potrzebna jest gwarantowana przepustowość albo osłona prawna, nie z powodu ceny za zapytanie. Minimalna wpłata $50 w DataForSEO wystarczy na wiele miesięcy pracy takiego agenta.

### 3.3 DataForSEO Labs (alternatywa dla rank trackera)

- Większość endpointów, w tym `ranked_keywords`, `keywords_for_site`, `domain_rank_overview`, `competitors_domain`: $0.012 za zadanie + $0.00012 za element
- Historical SERPs: $0.00012 za SERP
- Historical Rank / Historical Bulk Traffic Estimation: $0.12 za zadanie + $0.0012 za element
- Search Intent: $0.012 za zadanie + $0.00012 za frazę
- Tryb Live do 2 s

https://dataforseo.com/pricing/dataforseo-labs/dataforseo-google-api

`ranked_keywords` dla domeny w Polsce zwraca gotową listę fraz, na które domena rankuje, z pozycjami — jedno zapytanie zamiast setek pojedynczych SERP-ów. To najtańszy sposób na przegląd „gdzie w ogóle jesteśmy", zanim zdefiniuje się listę fraz do dokładnego trackingu.

### 3.4 Backlinki

| Dostawca | Koszt |
| --- | --- |
| DataForSEO Backlinks | $0.024 za zadanie + $0.000036 za wiersz (czyli $0.06 za 1000 wierszy), maks. 1000 wierszy na żądanie, Live do ok. 2 s |
| Ahrefs API v3 | min. 50 jednostek na żądanie + koszt na wiersz razy liczba pól; pula jednostek zależna od planu |
| Moz Links API | rozliczenie za wiersze; darmowy próg 50 wierszy mies. przy limicie 1 żądanie / 10 s `[do weryfikacji]`; płatne od $20 mies. za 3000 wierszy `[do weryfikacji]` |
| Bing Webmaster API | bezpłatne dla zweryfikowanej witryny |

Źródła: https://dataforseo.com/pricing/backlinks/backlinks , https://docs.ahrefs.com/en/api/docs/limits-consumption , https://busyless.space/seo-apis/moz `[do weryfikacji]` , https://learn.microsoft.com/en-us/dotnet/api/microsoft.bing.webmaster.api.interfaces.iwebmasterapi.getlinkcounts

Wielkość i częstotliwość odświeżania indeksu linków DataForSEO nie są podane na stronie cennika. `[do weryfikacji]`

Pełny profil linków mentzen.pl to prawdopodobnie rząd tysięcy do dziesiątek tysięcy wierszy. Miesięczny pełny zrzut z DataForSEO przy 20 000 wierszy to ok. $1.20 plus koszt zadań. Bez znaczenia budżetowego.

### 3.5 Widoczność w AI

| Endpoint | Koszt |
| --- | --- |
| DataForSEO LLM Responses, Live | $0.0006 za zadanie + opłata pobierana przez dane LLM API; do 120 s |
| DataForSEO LLM Responses, Standard | $0.0002 za zadanie + $0.01 przedpłaty zwracanej, jeśli faktyczny koszt LLM był niższy; do 72 h |
| DataForSEO LLM Scraper, Standard | $0.0012 za stronę wyników, do 45 min |
| DataForSEO LLM Scraper, Priority | $0.0024 za stronę wyników, do 5 min |
| DataForSEO LLM Scraper, Live | $0.004 za stronę wyników, ok. 90 s |
| DataForSEO SERP AI Summary | $0.01 za zadanie |
| DataForSEO SERP Screenshot | $0.004 za obraz |
| Google Search Console, raport Generative AI | bezpłatne |

Źródła: https://dataforseo.com/pricing/ai-optimization/llm-responses , https://dataforseo.com/pricing/ai-optimization/llm-scraper , https://dataforseo.com/apis/serp-api/pricing , https://support.google.com/webmasters/answer/16984139

Gotowe platformy śledzenia widoczności w AI (Otterly.AI, Peec AI, Profound): ceny podawane przez porównania firm trzecich to odpowiednio ok. $29/mies., od 89 EUR/mies. i $499/mies. `[do weryfikacji]`. Nie sprawdziłem, czy którakolwiek udostępnia API nadające się do integracji z lokalnym agentem. https://www.surmado.com/blog/best-ai-visibility-tools-2026

Rachunek dla wariantu własnego: 40 promptów markowych × 4 platformy × 1 raz w tygodniu = 640 zadań miesięcznie. Przez LLM Responses w trybie Standard to ok. $0.13 opłaty bazowej plus faktyczne koszty tokenów u dostawców modeli. Nawet z narzutem tokenowym mieści się to grubo poniżej najtańszego abonamentu SaaS.

### 3.6 Kwestia prawna i ryzyko

Automatyczne pobieranie wyników Google jest niezgodne z warunkami korzystania z usług Google, niezależnie od tego, czy robi to własny skrypt, czy dostawca API. Korzystanie z zewnętrznego SERP API przenosi warstwę operacyjną na dostawcę; SerpApi dodatkowo reklamuje osłonę prawną do 2 mln USD dla scrapowania i parsowania danych wyszukiwarek, warunkowaną zgodnością użycia z prawem (https://serpapi.com/pricing). Nie jest to porada prawna i nie sprawdzałem, jak ta osłona ma się do prawa polskiego ani do sytuacji podmiotu spoza USA — `[do weryfikacji]`, a przy kancelarii to akurat pytanie, na które użytkownik odpowie sobie lepiej niż ja.

Drugi wątek: wysyłanie promptów markowych do API modeli oznacza wysyłanie nazwy klienta i kontekstu do zewnętrznego dostawcy. Same prompty markowe („co wiesz o kancelarii X") nie zawierają danych klientów kancelarii, więc mieszczą się w regule z globalnego CLAUDE.md, ale warto to trzymać jako świadomą granicę: nie wolno wrzucać do tych promptów treści spraw ani danych klientów.

---

## 4. Biblioteki i przykłady integracji

### 4.1 DataForSEO

Dokumentacja nie wskazuje oficjalnych SDK dla Pythona ani Node (https://dataforseo.com/apis/serp-api). API jest zwykłym REST-em z Basic Auth, więc `httpx` w Pythonie albo `undici`/`fetch` w Node wystarczą. Wzór żądania (Python, `uv`):

```python
# uv add httpx
import base64, httpx

AUTH = base64.b64encode(f"{LOGIN}:{PASSWORD}".encode()).decode()

r = httpx.post(
    "https://api.dataforseo.com/v3/serp/google/organic/live/advanced",
    headers={"Authorization": f"Basic {AUTH}", "Content-Type": "application/json"},
    json=[{
        "keyword": "biuro rachunkowe dla spółek",
        "location_name": "Poland",
        "language_code": "pl",
        "device": "desktop",
        "depth": 20,
    }],
    timeout=60,
)
items = r.json()["tasks"][0]["result"][0]["items"]
```

Kody lokalizacji i języków pobrać raz z `/v3/serp/google/locations` i `/v3/serp/google/languages` i zacache'ować lokalnie — to nie są dane zmieniające się często. https://docs.dataforseo.com/v3/serp/google/organic/live/advanced/

Serwer MCP jako alternatywa dla własnego CLI: `npx dataforseo-mcp-server@latest`, zmienne `DATAFORSEO_LOGIN` i `DATAFORSEO_PASSWORD`, tryb stdio. Pozwala agentowi przeglądać indeks dokumentacji i wykonywać uwierzytelnione żądania bez pisania wrapperów. https://github.com/dataforseo/mcp-server-typescript

Ocena: MCP jest wygodny do eksploracji, ale do powtarzalnych zadań (cotygodniowy pomiar, diff) lepszy jest własny skrypt zapisujący do SQLite, bo tylko wtedy powstaje historia. MCP nie zapisuje nic między sesjami.

### 4.2 Serper

Zwykły REST, klucz w nagłówku, parametry `q`, `gl`, `hl`, `num`. Brak publicznej dokumentacji pod `docs.serper.dev` (DNS nie rozwiązuje się na 2026-08-28), więc parametry potwierdzić w panelu po rejestracji. `[do weryfikacji]`

### 4.3 SerpApi

Ma oficjalne biblioteki klienckie dla wielu języków, w tym Pythona i Node — nie potwierdziłem tego w dokumentacji w tym researchu, ale jest to standardowa część oferty. `[do weryfikacji]` na https://serpapi.com/integrations

### 4.4 Warstwa lokalna, której nikt nie dostarczy

Niezależnie od dostawcy trzeba samodzielnie napisać:

1. **Magazyn historii** — SQLite z tabelami `keywords`, `serp_snapshots`, `rankings`, `backlinks`, `llm_mentions`. Bez tego każdy pomiar jest jednorazowy i agent nie odpowie na pytanie „co się zmieniło".
2. **Warstwa diff** — porównanie dwóch snapshotów i zwrócenie tylko zmian. To jest właściwy interfejs dla agenta: nie 200 pozycji, tylko 6 fraz, które spadły.
3. **Normalizacja domen** — porównywanie `mentzen.pl`, `www.mentzen.pl`, subdomen i URL-i ze slashem na końcu.
4. **Budżetowanie** — twardy licznik zapytań na uruchomienie, żeby pętla agenta nie wydała miesięcznego budżetu w jednym przebiegu.

---

## 5. Zastosowanie dla agenta SEO — konkretne workflow

### 5.1 `serp-snapshot` — cotygodniowy pomiar pozycji

Wejście: lista fraz w pliku (kategorie: usługi, poradniki, marka, konkurencja).
Działanie: DataForSEO Standard queue, `location_name: "Poland"`, `language_code: "pl"`, `depth: 20`, desktop i mobile osobno dla fraz najważniejszych.
Zapis: pozycja mentzen.pl, URL rankujący, top 10 domen konkurencji, obecność `ai_overview`, featured snippet, PAA.
Wyjście dla agenta: tylko diff — frazy, które zmieniły pozycję o więcej niż 3 miejsca, frazy, które weszły lub wypadły z top 10, frazy, dla których pojawił się nowy AI Overview.

Wartość: agent wie, gdzie kierować pracę nad treścią, i widzi, czy zmiana na stronie przełożyła się na wynik.

### 5.2 `serp-inspect` — analiza SERP pod konkretną frazę przed pisaniem treści

Jedno zapytanie Live, pełne 20 wyników, plus People Also Ask.
Agent dostaje: kto rankuje (kancelarie, portale, blogi), jakie typy stron, jakie pytania zadaje Google w PAA, czy jest AI Overview i co cytuje.
To zastępuje ręczne googlowanie przed każdym briefem contentowym i jest jednym z niewielu miejsc, gdzie agent naprawdę potrzebuje surowego SERP-u.

### 5.3 `domain-keywords` — przegląd zasięgu

DataForSEO Labs `ranked_keywords` dla `mentzen.pl` i dla 3–5 konkurentów, raz w miesiącu.
Wyjście: frazy, na które konkurencja rankuje w top 10, a mentzen.pl nie rankuje wcale (luka contentowa), oraz frazy, na których mentzen.pl jest na pozycjach 11–20 (najtańsze do poprawy).
Koszt rzędu kilku centów miesięcznie.

### 5.4 `ai-visibility` — widoczność w modelach

Dwa niezależne strumienie:

- **Google**: cotygodniowy odczyt raportu Generative AI z Search Console — wyświetlenia w AI Overviews i AI Mode w podziale na strony i daty. Wymaga sprawdzenia, czy właściwość ma już dostęp do raportu i czy dane wychodzą przez API. https://support.google.com/webmasters/answer/16984139
- **Poza Google**: stały zestaw 30–50 promptów po polsku, odpowiadających realnym pytaniom klientów („jaka kancelaria podatkowa dla spółki z o.o.", „kto doradza w sprawie estońskiego CIT"), odpytywany cyklicznie przez LLM Responses (ChatGPT, Claude, Gemini, Perplexity) albo LLM Scraper. Zapis: czy marka padła, w którym miejscu odpowiedzi, jakie źródła zostały zacytowane.

Metryka, którą warto śledzić: udział promptów, w których marka jest wymieniona, oraz lista domen cytowanych zamiast nas. Ta druga lista jest bezpośrednim planem linkbuildingu i publikacji gościnnych.

Uzupełniająco: LLM Mentions daje gotowe agregaty wzmianek o domenie/marce bez budowania własnego zestawu promptów. Tańsze w utrzymaniu, ale mniej kontroli nad tym, o co pytamy. https://docs.dataforseo.com/v3/ai_optimization-llm_mentions-overview/

### 5.5 `backlinks-diff` — miesięczny przegląd profilu linków

DataForSEO Backlinks, pełny zrzut referring domains raz w miesiącu, porównanie z poprzednim.
Wyjście dla agenta: nowe domeny linkujące (kandydaci do podziękowania i pogłębienia relacji), utracone linki (kandydaci do odzyskania), nowe anchory (wykrywanie spamu i ataków SEO negatywnego).
Uzupełniająco Bing Webmaster API jako darmowa kontrola krzyżowa — jeśli w ogóle zwróci dane.

### 5.6 `brand-mentions` — wzmianki

Wariant minimalny i wystarczający na start: cotygodniowe zapytania do endpointu `news` i `search` z frazami markowymi, `gl=pl`, `hl=pl`, deduplikacja po URL względem bazy, zwracanie tylko nowych trafień.
Wyjście: lista nowych stron wspominających markę, z podziałem na te, które linkują, i te, które nie linkują. Ta druga lista to gotowe zadania linkbuildingowe (prośba o dodanie linku do istniejącej wzmianki — najtańszy link, jaki istnieje).

### 5.7 Czego agent nie powinien robić automatycznie

- Publikować ani modyfikować treści na produkcji na podstawie samego spadku pozycji
- Kontaktować się z podmiotami z listy wzmianek
- Zużywać budżetu API bez twardego limitu na przebieg

---

## 6. Wymagana konfiguracja po stronie użytkownika

### 6.1 Konieczne

1. **DataForSEO** — założyć konto na https://dataforseo.com , pobrać login i hasło API z https://app.dataforseo.com/api-access . Do testów wystarczy $1 kredytu przy rejestracji plus Sandbox; do pracy produkcyjnej minimalna wpłata $50 (starczy na wiele miesięcy przy jednym serwisie). Zapisać jako `DATAFORSEO_LOGIN` i `DATAFORSEO_PASSWORD`.
2. **Google Search Console** — sprawdzić w interfejsie, czy właściwość mentzen.pl ma już raport „Wyniki generatywnej AI". Jeśli nie ma, jedyne wyjście to czekać na rozszerzenie udostępniania; nie da się tego przyspieszyć.
3. **Lista fraz** — plik z frazami do trackingu, ułożony przez użytkownika lub wspólnie z agentem. To jedyny wsad, którego żadne API nie zastąpi, i od jego jakości zależy cała reszta.

### 6.2 Opcjonalne, warte rozważenia

4. **Bing Webmaster Tools** — zweryfikować mentzen.pl, wygenerować klucz API. Zero kosztu, potencjalnie darmowe dane o linkach i o zapytaniach w Bing (istotne, bo część systemów AI korzysta z indeksu Bing). Zweryfikować empirycznie, czy `GetLinkCounts` zwraca niepuste dane.
5. **Klucze do modeli** — jeśli monitoring widoczności w AI ma iść własną ścieżką zamiast przez DataForSEO, potrzebne są klucze do API modeli. Przez DataForSEO LLM Responses nie są potrzebne, bo koszt LLM jest doliczany do rachunku DataForSEO.

### 6.3 Do decyzji użytkownika przed wdrożeniem

6. **Kwestia zgodności z ToS Google** przy korzystaniu z SERP API — patrz sekcja 3.6. Przy kancelarii warto, żeby to była świadoma decyzja, a nie efekt uboczny wyboru narzędzia.
7. **Czy monitoring wzmianek ma obejmować social media** — jeśli tak, wariant DIY nie wystarczy i trzeba zweryfikować ofertę Brand24 (polski dostawca, prawdopodobnie najlepsze pokrycie polskich źródeł). Sprawdzić bezpośrednio dostępność i cennik API.

### 6.4 Czego nie kupować

| Rozwiązanie | Dlaczego to przerost przy jednym serwisie |
| --- | --- |
| Ahrefs / Semrush wyłącznie dla API | Abonament od ok. $129/mies. `[do weryfikacji]`, żeby dostać dane, które DataForSEO da za kilka dolarów rocznie przy tej skali. Ma sens tylko, jeśli użytkownik i tak korzysta z interfejsu Ahrefs w codziennej pracy |
| Dedykowana platforma AI visibility (Profound i podobne) | $499/mies. `[do weryfikacji]` za to, co przy 40 promptach da się odtworzyć za ułamek dolara. Wartością tych platform jest gotowy dashboard i benchmarki branżowe, a nie sam pomiar — agent CLI nie potrzebuje dashboardu |
| Moz Links API | Darmowy próg 50 wierszy miesięcznie `[do weryfikacji]` jest bezużyteczny, a płatne progi nie są tańsze od DataForSEO. Brak powodu, by dokładać trzecie źródło linków |
| Osobne SaaS do rank trackingu | Duplikuje pętlę, którą i tak trzeba zbudować wokół SERP API, i nie daje agentowi dostępu do surowych danych |
| Google Custom Search JSON API | Zamknięte dla nowych klientów, wyłączenie 1 stycznia 2027. https://developers.google.com/custom-search/v1/overview |
| Codzienny pełny zrzut backlinków | Profil linków zmienia się w skali tygodni, nie godzin. Miesięcznie wystarczy |
| Śledzenie tysięcy fraz | Przy jednym serwisie B2B realna lista to 150–300 fraz. Powyżej tego rośnie szum, nie informacja |

### 6.5 Rekomendowana kolejność wdrożenia

1. DataForSEO SERP (Standard queue) + SQLite + diff → `serp-snapshot`, `serp-inspect`
2. DataForSEO Labs `ranked_keywords` → `domain-keywords`, miesięcznie
3. Raport Generative AI w GSC, gdy tylko będzie dostępny → `ai-visibility`, strumień Google
4. DataForSEO Backlinks → `backlinks-diff`, miesięcznie
5. LLM Responses lub LLM Mentions → `ai-visibility`, strumień poza Google
6. Wzmianki DIY przez endpoint news → `brand-mentions`

Punkty 1–4 pokrywają większość realnej wartości. Punkty 5–6 są warte uruchomienia dopiero, gdy pierwsze cztery działają i mają historię, do której da się je odnieść.

---

## Podsumowanie źródeł

- https://dataforseo.com/apis/serp-api/pricing
- https://dataforseo.com/apis/serp-api
- https://dataforseo.com/pricing/dataforseo-labs/dataforseo-google-api
- https://dataforseo.com/pricing/backlinks/backlinks
- https://dataforseo.com/pricing/ai-optimization/llm-responses
- https://dataforseo.com/pricing/ai-optimization/llm-scraper
- https://dataforseo.com/ai-optimization-api
- https://docs.dataforseo.com/v3/serp/google/organic/live/advanced/
- https://docs.dataforseo.com/v3/ai_optimization-overview/
- https://docs.dataforseo.com/v3/ai_optimization-llm_mentions-overview/
- https://docs.dataforseo.com/v3/ai_optimization-llm_responses-overview/
- https://github.com/dataforseo/mcp-server-typescript
- https://serpapi.com/pricing
- https://serper.dev/
- https://support.google.com/webmasters/answer/16984139
- https://support.google.com/webmasters/answer/16983858
- https://support.google.com/webmasters/answer/16908024
- https://developers.google.com/search/blog/2026/06/gen-ai-performance-reports
- https://developers.google.com/webmaster-tools/v1/how-tos/all-your-data
- https://developers.google.com/custom-search/v1/overview
- https://docs.ahrefs.com/en/api/docs/limits-consumption
- https://docs.ahrefs.com/
- https://learn.microsoft.com/en-us/dotnet/api/microsoft.bing.webmaster.api.interfaces.iwebmasterapi.getlinkcounts
- https://learn.microsoft.com/en-us/answers/questions/5939109/bing-webmaster-tools-api-getlinkcounts-and-geturll

Źródła firm trzecich (użyte tylko tam, gdzie oznaczyłem `[do weryfikacji]`):
- https://apiserpent.com/blog/serper-pricing-credits-explained
- https://apiserpent.com/blog/serpapi-pricing-explained
- https://busyless.space/seo-apis/moz
- https://www.surmado.com/blog/best-ai-visibility-tools-2026
- https://www.pragma-code.de/en/blog-search-console-generative-ai-performance
- https://mateuszkozlowski.pl/blog/skuteczne-pozycjonowanie-stron-w-google-przy-uzyciu-senuto-api/
