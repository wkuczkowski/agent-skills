# Techniczne SEO w praktyce — checklista dla mentzen.pl (WordPress, B2B, Google.pl)

Research: 2026-08-28. Kontekst: kancelaria (prawo, doradztwo podatkowe, księgowość), rynek PL, WordPress.
Konwencja: **rób X / unikaj Y** + krótkie *dlaczego*. Twierdzenia bez potwierdzenia w źródle oznaczone `[do weryfikacji]`.

---

## 0. Największa zmiana, o której trzeba wiedzieć (stan na 2026-08)

**FAQPage rich results nie istnieją już w Google.** Dokumentacja FAQPage dostała notkę deprecation, a sama funkcja przestała pojawiać się w wynikach 7 maja 2026; typ zniknął też z listy obsługiwanych rich resultów w Search Gallery (pobrana 2026-08-28 lista 25 typów nie zawiera FAQ). Wcześniej, od sierpnia/września 2023, FAQ rich results i tak były ograniczone do „well-known, authoritative government and health websites".

- **Nie buduj** strategii treści FAQ pod rich results w Google — nie ma tam czego zdobyć.
- **Nie musisz** usuwać istniejącego markupu FAQPage: to nadal poprawny typ schema.org, a Google deklaruje, że nieużywane dane strukturalne nie szkodzą.
- **Zachowaj** sekcje FAQ jako treść dla użytkownika (i potencjalnie dla AI/asystentów) — ale traktuj to jako UX/content, nie jako technikę SERP.

Źródła: https://developers.google.com/search/docs/appearance/structured-data/faqpage (notka deprecation, „This feature will no longer appear in Google Search starting May 7, 2026"); https://developers.google.com/search/docs/appearance/structured-data/search-gallery (aktualizacja 2026-06-15, brak FAQ na liście); https://www.searchenginejournal.com/google-drops-faq-rich-results-from-search/574429/

---

## 1. Core Web Vitals i wydajność WordPressa

### 1.1 Progi i sposób pomiaru

| Metryka | „Good" | Co mierzy |
|---|---|---|
| LCP (Largest Contentful Paint) | ≤ 2,5 s | szybkość ładowania głównego elementu |
| INP (Interaction to Next Paint) | ≤ 200 ms | responsywność na interakcje |
| CLS (Cumulative Layout Shift) | ≤ 0,1 | stabilność wizualna |

- **Mierz na 75. percentylu** realnych wizyt (dane polowe CrUX), osobno mobile i desktop. Lab (Lighthouse) służy do diagnozy, nie do oceny „zdania testu". *Dlaczego:* Google ocenia CWV z danych polowych, nie z syntetyki.
- INP zastąpił FID w 2024 i jest metryką stabilną. *Źródło:* https://web.dev/articles/vitals (status metryk „stable", 2024-10-31).

### 1.2 Jak mocno to waży w rankingu — nie przesadzaj

- Google: *„Core Web Vitals are used by our ranking systems"*, ale też: *„There is no single signal. Our core ranking systems look at a variety of signals that align with overall page experience"* oraz *„getting good results in reports like Search Console's Core Web Vitals report… doesn't guarantee that your pages will rank at the top"*.
- Bardzo istotne dla kancelarii: **poza CWV pozostałe aspekty page experience (HTTPS, brak nachalnych interstitiali) nie podbijają rankingu bezpośrednio** — Google: *„Beyond Core Web Vitals, other page experience aspects don't directly help your website rank higher in search results"*. HTTPS robisz dla bezpieczeństwa i zaufania, nie dla pozycji.
- **Wniosek normatywny:** traktuj CWV jako higienę i czynnik konwersji, a nie jako dźwignię rankingową. Nie przepalaj budżetu na walkę o 95/100 w Lighthouse kosztem treści i E-E-A-T.
- *Źródło:* https://developers.google.com/search/docs/appearance/page-experience (aktualizacja 2025-12-10).

### 1.3 Checklista WordPress — kolejność działań (największy zwrot najpierw)

