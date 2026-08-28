# rsmpoland.pl — analiza SEO jako benchmark dla mentzen.pl

Data analizy: 2026-08-28
Metoda: `curl` z UA przeglądarkowym (parsowanie HTML, JSON-LD, nagłówków, linków), WebFetch na renderowanych stronach, analiza `sitemap.xml`. Brak dostępu do danych o ruchu i pozycjach — wnioski o widoczności opieram na strukturze serwisu, nie na metrykach.

> **Uwaga metodologiczna.** W trakcie pracy plik z sitemapą w współdzielonym katalogu roboczym został nadpisany przez równolegle działającą sesję (dane z `roedl.pl`). Wszystkie liczby w tej notatce przeliczyłem ponownie na zweryfikowanej kopii (`grep -o '<loc>https://[a-z.]*'` → wyłącznie `rsmpoland.pl`). Gdyby ktoś porównywał z surowymi logami: obowiązują liczby stąd.

---

## Podsumowanie w jednym akapicie

RSM Poland to przeciwieństwo Crido: **mały serwis (1 557 URL-i) z bardzo silnym E-E-A-T i bardzo dobrym on-page**, zamiast dużego serwisu z płytkim autorytetem. Przewagę budują trzema rzeczami: (1) **179 stron eksperckich** z numerami uprawnień zawodowych, spięte z artykułami przez `author.@id` w JSON-LD; (2) **format artykułu zoptymalizowany pod AI search** — ramka „Kluczowe informacje", nagłówki H2 w formie pytań, FAQ, checklisty; (3) **śródtekstowy blok CTA dobierany tematycznie do artykułu**, linkujący w głąb do konkretnych podusług, nie do strony głównej oferty. Ich słabości są równie pouczające: ~394 osierocone URL-e legacy z **pustymi tagami `<title>`**, sitemapa wskazująca w 100% na wersję bez `www` (same 301), `FAQPage` wdrożone jako 8 osobnych zakresów microdata zamiast jednego bloku i porzucona seria „Tax Alert".

---

## 1. Stack i skala

| Element | Wartość | Źródło |
|---|---|---|
| CMS | **Drupal** (`/core/`, `/profiles/`, `/sites/default/files/`, `themes/custom/rsm_theme`) | `robots.txt`, ścieżki assetów, 2026-08-28 |
| Łączna liczba URL w sitemapie | **1 557** (jedna płaska sitemapa, bez `sitemapindex`) | `https://www.rsmpoland.pl/sitemap.xml`, 2026-08-28 |
| Wersje językowe | na tej domenie tylko `/pl/`. Przełącznik języka wyprowadza poza serwis: EN → `www.rsm.global/poland/en` (`/en` daje 301), DE → `www.rsm.global/poland/de` (oba 200; sama ścieżka `/de` → 404), CN → osobna domena `rsmpoland.cn` [do weryfikacji — brak odpowiedzi z tej sieci] | `curl -I`, przełącznik języka na stronie głównej, 2026-08-28 |

### Rozkład URL-i wg sekcji

| Sekcja | URL-i |
|---|---|
| `/pl/blog` | **672** |
| `/pl/insights` (legacy) | **394** |
| `/pl/zespol` | **180** |
| `/pl/uslugi` | 88 |
| `/pl/multimedia` | 84 |
| root-level (legacy, bez prefiksu `/pl/`) | 98 |
| `/pl/job-posts` | 18 |

**To jest serwis ~3,5× mniejszy niż Crido** (1 557 vs ~5 400 URL-i), a mimo to konkuruje w tych samych niszach. Skala nie jest ich bronią — jakość pojedynczej strony jest.

### Kategorie bloga (7 głównych)

`/pl/blog/` + slug kategorii:
`doradztwo-podatkowe` (332 URL-e) · `audyt-i-ksiegowosc` (151) · `zakladanie-prowadzenie-firmy` (38, plus 18 na wariancie `zakladanie-i-prowadzenie-firmy`) · `ceny-transferowe` (25) · `it-consulting` (20) · `doradztwo-transakcyjne` (17) · `kadry-i-place` (13) · dodatkowo `doing-business-poland` (12), `german-desk` (9) i szczątkowe `hr-payroll` (3)

Dwa dublujące się slugi tej samej kategorii (`zakladanie-prowadzenie-firmy` / `zakladanie-i-prowadzenie-firmy`) oraz `kadry-i-place` obok `hr-payroll` to ślad po nieuporządkowanej migracji taksonomii.

Kategoria = segment URL-a, nie parametr. Struktura `/pl/blog/{kategoria}/{slug}` jest czytelna i płaska (3 poziomy).

---

## 2. Co robią DOBRZE — rzeczy do skopiowania

### 2.1. E-E-A-T: 179 stron eksperckich z numerami uprawnień

To jest ich najmocniejszy aktyw i największa różnica wobec Crido (którego E-E-A-T jest słabe).

Profil `/pl/zespol/piotr-liss` zawiera (stan 2026-08-28):

