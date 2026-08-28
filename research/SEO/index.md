# Research SEO dla mentzen.pl — indeks

Jedyny punkt wejścia do całego researchu. Research wykonany **2026-08-28**.

**Cel:** materiał źródłowy pod budowę skilla SEO (Claude Code) dla mentzen.pl — kancelarii
prawo/podatki/księgowość, B2B, WordPress (Divi + Yoast), rynek polski. Cała merytoryka serwisu
to YMYL (kategoria Financial Security wg Search Quality Rater Guidelines), więc każda rekomendacja
w researchu przechodzi przez filtr E-E-A-T i zasad etyki zawodowej (KIDP/OIRP/NRA).

## Mapa plików

- **[mentzen-kontekst.md](mentzen-kontekst.md)** — stan faktyczny serwisu: 17 stron usług + 523
  wpisy blogowe, model abonamentowy (Mentzen+), struktura URL-i, taksonomia, autorzy, ton marki,
  nieporządki strukturalne (osierocone interpretacje, podwójne archiwa kategorii, pusty robots.txt).
  Stan lokalizacji rozstrzygnięty (potwierdzenie użytkownika 2026-08-28): jedno biuro stacjonarne —
  Toruń, Grudziądzka 110-114/101, konsultacje na miejscu możliwe, większość klientów zdalnie,
  oddziały Warszawa/Gdańsk/Poznań nie istnieją. Nowa sekcja 8: kanał YouTube (491 filmów, 6,2 mln
  wyświetleń, milczy od 2026-06-13, odcięty od domeny; kanał osobisty z 1,09 mln subskrybentów nie
  linkuje do mentzen.pl wcale).
  Zawiera reguły operacyjne skilla: dwa szablony treści, mapa kategoria→usługa, czego nie ruszać
  bez pytania. **Najważniejszy wniosek:** 77% wpisów bloga nie ma ani jednego linku wewnętrznego
  w treści — linkowanie kontekstowe blog→usługi to pierwszy i najtańszy kierunek pracy.

- **[eeat-ymyl.md](eeat-ymyl.md)** — destylat QRG (2025-09-11) i dokumentacji Google dla treści
  YMYL: hierarchia Trust > reszta, progi Lowest/Low, sygnały na poziomie strony/autora/serwisu,
  granice użycia AI w treści, mity o E-E-A-T; nowa sekcja 7a — zasady etyki zawodowej jako bright
  lines nadrzędne wobec zaleceń E-E-A-T (źródła pierwotne KIDP/KIRP/NRA). **Najważniejszy wniosek:** E-E-A-T to nie czynnik
  rankingowy tylko zestaw warunków brzegowych — audyt zaczynaj od usuwania tego, co odbiera
  zaufanie (nieaktualny stan prawny, brak autora, brak podmiotu odpowiedzialnego), nie od
  dodawania biogramów.

- **[content-onpage.md](content-onpage.md)** — intencje wyszukiwania, keyword research B2B
  (money keyword matrix, frazy problemowe), klastry tematyczne, struktura artykułu
  (answer-first, encje, formaty), title/meta/H1, linkowanie wewnętrzne, aktualizacja i pruning,
  CTA. **Najważniejszy wniosek:** każda fraza przechodzi test SERP przed napisaniem tekstu —
  kolejność publikacji to najpierw strony usługowe, potem jeden klaster blogowy dokończony
  do końca, a priorytet ustala potencjał biznesowy, nie wolumen.

- **[technical-seo.md](technical-seo.md)** — checklista techniczna dla WordPressa: CWV jako
  higiena, robots/noindex/canonical/sitemap, crawl bloat WP, dane strukturalne typ po typie,
  paginacja, warstwa crawlerów AI, wybór wtyczki SEO. **Najważniejszy wniosek:** FAQPage rich
  results wycofane 2026-05-07, a schema nie jest dźwignią cytowań w AI (eksperyment Ahrefs) —
  wdrażać tanio jako identyfikację encji, nie sprzedawać jako dźwignię widoczności.

- **[ai-search.md](ai-search.md)** — mechanika AI Overviews/ChatGPT/Perplexity (RAG, query
  fan-out, chunking), skala zero-click, co realnie zwiększa cytowalność, różnice między
  platformami, pomiar trójwarstwowy. **Najważniejszy wniosek:** fundamentem widoczności w AI
  jest klasyczne SEO, a najlepiej udokumentowana dźwignia cytowalności to konkret faktograficzny
  (statystyki, sygnatury, przepisy: +30–40% wg badania Princeton); llms.txt i schema „pod AI"
  to inwestycje bez zwrotu.

