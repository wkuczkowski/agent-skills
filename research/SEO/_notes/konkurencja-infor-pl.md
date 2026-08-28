# infor.pl — analiza konkurencji SEO (benchmark dla mentzen.pl)

Data badania: 2026-08-28. Zakres: infor.pl + ksiegowosc.infor.pl, z odniesieniem do gofin.pl i pit.pl.
Metoda: WebSearch, WebFetch, analiza surowego HTML (curl + parsowanie JSON-LD, nagłówków, linków).

Zbadane URL-e:
- `https://ksiegowosc.infor.pl/wiadomosci/7524215,zmiana-formy-opodatkowania-2026-ryczalt-liniowy-czy-skala.html`
- `https://ksiegowosc.infor.pl/podatki/7496316,zmiana-formy-opodatkowania-2026-termin-ceidg-jak-zmienic-liniowy-skala-ryczalt-20-styczen-czy-20-luty-biznes-gov-pl.html`
- `https://ksiegowosc.infor.pl/podatki/kpir/abc-kpir/7514477,...kompletne-kompendium-zmian-w-kpir-2026.html`
- `https://ksiegowosc.infor.pl/tematy/podatki-2026/`, `/tematy/fundacja-rodzinna/`, `/podatki/`, `/ksef/`, `/vademecum/`
- `https://www.infor.pl/eksperci/38,Slawomir-Bilinski.html`
- `https://www.gofin.pl/firma/17,2,305,210541,jak-zalozyc-spolke-z-oo.html`
- `https://www.pit.pl/aktualnosci/jak-krok-po-kroku-zalozyc-spolke-z-o-o-przez-internet-1008854`

---

## 1. Architektura treści

### Sieć subdomen jako jedna encja
Infor prowadzi ruch przez ~7 hostów: `www.infor.pl` (parasol), `ksiegowosc.infor.pl`, `kadry.infor.pl`, `mojafirma.infor.pl`, `samorzad.infor.pl`, `podatki.infor.pl`, `druki.infor.pl`, plus `sklep.infor.pl` (monetyzacja) i `g.infor.pl` / `incdn.pl` (assety).

Spina je JSON-LD: każda podstrona każdej subdomeny deklaruje `WebSite @id: https://www.infor.pl/#website` i `NewsMediaOrganization @id: https://www.infor.pl/#organization`. Dla Google to jedna marka rozłożona na wiele hostów, nie siedem serwisów.

Artykuły linkują poprzecznie między subdomenami (z tekstu o podatkach do `kadry.infor.pl/tematy/pit/` i `mojafirma.infor.pl/tematy/firma/`).

### Dwie niezależne osie taksonomii

**Oś pionowa — kategorie w URL-u, 3–4 poziomy:**
```
/podatki/                                   751 artykułów
/podatki/pit/
/podatki/pit/pit/koszty/
/podatki/podatki-osobiste/pit/
/podatki/podatki-osobiste/spadki-darowizny/
/podatki/ryczalt/stawki-i-rozliczenia/
/podatki/kpir/abc-kpir/
/podatki/ordynacja-podatkowa/
/rachunkowosc/{amortyzacja,inwentaryzacja,sprawozdawczosc,...}
/obrot-gospodarczy/{spolki,windykacja,zamowienia-publiczne,...}
/zus-kadry/{skladki,urlopy,wynagrodzenia,...}
```

**Oś pozioma — tagi `/tematy/<slug>/`:**
```
/tematy/podatki-2026/            136 artykułów
/tematy/fundacja-rodzinna/        70 artykułów
/tematy/zmiana-formy-opodatkowania/
/tematy/wniosek-ceidg-zmiana/
/tematy/skladka-zdrowotna-2026/
/tematy/ryczalt/ /tematy/skala-podatkowa/ /tematy/pit/ /tematy/faktura/
```
Tag jest tu realnym klastrem: strona `/tematy/podatki-2026/` ma `<h1>Podatki 2026</h1>`, listę 136 tekstów z paginacją (50/stronę), `robots: index, follow`, canonical na siebie. Ten sam slug tagu istnieje na kilku subdomenach (`mojafirma.infor.pl/tematy/pit/`, `kadry.infor.pl/tematy/pit/`).