1. **Hosting + full-page cache po stronie serwera.** *Dlaczego:* problemem WordPressa jest LCP napędzane wysokim TTFB — cache zapisuje wyrenderowany HTML, więc PHP i baza nie odpalają się na każde żądanie. Zestaw „shared hosting bez cache → managed hosting z cache serwerowym + CDN" to zwykle najmocniejsza pojedyncza zmiana. [do weryfikacji — konkretne liczby „TTFB z 800 ms+ do <200 ms" pochodzą z blogów branżowych, nie ze źródeł wysokozaufanych]
2. **Obrazy:** nowoczesne formaty (WebP/AVIF), poprawne `width`/`height` lub `aspect-ratio` (przeciw CLS), `fetchpriority="high"` na obrazie LCP, lazy-load **wyłączony** dla obrazu LCP. *Dlaczego:* lazy-load na hero psuje LCP.
3. **Fonty:** self-hosting + `font-display: swap` + preload kluczowego kroju. *Dlaczego:* FOIT i późne fonty generują CLS i opóźniają LCP. Uwaga RODO: self-hosting zamiast Google Fonts jest w PL/UE także kwestią prawną — istotne dla kancelarii.
4. **INP:** to najczęściej oblewana metryka. Główne źródło problemu to skrypty third-party i długie zadania na main thread — banery cookie, chaty, piksele marketingowe, heavy page buildery. Rób: ładuj tagi analityczne/marketingowe późno lub po zgodzie, dziel długie zadania, ogranicz JS z buildera. *Źródła:* https://perfmatters.io/docs/interaction-to-next-paint/ ; https://web.dev/articles/vitals
5. **Ogranicz liczbę wtyczek** i wywal te ładujące CSS/JS na wszystkich podstronach, a używane na jednej. [do weryfikacji — powszechna praktyka, brak twardego źródła Google]

> Uwaga dla mentzen.pl: baner zgód i ewentualny widget kalendarza/rezerwacji (Calendesk pojawia się w projekcie) to najbardziej prawdopodobni sprawcy złego INP i CLS. Zmierz je najpierw.

---

## 2. Indeksacja: sitemap, robots.txt, canonical, noindex

### 2.1 robots.txt — co potrafi, a czego nie

- **robots.txt steruje crawlowaniem, nie indeksowaniem.** Google wprost: *„it is not a mechanism for keeping a web page out of Google"* — zablokowany URL może trafić do wyników (bez opisu), jeśli prowadzą do niego linki z zewnątrz.
- **Nigdy nie łącz** `Disallow` w robots.txt z `noindex` na stronie. Crawler nie zobaczy `noindex`, bo nie pobierze strony. To najczęstszy błąd „strona wciąż w indeksie mimo noindex".
- **Nie blokuj** CSS/JS potrzebnych do renderu strony. *Dlaczego:* Google renderuje strony; zablokowane zasoby psują ocenę mobile i treści.
- *Źródła:* https://developers.google.com/search/docs/crawling-indexing/robots/intro (2025-12-10); https://developers.google.com/search/docs/crawling-indexing/block-indexing (2025-12-10).

### 2.2 noindex

- Implementuj przez `<meta name="robots" content="noindex">` albo nagłówek `X-Robots-Tag` (dla nie-HTML, np. PDF-ów z wzorami pism).
- **Licz się z opóźnieniem** — Google musi stronę przecrawlować, żeby zobaczyć regułę; efekt bywa widoczny po tygodniach/miesiącach.
- **Nie używaj `noindex` do kanonikalizacji wewnątrz serwisu** — Google preferuje `rel="canonical"`. *Dlaczego:* `noindex` wycina stronę, zamiast skonsolidować sygnały.

### 2.3 rel=canonical