- **[konkurencja.md](konkurencja.md)** — analiza on-site ośmiu domen: pięciu z pierwszego przebiegu
  (crido.pl, ifirma.pl, infakt.pl, infor.pl, poradnikprzedsiebiorcy.pl) i dodanego 2026-08-28
  koszyka doradczo-audytorskiego (grantthornton.pl, rsmpoland.pl, roedl.pl — konkurenci bliżsi
  profilem niż SaaS-y księgowe): architektura, świeżość, formaty, schema, konwersja; wnioski
  przekrojowe jako reguły i priorytety dla mentzen.pl. **Najważniejszy wniosek:** recenzji
  merytorycznej treści YMYL i numeru uprawnień w byline nie ma nikt z ósemki — koszyk audytorski
  ma realne E-E-A-T autorów (RSM najbliżej: numery uprawnień na profilach + `author.@id`), więc
  przy bylinie „Zweryfikował: doradca podatkowy nr X" + pełnych stronach autorów liczy się tempo,
  nie sama decyzja.

- **[polski-rynek.md](polski-rynek.md)** — specyfika rynku PL: monopol Google (89%), AIO na
  polskich SERP-ach (dane Senuto), fleksja w keyword research, frazy cenowe jako substytut
  „best", kalendarz podatkowy jako kalendarz treści, dwie encje „Mentzen" (polityk vs
  kancelaria), ograniczenia etyki zawodowej. **Najważniejszy wniosek:** portfel fraz dziel na
  koszyki (informacyjne z ryzykiem AIO / transakcyjne / lokalne / brandowe usługowe / brandowe
  newsowe) i licz KPI osobno — wzrost wyświetleń w GSC nigdy nie jest sam w sobie sukcesem.

- **[link-building.md](link-building.md)** — linki i digital PR dla YMYL: reactive PR, dane
  własne jako linkable assets, unlinked mentions, czego nie kupować (SWL, katalogi, artykuły
  sponsorowane dofollow), linki vs wzmianki w erze AI. **Najważniejszy wniosek:** wzmianka bez
  linku i link to dwa osobne KPI — wzmianki korelują z widocznością w AI silniej niż backlinki
  i DR (Ahrefs, 75 tys. marek), więc publikacji bez linku nie odrzucaj.

- **[local-seo.md](local-seo.md)** — co z local SEO ma sens przy modelu zdalnym z jednym biurem
  w Toruniu (stan rozstrzygnięty — potwierdzony przez użytkownika 2026-08-28): profil GBP, proces opinii (polityka Google + dyrektywa Omnibus), strona biura ze
  schemą, spójność NAP/encji; twarde zakazy (strony miast, zachęty za opinie, aggregateRating).
  **Najważniejszy wniosek:** local pack poza Toruniem jest niedostępny z definicji — popyt
  z innych miast obsługuje się ogólnopolskimi frazami zdalnymi, a siatka stron „usługa + miasto"
  to doorway abuse.

- **[narzedzia-agenta.md](narzedzia-agenta.md)** — warsztat agenta SEO: GSC API (fundament),
  GA4 Data API, DataForSEO jako centrum danych SERP/keywords/backlinków/LLM, Trends, CrUX/PSI,
  advertools, WordPress REST; zasady architektury (własne cienkie CLI, cache w SQLite, twarde
  liczniki kwot) i kolejność budowy. **Najważniejszy wniosek:** fundament to GSC API + dzienny
  snapshot do SQLite od pierwszego dnia (retencja 16 miesięcy, dane przepadają), a Ahrefs/Semrush
  nie opłaca się kupować dla API przy jednym serwisie.

