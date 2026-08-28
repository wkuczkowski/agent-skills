# Blog mentzen.pl — jak jest zbudowany i o czym pisze

Data badania: 2026-08-28. Zakres: publiczny blog Kancelarii Mentzen (`https://mentzen.pl/blog/`).
Cel: opis stanu faktycznego pod przyszły skill SEO. To nie jest audyt techniczny.

Źródła danych: publiczne REST API WordPressa (`/wp-json/wp/v2/posts|categories|tags`, 523 wpisy,
pełna treść), HTML pojedynczych wpisów (szablon, schema Yoast, box autora) oraz lektura 7 wpisów
w całości.

Stack: WordPress + motyw Divi, Yoast SEO 28.3, własny box autora (klasy `aw-author-*`),
rezerwacja konsultacji przez Calendesk.

---

## 1. Skala i kategorie

523 opublikowane wpisy, 39 kategorii (drzewo dwu- i trójpoziomowe), 1547 tagów.

Rozkład wpisów po kategoriach (wpis może mieć kilka):

| Kategoria | Wszystkie | 2025–2026 | URL |
|---|---|---|---|
| Doradztwo podatkowe | 197 | 42 | https://mentzen.pl/blog/category/doradztwo-podatkowe/ |
| Doradztwo prawne | 84 | 28 | https://mentzen.pl/blog/category/doradztwo-prawne/ |
| Ulgi podatkowe | 48 | 11 | https://mentzen.pl/blog/category/doradztwo-podatkowe/ulgi-podatkowe/ |
| Księgowość | 32 | 0 | https://mentzen.pl/blog/category/ksiegowosc/ |
| Inne | 25 | 7 | https://mentzen.pl/blog/category/inne/ |
| VAT | 24 | 0 | https://mentzen.pl/blog/category/doradztwo-podatkowe/vat/ |
| Optymalizacja podatkowa | 18 | 1 | https://mentzen.pl/blog/category/doradztwo-podatkowe/optymalizacja-podatkowa/ |
| CIT estoński | 11 | 8 | https://mentzen.pl/blog/category/doradztwo-podatkowe/cit-estonski/ |
| Spółki | 11 | 0 | https://mentzen.pl/blog/category/doradztwo-prawne/spolki/ |
| Fundacja rodzinna | 4 | 4 | https://mentzen.pl/blog/category/doradztwo-prawne/fundacja-rodzinna/ |
| Kadry | 4 | 4 | https://mentzen.pl/blog/category/inne/kadry/ |
| Kryptowaluty | 6 | 3 | https://mentzen.pl/blog/category/inne/kryptowaluty/ |

Dziewięć kategorii ma 0 wpisów (m.in. „Składka zdrowotna”, „Spółka jawna”, „Spółka cywilna”,
„Spółka komandytowa”, „Zatrudnienie B2B”, „Doradztwo podatkowe dla budownictwa”) — struktura
została zaprojektowana szerzej, niż potem zapełniono treścią.

Struktura URL wpisu odzwierciedla kategorię i bywa niespójna głęboko:
- 338 wpisów: `/blog/{kategoria}/{slug}/` — np. https://mentzen.pl/blog/doradztwo-podatkowe/opodatkowanie-prop-tradingu/
- 166 wpisów: `/blog/{kat}/{podkat}/{slug}/` — np. https://mentzen.pl/blog/inne/kadry/opieka-na-dziecko-16-godzin-czy-2-dni-co-wybrac-i-kiedy-to-sie-bardziej-oplaca/
- 19 wpisów: cztery segmenty.

Tagów jest 1547 przy 523 wpisach — czyli ok. 3 na wpis, w większości jednorazowe. Najczęstsze:
`doradztwo podatkowe` (169), `vat` (84), `audyty` (73), `PIT` (71), `ulgi podatkowe` (68),
`podatki` (45), `spółki` (42). Jest też tag operacyjny `wylaczonewyszukiwanie` (38 wpisów) —
używany do sterowania wyświetlaniem, nie do tematyki.

Archiwa kategorii mają `noindex, follow` (Yoast), np.
https://mentzen.pl/blog/category/doradztwo-podatkowe/ — czyli kategorie pełnią funkcję nawigacyjną
i porządkującą URL-e, nie są celowanymi landingami SEO. Same wpisy są `index, follow`
(522 z 523; jeden wpis ma `noindex, nofollow`).

