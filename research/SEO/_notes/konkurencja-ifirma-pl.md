# ifirma.pl — analiza SEO jako benchmark dla mentzen.pl

Data badania: 2026-08-28
Metoda: WebSearch + WebFetch + analiza źródła HTML (curl) — sitemapy, JSON-LD, mikrodane, nagłówki HTTP.

## 1. Kim jest ten konkurent

IFIRMA SA (spółka notowana na GPW, siedziba Wrocław, ul. Grabiszyńska 241G) — model **software + usługa**:
aplikacja do księgowości/faktur oraz biuro rachunkowe z dedykowaną księgową. Pakiet dla spółki z o.o.
od **650 zł/mies.**, JDG od **149 zł/mies.**

To nie kancelaria. Nie ma doradców podatkowych na pierwszym planie, nie ma opinii prawnych.
A mimo to zajmuje pozycje na frazach doradczych z podatków spółek, w tym „estoński CIT — na czym polega,
ile wynosi oraz komu się opłaca".

## 2. Skala — punkt odniesienia

Z sitemap (`https://www.ifirma.pl/sitemap_index.xml`, Yoast):

| Zasób | Liczba |
|---|---|
| `post-sitemap` 1–6 (wpisy blogowe) | ~4 700 URL-i (5×911 + 141) |
| `page-sitemap` (strony ofertowe/statyczne) | 165 |
| `author-sitemap` (autorzy) | **47** |
| `qsm_quiz-sitemap` | osobna sitemapa quizów |

Blog wg licznika kategorii: Prowadzenie firmy 1 909, Aktualności 1 815, Podatki 1 493, Prawo 727,
Marketing 220, E-commerce 146, plus KSeF, Spółki, Rejestracja firmy, Ekonomia, Dla pracowników,
Działalność nierejestrowana.

**To jest przewaga wolumenowa budowana od 2001 r.** Mentzen nie dogoni jej ilością i nie powinien próbować.

## 3. Architektura treści

### 3.1 Trzy warstwy, wyraźnie rozdzielone

1. **Warstwa hub / kompendium** — strony w katalogu głównym (nie na blogu), np.
   `/jednoosobowa-dzialalnosc-gospodarcza-kompendium/`. Około 6 500–7 500 słów, spis treści z kotwicami
   (`#co-to-jest-jdg`, `#forma-opodatkowania-jdg`), 20+ linków do artykułów szczegółowych, pogrupowanych
   w bloki (ulgi ZUS, odpowiedzialność, podatki, sprawy praktyczne) plus grid „Więcej o JDG" z miniaturami.
   Ta strona ma zatrzymać użytkownika i rozdzielić link equity w dół.
2. **Warstwa blogowa** — `~/blog/<kategoria>/<slug>/`, ale nowsze wpisy trafiają na płaski `/blog/<slug>/`.
3. **Warstwa ofertowa** — `/ksiegowosc-dla-<segment>/`.

### 3.2 Klastry branżowe — najmocniejszy element architektury

Osobna strona ofertowa na segment klienta, każda z własnym URL-em i H1:

```
/ksiegowosc-dla-spolek/          /ksiegowosc-dla-programisty/
/ksiegowosc-dla-e-commerce/      /ksiegowosc-dla-startupow-i-it/
/ksiegowosc-dla-lekarzy/         /ksiegowosc-dla-freelancerow/
/ksiegowosc-dla-branzy-beauty/   /ksiegowosc-dla-rekruterow-i-hr/
/ksiegowosc-dla-sektora-zdrowia/ /ksiegowosc-dla-sportowcow-i-trenerow/
/ksiegowosc-dla-prawnikow-i-radcow-prawnych/
```

Uwaga: **`/ksiegowosc-dla-prawnikow-i-radcow-prawnych/`** — ifirma sprzedaje księgowość kancelariom.
To bezpośrednio dotyka rynku, w którym Mentzen działa.

Osobno pillary transakcyjne poza blogiem: `/spolka-z-o-o-rejestracja-spolki-krok-po-kroku/`,
`/jak-zalozyc-spolke-z-o-o-online-przez-s24/`, `/rejestracja-firmy/`, `/biuro-rachunkowe/`,
`/ksiegowosc-internetowa/`, `/cennik/pelny-cennik-uslug-biura-rachunkowego/`.