- **[pomysly-features.md](pomysly-features.md)** — backlog features agenta SEO odłożonych
  świadomie: warstwa SERP (gotowe API vs własny scraper — jedyna decyzja z konsekwencjami prawnymi
  dla kancelarii), rank tracking, `serp-inspect` przed pisaniem tekstu, badanie cytowań AI, bulk
  export GSC do BigQuery, Lighthouse CI, monitoring wzmianek, IndexNow/Bing, mapa sezonowości,
  warstwa zapisu do WordPressa; osobna sekcja „czego świadomie nie budujemy". **Najważniejszy
  wniosek:** decyzją użytkownika z 2026-08-28 najpierw powstaje CLI do GSC, SERP jest drugi
  w kolejce — a na tej jednej decyzji SERP wiszą naraz cztery features (rank tracking, przegląd
  SERP, detekcja AIO, monitoring newsowy).

- **[skill-authoring.md](skill-authoring.md)** — jak napisać sam skill: mechanika progressive
  disclosure, podział inline vs references, zasady pisania (test no-opa, prompt pozytywny,
  kryteria ukończenia), antywzorce, ewaluacje przed pisaniem; propozycja struktury `seo-mentzen`
  z frontmatterem i listą referencji. **Najważniejszy wniosek:** model już zna SEO — wartość
  skilla to kontekst mentzen.pl, gradacja dowodowa i lista mitów (`myths.md`), a najpierw trzeba
  zmierzyć baseline bez skilla na realnych zadaniach.

- **[zrodla.md](zrodla.md)** — bibliografia całego researchu z rangami zaufania [A]/[B]/[C]:
  kanon ~30 dokumentów Google + QRG, warstwa [A] prawa polskiego (Dz.U. przez ELI API Sejmu,
  teksty pierwotne KIDP/KIRP/NRA), badania z twardymi danymi, osiem filmów (siedem z konfliktem
  interesów), lista zakazu (źródła odrzucone), mapa twierdzeń `[do weryfikacji]` — po notatkach
  `uzup-*` z dawnych blokerów wdrożeniowych otwarta tylko retencja i limity GSC API — oraz gotchas
  dostępowe. **Najważniejszy wniosek:** o tym, „czego chce Google", wolno cytować wyłącznie
  źródła z kanonu — dokumentacja jest ruchoma i trzeba ją pobierać na żywo, nie cache'ować liczb.

## Najważniejsze wnioski przekrojowe

Punkty, które muszą przeżyć destylację do skilla:

1. **Cała merytoryka to YMYL, a Trust jest nadrzędny.** Żaden tekst podatkowo-prawny bez
   podpisanego autora z tytułem zawodowym i numerem wpisu, podstawy prawnej z sygnaturami
   i widocznego „stan prawny na DD.MM.RRRR". Sygnały jakości są site-wide — stary, martwy blog
   ciągnie w dół strony usługowe.
2. **E-E-A-T formułować jako warunki brzegowe, nie dźwignie.** Nigdy „dodaj X → wzrost pozycji";
   zawsze „brak X spycha do Low/Lowest". Kolejność audytu: najpierw usuwanie ryzyk Trust, potem
   budowa biogramów i schemy.
3. **Recenzja merytoryczna to przewaga, której konkurencja nie ma.** 0/8 badanych konkurentów
   podpisuje treści recenzentem z uprawnieniami, numeru uprawnień w byline artykułu nie ma nikt —
   byline + „Zweryfikował merytorycznie" + `reviewedBy` + pełne strony autorów (`Person`
   z `hasCredential`, `sameAs`) to najtańsza dźwignia całego researchu. Koszyk doradczo-audytorski
   ma realne E-E-A-T autorów (RSM najbliżej kompletu), więc liczy się tempo, nie sama decyzja.
4. **Jedna intencja = jeden URL; test SERP przed każdym tekstem.** Warianty fleksyjne nigdy nie
   uzasadniają osobnej strony; warianty leksykalne rozstrzyga porównanie SERP-ów. Kanibalizację
   diagnozować w GSC przed publikacją.
5. **Kolejność publikacji: najpierw money pages, potem jeden klaster do końca.** Priorytet fraz
   według potencjału biznesowego, nie wolumenu — frazy problemowe („kontrola celno-skarbowa co
   robić") i cenowe („ile kosztuje doradca podatkowy") mają najwyższą intencję; polski substytut
   „best" to frazy cenowe i porównawcze.
6. **Frazy informacyjne oddane AIO grają o wzmiankę, nie o kliknięcie.** CTR pozycji 1 przy AIO
   −58%; KPI liczyć per koszyk fraz, a metryką sukcesu bloga są zapytania ofertowe, nie sesje.
   Odporne na zero-click: kalkulatory, wzory, terminarze, frazy transakcyjne i brandowe.