- **Kanoniczny to podpowiedź, nie dyrektywa.** Google wybiera ostatecznie sam; jeśli nie wskażesz, wybierze wersję, którą uzna za najlepszą.
- **Siła sygnałów (malejąco): przekierowania 301 > `rel=canonical` > sitemap.** Sitemap to „weak signal". Warto sygnały stackować — muszą być spójne.
- **Rób:** self-referencing canonical na każdej stronie, URL-e absolutne, spójność canonical ↔ sitemap ↔ linkowanie wewnętrzne.
- **Unikaj:** canonical do fragmentu (`#`), canonical wstrzykiwanego JS-em i potem modyfikowanego, wskazywania w sitemapie innego URL-a niż w canonicalu, próby kanonikalizacji przez robots.txt lub narzędzie usuwania URL.
- *Źródło:* https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls (2026-07-10).

### 2.4 Sitemap XML

- **Wrzucaj wyłącznie kanoniczne URL-e, które chcesz w indeksie.** Wyklucz: noindex, przekierowania, thin archives, paginację niskiej wartości.
- Limity: **50 MB (nieskompresowane) lub 50 000 URL** na plik; powyżej → sitemap index.
- **`<lastmod>` używaj tylko wtedy, gdy jest wiarygodny** — Google bierze go pod uwagę tylko jeśli jest „consistently and verifiably accurate"; ma odzwierciedlać istotną zmianę treści/danych strukturalnych/linków, a nie aktualizację stopki czy roku w copyrighcie.
- **`<priority>` i `<changefreq>` Google ignoruje** — nie trać na nie czasu.
- URL-e absolutne, pełne. Zgłoszenie: Search Console → Sitemaps, plus wpis w robots.txt.
- *Źródło:* https://developers.google.com/search/docs/crawling-indexing/sitemaps/build-sitemap (2026-07-08).

> WordPress: rdzeń generuje własną sitemapę (`/wp-sitemap.xml`); wtyczki SEO wystawiają własną i wyłączają rdzeniową. **Upewnij się, że działa dokładnie jedna** i że nie zawiera archiwów/tagów, których nie indeksujesz.

### 2.5 llms.txt — nie rób

- Gary Illyes: Google nie wspiera llms.txt i nie planuje. Dokumentacja Google (akt. 2026-06-15): *„You don't need to create new machine readable files, AI text files, markup, or Markdown to appear in Google Search (including its generative AI capabilities), as Google Search itself doesn't use them."*
- Ahrefs (analiza 137 tys. serwisów): 97% plików llms.txt nigdy nie zostało pobranych przez żadnego bota.
- **Wniosek:** nie dodawaj llms.txt „na wszelki wypadek" i nie kupuj tej funkcji jako argumentu przy wyborze wtyczki.
- *Źródła:* https://www.searchenginejournal.com/google-says-llms-txt-is-purely-speculative-for-now/577576/ ; https://ahrefs.com/blog/llmstxt-study/ ; https://www.seroundtable.com/google-ai-llms-txt-39607.html

---

## 3. Crawl budget — czy mentzen.pl to w ogóle dotyczy?

**Prawdopodobnie nie w klasycznym sensie.** Google definiuje adresatów przewodnika jako:

- serwisy bardzo duże (**1 mln+ unikalnych stron**, aktualizowane co najmniej co tydzień),
- serwisy średnie/większe (**10 tys.+ stron**) z codziennymi zmianami,
- serwisy z dużą liczbą URL-i w statusie **„Discovered – currently not indexed"**.

Google wprost: mniejsze, wolno zmieniające się serwisy nie muszą tego optymalizować.