### 3.3 Czego NIE ma — słaby punkt

Kategoria `/blog/spolki/` (~50 artykułów) **nie ma artykułu przewodniego**. Jest opis SEO na górze
kategorii, ale artykuły leżą płasko, bez centralnego pillara „Spółka z o.o. — kompletny przewodnik"
i bez wewnętrznego linkowania cluster→pillar. Model kompendium zastosowano do JDG, nie do spółek.
**Tu jest luka do przejęcia.**

## 4. Format topowego artykułu — co robią dobrze

### 4.1 Kluczowe odkrycie: konsolidacja przez 301 na wersję rocznikową

Badany URL:
`https://www.ifirma.pl/blog/aktualnosci/estonski-cit-na-czym-polega-ile-wynosi-oraz-komu-sie-oplaca/`

zwraca **HTTP 301** →
`https://www.ifirma.pl/blog/cit-estonski-w-spolce-z-o-o-w-2026-warunki-limity-stawki-przeksztalcenie-jdg-w-spolke/`

Czyli: stary, wypozycjonowany evergreen został **przepisany, rozbudowany, przeniesiony pod nowy slug
ze stemplem roku i przekierowany 301**. Historia linków i sygnałów zostaje, treść jest świeża, snippet
w SERP mówi „2026". Stary URL nadal rankuje w wynikach pod starym tytułem, ale prowadzi do nowego contentu.

Dane z JSON-LD nowej wersji:

```json
"headline": "CIT estoński w spółce z o.o. w 2026 – warunki, limity, stawki 9% i 19%, 10% i 20% oraz jak działa po przekształceniu JDG?",
"datePublished": "2026-06-02T08:00:26+00:00",
"dateModified":  "2026-06-03T07:12:49+00:00",
"wordCount": 4865,
"articleSection": ["Blog", "Księgowość bliżej"]
```

### 4.2 Struktura artykułu (4 865 słów, deklarowany czas czytania 20 min)

- Interaktywny spis treści, 15 punktów z kotwicami
- H2/H3 zbudowane na intencjach użytkownika, nie na strukturze ustawy:
  „Polski CIT klasyczny a CIT estoński" → „Stawki CIT w 2026 – 9%, 19%, 10% i 20%" →
  „CIT estoński warunki" (WARUNEK NR 1–4) → „Jak przejść na estoński CIT?" → „Korekta wstępna" →
  „Przekształcenie z JDG w spółkę z o.o." → „Dochód z tytułu ukrytych zysków" →
  „Utrata prawa do estońskiego CIT" → „Estoński CIT pod lupą fiskusa" → „Estoński CIT: zmiany 2027?"
- **Tabele porównawcze** (klasyczny CIT vs. estoński, mały vs. duży podatnik) z policzonymi kwotami
- **Przykłady 1–5** z liczbami — nie opis przepisu, tylko wyliczenie
- **Podstawy prawne** wypunktowane: art. 28m ustawy o CIT, Ordynacja podatkowa,
  objaśnienia podatkowe MF (link do eureka.mf.gov.pl), wyroki WSA Wrocław i WSA Łódź
- **Sekcja przyszłościowa** „zmiany 2027" — łapie ruch na frazach wyprzedzających
- **FAQ na końcu**, 4 pytania w akordeonie

To jest treść na poziomie merytorycznym, w którym Mentzen jest lepszy, ale **w formacie, którego
Mentzen prawdopodobnie nie stosuje**. Kluczowa różnica: ifirma odpowiada na pytanie „ile mnie to
będzie kosztować" liczbą w tabeli, nie akapitem o wykładni.

### 4.3 Konkurencyjne pokrycie tematu CIT

Widoczne w SERP równolegle: artykuł o CIT estońskim, „Podatek CIT 2025. Jak go obliczyć i kto musi
go płacić?", „Zmiany podatkowe 2027: PIT, CIT, ryczałt". Trzy różne intencje, trzy URL-e, brak kanibalizacji.

## 5. E-E-A-T — jak to wygląda naprawdę

