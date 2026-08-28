# mentzen.pl — struktura serwisu i architektura informacji

Data badania: 2026-08-28. Zakres: mapowanie struktury URL i IA na potrzeby przyszłego skilla SEO.
Nie jest to audyt techniczny — obserwacje techniczne notuję tylko tam, gdzie wpływają na strukturę treści.

Stack: WordPress + Divi (obecny CPT `et_code_snippet_type`), Yoast SEO (sitemapy), Cloudflare przed originem.
Język: wyłącznie `pl-PL`, brak hreflang, brak wersji obcojęzycznych.

---

## 1. robots.txt

`https://mentzen.pl/robots.txt` — HTTP 200, `text/plain`, 1248 bajtów, serwowany przez Cloudflare (`cf-cache-status: HIT`).

Plik zawiera **wyłącznie blok komentarzy Cloudflare Content Signals Policy** (definicje sygnałów `search`, `ai-input`, `ai-train` plus zastrzeżenie praw z art. 4 dyrektywy UE 2019/790).
Po odfiltrowaniu linii zaczynających się od `#` i pustych **nie zostaje ani jedna dyrektywa**:

- brak `User-agent:`
- brak `Disallow:` / `Allow:`
- brak `Sitemap:`

Konsekwencja dla mapowania: sitemapy trzeba znaleźć konwencją, nie z robots.txt.

## 2. Sitemapy

Zarówno `https://mentzen.pl/sitemap.xml`, jak i `https://mentzen.pl/sitemap_index.xml` zwracają ten sam indeks Yoast (5 pozycji).

| Sitemapa | URL-e | lastmod indeksu |
|---|---:|---|
| https://mentzen.pl/post-sitemap.xml | 522 | 2026-08-27 |
| https://mentzen.pl/page-sitemap.xml | 44 | 2026-08-24 |
| https://mentzen.pl/praktyka-sitemap.xml | 1 | 2021-04-27 |
| https://mentzen.pl/interpretacje-sitemap.xml | 33 | 2025-02-25 |
| https://mentzen.pl/et_code_snippet_type-sitemap.xml | 1 | 2025-07-15 |
| **Razem** | **601** | |

Sitemapy zawierają rozszerzenie obrazkowe (`image:loc`): 593 wystąpienia w post-sitemap, 504 w page-sitemap.

Brak w indeksie: sitemapy taksonomii (archiwa kategorii bloga nie są zgłaszane) i sitemapy autorów.

## 3. Kategoryzacja URL-i wg wzorców

### 3.1 Rozkład ogólny

| Typ | Wzorzec URL | Liczba |
|---|---|---:|
| Wpisy blogowe | `/blog/<kategoria>[/<podkat>[/<podpodkat>]]/<slug>/` | 522 |
| Strony (usługi, produkty, firmowe, prawne) | `/<slug>/` (płaski, 1 segment) | 44 |
| Interpretacje (CPT) | `/blog/interpretacje/<slug>/` | 33 |
| Praktyka (CPT, martwy) | `/blog/praktyka/<slug>/` | 1 |
| Divi code snippet (techniczny) | `/blog/et_code_snippet_type/<slug>/` | 1 |

### 3.2 Strony (44) — podział funkcjonalny

**Usługi merytoryczne — 17 stron, płaskie URL-e jednosegmentowe:**

- Podatki: https://mentzen.pl/doradztwo-podatkowe/ (hub), /optymalizacja-podatkowa/, /cit-estonski/, /ceny-transferowe/, /vat/, /kryptowaluty/, /kontrola-i-postepowanie-podatkowe/
- Prawo: https://mentzen.pl/doradztwo-prawne/ (hub), /umowy-szyte-na-miare/, /sprawy-pracownicze/, /postepowania-sadowe-i-windykacja/, /wsparcie-zus/, /znaki-towarowe/
- Przekrojowe (linkowane z obu hubów): /fundacja-rodzinna/, /sukcesja/, /restrukturyzacja-dzialalnosci/
- Wejście dla nowych firm: https://mentzen.pl/start-z-mentzenem-zakladanie-firmy/ (stary slug `/start-z-mentzenem/` → 301)

**Produkty subskrypcyjne — 5 stron:**

