# Techniczne SEO — praktyczna checklista dla WordPress (mentzen.pl)

Research: 2026-08-28. Kontekst: kancelaria prawo/podatki/księgowość, B2B, rynek polski, WordPress, tematyka YMYL. Konwencja: **rób X / unikaj Y** + dlaczego. Twierdzenia bez potwierdzenia w wysokozaufanym źródle oznaczone `[do weryfikacji]`.

---

## TL;DR

1. **FAQPage rich results w Google już nie istnieją** (wyłączone 2026-05-07) — nie wdrażaj nowego markupu FAQ pod SERP; istniejący nie szkodzi, można zostawić.
2. **CWV to higiena, nie dźwignia rankingowa.** Google potwierdza, że CWV są używane w rankingu, ale nie gwarantują pozycji; pozostałe elementy page experience (HTTPS, brak interstitiali) nie podbijają rankingu bezpośrednio. Nie przepalaj budżetu na 95/100 w Lighthouse kosztem treści i E-E-A-T.
3. Największy zwrot wydajnościowy na WordPressie: **full-page cache po stronie serwera + CDN**, potem obrazy (WebP/AVIF, bez lazy-load na LCP), fonty self-hosted, odroczenie skryptów third-party (główny zabójca INP — baner zgód, chat, widget Calendesk).
4. **robots.txt steruje crawlowaniem, nie indeksowaniem.** Nigdy `Disallow` + `noindex` na tym samym URL-u — crawler nie zobaczy noindexu. Nie blokuj CSS/JS.
5. **Siła sygnałów kanonikalizacji: 301 > rel=canonical > sitemap.** Self-referencing canonical wszędzie, spójny z sitemapą i linkowaniem wewnętrznym.
6. Sitemap: tylko kanoniczne URL-e do indeksu, `lastmod` tylko gdy wiarygodny, `priority`/`changefreq` Google ignoruje. W WP dokładnie **jedna** sitemapa (wtyczka SEO albo rdzeń, nie obie).
7. **Crawl budget nie jest problemem serwisu wielkości mentzen.pl** (Google: dotyczy 10 tys.+ stron z codziennymi zmianami) — ale crawl bloat WordPressa (archiwa dat, attachment pages, `?s=`, puste tagi) trzeba posprzątać, bo generuje thin content i soft 404.
8. Dane strukturalne: wdrożyć tanio **Organization, LocalBusiness, Article, BreadcrumbList, ProfilePage** — jako kwalifikowalność do rich results i budowę encji, **nie** jako dźwignię widoczności w AI (eksperyment Ahrefs: brak istotnego efektu schema na cytowania AI).
9. **Mobile-first indexing ukończony (2023-10-31):** treść, linki i schema nieobecne w wersji mobilnej dla Google nie istnieją.
10. Nowa warstwa: **crawlery AI**. Sprawdź, czy robots.txt (i Cloudflare) nie blokuje GPTBot/OAI-SearchBot/ClaudeBot/Google-Extended; crawler ChatGPT **nie renderuje JS** — istotna treść musi być w serwerowym HTML. llms.txt — nie rób, Google i najwięksi dostawcy tego nie używają.

---

## 1. Core Web Vitals i wydajność

### 1.1 Progi i pomiar

| Metryka | „Good" | Mierzy |
|---|---|---|
| LCP | ≤ 2,5 s | ładowanie głównego elementu |
| INP | ≤ 200 ms | responsywność na interakcje (zastąpiła FID w 2024) |
| CLS | ≤ 0,1 | stabilność wizualna |

- **Mierz na danych polowych (CrUX / raport CWV w GSC), 75. percentyl, osobno mobile i desktop.** Lighthouse służy do diagnozy, nie do oceny „zdania testu" — Google ocenia z danych polowych. Źródło: https://web.dev/articles/vitals (2024-10-31).

### 1.2 Kalibracja: ile to naprawdę waży