### Co jest

- **47 autorów** z osobnymi stronami `/author/<slug>/` w sitemapie
- `Person` w JSON-LD z `@id`, avatarem, `description` i `url` do strony autora
- `<meta name="author">` w head
- Bio na stronie autora, np. Dorota Łesak:
  > „Księgowa i autorka tekstów. Jako księgowa w ifirma.pl każdego dnia zapewnia fachowe wsparcie
  > swoim klientom – małym firmom usługowym i handlowym. Pomiędzy codziennymi obowiązkami dzieli się
  > na blogu ifirma.pl swoim wieloletnim doświadczeniem i wiedzą dotyczącą tematów księgowo-podatkowych."
- Strona autora z pełną listą publikacji (27 stron paginacji dla jednej osoby)
- Autorytet organizacyjny: spółka na GPW, „ponad 250 ekspertów w zespole", „900+ opinii z Google i Trustpilot"

### Czego NIE ma — i to jest przestrzeń dla Mentzena

- **Brak recenzji merytorycznej.** Nigdzie nie ma „zweryfikowane przez doradcę podatkowego",
  numeru wpisu na listę, tytułu zawodowego. Autorka jest „księgową i autorką tekstów", nie doradcą
  podatkowym ani radcą prawnym.
- **Brak zdjęcia i linków do LinkedIn na stronie autora** (avatar jest tylko w schemacie)
- **Brak `reviewedBy` / `sameAs` dla osób** w danych strukturalnych
- Bio jest marketingowe, nie kwalifikacyjne — nie mówi o wykształceniu, uprawnieniach, latach praktyki
- Zdjęcie wyróżniające do artykułu o CIT estońskim to stock:
  `hr-manager-reading-employee-candidates-resumes-pil-2026-01-07-02-09-07-utc.jpg`, a `name` obrazka
  w schemacie to „Opinie IFIRMA" — czyli niedopilnowany recykling. Autorytet wizualny zerowy.

**Wniosek: ifirma wygrywa strukturą i wolumenem, nie autorytetem osobowym.** Google akceptuje to,
bo autorytet organizacyjny (marka, wiek domeny, opinie) nadrabia braki na poziomie autora.

## 6. Dane strukturalne — pełny obraz

Źródło: JSON-LD generowany przez Yoast SEO (`@graph`), jeden blok na stronę.

Na artykule blogowym:

| Typ | Uwagi |
|---|---|
| `Article` | `headline`, `datePublished`, `dateModified`, `wordCount`, `articleSection`, `inLanguage: pl-PL`, `thumbnailUrl`, `potentialAction: CommentAction` |
| `WebPage` | `description`, `primaryImageOfPage`, `isPartOf` → WebSite, `potentialAction: ReadAction` |
| `ImageObject` | wymiary 2048×1365 |
| `WebSite` | `potentialAction: SearchAction` z `urlTemplate` — sitelinks searchbox |
| `Organization` | logo 1200×412, `sameAs`: Facebook, X, YouTube |
| `Person` | autor z `@id`, avatarem, bio, URL |

**FAQ w mikrodanych, nie w JSON-LD.** To ciekawe — FAQ jest oznaczone atrybutami HTML:

```html
<div class="py-5 py-10" itemscope itemtype="https://schema.org/FAQPage">
  <li itemscope itemprop="mainEntity" itemtype="https://schema.org/Question">
    <h4 itemprop="name">Kiedy podatek w estońskim CIT?</h4>
```

Liczniki na stronie: `Question` ×4, `Answer` ×4, `FAQPage` ×1, `SiteNavigationElement` ×4.
Czyli FAQ jest wpisane w szablon akordeonu, a nie w plugin SEO — świadoma decyzja implementacyjna,
która sprawia, że schema nie może się rozjechać z widoczną treścią.

Meta w `<head>`:

```html
<meta name='robots' content='index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1'>
<link rel="canonical" href="...">
og:locale, og:type=article, og:title, og:description, og:url, og:site_name,
og:image + width/height/type, article:publisher, article:published_time, article:modified_time
twitter:card=summary_large_image, twitter:creator=@ifirmapl, twitter:site=@ifirmapl
```