- https://mentzen.pl/mentzen-plus/ — subskrypcja prawno-podatkowa
- https://mentzen.pl/mentzen-it/ — wariant dla branży IT
- https://mentzen.pl/mentzen-prime/ — księgowość
- https://mentzen.pl/kadry-mentzena/ — obsługa kadrowo-płacowa
- https://mentzen.pl/ewidencja-ip-box/ — usługa produktyzowana

**Firmowe i konwersyjne — 5 stron:**

- https://mentzen.pl/ (home)
- https://mentzen.pl/nasz-zespol/
- https://mentzen.pl/dane-kontaktowe/
- https://mentzen.pl/konsultacje-online/ (pełni rolę „Kontakt" w menu)
- https://mentzen.pl/panel-klienta-logowanie/

**Hub bloga — 1:** https://mentzen.pl/blog/

**Prawne / regulaminy — 16 stron** (36% wszystkich stron w page-sitemap), m.in.:
https://mentzen.pl/regulaminy/ (spis), /rodo/, /regulamin-strony-internetowej/, /regulamin-uslugi-mentzen-plus/, /regulamin-uslugi-mentzen-prime/, /regulamin-uslugi-ewidencja-ip-box/, /regulamin-swiadczenia-uslug-zakladania-spolek-w-portalu-s24/, /regulamin-uslugi-mentzen-nieruchomosci/, /regulamin-swiadczenia-uslugi-mentzen-podatki-prawo-2zl/, /regulamin-promocji-ksiegowosc-na-start-start-z-mentzenem/ oraz 4 załączniki (`/zalacznik-nr-*`).

Obserwacja ofertowa: regulaminy ujawniają usługi i promocje **bez własnych stron sprzedażowych** — Mentzen+ Nieruchomości, zakładanie spółek w portalu S24, promocja „Mentzen+ Podatki-Prawo za 2 zł", „Księgowość na start". Istnieją tylko jako dokumenty prawne, nie jako landingi.

### 3.3 Landing pages / lokalizacje