## 2. Główne tematy

Blog trzyma się bardzo wąsko profilu B2B kancelarii — podatki i prawo dla przedsiębiorcy, zero
treści lifestyle'owych i zero treści politycznych.

Powtarzające się osie tematyczne (2025–2026):

- **B2B kontra umowa o pracę i przekwalifikowanie kontraktu** — seria kilku wpisów, także z PIP
  w tle. Np. https://mentzen.pl/blog/doradztwo-podatkowe/zwrot-pit-po-przekwalifikowaniu-b2b/,
  https://mentzen.pl/blog/doradztwo-podatkowe/przekwalifikowanie-b2b-umowa-o-prace-pit-pracownik/,
  https://mentzen.pl/blog/doradztwo-prawne/nowe-kompetencje-pip-od-2026-roku-a-ustalenie-istnienia-stosunku-pracy-co-musza-wiedziec-firmy/
- **Fundacja rodzinna** (sukcesja, najem, licencje, audyt, rejestr) — np.
  https://mentzen.pl/blog/doradztwo-podatkowe/czy-przychody-licencyjne-fundacji-rodzinnej-sa-bezpieczne-podatkowo/,
  https://mentzen.pl/blog/doradztwo-prawne/audyt-w-fundacji-rodzinnej/
- **CIT estoński** — przekształcenia, ukryte zyski, zmiany 2026. Np.
  https://mentzen.pl/blog/doradztwo-podatkowe/zyski-sprzed-przeksztalcenia-a-estonski-cit/
- **Klauzula GAAR / spory z fiskusem** — mini-seria trzech wpisów w 2026 r., np.
  https://mentzen.pl/blog/doradztwo-podatkowe/10-lat-klauzuli-gaar-czy-jest-lepiej/
- **Ulgi: B+R, IP Box, ulga na ekspansję** — np.
  https://mentzen.pl/blog/doradztwo-podatkowe/ulga-br-i-ip-box-czy-mozna-stosowac-je-jednoczesnie/
- **KSeF 2026** — https://mentzen.pl/blog/doradztwo-podatkowe/ksef-2026-rewolucja-w-fakturowaniu-na-ktora-trzeba-przygotowac-sie-juz-dzis/,
  https://mentzen.pl/blog/doradztwo-podatkowe/tryby-offline-w-ksef/
- **Kadry i prawo pracy** — staż pracy, opieka na dziecko, zatrudnianie cudzoziemców.
- **Nisze przychodowe klientów** — prop trading, kryptowaluty, najem nieruchomości, IT.

Dominuje **treść reaktywna, wywołana zmianą prawa albo świeżą interpretacją KIS / wyrokiem NSA**
(„Nowe zasady…”, „Zmiany w…”, „Czy coś zmieni się w… w 2026 roku?”). Treści evergreen typu
kompletny poradnik są głównie starsze (2021–2022) i nieaktualizowane — np.
https://mentzen.pl/blog/doradztwo-podatkowe/skladka-zdrowotna-wszystko-co-powinienes-wiedziec/
(2022-10-31) nadal operuje płacą minimalną 3010 zł z 2022 r.

## 3. Częstotliwość publikacji

Wpisy wg roku publikacji: 2020 – 22, 2021 – 36, 2022 – 141, 2023 – 118, 2024 – 91, 2025 – 87,
2026 (do 28.08) – 28.

Miesięcznie 2025–2026: 2025-03: 12, 2025-04: 12, 2025-07: 10, 2025-09: 8, 2025-10: 8, 2025-11: 6,
2025-12: 3, 2026-01: 4, 2026-02: 2, 2026-03: 4, 2026-04: 4, 2026-05: 1, 2026-06: 2, 2026-07: 4,
2026-08: 7.

Wniosek: szczyt aktywności 2022 (~12 wpisów/mies.), potem stały spadek do ok. 3–4 wpisów/mies.
w 2026 r., z odbiciem w sierpniu 2026 (7 wpisów). W ostatnich 12 miesiącach: 60 wpisów, mediana
odstępu między publikacjami 5 dni, najdłuższa przerwa 37 dni (maj–czerwiec 2026). Kadencja jest
nieregularna — nie ma stałego dnia ani stałego tempa.

## 4. Typowa struktura wpisu

Pomiary na pełnej treści (`content.rendered`) wszystkich 523 wpisów:

| Cecha | Wszystkie wpisy | Wpisy od 2025 |
|---|---|---|
| Długość (słowa) — mediana | 549 | 479 |
| Długość — średnia | 615 | 510 |
| Długość — zakres | 2–2418 | 194–1213 |
| H2 — mediana | 4 | 4 |
| H3 — mediana | 0 | 0 |
| Linki wewnętrzne w treści — mediana | 0 | 0 |
| Wpisy bez żadnego linku wewnętrznego | 403 / 523 | 70 / 115 |
| Wpisy z sekcją FAQ | 3 | 3 |
| Wpisy z tabelą | 9 | 3 |
| Wpisy z listą punktowaną | 283 | 76 |

Rozkład nagłówków w całym korpusie: 2003 × H2, 90 × H3, 38 × H1 (H1 wewnątrz treści — czyli
duplikat tytułu), pojedyncze H4/H5/H6. Praktycznie brak zagnieżdżenia — płaska struktura H2.

Schemat, który się powtarza:

1. **Lead bez nagłówka** — 1–3 akapity wprowadzenia, często definicja pojęcia. Kilka wpisów
   startuje od razu H2 (np. ulga B+R / IP Box).
2. **4–6 sekcji H2**, bardzo często sformułowanych jako **pytanie klienta**: „Kto płaci zaliczki”,
   „Kiedy warto rozważyć taką umowę?”, „Czy pracownik może odzyskać wcześniej zapłacony PIT?”,
   „Gdzie fiskus poluje najczęściej?”. To najsilniejszy powtarzalny wzorzec redakcyjny bloga.
3. **Baner CTA wklejony w środek tekstu** — obrazek-link bez tekstu alternatywnego, prowadzący do
   `https://mentzen.pl/mentzen-plus/` (36 wpisów) lub `https://mentzen.pl/mentzen-prime/` (6 wpisów).
   Wstawiany zwykle po 2.–4. sekcji H2. W wpisach od 2025 r. jest w 40 ze 115.
4. **„Podsumowanie” / „Co warto zapamiętać?” / „Wnioski”** jako ostatni H2 — obecne w większości
   dłuższych wpisów.