- stanowisko: **Tax Partner**, lokalizacja: Poznań
- **„Doradca podatkowy, numer uprawnień 10240"** — konkretny numer wpisu, weryfikowalny w rejestrze KIDP
- rok uzyskania wpisu (2005), rok dołączenia do firmy (2004), rok awansu na partnera (2008)
- wykształcenie (Uniwersytet Ekonomiczny w Poznaniu, Wydział Zarządzania)
- 2 wypunktowane specjalizacje w polu „Specjalizacja" (Doradztwo podatkowe, Due diligence)
- działalność zewnętrzna: szkolenia dla zarządów, członkostwo w Center of Excellence in Tax, prelekcje konferencyjne
- publikacje w prasie branżowej (*Rzeczpospolita*, *Dziennik Gazeta Prawna*) i książki
- telefon bezpośredni, e-mail, **link do LinkedIn**

To jest wzorcowa realizacja sygnałów E-E-A-T dla YMYL: konkretne uprawnienie z numerem, staż liczony w latach, dowody uznania zewnętrznego, tożsamość weryfikowalna poza domeną.

**Słabość, którą warto ominąć:** profil **nie zawiera listy artykułów tej osoby**. Encja autora istnieje, ale nie jest hubem treści — traci się linkowanie wewnętrzne i sygnał „ten człowiek regularnie publikuje o X".

### 2.2. Autor spięty z artykułem przez JSON-LD `@id`

Każdy artykuł ma `BlogPosting` z autorem jako encją `Person` wskazującą na profil w zespole:

```json
"author": {
  "@type": "Person",
  "@id": "https://www.rsmpoland.pl/pl/zespol/emilia-kowalczys",
  "name": "Emilia KOWALCZYS",
  "url": "https://www.rsmpoland.pl/pl/zespol/emilia-kowalczys"
}
```
(źródło: HTML `/pl/blog/doradztwo-podatkowe/home-office-ryzyko-podatkowe`, 2026-08-28)

Użycie `@id` (a nie samego `name`) to poprawne modelowanie encji — Google dostaje jednoznaczny identyfikator osoby, ten sam we wszystkich jej artykułach. Pełny graf zawiera też `datePublished`, `dateModified`, `isAccessibleForFree` i `publisher` z logo.

W stopce artykułu autor jest podpisany widocznie: nagłówek **„Autorka"**, zdjęcie, imię i nazwisko, tytuł stanowiska („Tax Consultant", „Corporate Advisory Senior"), link do profilu.

### 2.3. Format artykułu zoptymalizowany pod AI search i snippety

Analiza dwóch artykułów flagowych:

**A. `/pl/blog/doradztwo-podatkowe/home-office-ryzyko-podatkowe`** (~1 050 słów, opublikowany i zaktualizowany 2026-08-24)

Struktura nagłówków:
```
H1  Home office a zakład podatkowy – nowa wersja komentarza OECD
H2  Kluczowe informacje:                      ← ramka z 3 punktami (ikony żarówek)
H2  Spory z organami podatkowymi potwierdzają niepewność...
H2  Sprawdź, jak możemy pomóc twojej firmie   ← BLOK CTA W ŚRODKU TREŚCI
    H3  Doradztwo podatkowe
    H3  Stałe miejsce prowadzenia działalności (CIT, VAT)
    H3  Rezydencja podatkowa i wykazywanie dochodów zagranicznych
H2  Czy ostatnia aktualizacja komentarza do Modelowej Konwencji OECD zmieniła sytuację...
H2  Wątpliwości dotyczące wpływu pracy zdalnej na ryzyko powstania zakładu...
H4  Autorka
H2  Przeczytaj również
H2  Zasubskrybuj newsletter RSM Poland
```

**B. `/pl/blog/zakladanie-prowadzenie-firmy/spolka-z-o-o-przewodnik-dla-obcokrajowcow`** (~1 600 słów, 2026-08-24)

```
H1  Zarządzanie polską spółką z o.o. bez znajomości języka polskiego. Kompletny przewodnik...
H2  Sprawdź, jak możemy pomóc twojej firmie   ← CTA wysoko, nad treścią
H2  Czy można zarządzać polską spółką z o.o. bez znajomości języka polskiego?
H2  Jak zagraniczny członek zarządu podpisuje dokumenty i sprawozdania finansowe?
H2  Czy PESEL jest obowiązkowy?
H2  Kiedy potrzebny jest tłumacz przysięgły?
H2  W jakich sytuacjach tłumacz przysięgły jest szczególnie istotny?
H2  Jak zagraniczna spółka-matka może kontrolować polski zarząd?
    H3  Cztery filary skutecznego nadzoru właścicielskiego
H2  Jakie decyzje warto objąć zgodą właściciela?
H2  Raportowanie właścicielskie: standard stosowany przez międzynarodowe grupy
H2  Najczęstsze bariery operacyjne zagranicznych inwestorów
    H3  5 najczęstszych błędów zagranicznych właścicieli spółek w Polsce
H2  Checklista dla zagranicznego właściciela polskiej spółki
H2  Najważniejsze pojęcia dla zagranicznego inwestora   ← glosariusz
H2  FAQ inwestorów zagranicznych                        ← 8 pytań
H2  Podsumowanie
```

Wzorce warte przeniesienia:

1. **Ramka „Kluczowe informacje" / „Najważniejsze informacje w 60 sekund" na samej górze** — 3 punkty streszczające. Idealny materiał do wyciągnięcia przez AI Overviews i featured snippet, zanim czytelnik dojdzie do treści.
2. **Nagłówki H2 w formie pytań** — w przewodniku B **7 z 13** nagłówków to pytania dosłownie odpowiadające zapytaniom użytkownika („Czy PESEL jest obowiązkowy?"). To jest mapowanie 1:1 na long-tail.
3. **Sekcja FAQ na końcu** (8 pytań) + **glosariusz pojęć** + **checklista** + **„5 najczęstszych błędów"** — cztery różne formaty ekstrakcji w jednym tekście.
4. **Deklarowany czas czytania** („4 minuty") i data widoczna dla użytkownika.
5. **Rozjazd H1 vs `<title>`, celowy.** H1 jest długi i opisowy, `<title>` krótki i frazowy:
   - H1: „Zarządzanie polską spółką z o.o. bez znajomości języka polskiego. Kompletny przewodnik dla zagranicznych właścicieli i członków zarządu"
   - `<title>`: „Zarządzanie polską spółką z o.o. przez obcokrajowców | RSM Poland"

   Podobnie w artykule A `<title>` („Home office a zakład podatkowy | RSM Poland") jest krótszy od H1 i od `og:title`. Trzy różne pola, trzy różne długości, każde pod swój kanał.
6. **Nazwy plików obrazów nasycone frazami:** `home-office-zaklad-podatkowy-doradztwo-podatkowe-dla-firm.jpg` — slug obrazu zawiera zarówno temat artykułu, jak i frazę ofertową.

### 2.4. Łączenie treści z ofertą — najlepszy element całego serwisu

Blok **„Sprawdź, jak możemy pomóc twojej firmie"** jest wstawiany **w środek treści artykułu** (między sekcje merytoryczne), a linki w nim są **dobierane tematycznie do konkretnego artykułu**, nie generyczne.

W artykule o home office i zakładzie podatkowym blok linkuje do:
- `/pl/uslugi/doradztwo-podatkowe`
- `/pl/uslugi/doradztwo-podatkowe/stale-miejsce-prowadzenia-dzialalnosci`
- `/pl/uslugi/doradztwo-podatkowe/opodatkowanie-expatow`

(źródło: parsowanie HTML artykułu, 2026-08-28)

Dwa z trzech linków to **podusługi drugiego poziomu**, precyzyjnie odpowiadające tematowi tekstu. To nie jest automat wstrzykujący losowe treści (jak widget CRIDOTEKA u Crido, gdzie linki bywają tematycznie niezwiązane) — to kuracja redakcyjna.

Ruch jest też dwukierunkowy: **strony usług mają sekcję „Przeczytaj również"** linkującą do artykułów. Oferta i treść karmią się nawzajem.

**Sekcja „Przeczytaj również"** w artykule linkuje do 3 tekstów z **tej samej kategorii i tego samego wąskiego tematu**:
- `/pl/blog/doradztwo-podatkowe/spolka-jako-agent-zalezny`
- `/pl/blog/doradztwo-podatkowe/fixed-establishment-vat`
- `/pl/blog/doradztwo-podatkowe/opodatkowanie-przedstawiciela-handlowego`

Wszystkie trzy dotyczą zakładu podatkowego / stałego miejsca działalności — czyli dokładnie klastra artykułu źródłowego.

### 2.5. Strona usługi jako hub podusług

`/pl/uslugi/doradztwo-podatkowe` (`<title>`: „Doradztwo podatkowe dla firm | RSM Poland") zawiera **26 nagłówków H3, z czego 25 to linkowane podusługi** (26. to nagłówek formularza kontaktowego), m.in.: Audyt podatkowy, Corporate Tax Compliance, Tax Litigation, Opinie podatkowe, Rezydencja podatkowa, Planowanie podatkowe, Programy motywacyjne, Ceny transferowe, VAT compliance, **Wdrożenie KSeF w firmie**, Stałe miejsce prowadzenia działalności, **Ulga B+R dla branży IT**, **Ulga Innovation Box dla branży IT**, Ulga na ekspansję / prototyp / robotyzację, Podatek od nieruchomości, Schematy podatkowe (MDR), **Pillar 2**, **DAC7**, Krajowy podatek minimalny, **Wdrożenie JPK_CIT**, Doradztwo dla branży nieruchomości, **Doradztwo dla sektora obronnego i zbrojeniowego**, Porozumienie Inwestycyjne.

Dwie obserwacje:
- **Segmentacja ulgi po branży** („Ulga B+R **dla branży IT**") — węższa fraza, mniejsza konkurencja, wyższa intencja niż samo „ulga B+R".
- **Strony pod świeże regulacje** (Pillar 2, DAC7, JPK_CIT, KSeF) — łapią popyt w momencie wejścia przepisów.

Struktura strony usługi: H1 → CTA z podusługami → sekcja korzyści („Profesjonalne doradztwo podatkowe: jakie korzyści daje konsultacja z ekspertem?") → zakres usług → **„Nasi specjaliści"** (E-E-A-T na stronie sprzedażowej) → formularz → „Przeczytaj również".

### 2.6. Świeżość i tempo publikacji

`lastmod` z sitemapy, 2026:

| Miesiąc | URL-i |
|---|---|
| 2026-01 | 11 |
| 2026-02 | 21 |
| 2026-03 | 19 |
| 2026-04 | 22 |
| 2026-05 | 32 |
| 2026-06 | 25 |
| 2026-07 | **38** |
| 2026-08 (do 27.) | **38** |

Łącznie 206 URL-i dotkniętych w 2026 r. Tempo **rośnie** — z ~11/mies. w styczniu do ~38/mies. latem. Do tego rozkład `changefreq`: 1 054 URL-e `never` (archiwum), 434 `yearly`, 45 `monthly`, 16 `weekly`, 5 `daily`, 3 `hourly` — czyli świadome sygnalizowanie, które strony są żywe.

Strony usług też są odświeżane: `/pl/uslugi/audyt-finansowy/audyt-wewnetrzny` ma `lastmod` 2026-08-06 i `changefreq monthly`.

### 2.7. Slugi: widoczna ewolucja praktyki

Stare artykuły mają długie, automatycznie generowane slugi:
`/pl/blog/audyt-i-ksiegowosc/mssf-15-przychody-z-umow-z-klientami-czesc-8-rozpoznanie-przychodow-w` (ucięty w połowie słowa)

Nowe (sierpień 2026) mają krótkie, ręcznie ustawione slugi frazowe:
`/pl/blog/doradztwo-podatkowe/zwolnienie-jpk-st-kr` · `/pl/blog/audyt-i-ksiegowosc/oplata-ewkf` · `/pl/blog/doradztwo-podatkowe/home-office-ryzyko-podatkowe`

To dowód, że ktoś tam realnie prowadzi SEO, a nie tylko publikuje.

---

## 3. Publikacje cykliczne — obraz mieszany

| Format | Stan | Ocena |
|---|---|---|
| **Blog** | 672 URL-e, ~38 publikacji/mies. w 2026 | Silny, rosnący |
| **Webinary** | 41 URL-i (`/pl/multimedia/webinar`) | Aktywny |
| **Nagrania** | 30 URL-i | Aktywny |
| **Podcast** | 12 URL-i, seria **„Inteligentny biznes"**; bieżąca seria **„Podatkowy GPS"** tylko na Spotify | Słaby SEO-wo — patrz niżej |
| **Newsletter** | `/pl/newsletter`, wybór języka PL/EN/DE | Brak opisu treści i częstotliwości |
| **Raporty** | `/pl/raporty` | **To nie jest thought leadership** |
| **Tax Alert** | 68 URL-i, seria **porzucona** | Zmarnowany aktyw |

Trzy rzeczy wymagają sprostowania wobec pierwszego wrażenia:

**„Raporty" to nie raporty branżowe.** `/pl/raporty` zawiera wyłącznie **sprawozdania z przejrzystości** firmy audytorskiej za lata obrotowe 2016–2022 — obowiązkowe dokumenty compliance wynikające z regulacji audytorskich, nie publikacje eksperckie. Pobranie nie wymaga formularza (brak lead magnetu). SEO-wo to prawie bezwartościowe. **RSM nie ma cyklicznego raportu branżowego** — to luka, nie przewaga.

**Podcast nie pracuje na SEO.** Seria „Inteligentny biznes" (M&A, ekspansja zagraniczna, cyberbezpieczeństwo, podatki) jest dystrybuowana przez Spotify / Apple / Google Podcasts, ale strona **nie zawiera transkrypcji ani rozbudowanych opisów tekstowych** — tylko linki do platform zewnętrznych, bez osadzonych odtwarzaczy. Zero treści do zaindeksowania.

Co więcej, **„Podatkowy GPS" to osobna, aktualnie prowadzona seria** — nie dawna nazwa „Inteligentnego biznesu". Strona główna promuje ją jako „Podcast RSM Poland — Podatkowy GPS. Twój przewodnik po świecie podatków", a link prowadzi **bezpośrednio na Spotify** (`open.spotify.com/show/6rSlvdMCnYhuj2FGuAiQVs`, 17+ odcinków, najnowsze z sierpnia 2026). W sitemapie nie ma ani jednego URL-a tej serii, a `/pl/multimedia/podcast` opisuje wyłącznie starszy „Inteligentny biznes" (12 URL-i, tematyka pandemiczna). Czyli **żywy podcast ma zerową obecność na własnej domenie** — cały ruch i cała treść oddane platformie zewnętrznej. To poważniejszy błąd niż brak transkrypcji.

**Seria „Tax Alert" została porzucona.** W sitemapie jest **68 URL-i** o wzorcu `/pl/blog/doradztwo-podatkowe/tax-alert-{numer}{rok}` (np. `tax-alert-112015`, `tax-alert-42013`). Numerowana seria alertów podatkowych z lat ~2013–2015, dziś nieciągnięta. Sam pomysł jest dobry — wykonanie zarzucone.

---

## 4. Czego NIE robią dobrze — luki do wykorzystania

### 4.1. ~394 legacy URL-e z pustymi tagami `<title>` [potwierdzone na próbce]

Sekcja `/pl/insights/` (394 URL-e: `centrum-prasowe` 308, `rsm-poland-blog` 30, `aktualnosci` 18, `multimedia` 19, `blog` 12, `sector-insights` 7) to archiwum sprzed migracji na `/pl/blog/`. Stan:

- odpowiadają **200 OK**, nie są przekierowane
- **self-canonical** (wskazują na siebie, nie na nowe odpowiedniki)
- **`<title>` jest pusty** — dosłownie `<title>| RSM Poland</title>`

Sprawdziłem 5 losowych stron z tej sekcji — **wszystkie 5** miały pusty tytuł:

```
fascynuje-mnie-praca-z-liczbami                <title>| RSM Poland</title>
5-pytan-do-rekrutera-z-rsm-poland              <title>| RSM Poland</title>
obowiazkowy-split-payment-problem...           <title>| RSM Poland</title>
uslugi-niematerialne-w-cit-czyli-540-powodow   <title>| RSM Poland</title>
podatek-minimalny-nawet-dla-zwolnionych-z-cit  <title>| RSM Poland</title>
```
(źródło: `curl` na URL-ach z sitemapy, 2026-08-28)

Do tego **98 URL-i legacy w korzeniu domeny** (bez prefiksu `/pl/`), np. `/zaswiadczenie-a1-i-watpliwosci-z-nim-zwiazane` — również 200 OK, bez przekierowania.

Czyli ok. **jedna trzecia indeksowalnego serwisu to zombie-strony** (394 + 98 + 26 z sekcji 4.3 = 518 z 1 557, ~33%), które konkurują z nowymi treściami i marnują crawl budget.

### 4.2. Sitemapa w 100% wskazuje na wersję bez `www` — same przekierowania 301

Wszystkie 1 557 wpisów w `sitemap.xml` używa `https://rsmpoland.pl/...`, podczas gdy kanoniczna wersja serwisu to `https://www.rsmpoland.pl/...`. Próbka 10 losowych URL-i z sitemapy: **10/10 zwróciło 301**.

Sitemapa powinna zawierać wyłącznie finalne, kanoniczne URL-e. Tu każdy wpis to dodatkowy skok.

### 4.3. 26 URL-i `wpisy-z-kategorii-*` — kolejna warstwa zombie, nie 404

W sitemapie jest 26 wpisów o wzorcu `/pl/blog/wpisy-z-kategorii-{kategoria}/{slug}` (np. `/pl/blog/wpisy-z-kategorii-it-consulting/netsuite-w-polsce`). Sprawdziłem **wszystkie 26**: każdy zwraca **200 OK**, jest **self-canonical**, a **25 z 26 ma pusty `<title>`** (`| RSM Poland`) — jedyny wyjątek to `polaczenie-spolek-metoda-nabycia`.

404 zwraca co innego: **sam segment kategorii bez slugu** (`/pl/blog/wpisy-z-kategorii-it-consulting`, `/pl/blog/wpisy-z-kategorii-audyt-i-ksiegowosc`) — ale tych ścieżek w sitemapie nie ma. Odpowiedniki w kanonicznej kategorii też nie istnieją: `/pl/blog/audyt-i-ksiegowosc/darowizna-ulga-na-cit` → 404, podczas gdy `/pl/blog/wpisy-z-kategorii-audyt-i-ksiegowosc/darowizna-ulga-na-cit` → 200. Czyli to nie duplikat starej i nowej ścieżki, tylko artykuły uwięzione pod błędnym slugiem kategorii po migracji.

Praktycznie: te 26 URL-i należy doliczyć do puli z 4.1, nie traktować jako osobny problem z 404.

### 4.4. `FAQPage` jest — ale rozbite na 8 osobnych zakresów microdata

Sekcja **„FAQ inwestorów zagranicznych" z 8 pytaniami** w przewodniku dla obcokrajowców **jest oznaczona semantycznie** — tyle że nie w JSON-LD, lecz w **microdata**, w HTML akordeonu. Każde pytanie ma poprawny komplet własności:

```html
<div itemscope itemtype="https://schema.org/FAQPage">
  <div itemscope itemprop="mainEntity" itemtype="https://schema.org/Question">
    <div itemprop="name">Czy cudzoziemiec może zostać członkiem zarządu polskiej spółki z o.o.?</div>
    <div itemscope itemprop="acceptedAnswer" itemtype="https://schema.org/Answer">
      <div itemprop="text"><p>Tak. Członkiem zarządu polskiej spółki z o.o. może być osoba…</p></div>
```

Problem jest subtelniejszy niż brak oznaczenia: **każdy element akordeonu otwiera własny `FAQPage`**. W kodzie jest 8× `schema.org/FAQPage`, 8× `Question`, 8× `acceptedAnswer`, 8× `Answer` — czyli osiem jednopytaniowych stron FAQ zamiast jednego `FAQPage` z ośmioma `mainEntity`. To artefakt renderowania paragrafu Drupala per element, nie decyzja redakcyjna.

Blok JSON-LD nadal jest tylko jeden (`BlogPosting`). Faktycznie brakuje `HowTo` i `BreadcrumbList` — tych w kodzie nie ma w żadnej postaci.

Wniosek do przeniesienia: oznaczenie FAQ owszem, ale **jeden `FAQPage` na stronę**, najlepiej w JSON-LD obok `BlogPosting`.

### 4.5. Błąd w JSON-LD: `image.url` to nazwa pliku, nie URL

```json
"image": {
  "@type": "ImageObject",
  "url": "home-office-zaklad-podatkowy-doradztwo-podatkowe-dla-firm.jpg"
}
```

Wartość nie jest bezwzględnym URL-em (poprawnie: `https://www.rsmpoland.pl/sites/default/files/2026-08/...`, co widać w `og:image`). Google prawdopodobnie odrzuca tę właściwość.

### 4.6. Strony usług nie mają żadnego JSON-LD

`/pl/uslugi/doradztwo-podatkowe` — **zero bloków JSON-LD**. Brak `Service`, `Organization`, `BreadcrumbList`, `LocalBusiness`. Schema jest wdrożona wyłącznie na artykułach. Przy 4 biurach (Poznań, Warszawa, Szczecin, Katowice) brak `LocalBusiness` to strata w SEO lokalnym.

### 4.7. Hub kategorii nie pokazuje autorów ani dat

Na `/pl/blog/doradztwo-podatkowe` przy artykułach widać tagi, ale **nie widać autorów, dat ani czasu czytania**. Najsilniejszy aktyw firmy (nazwiska z uprawnieniami) jest niewidoczny na stronie listującej. Do tego lista ładuje 6 artykułów na porcję i dalej działa przez „Load more items" (Drupal *views infinite scroll*). Wbrew pierwszemu wrażeniu **nie jest to czysty JS**: przycisk to realny `<a class="button" href="?page=1" rel="next">`, a `?page=1` zwraca 200 z kolejną porcją artykułów — archiwum pozostaje więc crawlowalne. Minus jest słabszy, niż wygląda: brak numerowanych linków i jeden `rel="next"` na krok oznaczają długi łańcuch, ale nie ślepy zaułek.

### 4.8. Profil eksperta nie listuje jego artykułów

179 profili, żaden nie działa jak hub treści autora. Brak linkowania profil → artykuły osłabia i encję osoby, i przepływ PageRank.

---

## 5. Porównanie: RSM vs Crido (na podstawie obu notatek)

| Wymiar | RSM Poland | Crido |
|---|---|---|
| Skala | 1 557 URL-i | ~5 400 URL-i |
| CMS | Drupal | WordPress + Yoast |
| Architektura | jeden blog, 7 kategorii w URL-u | osobne CPT per obszar, własne sitemapy |
| E-E-A-T | **silne** (179 profili, numery uprawnień, `author.@id`) | słabe |
| Dane strukturalne | `BlogPosting` (JSON-LD) + `FAQPage` (microdata, rozbite) na artykułach, **zero na usługach** | słabe |
| Format artykułu | **bardzo dobry** (ramka kluczowych informacji, H2-pytania, FAQ, checklisty) | przeciętny |
| Treść → oferta | **kuracja redakcyjna**, linki do podusług dopasowane tematycznie | automat, linki bywają nietrafione |
| Strony pillar | brak wyraźnych pillarów w korzeniu | pillary na płaskich URL-ach (`/ulga-b-r/`) |
| Higiena techniczna | **słaba** (puste tytuły, ~33% zombie-URL-i, 100% wpisów sitemapy na 301) | lepsza |
| Publikacje cykliczne | webinary + podcast bez transkrypcji, raporty = compliance | 97 raportów, hottopics, threads |

**Wniosek z porównania:** Crido wygrywa architekturą i skalą, RSM wygrywa jakością strony i autorytetem autorów. Żaden z nich nie ma obu naraz. To jest przestrzeń dla mentzen.pl.

---

## 6. Wnioski dla mentzen.pl

### Do skopiowania wprost (wysoki priorytet)

1. **Ramka „Kluczowe informacje" na górze każdego artykułu.** 3 punkty streszczenia przed treścią. Najtańszy sposób na obecność w AI Overviews i featured snippetach. RSM stosuje to konsekwentnie w nowych tekstach.

2. **Nagłówki H2 jako pytania użytkownika.** W przewodniku RSM 7 z 13 nagłówków to dosłowne pytania („Czy PESEL jest obowiązkowy?"). Mapowanie 1:1 na long-tail, bez sztuczności.

3. **Śródtekstowy blok CTA z linkami do podusług dobranymi do tematu artykułu.** Kluczowe: **nie generyczny**. RSM linkuje z artykułu o home office do `stale-miejsce-prowadzenia-dzialalnosci` i `opodatkowanie-expatow` — nie do strony głównej oferty. To wymaga decyzji redakcyjnej przy każdym tekście, ale to właśnie odróżnia ich od automatu Crido.

4. **Profile ekspertów z numerami uprawnień.** Numer wpisu doradcy podatkowego / biegłego rewidenta, rok wpisu, staż, specjalizacje, LinkedIn, telefon. W YMYL to jest waluta. Mentzen ma tu naturalną przewagę — rozpoznawalne nazwiska.

5. **`author` jako encja `Person` z `@id`** wskazującym na profil, identyczna we wszystkich artykułach danej osoby. Nie sam `name`.

6. **Trzy różne warianty tytułu:** długi opisowy H1, krótki frazowy `<title>`, oddzielny `og:title`. RSM różnicuje je świadomie.

7. **Nazwy plików obrazów z frazą tematyczną i ofertową**, np. `home-office-zaklad-podatkowy-doradztwo-podatkowe-dla-firm.jpg`.

8. **Dwukierunkowe linkowanie treść ↔ oferta.** Artykuł → podusługi, ale też strona usługi → „Przeczytaj również" z artykułami.

### Do zrobienia LEPIEJ niż RSM (luki konkurencyjne)

9. **Jeden `FAQPage` na stronę, nie jeden na pytanie.** RSM FAQ oznacza (microdata w akordeonie), ale generuje 8 osobnych `FAQPage` zamiast jednego z ośmioma `mainEntity` — typowa pułapka komponentu CMS-a renderowanego per element. Do tego `BreadcrumbList` i `Service` na stronach oferty, `LocalBusiness` na stronach biur — tych RSM nie ma w ogóle. Mentzen może to mieć od pierwszego dnia.

10. **Strona autora jako hub jego artykułów.** RSM ma 179 profili, żaden nie listuje publikacji. Lista tekstów pod bio wzmacnia encję i rozprowadza PageRank.

11. **Autor, data i czas czytania na listingach kategorii.** RSM ukrywa swój najmocniejszy aktyw na stronach listujących.

12. **Numerowana paginacja zamiast łańcucha `rel="next"`.** RSM ma tu poprawny fallback (`?page=N` w `<a href>`), więc archiwum jest crawlowalne — ale numerowane linki skracają drogę do głębokich stron zamiast wymuszać przejście krok po kroku.

13. **Realna publikacja cykliczna z lead magnetem.** RSM ma pod „Raportami" wyłącznie obowiązkowe sprawozdania z przejrzystości — nie ma żadnego raportu branżowego. Nisza stoi otworem.

14. **Transkrypcje podcastów i webinarów na stronie — i sama strona odcinka.** RSM ma 10 odcinków starego podcastu (12 URL-i w sitemapie razem ze stroną cyklu i indeksem) i 41 webinarów bez ani jednej transkrypcji, a bieżącą serię („Podatkowy GPS", 17 odcinków) trzyma wyłącznie na Spotify, bez ani jednego URL-a we własnym serwisie. Całość treści mówionej jest niewidoczna dla wyszukiwarki. Transkrypcja + streszczenie + rozdziały to darmowy long-tail.

15. **Ciągłość serii.** „Tax Alert" (68 URL-i) został porzucony ok. 2015 r. Seria cykliczna działa tylko, jeśli jest ciągnięta. Lepiej zadeklarować rzadszy rytm i go dotrzymać.

### Higiena techniczna — czego nie powtórzyć

16. **Sitemapa wyłącznie z kanonicznymi URL-ami** (właściwy wariant `www`, bez przekierowań). RSM ma 1 557 wpisów, z których każdy to 301.

17. **Przy każdej migracji URL-i: 301 na nowe adresy, nie zostawianie starych na 200 z self-canonical.** RSM ma ~394 + 98 + 26 zombie-URL-i, w większości z pustymi tagami `<title>`. To ok. 33% serwisu.

18. **Walidacja JSON-LD w CI.** Błąd `image.url` z samą nazwą pliku przeszedł u nich niezauważony.

### Strategia treści

19. **Segmentacja usług po branży.** RSM ma osobne strony „Ulga B+R **dla branży IT**" i „Ulga Innovation Box **dla branży IT**" — węższa fraza, słabsza konkurencja, wyższa intencja niż generyczne „ulga B+R", o które bije się Crido.

20. **Strony pod świeże regulacje, publikowane w momencie wejścia przepisów** — KSeF, JPK_CIT, Pillar 2, DAC7, krajowy podatek minimalny. RSM ma dedykowane strony ofertowe pod każdą z nich.

21. **Nisze o niskiej konkurencji:** RSM obsługuje „Doradztwo dla sektora obronnego i zbrojeniowego", „German Desk", „Doing Business in Poland". Segmenty wąskie, ale o wysokiej wartości klienta.

22. **Format „kompletny przewodnik" (~1 600 słów) z pełnym zestawem elementów ekstrakcji**: ramka 60-sekundowa + H2-pytania + „5 najczęstszych błędów" + checklista + glosariusz + FAQ + podsumowanie. Jeden tekst obsługuje kilkanaście long-taili naraz.

23. **Rozjazd struktury nawigacji i głębokości treści.** RSM trzyma płaskie `/pl/blog/{kategoria}/{slug}` (3 poziomy) i krótkie, ręcznie pisane slugi w nowych tekstach. Warto od początku wymusić ręczne slugi zamiast auto-generowanych z tytułu.

---

## Źródła

Wszystkie pobrania: **2026-08-28**.

- `https://www.rsmpoland.pl/` — struktura nawigacji
- `https://www.rsmpoland.pl/robots.txt` — identyfikacja Drupala
- `https://www.rsmpoland.pl/sitemap.xml` — 1 557 URL-i, `lastmod`, `changefreq`
- `https://www.rsmpoland.pl/pl/blog` — kategorie
- `https://www.rsmpoland.pl/pl/blog/doradztwo-podatkowe` — hub kategorii
- `https://www.rsmpoland.pl/pl/blog/doradztwo-podatkowe/home-office-ryzyko-podatkowe` — JSON-LD, nagłówki, CTA, linkowanie
- `https://www.rsmpoland.pl/pl/blog/zakladanie-prowadzenie-firmy/spolka-z-o-o-przewodnik-dla-obcokrajowcow` — format przewodnika, FAQ
- `https://www.rsmpoland.pl/pl/uslugi/doradztwo-podatkowe` — hub usług, brak JSON-LD
- `https://www.rsmpoland.pl/pl/zespol/piotr-liss` — profil eksperta
- `https://www.rsmpoland.pl/pl/raporty` — sprawozdania z przejrzystości
- `https://www.rsmpoland.pl/pl/multimedia/podcast` — seria „Inteligentny biznes"
- `https://www.rsmpoland.pl/pl/newsletter` — formularz subskrypcji
- URL-e legacy sprawdzone przez `curl -I` (kody odpowiedzi, `location`, `<title>`, canonical)

Weryfikacja adwersarialna 2026-08-28: liczby z sitemapy (1 557, rozkład sekcji, `changefreq`, `lastmod`, 68 „Tax Alert", 26 `wpisy-z-kategorii`), kody odpowiedzi, puste `<title>`, JSON-LD i bloki CTA przeliczone u źródła i potwierdzone. Skorygowano: liczbę słów obu artykułów, liczbę pytań FAQ, liczbę specjalizacji w profilu, liczbę profili w `/pl/zespol`, liczbę podusług na stronie oferty, udział zombie-URL-i oraz opis mechanizmu „Load more". Flaga `[do weryfikacji]` dla „Podatkowego GPS" — rozstrzygnięta: to osobna, aktywna seria kierowana wprost na Spotify.

Druga weryfikacja adwersarialna 2026-08-28 (ponowne pobranie sitemapy do izolowanego katalogu — poprzednia kopia robocza znów została nadpisana przez równoległą sesję). Potwierdzone bez zmian: 1 557 URL-i i 100% hosta bez `www`, rozkład sekcji (672/394/180/98/88/84/18), 179 profili, `lastmod` po miesiącach i `changefreq` co do sztuki, 68 „Tax Alert" z lat 2013–2015, dane profilu Piotra Lissa (nr uprawnień 10240, wpis 2005, RSM od 2004, partner od 2008, 2 specjalizacje), oba artykuły (nagłówki, `title`/`og:title`/H1, 7 z 13 H2-pytań, 8 pytań FAQ, daty 2026-08-24), 3 linki CTA i 3 linki „Przeczytaj również" co do adresu, 26 H3 na stronie usługi i zero danych strukturalnych tamże, błąd `image.url`, puste `<title>` na 13/13 sprawdzonych URL-ach legacy, raporty za lata obrotowe 2016–2022 bez formularza, podcast bez transkrypcji, 17 odcinków „Podatkowego GPS" i zero jego URL-i w sitemapie, crawlowalna paginacja `?page=1`.

Skorygowano w tej rundzie cztery twierdzenia:
- **sekcja 4.3 była błędna** — 26 URL-i `wpisy-z-kategorii-*` z sitemapy zwraca 200, nie 404 (sprawdzone wszystkie 26); 404 daje sam segment kategorii, którego w sitemapie nie ma;
- **sekcja 4.4 była błędna** — `FAQPage` jest wdrożone, w microdata, ale rozbite na 8 osobnych zakresów; brak dotyczy tylko `HowTo` i `BreadcrumbList`;
- listing kategorii ładuje 6, nie 7 artykułów na porcję;
- „12 odcinków" starego podcastu to 10 odcinków przy 12 URL-ach.

Konsekwentnie przeliczono udział zombie-URL-i (492 → 518, ~32% → ~33%) i uzupełniono kategorię `hr-payroll` oraz opis wersji językowych (istnieje działająca wersja DE na `rsm.global`, wbrew wcześniejszemu „`/de/` → 404").

Dodatkowe źródła weryfikacji:
- `https://www.rsmpoland.pl/pl/blog/doradztwo-podatkowe?page=1` — potwierdzenie działającej paginacji (200 + kolejne artykuły)
- `https://open.spotify.com/show/6rSlvdMCnYhuj2FGuAiQVs` — seria „Podatkowy GPS" (17 odcinków)
- wszystkie 26 URL-i `wpisy-z-kategorii-*` sprawdzone pojedynczo (`curl` → kod, `<title>`, canonical), 2026-08-28
- `https://www.rsm.global/poland/de` i `https://www.rsm.global/poland/en` — wersje językowe poza domeną (200)
