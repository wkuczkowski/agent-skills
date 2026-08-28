# Google Trends dla agenta SEO (mentzen.pl)

Stan na 2026-08-28. Wszystkie fakty z linkiem do źródła. Rzeczy niepotwierdzone oznaczone `[do weryfikacji]`.

Podsumowanie w jednym zdaniu: oficjalne API Google Trends po ponad roku wciąż jest alfą za formularzem zgłoszeniowym, `pytrends` jest zarchiwizowany od kwietnia 2025, więc realne ścieżki to darmowy scraping przez aktywnie utrzymywany fork (`trendspyg`) albo płatny pośrednik (DataForSEO / SerpApi / Glimpse).

---

## 1. Co daje

### 1.1. Czym są dane Google Trends

Trends nie podaje wolumenu wyszukiwań. Podaje **względny indeks zainteresowania 0–100**, znormalizowany w obrębie jednego zapytania (geo + zakres czasu + kategoria). Google opisuje proces tak: każdy punkt dzielony jest przez łączną liczbę wyszukiwań dla danej geografii i okresu, a wynik skalowany 0–100 względem udziału tematu ([Google Trends Help – dane](https://support.google.com/trends/answer/4365533)).

Co Trends odfiltrowuje, według tej samej strony:
- frazy o niskim wolumenie (pokazywane jako `0`),
- powtórzone wyszukiwania tej samej osoby,
- zapytania ze znakami specjalnymi (np. apostrof),
- wyszukiwania wewnętrzne produktów Google i usług AI.

Google jawnie zastrzega, że dane zawierają szum statystyczny, są próbką („largely unfiltered sample of actual search requests") i nie są odpowiednikiem badania sondażowego.

Uwaga praktyczna na strefy czasowe: dla okresów 30 dni i dłuższych dane są w UTC, dla zapytań do 7 dni w lokalnej strefie użytkownika ([tamże](https://support.google.com/trends/answer/4365533)).

### 1.2. Typy danych, po które sięga się w SEO

- **interest over time** — krzywa 0–100 w czasie (podstawa analizy sezonowości),
- **interest by region** — rozkład geograficzny (dla PL: województwa, miasta),
- **related queries / related topics** — frazy powiązane, w tym „breakout" (nagłe wzrosty),
- **comparison** — do 5 fraz na wspólnej skali 0–100 (limit UI to 5 pozycji; oficjalne API ma ten limit znieść, patrz 2.1),
- **trending now** — bieżące trendy dzienne dla kraju, dostępne publicznym RSS-em (patrz 2.3).

### 1.3. Wiarygodność danych — to trzeba wiedzieć zanim agent oprze na tym decyzje

Trends jest próbkowany, więc **te same zapytanie zadane w różne dni zwraca różne liczby**. Badanie reliability na danych COVID pokazało, że wartości RSV silnie zależą od dnia pobrania, z różnicami korelacji sięgającymi +625% dla regionów Włoch i +175% dla porównań międzynarodowych; autorzy rekomendują pobieranie serii przez kilka kolejnych dni i pracę na średnich, nie na pojedynczym pobraniu ([Reliability of Google Trends, PMC8186442](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8186442/), [preprint PDF](https://www.medrxiv.org/content/10.1101/2020.12.29.20248969.full.pdf)).

Systematyczny przegląd użycia Trends w naukach społecznych zarzuca literaturze, że większość prac nie testuje trafności wewnętrznej miary, nie sprawdza stabilności danych między próbkami i nie dyskutuje uogólnialności ([The (mis)use of Google Trends data in the social sciences, Social Science Research 2024](https://www.sciencedirect.com/science/article/pii/S0049089X24001212)).

Osobno: możliwość odtworzenia dokładnie tego samego szeregu bywa problematyczna, co opisywano już w 2017 ([Econbrowser: Can Google Trends Data Be Replicated?](https://econbrowser.com/archives/2017/06/guest-contribution-can-google-trends-data-be-replicated)).

**Wniosek dla agenta:** Trends nadaje się do ustalania *kształtu* sezonu (kiedy rośnie, kiedy szczyt, jak długo trwa) i do porównań względnych między frazami. Nie nadaje się do prognozowania ruchu w liczbach ani do decyzji na podstawie różnicy kilku punktów indeksu. Do wolumenów bierz Keyword Planner, Ahrefs/Semrush albo enrichment typu Glimpse.

---

## 2. Dostęp i auth

### 2.1. Oficjalne Google Trends API (alpha) — status na dziś

Google ogłosiło API 24 lipca 2025 na blogu Search Central ([Introducing the Google Trends API (alpha)](https://developers.google.com/search/blog/2025/07/trends-api)). Strona programowa: [developers.google.com/search/apis/trends](https://developers.google.com/search/apis/trends).

Co obiecuje, wg strony oficjalnej (stan pobrania: 2026-08-28):
- **rolling window ostatnich 5 lat** danych,
- agregacje **dzienne, tygodniowe, miesięczne, roczne**,
- dane dla **krajów i subregionów**,
- **consistently scaled data** — skala spójna między zapytaniami, dzięki czemu można porównywać dowolnie wiele fraz (UI ogranicza do kilku naraz) i dociągać tylko najnowszy fragment szeregu bez przeliczania całej historii.

Strona oficjalna **nie podaje**: endpointów, metody uwierzytelniania, limitów, cennika ani terminu GA. Wciąż widnieje jako alpha z formularzem „Apply for the alpha"; Google deklaruje priorytet dla zgłoszeń z konkretnym use case i gotowością do dawania feedbacku.

Doniesienia branżowe uzupełniają: okno to **1800 dni**, dane sięgają do ok. 2 dni przed zapytaniem, geografia wg **ISO 3166-2**, start zgłoszeń 24.07.2025, dostęp przyznawany rolling, rozróżnienie zastosowań komercyjnych i niekomercyjnych, brak daty GA ([PPC Land](https://ppc.land/google-opens-alpha-testing-for-new-trends-api-targeting-developers-and-journalists/), [Search Engine Journal](https://www.searchenginejournal.com/google-trends-api-alpha-launching-breaking-news/551935/)).

`[do weryfikacji]` Dokładna forma auth (klucz API vs OAuth vs Google Cloud project), nazwa hosta (`trends.googleapis.com/v1alpha/...` pojawia się w relacjach testerów) i kwoty — dostępne dopiero po przyjęciu do alfy. Nie planuj architektury pod to API jako podstawę; traktuj jako opcjonalny backend, który kiedyś podmienisz.

Uwaga terminologiczna: istnieje osobne, starsze **Google Trends for Health API** (metoda `getTimelinesForHealth`, opakowanie R `gtrendshealth`) — to inny program, dla badaczy zdrowia publicznego, nie mylić z alfą SEO-ową.

### 2.2. pytrends — martwy

[github.com/GeneralMills/pytrends](https://github.com/GeneralMills/pytrends): **„This repository was archived by the owner on Apr 17, 2025. It is now read-only."** README wcześniej wołał o maintainerów i zastrzegał: „This is not an official or supported API".

Objaw praktyczny: HTTP 429 już na pierwszym wywołaniu, bo bootstrap sesji nie pobiera ciasteczek, których wymaga obecny flow Google. Zgłoszenia: [#492](https://github.com/GeneralMills/pytrends/issues/492), [#561](https://github.com/GeneralMills/pytrends/issues/561), [#625](https://github.com/GeneralMills/pytrends/issues/625).

**Nie instaluj pytrends.**

### 2.3. Darmowy oficjalny kanał: RSS „Trending now"

Google udostępnia RSS jako jedną z metod eksportu na stronie Trending now ([Trends Help](https://support.google.com/trends/answer/3076011)).

Endpoint dla Polski: `https://trends.google.com/trending/rss?geo=PL`

Zweryfikowane 2026-08-28 — zwraca poprawny RSS 2.0 z przestrzenią `ht:`. Pola: `ht:approx_traffic` (np. „5000+"), `ht:picture`, `ht:picture_source`, `ht:news_item` (tytuł, URL, źródło, obrazek). Przykładowe pozycje z tego pobrania: „król norwegii" (5000+), „harald v" (2000+), „prezydent rosji" (500+).

Bez auth, bez limitu kontraktowego, zwykły HTTP GET. To jedyny kanał Trends, który da się odpytywać w pętli bez ryzyka bana `[do weryfikacji przy wysokiej częstotliwości — rozsądny interwał to 15–30 min]`.

Ograniczenie: to trendy dzienne (newsowe, głównie rozrywka i polityka), nie dane historyczne dla wybranej frazy. Dla kancelarii przydatne wyłącznie jako trigger newsjackingowy.

### 2.4. Biblioteki scrapujące (nieoficjalne)

Wszystkie sięgają do wewnętrznych endpointów `trends.google.com`, których Google nie gwarantuje. Ryzyko: 429, zmiana formatu bez zapowiedzi, kwestia zgodności z ToS Google. README `trendspy` zastrzega: „This library is not affiliated with Google" i przypomina o zgodności z ToS.

| Repo | Licencja | Gwiazdki | Ostatni push | Otwarte issues | Ocena |
|---|---|---|---|---|---|
| [flack0x/trendspyg](https://github.com/flack0x/trendspyg) | MIT | 44 | 2026-08-19 | 0 | **aktywnie utrzymywany, pierwszy wybór** |
| [yiromo/pytrends-modern](https://github.com/yiromo/pytrends-modern) | NOASSERTION | 62 | 2026-07-13 | 2 | aktywny, ale licencja nieokreślona |
| [sdil87/trendspy](https://github.com/sdil87/trendspy) | MIT | 118 | 2024-12-25 | 14 | bogaty API, ale ~20 mies. bez commita |
| [GeneralMills/pytrends](https://github.com/GeneralMills/pytrends) | Apache-2.0 | — | archiwum 2025-04-17 | — | martwy |

Dane z GitHub API, pobrane 2026-08-28.

Uwaga na treści marketingowe: blogi typu ScrapeBadger, apiserpent czy SocialCrawl twierdzą, że „trendspy jest utrzymywany i działa w każdym teście". Metadane repo tego nie potwierdzają — ostatni commit `sdil87/trendspy` to 25.12.2024. Te teksty są content marketingiem dostawców API, nie źródłem.

### 2.5. Płatni pośrednicy

**SerpApi** — [Google Trends API](https://serpapi.com/google-trends-api). Auth: klucz API w parametrze. Typy danych: `TIMESERIES` (interest over time, wiele fraz), `GEO_MAP` (porównanie regionów, wiele fraz), `GEO_MAP_0` (interest by region, jedna fraza), `RELATED_TOPICS`, `RELATED_QUERIES`. Parametry: `geo`, `hl`, `region` (COUNTRY / REGION / DMA / CITY), `date` (presety od „past hour" po „2004-present" plus zakresy własne), `cat`, `gprop` (web, images, news, shopping, youtube).

**DataForSEO** — [keywords_data/google_trends/explore/live](https://docs.dataforseo.com/v3/keywords_data/google_trends/explore/live/). Auth: HTTP Basic (login:hasło z panelu, Base64). Maks. **5 słów kluczowych na request** (przy `topics_list` / `queries_list` tylko 1). Parametry: `location_name` / `location_code`, `date_from` / `date_to` (yyyy-mm-dd) albo `time_range` (`past_hour`, `past_7_days`, `past_12_months`, …), `type` (`web` / `news` / `youtube` / `images` / `froogle`), `item_types` (`google_trends_graph`, `google_trends_map`, `google_trends_topics_list`, `google_trends_queries_list`).

**Glimpse** — [meetglimpse.com/google-trends-api](https://meetglimpse.com/google-trends-api/). Base URL `https://enterprise.meetglimpse.com`, auth nagłówkiem `apikey: <KLUCZ>`. Dwa endpointy: `/v1/interest` (znormalizowane 0–100) i `/v1/interest_enriched` (**bezwzględny wolumen wyszukiwań** — to jest wartość dodana Glimpse względem reszty). `geo` jako dwuliterowy kod kraju, **PL wspierane**; bez `geo` dane worldwide. Rozdzielczość: daily / weekly (domyślna) / monthly. Klucz zdobywa się kontaktem: `hello@meetglimpse.com`.

---

## 3. Limity i koszty

### 3.1. Ścieżka darmowa (scraping)

Najkonkretniejsze liczby podaje README `trendspyg`:
- ścieżka **Explore jest wrażliwa na rate limit**: ok. **8–10 świeżych sesji przeglądarki w 15 minut** wywołuje blokadę 429,
- powrót do sprawności obserwowany po **35+ minutach**,
- biblioteka rzuca `RateLimitError` natychmiast, zamiast retry'ować w ciemno,
- `cookies="disk"` (od 1.6.0) reużywa ciasteczek sesji i zmniejsza liczbę odrzuceń,
- ścieżka **RSS jest bezpieczna do ciągłego pollowania**.

Czasy odpowiedzi wg tego samego README: RSS poniżej sekundy (10–20 trendów), CSV ~10 s (480+ trendów, wymaga Chrome), Explore 10–90 s (historia i porównania, wymaga Chrome).

To jest realny sufit: **kilkadziesiąt zapytań Explore dziennie**, jeśli chce się uniknąć bana. Dlatego cache jest obowiązkowy, nie opcjonalny.

### 3.2. SerpApi

Z [cennika](https://serpapi.com/pricing) (pobrane 2026-08-28):

| Plan | Cena/mies. | Wyszukiwań/mies. | Przepustowość |
|---|---|---|---|
| Free | $0 | 250 | 50/h |
| Starter | $25 | 1 000 | 200/h |
| Developer | $75 | 5 000 | 1 000/h |
| Production | $150 | 15 000 | 3 000/h |
| Big Data | $275 | 30 000 | 6 000/h |

Liczą się tylko udane zapytania; cache'owane, błędne i nieudane nie obciążają limitu.

Dla naszego przypadku: przy koszyku ~200 fraz odświeżanym co miesiąc plan Free (250/mies.) wystarcza na start, Starter za $25 daje komfort.

### 3.3. DataForSEO

Z [cennika Google Trends API](https://dataforseo.com/pricing/keywords-data/google-trends):
- **kolejka standardowa: $0,0027 za task** (do 45 min na wynik),
- **tryb live: $0,011 za task** (śr. do 32 s).

Minimalna wpłata na konto: **$50** ([dataforseo.com/pricing](https://dataforseo.com/pricing)).

`[do weryfikacji]` Wtórne źródło (nextgrowth.ai) podaje $0,0012 za task live — to sprzeczne z cennikiem oficjalnym. Bierz $0,011 jako założenie budżetowe.

Limity techniczne wg dokumentacji: do **500 tys. requestów dziennie** łącznie na wszystkich endpointach Google Trends; przekroczenie ok. **250 tasków live w minucie** może zwrócić błąd limitu `[do weryfikacji — inne miejsce dokumentacji mówi o 2000 wywołań API/min, prawdopodobnie chodzi o różne warstwy limitu]`.

Rachunek dla nas: 200 fraz × 12 pobrań rocznie ÷ 5 fraz na request = 480 tasków rocznie. W trybie live to ~$5,30 rocznie. Koszt jest pomijalny, barierą jest tylko próg wpłaty $50.

### 3.4. Glimpse

Cennika API nie publikuje; wycena mailowa (`hello@meetglimpse.com`). Plany platformy webowej od $99/mies. wg źródła wtórnego `[do weryfikacji]` ([OutlierKit](https://outlierkit.com/resources/glimpse-pricing/)). To najdroższa opcja z tej trójki i jedyna, która daje wolumen bezwzględny.

### 3.5. Oficjalne API (alpha)

Brak opublikowanych limitów i cennika. Google zapowiedziało, że szczegóły pojawią się w trakcie testów.

---

## 4. Biblioteki i przykłady integracji

### 4.1. trendspyg (rekomendowany start, darmowy)

```bash
uv add "trendspyg[cli]"     # albo trendspyg[all]
```

Wymaga Pythona 3.8+; **Chrome/Chromium jest wymagany dla ścieżek CSV i Explore** (RSS działa bez przeglądarki). Wsparcie: 125 krajów + 51 stanów US, 20 kategorii. Extras: `[async]`, `[cli]`, `[mcp]` (serwer MCP, Python 3.10+), `[all]`.

```python
# 1. Trendy bieżące (bez przeglądarki, sub-sekundowe)
from trendspyg import download_google_trends_rss
trends = download_google_trends_rss(geo='PL')
print(trends[0]['keyword'], trends[0]['traffic'])

# 2. Pełny obraz dla frazy: krzywa + related queries + regiony
from trendspyg import download_google_trends_explore
data = download_google_trends_explore("rozliczenie pit", geo="PL")
print(data["interest_over_time"][-1])

# 3. Porównanie 2–5 fraz na wspólnej skali
from trendspyg import download_google_trends_comparison
env = download_google_trends_comparison(
    ["ksef", "krajowy system e-faktur", "e-faktura"],
    geo="PL"
)
print(env["averages"])
```

CLI (przydatne, bo agent może wołać shellem bez pisania kodu):

```bash
trendspyg rss --geo PL
trendspyg explore -k "składka zdrowotna" -k "ryczałt"
trendspyg explore -k ksef --gprop youtube
trendspyg watch --geo PL --interval 900
```

Extras `[mcp]` daje gotowy serwer MCP — czyli agent może dostać Trends jako natywne narzędzie bez pisania wrappera. `[do weryfikacji]` — sprawdzić zakres narzędzi wystawianych przez ten serwer przed wpięciem.

### 4.2. trendspy (bogatsze API, ale nieutrzymywane)

Wart uwagi wyłącznie dla funkcji, których nie ma nigdzie indziej: `multirange` (porównanie różnych okresów), `trending_now_showcase_timeline()` (historia dla 500+ trendujących fraz z niezależną normalizacją), `trending_now_news_by_ids()`.

```python
from trendspy import Trends
tr = Trends()                                   # albo Trends(proxy="http://...")
df  = tr.interest_over_time(['pit', 'zus'])
reg = tr.interest_by_region('pit', geo='PL', resolution='REGION')
rel = tr.related_queries('pit')
```

Kody geo obsługują subregiony w formacie `XX-YY` (README pokazuje `US-NY`, `US-CA`). Dla Polski analogicznie `PL-MZ` (mazowieckie) itd. `[do weryfikacji — przetestować, czy backend zwraca dane dla polskich województw]`.

### 4.3. DataForSEO (Python, płatny, stabilny)

Zwykły REST + Basic auth, nie potrzeba SDK:

```python
import os, requests
r = requests.post(
    "https://api.dataforseo.com/v3/keywords_data/google_trends/explore/live",
    auth=(os.environ["DATAFORSEO_LOGIN"], os.environ["DATAFORSEO_PASSWORD"]),
    json=[{
        "keywords": ["rozliczenie pit", "pit 37", "twój e-pit"],  # maks. 5
        "location_name": "Poland",
        "language_name": "Polish",
        "date_from": "2021-01-01",
        "type": "web",
        "item_types": ["google_trends_graph"],
    }],
    timeout=60,
)
r.raise_for_status()
```

Zaleta wobec scrapingu: brak 429, deterministyczne czasy, dane historyczne w jednym wywołaniu. Wada: koszt i próg wpłaty.

### 4.4. Glimpse (gdy potrzebny wolumen bezwzględny)

```python
import os, requests
r = requests.get(
    "https://enterprise.meetglimpse.com/v1/interest_enriched",
    headers={"apikey": os.environ["GLIMPSE_API_KEY"]},
    params={"keyword": "ksef", "geo": "PL", "resolution": "weekly"},
    timeout=30,
)
```

`[do weryfikacji]` Dokładne nazwy parametrów query — dokumentacja ma live playground, potwierdzić przy pierwszym wywołaniu.

### 4.5. RSS bez żadnej biblioteki

```python
import feedparser
feed = feedparser.parse("https://trends.google.com/trending/rss?geo=PL")
for e in feed.entries:
    print(e.title, e.get("ht_approx_traffic"))
```

---

## 5. Zastosowanie dla agenta SEO

Kontekst: mentzen.pl, B2B prawo/podatki/księgowość, rynek polski. Branża jest skrajnie sezonowa i sterowana kalendarzem ustawowym, więc Trends ma tu wyższą wartość niż w większości nisz.

### 5.1. Workflow: mapa sezonowości koszyka fraz (fundament, robiony raz, odświeżany kwartalnie)

1. Zdefiniuj koszyk 150–300 fraz filarowych (PIT, CIT, VAT, ZUS, składka zdrowotna, KSeF, JPK_V7, ryczałt, estoński CIT, kasa fiskalna, sukcesja, fundacja rodzinna, ulga B+R itd.).
2. Dla każdej pobierz **5-letni szereg tygodniowy**, `geo=PL`. Pięć lat, nie dwanaście miesięcy — sezon poznaje się dopiero po powtórzeniu.
3. Wylicz profil roczny: średnia z pięciu lat per tydzień ISO, znormalizowana do własnego maksimum frazy.
4. Wyznacz dla każdej frazy: **tydzień szczytu**, **tydzień startu wzrostu** (pierwszy tydzień, w którym profil przekracza np. 40% rocznego maksimum) i **szerokość okna**.
5. Zapisz jako tabelę `fraza → tydzień_startu → tydzień_szczytu → siła_sezonu`.

Produkt: kalendarz redakcyjny, w którym **publikacja lub refresh wypada 4–6 tygodni przed tygodniem startu wzrostu**, żeby strona zdążyła się zaindeksować i wejść na pozycje przed pikiem.

### 5.2. Workflow: kolejka odświeżania istniejących treści

Mapuj URL → fraza główna → profil sezonowy z 5.1. Agent co tydzień wypluwa listę URL-i, których sezon startuje za 4–6 tygodni, posortowaną wg (siła sezonu × obecna pozycja w GSC). To zamienia „trzeba by coś zaktualizować" w konkretną kolejkę zadań na dany tydzień.

### 5.3. Workflow: rozróżnienie „spadek bo sezon" od „spadek bo utrata pozycji"

Najmocniejsze zastosowanie dla agenta. Zestaw krzywą Trends z krzywą wyświetleń z Google Search Console dla tej samej frazy i tego samego okresu:

- Trends spada, GSC spada w podobnym tempie → **sezon**, nie ruszaj strony,
- Trends płaski lub rośnie, GSC spada → **problem SEO** (utrata pozycji, kanibalizacja, update algorytmu), eskaluj,
- Trends rośnie, GSC płaski → **niewykorzystana szansa**, strona nie łapie rosnącego popytu.

Ta reguła powinna być wpisana na sztywno w prompt agenta analizującego GSC — bez niej agent regularnie diagnozuje sezonowe spadki jako awarię.

### 5.4. Workflow: wybór wariantu frazy do title/H1

Branża ma dużo konkurujących nazw tego samego: „KSeF" vs „Krajowy System e-Faktur", „spółka z o.o." vs „sp. z o.o.", „składka zdrowotna" vs „danina zdrowotna", „Polski Ład" vs nazwy poszczególnych zmian. Porównanie na wspólnej skali 0–100 (`download_google_trends_comparison`, maks. 5 fraz) rozstrzyga, która forma idzie do title i H1, a która zostaje jako wariant w treści.

### 5.5. Workflow: wykrywanie tematów rosnących rok do roku

Dla każdej frazy z koszyka policz średnią z ostatnich 12 tygodni i porównaj z analogicznym oknem sprzed roku (ten sam zakres tygodni ISO, żeby wyeliminować sezon). Ranking wg dynamiki YoY daje listę tematów „na fali" — w tej branży to zwykle nowe regulacje wchodzące w życie. Frazy z ujemną dynamiką i indeksem blisko zera odkładaj: przy niskim wolumenie Trends pokazuje `0` i szum, więc dalsza analiza jest bezwartościowa.

### 5.6. Workflow: newsjacking na RSS

`https://trends.google.com/trending/rss?geo=PL` odpytywany co 15–30 minut, filtr po słowniku branżowym (regex: `podatk|PIT|CIT|VAT|ZUS|składk|KSeF|faktur|ustaw|Sejm|Trybunał|NSA|urząd skarbowy|mandat|emerytur`). Trafienie → alert do agenta z sugestią szybkiego komentarza eksperckiego albo aktualizacji istniejącego artykułu. `ht:news_item` w feedzie daje od razu linki do źródeł, więc agent ma z czego budować brief.

Realistycznie: większość polskich trendów dziennych to sport i rozrywka, więc trafień będzie mało. Ale kiedy trafi (wyrok TK, zmiana stawki, awaria e-Urzędu Skarbowego), okno na wyprzedzenie konkurencji liczy się w godzinach.

### 5.7. Workflow: priorytety geograficzne

`interest_by_region` z `geo=PL` przy `resolution='REGION'` daje rozkład na województwa. Użyteczne przy stronach oddziałów i frazach lokalnych („biuro rachunkowe Toruń"), pod warunkiem że fraza ma dość wolumenu, żeby regiony nie były zerami.

### 5.8. Higiena danych — reguły do wpisania w skill agenta

1. **Uśredniaj po dniach pobrania.** Ten sam szereg pobieraj 3–5 dni z rzędu i pracuj na średniej (rekomendacja z [PMC8186442](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8186442/)).
2. **Loguj metadane każdego pobrania**: data, `geo`, `cat`, `timeframe`, źródło (trendspyg/DataForSEO), wersja biblioteki. Bez tego porównania między pobraniami są bezwartościowe.
3. **Cache lokalny obowiązkowy** (SQLite albo parquet). Limit zapytań to wąskie gardło, nie CPU.
4. **Nigdy nie mieszaj serii pobranych z różnymi `timeframe`** — normalizacja 0–100 jest liczona w obrębie zapytania, więc dwa różne okna to dwie nieporównywalne skale.
5. **Nie raportuj indeksu jako wolumenu.** W outputach agenta pisz „indeks zainteresowania", nie „liczba wyszukiwań".
6. **Ignoruj różnice poniżej ~5 punktów indeksu** — to szum, który Google sam deklaruje.
7. Do porównań kotwicz frazy: dorzucaj do każdego zapytania jedną stałą frazę referencyjną o znanej, stabilnej popularności, żeby móc łączyć wyniki z różnych zapytań na wspólnej skali. (Rozwiązuje to samo, co obiecuje „consistently scaled data" w oficjalnej alfie.)

### 5.9. Czego Trends nie zrobi

- nie poda wolumenu (chyba że Glimpse/`interest_enriched`),
- nie poda trudności frazy ani intencji,
- nie pokaże fraz long-tail o niskim wolumenie (a to duża część ruchu B2B kancelarii),
- nie zastąpi Keyword Plannera, Ahrefs/Semrush ani GSC.

Trends jest warstwą **czasową** w stosie SEO. Warstwę wolumenową i konkurencyjną bierz skądinąd.

---

## 6. Wymagana konfiguracja po stronie użytkownika

### 6.1. Decyzja o ścieżce

| Ścieżka | Koszt | Kiedy wybrać |
|---|---|---|
| A. `trendspyg` + RSS | $0 | start, PoC, mały koszyk fraz |
| B. DataForSEO | wpłata $50, ~$5/rok użycia | produkcja, stabilność, brak 429 |
| C. SerpApi | Free 250/mies. lub $25/mies. | jeśli i tak używasz SerpApi do SERP-ów |
| D. Glimpse | wycena mailowa, drogo | tylko gdy potrzebny wolumen bezwzględny |
| E. wniosek o alfę Google | $0 | równolegle do A–D, jako opcja na przyszłość |

Rekomendacja: **A na start, B jako docelowe**. C ma sens tylko przy konsolidacji dostawców.

### 6.2. Ścieżka A — trendspyg

- zainstalowany Chrome lub Chromium (`chromium-browser` / `google-chrome`), wymagany dla CSV i Explore,
- `uv add "trendspyg[cli]"` w projekcie,
- katalog na ciasteczka sesji (dla `cookies="disk"`),
- opcjonalnie proxy z polskim IP, jeśli zaczną się 429 `[do weryfikacji, czy potrzebne — testować bez proxy]`,
- **przed instalacją**: zgodnie z regułą z CLAUDE.md zleć subagentowi audyt pakietu. `trendspyg` to młody projekt (utworzony 2025-11-03), 44 gwiazdki, jeden autor, PyPI. Nazwa jest myląco podobna do `trendspy` — to realne ryzyko pomyłki i wektor typosquattingu. Zapinaj dokładną wersję w `pyproject.toml`, nie zakres.

### 6.3. Ścieżka B — DataForSEO

- konto na dataforseo.com, wpłata min. **$50**,
- login i hasło API z panelu → zmienne środowiskowe `DATAFORSEO_LOGIN`, `DATAFORSEO_PASSWORD`,
- auth to HTTP Basic, żadnego SDK nie trzeba.

### 6.4. Ścieżka C — SerpApi

- konto na serpapi.com, plan Free wystarczy na test (250 zapytań/mies.),
- klucz API → `SERPAPI_API_KEY`.

### 6.5. Ścieżka D — Glimpse

- mail na `hello@meetglimpse.com` po klucz API i wycenę pod nasz wolumen,
- klucz → `GLIMPSE_API_KEY`, wysyłany nagłówkiem `apikey`.

### 6.6. Ścieżka E — wniosek o alfę

Formularz „Apply for the alpha" na [developers.google.com/search/apis/trends](https://developers.google.com/search/apis/trends). We wniosku trzeba opisać use case, harmonogram wdrożenia i gotowość do feedbacku; jest rozróżnienie zastosowania komercyjnego i niekomercyjnego. Nasz przypadek jest komercyjny — nie ukrywaj tego, ale opisz konkret (analiza sezonowości fraz podatkowych dla planowania treści edukacyjnych), bo Google deklaruje priorytet dla zgłoszeń z konkretnym use case.

### 6.7. Sieć i bezpieczeństwo

Wszystko działa jako lokalne skrypty CLI. Nic nie wymaga otwierania portu, więc `fw-temp` jest zbędny. Jeśli powstanie lokalne API nad cache'em Trends, wiąż je na `127.0.0.1`.

### 6.8. Do ustalenia przez agenta przed pierwszym uruchomieniem

- `[do weryfikacji]` polskie kody subregionów w Trends (`PL-MZ`, `PL-DS`, …) — przetestować na jednej frazie o dużym wolumenie,
- `[do weryfikacji]` czy `trendspyg` obsługuje `geo='PL'` w ścieżce CSV (README wymienia 125 krajów, ale przykłady CSV są dla `US-CA`),
- `[do weryfikacji]` aktualne terminy podatkowe w PL (start Twój e-PIT, deadline rozliczenia rocznego, terminy JPK/CIT) — potwierdzić na [podatki.gov.pl](https://www.podatki.gov.pl/), bo to kotwice, do których agent będzie przykładał krzywe sezonowe. Nie zakładać dat z pamięci.
- `[do weryfikacji]` zakres narzędzi serwera MCP z `trendspyg[mcp]`.

---

## Źródła

Oficjalne:
- [Google Search Central: Introducing the Google Trends API (alpha)](https://developers.google.com/search/blog/2025/07/trends-api)
- [Google: Get early access to the Google Trends API alpha](https://developers.google.com/search/apis/trends)
- [Google Trends Help: skąd pochodzą dane Trends](https://support.google.com/trends/answer/4365533)
- [Google Trends Help: Trending now i eksport RSS](https://support.google.com/trends/answer/3076011)
- [Trending now RSS dla Polski](https://trends.google.com/trending/rss?geo=PL)

Biblioteki:
- [GeneralMills/pytrends (zarchiwizowany)](https://github.com/GeneralMills/pytrends) + issues [#492](https://github.com/GeneralMills/pytrends/issues/492), [#561](https://github.com/GeneralMills/pytrends/issues/561), [#625](https://github.com/GeneralMills/pytrends/issues/625)
- [flack0x/trendspyg](https://github.com/flack0x/trendspyg)
- [sdil87/trendspy](https://github.com/sdil87/trendspy) / [PyPI](https://pypi.org/project/trendspy/)
- [yiromo/pytrends-modern](https://github.com/yiromo/pytrends-modern)

Dostawcy:
- [SerpApi Google Trends API](https://serpapi.com/google-trends-api) / [cennik](https://serpapi.com/pricing)
- [DataForSEO google_trends/explore/live](https://docs.dataforseo.com/v3/keywords_data/google_trends/explore/live/) / [cennik](https://dataforseo.com/pricing/keywords-data/google-trends)
- [Glimpse API](https://meetglimpse.com/google-trends-api/)

Wiarygodność danych:
- [Reliability of Google Trends (PMC8186442)](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8186442/)
- [The (mis)use of Google Trends data in the social sciences (Social Science Research)](https://www.sciencedirect.com/science/article/pii/S0049089X24001212)
- [Econbrowser: Can Google Trends Data Be Replicated?](https://econbrowser.com/archives/2017/06/guest-contribution-can-google-trends-data-be-replicated)

Prasa branżowa (źródła wtórne):
- [PPC Land: Google opens alpha testing for new Trends API](https://ppc.land/google-opens-alpha-testing-for-new-trends-api-targeting-developers-and-journalists/)
- [Search Engine Journal: Google Trends API (Alpha) Launching](https://www.searchenginejournal.com/google-trends-api-alpha-launching-breaking-news/551935/)