- **Rób:** sprawdź w GSC → Ustawienia → Statystyki indeksowania, czy Googlebot nie przepala żądań na archiwach, feedach, `?s=` i parametrach. Jeśli tak — to realny problem nawet przy małym serwisie.
- **Mechanika:** crawl budget = *crawl capacity limit* (ile serwer wytrzyma; rośnie, gdy odpowiada szybko) × *crawl demand* (jak bardzo Google chce cię crawlować — rozmiar, częstotliwość zmian, jakość, popularność).
- **Dobre praktyki (Google):** konsoliduj duplikaty; blokuj w robots.txt strony nieistotne; zwracaj **404/410** dla trwale usuniętych; naprawiaj **soft 404**; utrzymuj aktualną sitemapę; unikaj długich łańcuchów przekierowań; przyspieszaj stronę; obsługuj **HTTP 304** (If-Modified-Since).
- **Czego NIE robić:** `noindex` **nie oszczędza** crawl budgetu — Google i tak pobiera stronę, żeby zobaczyć regułę. Blokowanie w robots.txt nie „przekieruje" budżetu na inne strony, chyba że faktycznie stoisz pod limitem pojemności; zablokowane URL-e wiszą w kolejce dłużej niż 404.
- **Tylko dwa sposoby na zwiększenie budżetu:** więcej zasobów serwerowych albo lepsza jakość/wartość treści.
- *Źródło:* https://developers.google.com/search/docs/crawling-indexing/large-site-managing-crawl-budget (2026-07-22).

**Soft 404 — osobno warto pilnować.** Strona zwraca 200, a treścią jest „brak wyników / nic tu nie ma" (puste archiwum tagu, pusta strona wyników wyszukiwarki wewnętrznej). Google klasyfikuje to jako soft 404 w raporcie Page Indexing. Napraw treścią albo zwróć 404/410.
*Źródła:* https://developers.google.com/search/docs/crawling-indexing/troubleshoot-crawling-errors ; https://searchengineland.com/soft-404s-indexing-issues-traffic-collapse-477116

---

## 4. Dane strukturalne — co ma sens dla kancelarii i jaki jest realny wpływ

### 4.1 Zasady ogólne (obowiązkowe do przestrzegania)

- **JSON-LD** to format rekomendowany przez Google (Microdata i RDFa też są obsługiwane).
- **Oznaczaj tylko treść widoczną dla użytkownika** — *„Don't mark up content that is not visible to readers of the page"*. Naruszenie = manual action odbierający kwalifikowalność do rich resultów.
- Strona z danymi strukturalnymi **nie może być zablokowana w robots.txt ani mieć `noindex`**.
- **Google nie gwarantuje wyświetlenia rich resultu**, nawet przy poprawnym markupie.
- Wiele typów na jednej stronie jest OK — zagnieżdżone pod jednym elementem głównym albo osobne bloki spięte przez `@id`.
- *Źródło:* https://developers.google.com/search/docs/appearance/structured-data/sd-policies (2026-07-10).

### 4.2 Realny wpływ — kalibracja oczekiwań

- Schema **nie jest** czynnikiem rankingowym; daje kwalifikowalność do wyglądu wyniku (rich result / knowledge panel) i pomaga Google rozumieć encje.
- Eksperyment Ahrefs na 1 885 stronach: dodanie JSON-LD **nie dało istotnego wzrostu cytowań w AI Overviews, AI Mode ani ChatGPT** (dla AIO nawet ‑4,6% względem grupy kontrolnej), mimo silnej korelacji w danych obserwacyjnych. Korelacja bierze się stąd, że serwisy wdrażające schema robią też resztę SEO dobrze.
- **Wniosek normatywny:** wdrażaj schema poprawnie i tanio (automatycznie z wtyczki), ale **nie sprzedawaj klientowi wewnętrznemu schema jako dźwigni widoczności w AI**.
- *Źródła:* https://ahrefs.com/blog/ (badanie schema vs. AI citations, 2026) — oryginał przez omówienia: https://www.searchenginejournal.com/ ; https://www.stanventures.com/news/schema-markup-has-no-meaningful-impact-on-ai-citations-7231/ [do weryfikacji — oryginalny URL badania Ahrefs nie został potwierdzony w tym researchu]

### 4.3 Typ po typie — co wdrożyć na mentzen.pl

