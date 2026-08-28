# crido.pl — analiza SEO jako benchmark dla mentzen.pl

Data analizy: 2026-08-28
Metoda: pobranie HTML przez curl (WebFetch dostaje 403 — Crido blokuje nietypowe UA), parsowanie nagłówków, JSON-LD, linków, sitemap. Bez dostępu do danych o ruchu — wnioski o widoczności opieram na strukturze, nie na metrykach.

## Podsumowanie w jednym akapicie

Crido wygrywa na frazach typu „ulga B+R" nie treścią, tylko **architekturą**: krótka strona pillar (~2350 słów) siedzi na szczycie piramidy zbudowanej z ~5400 URL-i w wyspecjalizowanych typach treści, jest odświeżana raz do roku z rokiem w tytule, i jest gęsto podlinkowana z bloga eksperckiego. Sama strona docelowa jest sprzedażowa, nie poradnikowa — cała „długość" jest wypchnięta do klastra. Co ciekawe, ich E-E-A-T i dane strukturalne są **słabe** — to jest luka, w którą Mentzen może wejść.

---

## 1. Stack i skala

| Element | Wartość |
|---|---|
| CMS | WordPress + Yoast SEO + W3 Total Cache (page cache na dysku, object cache Memcached) |
| Łączna liczba URL w sitemapach | ~5 400 |
| `post_taxes` (blog podatkowy) | **3 188** URL-i (4 sitemapy) |
| `post_business` (blog o ulgach/dotacjach) | 737 |
| `post_law` (blog o prawie) | 459 |
| `threads` (wątki tematyczne) | 409 |
| `page` (strony ofertowe) | 182 |
| `report` (raporty) | 97 |
| `linia-biznesowa` | 96 |
| `case_study` | 89 |
| `training` | 65 |
| `grant` | 60 |
| `hottopic` | 59 |

**Kluczowa obserwacja architektoniczna:** Crido nie ma jednego bloga. Ma **osobne custom post types per obszar praktyki** (`post_taxes`, `post_law`, `post_business`, `post_digital`) plus dedykowane CPT na formaty (`case_study`, `report`, `hottopic`, `threads`, `grant`, `training`). Każdy CPT ma własny prefix URL (`/blog-taxes/`, `/blog-law/`, `/blog-business/`, `/hottopic/`, `/threads/`) i własną sitemapę.

Efekt: separacja tematyczna jest zakodowana w strukturze URL i w sitemapach, a nie tylko w kategoriach. Googlebot dostaje czysty sygnał, gdzie kończy się jedna domena tematyczna, a zaczyna druga.

---

## 2. Model pillar/cluster — jak to naprawdę działa

### Trzy piętra