5. Opcjonalnie: **lista powołanych interpretacji indywidualnych KIS z sygnaturami** na końcu (np.
   https://mentzen.pl/blog/doradztwo-podatkowe/opodatkowanie-prop-tradingu/ — pięć sygnatur),
   albo osadzone wideo pod H2 „Zobacz nasz ostatni film” (53 wpisy zawierają odwołanie do
   YouTube/filmu).

Ton: rzeczowy, „ekspert tłumaczy przedsiębiorcy”, druga osoba pojedyncza pojawia się nieregularnie.
Starsze wpisy (2022–2024) bywają wyraźnie bardziej publicystyczne i żartobliwe w tytułach
(„Daj psu pełną michę i wrzuć go w koszty”, „Faktura pro forma: hit czy kit?”,
„Dopłaty do kapitału — sztuczka kreatywnego księgowego”). Wpisy z 2026 r. są krótsze, chłodniejsze
i bardziej „interpretacyjne”.

**FAQ** to wyjątek, nie standard — trzy wpisy, wszystkie z działu prawnego, wszystkie z 2025–2026.
Wzorcowy: https://mentzen.pl/blog/doradztwo-prawne/regulamin-newslettera-co-musi-zawierac/ — ma H2
„FAQ – regulamin newslettera” i cztery pytania jako H3, a pod nimi H2-CTA
„Potrzebujesz regulaminu newslettera dopasowanego do swojego modelu działania?”. Ten wpis nie ma
przy tym schematu `FAQPage` — Yoast generuje wyłącznie `Article` + `WebPage` + `BreadcrumbList` +
`Organization` + `Person`.

**CTA w treści** występuje w trzech formach:
- baner graficzny w środku (opisany wyżej), praktycznie jedyny link do stron usługowych z treści;
- zdanie zamykające typu „W razie zainteresowania zawarciem umowy spółki cichej zapraszamy na
  konsultację” (https://mentzen.pl/blog/doradztwo-prawne/umowa-spolki-cichej-elastyczna-forma-finansowania-dzialalnosci/) —
  bez linku, sam tekst;
- osobny H2-CTA na końcu (tylko nieliczne wpisy, jak regulamin newslettera).

Metadane: mediana długości `<title>` 72 znaki, mediana meta description 139 znaków, 12 wpisów bez
opisu. Opisy są pisane pod klik i zwykle kończą się wezwaniem: „Sprawdź zasady opodatkowania.”,
„Sprawdź, jak działa ta umowa i co musi regulować, by była bezpieczna.”

## 5. Autorzy i sygnały E-E-A-T

144 różnych autorów na 523 wpisy — bardzo rozproszone autorstwo, mediana kilku wpisów na osobę.
Najwięcej opublikowali: Natalia Filipska (20), Kacper Boroń (18), Wioletta Rudewicz (14),
Natalia Majewska (12), Aleksander Serwiński (12), Marta Matuszewska (11), Klaudia Grzesiak (11).

Od 2025 r. aktywnych jest 42 autorów, żaden nie przekracza 6 wpisów. Czołówka bieżąca: Kacper
Boroń, Konrad Tonkiewicz, Szymon Mackiewicz, Maciej Malicki, Marta Matuszewska, Joanna Zawadzka,
Krzysztof Soczyński — po 6 wpisów.

Co jest po stronie E-E-A-T:

- **Box autora pod wpisem** (`div.aw-author-card`): zdjęcie 64–80 px, imię i nazwisko, stanowisko
  z działem. Przykłady stanowisk: „Starszy Ekspert - Dział doradztwa podatkowego” (Kacper Boroń),
  „Dział kadr i płac” (Julia Gitner), „Radca Prawny, Dział Doradztwa Prawnego” (Joanna Zawadzka),
  „Menedżer, Doradca podatkowy” (Aleksander Serwiński), „Starszy Ekspert, Doradca podatkowy
  i Radca prawny” (Kamil Wielewicki). Stanowiska są konkretne i zawierają tytuły zawodowe.
- **CTA przy autorze**: dwa przyciski — „Umów konsultację” (deep link do Calendesk z parametrami
  `services` i `employees`, czyli rezerwacja u konkretnej osoby) oraz „Napisz do nas”
  (`/konsultacje-online`). To realny sygnał „autor jest praktykiem, u którego można kupić usługę”.
- **Schema `Person`** w grafie Yoast na każdym wpisie: `name`, `image`, `description` (stanowisko),
  `url` archiwum autora. Przykład:
  `{"@type":"Person","name":"Kacper Boroń","description":"Ekspert, Doradca podatkowy","url":"https://mentzen.pl/blog/author/kacper-boron/"}`.
- **Archiwa autorów** działają i są indeksowalne: https://mentzen.pl/blog/author/kacper-boron/,
  https://mentzen.pl/blog/author/szymon-mackiewicz/ — tytuł „Imię Nazwisko, Autor w serwisie
  Kancelaria Mentzen”, box autora u góry, lista wpisów.
- **Cytowanie źródeł pierwotnych w treści** — konkretne przepisy z artykułami („art. 22 ust. 9
  pkt 4 ustawy o PIT”, „art. 188 Kodeksu pracy”, „art. 353¹ k.c.”), sygnatury interpretacji KIS
  z datami, powołania na objaśnienia ZUS i wyroki NSA. To najmocniejszy merytoryczny sygnał
  eksperckości na tym blogu.
- **Daty**: `datePublished` i `dateModified` w schema; przy wpisie z 27.08.2026 różnica dwóch minut,
  czyli daty modyfikacji odzwierciedlają realne edycje, nie sztuczne odświeżanie.
- `wordCount` i `keywords` w schema `Article`, `articleSection` = nazwa kategorii.

Czego brakuje jako sygnału:

- **Box autora nie zawiera biogramu** — tylko imię, nazwisko, stanowisko. Zero zdań o
  doświadczeniu, specjalizacji, publikacjach, numerze wpisu na listę doradców podatkowych.
- **Imię autora w boxie nie jest linkiem** do archiwum autora ani do profilu na
  https://mentzen.pl/nasz-zespol/. Archiwa autorów istnieją, ale z poziomu wpisu nie da się do
  nich przejść.
- **Yoast `Person.description` jest puste dla ok. 1/3 autorów**, w tym dla większości najbardziej
  aktywnych w 2025–2026 (Szymon Mackiewicz, Maciej Malicki, Marta Matuszewska, Krzysztof
  Soczyński, Jakub Rojewski, Bartosz Burski, Julia Gitner). Stanowisko wyświetla się wtedy tylko
  w HTML boxa, nie w danych strukturalnych.
- **Brak `sameAs`** przy `Person` (LinkedIn, profil w zespole) i brak `author` powiązanego
  z profilem organizacji.
- Rotacja autorów jest tak duża, że żadna osoba nie buduje rozpoznawalnej sygnatury tematycznej —
  ten sam temat (np. fundacja rodzinna) piszą różni ludzie w kolejnych miesiącach.

## 6. Linkowanie wewnętrzne

To najsłabszy element całej konstrukcji.

W treści 523 wpisów jest łącznie **158 linków wewnętrznych** do 74 unikalnych celów — czyli
0,3 linku na wpis, mediana 0. **403 wpisy (77%) nie mają w treści ani jednego linku wewnętrznego.**
Wśród wpisów od 2025 r. jest nieco lepiej, ale wciąż 70 ze 115 (61%) nie linkuje nigdzie.

Rozkład celów tych 158 linków:

| Cel | Liczba |
|---|---|
| https://mentzen.pl/mentzen-plus/ | 36 |
| https://www.mentzen.pl/ulga-ip-box-dla-programistow/ | 10 |
| https://mentzen.pl/dane-kontaktowe/ | 9 |
| https://mentzen.pl/mentzen-prime/ | 6 |
| https://www.mentzen.pl/podatek-liniowy-czy-skala-podatkowa-co-wybrac/ | 5 |
| https://www.mentzen.pl/ulga-br/ | 5 |
| https://mentzen.pl/cit-estonski/ | 3 |
| https://www.mentzen.pl/ceny-transferowe/ | 3 |
| pozostałe (pojedynczo) | ~70 |

Obserwacje:

- Zdecydowana większość linków wewnętrznych to **ten sam baner do `/mentzen-plus/`**, a nie link
  kontekstowy z tekstu. Realne linki z frazy w zdaniu są rzadkością.
- **Strony usługowe prawie nie dostają linków z bloga.** Poza `/mentzen-plus/` i `/mentzen-prime/`
  są to pojedyncze trafienia: `/cit-estonski/` (3), `/ceny-transferowe/` (3),
  `/rezydencja-podatkowa/` (2). Landingi takie jak `/fundacja-rodzinna/`, `/vat/`, `/kryptowaluty/`,
  `/sukcesja/`, `/znaki-towarowe/`, `/wsparcie-zus/`, `/optymalizacja-podatkowa/`,
  `/kontrola-i-postepowanie-podatkowe/`, `/umowy-szyte-na-miare/` **nie dostają z bloga żadnego
  linku w treści**, mimo że mają na blogu dedykowane serie wpisów (fundacja rodzinna, GAAR/kontrola,
  kryptowaluty).
- **Linkowanie wpis→wpis jest szczątkowe** — kilkanaście linków w całym korpusie. Serie tematyczne
  (trzy wpisy o GAAR, trzy o przekwalifikowaniu B2B) nie linkują się nawzajem.
- W treści siedzą **stare i uszkodzone adresy**: część linków wskazuje na `www.mentzen.pl` na
  strukturę sprzed przeniesienia bloga (`https://www.mentzen.pl/ulga-ip-box-dla-programistow/`,
  `https://www.mentzen.pl/roczne-zeznanie-podatkowe-przedsiebiorcy/`), są też adresy ze spacją lub
  łamaniem wiersza w środku (`/dzialalnosc- nierejestrowana-...`, `/konsekwencje-przekwalifikowania-umowy-b2b-\nna-umowe-o-prace/`)
  i literówka w segmencie kategorii (`/blog/doardztwo-prawne/...`).
- Jest też link wychodzący do serwisu siostrzanego: `https://szkolenia.mentzen.pl/`.

Linkowanie **szablonowe** (poza treścią) wygląda tak:

- Sekcja **„Zobacz również / Teksty, które musisz przeczytać!”** pod każdym wpisem — ale to
  **stała, ręcznie wybrana lista czterech tych samych wpisów na całym blogu**, identyczna pod
  artykułem o prop tradingu i pod artykułem o opiece na dziecko:
  `/blog/doradztwo-prawne/wystapienie-wspolnika-ze-spolki-cywilnej/`,
  `/blog/doradztwo-podatkowe/optymalizacja-podatkowa/sprzedaz-samochodu-wykupionego-z-leasingu/`,
  `/blog/doradztwo-podatkowe/optymalizacja-podatkowa/danina-solidarnosciowa-kogo-dotyczy-i-na-czym-polega/`,
  `/blog/ksiegowosc/doplaty-do-kapitalu-sztuczka-kreatywnego-ksiegowego/`.
  Odpowiada to kategorii „Najczęściej czytane” (4 wpisy). Brak automatycznych powiązanych wpisów
  po kategorii lub tagu.
- Breadcrumbs (`BreadcrumbList` w schema) — ale tylko dwupoziomowe: „Strona główna → Kategoria”,
  bez ogniwa „Blog”.
- Menu główne linkuje z bloga do wszystkich stron usługowych — to jedyny stały kanał link equity
  z bloga do oferty i jest identyczny na całej witrynie.

## 7. Prezentacja i nawigacja bloga

- `https://mentzen.pl/blog/` — H1 „Blog”, H2 „Wiedza podatkowa na wyciągnięcie ręki”,
  title „Wiedza podatkowa na wyciągnięcie ręki — Blog Kancelarii Mentzen”, meta description
  „Doradztwo prawne i podatkowe, ulgi podatkowe dla firm, ciekawe artykuły księgowe. Bądź na
  bieżąco z praktyczną wiedzą prawno-podatkową!”
- **Lista wpisów na `/blog/` renderuje się po stronie klienta** — w HTML serwera nie ma ani jednego
  linku do wpisu ani do kategorii. Jest wyszukiwarka, nie ma widocznych filtrów kategorii ani
  przycisku „załaduj więcej”.
- `https://mentzen.pl/blog/page/2/` zwraca 200, ale ma `canonical` ustawiony na
  `https://mentzen.pl/blog/`.
- Każdy wpis ma obrazek wyróżniający (własna grafika 1672×941 px, nazwy plików opisowe, np.
  `opodatkowanie_prop_tradingu_blog.jpg`) i przyciski udostępniania w social media.
- Ikony społecznościowe w stopce artykułu prowadzą m.in. na https://www.youtube.com/@kancelariamentzen —
  blog jest częścią szerszego ekosystemu treści (YouTube, szkolenia).

## 8. Wnioski pod przyszły skill SEO

Co blog robi dobrze i warto zachować jako wzorzec:

- H2 jako pytania klienta — gotowy materiał pod featured snippets i AI Overviews.
- Cytowanie sygnatur interpretacji KIS, artykułów ustaw i wyroków NSA — mocne, weryfikowalne źródła.
- Box autora ze stanowiskiem i osobistym CTA do rezerwacji terminu u konkretnego eksperta.
- Konsekwentne, sprzedażowe meta description z wezwaniem do działania.
- Ścisłe trzymanie się profilu B2B, bez rozmycia tematycznego.

Gdzie jest największy niewykorzystany potencjał (to są kierunki dla skilla, nie audyt techniczny):

1. **Linkowanie kontekstowe blog → strony usługowe.** 523 wpisy, a landingi usługowe dostają
   z treści niemal zero linków. Jest gotowe mapowanie kategoria → usługa
   (`fundacja-rodzinna`, `cit-estonski`, `vat`, `kryptowaluty`, `optymalizacja-podatkowa`,
   `kontrola-i-postepowanie-podatkowe`), którego nikt nie użył.
2. **Klastry tematyczne.** Istnieją naturalne serie (GAAR, przekwalifikowanie B2B, fundacja
   rodzinna, CIT estoński, KSeF), które nie są ze sobą połączone ani nie mają strony filarowej.
3. **Długość.** Mediana 479 słów w 2026 r. przy tematach, w których konkurencja publikuje
   kompleksowe poradniki.
4. **FAQ i dane strukturalne.** Tylko 3 wpisy mają FAQ, żaden nie ma `FAQPage`.
5. **Aktualizacja evergreenów.** Najdłuższe i najbardziej „poradnikowe” teksty pochodzą z 2021–2022
   i zawierają nieaktualne liczby.
6. **Uzupełnienie `Person.description` i `sameAs`** dla autorów aktywnych obecnie oraz podlinkowanie
   boxu autora do archiwum i profilu w zespole.
7. **Powiązane wpisy** — zastąpienie stałej listy czterech linków rekomendacjami po kategorii/tagu.
8. **Puste kategorie** (9 sztuk) i 1547 tagów w większości jednorazowych — porządek taksonomii.