- **Brak stron lokalizacyjnych.** Zero URL-i typu miasto/oddział. Sprawdzone i zwracają 404: `/oddzialy/`, `/biuro-rachunkowe-torun/`, `/ksiegowosc-torun/`, `/doradca-podatkowy-warszawa/`.
- Serwis podaje **jeden adres**: Amicus Business Park, ul. Grudziądzka 110-114/101, 87-100 Toruń (https://mentzen.pl/dane-kontaktowe/), tel. +48 563 000 363. Cała komunikacja jest zdalna („konsultacje online").
- Brak dedykowanych landingów kampanijnych w sitemapie — rolę stron docelowych pełnią strony usług i produktów.
- Brak typowych zasobów lead-magnet: 404 na `/faq/`, `/baza-wiedzy/`, `/slownik/`, `/kalkulatory/`, `/wzory-dokumentow/`, `/poradniki/`, `/webinary/`, `/wideo/`. Kalkulatory istnieją, ale są osadzone wewnątrz stron usług (np. „Kalkulator Estoński CIT" na https://mentzen.pl/cit-estonski/).

### 3.4 Blog — głębokość URL

522 wpisy, wszystkie pod `/blog/`. Rozkład liczby segmentów po `/blog/`:

- 2 segmenty (`/blog/<kategoria>/<slug>/`): 337
- 3 segmenty (`/blog/<kategoria>/<podkategoria>/<slug>/`): 166
- 4 segmenty (`/blog/<kat>/<podkat>/<podpodkat>/<slug>/`): 19

Rozkład wg kategorii nadrzędnej w URL-u: `doradztwo-podatkowe` 306, `doradztwo-prawne` 107, `inne` 77, `ksiegowosc` 32.

Slug wpisu jest zawsze pełnym, opisowym tytułem-frazą (często żartobliwym, nie keywordowym), np.
https://mentzen.pl/blog/doradztwo-podatkowe/piekne-paznokcie-w-kosztach-uzyskania-przychodu/,
https://mentzen.pl/blog/inne/ryczalt/inni-lekarze-go-nienawidza-placi-85-ryczaltu-od-przychodow/,
https://mentzen.pl/blog/doradztwo-prawne/piles-nie-pracuj-prewencyjne-badanie-trzezwosci/.

## 4. Taksonomia bloga

Pełne drzewo kategorii odczytane z danych osadzonych w HTML strony https://mentzen.pl/blog/ (komponent `categories-wrapper`; liczby = `count` z tego JSON-a):

- **Doradztwo podatkowe** (197)
  - Ulgi podatkowe (48), VAT (24), Optymalizacja podatkowa (18), CIT estoński (11), Doradztwo podatkowe dla małych firm (6), Doradztwo podatkowe dla IT (2), Doradztwo podatkowe dla transportu (1), Kontrola podatkowa (1), Postępowanie podatkowe (1), Zatrudnienie i B2B (1)
- **Doradztwo prawne** (84)
  - Spółki (11) → Spółka z o.o. (3); Fundacja rodzinna (4); ZUS (1) → Składki ZUS (2), Mały ZUS (1); Nieruchomosci (1, literówka w nazwie termu)
- **Inne** (25)
  - Zakładanie firmy (8), Nowy Ład (8), Kryptowaluty (6), Ryczałt (5), Zatrudnienie (5) → Praca tymczasowa (7), Praca zdalna (5), Zatrudnienie na umowę o pracę (1); Kadry (4), Najczęściej czytane (4), Koszty uzyskania przychodu (3)
- **Księgowość** (32) — bez podkategorii

Hierarchia sięga 3 poziomów. Kategorie usługowe („dla IT", „dla małych firm", „dla transportu") są zdefiniowane, ale niemal puste (1–6 wpisów).

**Dwa równoległe adresy archiwów** — oba HTTP 200 z tym samym `<title>` („Archiwa: Doradztwo podatkowe -"):

- https://mentzen.pl/blog/doradztwo-podatkowe/
- https://mentzen.pl/blog/category/doradztwo-podatkowe/

Wpisy kanonicznie żyją pod pierwszym wzorcem, a nawigacja kategorii w UI linkuje do drugiego. Archiwa mają paginację (`/blog/page/2/` → 200, „« Starsze wpisy"), po 6 wpisów na stronę.

Archiwum podkategorii nie zawsze działa: https://mentzen.pl/blog/doradztwo-podatkowe/ulgi-podatkowe/ robi 301 na konkretny wpis (`.../ulgi-podatkowe-przyslugujace-rodzicom-sprawdz-czy-mozesz-skorzystac/`) — kolizja slugów. Wariant `/blog/category/...` tej kolizji nie ma.

Brak taksonomii tagów i archiwów autorów: `/blog/tag/`, `/blog/author/`, `/blog/autor/` → 404.

## 5. Custom post types poza blogiem

**Interpretacje** — 33 URL-e pod https://mentzen.pl/blog/interpretacje/<slug>/, np.
https://mentzen.pl/blog/interpretacje/pies-jako-koszt-uzyskania-przychodu/,
https://mentzen.pl/blog/interpretacje/kup-jacht-i-odlicz-od-tego-podatek-vat/,
https://mentzen.pl/blog/interpretacje/zderzenie-mocarstw-jak-pokonalismy-kis-w-kwestii-ukrytych-zyskow/.

To krótkie (400–500 słów) omówienia wygranych spraw i interpretacji indywidualnych, z sygnaturą w nagłówku (np. `0113-KDIPT2-1.4011.31.2022.1.MM`), podpisane konkretnym specjalistą ze zdjęciem. Format „case study bez etykiety case study" — najsilniejszy dowód kompetencji, jaki serwis posiada.

Sekcja jest jednak **osierocona**: `/blog/interpretacje/` → 404, brak linków do niej z https://mentzen.pl/ i z https://mentzen.pl/blog/ (0 wystąpień `blog/interpretacje` w HTML obu stron). Dostępna wyłącznie z sitemapy i wyszukiwarek. Ostatni `lastmod` w tej sitemapie: 2025-02.

**Praktyka** — 1 URL, https://mentzen.pl/blog/praktyka/copywriting-pisanie-tekstow/, `lastmod` 2021. Archiwum `/blog/praktyka/` → 404. Porzucony CPT.

**et_code_snippet_type** — 1 URL techniczny Divi, https://mentzen.pl/blog/et_code_snippet_type/et_code_snippet_html_js/, bez wartości treściowej.

## 6. Menu i architektura informacji

Menu główne — 6 pozycji, dwie z rozwijanymi podmenu:

1. **Subskrypcje** → /mentzen-plus/, /mentzen-it/, /mentzen-prime/, /kadry-mentzena/, /ewidencja-ip-box/, /panel-klienta-logowanie/
2. **Usługi** → /start-z-mentzenem-zakladanie-firmy/; **Doradztwo podatkowe** (/doradztwo-podatkowe/ + 9 podstron: optymalizacja, fundacja rodzinna, CIT estoński, ceny transferowe, restrukturyzacja, sukcesja, postępowania podatkowe, VAT, kryptowaluty); **Doradztwo prawne** (/doradztwo-prawne/ + 8 podstron: fundacja rodzinna, umowy, sprawy pracownicze, sukcesja, restrukturyzacja, postępowania sądowe i windykacja, wsparcie ZUS, znaki towarowe); **Księgowość** → /mentzen-prime/
3. **Blog** → /blog/
4. **Nasz zespół** → /nasz-zespol/
5. **Inwestorzy** → https://corporate.mentzen.pl/ (wyjście poza domenę)
6. **Kontakt** → /konsultacje-online/

Stopka: /regulaminy/, /rodo/ oraz link do wykonawcy (appworks.pl). Stopka jest minimalna — nie pełni roli mapy serwisu ani hubu linkowania wewnętrznego.

Obserwacje o strukturze:

- **Trzy równoległe osie oferty**: produkt subskrypcyjny (Mentzen+ / Prime / Kadry), usługa merytoryczna (VAT, CIT estoński, ceny transferowe…), moment w cyklu życia firmy (zakładanie → optymalizacja → restrukturyzacja/sukcesja). Menu miesza te osie: „Księgowość" w zakładce Usługi prowadzi do produktu /mentzen-prime/, a nie do strony usługowej.
- **Fundacja rodzinna, sukcesja i restrukturyzacja** figurują w podmenu podatkowym i prawnym jednocześnie — jeden URL, dwa wejścia.
- **Blog jest odcięty od oferty na poziomie URL i taksonomii**: kategorie bloga (`ulgi-podatkowe`, `zatrudnienie`, `nowy-lad`, `ryczalt`) nie odwzorowują slugów stron usług (`optymalizacja-podatkowa`, `cit-estonski`, `sprawy-pracownicze`). Wspólne nazwy mają tylko: VAT, CIT estoński, optymalizacja podatkowa, fundacja rodzinna, kryptowaluty.
- **Brak warstwy pośredniej** między hubem usługi a wpisem blogowym — żadnych stron pillar/„wszystko o…", brak breadcrumbów widocznych na wpisie.
- Kontakt = umówienie konsultacji. Rezerwacja terminów działa na zewnętrznym Calendesk (`konsultacje.calendesk.net/?services=…&employees=…`), linkowanym ze stron usług i z /nasz-zespol/.

## 7. Wzorzec strony usługowej

Na przykładzie https://mentzen.pl/cit-estonski/ (~1800–2000 słów) kolejność sekcji:

1. H1 z hasłem sprzedażowym („CIT Estoński", lead: „Obniżenie podatków jeszcze nigdy nie było tak proste!")
2. „<Usługa> – niższe podatki i brak zaliczek na CIT!"
3. „Dlatego warto się tym zainteresować!"
4. Formularz: „Wypełnij formularz, a my zajmiemy się Twoją sprawą indywidualnie i kompleksowo"
5. „Kto będzie mógł skorzystać z…?" (kwalifikacja leada)
6. „W czym Kancelaria Mentzen może Ci pomóc?"
7. „Dlaczego warto?"
8. Narzędzie interaktywne (kalkulator)
9. „Dowiedz się więcej na temat…" (blok pod linki do bloga)
10. Dowód społeczny: „Mówili o nas" (opinie klientów), „Pisali o nas" (logotypy Wprost, Rzeczpospolita, Business Insider, Money, Strefa Inwestorów)
11. Trzech imiennych specjalistów z przyciskiem „Umów konsultację"

Elementy nieobecne na stronach usług: FAQ (jest tylko na stronach subskrypcji), ceny (jawne wyłącznie na produktach subskrypcyjnych).

Strona subskrypcji https://mentzen.pl/mentzen-plus/ jest jedynym miejscem z cennikiem: Mentzen+ Podatki od 699 zł netto/mc, Mentzen+ Prawo i Podatki od 999 zł, pakiety wg przychodu (Standard do 1 mln – 699 zł, Premium do 10 mln – 1299 zł, Pro powyżej 10 mln – 2499 zł), plus sekcja „Najczęściej zadawane pytania".

## 8. Świeżość i tempo publikacji

`lastmod` w post-sitemap wg roku: 2023 – 312, 2024 – 90, 2025 – 75, 2026 – 45.
Wysokie 2023 wskazuje na masową migrację/aktualizację, nie na publikacje.

`lastmod` 2026 wg miesięcy: 03 – 5, 04 – 26 (skok, prawdopodobnie zbiorcza edycja), 05 – 1, 06 – 2, 07 – 4, 08 – 7.
Bieżące tempo modyfikacji: kilka URL-i miesięcznie. Sitemapa interpretacji nie ma nic nowszego niż 2025-02.

Strony (page-sitemap): 2026 – 25, 2025 – 16, 2023 – 3. Warstwa ofertowa jest odświeżana częściej niż blog.

## 9. Dane strukturalne i domeny powiązane

Home (https://mentzen.pl/) zawiera JSON-LD Yoast: `Organization`, `WebSite` (z `SearchAction`), `WebPage`, `BreadcrumbList`, `ImageObject`.
Brak `LegalService` / `AccountingService` / `LocalBusiness`, brak `Service`, brak `FAQPage` mimo istniejących sekcji FAQ na stronach subskrypcji, brak `Person` na /nasz-zespol/.

Domeny i systemy powiązane:

- https://corporate.mentzen.pl/ — relacje inwestorskie Grupy Kapitałowej Mentzen S.A. (Aktualności, O grupie, Model biznesowy i strategia, Zarząd i Rada Nadzorcza, Raporty ESPI/EBI, WZA, Do pobrania, Oferta publiczna, Kontakt). Osobny serwis, linkowany z menu głównego, z linkiem powrotnym „Przejdź na stronę Kancelarii Mentzen".
- `konsultacje.calendesk.net` — zewnętrzna rezerwacja konsultacji.
- https://mentzen.pl/panel-klienta-logowanie/ — wejście do panelu klienta.
- Nie odpowiadają: sklep.mentzen.pl, panel.mentzen.pl, app.mentzen.pl, akademia.mentzen.pl.

## 10. Wnioski dla przyszłego skilla SEO

1. Serwis jest mały po stronie oferty (17 stron usług + 5 produktów) i duży po stronie bloga (522 wpisy + 33 interpretacje) — przy czym blog nie jest strukturalnie połączony z ofertą.
2. Zero lokalności: jeden adres w Toruniu, brak stron miast, brak `LocalBusiness`. Model jest zdalny i ogólnopolski, więc lokalne SEO jest świadomie (lub przez zaniedbanie) poza strategią.
3. Trzy CPT z czego dwa martwe lub osierocone (`interpretacje`, `praktyka`) — 34 URL-e bez wejścia z nawigacji.
4. Dublujące się ścieżki archiwów (`/blog/<kat>/` i `/blog/category/<kat>/`) plus kolizja slug podkategorii z wpisem to główne nieporządki strukturalne w IA.
5. Regulaminy zdradzają ofertę większą niż widoczna w menu (Nieruchomości, S24, promocje) — potencjał na brakujące strony sprzedażowe.
6. Ton treści blogowych jest celowo nieformalny i żartobliwy (slugi typu `pies-jako-koszt-uzyskania-przychodu`, `inni-lekarze-go-nienawidza`), co jest wyróżnikiem marki, ale oznacza slugi i tytuły oderwane od fraz wyszukiwania.

---

### Materiały robocze

Pobrane sitemapy i HTML (katalog tymczasowy sesji):
`/tmp/claude-1000/-home-wkuczkowski-projects/1e1e322f-19c5-48fe-9762-d3f2da53136f/scratchpad/`
— `post.xml`, `page.xml`, `praktyka.xml`, `interpretacje.xml`, `et_code_snippet_type.xml`, `index.xml`, `robots.txt`, `posts.txt`, `pages.txt`, `blog.html`, `home.html`.
