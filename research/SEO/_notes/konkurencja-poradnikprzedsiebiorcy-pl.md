# poradnikprzedsiebiorcy.pl — analiza konkurencji SEO

Data analizy: 2026-08-28
Cel: benchmark dla mentzen.pl w niszy podatkowo-prawnej (JDG, spółki, ZUS).
Metoda: WebFetch + surowy HTML (curl) + sitemapy. Bez dostępu do Ahrefs/Senuto — wnioski
opisują to, co widać w kodzie i strukturze serwisu, nie realne wolumeny ruchu.

## 1. Kim jest ten konkurent

- Content marketing operatora SaaS **wFirma.pl** (księgowość online). Portal nie jest
  wydawnictwem — jest lejkiem do produktu. To zmienia ekonomikę: mogą utrzymywać
  redakcję, bo artykuł nie musi zarabiać na reklamie, tylko przyprowadzić rejestrację.
- Siostrzany serwis: **poradnikpracownika.pl** (ta sama grupa, druga persona — pracownik
  zamiast przedsiębiorcy). Klasyczne rozbicie intencji na dwie domeny.
- Skala treści z sitemap indeksu (`/sitemap.xml`, 25 plików):
  - 13 × 2000 + 531 = **~26,5 tys. artykułów**
  - 72 kategorie, 20 kalkulatorów, **78 stron autorów**, 3 „main articles"
- `robots.txt`: `Allow: /` dla wszystkiego; blokują tylko `/categories/search` dla
  bingbot, mj12bot, **SemrushBot** i **PerplexityBot**. Czyli: świadomie utrudniają
  konkurencji analizę i nie chcą karmić Perplexity — ale Google i GPTBot mają pełny wstęp.

## 2. Architektura treści i klastry

### Taksonomia — 6 pionów, każdy z 5–6 podkategoriami

| Pion | URL | Podkategorie |
|---|---|---|
| Biznes | `/biznes` | Zarządzanie, Cyberbezpieczeństwo, Dotacje, Prowadzenie biznesu, Pomysł na biznes, HR w praktyce, Marketing internetowy, E-commerce, Finansowanie, Rozwój osobisty |
| Księgowość | `/ksiegowosc` | Co się zmienia, Księgowość firm, Środki trwałe, Kasy rejestrujące, Sprawozdawczość, Ewidencje |
| Podatki | `/podatki` | KSeF, Podatki dochodowe, VAT, Samochód w firmie, Transakcje zagraniczne, Ordynacja podatkowa |
| Prawo | `/prawo` | Prawo gospodarcze, Prawa konsumenta, Ochrona danych, Prawo spółek, Prawa autorskie, Windykacja |
| Kadry | `/kadry` | Prawo pracy, Ubezpieczenie ZUS, Urlopy, Obowiązki pracownika, BHP, Dla pracownika |
| Biura rachunkowe | `/biura-rachunkowe` | Prowadzenie, Prawa i obowiązki, Pozyskiwanie klientów, Nowe technologie |

Osobny pion **Biura rachunkowe** to sprzedaż B2B do biur (wFirma ma wersję dla biur) —
segment, który nie jest „poradnikiem dla przedsiębiorcy", tylko akwizycją partnerów.

### Kluczowa obserwacja: to NIE jest klasyczny model pillar/cluster

Sprawdziłem `/podatki` w surowym HTML:

- `<title>` = „Serwis Podatkowy", `meta description` = „Serwis Podatkowy - Poradnik
  Przedsiębiorcy" — **generyczne, niezoptymalizowane**
- H1 = „Serwis Podatkowy", potem 15× H2, ale każde H2 to tytuł artykułu z feedu
- **zero tekstu wprowadzającego, zero definicji, zero linkowania do artykułów filarowych**
- paginacja do `/podatki/409` → ~409 stron listingu

Czyli strony kategorii są **czystymi feedami chronologicznymi**, nie hubami. Rolę filara
pełnią u nich dwa inne typy stron:

1. **Gigantyczne artykuły evergreen** aktualizowane co roku (patrz sekcja 3)
2. **Strony-huby przy lead magnetach** — np. `/-formy-opodatkowania`:
   - `<title>`: „Formy opodatkowania - darmowy ebook PDF bez rejestracji"
   - H1: „Formy opodatkowania - Kompendium", `datePublished` = `dateModified` = 2023-02-01
   - pod treścią ebooka **~20 nagłówków H3 = dynamiczna lista artykułów klastra**
     („Ryczałt od przychodów ewidencjonowanych - stawki 2026", „Zmiana formy
     opodatkowania - jak jej dokonać?", „Podatek liniowy - wady i zalety"…)
   - Efekt: strona z 2023 r. cały czas dostaje świeże linki wychodzące do nowych
     artykułów. Hub tematyczny + lead magnet + PDF „bez rejestracji" w tytule (świadome
     przechwytywanie frazy „darmowy ebook bez rejestracji").

**Wniosek architektoniczny:** ich siła nie leży w eleganckiej strukturze, tylko w
brutalnej **głębokości pokrycia tematu**. Na jedną frazę-parasol („składka zdrowotna")
mają kilkanaście osobnych URL-i pokrywających każdy wariant intencji:

- `/-nowy-polski-lad-skladka-zdrowotna-uzalezniona-od-dochodu-i-bez-odliczenia` (główny)
- `/-wyliczenie-skladki-zdrowotnej-u-ryczaltowca` (per forma opodatkowania)
- `/-skladka-zdrowotna-w-kosztach` (per konsekwencja podatkowa)
- `/-roczne-rozliczenie-skladki-zdrowotnej` + `/-roczna-skladka-zdrowotna` (per moment)
- `/-rozpoczecie-dzialalnosci-od-stycznia-a-roczne-rozliczenie-skladki-zdrowotnej` (edge case)
- `/kalkulator-skladki-zdrowotnej` (intencja narzędziowa)

To jest odpowiedź na „dlaczego zajmują dwa miejsca naraz" — nie sitelinki, tylko dwa
osobne, wystarczająco różne URL-e na tę samą frazę.

### URL-e

Wszystkie artykuły siedzą **płasko w roocie z prefiksem myślnika**: `/-slug`. Zero
katalogów kategorii w ścieżce. Zalety: brak problemu przy zmianie kategorii, maksymalna
płaskość (1 klik od roota). Wada: URL nie niesie kontekstu tematycznego. To relikt
techniczny, nie best practice — nie kopiować.

## 3. Format topowego artykułu (rozbiór 2 stron)

### A. `/-wyliczenie-skladki-zdrowotnej-u-ryczaltowca` („składka zdrowotna ryczałt 2026")

- **~3 580 słów** w obrębie treści, 300 KB HTML
- **Rozjazd title / H1 / H2 — celowy i wart skopiowania:**
  - `<title>`: „Składka zdrowotna ryczałt 2026 - jak ją wyliczyć?" ← *dokładna fraza + rok*
  - `H1`: „Jak wygląda wyliczenie składki zdrowotnej u ryczałtowca w 2026 roku?" ← *long tail, forma pytania*
  - pierwsze `H2`: „Składka zdrowotna ryczałt 2026 - jak ją wyliczyć?" ← *powtórzenie exact match*
  - `itemprop="headline"` = wersja z title (nie H1)

  Trzy różne sformułowania tej samej intencji → trzy szanse na dopasowanie.
- **Spis treści „Na skróty:"** — linki kotwiczące z czytelnymi slugami
  (`#0-Co-wyjasniamy-w-ebooku-...`). Zbiera potencjalne sitelinki w SERP.
- **Nagłówki H2 to gotowe pytania z People Also Ask**, m.in.:
  - „Przeciętne wynagrodzenie ma wpływ na wysokość składki ryczałtowca"
  - „Podstawa składki zdrowotnej ryczałt 2026" / „…ryczałt 2025" ← *rok N i N-1 obok siebie*
  - „Które z wyliczeń składki zdrowotnej jest bardziej korzystne?" ← *intencja decyzyjna*
  - „Obowiązek przekazywania ZUS DRA" ← *krok proceduralny*
- **Sekcja „Najczęstsze pytania"** = 3–4 H3 z pytaniami, opakowana w `FAQPage`
- **3 tabele** (progi, stawki, podstawy), **numerowane „Przykład 1–4"** z wyliczeniami na
  konkretnych kwotach
- **242 unikalne linki wewnętrzne** na stronie (347 wystąpień) — masywne, ale to głównie
  moduły szablonowe („Podobne", „Zobacz także", „Najchętniej czytane", 30 × `box-promo-links`)
- **Linki zewnętrzne: tylko 6 × wfirma.pl + 1 × poradnikpracownika.pl.** Zero linków do
  ISAP, sejm.gov.pl, ZUS, KIS. **Zero sekcji „Podstawa prawna"** — w całym artykule
  0 wystąpień „ustawy z dnia" i 0 wystąpień „art. \d". To ich największa dziura E-E-A-T.

### B. `/kalkulator-skladki-zdrowotnej` — hybryda narzędzie + artykuł

- **4 639 słów** obudowy tekstowej wokół działającego kalkulatora
- 9 × H2 rozbite **per forma opodatkowania** (skala / liniowy / ryczałt / karta)
- Potem **12 × H3 per miesiąc**: „Kalkulator składki zdrowotnej za styczeń…", „…za luty…"
  — jawne pokrycie sezonowych long tailów wewnątrz jednej strony
- Oznaczony jako `schema.org/Article`, **nie** `SoftwareApplication`/`WebApplication`
- `<title>`: „Kalkulator składki zdrowotnej 2025/2024…" — **nieaktualny w sierpniu 2026**

To potwierdza wzorzec „długi poradnik + kalkulator": kalkulator nie jest osobnym,
lekkim narzędziem — jest pretekstem do kolejnej strony 4,5 tys. słów.

## 4. Sygnały E-E-A-T — słabiej, niż sugeruje pozycja w SERP

**Co mają:**
- Podpis autora przy artykule, opakowany w `schema.org/Person` z `itemprop="name"`
- Zdjęcie autora (`/images/fx/crop,150,150/247786`), etykieta „Nasz ekspert:"
- Link „Artykuły autora" → `/autor/dorociak-katarzyna`
- 78 stron autorów w sitemapie (indeksowane, więc traktują je jako aktywa SEO)
- Widoczna data (`2026-01-26`), sekcja „Zdaniem eksperta" w każdym pionie
- Program `/dolacz-do-ekspertow` — rekrutacja ekspertów przez formularz

**Czego brakuje — i to jest luka do wykorzystania:**
- **Strona autora nie ma bio.** Sprawdziłem `/autor/dorociak-katarzyna`: sam listing
  artykułów + paginacja. Zero opisu doświadczenia, zero tytułu zawodowego, zero
  uprawnień (doradca podatkowy nr X / radca prawny), zero LinkedIna, **nawet bez zdjęcia**
  na stronie autora (zdjęcie jest tylko w artykule).
- `datePublished` **równa się** `dateModified` (2026-01-26 = 2026-01-26). Nadpisują datę
  publikacji przy aktualizacji zamiast pokazać obie. Traci się sygnał „artykuł z 2021
  utrzymywany od 5 lat" i nie da się zweryfikować historii zmian.
- **Brak recenzji merytorycznej** — nigdzie `reviewedBy`, „zweryfikowane przez",
  „recenzja: doradca podatkowy".
- **Brak cytowań źródeł pierwotnych.** Zero linków do ustaw, interpretacji KIS, orzeczeń.
- `/dolacz-do-ekspertow` nie ujawnia żadnych wymogów redakcyjnych ani procesu weryfikacji
  — formularz zbiera imię, mail, telefon i dziedzinę. Wygląda na rekrutację autorów, nie
  na giełdę guest postów, ale przejrzystości zero.

**Ślady problemów z jakością przy tej skali:**
- W artykule „…2026" wisi nagłówek H3 **„Kto nie płaci składki zdrowotnej w 2024?"** —
  aktualizacja objęła tytuł i część treści, nie całość. Klasyczny dług refreshu.
- Ten sam artykuł miał w treści przypis „ok. 3000+ słów" i sekcje z rokiem 2025 obok 2026.
- `<title>` kalkulatora zatrzymany na „2025/2024".
- Autorka podpisana pod artykułem o składce zdrowotnej ryczałtowca ma na swojej stronie
  autora niemal wyłącznie teksty kadrowe (urlopy, chorobowe, umowy o pracę) — podpis
  wygląda na przypisany, nie na specjalizację.

## 5. Dane strukturalne — mikrodane, nie JSON-LD

W surowym HTML `/-wyliczenie-skladki-zdrowotnej-u-ryczaltowca`:

```
0  × application/ld+json          ← ZERO JSON-LD
1  × itemtype schema.org/Article
1  × schema.org/FAQPage
2  × schema.org/Question + 2 × schema.org/Answer
1  × schema.org/BreadcrumbList + 3 × ListItem
1  × schema.org/Person   (author)
1  × schema.org/Organization (publisher) + logo ImageObject
1  × schema.org/WebPage
```

Użyte `itemprop`: `headline`, `datePublished`, `dateModified`, `author`, `publisher`,
`image`, `mainEntityOfPage`, `mainEntity`, `acceptedAnswer`, `position`, `item`.

Uwagi:
- **Wszystko na mikrodanych (`itemscope`/`itemprop`) wplecionych w markup.** Google to
  czyta, ale jest to trudniejsze w utrzymaniu i łatwiej się rozjeżdża przy zmianie
  szablonu. Google od lat rekomenduje JSON-LD.
- `BreadcrumbList` ma tylko 3 elementy: Serwis Kadrowy → Ubezpieczenie ZUS → artykuł.
  Czyli okruszki odtwarzają kategorię, mimo że URL jest płaski.
- **Brak `HowTo`** (wycofane przez Google), **brak `SoftwareApplication` na kalkulatorach**,
  brak `speakable`, brak `about`/`mentions` z encjami.
- `Person` ma **wyłącznie `name`** — bez `jobTitle`, `url`, `sameAs`, `knowsAbout`,
  `hasCredential`. Dla Google to autor bez tożsamości w grafie wiedzy.
- Meta: pełen zestaw `og:` + `twitter:` (w tym `og:image` 1200×628 z generatora
  `/images/fx/max,1200,628/…`), `canonical` self-referencyjny, `og:locale` = `pl_PL`.
- **Brak `<lastmod>` w sitemapach** — nie dają Google sygnału o świeżości przez sitemapę.
- Brak znacznika `<article>` w DOM (semantyka HTML5 nieużywana).

## 6. Konwersja — jak zszywają treść z produktem

- Na stronie artykułu **14 wystąpień „wfirma"**, w tym 6 linków wychodzących:
  - `wfirma.pl/rejestracja` z anchorem **„Załóż bezpłatne konto"**
  - `wfirma.pl/ksiegowosc-online-dla-biur-rachunkowych`
  - `wfirma.pl/program-kadrowo-placowy`
  - `pomoc.wfirma.pl` (baza wiedzy produktu)
- **CTA kontekstowe wplecione w treść**, nie tylko w sidebarze: „Rozliczaj wygodnie
  składki ZUS" + przycisk, umieszczone dokładnie w miejscu, gdzie tekst tłumaczy ręczne
  wyliczenie. Klasyczne „pokaż ból, potem podaj narzędzie".
- Klasy banerów w HTML: `banner-expandable` (×2, rozwijalne), `banner-uneditable
  oprogramowanie-dla-biur`, `banner-list`, `banner-collapsible hidden-xs hidden-sm`
  (ukryty na mobile — nie kanibalizują CWV/UX na telefonie).
- **30 × `box-promo-links`** — moduły linkowania wewnętrznego traktowane jako element
  promocyjny.
- **Newsletter: 34 wystąpienia** na jednej stronie. Zapis do newslettera wstawiony
  bezpośrednio pod nagłówkiem artykułu, przed treścią.
- **Lead magnety:** `/biblioteka-ebookow`, ebooki-hubu (`/-formy-opodatkowania`),
  prezentacje PDF do pobrania wewnątrz artykułu, wzory dokumentów `/pobierz`,
  wskaźniki `/wskazniki`, szkolenia `/szkolenia`, `/porady-online`.
- Monetyzacja poboczna: `/reklama` (sprzedaż powierzchni), „Patronaty" konferencji.

**Model lejka:** fraza informacyjna → długi artykuł → w środku moment „to jest
skomplikowane" → CTA do darmowego konta wFirma. Treść nigdy nie prowadzi do rozmowy
z człowiekiem — prowadzi do self-service SaaS.

## 7. Local SEO

**Praktycznie nie istnieje i nie musi.** Brak `LocalBusiness`, brak stron miejskich,
brak NAP w markupie, brak „biuro rachunkowe Kraków" itp. Produkt jest ogólnopolskim
SaaS-em, więc geografia nie ma znaczenia. To istotna różnica względem Mentzena, który
ma fizyczne oddziały — tu benchmark **nie ma zastosowania** i nie ma po co go kopiować.

---

## Wnioski dla mentzen.pl

### Warto zaadaptować

1. **Rozbicie frazy-parasola na siatkę wariantów intencji.** Nie jeden artykuł „składka
   zdrowotna", tylko osobny URL na każdy przekrój: forma opodatkowania × moment roku ×
   status (start/zawieszenie/likwidacja) × forma prawna (JDG / spółka z o.o. / komandytowa).
   To jest mechanizm, który daje im dwa miejsca w TOP 3 naraz. Zacząć od 2–3 fraz, gdzie
   Mentzen ma realną przewagę merytoryczną, nie od całego pionu.

2. **Wzorzec title ≠ H1 ≠ pierwszy H2.** Title = exact match + rok. H1 = to samo w formie
   pytania long tail. Pierwszy H2 = powtórzenie exact match. Zero kosztu wdrożenia,
   działa od razu.

3. **Rok w tytule + świadome utrzymywanie roku N i N-1 w tej samej treści.** Sekcja
   „Podstawa … 2026" obok „Podstawa … 2025" łapie użytkowników rozliczających rok wstecz,
   a jednocześnie sygnalizuje aktualność. **Warunek:** procedura corocznego refreshu musi
   objąć całą treść, nie tylko tytuł (patrz „czego unikać").

4. **Kalkulator jako typ strony, nie jako gadżet.** Ich kalkulator składki zdrowotnej ma
   4,6 tys. słów obudowy. Dla Mentzena naturalne: kalkulator formy opodatkowania,
   kalkulator „JDG vs. spółka z o.o.", kalkulator składki zdrowotnej, kalkulator estońskiego
   CIT. Ale zrobić to **lepiej niż oni**: oznaczyć `SoftwareApplication` w JSON-LD
   (oni tego nie mają) i utrzymywać rok w tytule (u nich zatrzymany na „2025/2024").

5. **Spis treści z kotwicami** („Na skróty") w każdym długim tekście.

6. **Numerowane przykłady z konkretnymi kwotami** („Przykład 1", „Przykład 2") i tabele
   progów. To format, który Google w tej niszy nagradza, a AI Overviews chętnie cytują.

7. **Sekcja „Najczęstsze pytania" z `FAQPage`** na końcu artykułu — z pytaniami zdjętymi
   z PAA, a nie wymyślonymi.

8. **Lead magnet jako hub tematyczny.** Ich `/-formy-opodatkowania` to ebook + dynamiczna
   lista ~20 artykułów klastra. Strona z 2023 wciąż zbiera świeże linki wewnętrzne.
   Mentzen ma naturalny odpowiednik: raport/kompendium + lista powiązanych analiz.
   Dodatkowo „bez rejestracji" w tytule jako fraza — u nich zadziałało.

9. **Podział na dwie persony/domeny lub przynajmniej dwa piony treści.** Oni mają
   przedsiębiorcę i pracownika osobno. Mentzen: przedsiębiorca JDG vs. spółka /
   właściciel vs. inwestor — intencje i słownictwo są różne.

### Gdzie Mentzen ma przewagę i powinien uderzyć

10. **E-E-A-T to ich najsłabszy punkt, a dla Mentzena to naturalne aktywo.** Ich strony
    autorów są puste — sam listing, bez bio, bez uprawnień, bez zdjęcia. Kancelaria
    z doradcami podatkowymi i radcami prawnymi wygrywa to bez wysiłku:
    - pełna strona autora: numer wpisu na listę doradców podatkowych / radców prawnych,
      specjalizacje, publikacje, wystąpienia, LinkedIn
    - `schema.org/Person` z `jobTitle`, `hasCredential`, `sameAs`, `knowsAbout`
      (oni mają **wyłącznie `name`**)
    - podpis „Weryfikacja merytoryczna: [doradca podatkowy nr X]" + `reviewedBy` —
      czego oni nie mają w ogóle

11. **Cytowania źródeł pierwotnych.** W ich artykule 0 odwołań do ustaw i 0 do artykułów
    prawnych. Sekcja „Podstawa prawna" z linkami do ISAP, interpretacji KIS i orzeczeń
    NSA to różnica jakościowa, którą Google i modele językowe czytają jako sygnał
    wiarygodności — i której portal SaaS-owy strukturalnie nie dowiezie.

12. **JSON-LD zamiast mikrodanych.** Oni są na `itemprop` wplecionym w markup, zero
    JSON-LD. Czysty JSON-LD (`Article` + `FAQPage` + `BreadcrumbList` + `Person`
    z pełnymi atrybutami + `Organization`/`LegalService` + `SoftwareApplication` na
    kalkulatorach) jest łatwiejszy w utrzymaniu i mniej podatny na rozjazd.

13. **`<lastmod>` w sitemapie oraz rozdzielone `datePublished` / `dateModified`.**
    Oni nadpisują datę publikacji i nie dają lastmod. Pokazanie „opublikowano 2021,
    zaktualizowano 2026-08" to mocniejszy sygnał trwałej opieki nad treścią.

### Czego unikać

14. **Nie kopiować płaskich URL-i `/-slug`.** To ich dług techniczny. Mentzen powinien
    trzymać `/kategoria/temat` — URL niosący kontekst.

15. **Nie kopiować pustych stron kategorii.** Ich `/podatki` to feed z tytułem „Serwis
    Podatkowy" i taką samą meta description. 409 stron paginacji bez wartości. Strona
    kategorii u Mentzena powinna być prawdziwym hubem: definicja, mapa tematu, linki do
    filarów, FAQ.

16. **Nie gonić skali za wszelką cenę.** ~26,5 tys. artykułów przy braku widocznej
    kontroli jakości daje artykuł „2026" z nagłówkiem „…w 2024?" w środku i kalkulator
    z tytułem „2025/2024" w sierpniu 2026. Dla kancelarii z nazwiskiem na szyldzie
    nieaktualna porada podatkowa to nie jest problem SEO — to problem reputacyjny
    i potencjalnie odpowiedzialność. Lepiej 300 tekstów z twardym cyklem przeglądu
    niż 3000 gnijących.

17. **Nie podpisywać autora „z automatu".** U nich pod tekstem o ryczałcie podpisana jest
    osoba pisząca głównie o kadrach. Przy budowaniu E-E-A-T podpis musi odpowiadać
    realnej specjalizacji, inaczej strona autora działa przeciwko nam.

18. **Nie wchodzić w local SEO na wzór tego portalu** — oni go nie mają, bo nie
    potrzebują. Mentzen z oddziałami potrzebuje odwrotnie: `LocalBusiness`/`LegalService`,
    spójny NAP, strony oddziałów, GBP. To obszar, w którym ten benchmark nie mówi nic.

19. **Nie budować lejka wyłącznie na self-service.** Ich CTA to zawsze „Załóż bezpłatne
    konto". Mentzen sprzedaje usługę doradczą — CTA powinno prowadzić do konsultacji,
    a kalkulator/ebook mają być kwalifikatorem leada, nie końcem ścieżki. Sam wzorzec
    „CTA w miejscu, gdzie tekst pokazuje trudność" jest jednak wart skopiowania 1:1.

---

## Rzeczy do zweryfikowania (poza zasięgiem tej analizy)

- Realne wolumeny ruchu i profil linków (Ahrefs/Senuto) — tu tylko struktura on-site.
- Czy `/dolacz-do-ekspertow` to w praktyce kanał pozyskiwania linków zwrotnych.
- Core Web Vitals — 300 KB HTML na artykuł i 242 linki wewnętrzne to dużo; warto
  zmierzyć, czy Mentzen może wygrać na wydajności.
- Czy ich strony pojawiają się w AI Overviews mimo blokady PerplexityBota.

## Źródła

- https://poradnikprzedsiebiorcy.pl/
- https://poradnikprzedsiebiorcy.pl/-wyliczenie-skladki-zdrowotnej-u-ryczaltowca (analiza HTML)
- https://poradnikprzedsiebiorcy.pl/-nowy-polski-lad-skladka-zdrowotna-uzalezniona-od-dochodu-i-bez-odliczenia
- https://poradnikprzedsiebiorcy.pl/-formy-opodatkowania (analiza HTML)
- https://poradnikprzedsiebiorcy.pl/kalkulator-skladki-zdrowotnej (analiza HTML)
- https://poradnikprzedsiebiorcy.pl/podatki (analiza HTML)
- https://poradnikprzedsiebiorcy.pl/kalkulatory
- https://poradnikprzedsiebiorcy.pl/autor/dorociak-katarzyna
- https://poradnikprzedsiebiorcy.pl/dolacz-do-ekspertow
- https://poradnikprzedsiebiorcy.pl/robots.txt , /sitemap.xml i sitemapy składowe