7. **Fundament widoczności w AI to klasyczne SEO plus konkret faktograficzny.** Każdy istotny
   akapit z zaczepem: liczba, kwota, sygnatura, przepis (+30–40% widoczności w silnikach
   generatywnych). Sekcje atomowe, answer-first, tabele z samodzielnymi wierszami. Nie
   inwestować w llms.txt ani schemę „pod AI".
8. **Wzmianki i linki to osobne KPI.** Większość cytowań AI pochodzi ze stron trzecich; wzmianka
   bez linku ma samodzielną wartość dla AI, żadną dla klasycznego rankingu. Najlepszy stosunek
   efektu do nakładu: komentarze eksperckie w mediach + kwartalny materiał z danymi własnymi
   (zagregowanymi, zanonimizowanymi). Zero kupowanych linków dofollow.
9. **Aktualizacja archiwum bije produkcję nowego.** Stan prawny starzeje treść z każdą
   nowelizacją; refresh klastra zbiorczo, w kalendarzu sprzężonym z legislacją, z realną zmianą
   treści — sama podmiana daty nie działa i jest sygnałem spamu. Publikować 4–8 tygodni przed
   szczytem sezonu, najlepiej na etapie projektu ustawy. Terminy potwierdzać w Dz.U. przez ELI API
   Sejmu (`_notes/uzup-terminy-podatkowe.md`), nie w portalach.
10. **Linkowanie wewnętrzne to najsłabszy element mentzen.pl i najtańsza naprawa.** Każdy nowy
    lub edytowany wpis: minimum jeden kontekstowy link do landinga usługowego (mapa
    kategoria→usługa jest mechaniczna) i linki do wpisów z tej samej serii.
11. **Local SEO = domknięty moduł „Toruń + reputacja".** GBP, proces opinii bez zachęt (polityka
    Google + Omnibus), strona biura, spójny NAP. Strony miast bez biura, aggregateRating dla
    własnych opinii i słowa kluczowe w nazwie GBP — zakazy zaszyte w skill.
12. **Brand „Mentzen" to dwie encje.** Nie optymalizować pod gołe nazwisko (SERP należy do
    polityka); budować frazy „kancelaria mentzen", „mentzen+ cena", nazwy produktów; ruch
    brandowy raportować z podziałem usługowy/newsowy; E-E-A-T rozkładać na cały zespół doradców.
13. **Dyscyplina dowodowa: tylko kanon Google jako źródło twierdzeń o Google.** Liczby branżowe
    z atrybucją i datą; polskie blogi agencyjne nigdy jako źródło liczb; tezy z filmów zawsze
    z autorem i jego konfliktem interesów. W researchu wyłapano co najmniej dwa fałszywe „fakty"
    krążące po branży.
14. **Pomiar od pierwszego dnia:** GSC API + snapshot do SQLite (dane przepadają po 16
    miesiącach), grupa kanałów AI w GA4, pytanie „skąd o nas wiesz" w formularzu, reguła
    sezon-vs-problem (Trends × GSC) przed każdą diagnozą spadku.
15. **Ograniczenia etyki zawodowej jako bright lines, nie miękki filtr.** Rozstrzygnięte 2026-08-28
    w źródłach pierwotnych KIDP/KIRP/NRA (`_notes/uzup-etyka-doradcy.md`,
    `_notes/uzup-etyka-prawnicy.md`; werdykty w [eeat-ymyl.md](eeat-ymyl.md) §7a
    i [link-building.md](link-building.md) §4a): cenniki, case studies i opinie planować po stronie
    doradztwa podatkowego/księgowości; przy treści dotyczącej adwokatów opinie klientów, publiczny
    cennik i publikacje płatne są zakazane, a dla treści mieszanej obowiązuje reżim najsurowszy.
    Skill egzekwuje bright lines sam; do samorządu/compliance kieruje wyłącznie kwestie oznaczone
    `[do weryfikacji]`.

## Materiały towarzyszące

Surowe notatki researchowe (38 plików: analizy konkurencji per domena — w tym trzy
doradczo-audytorskie dodane 2026-08-28, badanie serwisu i kanałów YouTube mentzen.pl, notatki
narzędziowe, notatki z filmów oraz uzupełnienia `uzup-*` z terminami podatkowymi i etyką zawodową
ze źródeł pierwotnych) leżą w [`_notes/`](_notes/).
Transkrypty filmów YouTube użytych w researchu: `~/projects/yt-transcript/transcripts/`.