| Typ | Wdrażać? | Uwagi |
|---|---|---|
| **Organization** | **Tak, priorytet** | Brak właściwości wymaganych; dodaj `name`, `url`, `logo` (min. 112×112 px), `address`, `telephone`, `email`, `description`, `sameAs` (LinkedIn, YouTube, X), `contactPoint`, opcjonalnie `vatID`/`foundingDate`. **Umieszczaj na home albo na jednej stronie „O nas" — nie trzeba na każdej podstronie.** Wpływa na to, jakie logo i jakie dane Google pokaże w wynikach i w knowledge panelu. Źródło: https://developers.google.com/search/docs/appearance/structured-data/organization (2026-04-15) |
| **LocalBusiness / ProfessionalService / LegalService / AccountingService** | **Tak, jeśli są fizyczne biura** | Wymagane tylko `address` i `name`; rekomendowane `geo` (≥5 miejsc po przecinku), `openingHoursSpecification`, `telephone`, `url`, `priceRange`. Google każe używać **najbardziej szczegółowego podtypu** LocalBusiness. Uwaga: dokumentacja Google nie wymienia wprost `LegalService`/`AccountingService`/`Attorney` wśród przykładów — to poprawne typy schema.org, ale bez gwarancji rich resultu `[do weryfikacji]`. Realny efekt lokalny bierze się głównie z **Profilu Firmy w Google**, nie z markupu. Źródło: https://developers.google.com/search/docs/appearance/structured-data/local-business (2025-12-10) |
| **Article / BlogPosting** | **Tak, dla bloga** | Brak właściwości wymaganych. Kluczowe: `author` jako osobne pola `Person` dla każdego autora (nie sklejaj w jeden string), `author.url`/`sameAs` → strona autora, `datePublished` i `dateModified` w ISO 8601 ze strefą. **W polu `name` autora tylko imię i nazwisko — bez tytułów („doradca podatkowy", „adw.") i bez nazwy wydawcy.** Źródło: https://developers.google.com/search/docs/appearance/structured-data/article (2025-12-10) |
| **BreadcrumbList** | **Tak** | ≥2 `ListItem` z `position`, `name`, `item` (ostatni bez `item`). Zamienia ścieżkę URL w SERP na czytelną hierarchię. Można podać kilka ścieżek do tej samej strony jako tablicę — ale Google zaleca odwzorowanie **typowej ścieżki użytkownika**, nie struktury URL. Źródło: https://developers.google.com/search/docs/appearance/structured-data/breadcrumb (2025-12-10) |
| **Person / ProfilePage** | **Tak, na stronach zespołu** | `ProfilePage` z wymaganym `mainEntity` (Person/Organization + `name`). Google wymienia jako poprawny przypadek m.in. **„employee pages on company websites"** i strony autorów. Główne zastosowanie: funkcja Discussions and Forums — dla kancelarii wartość jest głównie w **budowaniu encji autora pod E-E-A-T**, nie w rich resulcie. Źródło: https://developers.google.com/search/docs/appearance/structured-data/profile-page (2025-12-10) |
| **Service** | **Opcjonalnie, niski priorytet** | `Service` **nie występuje na liście typów wspierających rich results** w Google Search Gallery (stan 2026-06-15). Wdrażaj tylko jako sygnał semantyczny opisujący usługę i powiązanie z `provider` (Organization) — bez oczekiwania efektu w SERP. |
| **FAQPage** | **Nie wdrażaj nowego** | Patrz sekcja 0. Istniejący markup zostaw. |
| **Review / AggregateRating** | **Ostrożnie** | Self-serving reviews i fałszywe oceny to naruszenie polityk (manual action). Dla kancelarii ryzyko reputacyjne i prawne > korzyść. `[do weryfikacji — konkretna klauzula o self-serving reviews nie została w tym researchu zacytowana z dokumentacji]` |

### 4.4 Testowanie

- Rich Results Test i URL Inspection w Search Console — walidacja. Pamiętaj: pozytywny wynik testu ≠ gwarancja wyświetlenia.

---

## 5. WordPress — typowe problemy techniczne

### 5.1 Crawl bloat generowany przez rdzeń

