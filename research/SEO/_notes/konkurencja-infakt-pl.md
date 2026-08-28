# infakt.pl — analiza SEO jako benchmark dla mentzen.pl

Data badania: 2026-08-28. Metoda: WebFetch + surowy HTML (curl), sitemapy, JSON-LD.
Zakres: architektura treści, format artykułów, E-E-A-T, dane strukturalne, powiązanie treści z ofertą, local SEO.

---

## 1. Model biznesowy a SEO

inFakt Sp. z o.o. (Kraków, Szlak 49, założona 2009-03-10, obecnie w grupie Visma) sprzedaje:

- program do księgowości / KSeF (0–49,99 zł/m-c),
- księgowość z osobistym księgowym dla JDG (od 200 zł),
- **pełną księgowość dla spółek — od 619–620 zł netto/m-c**,
- **założenie spółki z o.o. za 199 zł brutto**.

Dwa ostatnie produkty to bezpośrednia kolizja z ofertą Mentzena. Cały ruch contentowy jest pod nie podpięty.

---

## 2. Architektura treści

### 2.1 Trzy warstwy domeny

| Warstwa | Ścieżka | Rola |
|---|---|---|
| Blog (WordPress) | `/blog/` | pozyskanie ruchu informacyjnego, top-of-funnel |
| Landingi usługowe | `/ksiegowosc-dla-spolek/`, `/wygodne-zakladanie-spolki/`, `/spolki-oferta/`, `/cennik/` | konwersja, frazy transakcyjne |
| Warstwa lokalna | `/ksiegowi/` | local SEO + long tail, **3102 URL-e w sitemapie** |

Blog siedzi w podkatalogu `/blog/` na tej samej domenie — cały link equity zostaje w `infakt.pl`. To podstawa modelu i pierwsza rzecz do skopiowania.

### 2.2 Kategorie bloga (10)

`aktualnosci`, `bankowosc`, `formularze`, `jednoosobowa-dzialalnosc`, `koszty`, `ksef`, `na-dobry-start`, `podatki`, `spolki`, `zatrudnienie`.

Kategorie odpowiadają etapom życia firmy, nie taksonomii prawa. `na-dobry-start` → `jednoosobowa-dzialalnosc` → `spolki` to lejek JDG→spółka odwzorowany w URL-ach. Kategoria jest też elementem breadcrumbów i `articleSection` w schemacie.

### 2.3 Klaster „spółka z o.o." — jak jest zbudowany

Kategoria `/blog/kategoria/spolki/` zawiera kilkadziesiąt artykułów. Widoczna struktura pillar/cluster:

**Pillar (hub):** `/blog/spolka-z-o-o-kompendium-krok-po-kroku/`
— „Spółka z o.o. – Wszystko co musisz wiedzieć: wady i zalety, czym jest, kiedy i dla kogo odpowiednia?", **6485 słów**, publikacja 2024-11-21, aktualizacja 2026-06-02.

**Klaster (spokes)** — pojedyncze intencje, każda osobny URL:
- jak założyć spółkę z o.o. krok po kroku (2629 słów)
- księgowość spółki z o.o. — kompendium (~1700 słów)
- spółka z o.o. a JDG — co wybrać
- spółka z o.o. a spółka akcyjna / a prosta spółka akcyjna
- podatki w spółce z o.o.
- podział zysku w spółce z o.o.
- pokrywanie straty w sp. z o.o.
- wynagrodzenie członka zarządu
- podatek PCC w spółce z o.o.
- odpowiedzialność za zobowiązania podatkowe
- prokurent / pełnomocnik w sp. z o.o.
- zawieszenie działalności spółki
- rozwiązanie spółki bez likwidacji
- zatrudnienie wspólnika
- firma (nazwa) spółki z o.o.
- księgowość spółki komandytowej / komandytowo-akcyjnej

Wzorzec: **jedna wątpliwość = jeden URL = jedno pytanie z Google**. Zero kanibalizacji, bo tytuły są rozstrzelone semantycznie (podział zysku ≠ pokrywanie straty ≠ podatki).

### 2.4 Podwójne obstawienie frazy „jak założyć spółkę z o.o."