`max-snippet:-1` i `max-image-preview:large` — agresywne otwarcie na duże snippety i AI Overviews.

### Braki w danych strukturalnych

- **Brak `BreadcrumbList`** — mimo że nawigacja okruszkowa jest widoczna. Traci ładniejszy wynik w SERP.
- **Brak `hreflang`** (jedna wersja językowa, więc OK)
- **Brak `LocalBusiness` / `AccountingService`** na stronach lokalnych (patrz sekcja 8)
- **Brak `Product` / `Offer` / `AggregateRating`** na stronach ofertowych mimo 900+ opinii i podanej ceny
  — realna, niewykorzystana szansa u nich

### Selektywność odświeżania

Porównanie dwóch artykułów o CIT:

| | CIT estoński 2026 | Podatek CIT 2025 |
|---|---|---|
| `wordCount` | 4 865 | 1 928 |
| `dateModified` | jest (2026-06-03) | **brak** |
| FAQPage | tak | nie |
| tabele/przykłady | rozbudowane | podstawowe |

Czyli ifirma **nie odświeża wszystkiego**. Inwestuje w te artykuły, które siedzą blisko pieniędzy
(spółka z o.o. = pakiet 650 zł/mies.), a resztę zostawia jako masę długiego ogona.

## 7. Powiązanie treści z konwersją

### Na artykule

- CTA „Załóż konto w aplikacji ifirma.pl" powtarzane wielokrotnie: pod wstępem, w treści, w stopce
- Floating CTA „Napisz do nas lub zadzwoń +48 735 209 003" z linkiem WhatsApp
- Formularz „Zleć nam księgowość!" wstawiony **w środku** kompendium, nie na końcu
- Linkowanie z treści do stron ofertowych — z artykułu o CIT (dane z HTML):
  `/biuro-rachunkowe/` ×3, `/rejestracja-firmy/` ×3, `/ksiegowosc-internetowa/` ×2,
  `/program-do-faktur/` ×2, `/cennik/` ×2, `/spolka-z-o-o-rejestracja-spolki-krok-po-kroku/` ×2,
  `/ksiegowosc-dla-programisty/`, `/ksiegowosc-dla-freelancerow/`, `/app/wa/register` ×3
- Linki wychodzące do źródeł rządowych (eureka.mf.gov.pl) — sygnał wiarygodności bez utraty ruchu
- Sekcja „Może te tematy też Cię zaciekawią" (4 artykuły) + poprzedni/następny wpis

### Na stronie ofertowej `/ksiegowosc-dla-spolek/`

H1: „Pełna księgowość dla spółek z o.o."
Podtytuł: „Ty prowadzisz spółkę - a jej rozliczenia poprowadzi za Ciebie osobista księgowa IFIRMA."

Kolejność sekcji: hero z CTA → karuzela 150+ integracji → trzy filary (KSeF, aplikacja, pełna księgowość)
→ opis usługi → **pakiet od 650 zł/mies. z rozpisaną zawartością** → kontakt → proces 3-etapowy
(Załóż konto → Wystawiaj faktury → Zleć księgowość) → FAQ (4 pytania) → 10 funkcjonalności →
**10 wpisów blogowych + „Zobacz więcej wpisów"** → sekcja rejestracji → stopka.

5 CTA na stronie: „Zleć księgowość" ×2 (jeden do `/app/wa/register?produktId=2`),
„Załóż konto za darmo", „Załóż konto", floating telefon/WhatsApp.

Dowód społeczny: „Znakomita" ocena, „900+ opinii z Google i Trustpilot", „Ponad 250 ekspertów w zespole",
„spółka notowana na GPW". Brak liczby klientów, brak OC, brak certyfikatów.

**Pętla jest zamknięta w obie strony:** artykuł → oferta (linki kontekstowe) i oferta → artykuły
(blok 10 wpisów). Money page zbiera sygnały tematyczne z bloga, blog przekazuje intencję do money page.

### Pytania FAQ na stronie ofertowej są frazami z długiego ogona