WordPress domyślnie tworzy archiwa: kategorie, tagi, autorzy, daty, taksonomie własne, plus **attachment pages** (osobny URL na każdy załadowany plik) i feedy. Przy kilkuset wpisach i szczodrym tagowaniu robi się z tego tysiące cienkich URL-i.

Rekomendacje (praktyka branżowa, nie dokumentacja Google — `[do weryfikacji]` co do siły efektu):

- **Attachment pages:** przekieruj na plik lub na wpis nadrzędny (obie duże wtyczki SEO mają przełącznik). *Dlaczego:* to thin content bez wartości wyszukiwawczej.
- **Archiwa dat:** `noindex`. Prawie nigdy nie mają intencji wyszukiwawczej.
- **Archiwa autorów:** przy jednym autorze `noindex` (duplikat strony głównej bloga). Przy wielu ekspertach w kancelarii — **indeksuj**, bo to nośnik E-E-A-T; ale dodaj do nich realną treść (bio, specjalizacja), żeby nie były soft 404.
- **Tagi:** indeksuj tylko te z realną treścią i intencją. Puste/1-wpisowe → `noindex` albo usuń.
- **Wyniki wyszukiwarki wewnętrznej (`/?s=`):** `noindex` + `Disallow`. *Dlaczego:* nieskończona przestrzeń URL-i i soft 404.
- **Weryfikuj w GSC**, na co Googlebot faktycznie zużywa żądania, zanim zaczniesz masowo noindeksować.
- *Źródła:* https://www.joinindexed.com/blog/technical-seo-for-wordpress-the-settings-plugins-and-fixes-that-actually-matter ; https://redshaw.consulting/rsc-seo-suite/wordpress-seo/wordpress-foundations/wordpress-seo-problems/

### 5.2 Higiena URL i konfiguracji

- Permalinki: `/%postname%/` (albo z jednym poziomem kategorii). Bez dat w URL — utrudniają aktualizowanie treści.
- Jedna wersja domeny: wybierz `https://` + (nie)`www` i przekieruj resztę **jednym skokiem 301**. Bez łańcuchów.
- Wyłącz indeksowanie stagingu (`noindex` + basic auth). Klasyczny błąd: staging w indeksie jako duplikat.
- `wp-json`, `xmlrpc.php`, `wp-admin` — nie muszą być crawlowane; `wp-admin` jest domyślnie w robots.txt WP.

### 5.3 Wtyczki SEO — krótkie porównanie

**Zasada nadrzędna: dokładnie jedna wtyczka SEO na serwis.** Dwie generują konflikty canonicali, zdublowane metatagi i podwójny graf schema.

| | Yoast SEO | Rank Math | SEOPress |
|---|---|---|---|
| Pozycja | najstarszy standard, największy ekosystem wsparcia, ~10 mln instalacji | najwięcej funkcji w darmowej wersji, ~3 mln instalacji | czysty kod, mniej „przewodnictwa", nastawiony na agencje/freelancerów |
| Schema | spójny „unified graph" — jeden połączony graf encji, generowany automatycznie | 25+ predefiniowanych typów, generator i szablony schema | konfigurowalne, mniej podpowiedzi |
| Kiedy wybrać | gdy priorytetem jest stabilność, przewidywalność i długie wsparcie | gdy chcesz dużo funkcji bez licencji i akceptujesz więcej ustawień | gdy zależy ci na lekkości i kontroli, a zespół wie, co klika |