## Co dalej

**Budowa skilla** (plan w [skill-authoring.md](skill-authoring.md)):

1. Ewaluacje przed pisaniem: uruchomić agenta bez skilla na realnych zadaniach mentzen.pl,
   zapisać porażki, zbudować ≥3 scenariusze, zmierzyć baseline.
2. `SKILL.md` (cel <200 linii): rama, procedura, non-negotiables, tabela gradacji dowodowej,
   indeks referencji, kalibracja. Start referencji: `anti-patterns.md`, `myths.md`, `on-page.md` —
   reszta po zaobserwowaniu realnych luk.
3. Blokery wdrożeniowe z [zrodla.md](zrodla.md) §12 są domknięte (2026-08-28): terminy podatkowe
   (`_notes/uzup-terminy-podatkowe.md` — Dz.U. przez ELI API Sejmu), etyka zawodowa
   a opinie/case studies/cenniki (`_notes/uzup-etyka-doradcy.md`, `_notes/uzup-etyka-prawnicy.md` —
   źródła pierwotne KIDP/KIRP/NRA), a wcześniej rich results dla
   `LegalService`/`AccountingService` i polityka self-serving `AggregateRating`
   ([technical-seo.md](technical-seo.md) §4.3 i §11). Z blokerów została tylko pomiarowa retencja
   i limity GSC API — zweryfikować empirycznie zaraz po uzyskaniu dostępu do właściwości; resztki
   `[do weryfikacji]` (pierwszy rocznik terminu JPK ksiąg, dzienna data ORD-U, sezonowość
   zapytań) nie blokują napisania skilla, tylko pojedyncze treści.

**Narzędzia wymagające konfiguracji użytkownika** (checklista w
[narzedzia-agenta.md](narzedzia-agenta.md)):

- Projekt GCP: GSC API + GA4 Data API + PSI/CrUX, service account z kluczem JSON, dodanie konta
  w GSC (poziom „Pełny") i GA4 (przeglądający); weryfikacja key events w GA4 (formularz, telefon).
- Konto DataForSEO (wpłata $50, limit wydatków w panelu) + świadoma decyzja o ToS Google przy
  SERP API.
- WordPress: osobne konto agenta z Application Password (zapis tylko jako draft), snippet
  `register_post_meta` dla kluczy Yoasta (zmiana w kodzie).
- Wnioski równoległe: Google Ads API (poziom Basic), alfa Trends API. Opcjonalnie: Bing Webmaster
  Tools, wtyczka IndexNow, Senuto (decyzja budżetowa), Brand24.
- Lista 150–300 fraz filarowych — jedyny wsad, którego żadne API nie zastąpi; praca wspólna
  z agentem.

**Otwarte decyzje użytkownika:**

- Status produktów znanych tylko z regulaminów (Mentzen Nieruchomości, S24, Legalny Mentzen) —
  przed proponowaniem landingów.
- Przywrócenie sekcji interpretacji (33 case studies z sygnaturami) do nawigacji.
- Lokalne SEO ponad moduł Toruń (strategia jest świadomie zdalna; stan faktyczny rozstrzygnięty
  2026-08-28: jedno biuro w Toruniu, oddziały nie istnieją) i ewentualne przyszłe oddziały.
- Wybór wariantu SERP: gotowe API czy własny scraper — decyzja druga w kolejce po CLI do GSC
  (porównanie i konsekwencje prawne: [pomysly-features.md](pomysly-features.md) §1; tam też
  pozostałe decyzje z 2026-08-28 — najpierw GSC, badanie cytowań AI po uzyskaniu dostępu).
- Scalenie podwójnych archiwów kategorii bloga (zmiana strukturalna).
- Budżety: Senuto, Screaming Frog, Brand24 — dopiero gdy fundament (GSC + DataForSEO) działa.
- Badania własne do wykonania: domeny cytowane w AIO/ChatGPT dla polskich zapytań
  podatkowo-prawnych, baseline SERP-u brandowego, udział AIO na własnej próbce fraz, realny
  ruch/linki konkurencji (Ahrefs/Senuto).