„Jak prowadzić księgowość spółki z o.o.?", „Czym jest pełna księgowość dla spółek z o.o.?",
„Kiedy należy przejść na pełną księgowość?", „Jak założyć własną spółkę?" — czyli **strona sprzedażowa
łapie też ruch informacyjny**, bez tworzenia osobnego artykułu.

## 8. Local SEO — robione słabo, warto zrobić lepiej

11 stron lokalnych: Wrocław (×2), Warszawa, Kraków, Poznań, Katowice, Legnica, Głogów, Jelenia Góra, Lubin.

Analiza `/rzetelne-biuro-rachunkowe-w-warszawie/`:

- Nazwa miasta pada **tylko w H1** („Rzetelne biuro rachunkowe w Warszawie") i tytule. Reszta treści
  jest szablonowa, identyczna dla wszystkich miast.
- **Brak `LocalBusiness` / `AccountingService` w schema** — jedyne typy to `WebPage`, `WebSite`,
  `Organization` (wrocławska centrala) + `SiteNavigationElement` w mikrodanych.
- Brak mapy, brak adresu warszawskiego (podany adres to siedziba we Wrocławiu), brak lokalnych opinii.
- Jest osobny numer telefonu (+48 793 885 463) i godziny otwarcia działów.
- Cena „już za 149 zł/mies."
- Bardzo krótka strona (2 nagłówki: H1 + „Sprawdź, jak działamy").

To są cienkie strony doorwayowe, które działają wyłącznie dlatego, że domena jest silna. **Dla kancelarii
z realnymi biurami to jest łatwa przewaga do zbudowania.**

## 9. Techniczne drobiazgi

- WordPress + Yoast SEO, sitemapy Yoastowe
- `robots.txt` minimalny: blokuje `/wp-content/noindex-google/` i paginację opinii `?reviews-page=`
  (dobra higiena — chroni przed thin/duplicate content w paginacji recenzji)
- Kanoniczne URL-e obecne wszędzie
- Płaska struktura URL dla nowych wpisów (`/blog/<slug>/`) zamiast starej z kategorią
- Slug SEO nowego pillara zawiera rok i stawki:
  `cit-estonski-w-spolce-z-o-o-w-2026-warunki-limity-stawki-przeksztalcenie-jdg-w-spolke`
  — długi, ale nasycony wariantami zapytań
- Osobna sitemapa quizów (`qsm_quiz-sitemap.xml`) — quizy jako lead magnet i typ treści

## 10. Dodatkowe formaty treści

Sekcja „Baza wiedzy" w nawigacji głównej agreguje: Blog, E-booki, Wzory dokumentów, Pomoc,
Kompendium tematyczne. E-booki mają własne URL-e (`/ebook/podstawy-scrum/`,
`/ebook/polski-lad-2-0-ebook-...`, `/kluczowe-zmiany-w-2025-roku-ebook/`) — czyli lead magnet
jest jednocześnie stroną indeksowaną. Do tego kanał YouTube i seria „Księgowość bliżej"
(widoczna jako `articleSection` w schemacie — content brand wewnątrz bloga).

---

# Wnioski dla mentzen.pl

## Co zaadaptować

**1. Model kompendium dla spółek — natychmiast.**
ifirma zbudowała pillar dla JDG (`/jednoosobowa-dzialalnosc-gospodarcza-kompendium/`, 7 tys. słów,
kotwice, 20+ linków w dół), ale **nie zbudowała go dla spółek**. Kategoria `/blog/spolki/` jest płaska.
Pillar „Podatki spółki z o.o. — kompletny przewodnik" z klastrem (estoński CIT, ukryte zyski,
dywidenda, przekształcenie JDG→sp. z o.o., składka zdrowotna wspólnika, CIT klasyczny 9/19%)
uderza dokładnie w lukę. To rdzeń merytoryczny Mentzena, więc przewaga jakościowa jest naturalna.

**2. Format artykułu: liczby przed wykładnią.**
To, co daje ifirmie pozycje, to nie prawo — to tabela porównawcza z kwotami i pięć wyliczonych
przykładów. Kancelaria zwykle pisze o wykładni. Trzeba dołożyć warstwę „ile to kosztuje": tabela
klasyczny CIT vs. estoński, próg opłacalności, wyliczenie na konkretnym zysku. Merytoryka Mentzena
plus format kalkulacyjny ifirmy to kombinacja, której na rynku nie ma.

**3. Przepisywanie evergreenów na wersje rocznikowe z 301.**
Ich manewr: stary URL → 301 → nowy slug ze stemplem roku i rozbudowaną treścią. Zachowuje linki,
odświeża snippet. Warto to wpisać w proces redakcyjny: raz w roku top 20 artykułów przechodzi rewizję,
rozbudowę i migrację sluga z 301. Alternatywa lżejsza (i bezpieczniejsza): zostawić slug, aktualizować
treść i `dateModified`, dodawać sekcję „zmiany w <rok+1>".

**4. Sekcja „zmiany 2027?" w każdym pillarze.**
ifirma dokłada blok o planowanych zmianach. Łapie ruch wyprzedzający i daje pretekst do aktualizacji.
Dla kancelarii to naturalne — legislacja to codzienna praca.

**5. FAQ w mikrodanych, wpięte w szablon akordeonu.**
Ich rozwiązanie (`itemscope itemtype="https://schema.org/FAQPage"` na widocznym akordeonie) gwarantuje,
że schema nigdy nie rozjedzie się z treścią. Prostsze i trwalsze niż FAQ generowane przez plugin.

**6. FAQ z frazami long-tail na stronach usługowych, nie tylko na blogu.**
`/ksiegowosc-dla-spolek/` łapie „jak prowadzić księgowość spółki z o.o." bez osobnego artykułu.
Strony usługowe Mentzena powinny mieć taki blok.

**7. Dwustronna pętla blog ↔ oferta.**
Blok „10 wpisów blogowych" na stronie ofertowej + kontekstowe linki z artykułu do usługi
(u nich 3× do `/biuro-rachunkowe/` w jednym artykule). To tanie i mierzalne.

**8. `max-image-preview:large` i `max-snippet:-1`.**
Sprawdzić, czy mentzen.pl ma. Bez tego traci się widoczność w rozszerzonych snippetach i AI Overviews.

**9. Segmentacja ofertowa po branży klienta.**
11 stron `/ksiegowosc-dla-<segment>/`. Odpowiednik dla kancelarii: `/obsluga-prawna-dla-<segment>/`
albo `/doradztwo-podatkowe-dla-<segment>/` (IT, e-commerce, medycyna, nieruchomości, kryptowaluty).
Uwaga: ifirma ma już `/ksiegowosc-dla-prawnikow-i-radcow-prawnych/` — atakuje segment, w którym
Mentzen jest u siebie.

## Gdzie ifirma jest słaba — tam budować przewagę

**1. Zero recenzji merytorycznej.** Autorzy to „księgowe i autorki tekstów", nie doradcy podatkowi.
Mentzen może dołożyć to, czego ifirma nie ma i nie może mieć:
- byline z tytułem zawodowym i numerem wpisu na listę doradców podatkowych / radców prawnych
- „Zweryfikowano merytorycznie: <imię>, doradca podatkowy nr <numer>, <data>"
- `Person` w schemacie z `sameAs` (LinkedIn, profil w izbie), `jobTitle`, `hasCredential`
- `reviewedBy` na artykule
- zdjęcia autorów, realne biogramy z wykształceniem i praktyką
To jest najczystsza przewaga E-E-A-T dostępna kancelarii wobec software house'u.

**2. Brak `BreadcrumbList`.** Mentzen może to mieć od ręki.

**3. Brak `AggregateRating` / `Offer`** mimo 900+ opinii i podanej ceny. Jeśli Mentzen ma opinie,
warto je oznaczyć (ostrożnie i zgodnie z wytycznymi Google — tylko opinie zbierane samodzielnie
o samej firmie, na właściwej encji).

**4. Local SEO robione minimalnie.** 11 stron miastowych ze zmienionym wyłącznie H1, bez `LocalBusiness`,
bez mapy, bez lokalnego adresu. Kancelaria z realnymi oddziałami może zrobić strony lokalne z prawdziwą
treścią, `LocalBusiness`/`LegalService` w schemacie, NAP zgodnym z Profilem Firmy w Google, zespołem
lokalnym i lokalnymi case'ami. Tam ifirma nie ma czym odpowiedzieć.

**5. Stockowe grafiki i niedopilnowana schema obrazków.** Obraz do artykułu o CIT estońskim to zdjęcie
HR-owca z `name: "Opinie IFIRMA"`. Własne diagramy, schematy decyzyjne i tabele w grafice to
jednocześnie sygnał jakości i materiał do Google Images / Discover.

## Czego unikać

**1. Nie ścigać wolumenu.** 4 700 artykułów budowanych od 2001 r. Próba dogonienia da masę cienkich
tekstów, które rozcieńczą autorytet tematyczny. Lepiej 50 pillarów, których nikt nie przebije,
niż 500 przeciętnych.

**2. Nie kopiować szablonowych stron miastowych.** Ich strony lokalne działają tylko dzięki sile domeny.
Dla kancelarii cienkie doorway pages to ryzyko wizerunkowe i algorytmiczne.

**3. Nie rozmywać kategorii.** ifirma ma na blogu podatkowym „Marketing", „Design", „Social media",
„AI w biznesie", „Project Manager w 2026 roku". To rozjeżdża sygnał tematyczny. Mentzen powinien trzymać
wąski, głęboki zakres: podatki, prawo spółek, sukcesja, kontrole i spory.

**4. Nie kopiować agresywności CTA.** 5 CTA na stronie ofertowej plus floating WhatsApp plus powtarzane
„Załóż konto" w treści artykułu pasuje do produktu SaaS za 149 zł. Dla usługi doradczej z wyższą wartością
kontraktu i wyższym progiem zaufania to obniża postrzeganą jakość. Właściwe CTA dla kancelarii:
konsultacja, wycena, kontakt z konkretnym doradcą.

**5. Nie zostawiać starych artykułów bez `dateModified`.** ifirma ma z tym problem
(„Podatek CIT 2025" bez daty modyfikacji, 1 928 słów). W tematach podatkowych nieaktualna treść bez
widocznej daty rewizji to ryzyko dla E-E-A-T większe dla kancelarii niż dla software house'u — od
kancelarii oczekuje się aktualności.

## Priorytet działań (propozycja)

1. Pillar „Podatki spółki z o.o." + klaster (luka u konkurenta, rdzeń kompetencji Mentzena)
2. Warstwa E-E-A-T: byline z uprawnieniami, recenzja merytoryczna, `Person`+`reviewedBy` w schemacie
3. Przebudowa formatu top artykułów: tabele, wyliczenia, progi opłacalności, FAQ w mikrodanych
4. `BreadcrumbList` + audyt meta robots (`max-snippet`, `max-image-preview`)
5. Strony lokalne z prawdziwą treścią i `LegalService`/`LocalBusiness` (jeśli są oddziały)
6. Dwustronne linkowanie blog ↔ strony usługowe + FAQ long-tail na stronach usługowych
7. Roczny cykl rewizji top 20 artykułów z aktualizacją `dateModified`

---

## Źródła

- https://www.ifirma.pl/blog/
- https://www.ifirma.pl/blog/cit-estonski-w-spolce-z-o-o-w-2026-warunki-limity-stawki-przeksztalcenie-jdg-w-spolke/ (cel 301 ze starego URL-a)
- https://www.ifirma.pl/blog/aktualnosci/estonski-cit-na-czym-polega-ile-wynosi-oraz-komu-sie-oplaca/ (301)
- https://www.ifirma.pl/blog/podatek-cit-czym-jest-jak-go-obliczyc-i-kto-musi-go-placic-kluczowe-informacje-2025/
- https://www.ifirma.pl/blog/spolki/
- https://www.ifirma.pl/ksiegowosc-dla-spolek/
- https://www.ifirma.pl/jednoosobowa-dzialalnosc-gospodarcza-kompendium/
- https://www.ifirma.pl/author/dorota-lesak/
- https://www.ifirma.pl/rzetelne-biuro-rachunkowe-w-warszawie/
- https://www.ifirma.pl/sitemap_index.xml, /post-sitemap*.xml, /page-sitemap.xml, /author-sitemap.xml
- https://www.ifirma.pl/robots.txt