- Google: CWV „are used by our ranking systems", ale „there is no single signal" i dobre wyniki w raporcie CWV „doesn't guarantee that your pages will rank at the top". **Poza CWV pozostałe aspekty page experience (HTTPS, brak nachalnych interstitiali) nie podbijają rankingu bezpośrednio.** Źródło: https://developers.google.com/search/docs/appearance/page-experience (2025-12-10).
- **Rób:** traktuj CWV jako higienę i czynnik konwersji. **Unikaj:** wielotygodniowych projektów „performance" zanim treść i E-E-A-T są zrobione.
- Kontrapunkt pod AI: wg Samo Cerara z kursu Ahrefs (https://www.youtube.com/watch?v=uza9GX0E2mw) szybkość jest **ważniejsza** przy retrievalu AI niż w klasycznym SEO — model pobiera i parsuje stronę w czasie rzeczywistym i zbyt wolną stronę potrafi odrzucić przed oceną. Podobnie Szymon Parzych (Vestigio, webinar Senuto, https://www.youtube.com/watch?v=MKwi0qmVSS8): crawlery mają skończony budżet czasu, wolny serwer = ryzyko pobrania fragmentu albo rezygnacji. Opinie praktyków, ale spójne — dobre CWV załatwiają obie sprawy naraz.

### 1.3 Kolejność działań na WordPressie (największy zwrot najpierw)

1. **Hosting + full-page cache po stronie serwera + CDN.** Problemem WP jest LCP napędzane wysokim TTFB; cache serwuje wyrenderowany HTML bez odpalania PHP i bazy na każde żądanie. `[do weryfikacji — konkretne liczby typu „TTFB z 800 ms do <200 ms" pochodzą z blogów branżowych]`
2. **Obrazy:** WebP/AVIF; zawsze `width`/`height` lub `aspect-ratio` (przeciw CLS); na obrazie LCP `fetchpriority="high"` i **wyłączony lazy-load** (lazy-load na hero psuje LCP).
3. **Fonty:** self-hosting + `font-display: swap` + preload kluczowego kroju. W PL/UE self-hosting zamiast Google Fonts to także kwestia RODO — istotne wizerunkowo dla kancelarii.
4. **INP — najczęściej oblewana metryka.** Główny sprawca: skrypty third-party i długie zadania na main thread — baner cookie, chat, piksele marketingowe, page buildery. Ładuj tagi późno lub po zgodzie, dziel długie zadania, ogranicz JS buildera. Źródła: https://perfmatters.io/docs/interaction-to-next-paint/ ; https://web.dev/articles/vitals
5. **Ogranicz wtyczki** ładujące CSS/JS globalnie, gdy używane są na jednej podstronie. `[do weryfikacji — powszechna praktyka, brak twardego źródła Google]`

> **mentzen.pl:** najbardziej prawdopodobni sprawcy złego INP/CLS to baner zgód i widget kalendarza/rezerwacji (Calendesk). Zmierz je jako pierwsze, zanim ruszysz cokolwiek innego.

---

## 2. Indeksacja: robots.txt, noindex, canonical, sitemap

### 2.1 robots.txt

- **Steruje crawlowaniem, nie indeksowaniem** — Google: „it is not a mechanism for keeping a web page out of Google"; zablokowany URL może trafić do indeksu (bez opisu), jeśli ma linki z zewnątrz.
- **Nigdy nie łącz `Disallow` z `noindex` na tym samym URL-u** — crawler nie pobierze strony, więc nie zobaczy noindexu. To najczęstsza przyczyna „strona w indeksie mimo noindex".
- **Nie blokuj CSS/JS** potrzebnych do renderu — psuje ocenę mobile i treści.
- Źródła: https://developers.google.com/search/docs/crawling-indexing/robots/intro (2025-12-10); https://developers.google.com/search/docs/crawling-indexing/block-indexing (2025-12-10).

### 2.2 noindex

- `<meta name="robots" content="noindex">` albo nagłówek `X-Robots-Tag` (dla nie-HTML, np. PDF-ów z wzorami pism).
- Efekt z opóźnieniem (Google musi przecrawlować stronę) — tygodnie, czasem dłużej.
- **Nie używaj noindex do kanonikalizacji** — wycina stronę zamiast skonsolidować sygnały; do tego jest `rel=canonical`.

### 2.3 rel=canonical

- **Podpowiedź, nie dyrektywa** — Google wybiera sam. Siła sygnałów malejąco: **301 > rel=canonical > sitemap** (sitemap to „weak signal"). Sygnały stackuj i utrzymuj spójne.
- **Rób:** self-referencing canonical na każdej stronie, URL-e absolutne, spójność canonical ↔ sitemap ↔ linkowanie wewnętrzne.
- **Unikaj:** canonical do fragmentu (`#`), canonical wstrzykiwanego/modyfikowanego JS-em, innego URL-a w sitemapie niż w canonicalu, „kanonikalizacji" przez robots.txt lub narzędzie usuwania URL.
- Źródło: https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls (2026-07-10).

### 2.4 Sitemap XML

- **Tylko kanoniczne URL-e przeznaczone do indeksu.** Wyklucz noindexy, przekierowania, thin archives, paginację niskiej wartości.
- Limity: 50 MB / 50 000 URL na plik; powyżej — sitemap index.
- **`lastmod` tylko gdy „consistently and verifiably accurate"** — ma odzwierciedlać istotną zmianę treści, nie aktualizację stopki. **`priority` i `changefreq` Google ignoruje.**
- Zgłoszenie: GSC → Sitemaps + wpis w robots.txt. Źródło: https://developers.google.com/search/docs/crawling-indexing/sitemaps/build-sitemap (2026-07-08).
- **WordPress:** rdzeń generuje `/wp-sitemap.xml`, wtyczki SEO wystawiają własną i wyłączają rdzeniową — **upewnij się, że działa dokładnie jedna** i nie zawiera archiwów/tagów wyłączonych z indeksacji.

### 2.5 llms.txt — nie rób

- Google (dok. 2026-06-15): „You don't need to create new machine readable files, AI text files, markup, or Markdown to appear in Google Search (including its generative AI capabilities)"; Gary Illyes: brak wsparcia i planów. Ahrefs (137 tys. serwisów): 97% plików llms.txt nigdy nie zostało pobranych przez żadnego bota.
- To samo mówi Samo Cerar w kursie Ahrefs: OpenAI nie używa, Anthropic nie potwierdził, że jego crawlery czytają, Google nie zaadaptował — „robots.txt jest tym plikiem, który realnie działa".
- **Nie dodawaj llms.txt i nie traktuj go jako argumentu przy wyborze wtyczki SEO.**
- Źródła: https://www.searchenginejournal.com/google-says-llms-txt-is-purely-speculative-for-now/577576/ ; https://ahrefs.com/blog/llmstxt-study/ ; https://www.seroundtable.com/google-ai-llms-txt-39607.html

---

## 3. Crawl budget i crawl bloat WordPressa

**Klasyczny crawl budget mentzen.pl nie dotyczy.** Google adresuje przewodnik do serwisów 1 mln+ stron (zmiany co tydzień) albo 10 tys.+ (zmiany codzienne) albo z masą URL-i „Discovered – currently not indexed"; mniejsze serwisy nie muszą tego optymalizować. Źródło: https://developers.google.com/search/docs/crawling-indexing/large-site-managing-crawl-budget (2026-07-22).

Co z tego mimo wszystko stosować:

- **Sprawdź w GSC → Ustawienia → Statystyki indeksowania**, czy Googlebot nie przepala żądań na archiwach, feedach, `?s=` i parametrach — to realny problem nawet przy małym serwisie.
- 404/410 dla trwale usuniętych, naprawa **soft 404** (strona 200 z treścią „brak wyników" — puste archiwum tagu, pusta strona `?s=`; Google klasyfikuje w raporcie Page Indexing), krótkie przekierowania bez łańcuchów, obsługa **HTTP 304** (If-Modified-Since). Źródła: https://developers.google.com/search/docs/crawling-indexing/troubleshoot-crawling-errors ; https://searchengineland.com/soft-404s-indexing-issues-traffic-collapse-477116
- **`noindex` nie oszczędza crawl budgetu** — Google i tak pobiera stronę, żeby zobaczyć regułę.

### Crawl bloat rdzenia WP — decyzje per typ strony

Rekomendacje to praktyka branżowa, nie dokumentacja Google `[do weryfikacji co do siły efektu]`; źródła: https://www.joinindexed.com/blog/technical-seo-for-wordpress-the-settings-plugins-and-fixes-that-actually-matter ; https://redshaw.consulting/rsc-seo-suite/wordpress-seo/wordpress-foundations/wordpress-seo-problems/

| Typ URL-a | Decyzja | Dlaczego |
|---|---|---|
| Attachment pages | przekieruj na plik lub wpis nadrzędny (przełącznik w Yoast/Rank Math) | thin content bez intencji wyszukiwawczej |
| Archiwa dat | `noindex` | prawie nigdy brak intencji |
| Archiwa autorów | **indeksuj** — przy wielu ekspertach kancelarii to nośnik E-E-A-T; dodaj bio i specjalizację, żeby nie były soft 404 | przy jednym autorze byłby to duplikat bloga → wtedy `noindex` |
| Tagi | indeksuj wybiórczo; puste/1-wpisowe → `noindex` lub usuń | tag ≈ kategoria to kanibalizacja; tag z 1 wpisem to thin |
| `/?s=` (wyszukiwarka wewnętrzna) | **`noindex`, bez `Disallow`** — domyślnie. Sam `Disallow` (bez noindexu) tylko wtedy, gdy celem jest oszczędność crawl budgetu i akceptujesz ryzyko „zaindeksowane bez treści". **Nigdy oba naraz** (sekcja 2.1) | nieskończona przestrzeń URL-i, soft 404 |
| Facety/filtry/kombinacje parametrów | nie indeksuj; nie twórz kombinatorycznej przestrzeni (tag × autor × sort) | duplikaty tej samej listy |

Dodatkowo per Vestigio (webinar Senuto): strony autorów i sekcje „powiązane wpisy" działają jako wewnętrzne huby linkowe — bywają krótszą drogą bota do nowych URL-i niż paginacja kategorii; linkuj **ze starych wpisów do nowych**, bo boty stare i tak odwiedzają.

### Higiena URL i konfiguracji

- Permalinki `/%postname%/` (ew. jeden poziom kategorii), **bez dat w URL** — daty utrudniają aktualizowanie treści.
- Jedna wersja domeny: `https://` + (nie)`www`, reszta 301 **jednym skokiem**.
- **Staging poza indeksem: noindex + basic auth** (klasyczny błąd: staging w indeksie jako duplikat).
- `wp-json`, `xmlrpc.php`, `wp-admin` nie muszą być crawlowane (`wp-admin` jest w robots.txt WP domyślnie).

---

## 4. Dane strukturalne

### 4.1 Zasady twarde (Google, https://developers.google.com/search/docs/appearance/structured-data/sd-policies, 2026-07-10)

- Format rekomendowany: **JSON-LD**.
- **Oznaczaj wyłącznie treść widoczną dla użytkownika** — naruszenie grozi manual action.
- Strona z markupem nie może być zablokowana w robots.txt ani mieć noindex.
- Poprawny markup **nie gwarantuje** rich resultu. Wiele typów na stronie OK (zagnieżdżenie lub spięcie przez `@id`).
- Walidacja: Rich Results Test + URL Inspection w GSC.

### 4.2 Kalibracja oczekiwań — schema a AI (rozbieżność źródeł)

- Schema **nie jest czynnikiem rankingowym**; daje kwalifikowalność do rich results i pomaga Google rozumieć encje.
- **Eksperyment Ahrefs (1 885 stron): dodanie JSON-LD nie dało istotnego wzrostu cytowań w AI Overviews, AI Mode ani ChatGPT** (dla AIO nawet −4,6% vs kontrola). Korelacje obserwacyjne biorą się stąd, że serwisy ze schema robią dobrze resztę SEO. Źródło przez omówienia: https://www.stanventures.com/news/schema-markup-has-no-meaningful-impact-on-ai-citations-7231/ `[do weryfikacji — oryginalny URL badania Ahrefs niepotwierdzony]`. Zgodnie: Samo Cerar (kurs Ahrefs) — „brak potwierdzonych danych, że schema poprawia szanse na cytowanie przez AI; nie inwestuj w schema czasu przeznaczonego na SSR/robots/szybkość".
- **Odmiennie:** wg Dale'a Daviesa i Charlie Marchant z kanału Exposure Ninja (https://www.youtube.com/watch?v=vrGLaJOAKas) schema jest „teraz ważniejsza, bo systemy AI chętnie po nią sięgają" — to opinia agencyjna bez danych; eksperyment Ahrefs jej przeczy. Wg Vestigio (webinar Senuto, powołując się na Google): schema nie wpływa bezpośrednio na AIO, wpływa pośrednio przez klasyczny search.
- **Wniosek normatywny:** wdrażaj schema poprawnie i tanio (automatem z wtyczki, maksymalnie wypełnione właściwości, zagnieżdżanie), ale **nie sprzedawaj schema wewnętrznie jako dźwigni widoczności w AI**.

### 4.3 Co wdrożyć na mentzen.pl — typ po typie

| Typ | Decyzja | Kluczowe szczegóły |
|---|---|---|
| **Organization** | **Tak, priorytet** | `name`, `url`, `logo` (min. 112×112), `address`, `telephone`, `email`, `sameAs` (LinkedIn, YouTube, X), `contactPoint`, opcjonalnie `vatID`. **Na home albo „O nas" — nie na każdej podstronie.** Wpływa na logo/dane w SERP i knowledge panel. https://developers.google.com/search/docs/appearance/structured-data/organization (2026-04-15) |
| **LocalBusiness (najkonkretniejszy podtyp)** | **Tak, dla fizycznych biur** | Wymagane `name`+`address`; rekomendowane `geo` (≥5 miejsc po przecinku), `openingHoursSpecification`, `telephone`, `priceRange`. Google każe brać najbardziej szczegółowy podtyp, ale jako przykłady wymienia tylko Restaurant, DaySpa, HealthClub, Electrician, Plumber, Locksmith, Pharmacy — `LegalService`/`AccountingService`/`Attorney` to poprawne typy schema.org, których dokumentacja Google nie wymienia (sprawdzone u źródła 2026-08-28), więc bez gwarancji rich resultu. Realny efekt lokalny robi Profil Firmy w Google, nie markup. https://developers.google.com/search/docs/appearance/structured-data/local-business (2025-12-10) |
| **Article / BlogPosting** | **Tak, blog** | `author` jako osobne obiekty `Person` per autor (nie string zbiorczy), `author.url`/`sameAs` → strona autora, `datePublished`/`dateModified` ISO 8601 ze strefą. **W `name` autora tylko imię i nazwisko — bez „adw.", „doradca podatkowy", bez nazwy firmy.** https://developers.google.com/search/docs/appearance/structured-data/article (2025-12-10) |
| **BreadcrumbList** | **Tak, wszędzie** | ≥2 `ListItem` z `position`/`name`/`item` (ostatni bez `item`); odwzoruj typową ścieżkę użytkownika, nie strukturę URL. https://developers.google.com/search/docs/appearance/structured-data/breadcrumb (2025-12-10) |
| **ProfilePage / Person** | **Tak, strony zespołu** | Wymagane `mainEntity` (Person + `name`). Google wymienia „employee pages on company websites". Wartość: encja autora pod E-E-A-T, nie rich result. https://developers.google.com/search/docs/appearance/structured-data/profile-page (2025-12-10) |
| **Service** | Opcjonalnie, niski priorytet | Nie występuje w Search Gallery (2026-06-15) — tylko sygnał semantyczny z `provider` → Organization, bez oczekiwań SERP. |
| **FAQPage** | **Nie wdrażaj nowego** | Rich result wyłączony 2026-05-07 (wcześniej od 2023 tylko dla stron rządowych/zdrowotnych). Istniejący markup nie szkodzi — zostaw; sekcje FAQ zachowaj jako UX/content. https://developers.google.com/search/docs/appearance/structured-data/faqpage ; https://www.searchenginejournal.com/google-drops-faq-rich-results-from-search/574429/ |
| **Review / AggregateRating** | **Ostrożnie / nie** | Google: „If the entity that's being reviewed controls the reviews about itself, their pages that use `LocalBusiness` or any other type of `Organization` structured data are ineligible for star review feature" — dotyczy też opinii wstawianych widgetem third-party na własnej stronie. Czyli nie kara, tylko brak kwalifikowalności do gwiazdek; fałszywe oceny to osobno naruszenie polityk. Dla kancelarii korzyść znika, ryzyko zostaje. https://developers.google.com/search/docs/appearance/structured-data/review-snippet |

---

## 5. Mobile i HTTPS

- **Mobile-first indexing ukończony 2023-10-31** — Google indeksuje cały web crawlerem mobilnym. **Konsekwencja: treść, linki wewnętrzne i schema nieobecne w wersji mobilnej dla Google nie istnieją.** Sprawdź, czy motyw nie usuwa sekcji z DOM na mobile i czy accordiony renderują treść w HTML. Źródło: https://developers.google.com/search/blog/2023/10/mobile-first-is-here
- Unikaj nachalnych interstitiali na mobile (popup newslettera nad treścią, pełnoekranowy baner cookie) — element self-assessmentu page experience, choć bez bezpośredniego wpływu rankingowego.
- **HTTPS:** cały serwis, HSTS, 301 z HTTP jednym skokiem, zero mixed content — z powodów bezpieczeństwa i zaufania (formularze z danymi w kancelarii), **nie licz na efekt rankingowy** (https://developers.google.com/search/docs/appearance/page-experience, 2025-12-10).

---

## 6. Paginacja i taksonomie

Źródło paginacji: https://developers.google.com/search/docs/specialty/ecommerce/pagination-and-incremental-page-loading (2025-12-10).

- **Każda strona paginacji: własny URL** (`/page/n/`, nie fragment `#` — Google go ignoruje) **i własny self-referencing canonical. Nie ustawiaj strony 1 jako kanonicznej dla serii** — Google mówi to wprost.
- `rel=next/prev` Google nie używa od 2019 (Bing tak — istniejące zostaw, nowych nie dodawaj).
- **Linki paginacji jako zwykłe `<a href>` w HTML renderowanym serwerowo.** Infinite scroll / „Load more": Googlebot nie klika przycisków — zapewnij równoległą paginację URL-ową.
- Tytuły stron paginacji różnicuj („— strona 2") `[do weryfikacji — praktyka, nie dokumentacja]`.
- **Kategorie = pełnoprawne landing pages:** unikalny wstęp opisowy (np. „Podatek u źródła", „Sukcesja firm") — to naturalne strony pod frazy tematyczne B2B. Indeksuj.
- **Facety/filtry:** blokuj duplikujące się zestawy wyników (noindex albo robots.txt).

---

## 7. Warstwa AI-crawlerów (nowość względem klasycznej checklisty)

Materiał głównie z kursu Ahrefs (Samo Cerar, https://www.youtube.com/watch?v=uza9GX0E2mw — dane z badań własnych Ahrefs, nierecenzowane) i webinaru Senuto/Vestigio (Szymon Parzych, https://www.youtube.com/watch?v=MKwi0qmVSS8):

1. **robots.txt vs boty AI — kontrola 5-minutowa.** Sprawdź na `mentzen.pl/robots.txt` cztery nazwy: `GPTBot`, `OAI-SearchBot` (OpenAI), `ClaudeBot` (Anthropic), `Google-Extended`. Wg danych Ahrefs ~5,9% ze 140 mln witryn blokuje GPTBot, często nieświadomie — typowy winowajca to **Cloudflare z domyślnie włączoną funkcją „instruct AI bot traffic with robots.txt"**, która sama dopisuje reguły. Sprawdź też blokady poza robots.txt: WAF/hosting muszą zwracać 200 dla oficjalnych botów.
2. **JavaScript.** Wg kursu Ahrefs: Gemini i Copilot renderują JS, **crawler ChatGPT nie** — treść doładowywana klientem jest dla niego pustą skorupą. Wg Vestigio ostrzej: „roboty AI nie wykonują JavaScriptu" w ogóle. Rozbieżność w szczegółach, wniosek wspólny: **istotna treść (artykuł, title, meta, nawigacja) w HTML serwerowym**; po stronie klienta zostaw tylko rzeczy nieistotne dla cytowania (chat, liczniki). Test: wyłącz JS w przeglądarce — jeśli treść znika, jest problem. Na klasycznym WP z motywem renderowanym serwerowo to zwykle spełnione; uważaj na sekcje budowane JS-em przez page builder.
3. **Dyrektywy snippetów.** `nosnippet`, `data-nosnippet`, `max-snippet` blokują fragmenty, a przez to obecność w AI Overviews — zweryfikuj, czy nikt nie wdrożył ich „na wszelki wypadek" (wg Vestigio).
4. **Czysta hierarchia nagłówków** (H1 → H2 → H3, akapit = jedna myśl): modele parsują wzdłuż struktury HTML i tną tekst na granicach nagłówków — struktura to wymóg techniczny, nie stylistyczny (wg kursu Ahrefs).
5. **Halucynowane URL-e.** Dane Ahrefs: asystenci AI kierują na 404 2,87× częściej niż Google (ChatGPT ~1% klikniętych URL-i). **Rób:** monitoruj w analityce 404 z referrerów AI i przekierowuj na najbliższą istniejącą stronę.
6. Wg autora kursu Surfer Academy (https://www.youtube.com/watch?v=7DRO4rEIHDk): strona niewidoczna w Google/Bing jest z dużym prawdopodobieństwem niewidoczna dla asystentów (ChatGPT opiera się na indeksie Bing) — klasyczne techniczne SEO **jest** technicznym AEO; nie kupuj „GEO" jako osobnej usługi (opinia sprzedawcy narzędzia, ale spójna z mechaniką systemów).

---

## 8. Wtyczka SEO i narzędzia

- **Dokładnie jedna wtyczka SEO** — dwie generują konflikt canonicali, zdublowane metatagi i podwójny graf schema.
- Yoast (stabilność, spójny „unified graph" schema, ~10 mln instalacji) albo Rank Math (najwięcej funkcji za darmo, 25+ typów schema, ~3 mln); SEOPress dla lubiących kontrolę. **Kryterium decyzji: jakość grafu schema + kontrola indeksacji archiwów** — to jedyne dwie rzeczy, które wtyczka realnie robi za ciebie; „zielone kropki" to narzędzie redakcyjne, nie technika. `[do weryfikacji — porównanie ze źródeł średniozaufanych i materiałów producentów]`
- **Po migracji między wtyczkami zawsze zweryfikuj:** canonicale, noindexy archiwów, sitemapę — najczęstsze miejsca rozjazdu.
- **Audyt crawlerem (Screaming Frog, darmowy do 500 URL-i)** — zepsute linki, brakujące title, duplikaty, łańcuchy 301, obrazy bez alt — wg autora kursu Surfer Academy; wg Exposure Ninja audyt techniczny **co najmniej kwartalnie**, bo błędy narastają naturalnie (nowe treści, landingi kampanii), a agenci AI porzucają strony z błędami 404/403/pętlami przekierowań.
- Zarazem (Surfer Academy): **nie wpadaj w króliczą norę technicznego SEO** — na WordPressie jesteś w ~90% na miejscu; blokadą małej firmy jest brak treści, nie technikalia.

---

## 9. Checklista wdrożeniowa (do przeklejenia)

**Indeksacja**
- [ ] Jedna wersja domeny, HTTPS+HSTS, 301 jednym skokiem, zero mixed content
- [ ] Dokładnie jedna sitemapa XML: tylko kanoniczne URL-e, bez priority/changefreq, wiarygodny lastmod
- [ ] robots.txt nie blokuje CSS/JS; nigdzie Disallow+noindex razem
- [ ] Self-referencing canonical wszędzie, spójny z sitemapą i linkowaniem
- [ ] Staging: noindex + basic auth

**WordPress**
- [ ] Attachment pages przekierowane; archiwa dat noindex
- [ ] Archiwa autorów indeksowane z realnym bio (E-E-A-T)
- [ ] `/?s=` noindex bez Disallow (sam Disallow tylko zamiast noindexu, gdy liczy się crawl budget); puste tagi usunięte/noindex
- [ ] Jedna wtyczka SEO; po zmianach zweryfikowane canonicale/noindexy/sitemapa

**Wydajność**
- [ ] Full-page cache serwerowy + CDN
- [ ] Obraz LCP: bez lazy-load, fetchpriority="high", WebP/AVIF; wszystkie obrazy z width/height
- [ ] Fonty self-hosted, font-display: swap
- [ ] Third-party (baner zgód, chat, piksele, Calendesk) odroczone/po zgodzie — pod INP
- [ ] Pomiar: CrUX/GSC, p75, mobile osobno

**Dane strukturalne**
- [ ] Organization (home/„O nas"), LocalBusiness dla biur, Article z autorem-Person, BreadcrumbList wszędzie, ProfilePage dla zespołu
- [ ] Bez nowego FAQPage; bez self-serving Review
- [ ] Walidacja: Rich Results Test + URL Inspection

**AI-crawlery**
- [ ] robots.txt/Cloudflare/WAF nie blokują GPTBot, OAI-SearchBot, ClaudeBot, Google-Extended
- [ ] Istotna treść widoczna przy wyłączonym JS
- [ ] Brak nosnippet/data-nosnippet/max-snippet wdrożonych „na wszelki wypadek"
- [ ] Monitoring 404 z referrerów AI + przekierowania

**Paginacja/taksonomie**
- [ ] Self-canonical na każdej stronie paginacji (strona 1 NIE kanoniczna dla serii); linki `<a href>` w HTML serwerowym
- [ ] Kategorie z opisem jako landing pages; tagi wybiórczo; facety nieindeksowane

---

## 10. Zastosowanie dla mentzen.pl — priorytety w kolejności

1. **Audyt crawl bloatu:** GSC Statystyki indeksowania + crawl Screaming Frogiem; decyzje per typ strony z sekcji 3 (szczególnie archiwa autorów → indeksować i rozbudować, bo kancelaria ma wielu ekspertów-nośników E-E-A-T).
2. **Pomiar INP/CLS na danych polowych** z podejrzeniem na baner zgód i Calendesk; potem cache serwerowy, jeśli TTFB wysoki.
3. **Graf schema przez wtyczkę:** Organization + LocalBusiness (biura) + Article z autorami-Person + BreadcrumbList + ProfilePage zespołu. Bez FAQPage, bez Review, bez oczekiwań co do AI.
4. **Kontrola AI-crawlerów** (robots.txt/Cloudflare, test bez JS, snippet-dyrektywy) — 30 minut, jednorazowo + przy każdej zmianie infrastruktury.
5. **Kategorie bloga jako landing pages** z opisami pod frazy tematyczne B2B („podatek u źródła", „sukcesja firm", „fundacja rodzinna") — to jest miejsce, gdzie techniczne SEO styka się z architekturą treści; wg Exposure Ninja typowa patologia to blog-śmietnik z setkami wpisów bez kategoryzacji i powiązania z ofertą — naprawa architektury jest tańsza niż kolejne 50 wpisów.
6. **Rytm:** audyt techniczny kwartalnie (wg Exposure Ninja); po każdej migracji wtyczek — weryfikacja canonicali/noindexów/sitemapy.

## 11. Do dalszego sprawdzenia

- Oryginalny URL badania Ahrefs schema vs cytowania AI (tu przez omówienia).
- `LegalService`/`AccountingService`/`Attorney` w rich results — sprawdzone 2026-08-28: dokumentacja LocalBusiness ich nie wymienia (patrz 4.3), temat zamknięty.
- Self-serving reviews — klauzula zacytowana w 4.3 z dokumentacji Review snippet, temat zamknięty.
- `/?s=`: `noindex` vs `Disallow` — rozstrzygnięte zgodnie z regułą z 2.1: domyślnie sam `noindex`, `Disallow` wyłącznie jako alternatywa pod crawl budget (patrz 3 i 9), temat zamknięty.
- Aktualne zachowanie Yoast/Rank Math/SEOPress w zakresie grafu schema (porównanie ze źródeł średniozaufanych).
- Rozbieżność „które crawlery AI renderują JS" (Ahrefs: Gemini/Copilot tak; Vestigio: żadne) — zweryfikować u dostawców przed inwestycją w SSR ponad to, co WP daje domyślnie.