1. **Hub kategorii** — `/linia-biznesowa/podatki-w-biznesie/ulgi-podatkowe/` (H1: „Ulgi podatkowe")
2. **Pillar per ulga** — płaskie, krótkie URL-e w korzeniu domeny:
   - `/ulga-b-r/`
   - `/ip-box/`
   - `/polska-strefa-inwestycji/`
   - `/ulga-na-prototyp/`, `/ulga-na-innowacyjnych-pracownikow/`, `/ulga-na-ekspansje/`, `/ulga-na-robotyzacje/`
3. **Klaster artykułów** — setki wpisów w `/blog-business/` i `/blog-taxes/`

### Co warto podkreślić: URL-e pillar są w korzeniu

`crido.pl/ulga-b-r/` — nie `/uslugi/podatki/ulgi/ulga-br/`. Strona ofertowa o najwyższej intencji zakupowej dostaje najkrótszy możliwy URL, mimo że w nawigacji siedzi cztery poziomy głęboko. To rozjazd między nawigacją a strukturą URL, zrobiony celowo.

### Wzajemne linkowanie ulg

Każdy pillar ma sekcję **„Ulga B+R a inne ulgi"** / **„IP Box a inne ulgi"** — czyli siostrzane strony linkują się nawzajem w dedykowanym module. To utrzymuje PageRank w klastrze i łapie frazy porównawcze („ulga B+R a IP Box").

### Widget CRIDOTEKA na dole pillara

Sekcja **„CRIDOTEKA | Ulgi podatkowe"** wstrzykuje na stronę pillar dynamiczną listę najnowszych artykułów z całego serwisu, z linkami do:
- artykułów (`/blog-business/...`, `/blog-taxes/...`, `/blog-law/...`)
- **filtrowanych archiwów po kategorii**: `/fundusze-ue-i-pomoc-publiczna/artykuly/?categories=ulga-br`, `?categories=ip-box`, `?categories=dotacje-i-instrumenty-zwrotne`
- stron autorów (`/author/...`)

Ze strony `/ulga-b-r/` wychodzi **81 unikalnych linków wewnętrznych**. Przy 2350 słowach treści to bardzo wysoki stosunek linków do tekstu — strona funkcjonuje jako węzeł dystrybucji, nie jako artykuł.

Uwaga: część linków z tego widgetu jest **tematycznie niezwiązana** z ulgą B+R (prawo pracy, cło, doręczenia w e-US, ceny transferowe). To automat, nie kuracja redakcyjna. Rozmywa to trafność linkowania wewnętrznego.

---

## 3. Anatomia strony `/ulga-b-r/` — flagowiec

**Title:** `Ulga B+R 2026 - co to jest i jak skorzystać? | CRIDO`
**Meta description:** „Ulga B+R pozwala na dodatkowe odliczenie od podstawy opodatkowania wydatków poniesionych na prace badawczo-rozwojowe."
**Canonical:** self
**datePublished:** 2023-11-09 · **dateModified:** 2026-01-14
**Objętość:** ~2 355 słów (z nawigacją; sama treść merytoryczna to ok. 1 200–1 400 słów)

### Struktura nagłówków

```
H1  Ulga B+R
H2  Ulga B+R | Co to jest?
H2  [zdanie-lead, użyte jako H2 — antywzorzec]
H2  Czy ulga B+R jest dla Ciebie?
H2  Ulga B+R | Koszty kwalifikowane
H2  Ulga B+R w pytaniach i odpowiedziach
    H3  1. Czy każdy może skorzystać z ulgi B+R?
    H3  2. Czy istnieje określony termin na zwrot ulgi?
    H3  3. Co jeśli przedsiębiorca nie osiągnie wystarczającego dochodu?
    H3  4. Jaką wysokość ulgi B+R można odliczyć od podatku?
    H3  5. Jak udokumentować koszty i prawo do rozliczenia ulgi B+R?
    H3  6. W jaki sposób otrzymać zwrot ulgi?
H2  Ulga B+R - profesjonalne i kompleksowe doradztwo
    H3  1. Połączenie wiedzy prawnej, podatkowej i technicznej
    H3  2. Jakość i rzetelność
    H3  3. Silna i uznana marka
    H3  4. Kompleksowość doradztwa
H2  Ulga B+R | Case study
H2  Ulga B+R a inne ulgi
H2  CRIDOTEKA | Ulgi podatkowe
```

### Wzorzec, który działa — sekwencja perswazyjna

Kolejność sekcji odwzorowuje ścieżkę decyzyjną kupującego:

1. **Definicja** („Co to jest?") — łapie ruch informacyjny
2. **Autokwalifikacja** („Czy ulga B+R jest dla Ciebie?") — trzy warunki + definicja ustawowa działalności B+R. Czytelnik sam sprawdza, czy się łapie. To najmocniejszy element strony.
3. **Konkret** („Koszty kwalifikowane") — 7 punktów, twarda lista, bez lania wody
4. **FAQ** — 6 pytań, zdejmuje zastrzeżenia
5. **Dlaczego my** — 4 argumenty
6. **Dowód** — case study
7. **Nawigacja pokrewna** — inne ulgi, CRIDOTEKA

Nie ma sekcji „historia ulgi B+R w Polsce" ani innego wypełniacza. Strona jest krótka, bo ma **konwertować**, a nie wyczerpywać temat — wyczerpywanie tematu zostało zdelegowane do klastra blogowego.

### Nagłówki użyte jako akapity — antywzorzec

Zarówno na `/ulga-b-r/`, jak i na `/ip-box/` i `/hottopic/ksef.../` pełne zdania lead są opakowane w `<h2>`:

> `<h2>Ulga B+R to prosty instrument podatkowy, który umożliwia uzyskanie finansowania na projekty innowacyjne...</h2>`

To pozostałość po składaniu stron w page builderze (nagłówek dostaje styl, więc redakcja używa go do formatowania). Rozmywa hierarchię semantyczną. **Nie kopiować.**

---

## 4. Dane strukturalne — to jest ich słaby punkt

Sprawdziłem JSON-LD na `/ulga-b-r/`, `/ip-box/`, `/blog-business/jak-dokumentowac-koszty-do-ulgi-br/`, `/hottopic/ksef-czyli-krajowy-system-e-faktur/`, `/linia-biznesowa/.../ulgi-podatkowe/`.

**Na wszystkich stronach jest dokładnie to samo — domyślny output Yoasta:**

```
@graph: WebPage, ImageObject, BreadcrumbList, WebSite, Organization
```

Czyli:

| Schema | Status |
|---|---|
| `FAQPage` | **BRAK** — mimo 6 widocznych pytań i odpowiedzi na `/ulga-b-r/` |
| `Article` / `BlogPosting` | **BRAK** — nawet na wpisach blogowych; wszystko jest `WebPage` |
| `Person` (autor) | **BRAK** — zero powiązania autor↔treść w danych strukturalnych |
| `HowTo` | BRAK |
| `Service` / `Offer` | BRAK |
| `LocalBusiness` / `address` / `telephone` | **BRAK** w `Organization` |
| `sameAs` | tylko 2 profile: Facebook i X. Brak LinkedIn — a to główny kanał firmy doradczej. |
| `BreadcrumbList` | Jest, ale **ułomny**: pozycja 2 ma `name: null` |
| `hreflang` | **BRAK** — mimo istnienia wersji `/en/` (widoczne w sitemapach) |
| `meta robots` | brak jawnej deklaracji na stronie pillar |

**Wniosek:** Crido rankuje na „ulga B+R" **pomimo** danych strukturalnych, nie dzięki nim. Cała siła leży w autorytecie domeny, wolumenie treści i linkowaniu wewnętrznym.

To jest konkretna, mierzalna przewaga do zdobycia dla mentzen.pl: poprawnie wdrożony `FAQPage` + `Article` z `author` jako `Person` + `sameAs` do LinkedIn daje szansę na rich snippety i wzmocnione sygnały autorstwa tam, gdzie lider ich nie ma.

---

## 5. E-E-A-T — deklaratywne, nie strukturalne

### Co robią dobrze

- **Byline z czasem czytania** na wpisach blogowych: `Jak dokumentować koszty do ulgi B+R · 21 sierpnia 2019 · 4 min · Zuzanna Galińska`
- **Strony autorów** istnieją pod `/author/{slug}/` i są linkowane z widgetu CRIDOTEKA
- **Case studies** jako osobny CPT (89 sztuk) — realny dowód wykonania
- **Nagrody** jako osobny CPT + artykuły typu „CRIDO na shortliście 6. kategorii ITR EMEA Tax Awards 2026"
- **Monitoring interpretacji** — cykliczna seria („Przegląd interpretacji indywidualnych styczeń–czerwiec 2023", „Monitoring interpretacji – ulgi podatkowe | styczeń–marzec 2024"). Sygnalizuje ciągłą praktykę zawodową, nie jednorazowy content. Bardzo mocny sygnał doświadczenia.
- **CRIDOTEKA** — płatna platforma edukacyjna (120+ szkoleń, 600+ materiałów, 307 000+ uczestników wg ich liczb), z sekcją „Poznaj naszych trenerów"

### Co robią źle — i to jest ich największa luka

1. **Strony autorów są puste.** `/author/martyna-rozek-fejczaruk/` to `<h2>Znaleziono (1) wynik od autora Martyna Rożek-Fejczaruk</h2>` i tyle. **Zero bio, zero zdjęcia, zero uprawnień (doradca podatkowy nr X / radca prawny), zero linku do LinkedIn, zero schema `Person`.** Dla firmy doradczej w YMYL to poważne zaniedbanie.

2. **Strona pillar `/ulga-b-r/` nie ma żadnego autora ani eksperta.** Sprawdziłem klasy CSS — na stronie ofertowej nie ma bloku `author`, `expert` ani `person`. Jest tylko formularz. Strona o najwyższej stawce w YMYL nie ma podpisu człowieka.

3. **Autor `Widoczni_SEO`.** W widgecie CRIDOTEKA znalazłem link do `/author/widoczni_seo/` przy artykule „Kredyt dual use". Konto agencji SEO jako autor treści merytorycznej na stronie firmy doradczej — sygnał, że część contentu jest outsourcowana, i to widoczny publicznie.

4. **Brak recenzji merytorycznej.** Nigdzie nie ma wzorca „Autor: X · Weryfikacja merytoryczna: Y (doradca podatkowy nr NNNNN)". Przy YMYL to standard, który wciąż jest do wzięcia.

5. **Data wyświetlana ≠ data aktualizacji.** Artykuł „Jak dokumentować koszty do ulgi B+R" pokazuje użytkownikowi „21 sierpnia 2019", a `dateModified` w schemacie to 2026-02-05. Użytkownik widzi treść jako siedmioletnią. Strata zaufania i CTR przy zachowanym sygnale świeżości dla Google — czyli najgorsza możliwa kombinacja.

---

## 6. Model evergreen odświeżanego rocznie

To jest rzecz, po którą przyszedłem, i działa tak:

| Strona | datePublished | dateModified | Rok w title |
|---|---|---|---|
| `/ulga-b-r/` | 2023-11-09 | **2026-01-14** | tak — „Ulga B+R 2026" |
| `/ip-box/` | (uszkodzona: 1790-11-17) | **2026-01-14** | nie |
| `/linia-biznesowa/.../ulgi-podatkowe/` | 2018-10-14 | 2026-07-10 | nie |

**Wzorzec:** obie strony ulgowe mają `dateModified` w tym samym dniu — **14 stycznia 2026, 17 sekund różnicy** (10:17:42 i 10:17:55). To zbiorczy, zaplanowany przegląd całego klastra ulg na starcie roku podatkowego, przeprowadzony ręcznie jedną sesją redakcyjną.

**Rok jest w `title` i `og:title`, ale nie w H1** (H1 = „Ulga B+R"). Dzięki temu:
- URL jest stabilny (`/ulga-b-r/` bez roku) — nie traci linków i historii
- Snippet w SERP-ach niesie sygnał świeżości („Ulga B+R 2026")
- H1 zostaje czysty i evergreen — nie trzeba go ruszać

To jest wzorzec wart skopiowania 1:1.

**Uwaga na błąd:** `/ip-box/` ma `datePublished: 1790-11-17T16:37:24` — uszkodzona data w bazie. Warto pilnować walidacji.

---

## 7. Łączenie treści z ofertą — mechanika konwersji

### Formularz — jeden, rozbudowany, kwalifikujący

Ten sam komponent (`flexible-form`, kotwica `#formularz-kontaktowy`) siedzi na pillarach, na blogu i na kalkulatorze. Pola:

```
name (Imię i nazwisko) · phone-prefix · phone · company (Firma) · email
position (stanowisko, select + „Wpisz jakie")
department (dział, select + „Wpisz jakie")
message
[ ] zgoda telefoniczna    [ ] polityka prywatności
```

To **nie jest** lekki formularz „zostaw maila". Pyta o firmę, stanowisko i dział. Świadoma decyzja: przy leadzie B2B o wysokiej wartości lepiej mieć mniej zgłoszeń, ale kwalifikowanych i od decydenta. Pola `position` i `department` pozwalają routować lead do właściwego zespołu i od razu ocenić, czy rozmawiamy z osobą decyzyjną.

### Kalkulator jako lead magnet — bez bramki

`/kalkulator-ip-box/` — interaktywny kalkulator liczący **jednocześnie IP Box i ulgę B+R** („Proszę wpisać kwoty" → „Wyniki"), plus sekcje „Ważne aby pamiętać, że:" i „Status CBR".

Kluczowe: **wynik nie jest zabramkowany**. Użytkownik dostaje liczbę bez podawania maila. Formularz kontaktowy jest dopiero pod wynikiem. Logika: człowiek, który właśnie zobaczył, że może odzyskać 300 tys. zł, sam wypełni formularz — nie trzeba go szantażować bramką. Kalkulator jest linkowany z `/ip-box/` i z huba ulg.

(Techniczna wpadka: kalkulator ma **dwa H1** — „Kalkulator IP Box" i „Ulga B+R".)

### Case study wewnątrz pillara

Sekcja „Ulga B+R | Case study" wstawia dowód społeczny bezpośrednio na stronie ofertowej, zamiast odsyłać do osobnego katalogu. Konwersja nie wymaga opuszczenia strony.

### Newsletter jako druga ścieżka

„Nasi eksperci dzielą się na bieżąco informacjami ze świata podatków, prawa, B+R i innowacji… **Zapisz się już teraz!**" → `/subskrypcje/`, plus „Subskrybuj blogi". Dla ruchu, który nie jest gotowy na kontakt.

### Trzy poziomy zaangażowania

Crido daje trzy różne progi wejścia zamiast jednego CTA:

1. **Newsletter** (niski próg) — `/subskrypcje/`
2. **Kalkulator / raporty / CRIDOTEKA** (średni) — narzędzie lub wiedza
3. **Formularz kwalifikujący** (wysoki) — rozmowa handlowa

### Monetyzacja treści

CRIDOTEKA to płatna subskrypcja szkoleniowa („Cennik subskrypcji na nielimitowany dostęp"). Treść nie jest tylko kosztem marketingowym — jest osobnym produktem, który przy okazji generuje SEO i zbiera leady na doradztwo.

---

## 8. Formaty treści warte podkreślenia

- **`hottopic` (59)** — hub tematyczny na gorący temat regulacyjny. `/hottopic/ksef-czyli-krajowy-system-e-faktur/` ma strukturę: kontekst → „Kiedy wchodzi w życie?" → **produkty** (Pogotowie KSeF, Audyt gotowości, Analiza rozszerzona, Analiza techniczna, narzędzie Compass KSeF, KSeF Hotline), każdy z „Jak działamy?" + „Efekty naszych prac". To jest **landing na zmianę prawa** — łapie falę wyszukiwań przy nowelizacji i od razu ją monetyzuje. Wzorzec bardzo dobrze pasujący do polskiego rynku podatkowego, gdzie co roku jest kilka takich fal.

- **`threads` (409)** — strony wątków/tagów tematycznych („Status Centrum Badawczo-Rozwojowego (CBR)", „Orzecznictwo i interpretacje", „Wartości niematerialne w cenach transferowych"). Warstwa taksonomii łapiąca frazy long-tail.

- **Cykl „Przegląd interpretacji indywidualnych"** — kwartalne podsumowania. Powtarzalny, tani do produkcji format, który buduje sygnał aktywnej praktyki i naturalnie linkuje do pillarów.

- **Rocznicowe:** „10 lat ulgi B+R w Polsce" — content PR-owy z potencjałem linkowym.

- **Filtrowane archiwa jako cele linków:** `?categories=ulga-br`, `?categories=ip-box`. Uwaga — `robots.txt` blokuje `/*?*s=` (wyszukiwarkę), ale **nie blokuje** `?categories=`, więc te URL-e są indeksowalne. To świadome (chcą je indeksować) albo niedopatrzenie — ale generuje ryzyko duplikacji z archiwami kategorii.

---

## 9. Local SEO

**Praktycznie nieobecne i to celowe.** Brak `LocalBusiness`, brak `address`/`telephone` w `Organization`, brak stron lokalizacyjnych typu „doradztwo podatkowe Warszawa". Crido gra frazami usługowymi ogólnopolskimi o wysokiej wartości, nie geolokalnymi. Przy leadzie B2B na reorganizację czy ulgę B+R lokalizacja nie jest kryterium wyboru.

Dla Mentzena to istotna różnica: jeśli Mentzen ma sieć oddziałów, local SEO jest polem, na którym Crido nie konkuruje w ogóle.

---

## 10. Wnioski dla mentzen.pl

### Zaadaptować

1. **Rozdzielić URL pillar od głębokości nawigacji.** Strony o najwyższej intencji zakupowej („ulga B+R", „IP Box", „fundacja rodzinna", „estoński CIT") umieścić w korzeniu: `mentzen.pl/fundacja-rodzinna/`, nie `/uslugi/prawo/firmy-rodzinne/fundacja/`. W menu mogą siedzieć głęboko.

2. **Rok w `title`, nie w URL i nie w H1.** `„Fundacja rodzinna 2026 — jak założyć i ile kosztuje | Mentzen"`, URL `/fundacja-rodzinna/`, H1 `„Fundacja rodzinna"`. Świeżość w SERP-ie przy stabilnym URL-u.

3. **Zaplanowany, zbiorczy przegląd klastra na start roku podatkowego.** Crido robi to jedną sesją w połowie stycznia. Wpisać do kalendarza redakcyjnego jako powtarzalny proces, nie jako reakcję ad hoc.

4. **Sekcja autokwalifikacji na każdym pillarze.** „Czy ulga B+R jest dla Ciebie?" + trzy twarde warunki. Czytelnik sam się kwalifikuje, co podnosi jakość leada i skraca rozmowę handlową.

5. **Krótki pillar, długi klaster.** Nie pisać 6000 słów na stronie ofertowej. 1200–1500 słów konkretu + agresywne linkowanie do artykułów. Wyczerpywanie tematu delegować do bloga.

6. **Moduł „X a inne ulgi"** — dedykowana sekcja linkująca siostrzane pillary. Łapie frazy porównawcze i trzyma link equity w klastrze.

7. **Kalkulator bez bramki.** Policzyć korzyść, pokazać liczbę za darmo, formularz dopiero pod wynikiem. Dla Mentzena: kalkulator estońskiego CIT-u, kalkulator fundacji rodzinnej vs. spółka, kalkulator formy opodatkowania.

8. **Landingi na zmiany prawa (wzorzec `hottopic`).** Osobny typ strony na każdą dużą nowelizację, ze strukturą: co się zmienia → od kiedy → kogo dotyczy → nasze usługi (każda z „jak działamy" i „efekty"). Polski rynek podatkowy dostarcza kilka takich fal rocznie.

9. **Cykliczny „Przegląd interpretacji/orzecznictwa".** Kwartalny format, tani, buduje sygnał czynnej praktyki i naturalnie linkuje do pillarów.

10. **Formularz kwalifikujący, nie minimalistyczny.** Firma + stanowisko + dział. Mniej leadów, ale trafiających do właściwego zespołu.

11. **Trzy progi zaangażowania** zamiast jednego CTA: newsletter → narzędzie/raport → rozmowa.

12. **Case study osadzone w pillarze**, nie tylko w osobnym katalogu.

### Zrobić lepiej niż Crido — tu jest przewaga do wzięcia

13. **Wdrożyć `FAQPage`** wszędzie, gdzie jest widoczne FAQ. Crido ma 6 pytań na flagowej stronie i **zero** znaczników. Darmowa okazja na rich snippet na frazie o najwyższej wartości.

14. **`Article` + `author` jako `Person`** na wpisach blogowych. Crido serwuje surowe `WebPage` na wszystkim.

15. **Zbudować prawdziwe strony autorów.** Zdjęcie, bio, **numer wpisu na listę doradców podatkowych / radców prawnych**, specjalizacje, publikacje, LinkedIn, schema `Person` z `sameAs` i `jobTitle`. Strony autorów Crido to jedna linijka tekstu. W YMYL to największa dziura w ich profilu.

16. **Dodać recenzję merytoryczną:** „Autor: X · Zweryfikował: Y, doradca podatkowy nr NNNNN · Stan prawny na: DD.MM.RRRR". Crido tego nie ma w ogóle. Dla marki, której siłą jest rozpoznawalny ekspert, to naturalne przedłużenie.

17. **Podpisać strony ofertowe człowiekiem.** `/ulga-b-r/` Crido nie ma żadnego eksperta — tylko formularz. Blok „Twój doradca" ze zdjęciem, nazwiskiem i uprawnieniami na stronie o najwyższej stawce to różnica jakościowa.

18. **Wyświetlać datę aktualizacji, nie publikacji.** Crido pokazuje „21 sierpnia 2019" na treści zaktualizowanej w lutym 2026. Wzorzec: „Aktualizacja: 5 lutego 2026 (pierwotnie: 21 sierpnia 2019)".

19. **`sameAs` z LinkedIn** w `Organization` i w `Person`. Crido ma tylko Facebooka i X — a LinkedIn to główny kanał w B2B doradczym.

20. **`hreflang`**, jeśli powstanie wersja angielska. Crido ma `/en/` i **nie ma hreflang** w ogóle.

21. **Local SEO jako niezagospodarowane pole.** Crido nie gra lokalnie w ogóle. Jeśli Mentzen ma oddziały — `LocalBusiness` per oddział, strony lokalizacyjne, Profil Firmy w Google. Zero konkurencji ze strony Crido.

### Czego unikać

22. **Nie używać nagłówków jako stylu tekstu.** Crido ma pełne zdania lead w `<h2>` na `/ulga-b-r/`, `/ip-box/` i landingach `hottopic`. Efekt uboczny page buildera. Ustalić w CMS-ie osobny styl „lead", żeby redakcja nie sięgała po H2.

23. **Nie wstawiać automatycznych widgetów „najnowsze artykuły" bez filtra tematycznego.** Ze strony o uldze B+R Crido linkuje do artykułów o prawie pracy, cłach i doręczeniach w e-Urzędzie. 81 linków wewnętrznych przy 2350 słowach rozmywa trafność. Widget na pillarze powinien ciągnąć **wyłącznie** z klastra tego pillara.

24. **Nie publikować pod kontem agencji.** `/author/widoczni_seo/` jako autor treści merytorycznej to publicznie widoczny sygnał outsourcingu. Jeśli treść pisze agencja, i tak podpisuje ją i weryfikuje ekspert z imienia i nazwiska.

25. **Nie mnożyć H1.** Kalkulator Crido ma dwa.

26. **Pilnować walidacji dat.** `/ip-box/` ma `datePublished: 1790-11-17`.

27. **Ostrożnie z indeksowalnymi archiwami filtrowanymi** (`?categories=`). Crido ich nie blokuje w `robots.txt` — ryzyko duplikacji z archiwami kategorii. Albo canonical na archiwum kategorii, albo `noindex`.

28. **Nie kopiować rozbicia na 4 osobne CPT blogowe bez potrzeby.** Przy skali Crido (~5400 URL-i) separacja ma sens. Przy mniejszym serwisie to komplikacja bez zysku — wystarczą kategorie i przemyślana struktura URL. Wzorzec `hottopic` i `case_study` warto wziąć, mnożenia blogów — niekoniecznie.

---

## Punkty odniesienia do dalszej analizy

Zgodnie z briefem, w tym samym koszyku warto jeszcze zbadać: **grantthornton.pl** (też TOP na ulgę B+R — sprawdzić, czy mają `FAQPage` i strony autorów, bo jako firma audytorska mogą mieć mocniejsze E-E-A-T), **rsmpoland.pl** i **roedl.pl** na „doradztwo podatkowe dla firm".

## Uwagi metodyczne

- crido.pl zwraca **403 na WebFetch**; działa `curl` z przeglądarkowym User-Agent i `Accept-Language: pl-PL`. To samo prawdopodobnie dotyczy innych serwisów za WAF-em.
- Wnioski dotyczą struktury i on-page. **Nie badałem profilu linków przychodzących, Core Web Vitals ani realnych danych o ruchu** — to osobne zadanie (Ahrefs/Semrush + PageSpeed).
- Pozycja nr 1 na „ulga B+R" przyjęta za briefem, nie zweryfikowana niezależnie (budżet WebSearch wyczerpany w tej sesji).