Dwa osobne URL-e pod tę samą intencję:
- `/blog/jak-zalozyc-spolke-z-o-o-krok-po-kroku/` — artykuł blogowy
- `/zalozenie-spolki-z-o-o-kompendium-wiedzy` — landing w korzeniu domeny

Landing jest hybrydą: ma treść kompendium (charakterystyka, dokumenty, wkłady, KRS, koszty, instrukcja S24), ale sekcje przeplatane są formularzami rejestracyjnymi z polem hasła — czyli **rejestracja konta odbywa się w treści artykułu, bez przekierowania**. To główna przewaga konwersyjna nad zwykłym blogiem: landing bierze ten sam ruch informacyjny i zamyka go w miejscu.

Do tego trzeci URL czysto transakcyjny: `/wygodne-zakladanie-spolki/` (H1 „Załóż spółkę bez wychodzenia z domu", 199 zł, FAQ, 4,6/5 z 191 opinii).

Efekt: trzy pozycje w TOP na wariantach frazy zamiast jednej.

### 2.5 Narzędzia jako osobny silnik ruchu — 23 kalkulatory

`/kalkulatory/` + 22 podstrony. Wybrane, istotne dla lejka:

- `kalkulator-jdg-czy-spolka` — dokładnie decyzja, którą chce wygrać Mentzen
- `kalkulator-wyplat-w-spolce`
- `kalkulator-skladki-zdrowotnej`
- `kalkulator-stawek-ryczaltu`
- `kalkulator-przejscia-uop-b2b`
- `kalkulator-limitu-zwolnienia-z-vat`
- `kalkulator-maly-zus-plus`, `kalkulator-podatkowy`, `kalkulator-wynagrodzen`, `kalkulator-oplacalnosci-najmu`, `kalkulator-zysku-firmowego` i inne

Kalkulatory łapią frazy narzędziowe („kalkulator składki zdrowotnej"), mają wysoki potencjał linkowy i naturalnie prowadzą do oferty. Kalkulatory są linkowane z artykułów blogowych.

Poza tym: `/blog/darmowe-ebooki/` — lead magnet pod newsletter.

---

## 3. Format i struktura topowych artykułów

Przeczytane w całości: `jak-zalozyc-spolke-z-o-o-krok-po-kroku`, `ksiegowosc-spolki-z-o-o-kompendium`, oraz pillar `spolka-z-o-o-kompendium-krok-po-kroku`.

### 3.1 Anatomia artykułu „Jak założyć spółkę z o.o."

- **Title:** „Jak założyć spółkę z o.o. – krok po kroku: przez internet i S24, notariusz, KRS" — fraza główna + trzy modyfikatory dla long tail w jednym tagu.
- **Meta description** w formie trzech pytań: „Jak założyć spółkę z o.o.? Kto może to zrobić? Ile to kosztuje i jakie są wymagania?"
- **Długość:** 2629 słów, 12 min czytania (deklarowane on-page).
- **Spis treści** kotwiczący na górze.

**Drabinka nagłówków — wzorzec do skopiowania:**

```
H1  Jak założyć spółkę z o.o. – krok po kroku: przez internet i S24, notariusz, KRS
H2  Spółka z o.o. – charakterystyka ogólna i zalety
H2  W jaki sposób można założyć spółkę z ograniczoną odpowiedzialnością?
H2  Jakie dokumenty są potrzebne do rejestracji spółki z o.o.?
    H3 Umowa lub statut spółki
    H3 Rejestracja i wpis do KRS
    H3 Wniesienie wkładów własnych
H2  Ile kosztuje założenie spółki z o.o.?
H2  Rejestracja spółki z o.o. przez internet – instrukcja krok po kroku w S24
    H3 ... – Krok 1        (przygotowanie danych)
    H4 Ustalenie nazwy     – Krok 2
    H3 ... – Krok 3..10    (umowa, podpisy, opłata, KRS, PCC, NIP-8, VAT-R, kapitał)
H2  Podsumowanie
H2  FAQ – Najczęściej zadawane pytania o zakładanie spółki z o.o.
```

Kluczowe obserwacje:

1. **Każdy nagłówek zawiera pełną frazę** „spółka z o.o." / „spółki z o.o." — nagłówki są pisane pod dopasowanie, nie pod elegancję. Skrajnie repetytywne, ale skuteczne.
2. **Numerowane kroki w H3** („– Krok 5") — idealny materiał pod featured snippet listowy i pod AI Overviews.
3. **H2 są pytaniami** („Jakie dokumenty…", „Ile kosztuje…") — dopasowanie do zapytań PAA.
4. **FAQ jako osobna sekcja H2** z 4 pytaniami, spięta ze schematem FAQPage:
   - Czy spółkę z o.o. można założyć samemu?
   - Co potrzeba do założenia spółki z o.o.?
   - Kto nie może założyć spółki z o.o.?
   - Czy po założeniu spółki z o.o. można przekształcić ją w JDG?
5. **Konkretne liczby w treści:** 5 000 zł kapitału, 250 zł KRS online / 500 zł tradycyjnie, 100 zł MSiG, PCC ~23,25 zł, notariusz 195 zł netto, łącznie 350 zł online. Cyfry są tym, co Google cytuje w snippetach i co AI Overviews wyciąga.
6. **Boxy „Warto wiedzieć"** — wyróżnione wtrącenia z ikoną, rozbijają ścianę tekstu.
7. Brak tabel porównawczych w tym artykule (poza zestawieniem opłat) — słabszy punkt.

### 3.2 Kadencja aktualizacji

Oba badane artykuły i pillar mają `dateModified` = **2026-06-02** przy różnych datach publikacji (2024-11-21, 2024-11-28). To sygnał zorganizowanego sprintu odświeżeniowego całego klastra „spółki" — cały cluster refreshowany razem, nie artykuł po artykule.

**Ale:** on-page widoczna jest tylko data publikacji („28 listopada 2024"), data aktualizacji siedzi wyłącznie w JSON-LD. Użytkownik w SERP-ie i na stronie widzi treść sprzed 18 miesięcy. To ich błąd, nie wzorzec.

---

## 4. Sygnały E-E-A-T

### 4.1 Co robią

- **Autor podpisany imiennie** przy każdym artykule, z linkiem do strony autora (4 linki do `/blog/author/maciej-sztykiel/` z jednego artykułu: nagłówek, box autora, itd.).
- **Box autora pod tekstem** z awatarem i bio: „Redaktor naczelny inFakt Blog. Były dziennikarz ekonomiczny RMF FM oraz Radia ZET. W inFakcie odpowiadam za tłumaczenie języka podatkowego na ludzki…"
- **Strona autora** z pełnym archiwum i schematem `ProfilePage` + `Person`.
- **Sygnały organizacyjne** w schemacie na każdej stronie: `Corporation` z NIP-owym adresem, telefonem, godzinami wsparcia 7:00–22:00 (pon.–sob.), `sameAs` do 6 profili społecznościowych (FB, YouTube, X, TikTok, Instagram, LinkedIn).
- **Sekcja „Media o nas"** na stronie `/ksiegowi/`.
- **Twarze księgowych z ocenami** na landingu usługowym (imię, nazwisko, zdjęcie, specjalizacja, ocena) — to realny sygnał „experience".

### 4.2 Czego NIE robią (luka do wykorzystania)

**Brak recenzji merytorycznej.** Artykuł o rejestracji spółki, PCC, NIP-8 i VAT-R jest podpisany przez byłego dziennikarza radiowego, a nie przez doradcę podatkowego, radcę prawnego czy księgowego z licencją. Nie ma pola „zweryfikowane merytorycznie przez ___", nie ma numeru wpisu na listę doradców podatkowych, nie ma `reviewedBy` w schemacie.

W tematyce YMYL (finanse, prawo) to najsłabszy punkt inFaktu i **największa dźwignia dla Mentzena**, który ma na pokładzie prawdziwych doradców podatkowych i radców prawnych.

---

## 5. Dane strukturalne (schema.org)

Na `/blog/jak-zalozyc-spolke-z-o-o-krok-po-kroku/` są **trzy niezależne bloki `application/ld+json`**:

**Blok 1 — Yoast SEO `@graph`:**
- `Article` (author jako referencja `@id`, `wordCount: 2629`, `articleSection: ["Spółki"]`, `inLanguage: pl-PL`, `datePublished`, `dateModified`)
- `WebPage` z **podwójnym typem `["WebPage","FAQPage"]`** i `mainEntity` → 4 węzły `Question`/`Answer`
- `ImageObject`, `BreadcrumbList` (Blog → Spółki → tytuł), `WebSite` z `SearchAction` (sitelinks searchbox), `Person` (autor z bio i URL profilu)

**Blok 2 — własna wtyczka `infakt-rating`:**
```json
{"@type":"Article","aggregateRating":{"ratingValue":4.7,"ratingCount":"33",
 "itemReviewed":{"@type":"CreativeWorkSeries","name":"Jak założyć spółkę…"}}}
```
Gwiazdki w SERP-ie z głosowania czytelników. To **ryzykowna taktyka** — self-serving AggregateRating na własnym contencie, poza dozwolonymi typami; Google potrafi to zignorować lub potraktować jako spam strukturalny.

**Blok 3 — ręcznie wstawiony `@graph`:**
- `Corporation` (inFakt Sp. z o.o., adres, telefon, `foundingDate`, `hoursAvailable`, `sameAs` ×6)
- `NewsArticle` — **duplikat tego samego artykułu pod innym typem**, z autorem jako `Person`

Podsumowanie: ten sam artykuł jest opisany jednocześnie jako `Article` (Yoast), `Article` (rating) i `NewsArticle` (ręczny). Trzy autorzy w trzech blokach (`Person: Maciej Sztykiel` vs `Organization: Infakt Blog`). Sprzeczne i nadmiarowe — działa, ale to bałagan po migracjach, a nie wzorzec.

**Co warto skopiować:** FAQPage na artykułach, BreadcrumbList z kategorią, Person autora z bio i URL, Organization z NAP + godzinami + sameAs, WebSite z SearchAction.
**Czego nie kopiować:** AggregateRating na artykule, dublowanie Article/NewsArticle.

---

## 6. Powiązanie treści z ofertą — jak działa konwersja

To najmocniejsza część ich systemu.

### 6.1 Linkowanie z artykułu do oferty

Z jednego artykułu blogowego wychodzą linki do:
- `/wygodne-zakladanie-spolki/` (landing zakładania spółki)
- `/spolki-oferta/?accountant=blog` — **z parametrem atrybucji źródła**
- `/ksiegowosc-dla-spolek/`
- **imienne profile księgowych** (`/ksiegowi/monika-dolata-tluszcz`, `/ksiegowi/katarzyna-jozefczak`, `/ksiegowi/paulina-klauzinska`) z rozbudowanym query stringiem `pricing_bundle[...]` prekonfigurowującym pakiet dla spółki
- `/program-do-ksef/`, `/program-do-ksiegowosci/`, `/cennik/`

Parametry `?pricing_bundle` są **zablokowane w robots.txt** (`Disallow: *?pricing_bundle`) — linki służą konwersji, nie indeksowaniu. Czysto rozwiązane.

### 6.2 Elementy konwersji w treści artykułu

Zliczone na jednej stronie:
- baner „Księgowość dla spółek? Skorzystaj z pomocy najlepszych!"
- tabela z trzema księgowymi (zdjęcie, ocena, cena od 619 zł netto/m-c) — soft-sell w środku artykułu
- formularz „Zamów rozmowę z księgowym" (imię, nazwisko, telefon, email)
- formularz indywidualnej oferty („Chcę otrzymać ofertę")
- CTA „Chcę założyć spółkę"
- pop-up konsultacyjny
- zapis do newslettera
- linki wewnętrzne do 5 pokrewnych artykułów (recyrkulacja ruchu w klastrze)

Ton nie jest agresywny — oferta jest wpleciona jako rozwiązanie problemu opisanego akapit wyżej („to skomplikowane → oto ktoś, kto to zrobi za 619 zł").

### 6.3 Asymetria: blog linkuje do oferty, oferta nie linkuje do bloga

`/ksiegowosc-dla-spolek/` **nie ma linków do artykułów blogowych** ani do poradników. Landing jest krótki, bez rozbudowanej treści SEO, z 6 kartami cenowymi, 3 księgowymi i 2 opiniami klientów, bez FAQ. To znaczy, że landing usługowy sam z siebie rankuje słabo i **żyje z link juice'u z bloga oraz z płatnego ruchu**.

To luka. Landing na frazę „księgowość spółki z o.o." bez treści i bez FAQ jest do pobicia contentowo.

### 6.4 Landing hybrydowy jako broń

`/zalozenie-spolki-z-o-o-kompendium-wiedzy` łamie ten podział: ma pełną treść kompendium + formularz rejestracji konta wstawiony po każdej głównej sekcji. Ten typ strony — nie blog, nie czysty landing — jest tym, co realnie wygrywa u nich frazę transakcyjno-informacyjną.

---

## 7. Local SEO — skala programatyczna

`https://www.infakt.pl/ksiegowi/sitemap.xml` → **3102 URL-e**:

- **~445 stron miejscowości** (`/ksiegowi/warszawa`, `/ksiegowi/krakow`, ale też `/ksiegowi/kleczewo`, `/ksiegowi/julianow`, `/ksiegowi/emilcin`, `/ksiegowi/wyzral` — wsie kilkusetosobowe, a nawet `/ksiegowi/paryz`)
- **~2657 stron profili księgowych** (`/ksiegowi/monika-dolata-tluszcz`)

**Strona miasta** (`/ksiegowi/warszawa`):
- Title generowany dynamicznie z licznikiem: „Księgowi Warszawa - 32 polecanych księgowych"
- H2: „Polecani Księgowi: Warszawa i okolice (32)"
- Rozbudowany panel filtrów (typ działalności, sposób rozliczania, VAT, liczba dokumentów, branża, pracownicy, przychody spółki, konto walutowe, środki trwałe, VAT UE, zasiłek macierzyński, obsługa w języku obcym, dojazd do klienta)
- Sekcja pytań: „Czym się kierować przy wyborze księgowego?", „Czy muszę mieć księgowego ze swojej miejscowości?"

**Strona księgowego** (`/ksiegowi/monika-dolata-tluszcz`):
- H1 = imię i nazwisko, formularz „Wyślij do mnie pytanie", „Jak działa usługa", „Opinie (7)"
- ~15 KB, bardzo lekka

**Słabości warstwy lokalnej:**
- **Zero danych strukturalnych** na stronach miast i na profilach księgowych (`LD typy: []`). Brak `LocalBusiness`, brak `Person`, brak `AggregateRating` tam, gdzie akurat byłby uprawniony — a jest wciśnięty na artykuły blogowe, gdzie nie jest. Odwrotnie niż powinno.
- Setki miejscowości bez realnie przypisanego księgowego = cienka treść generowana z szablonu. Ryzyko przy Helpful Content / spam updates.
- Meta description identyczna na wszystkich stronach lokalnych („Tylko sprawdzone biura księgowe i polecani Księgowi. Porównaj opinie ich Klientów. Dopasuj ofertę do siebie.").

Mimo słabości: 3000 indeksowalnych URL-i pod frazy „księgowy + miasto" to potężny long tail i realne źródło ruchu z intencją zakupową.

---

## 8. Techniczne drobiazgi

- `robots.txt` bardzo szczegółowy: blokuje `/tagi/` (tagi WP wycięte z indeksu — dobra decyzja anty-duplikacyjna), parametry `?utm_source=`, `?gclid=`, `?preview=`, `?tab=`, `?pricing_bundle`, `*?amp`, `/blog/ajax-posts`, `/blogstaging/`, `*/feed`, `/blog-ksiegowy/`.
- Deklarowane 5 sitemap: `/sitemap.xml`, `/ksiegowi/sitemap.xml`, `/blog/sitemap_index.xml`, `/sitemap_index.xml`, `sitemap-news.xml` (news sitemap = kandydowanie do Google News z kategorii „Aktualności").
- Blog na WordPressie z Yoastem, główny serwis to osobna aplikacja — spójność techniczna niepełna (stąd trzy niezgodne bloki JSON-LD).
- Sitemapy i część zasobów zwracają 403 dla nietypowych user-agentów (ochrona anty-bot).
- Kanoniczne self-referencing, `inLanguage: pl-PL`, fonty własne (Basier Circle) z preload.
- Widżet oceny artykułu (★ 4,7 / 33 głosy) wyświetlany pod tytułem — realny sygnał zaangażowania + wsad do schematu.

---

## 9. Wnioski dla mentzen.pl

### 9.1 Co zaadaptować

**A. Struktura pillar → cluster → landing, jeden URL na jedną wątpliwość.**
Zbudować hub „spółka z o.o." (5–7 tys. słów) i pod niego 15–25 artykułów satelitarnych, każdy na jedną decyzję/pytanie: podział zysku, wynagrodzenie zarządu, PCC, odpowiedzialność członka zarządu, estoński CIT, zawieszenie, likwidacja, zatrudnienie wspólnika. Wszystkie linkują do huba i do landingu usługowego. Analogicznie dla „fundacja rodzinna", „przekształcenie JDG w spółkę", „estoński CIT".

**B. Blog w podkatalogu `mentzen.pl/blog/`, nigdy na subdomenie.**
inFakt trzyma cały autorytet contentowy na domenie sprzedażowej. To fundament, na którym opiera się reszta.

**C. Wzorzec nagłówków: pytania + numerowane kroki + pełna fraza w każdym H2/H3.**
H2 formułowane jako pytania z PAA, procedury rozbite na „– Krok 1…N", konkretne kwoty i terminy w treści. To materiał pod featured snippets i AI Overviews.

**D. Landing hybrydowy pod frazy transakcyjno-informacyjne.**
Osobna strona w korzeniu domeny (nie na blogu) łącząca kompendium wiedzy z formularzem kontaktowym po każdej sekcji. Pod „zakładanie spółki z o.o." i „księgowość spółki z o.o." — dwie takie strony obok artykułów blogowych. Podwójne obstawienie frazy działa.

**E. Kalkulatory jako osobny silnik.**
Priorytet: „JDG czy spółka" (kalkulator porównawczy z estońskim CIT-em), „składka zdrowotna", „wypłata ze spółki — dywidenda vs. wynagrodzenie vs. powtarzające się świadczenia niepieniężne", „koszt przekształcenia JDG w spółkę". Frazy narzędziowe, wysoka linkowalność, twarda intencja komercyjna.

**F. FAQPage + BreadcrumbList + Person + Organization w schemacie na każdym artykule.**
FAQ jako realna sekcja H2 na dole tekstu, 4–6 pytań, spięta z `mainEntity`. Breadcrumby z kategorią. Organizacja z NAP, godzinami i `sameAs`.

**G. Sekcja FAQ i realna treść na landingach usługowych.**
Landing inFaktu na „księgowość dla spółek" jest ubogi i bez FAQ — to punkt, w którym można go pokonać stroną z rozbudowaną treścią, FAQ ze schematem, tabelą porównawczą pakietów i case'ami.

**H. Regularne, wsadowe odświeżanie klastrów.**
inFakt odświeżył cały klaster „spółki" jednego dnia (2026-06-02). Warto ustawić kwartalny cykl przeglądu klastra przy każdej zmianie przepisów — i **wyświetlać datę aktualizacji on-page**, czego inFakt nie robi.

**I. Twarze i profile ekspertów jako strony indeksowalne.**
Profile doradców podatkowych / radców z bio, specjalizacją, opiniami i formularzem — z poprawnym schematem `Person`. U Mentzena to jest naturalna przewaga: prawdziwi doradcy z licencją, a nie dziennikarze.

**J. Wycięcie tagów z indeksu (`Disallow: /tagi/`) i blokada parametrów.**
Prosta higiena, którą inFakt ma zrobioną poprawnie.

### 9.2 Gdzie inFakt jest słaby — tam bić

| Luka inFaktu | Ruch dla Mentzena |
|---|---|
| Autorem treści YMYL jest były dziennikarz radiowy; brak recenzji merytorycznej i brak `reviewedBy` | Podpis doradcy podatkowego z numerem wpisu + widoczne „Zweryfikował merytorycznie: ___" + `reviewedBy` w schemacie. Najmocniejsza dostępna dźwignia E-E-A-T. |
| Landing usługowy bez treści, bez FAQ, bez linków do bloga | Landing z pełną treścią, FAQ ze schematem i linkami w obie strony. |
| Brak `LocalBusiness`/`Person` na 3000 stron lokalnych i profilach | Mniej stron, ale z poprawnym schematem i realną treścią o oddziale. |
| Widoczna data publikacji sprzed 18 miesięcy przy świeżym `dateModified` | Widoczna data ostatniej aktualizacji + krótkie „co się zmieniło". |
| Treść generyczna, „przepisana z ustawy" — brak interpretacji i stanowiska | Mentzen ma markę opiniotwórczą: analiza „co to znaczy dla ciebie", stanowisko wobec zmian, interpretacje KIS. Tego inFakt z definicji nie zrobi. |
| Brak tabel porównawczych i kalkulacji wariantowych w artykułach | Tabele „JDG vs. sp. z o.o. vs. estoński CIT" z policzonymi wariantami — magnes na snippety i linki. |
| Cienkie strony wiejskich miejscowości = ekspozycja na spam/HCU update | Nie kopiować tej skali. Local tylko tam, gdzie są realne oddziały. |

### 9.3 Czego NIE kopiować

1. **`AggregateRating` na artykułach blogowych** (gwiazdki z głosowania czytelników w `Article`) — self-serving review markup poza dozwolonymi typami. Ryzyko manual action, zysk krótkoterminowy.
2. **Trzy niezgodne bloki JSON-LD na jednej stronie**, w tym duplikat `Article` + `NewsArticle` i sprzeczne pola `author` (raz `Person`, raz `Organization`). Jeden spójny `@graph` na stronę.
3. **Programatyczne strony dla setek miejscowości bez realnej treści i bez księgowego** — to dług techniczny czekający na core update.
4. **Skrajnie repetytywne nagłówki** („spółki z o.o." w każdym H2/H3) — dopasowanie działa, ale czytelność cierpi; Mentzen sprzedaje głosem, nie schematem, i keyword stuffing w nagłówkach uderzy w markę.
5. **Ukrywanie daty aktualizacji przed użytkownikiem** przy jednoczesnym raportowaniu jej Google'owi.
6. **Landing usługowy bez treści** — pozornie „czysty konwersyjnie", w praktyce niezdolny do samodzielnego rankowania.

### 9.4 Kolejność działań (propozycja)

1. Audyt i przebudowa dwóch landingów transakcyjnych: „księgowość spółki z o.o." i „zakładanie spółki z o.o." — treść + FAQ + schema + linkowanie w obie strony z blogiem.
2. Hub „spółka z o.o." + 10 pierwszych artykułów satelitarnych z podpisem doradcy podatkowego i widoczną recenzją merytoryczną.
3. Kalkulator „JDG czy spółka" z uwzględnieniem estońskiego CIT-u — jako flagowe narzędzie i magnes linkowy.
4. Schema: `FAQPage` + `BreadcrumbList` + `Person` z `reviewedBy` + `Organization` jako jeden spójny `@graph`.
5. Cykl kwartalnego odświeżania klastra z widoczną datą aktualizacji.
6. Dopiero potem: warstwa lokalna, wyłącznie dla realnych oddziałów.

---

## Źródła

- https://www.infakt.pl/blog/jak-zalozyc-spolke-z-o-o-krok-po-kroku/
- https://www.infakt.pl/blog/spolka-z-o-o-kompendium-krok-po-kroku/
- https://www.infakt.pl/blog/ksiegowosc-spolki-z-o-o-kompendium/
- https://www.infakt.pl/blog/kategoria/spolki/
- https://www.infakt.pl/blog/ i https://www.infakt.pl/blog/author/maciej-sztykiel/
- https://www.infakt.pl/zalozenie-spolki-z-o-o-kompendium-wiedzy
- https://www.infakt.pl/wygodne-zakladanie-spolki/
- https://www.infakt.pl/ksiegowosc-dla-spolek/
- https://www.infakt.pl/ksiegowi/ , /ksiegowi/warszawa , /ksiegowi/monika-dolata-tluszcz
- https://www.infakt.pl/kalkulatory/
- https://www.infakt.pl/robots.txt , https://www.infakt.pl/ksiegowi/sitemap.xml