- **Rekomendacja dla kancelarii:** Yoast albo Rank Math — decydująca jest **jakość i spójność grafu schema oraz kontrola nad indeksacją archiwów**, bo to jedyne dwie rzeczy, które wtyczka SEO realnie robi za ciebie. Reszta (analiza treści, „zielone kropki") to narzędzie redakcyjne, nie technika. `[do weryfikacji — porównanie oparte na źródłach o średnim zaufaniu i materiałach producentów; przed migracją zweryfikuj na aktualnych wersjach]`
- **Ignoruj llms.txt jako argument sprzedażowy wtyczki** (sekcja 2.5).
- **Migracja między wtyczkami:** Rank Math ma import z Yoast/AIOSEO; po imporcie **zawsze zweryfikuj canonicale, ustawienia noindex archiwów i sitemapę**, bo to najczęstsze miejsca rozjazdu.
- *Źródła:* https://www.searchenginejournal.com/wordpress-seo/wordpress-seo-checklist-get-ready-for-site-launch/ ; https://yoast.com/choosing-the-right-wordpress-seo-plugin-for-your-business-yoast-vs-rank-math/ ; https://rankmath.com/blog/best-schema-markup-plugins-for-wordpress/ (materiały producentów — traktuj jako stronnicze)

---

## 6. HTTPS i mobile

### 6.1 HTTPS

- **Rób:** cały serwis na HTTPS, HSTS, przekierowanie 301 z HTTP jednym skokiem, brak mixed content.
- **Ale nie licz na ranking:** Google mówi wprost, że poza CWV pozostałe elementy page experience — w tym HTTPS — nie podbijają pozycji bezpośrednio. HTTPS robisz z powodów bezpieczeństwa i zaufania (w kancelarii: również formularze kontaktowe z danymi).
- *Źródło:* https://developers.google.com/search/docs/appearance/page-experience (2025-12-10).

### 6.2 Mobile

- **Mobile-first indexing jest ukończony** — od 31 października 2023 Google indeksuje cały web crawlerem mobilnym; informacja o crawlerze indeksującym zniknęła z ustawień Search Console, a crawl desktopowy został ograniczony.
- **Konsekwencja normatywna:** **treść, linki wewnętrzne i dane strukturalne, których nie ma w wersji mobilnej, dla Google nie istnieją.** Sprawdź, czy motyw nie chowa sekcji na mobile przez `display:none` w sposób usuwający je z DOM, i czy accordiony renderują treść w HTML.
- Unikaj nachalnych interstitiali na mobile (baner cookie zajmujący ekran, popup newslettera nad treścią) — to element self-assessmentu page experience, choć bez bezpośredniego wpływu na ranking.
- *Źródła:* https://developers.google.com/search/blog/2023/10/mobile-first-is-here (2023-10-31); https://www.searchenginejournal.com/google-completes-switch-to-mobile-first-indexing/499810/

---

## 7. Paginacja i taksonomie

### 7.1 Paginacja (blog, archiwa kategorii)

Wytyczne Google (aktualizacja 2025-12-10, https://developers.google.com/search/docs/specialty/ecommerce/pagination-and-incremental-page-loading):

- **Każda strona paginacji ma mieć własny, unikalny URL** — parametr typu `?page=n` lub `/page/n/`. **Nie używaj fragmentu `#`** do numeru strony; Google go ignoruje i może pominąć crawl.
- **Każda strona paginacji ma własny, self-referencing canonical.** **Nie ustawiaj strony 1 jako kanonicznej dla całej serii** — Google mówi to wprost.
- **`rel="next"`/`rel="prev"` są przez Google nieużywane** (wycofane, komunikat z 21 marca 2019). Bing nadal z nich korzysta do discovery, więc jeśli już są — zostaw; nie ma powodu dodawać ich do nowych szablonów.
- **Linkuj sekwencyjnie zwykłymi `<a href>`** między kolejnymi stronami, opcjonalnie ze wszystkich do strony pierwszej (sygnał, że to główny landing kolekcji).
- **Linki paginacji muszą być w HTML renderowanym serwerowo**, nie dopiero po hydracji JS.
- **Infinite scroll / „Load more":** Googlebot nie klika przycisków i zwykle nie odpala funkcji JS. Jeśli używasz — zapewnij równoległą, crawlowalną paginację URL-ową i/lub sitemapę.
- **Tytuły stron paginacji różnicuj** (np. dopisek „— strona 2"), żeby nie wyglądały jak duplikat. `[do weryfikacji — praktyka branżowa, nie ma tego wprost w cytowanej dokumentacji]`

### 7.2 Taksonomie i filtrowanie

- **Kategorie:** traktuj jako pełnoprawne landing pages — nadaj im unikalny wstęp opisowy (np. „Podatek u źródła", „Sukcesja firm"), bo to naturalne strony pod frazy tematyczne B2B. Indeksuj.
- **Tagi:** indeksuj wybiórczo. Gdy tag ≈ kategoria → kanibalizacja; gdy tag ma 1–2 wpisy → thin/soft 404.
- **Warianty filtrowane / facety:** Google zaleca blokowanie duplikujących się zestawów wyników przez `noindex` albo robots.txt, żeby uniknąć indeksowania wariacji tej samej listy.
- **Nie twórz** kombinatorycznej przestrzeni URL-i (tag × autor × data × sort). To najprostszy sposób na przepalenie crawl budgetu i soft 404.

---

## 8. Checklista wdrożeniowa (skrót do przeklejenia)

**Indeksacja**
- [ ] Jedna wersja domeny, HTTPS, 301 jednym skokiem, brak mixed content
- [ ] Dokładnie jedna sitemapa XML, tylko kanoniczne URL-e, bez `priority`/`changefreq`, wiarygodny `lastmod`
- [ ] robots.txt nie blokuje CSS/JS; nigdzie `Disallow` + `noindex` na tym samym URL
- [ ] Self-referencing canonical wszędzie; canonical spójny z sitemapą i linkowaniem wewnętrznym
- [ ] Staging poza indeksem (noindex + basic auth)

**WordPress**
- [ ] Attachment pages przekierowane
- [ ] Archiwa dat `noindex`; archiwa autorów rozstrzygnięte świadomie (E-E-A-T vs. duplikat)
- [ ] `/?s=` noindex + disallow
- [ ] Puste tagi usunięte lub `noindex`
- [ ] Jedna wtyczka SEO; po migracji zweryfikowane canonicale, noindexy, sitemapa

**Wydajność**
- [ ] Cache serwerowy full-page + CDN
- [ ] Obraz LCP: bez lazy-load, `fetchpriority="high"`, WebP/AVIF
- [ ] Wszystkie obrazy z `width`/`height`
- [ ] Fonty self-hosted, `font-display: swap`
- [ ] Skrypty third-party (baner zgód, chat, piksele, widget rezerwacji) odroczone / po zgodzie — pod INP
- [ ] Pomiar na danych polowych (CrUX/GSC), p75, mobile osobno

**Dane strukturalne**
- [ ] Organization na home lub „O nas" (logo, sameAs, contactPoint)
- [ ] LocalBusiness/ProfessionalService dla biur (address, geo, openingHours)
- [ ] Article/BlogPosting z poprawnym `author` (Person + url) i datami ISO 8601
- [ ] BreadcrumbList na wszystkich podstronach
- [ ] ProfilePage/Person na stronach zespołu
- [ ] Bez nowego FAQPage
- [ ] Walidacja w Rich Results Test + URL Inspection

**Paginacja / taksonomie**
- [ ] Unikalny URL i self-canonical na każdej stronie paginacji (strona 1 NIE jest kanoniczna dla serii)
- [ ] Linki paginacji jako `<a href>` w HTML serwerowym
- [ ] Kategorie z własnym opisem; tagi indeksowane wybiórczo
- [ ] Facety/filtry nieindeksowane

---

## 9. Do dalszego sprawdzenia

- Oryginalny URL badania Ahrefs o schema vs. cytowania AI (tu opierałem się na omówieniach).
- Czy Google dokumentuje gdziekolwiek wprost obsługę podtypów `LegalService` / `AccountingService` / `Attorney` w rich resultach.
- Klauzule polityk Google dot. self-serving reviews / AggregateRating — do zacytowania wprost przed decyzją o wdrożeniu ocen.
- Wpływ zmian z Google I/O 2026 na pozostałe typy schema — sprawdzić changelog Search Central.
- Aktualne wersje i zachowanie Yoast / Rank Math / SEOPress w zakresie grafu schema (porównanie oparte na źródłach o średnim zaufaniu).
