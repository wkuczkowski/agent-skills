# grantthornton.pl — analiza SEO jako benchmark dla mentzen.pl

Data analizy: 2026-08-28
Metoda: pobranie HTML i sitemap przez `curl` (User-Agent przeglądarkowy, `--compressed`), parsowanie JSON-LD / mikrodanych / nagłówków / linków w Pythonie, plus WebFetch do odczytu warstwy widocznej. Bez dostępu do danych o ruchu i pozycjach — wnioski dotyczą **struktury i sygnałów**, nie zmierzonej widoczności.

**Ostrzeżenie metodyczne:** serwis stoi za W3 Total Cache i przy pierwszym pobraniu zwrócił **skróconą wersję strony** (121 KB zamiast 665 KB), pozbawioną boksu autora, CTA i modułów powiązanych. Wnioski z pierwszego pobrania („brak autora w HTML") okazały się fałszywe po ponownym pobraniu. Przy audytach konkurencji tego typu warto pobierać stronę **dwukrotnie** i porównywać rozmiar. Wszystkie liczby poniżej pochodzą z pełnej wersji (`art2.html`, 662 812 znaków).

[do weryfikacji] Przy powtórnej kontroli 2026-08-28 skrócona odpowiedź **nie wystąpiła** — trzy kolejne pobrania zwróciły stabilnie 665 779–665 780 znaków (pełna wersja z boksem autora). Samo zjawisko obcięcia jest więc niepotwierdzalne po fakcie i mogło być jednorazowym stanem cache; podana wyżej wartość 662 812 znaków dziś nie odtwarza się (rozjazd ~3 tys. znaków to prawdopodobnie dynamiczne fragmenty HTML, m.in. znacznik czasu w komentarzu W3TC). Zalecenie o dwukrotnym pobieraniu pozostaje słuszne niezależnie od tego.

---

## Podsumowanie w jednym akapicie

Grant Thornton wygrywa nie objętością bloga (3,3 tys. publikacji to mniej niż Crido), tylko **spięciem trzech osi w jedną strukturę URL**: taksonomia kategorii artykułów jest lustrzanym odbiciem taksonomii usług (`/articles_categories/uslugi/doradztwo-podatkowe/podatek-vat/` ↔ `/usluga/doradztwo-podatkowe/`), a każdy artykuł i każda usługa kończy się **imiennym boksem eksperta z tytułem zawodowym, mailem i telefonem komórkowym**. Do tego dochodzi warstwa, której polska konkurencja doradcza w większości nie ma: **własne dane cykliczne** (comiesięczny raport o ofertach pracy z autorskiego systemu, marki „Purpurowy Informator", „Barometr prawa") — treść, którą media cytują, więc linki przychodzą same. Warstwa techniczna jest natomiast **zaskakująco zaniedbana**: puste meta description na stronach pieniężnych, zdublowany sufiks marki w `<title>`, hreflang wskazujący na stronę główną zamiast na tłumaczenie, autor w JSON-LD bez `@id`/`url` łączącego go ze stroną eksperta. To jest dokładnie ta luka, w którą Mentzen może wejść niskim kosztem.

---

## 1. Skala i typy treści

Sitemapy (`https://grantthornton.pl/sitemap_index.xml`, pobrane 2026-08-28) — Yoast SEO, WordPress, W3 Total Cache (Redis page cache + Memcached object cache; ujawnione w komentarzu HTML na końcu sitemapy).

| Sitemapa | Liczba URL |
|---|---|
| `article` (4 pliki) | **3 595** |
| — w tym `/publikacja/` (PL) | 3 330 |
| — w tym `/en/article/` | 263 |
| — w tym `/de/` | 2 |
| `post` | 356 |
| `service` (`/usluga/`) | **246** |
| `worker` (`/pracownik/`) | **314** |
| `event` | 190 |
| `page` | 117 |
| `articles_categories` | 88 |
| `tag` | 81 |
| `custom_author` (`/autorzy/`) | 68 |
| `career` | 22 |
| `market` | 9 |
| `news` (Google News) | 5 |
| `category` | 3 |
| **Razem** | **~5 100** (dokładnie 5 094) |

Źródło: `https://grantthornton.pl/sitemap_index.xml` oraz poszczególne sitemapy, pobrane 2026-08-28.

### Co z tego wynika

**246 stron usługowych na 3 330 artykułów** to stosunek ok. 1:13. Dla porównania Crido ma 182 strony ofertowe na ~5 400 URL-i (zob. `konkurencja-crido-pl.md`). Grant Thornton stawia więc **dużo grubszą warstwę komercyjną** — usługi są rozdrobnione do poziomu pojedynczego zagadnienia (`/usluga/ceny-transferowe/uprzednie-porozumienia-cenowe-apa/`, `/usluga/audyt/badanie-deklaracji-kompletnosci-lucid-verpackg/`), a nie tylko do poziomu działu praktyki.

**314 stron pracowników** to osobny typ treści (CPT `worker`), nie profile WordPressa. Każdy ekspert jest samodzielnym URL-em z własnym miejscem w sitemapie.

---

## 2. Architektura: taksonomia treści jest kopią taksonomii oferty

To jest najmocniejszy element tej domeny i główny wniosek benchmarku.

Ścieżki kategorii artykułów **odwzorowują hierarchię usług**:

```
/usluga/doradztwo-podatkowe/                    →  /articles_categories/uslugi/doradztwo-podatkowe/
/usluga/doradztwo-podatkowe/audyt-podatkowy/    →  /articles_categories/uslugi/doradztwo-podatkowe/audyt-podatkowy/
/usluga/audyt/audyt-wewnetrzny/                 →  /articles_categories/uslugi/audyt/audyt-wewnetrzny/
/usluga/kancelaria-prawna/fundacja-rodzinna/    →  /articles_categories/uslugi/kancelaria-prawna/fundacja-rodzinna/
```

Źródło: `https://grantthornton.pl/articles_categories-sitemap.xml` i `https://grantthornton.pl/service-sitemap.xml`, pobrane 2026-08-28.

Z 88 kategorii artykułów **61 leży pod prefiksem `/articles_categories/uslugi/...`**, a kolejne **9 pod `/rynki/`** (branże: `nieruchomosci-i-budownictwo`, `gazownictwo-i-energetyka`, `firmy-rodzinne`, `sport`, `przemysl-drzewny-i-meblarstwo`; uwaga — jedna z tych 9 pozycji to sam URL nadrzędny `/articles_categories/rynki/`, więc realnych branż jest 8). Pozostałe **18** to kategorie „poprzeczne", nieprzypisane do usługi — m.in. `esg`, `ai-w-biznesie`, `automatyzacje-i-ai`, `sukcesja`, `prawo-pracy`, `ekologia`, `inspiracje`, `podcast-grant-thornton`; w tej samej resztce siedzą też trzy kategorie anglojęzyczne (`/en/articles_categories/...`) oraz dublet tematyczny `jpk-cit` i `raportowanie-jpk-cit`.

### Dlaczego to działa

Każdy artykuł ma **z góry określony adresat komercyjny**. Nie ma pytania „do jakiej usługi podlinkować ten tekst" — kategoria artykułu *jest* usługą. Linkowanie artykuł → oferta jest wtedy generowane systemowo, a nie ręcznie per tekst. Odwrotnie też: strona usługi ciągnie moduł „Inne artykuły z kategorii: Doradztwo podatkowe" bez ręcznej kuracji.

Efektem ubocznym są jednak **trzy równoległe systemy taksonomiczne**: `articles_categories` (88), `tag` (81) i `category` (3). Ten ostatni to prawdopodobnie pozostałość po natywnych kategoriach WordPressa. Strony tagów **są indeksowalne**: `/tag/oferty-pracy-raport/` ma `<meta name='robots' content='index, follow, ...'>`, kanoniczny self-referencing i pusty `meta description` (sprawdzone 2026-08-28). [do weryfikacji] czy `tag` i `articles_categories` faktycznie się kanibalizują — nie porównywałem list artykułów pod obiema taksonomiami.

### Dwa równoległe systemy ludzi — i jeden z nich wygląda na osierocony

- `/pracownik/` (CPT `worker`, 314 URL-i) — **linkowany z artykułów i stron usług** (8 wystąpień w HTML zarówno artykułu, jak i strony usługi).
- `/autorzy/` (CPT `custom_author`, 68 URL-i) — **zero linków** w przebadanym artykule, na stronie usługi i w raporcie o ofertach pracy, mimo że URL-e odpowiadają 200 (sprawdzone: `/autorzy/krzysztof-jeromin/` → HTTP 200, `meta robots: index, follow`).

Wygląda to na typ osierocony: żyje w sitemapie, ale nie w linkowaniu wewnętrznym. Kontrola 2026-08-28 rozszerzyła próbę do **pięciu szablonów** (artykuł, strona usługi, raport o ofertach pracy, profil `/pracownik/`, strona główna) — w każdym z nich zero wystąpień ciągu `/autorzy/`. [do weryfikacji] pozostaje, czy typ jest linkowany ze szablonów nieprzebadanych (np. podcast, wydarzenia).

---

## 3. Format artykułu — rozbiór dwóch tekstów

### 3.1. Artykuł ekspercki „problemowy"

`https://grantthornton.pl/publikacja/pierwsze-raportowanie-jpk-kr-pd-jakie-bledy-najczesciej-pojawialy-sie-w-plikach/` (opublikowany 2026-08-27, odczyt 2026-08-28)

- **Objętość obszaru artykułu: ok. 1 700 słów** (liczone od `<h1>` do modułu „Artykuły z kategorii"; pomiar kontrolny 2026-08-28 na tej samej granicy dał 1 677 słów — rozbieżność ~2% wynika ze sposobu liczenia tokenów, nie ze zmiany treści). To istotnie mniej niż sugeruje pierwsze wrażenie — nie jest to tekst 3 000-słowowy.
- **Struktura nagłówków: 10 × H2 + 3 × H3.** **6 z 10 H2 to wprost pytania** (kończą się znakiem zapytania); pozostałe cztery to etykiety tematów: „Prezentacja danych kontrahentów w JPK_KR_PD", „Nieprawidłowości w węźle Dziennik", „1. Data operacji gospodarczej a data ujęcia w księgach" i H2 bloku FAQ. Przykłady pytań:
  - „Dlaczego jakość danych w JPK_KR_PD jest tak istotna?"
  - „Jakie błędy uniemożliwiają wysłanie JPK_KR_PD?"
  - „Jakich podmiotów nie należy wykazywać jako kontrahenta?"
  - „Brak zbilansowania się zapisów – co może oznaczać?"
  - „Jakie problemy występują przy operacjach walutowych?"
- **Blok FAQ na końcu**: H2 „Najczęściej zadawane pytania o błędy w JPK_KR_PD i przygotowanie JPK CIT" + **3 pytania w H3**, opakowane w mikrodane (szczegóły w sekcji 5).
- **Spis treści**: obecny, klikalny (potwierdzony w warstwie widocznej przez WebFetch).
- **Tytuł artykułu operuje bardzo wąską frazą** (`JPK_KR_PD`) — nie „JPK CIT" ogólnie. Tekst łapie długi ogon na etapie, gdy fraza dopiero się formuje.

**Wada strukturalna:** jeden z nagłówków to `H2: 1. Data operacji gospodarczej a data ujęcia w księgach` — numerowany podpunkt na poziomie H2, choć logicznie należy do sekcji „Nieprawidłowości w węźle Dziennik". Hierarchia nagłówków jest tu spłaszczona.

**Linkowanie wewnętrzne w obszarze artykułu** (32 unikalne URL-e):
- `/usluga/` — 43 wystąpienia
- `/publikacja/` — 18
- `/pracownik/` — 8

**Linki zewnętrzne w obszarze artykułu: praktycznie żadne.** Jedyne wychodzące to profil Google News, Twitter i LinkedIn Grant Thornton. Artykuł powołuje się w treści na wyjaśnienia i sekcję Q&A Ministerstwa Finansów, ale **nie linkuje do nich ani nie podaje przypisów do przepisów**. Dla treści YMYL to realna słabość — brak weryfikowalnego śladu źródłowego.

### 3.2. Raport cykliczny z danych własnych

`https://grantthornton.pl/publikacja/oferty-pracy-w-lipcu-2026-rynek-rekrutacyjny-na-plusie/` (odczyt 2026-08-28)

Zupełnie inny gatunek i — z punktu widzenia link buildingu — ważniejszy:

- **Źródło danych: system rekrutacyjny Element dla Grant Thornton**, analizujący ogłoszenia z 50 największych portali rekrutacyjnych w Polsce. Metodologia podana wprost.
- **Konkretne liczby w leadzie**: 275,6 tys. ogłoszeń w lipcu 2026, wzrost o 2% r/r (+4,6 tys. ofert); rozbicie na miasta (Wrocław +9%, Poznań +7%, Warszawa +5%).
- **Cytat imienny eksperta** gotowy do przeklejenia przez dziennikarza: Magdalena Marcinowska, Partner.
- **Krótki** — ok. 230 słów samej prozy (lead + jedna sekcja + cytat). Raport nie musi być długi; nośnikiem jest liczba.
- **Cykl od 2020 roku**, wydania miesięczne.
- CTA: pobranie pełnego raportu PDF + linki do usług outsourcingu kadr i płac.

To jest klasyczny **linkable asset**: własna liczba + nazwisko + regularność. Media branżowe cytują takie dane, bo nie mają alternatywnego źródła.

### Marki publikacji cyklicznych

Widoczne jako osobne kategorie w `articles_categories-sitemap.xml`:

- **Purpurowy Informator** (`/articles_categories/uslugi/raporty/purpurowy-informator/`)
- **Purpurowy Kalkulator** (`/articles_categories/uslugi/raporty/purpurowy-kalkulator/`)
- **Magazyn PLUS** (`/articles_categories/uslugi/raporty/magazyn-plus/`)
- **Barometr prawa** (`/articles_categories/uslugi/barometr-prawa/`)
- **Oferty pracy — raport** (`/articles_categories/uslugi/raporty/oferty-pracy-raport/`)
- **Transfery piłkarskie** (`/articles_categories/uslugi/raporty/transfery_pilkarskie/`)
- **Podcast Grant Thornton** (`/articles_categories/podcast-grant-thornton/`)

Każdy cykl ma **własną nazwę marketingową i własny URL kategorii**, czyli własną stronę zbiorczą akumulującą linki przez lata. „Transfery piłkarskie" w firmie audytorskiej to czysty content marketing pod zasięg i linki z mediów sportowych — temat kompletnie poza ofertą, ale generujący autorytet domeny.

[do weryfikacji] częstotliwość i aktualność poszczególnych marek poza raportem o ofertach pracy (miesięczny) — nie otwierałem każdej kategorii osobno.

---

## 4. E-E-A-T — najmocniejsza warstwa

### Boks eksperta pod artykułem

Zawiera dokładnie (dosłowny odczyt z HTML):

> Elżbieta Ślusarczyk — **Senior Menedżer, Doradca Podatkowy** — „Zobacz dane kontaktowe" — elzbieta.slusarczyk@pl.gt.com — +48 661 538 561

Nagłówek nad boksem: **„Skontaktuj się"**. Czyli boks autora jest jednocześnie **elementem konwersji** — nie tylko sygnałem zaufania. Podanie **komórki eksperta wprost przy artykule** to mocna deklaracja dostępności, rzadka na polskim rynku doradczym.

### Strona eksperta

`https://grantthornton.pl/pracownik/elzbieta-cybulska/` (odczyt 2026-08-28) zawiera:

- stanowisko + tytuł zawodowy („Senior Menedżer, Doradca Podatkowy"),
- **rok dołączenia do zespołu (2010)** — konkretny staż, nie ogólnik („W 2010 roku dołączyła do zespołu doradztwa podatkowego Grant Thornton"),
- opis doświadczenia (bieżące doradztwo, audyty podatkowe, due diligence),
- specjalizacje jako klikalne etykiety — tylko dwie: „Audyt podatkowy" i „Doradztwo podatkowe". VAT i podatki dochodowe pojawiają się wyłącznie w prozie biogramu, nie jako osobne tagi,
- mail i telefon,
- **listę publikacji autora**,
- linki do powiązanych usług (`/usluga/doradztwo-podatkowe/`, `/usluga/doradztwo-podatkowe/audyt-podatkowy/`).

To domyka pętlę: artykuł → ekspert → publikacje eksperta → usługa.

### Dowody zaufania na stronie usługi

Na `/usluga/doradztwo-podatkowe/`:
- **liczby zespołu**: „13 doradców podatkowych w Poznaniu i Warszawie", „25+ obsługiwanych spółek giełdowych", „6000+ rozwiązywanych zagadnień podatkowych rocznie",
- **rankingi**: „Rzeczpospolitej", „Dziennika Gazety Prawnej", ITR World Tax, ITR Tax Awards,
- imienny opiekun usługi: **Dariusz Gałązka, Partner, Biegły rewident**,
- H2 „Grzegorz Szysz i Łukasz Boszko wyróżnieni w jubileuszowej edycji rankingu DGP" — **wyróżnienia branżowe wpuszczone wprost na stronę pieniężną**, nie schowane w zakładce „O nas",
- FAQ na stronie usługi zawiera m.in. pytanie o **uprawnienia doradców i różnice między zawodami** — czyli treść wprost budująca zrozumienie kwalifikacji.

### Czego brakuje

- **Brak numerów wpisu na listę doradców podatkowych / radców prawnych / biegłych rewidentów** w przebadanych profilach. To najtwardszy możliwy dowód kwalifikacji w polskim YMYL, a nie jest wykorzystany.
- **Brak dat przeglądu merytorycznego** („zweryfikowano merytorycznie przez…"). Jest tylko data aktualizacji.
- **Brak recenzenta** — jeden autor, brak drugiej pary oczu w widocznej warstwie i w danych strukturalnych (`reviewedBy` nie występuje).

---

## 5. Dane strukturalne — konkret ze źródła HTML

Analiza `https://grantthornton.pl/publikacja/pierwsze-raportowanie-jpk-kr-pd-.../` oraz `/usluga/doradztwo-podatkowe/`, pobrane 2026-08-28.

### Artykuł: 3 bloki JSON-LD

**Blok 1 — graf Yoast SEO:** `WebPage` + `BreadcrumbList` + `WebSite`.
Uwaga: w grafie Yoast **nie ma węzła `Article` ani `Person`**.

**Blok 2 — ręczny graf firmowy:** `Organization` + **7 × `AccountingService`**, po jednym na biuro, każdy z pełnym `PostalAddress`:

| Miasto | Adres | Telefon |
|---|---|---|
| Poznań | Antoniego Baraniaka 88E, 61-131 | +48 61 625 1100 |
| Warszawa | Chłodna 52, 00-872 | +48 22 205 4800 |
| Wrocław | Sokolnicza 5/71, 53-676 | +48 71 733 7560 |
| Katowice | Francuska 34, 40-028 | 48 32 721 3700 |
| Kraków | Dietla 52, 31-039 | +48 12 307 0785 |
| Toruń | Grudziądzka 46-48, 87-100 | +48 56 663 7040 |
| Toruń | Bartosza Głowackiego 20, 87-100 | +48 56 657 5591 |

Ten blok jest **wstrzykiwany na każdą stronę** (występuje identycznie na artykule i na stronie usługi). Sygnał lokalny jest więc rozprowadzony po całym serwisie, a nie tylko na stronie kontaktu.

**Blok 3 — ręczny `Article`:**
```json
{
  "headline": "Pierwsze raportowanie JPK KR PD – jakie błędy...",
  "author": {"@type": "Person", "name": "Elżbieta Ślusarczyk"},
  "datePublished": "2026-08-27",
  "dateModified": "2026-08-10",
  "publisher": {"@type": "Organization", "name": "Grant Thornton", "logo": {...}},
  "description": "Jakie błędy w JPK_KR_PD pojawiają się najczęściej? ...",
  "mainEntityOfPage": "https://grantthornton.pl/publikacja/..."
}
```

### FAQ — mikrodane, nie JSON-LD

Na stronach z blokiem FAQ element `<html>` dostaje:
```html
<html itemscope itemtype="https://schema.org/FAQPage" lang="pl-PL">
```
plus **3 komplety** `schema.org/Question` + `acceptedAnswer` + `schema.org/Answer` + `itemprop="name"`.

Deklaracja jest **warunkowa i poprawna** — sprawdziłem trzy strony bez FAQ (strona główna, `/pracownik/elzbieta-cybulska/`, raport o ofertach pracy) i żadna nie ma `FAQPage` na `<html>` ani węzłów `Question`. Czyli szablon dokłada znaczniki tylko tam, gdzie FAQ realnie istnieje.

### Wady danych strukturalnych — lista do wykorzystania

1. **`author` bez `@id` i bez `url`.** Encja autora w JSON-LD to gołe `{"@type":"Person","name":"..."}`. Nie jest połączona ze stroną `/pracownik/...`, mimo że ta strona istnieje i zawiera bio, specjalizacje i tytuł zawodowy. Google nie dostaje sygnału, że to ta sama osoba. **Najtańsza możliwa poprawa E-E-A-T w całym serwisie.**
2. **`dateModified` (2026-08-10) wcześniejszy niż `datePublished` (2026-08-27)** w bloku `Article` — sprzeczne z blokiem `WebPage`, gdzie obie daty to `2026-08-27T13:41:01+00:00`. Warstwa widoczna idzie za blokiem `Article`: `single-article__meta__published` to 27.08.2026, a `single-article__meta__updated` to „Aktualizacja: 10.08.2026". Czyli dwa źródła dat rozjeżdżają się między sobą, a to widoczne pokazuje aktualizację wcześniejszą niż publikacja.
3. **`BreadcrumbList` ma tylko 2 poziomy**: „Strona główna" → tytuł artykułu. Cała hierarchia `uslugi/doradztwo-podatkowe/...` **nie jest odwzorowana w okruszkach**, mimo że istnieje w taksonomii. To samo na stronie usługi: „Strona główna" → „Doradztwo podatkowe", bez poziomów pośrednich.
4. **Brak schematu `Service`** na stronach usług (sprawdzone: `"Service"` nie występuje w HTML `/usluga/doradztwo-podatkowe/`).
5. **Brak `reviewedBy`, brak `about`, brak `citation`** — nic, co wiązałoby tekst YMYL z weryfikacją lub źródłem prawa.

---

## 6. Warstwa techniczna — zaniedbania

Wszystko sprawdzone 2026-08-28 na żywym HTML.

| Problem | Dowód |
|---|---|
| **Puste `meta description`** na artykule, stronie usługi i stronie eksperta | `<meta name="description" content="">` — pusty na `/publikacja/pierwsze-raportowanie-jpk-kr-pd-.../`, `/usluga/doradztwo-podatkowe/`, `/pracownik/elzbieta-cybulska/`, `/publikacja/oferty-pracy-w-lipcu-2026-.../`. Strona główna ma wypełniony. Co istotne, **opis istnieje w JSON-LD** (`description` w bloku `Article`), ale nie trafia do metatagu. |
| **Zdublowany sufiks marki w `<title>`** | `Pierwsze raportowanie JPK KR PD - ... - GrantThornton - Grant Thornton`; `Doradztwo podatkowe - GrantThornton - Grant Thornton`; `Elżbieta Ślusarczyk - PL - GrantThornton - Grant Thornton`. Marnuje ~20 znaków w każdym tytule i w profilu eksperta dokłada jeszcze śmieciowe „- PL". |
| **hreflang wskazuje na stronę główną, nie na tłumaczenie** | Na polskim artykule jedyne `hreflang` to `en-GB` → `https://grantthornton.pl/en/`. Tymczasem tłumaczenia artykułów istnieją (np. `/en/article/accounting-outsourcing-for-a-holding-company-.../`). Wersje EN i DE (263 + 2 URL-e) nie są poprawnie sparowane z polskimi. |
| **Rozjazd slug ↔ nazwisko** | Boks autora „Elżbieta Ślusarczyk" linkuje do `/pracownik/elzbieta-cybulska/`. Najpewniej zmiana nazwiska bez aktualizacji sluga. Kosmetyczne, ale w profilu eksperta URL i nazwisko powinny się zgadzać. |
| **Typ `/autorzy/` osierocony** | 68 URL-i w sitemapie, 0 linków w przebadanych szablonach artykułu i usługi. |

### Co jest zrobione dobrze technicznie

- **`robots.txt`** blokuje wyniki wyszukiwarki wewnętrznej i strony „dziękujemy za udział" (typowy thin content po wydarzeniach):
  ```
  Disallow: /*?*s=
  Disallow: /search/
  Disallow: /sniadanie-biznesowe-...-dziekujemy-za-udzial/
  ```
  Uwaga: te reguły stoją w bloku `User-Agent: Googlebot`. Jedyny blok `User-agent: *` zawiera wyłącznie `Disallow: /wp-content/uploads/wp-import-export-lite/`, więc dla pozostałych botów wyszukiwarka wewnętrzna i strony „dziękujemy" nie są zablokowane. W pliku jest też `Disallow: /wsparcie/`. Źródło: `https://grantthornton.pl/robots.txt`, 2026-08-28.
- **Sitemapa Google News** (`news-sitemap.xml`, 5 najświeższych URL-i) + link do profilu wydawcy w Google News w stopce artykułu. Kanał, którego polskie kancelarie prawie nie używają.
- **Kanoniczne self-referencing** poprawne.
- **`meta robots` bez restrykcji** na treści — wszystkie badane szablony (artykuł, usługa, pracownik, autorzy, tag) mają `index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1`. Nic nie jest przypadkiem odcięte.

---

## 7. Częstotliwość publikacji

Z `lastmod` w czterech sitemapach artykułów (3 330 polskich publikacji):

| Miesiąc | Liczba URL z `lastmod` |
|---|---|
| 2025-08 | 45 |
| 2025-09 | 44 |
| 2025-10 | 58 |
| 2025-11 | 14 |
| 2025-12 | 36 |
| 2026-01 | **103** |
| 2026-02 | 45 |
| 2026-03 | 43 |
| 2026-04 | 55 |
| 2026-05 | 50 |
| 2026-06 | 52 |
| 2026-07 | 49 |
| 2026-08 (do 28.) | **123** |

**Zastrzeżenie:** `lastmod` to data ostatniej modyfikacji, nie publikacji. Liczby mieszają nowe teksty z aktualizacjami starych.

Rytm bazowy to **ok. 45–55 URL-i miesięcznie** (ok. 12 tygodniowo). Dwa wyraźne szczyty:
- **styczeń 2026 (103)** — zmiany prawa wchodzące od 1 stycznia; klasyczna kumulacja w branży podatkowej,
- **sierpień 2026 (123)** — nietypowo wysoki jak na wakacje i jak na niepełny miesiąc. Może oznaczać kampanię masowej aktualizacji archiwum. [do weryfikacji] — nie rozdzieliłem nowych publikacji od odświeżeń.

### Rozkład roczny `lastmod` — sygnał rozkładu treści

| Rok | URL-i |
|---|---|
| 2018 | 356 |
| 2019 | **764** |
| 2020 | 153 |
| 2021 | 25 |
| 2022 | 263 |
| 2023 | 234 |
| 2024 | 544 |
| 2025 | 471 |
| 2026 | 520 |

**1 120 URL-i (34% archiwum) nie było ruszanych od 2018–2019 roku.** W treściach podatkowych to materiał w dużej mierze nieaktualny — a mimo to nadal w sitemapie i, jak można zakładać, w indeksie. To realny balast, który rozcieńcza sygnał świeżości domeny. Grant Thornton ewidentnie publikuje szybciej, niż porządkuje archiwum.

---

## 8. Łączenie treści z ofertą

Trzy mechanizmy działające równolegle:

1. **Taksonomia** — kategoria artykułu *jest* usługą (sekcja 2). Powiązanie jest systemowe, nie redakcyjne.
2. **Boks eksperta = CTA** — nagłówek „Skontaktuj się", nazwisko, tytuł zawodowy, mail, komórka. Zaufanie i konwersja w jednym module.
3. **Formularze kontekstowe** — na artykule „Porozmawiajmy o Twoich wyzwaniach" (imię/nazwisko, e-mail/telefon, zgoda RODO); na stronie usługi „Zapytaj o ofertę". Krótkie, 2–3 pola.

Dodatkowo w treści artykułu jest **kontekstowy link do dokładnie pasującej usługi** — w analizowanym tekście do „JPK CIT: wsparcie podatkowe, księgowe i technologiczne". Nie ogólne „doradztwo podatkowe", tylko podusługa odpowiadająca problemowi z artykułu. To jest możliwe wyłącznie dlatego, że katalog usług jest rozdrobniony do 246 pozycji.

**Newsletter** jest wpięty w wiele miejsc (2 × H4 „Newsletter" w samym artykule), z segmentacją po **regionie i obszarach zainteresowania** — czyli od razu buduje bazę do przypisania handlowcowi.

**Moduły recyrkulacji** pod artykułem: „Artykuły z kategorii: JPK CIT", „Najczęściej czytane" (3 pozycje), „Zobacz także", plus odnośniki do wyspecjalizowanych blogów satelickich na osobnych domenach: `doradcypodatkowi.blog`, `ksiegowoscjestsexy.blog`, `poradnikhr.blog`.

---

## 9. Wnioski dla mentzen.pl

Uszeregowane wg stosunku efektu do kosztu.

### A. Do skopiowania — wysoki zwrot

**1. Zepnij taksonomię bloga z taksonomią oferty.**
To jest główna lekcja z tej domeny. Kategorie wpisów powinny odwzorowywać strukturę usług, żeby linkowanie tekst → oferta było generowane przez szablon, a nie decydowane per artykuł. Przy WordPressie mentzen.pl da się to zrobić własną taksonomią odwzorowującą drzewo usług. Bez tego każdy nowy tekst wymaga ręcznej decyzji, gdzie podlinkować — i w praktyce ta decyzja nie zapada.

**2. Boks autora jako moduł konwersji, nie tylko stopka.**
Wzór Grant Thornton: nagłówek „Skontaktuj się" + zdjęcie + nazwisko + **tytuł zawodowy** + mail + telefon. Mentzen ma tu naturalną przewagę — kancelaria z rozpoznawalnymi nazwiskami. Warto sprawdzić, czy istniejąca wtyczka `mentzen-autorzy` (box autora + drugi autor, klucze meta `drugi_autor_opcjonalnie`/`dzial`/`calendesk`) może dołożyć tytuł zawodowy i dane kontaktowe — infrastruktura już jest, brakuje pól.

**3. Rozdrobnij katalog usług.**
246 stron usługowych pozwala linkować z artykułu do usługi *dokładnie* odpowiadającej problemowi. Przy kilkunastu ogólnych stronach oferty każdy link prowadzi do zbyt szerokiej strony i traci intencję. To warunek konieczny punktu 1.

**4. Zbuduj co najmniej jeden cykl na własnych danych.**
To najsłabiej obsadzone pole u polskiej konkurencji doradczej i jedyne, które generuje linki bez outreachu. Kancelaria ma dane, których nie ma nikt inny — np. rozkład rodzajów spraw, czas uzyskania interpretacji indywidualnej, statystyka rozstrzygnięć. Wzór: **konkretna liczba w leadzie + metodologia + cytat imienny + stała częstotliwość + własna nazwa marki + własny URL kategorii**. Raport nie musi być długi — analizowany miesięcznik ma ok. 230 słów prozy.

**5. Nagłówki H2 jako pytania klienta.**
W analizowanym artykule 6 z 10 H2 to wprost pytania; reszta to etykiety tematów. Plus blok FAQ z mikrodanymi `FAQPage` warunkowo dokładanymi tylko tam, gdzie FAQ istnieje.

**6. Sitemapa Google News.**
Grant Thornton ją ma i linkuje profil wydawcy ze stopki artykułu. Przy treściach o zmianach prawa to realny kanał, prawie nieużywany przez polskie kancelarie.

### B. Luki konkurenta — tu Mentzen może wygrać przewagą, nie parytetem

**7. Powiąż encję autora w danych strukturalnych ze stroną eksperta.**
Grant Thornton tego **nie robi** — `author` to gołe `{"@type":"Person","name":"..."}`. Mentzen powinien dawać `author` z `@id` i `url` wskazującym na stronę autora, a na tej stronie pełną encję `Person` z `jobTitle`, `knowsAbout`, `worksFor` i `sameAs`. Koszt: jedna zmiana w szablonie.

**8. Dodaj numery wpisu na listy zawodowe.**
Ani Grant Thornton, ani (wg wcześniejszej analizy) Crido tego nie eksponują. W polskim YMYL numer wpisu na listę doradców podatkowych czy radców prawnych jest **najtwardszym możliwym dowodem kwalifikacji** — weryfikowalnym w publicznym rejestrze. To przewaga dostępna od ręki.

**9. Linkuj do źródeł prawa.**
Analizowany artykuł YMYL powołuje się na wyjaśnienia MF, ale **nie linkuje do nich**. Zero cytowań zewnętrznych w treści. Mentzen linkujący do ISAP, interpretacji na eureka.mf.gov.pl i orzeczeń buduje ślad weryfikowalności, którego konkurencja nie ma.

**10. Dodaj przegląd merytoryczny z datą.**
Nikt w tej grupie konkurentów nie pokazuje recenzenta. „Zweryfikowano merytorycznie: [nazwisko, tytuł zawodowy], [data]" + `reviewedBy` w danych strukturalnych to widoczny wyróżnik przy treściach podatkowych.

**11. Rób pełne okruszki.**
`BreadcrumbList` u Grant Thornton ma 2 poziomy i gubi całą hierarchię tematyczną. Mentzen powinien odwzorować w okruszkach pełną ścieżkę kategorii — zarówno wizualnie, jak i w JSON-LD.

**12. Nie zostawiaj pustych `meta description`.**
Grant Thornton ma je puste na stronach usług, artykułach i profilach ekspertów, mimo że opis istnieje w JSON-LD. Podstawowa higiena, której konkurent nie utrzymuje.

### C. Ostrzeżenia — czego nie kopiować

**13. Nie publikuj szybciej, niż porządkujesz archiwum.**
34% archiwum Grant Thornton nie było ruszane od 2018–2019. W podatkach to treść w dużej mierze nieaktualna, wciąż w sitemapie. Dla mentzen.pl — mniejszej domeny — taki balast byłby proporcjonalnie groźniejszy. **Zaplanuj cykl przeglądu (np. rewizja lub deindeksacja po 24 miesiącach) razem z planem publikacji, a nie po fakcie.**

**14. Nie mnóż równoległych taksonomii.**
Trzy systemy (`articles_categories`, `tag`, `category`) i dwa typy ludzi (`worker`, `autorzy` — ten drugi osierocony) to dług narosły przez lata. Jedna taksonomia treści i jeden typ autora od początku.

**15. Pilnuj spójności dat.**
`dateModified` wcześniejszy niż `datePublished` w jednym bloku, inne wartości w drugim, a warstwa widoczna pokazuje „Aktualizacja" sprzed daty publikacji. Jedno źródło prawdy dla dat.

---

## Źródła

Wszystkie odczyty wykonane 2026-08-28.

- `https://grantthornton.pl/` — struktura nawigacji i oferty
- `https://grantthornton.pl/robots.txt`
- `https://grantthornton.pl/sitemap_index.xml` oraz sitemapy: `article-sitemap[1-4]`, `service-sitemap`, `worker-sitemap`, `custom_author-sitemap`, `articles_categories-sitemap`, `news-sitemap`, `post-sitemap`, `page-sitemap`, `tag-sitemap`, `event-sitemap`, `market-sitemap`, `career-sitemap`, `category-sitemap`
- `https://grantthornton.pl/publikacja/pierwsze-raportowanie-jpk-kr-pd-jakie-bledy-najczesciej-pojawialy-sie-w-plikach/` — źródło HTML, JSON-LD, mikrodane, nagłówki, linkowanie
- `https://grantthornton.pl/publikacja/oferty-pracy-w-lipcu-2026-rynek-rekrutacyjny-na-plusie/` — format raportu cyklicznego
- `https://grantthornton.pl/usluga/doradztwo-podatkowe/` — źródło HTML strony usługi
- `https://grantthornton.pl/pracownik/elzbieta-cybulska/` — profil eksperta
- `https://grantthornton.pl/autorzy/krzysztof-jeromin/` — weryfikacja typu `custom_author` (HTTP 200)
- `https://grantthornton.pl/artykuly/` — hub artykułów
- `https://grantthornton.pl/articles_categories/uslugi/raporty/` — publikacje cykliczne

Notatki powiązane: `konkurencja-crido-pl.md`, `web-eeat-ymyl.md`, `mentzen-struktura.md`, `mentzen-blog.md`