Uwaga: strony tagów mają **generyczny meta description** (ten sam boilerplate „Księgowość w Infor.pl to profesjonalny serwis dla księgowych…" dla każdego tagu) i **zero tekstu wstępnego**. Czysta lista. Rankują wyłącznie siłą domeny i świeżością listingu.

### Huby legislacyjne
`/ksef/` — 330 artykułów, własna nawigacja: Aktualności / Oprogramowanie / INFORLEX / Porady i artykuły / Akty prawne / Wideoporady. Ma akapit definicyjny na górze („Krajowy System e-Faktur (KSeF) to centralny…") plus harmonogram wdrożenia. To najbliższe temu, co w metodyce nazywamy pillar page — ale pillar jest tu cienki, robotę robi listing.

`/vademecum/` — hub poradnikowy dla zawodu (standardy rachunkowości, doskonalenie zawodu), teksty 2000–3000 słów.

### Warstwa narzędziowa (najmocniejszy element architektury)
```
/kalkulatory/          12+ kalkulatorów (składka zdrowotna przedsiębiorcy, wynagrodzeń,
                       VAT, umowy zlecenia, umowy o dzieło, odsetki od zaległości, daty)
/wskazniki/            odsetki ustawowe, skala PIT, KUP, ryczałt, stawki amortyzacyjne,
                       podatek rolny, akcyza
druki.infor.pl         aktywne formularze PIT/VAT/rachunkowość
/terminarz/            osobny URL na każdy dzień: /terminarz/28,08,2026.html
/testy/                quizy
podatki.infor.pl/interpretacje-podatkowe/
```
Cały ten blok jest wklejony w **stopkę każdego artykułu**. Efekt: strony narzędziowe dostają link ze wszystkich ~46 tys. publikacji portalu i rankują na frazy transakcyjne („kalkulator składki zdrowotnej"), których artykuł nigdy nie zdobędzie.

---

## 2. Tempo publikacji i klastry newsów wokół evergreenów

### Kluczowa obserwacja: nie aktualizują, tylko publikują od nowa

We wszystkich trzech zbadanych artykułach `dateModified` **jest identyczny** z `datePublished`:

| URL | Sekcja | datePublished | dateModified |
|---|---|---|---|
| 7496316 | Podatki | 2026-01-13T11:57:54 | 2026-01-13T11:57:54 |
| 7524215 | Wiadomości | 2026-02-19T07:25:42 | 2026-02-19T07:25:42 |
| 7514477 (kompendium KPiR) | ABC KPiR | 2026-03-18T10:48:08 | 2026-03-18T10:48:08 |

Zamiast odświeżać jeden URL, Infor **obsługuje tę samą intencję kolejnymi artykułami w różnych sekcjach**:

- **13 stycznia** (5 tygodni przed terminem), sekcja `/podatki/`, autor Tomasz Piwowarski — tekst edukacyjny „prostujący mit": *„termin mija 20 stycznia czy 20 lutego? Wielu przedsiębiorców żyje w błędzie"*.
- **19 lutego** (dzień przed terminem), sekcja `/wiadomosci/`, autor Sławomir Biliński — tekst domykający: *„ostatni dzień na decyzję"*.

Oba celują w to samo zapytanie. Oba są w TOP. Infor stać na kanibalizację, bo autorytet domeny sam rozstrzyga, który URL wyświetlić.

### Publikują na etapie projektu ustawy, nie po wejściu w życie
Z feedu `/wiadomosci.feed` (sierpień 2026), gdy 2026 dopiero trwa, klaster „podatki 2027" jest już budowany:
- „Nowy CIT dla największych firm. **Projekt zakłada** podwyżkę stawki do 22 proc."
- „Ryczałt po nowemu. **Projekt zakłada** niższy limit i wyższą stawkę"
- „Zmiany w podatkach od 2027 r. **(projekt)** 130 tys. zł II próg podatkowy w PIT"
- „Co nas czeka w podatkach w latach 2027–2029?"
- „Zmiany w VAT 2027 – cudowne rozmnożenie podatników. Prof. Modzelewski…"

To jest mechanizm ich przewagi czasowej: URL na frazę „zmiany w podatkach 2027" istnieje, zanim ktokolwiek jej szuka. Gdy ruch przychodzi, strona ma już kilkanaście miesięcy wieku i linków wewnętrznych.

### Wolumen
Feed Atom `ksiegowosc.infor.pl/wiadomosci.feed` (50 najnowszych wpisów) daje **3–5 tekstów dziennie** — a to jedna sekcja jednej subdomeny. Portal deklaruje 46 tys.+ publikacji. Ścigać się wolumenem nie ma sensu.

### Kalendarz sezonowy, który obsługują
20 stycznia / 20 lutego (forma opodatkowania), 1 lutego i 1 kwietnia 2026 (etapy KSeF), 31 marca (sprawozdania), sezon PIT, plus `/terminarz/` z osobnym URL-em na każdy dzień roku.

---

## 3. Format i struktura najlepszych artykułów

### Metryki (z surowego HTML, treść wewnątrz `<article>`)

| | 7496316 (poradnik) | 7524215 (news) | 7514477 (kompendium) |
|---|---|---|---|
| Słowa | 2 201 | 2 011 | 3 610 |
| H2 | 5 | 6 | ~8 |
| H3 | 3 | 0 | ~14 |
| `<table>` | **0** | **0** | **0** |
| `<ul>` / `<ol>` | 7 / 1 | 6 / 0 | — |
| `<strong>` | 50 | 10 | — |
| Linki w `<article>` | 115 | 111 | — |

**Zero tabel we wszystkich trzech tekstach** — również w artykule, którego cała teza to porównanie trzech form opodatkowania. To najbardziej rzucająca się w oczy luka.

### Nagłówki pisane językiem problemu, nie nazwą przepisu

Artykuł 7496316:
```
H1  Zmiana formy opodatkowania na 2026 rok - termin mija 20 stycznia czy 20 lutego?
    Wielu przedsiębiorców żyje w błędzie
H2  Wielkie zamieszanie z datami. 20 stycznia czy 20 lutego?
H2  Pułapka pierwszej faktury w 2026 i składki zdrowotnej
H2  Co się teraz opłaca? Krótki przegląd na 2026 rok
    H3  Kiedy zostać na skali podatkowej (czyli zasady ogólne)?
    H3  Dla kogo podatek liniowy (sztywna stawka 19%)?
    H3  Ryczałt – król opłacalności, ale nie dla każdego
H2  Instrukcja obsługi: Jak zmienić formę opodatkowania bez wychodzenia z domu?
H2  Najem prywatny – tu nie masz wyboru co do formy
```

Artykuł 7524215:
```
H1  Zmiana formy opodatkowania 2026: ryczałt, liniowy czy skala - ostatni dzień na decyzję
H2  Termin, którego nie można przegapić
H2  Trzy ścieżki podatkowe — krótki przegląd
H2  Kiedy zmiana się opłaca?
H2  Składka zdrowotna jako zmienna decyzyjna
H2  Jak przeprowadzić zmianę: krok po kroku
H2  Liczy się całościowy rachunek
```

Żaden nagłówek nie brzmi „Art. 9a ust. 2 ustawy o PIT". H3 pojawiają się wyłącznie tam, gdzie użytkownik podejmuje decyzję (jedna forma opodatkowania = jeden H3) — to dobrze zmapowane pod fragmenty i pod AI Overviews.

### Rozjazd `<title>` i `<h1>` (świadomy)

Artykuł 7496316:
- `<title>`: **„Zmiana formy opodatkowania 2026. Do kiedy termin? Ryczałt, liniowy czy skala – jak zmienić w CEIDG? - Infor.pl"** — upchane frazy: rok, „do kiedy termin", trzy formy, CEIDG.
- `<h1>` i `schema:headline`: **„…termin mija 20 stycznia czy 20 lutego? Wielu przedsiębiorców żyje w błędzie"** — wersja emocjonalna, pod CTR i Discover.

Title tag został ewidentnie przepisany po publikacji pod frazy (H1 i JSON-LD zostały ze starą wersją). Rozdzielenie SERP-owego title od on-page H1 jest u nich regułą, nie wypadkiem.

### Stałe elementy szablonu
- **Spis treści z kotwicami** na górze, zwijalny („rozwiń >").
- Ramka **„Ważne"** — podsumowanie warunku/wyjątku.
- Ramka **„Podstawa prawna"** na końcu — konkretna ustawa (ustawa z 26 lipca 1991 r. o PIT). Bez numerów artykułów.
- Listy numerowane na procedurę („5 kroków w CEIDG"), punktowane na warunki/plusy/minusy.
- Blok „Tematy" (H3) z linkami do `/tematy/*` — tożsamy z `meta keywords`.
- Bloki „Powiązane", „Polecamy", „Najnowsze" — generowane automatycznie.

### `meta keywords` wciąż uzupełniane — i to nie przypadek
```html
<meta name="keywords" content="zmiana formy opodatkowania, podatki 2026,
      wniosek CEIDG zmiana, ryczałt, składka zdrowotna 2026" />
```
To nie relikt: ta lista jest **jeden do jednego** listą tagów `/tematy/*` podlinkowanych pod artykułem. Pole `keywords` pełni u nich funkcję konfiguracji klastrowania, nie sygnału dla Google.

---

## 4. Sygnały E-E-A-T

### Co mają
- **Imienny autor z linkiem do profilu** w każdym artykule: `https://www.infor.pl/eksperci/38,Slawomir-Bilinski.html`. Profil zawiera zdjęcie, bio, i **pełną listę publikacji z paginacją** — Biliński ma 340 artykułów (17 stron). To buduje topical authority per osoba, nie tylko per domena.
- **Bio krótkie i konkretne**: „prawnik, dziennikarz, prowadzący szkolenia, autor licznych publikacji z prawa podatkowego"; „radca prawny od 2017 roku, od 2025 w redakcji Infor.pl; pisze o prawie cywilnym, gospodarczym, ubezpieczeniach społecznych i nieruchomościach".
- **`NewsMediaOrganization`** (nie zwykła `Organization`) z `sameAs` do Facebooka, X, LinkedIna i YouTube'a — kwalifikacja jako wydawca prasowy pod Google News i Discover.
- **Model współautorstwa z ekspertami zewnętrznymi.** Podpisy z feedu:
  - `Witold Modzelewski, Instytut Studiów Podatkowych Modzelewski i Wspólnicy, oprac. Paweł Huczko`
  - `Krajowa Izba Gospodarcza, Piotr Soroczyński, oprac. Adam Kuchta`
  - `IFIRMA, oprac. Paweł Huczko`, `SaldeoSMART, oprac. Adam Kuchta`
  - `Ogólnopolska Sieć Certyfikowanych Biur Rachunkowych (OSCBR), oprac. Paweł Huczko`

  Redakcja przetwarza materiały instytucji i firm, podpisuje podwójnie. Tanio pozyskują nazwiska z afiliacją, zachowując imienny byline.
- **Otwarty nabór autorów**: link „Dołącz do grona ekspertów" (`/eksperci/kontakt/`) w każdym artykule.

### Czego nie mają (to są luki do wykorzystania)
- **Brak `Person` schema.** Autor w JSON-LD to gołe `{@type: Person, @id, name, url}` — bez `jobTitle`, `hasCredential`, `knowsAbout`, `sameAs`, `worksFor`. Profil eksperta też nie ma własnego `Person` markupu ani linków do LinkedIna.
- **Brak jakiejkolwiek recenzji merytorycznej.** Żadnego „zweryfikowane przez", `reviewedBy`, nazwiska doradcy podatkowego z numerem uprawnień.
- **Brak dat aktualizacji.** `dateModified == datePublished` w 3/3 przypadków. Nie ma widocznej informacji „zaktualizowano", nie ma changelogu.
- **Brak cytowanych źródeł pierwotnych.** „Podstawa prawna" wskazuje ustawę, ale bez artykułów, bez linku do ISAP, bez sygnatur interpretacji w tekście.
- `oprac.` sygnalizuje, że tekst przetworzył redaktor, a nie napisał praktyk.

---

## 5. Dane strukturalne (analiza surowego HTML)

Dokładnie **jeden** blok `<script type="application/ld+json">` na stronę, zbudowany jako `@graph` z referencjami `@id` (wzorzec znany z Yoasta, ale własna implementacja). **Zero mikrodanych** (`itemtype` = 0 wystąpień).

```
@graph:
├── WebSite            @id: https://www.infor.pl/#website
│                      + potentialAction: SearchAction
│                        target: https://www.infor.pl/wyniki/?fraza={search_term_string}
├── NewsMediaOrganization  @id: https://www.infor.pl/#organization
│                      logo: SVG 300×112 + caption
│                      sameAs: [facebook, x, linkedin, youtube]
├── WebPage            @id: <url>#webpage
│                      name, description (= meta description),
│                      isPartOf → #website, mainEntity → #article, breadcrumb → #breadcrumb
├── BreadcrumbList     @id: <url>#breadcrumb   — 4 poziomy
│                      Infor.pl > Księgowość > Wiadomości > <tytuł artykułu>
│                      (dla /podatki/: Infor.pl > Księgowość > Podatki > <tytuł>)
└── NewsArticle        @id: <url>#article
                       headline, articleSection ("Wiadomości" / "Podatki" / "ABC KPiR"),
                       datePublished, dateModified,
                       mainEntityOfPage → #webpage,
                       author: {Person, @id: <profil eksperta>#person, name, url},
                       publisher → #organization,
                       isAccessibleForFree: true,
                       image: {ImageObject, webp 1200×800, caption}
```

**Czego brakuje w schemacie:**
- `FAQPage` — nie ma nigdzie, mimo że artykuły są zbudowane z pytań („Do kiedy termin?", „Dla kogo podatek liniowy?").
- `HowTo` — nie ma, mimo sekcji „Instrukcja obsługi: jak zmienić formę opodatkowania" z listą numerowaną.
- `about` / `mentions` — brak powiązań encji.
- `wordCount`, `speakable`, `articleBody`.
- `Person` z kwalifikacjami.
- `NewsArticle` używany również dla evergreenowego kompendium KPiR — świadome nadużycie pod Top Stories/Discover.

**Pozostała warstwa techniczna:**
```html
<meta name="robots" content="index, follow, max-image-preview:large" />
<link rel="canonical" href="..." />
<link rel="amphtml" href="....html.amp" />        <!-- AMP wciąż utrzymywane -->
<link rel="alternate" type="application/rss+xml"
      title="Infor.pl - Wiadomości" href="https://ksiegowosc.infor.pl/wiadomosci.feed" />
<meta property="og:type" content="article" />
<meta name="twitter:card" content="summary_large_image" />
og:image 1200×800 webp + og:image:alt
```
Feed Atom per sekcja. `max-image-preview:large` — nastawienie na Discover. Obrazy serwowane przez własny konwerter webp (`webp-konwerter.incdn.pl`) z base64 w URL-u.

---

## 6. Linkowanie wewnętrzne

- **428 unikalnych** linków wewnętrznych na całej stronie artykułu; **~111–115 wewnątrz `<article>`**.
- W samym **ciele tekstu** linków jest mało i prowadzą głównie do **tagów** (`/tematy/faktura/`, `/tematy/skladki/`, `/tematy/skala-podatkowa/`), nie do innych artykułów. Kilka z nich wskazuje na tagi na innych subdomenach.
- Linki do artykułów siedzą w automatycznych blokach: „Powiązane", „Polecamy", „Najnowsze" (~40 pozycji), rozsianych po całej stronie.
- **Stopka narzędziowa powtórzona pod każdym artykułem** (kalkulatory, wskaźniki, druki, terminarz, interpretacje, quizy) — kilkadziesiąt linków z każdej z 46 tys. stron do stron narzędziowych.
- Efekt netto: przepływ mocy idzie z artykułów do **tagów i narzędzi**, nie między artykułami. Klastry są spinane przez strony zbiorcze, nie przez linki poziome.

---

## 7. Połączenie treści z ofertą i konwersją

Infor nie sprzedaje doradztwa. Sprzedaje produkt wydawniczy i szkoleniowy. Ścieżka:

**Wewnątrz artykułu** (bloki oznaczone „Autopromocja", wplecione w środek tekstu, nie tylko na końcu):
- „Cykl szkoleń online: Akademia podatkowa 2026/2027" + nazwisko prowadzącego (Radosław Kowalski) + przycisk „Zapisz się"
- „V Ogólnopolskie Forum Biur Rachunkowych" (Warszawa) + „Zapisz się"
- „Komplet KSeF: wdrożenie w firmie, zasady wystawiania faktur…"
- „KOMPLET PODATKI 2026"

**Wszystkie z pełnym UTM:**
```
?utm_source=infor.pl&utm_medium=link-w-artykule&...
?utm_source=ksiegowosc.infor.pl&utm_medium=...
?utm_source=serwisy-internetowe&utm_medium=link-w-artykule
```
Mierzą, która subdomena i które miejsce w artykule sprzedaje.

**Poza artykułem:** newsletter (formularz z double opt-in), INFORLEX (subskrypcja bazy wiedzy dla biur rachunkowych), sklep.infor.pl, InforAkademia, „Kawa z INFORLEX" (cykl webinarów, publikowany jako artykuły — treść, która jest reklamą produktu i jednocześnie rankuje).

Treści sponsorowane oznaczone „(artykuł sponsorowany)".

Model konwersji jest miękki i wieloetapowy: ruch SEO → newsletter → szkolenie / subskrypcja. Sprzedaż produktu o niskiej-średniej wartości przy dużym wolumenie.

---

## 8. Local SEO

Praktycznie nie istnieje. Brak `LocalBusiness` / `LegalService` schema. Tagi geograficzne (`mojafirma.infor.pl/tematy/warszawa/`) używane są do szkoleń stacjonarnych, nie do pozycjonowania lokalnego. Portal jest ogólnopolski i to nie jest jego oś konkurencji — dla Mentzena (kancelaria z oddziałami) to obszar, gdzie Infor w ogóle nie gra.

---

## 9. Koszyk portalowy: gofin.pl i pit.pl

### gofin.pl
- **`<h1>` to „GOFIN.PL"** — nazwa marki, na każdej stronie. Tytuł artykułu jest dopiero w H2/H3 („Firma" → „Zakładanie firmy" → „Wymogi z K.s.h."). Poważny błąd on-page, który nie przeszkadza im rankować.
- `NewsArticle` z **`author: Organization`** (Wydawnictwo Podatkowe GOFIN sp. z o.o.), nie osoba. Zero E-E-A-T na poziomie autora.
- `datePublished` = `dateModified` = **2021-05-27**. Treść sprzed pięciu lat wciąż w TOP.
- Źródło pod artykułem: „Gazeta Podatkowa nr 42 (1813) z dnia 27.05.2021, strona 21" — recykling papieru do sieci.
- **Dwa systemy URL:** widoczny `/firma/17,2,305,210541,jak-zalozyc-spolke-z-oo.html`, canonical na czystszy `/firma/zakladanie-firmy/43120/jak-zalozyc-spolke-z-o-o`.
- `Organization` schema z pełnym adresem, telefonem, faksem, e-mailem i `ContactPoint`.
- **Zwraca 403 dla nietypowego User-Agenta.** Agresywna ochrona przed botami wycina też crawlery AI — to kosztuje ich widoczność w AI Overviews.
- Monetyzacja: prenumerata czasopism i serwisów (Gazeta Podatkowa, Poradnik VAT), sklep.

### pit.pl
- Autor: „Redakcja PIT.pl", ale w tekście wyróżniony **ekspert zewnętrzny z kancelarii** (Rafał Knap, radca prawny, kancelaria Graś i Wspólnicy). Model „kancelaria jako źródło merytoryczne portalu" — dokładnie ta rola, którą Mentzen mógłby przejąć u innych wydawców.
- Artykuł z **10 października 2023** wciąż w TOP na „jak założyć spółkę z o.o. przez internet".
- ~1500 słów, 5 H2, brak tabel, brak FAQ.
- **Brak JSON-LD.** Zero danych strukturalnych na artykule.
- Monetyzacja: **program e-pity** (produkt SaaS do rozliczeń) + „Twój e-PIT" + kalkulatory i druki. Najostrzej domknięta ścieżka treść → produkt w całym koszyku: artykuł o podatkach prowadzi do narzędzia, które te podatki rozlicza.

### Wniosek z koszyka
Cała trójka wygrywa **wiekiem URL-a i autorytetem domeny**, a nie jakością on-page. Gofin ma złamany H1, pit.pl nie ma schema, infor nie ma tabel, FAQ ani dat aktualizacji. Techniczna i merytoryczna przewaga jest do zdobycia — brakuje tylko czasu i linków.

---

## Wnioski dla mentzen.pl

### Do zaadaptowania

1. **Podwójny cykl publikacji na frazy sezonowe.** Evergreen wypuszczany wcześnie (listopad–grudzień na „forma opodatkowania 2027"), plus krótki news domykający na 1–3 dni przed terminem. Kancelaria zna kalendarz terminów lepiej niż redakcja portalu — to przewaga informacyjna, nie tylko merytoryczna.

2. **Publikować na etapie projektu ustawy, nie po wejściu w życie.** To jedyny mechanizm, który pozwala wyprzedzić autorytet domeny: być pierwszym URL-em na frazę, zanim fraza zacznie generować ruch. Infor w sierpniu 2026 ma już zbudowany klaster „podatki 2027". Doradcy Mentzena widzą projekty i konsultacje wcześniej niż dziennikarze — trzeba to przekuć w URL-e z datą.

3. **Warstwa narzędziowa jako trwały aktyw.** Kalkulator wyboru formy opodatkowania, kalkulator składki zdrowotnej, kalkulator opodatkowania fundacji rodzinnej, terminarz podatkowy. Infor linkuje do swoich narzędzi z każdego artykułu i dzięki temu rankuje na frazy transakcyjne. Mentzen może zrobić to lepiej: kalkulator, którego wynik kończy się realną rekomendacją doradcy, a nie tylko liczbą.

4. **Druga oś taksonomii: `/tematy/<tag>/` niezależna od kategorii.** Tania i skuteczna. Jedna strona zbiorcza per klaster, linkowana z każdego artykułu w klastrze. Ale w odróżnieniu od Infora — z **własnym tekstem wstępnym i unikalnym meta description**, nie boilerplate'em.

5. **Strony autorów z pełną listą publikacji.** U Infora to działa mimo ubogiego bio. U Mentzena to naturalnie mocniejsze: doradca podatkowy z numerem uprawnień, radca prawny z numerem wpisu. Zrobić z pełnym `Person` schema: `jobTitle`, `hasCredential`, `knowsAbout`, `worksFor`, `sameAs` (LinkedIn), `alumniOf`.

6. **JSON-LD jako `@graph` z referencjami `@id`.** Jedna encja marki, `BreadcrumbList` czteropoziomowy, `publisher` z `sameAs`. Higiena techniczna, jednorazowy koszt wdrożenia w szablonie.

7. **Nagłówki językiem problemu użytkownika.** „Pułapka pierwszej faktury", „Ryczałt – król opłacalności, ale nie dla każdego". Jeden H3 na jedną decyzję, którą czytelnik ma podjąć. To dobrze mapuje się na fragmenty w SERP i na cytowania w AI Overviews.

8. **Ramka „Podstawa prawna".** Infor podaje samą ustawę. Mentzen może podać ustawę, artykuł, ustęp, link do ISAP i sygnaturę interpretacji — i to jest dokładnie ta różnica, która buduje E-E-A-T kancelarii wobec portalu.

9. **Świadome rozdzielenie `<title>` i `<h1>`.** Title frazowy pod SERP, H1 angażujący pod czytelnika. U Infora wyszło to z zaniedbania (title przepisany, H1 i schema zostały stare) — u nas ma być decyzją, z `headline` w JSON-LD zgodnym z H1.

10. **Odwrócić model ekspercki Infora.** Oni pozyskują komentarze od Modzelewskiego, KIG, OSCBR. Mentzen jest po drugiej stronie tego równania — dostarczanie komentarzy eksperckich portalom (infor, pit.pl, gofin) daje linki, cytowania nazwiska i obecność w klastrach konkurenta.

### Czego unikać

1. **Nie kopiować modelu „nowy artykuł zamiast aktualizacji".** Infor może sobie na to pozwolić dzięki autorytetowi domeny. Mentzen bez tego wygeneruje kanibalizację. Strategia odwrotna: **jeden kanoniczny URL na frazę**, faktycznie aktualizowany, z realnie zmienianym `dateModified`, widoczną datą aktualizacji i krótkim changelogiem („Aktualizacja 20.02.2026: dodano stawki składki zdrowotnej"). To jest dokładnie luka Infora — 0/3 artykułów miało `dateModified` różny od `datePublished`.

2. **Nie kopiować braku tabel.** Zero tabel w tekście porównującym trzy formy opodatkowania to prezent. Tabela porównawcza + kalkulator + `FAQPage` schema daje przewagę w feature'ach SERP, gdzie Infor w ogóle nie startuje.

3. **Nie kopiować braku `FAQPage` i `HowTo`.** Artykuły Infora są zbudowane z pytań i procedur krok po kroku, ale nigdy tego nie oznaczają. Mentzen może.

4. **Nie zaśmiecać artykułu setką linków i pięcioma boksami autopromocji.** Infor sprzedaje szkolenie za kilkaset złotych przy dużym wolumenie — stać go na rozpraszanie uwagi. Mentzen sprzedaje usługę doradczą o wysokiej wartości; jedno mocne, kontekstowe CTA („sprawdź, czy zmiana formy opodatkowania Ci się opłaca — konsultacja") konwertuje lepiej niż pięć banerów.

5. **Nie używać `NewsArticle` dla evergreenów.** Infor oznacza tak nawet kompendium KPiR — to gra pod Top Stories dostępna wydawcy prasowemu. Bez statusu wydawcy właściwe jest `Article` / `BlogPosting` z uczciwym `dateModified`.

6. **Nie wprowadzać modelu „oprac.".** Przewaga merytoryczna Mentzena polega dokładnie na tym, że pisze doradca, a nie redaktor przetwarzający komunikat prasowy. Podpisywać w pełni, z uprawnieniami i afiliacją.

7. **Nie ścigać się wolumenem.** 3–5 tekstów dziennie w jednej sekcji jednej subdomeny jest nie do dogonienia. Wygrywać głębią (kompendia z wyliczeniami i przypadkami z praktyki kancelarii) i szybkością na wąskich frazach eksperckich.

8. **Nie blokować crawlerów tak jak gofin** (403 na nietypowy User-Agent). To wycina AI-crawlery i widoczność w AI Overviews — koszt rosnący z każdym kwartałem.

### Punkt strategiczny: fundacja rodzinna

Tag `/tematy/fundacja-rodzinna/` u Infora ma **70 artykułów**, w większości newsów o projektach zmian („Idą zmiany w fundacjach rodzinnych", „Zasadzka legislacyjna na fundacje rodzinne", „Ekspert: zmiany nie mają silnych podstaw merytorycznych") i komentarzy zewnętrznych ekspertów. Kompendiów jest niewiele — jedyne widoczne to „ABC Fundacji rodzinnej. Jak założyć, jakie podatki trzeba płacić?".

Dla porównania `/tematy/podatki-2026/` ma 136 artykułów, a kategoria `/podatki/` — 751. Fundacja rodzinna jest u nich tematem cienkim, obsługiwanym głównie cudzymi komentarzami. Jednocześnie to temat, w którym Mentzen ma najgłębszą praktykę i najwyższą wartość leada.

To najlepszy punkt startowy: głęboki klaster (pillar + 15–25 tekstów satelitarnych + kalkulator + tabela porównawcza + FAQ schema), gdzie przewaga merytoryczna realnie przebije autorytet domeny, bo konkurent nie ma czego bronić.
