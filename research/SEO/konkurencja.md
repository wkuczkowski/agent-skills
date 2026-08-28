# Konkurencja w polskich SERP-ach podatkowo-prawnych: benchmarki i wnioski

Data syntezy: 2026-08-28. Podstawa: analizy on-site pięciu domen (crido.pl, ifirma.pl, infakt.pl, infor.pl + gofin.pl/pit.pl, poradnikprzedsiebiorcy.pl) wykonane 2026-08-28 metodą: surowy HTML (curl), sitemapy, JSON-LD/mikrodane, struktura nagłówków i linków. Uzupełnienie 2026-08-28: dołączona synteza trzech domen doradczo-audytorskich — grantthornton.pl, rsmpoland.pl, roedl.pl (zob. sekcję „Konkurencja doradczo-audytorska"; pełne analizy: `_notes/konkurencja-grantthornton-pl.md`, `_notes/konkurencja-rsmpoland-pl.md`, `_notes/konkurencja-roedl-pl.md`), badanych tą samą metodą. **Żadna z analiz nie obejmuje danych o realnym ruchu, profilu linków przychodzących ani Core Web Vitals** — wnioski o widoczności oparte są na strukturze [do weryfikacji przez Ahrefs/Senuto/PageSpeed]. Pozycja Crido nr 1 na „ulga B+R" przyjęta za briefem, niezweryfikowana niezależnie [do weryfikacji].

## TL;DR

1. **Nikt nie ma recenzji merytorycznej treści YMYL — także po rozszerzeniu koszyka: 0/8.** Zero wystąpień „zweryfikował doradca podatkowy nr X", zero `reviewedBy` w schemacie. RSM i Rödl mają numery uprawnień w profilach ekspertów, ale nikt nie pokazuje recenzenta ani nie wystawia numeru uprawnień w byline artykułu. To największa i najtańsza dźwignia E-E-A-T dla kancelarii: byline z uprawnieniami + widoczna weryfikacja + `Person` z `hasCredential`/`sameAs`.
2. **Wszyscy wygrywają autorytetem domeny i wolumenem, nie jakością on-page.** Infor ~46 tys. publikacji, poradnikprzedsiebiorcy ~26,5 tys., ifirma ~4,7 tys., Crido ~5,4 tys. Nie ścigać się ilością — wygrywać głębią na wąskich klastrach (50 pillarów nie do przebicia zamiast 500 przeciętnych).
3. **Dane strukturalne to u wszystkich słaby punkt:** Crido — goły Yoast bez `Article` i `Person`; poradnikprzedsiebiorcy — mikrodane zamiast JSON-LD, `Person` z samym `name`; inFakt — trzy sprzeczne bloki JSON-LD; pit.pl — generyczny `@graph` Yoasta z autorem „Redakcja PIT.pl", bez eksperta w schemacie. Koszyk doradczo-audytorski tego nie zmienia: Grant Thornton daje `author` bez `@id`/`url`, Rödl ma **zero JSON-LD na całej domenie**, a jedynie RSM robi `author.@id` poprawnie (przy zerze schemy na stronach usług). Poprawny, spójny `@graph` (Article + BreadcrumbList + Person + Organization) to przewaga do wzięcia od ręki. **Bez `FAQPage`/`HowTo`** — rich results wycofane 2026-05-07 (zob. technical-seo.md, content-onpage.md §5.4); braku tych znaczników u konkurencji nie traktuj jako luki.
4. **Świeżość: rok w `<title>`, nigdy w URL i H1** (wzorzec Crido: title „Ulga B+R 2026", URL `/ulga-b-r/`, H1 „Ulga B+R"). Refresh klastra robić zbiorczo, raz–dwa razy w roku, jedną sesją (Crido: cały klaster ulg 14.01.2026; inFakt: cały klaster spółek 02.06.2026) — i **wyświetlać datę aktualizacji on-page**, czego nie robi nikt z piątki.
5. **Format, który rankuje w tej niszy: liczby przed wykładnią.** Tabele porównawcze z kwotami, numerowane przykłady z wyliczeniami, H2 jako pytania z PAA, kroki „1…N", FAQ na końcu, spis treści z kotwicami. Infor — lider wolumenowy — ma **zero tabel** nawet w artykule porównującym trzy formy opodatkowania; to prezent.
6. **Kalkulatory to osobny silnik ruchu** (inFakt: 23 sztuki, w tym „JDG czy spółka"; Infor: linkowane ze stopki wszystkich ~46 tys. stron). Wynik bez bramki (wzorzec Crido) — formularz dopiero pod wynikiem.
7. **Architektura: krótki pillar ofertowy w korzeniu domeny + długi klaster blogowy + jedna wątpliwość = jeden URL.** Blog zawsze w podkatalogu tej samej domeny, nigdy na subdomenie (wyjątek Infor działa tylko dzięki spięciu subdomen wspólnym `@id` organizacji i sile marki wydawcy).
8. **Local SEO: gra się jedną mocną stroną biura, nie siecią oddziałów.** Kancelaria ma jedną stacjonarną lokalizację (Toruń, Grudziądzka 110-114/101; konsultacje stacjonarne możliwe, większość klientów obsługiwana zdalnie) — oddziały Warszawa/Gdańsk/Poznań nie istnieją i nie wolno pod nie budować stron. W koszyku SaaS/wydawców pole puste (cienkie doorway'e ifirmy/inFaktu bez `LocalBusiness`); uwaga: Grant Thornton siedzi w Toruniu (dwa biura, w tym Grudziądzka 46-48) i rozprowadza schema biur po całym serwisie.
9. **Landingi usługowe konkurencji są cienkie** (inFakt: `/ksiegowosc-dla-spolek/` bez treści, FAQ i linków do bloga). Landing hybrydowy — pełna treść kompendium + formularz po sekcjach + widoczne FAQ — bije je contentowo.
10. **Najlepsze punkty startowe tematyczne:** „podatki spółki z o.o." (ifirma nie ma pillara dla spółek — kategoria płaska) i „fundacja rodzinna" (u Infora ledwie 70 tekstów, głównie newsy z cudzymi komentarzami, bez kompendiów) — oba w rdzeniu kompetencji Mentzena.
11. **Konkurencja doradczo-audytorska (grantthornton.pl, rsmpoland.pl, roedl.pl) to konkurenci bliżsi profilem niż SaaS-y księgowe** — firmy usług profesjonalnych z realnym E-E-A-T: tytułami zawodowymi, numerami uprawnień, cyklicznymi publikacjami z danych własnych. Wobec nich przewaga Mentzena nie polega na „mieć ekspertów" (mają), tylko na domknięciu tego, czego nie domyka żaden z trzech: recenzja merytoryczna + kompletna warstwa maszynowa (`Person` z `hasCredential`, `author.@id`, `reviewedBy`, `Article` z datami) + higiena techniczna (GT: puste meta description i 34% archiwum nietykane od 2018–19; RSM: ~33% zombie-URL-i i sitemapa w 100% na 301; Rödl: zero danych strukturalnych, URL-e ze spacjami).

---

## Mapa konkurentów

| Domena | Model biznesowy | Skala treści | Czym wygrywa | Główna słabość |
|---|---|---|---|---|
| crido.pl | doradztwo B2B (bezpośredni konkurent) | ~5 400 URL-i | architektura pillar/cluster, refresh roczny, formaty (hottopic, case study) | schema = goły Yoast; puste strony autorów; pillar bez podpisu eksperta |
| ifirma.pl | SaaS + biuro rachunkowe (GPW, od 2001) | ~4 700 wpisów, 47 autorów | wolumen + format kalkulacyjny (tabele, przykłady z kwotami), 301 na wersje rocznikowe | autorzy bez uprawnień, brak `BreadcrumbList`, stock grafiki, doorway pages miastowe |
| infakt.pl | SaaS + księgowość (Visma), spółki od 619 zł | klaster spółek + 23 kalkulatory + 3 102 URL-e lokalne | domknięta pętla treść→konwersja, kalkulatory, podwójne obstawienie fraz | autor = były dziennikarz radiowy; bałagan w JSON-LD; landingi bez treści |
| infor.pl (+gofin, pit.pl) | wydawca (subskrypcje, szkolenia) | ~46 tys. publikacji, 3–5 dziennie/sekcję | wiek URL-i, tempo, publikacja na etapie projektu ustawy, warstwa narzędziowa | brak aktualizacji (dateModified==datePublished 3/3), zero tabel, model „oprac." |
| poradnikprzedsiebiorcy.pl | content marketing SaaS wFirma | ~26,5 tys. artykułów | brutalna głębokość pokrycia (kilkanaście URL-i na jedną frazę-parasol) | zero cytowań ustaw/KIS, puste strony autorów, gnijące refreshe („2024" w artykule „2026") |
| grantthornton.pl | audyt + doradztwo B2B (bliski profilem) | ~5 100 URL-i, w tym 3 330 publikacji PL | taksonomia bloga = taksonomia usług, 246 stron usług, raporty cykliczne z danych własnych, boks eksperta z komórką | puste meta description, `author` bez `@id`, okruszki 2-poziomowe, 34% archiwum nietykane od 2018–19 |
| rsmpoland.pl | audyt + doradztwo B2B (bliski profilem) | 1 557 URL-i | 179 profili z numerami uprawnień, `author.@id`, format artykułu pod AI search, CTA śródtekstowe do podusług | ~33% zombie-URL-i z pustymi `<title>`, sitemapa w 100% na 301, zero schemy na usługach, brak raportu branżowego |
| roedl.pl | audyt + doradztwo, kapitał niemiecki (konkurencja częściowa — inwestor zagraniczny) | 1 472 URL-e PL (z 3 609) | bio ekspertów z rokiem wpisu na listę, blok Q&A 10 pytań, broszury roczne, 5 formatów wokół klastrów | **zero danych strukturalnych na całej domenie**, URL-e ze spacjami/diakrytykami, profil pod `<title>` „Profil \| Rödl" |

---

## Syntezy per domena

### crido.pl — architektura zamiast długości

Źródła: crido.pl/ulga-b-r/, /ip-box/, /kalkulator-ip-box/, sitemapy (2026-08-28; serwis zwraca 403 na WebFetch — badać curlem z przeglądarkowym UA).

Wygrywa frazy typu „ulga B+R" **krótką stroną pillar (~1 200–1 400 słów merytoryki) w korzeniu domeny** (`/ulga-b-r/`, nie `/uslugi/podatki/...`), siedzącą na szczycie klastra ~5 400 URL-i podzielonych na custom post types per obszar (`/blog-taxes/` 3 188, `/blog-business/` 737, `/blog-law/` 459, plus `hottopic`, `case_study`, `threads`). Sekwencja pillara odwzorowuje decyzję kupującego: definicja → **autokwalifikacja („Czy ulga B+R jest dla Ciebie?" + 3 twarde warunki)** → koszty kwalifikowane → FAQ → dlaczego my → case study → linki do siostrzanych ulg („Ulga B+R a inne ulgi" — łapie frazy porównawcze). Refresh: cały klaster ulg jedną sesją redakcyjną (obie strony ulgowe `dateModified` 2026-01-14, 17 sekund różnicy), rok tylko w title.

Formaty warte kopiowania: **`hottopic` = landing na zmianę prawa** (KSeF: co się zmienia → od kiedy → produkty z „jak działamy"/„efekty"), kwartalny „Przegląd interpretacji indywidualnych" (tani, sygnalizuje czynną praktykę), kalkulator IP Box/B+R **bez bramki** (wynik za darmo, formularz pod nim), formularz kwalifikujący B2B (firma+stanowisko+dział — mniej leadów, lepsze), trzy progi zaangażowania (newsletter → narzędzie → rozmowa).

Słabości: schema to goły Yoast (`WebPage` na wszystkim — **brak `Article`, brak `Person`**, `sameAs` bez LinkedIna, brak hreflang mimo `/en/`); strony autorów puste (jedna linijka, zero bio/uprawnień); **pillar o najwyższej stawce nie ma podpisu żadnego człowieka**; publicznie widoczny autor `/author/widoczni_seo/` (konto agencji); użytkownik widzi datę „21 sierpnia 2019" na treści aktualizowanej w 2026; H2 używane jako styl akapitu (artefakt page buildera); automatyczny widget „najnowsze" linkuje z pillara do treści niezwiązanych (81 linków przy 2 350 słowach).

### ifirma.pl — wolumen + format kalkulacyjny

Źródła: ifirma.pl/blog/cit-estonski-w-spolce-z-o-o-w-2026-...-przeksztalcenie-jdg-w-spolke/, /jednoosobowa-dzialalnosc-gospodarcza-kompendium/, /ksiegowosc-dla-spolek/, sitemapy (2026-08-28).

Topowy artykuł o estońskim CIT (4 865 słów) rankuje **formatem: tabele porównawcze z policzonymi kwotami, przykłady 1–5 z wyliczeniami, podstawy prawne z linkiem do eureka.mf.gov.pl i sygnaturami WSA, sekcja wyprzedzająca „zmiany 2027?", FAQ w akordeonie**. Odpowiada liczbą w tabeli na pytanie „ile mnie to będzie kosztować" — kancelarie zwykle odpowiadają akapitem o wykładni. Kluczowy manewr: **stary evergreen → 301 → nowy slug z rokiem i rozbudowaną treścią** (zachowuje linki, odświeża snippet). FAQ oznaczone **mikrodanymi wpiętymi w szablon akordeonu** — schema nie może się rozjechać z widoczną treścią. Odświeżają selektywnie: tylko artykuły blisko pieniędzy (spółka = pakiet 650 zł/mies.). Meta robots: `max-snippet:-1, max-image-preview:large` — otwarcie na duże snippety i AI Overviews. Segmentacja ofertowa: 11 stron `/ksiegowosc-dla-<segment>/` (w tym `/ksiegowosc-dla-prawnikow-i-radcow-prawnych/` — atakują segment Mentzena). Dwustronna pętla: artykuł linkuje do oferty kontekstowo, oferta ma blok 10 wpisów blogowych; FAQ na stronie ofertowej łapie frazy informacyjne bez osobnego artykułu.

Słabości: kompendium zbudowane dla JDG, **ale nie dla spółek — kategoria `/blog/spolki/` (~50 artykułów) leży płasko bez pillara**; autorzy to „księgowe i autorki tekstów" bez uprawnień, bio marketingowe; brak `BreadcrumbList`; brak `AggregateRating`/`Offer` mimo 900+ opinii i cen; strony miastowe = doorway (miasto tylko w H1, adres wrocławski, bez `LocalBusiness`); stockowe zdjęcia z rozjechaną schemą obrazka; stare artykuły bez `dateModified`.

### infakt.pl — domknięta pętla treść → konwersja

Źródła: infakt.pl/blog/jak-zalozyc-spolke-z-o-o-krok-po-kroku/, /blog/spolka-z-o-o-kompendium-krok-po-kroku/, /zalozenie-spolki-z-o-o-kompendium-wiedzy, /ksiegowi/ (2026-08-28).

Najczystszy model pillar/cluster w koszyku: hub „spółka z o.o." (6 485 słów) + kilkanaście satelitów, **jedna wątpliwość = jeden URL** (podział zysku ≠ pokrywanie straty ≠ wynagrodzenie zarządu — zero kanibalizacji). **Podwójne obstawienie frazy:** artykuł blogowy + landing hybrydowy w korzeniu (`/zalozenie-spolki-z-o-o-kompendium-wiedzy` — treść kompendium przeplatana formularzami rejestracji) + czysty landing transakcyjny = trzy pozycje na wariantach frazy. 23 kalkulatory (w tym `kalkulator-jdg-czy-spolka` — dokładnie decyzja, którą chce wygrywać Mentzen). Nagłówki: pytania z PAA + numerowane kroki + pełna fraza w każdym H2/H3; konkretne kwoty w treści (250 zł KRS online, PCC 23,25 zł...). Konwersja wpleciona jako rozwiązanie problemu z akapitu wyżej (tabela księgowych z cenami w środku artykułu), linki konwersyjne z parametrami atrybucji zablokowanymi w robots.txt. Cały refresh klastra spółek jednego dnia (2026-06-02). Higiena: `Disallow: /tagi/`, blokady parametrów.

Słabości: **artykuły o rejestracji spółki, PCC i VAT-R podpisuje były dziennikarz RMF FM** — zero recenzji merytorycznej; trzy sprzeczne bloki JSON-LD na jednej stronie (Article z Yoasta + Article z ratingiem + ręczny NewsArticle, autor raz `Person`, raz `Organization`); **self-serving `AggregateRating` na artykułach** (ryzyko manual action — nie kopiować); landing usługowy bez treści i FAQ, żyje z link juice'u bloga; on-page widoczna tylko data publikacji sprzed 18 miesięcy przy świeżym `dateModified` w schemacie; ~445 programatycznych stron miejscowości (łącznie z wsiami i `/ksiegowi/paryz`) bez schema i treści — dług czekający na core update.

### infor.pl (+ gofin.pl, pit.pl) — wiek URL-a i tempo wydawcy

Źródła: ksiegowosc.infor.pl/wiadomosci/7524215..., /podatki/7496316..., /tematy/podatki-2026/, /tematy/fundacja-rodzinna/, /ksef/, gofin.pl, pit.pl (2026-08-28).

Mechanizmy przewagi: (a) **publikacja na etapie projektu ustawy** — w sierpniu 2026 klaster „podatki 2027" już istnieje („Projekt zakłada podwyżkę CIT do 22%..."); gdy ruch przychodzi, URL ma miesiące wieku i linków; (b) **podwójny cykl sezonowy**: evergreen 5 tygodni przed terminem + news domykający dzień przed (oba w TOP — stać ich na kanibalizację, Mentzena nie); (c) **warstwa narzędziowa w stopce każdego artykułu** (kalkulatory, wskaźniki, druki, terminarz z URL-em na każdy dzień) — narzędzia dostają linki ze wszystkich ~46 tys. stron; (d) druga oś taksonomii `/tematy/<tag>/` jako realne klastry (`/tematy/podatki-2026/` — 126 tekstów), spinane z artykułów; (e) subdomeny spięte w jedną encję przez wspólne `@id` `WebSite`/`NewsMediaOrganization` w JSON-LD; (f) nagłówki językiem problemu („Pułapka pierwszej faktury", „Ryczałt – król opłacalności, ale nie dla każdego"), świadomy rozjazd title (frazowy) vs H1 (emocjonalny, pod CTR/Discover); (g) profile ekspertów z pełną listą publikacji (Biliński: 340 tekstów) budujące topical authority per osoba.

Słabości: **`dateModified == datePublished` w 3/3 badanych artykułów** — nie aktualizują, publikują od nowa; **zero tabel** we wszystkich trzech tekstach, w tym w porównaniu trzech form opodatkowania; `Person` bez kwalifikacji; „Podstawa prawna" bez artykułów ustawy i linków do ISAP; model „oprac." (redaktor przetwarza cudzy materiał); `NewsArticle` nadużywany dla evergreenów (gra pod Top Stories dostępna wydawcy prasowemu — bez tego statusu używać `Article`). Koszyk: gofin.pl ma H1=„GOFIN.PL" na każdej stronie i treść z 2021 w TOP; pit.pl ma tylko generyczny `@graph` Yoasta (autor „Redakcja PIT.pl"), za to model „kancelaria jako źródło merytoryczne portalu" (radca z kancelarii Graś i Wspólnicy wyróżniony w tekście) — **rola do przejęcia przez Mentzena: dostarczanie komentarzy portalom = linki + cytowania nazwiska**. Gofin zwraca 403 nietypowym UA — wycina też crawlery AI; nie kopiować.

### poradnikprzedsiebiorcy.pl — brutalna głębokość pokrycia

Źródła: poradnikprzedsiebiorcy.pl/-wyliczenie-skladki-zdrowotnej-u-ryczaltowca, /kalkulator-skladki-zdrowotnej, /-formy-opodatkowania, /podatki, /autor/dorociak-katarzyna (2026-08-28).

~26,5 tys. artykułów jako lejek do SaaS wFirma. Mechanizm „dwóch miejsc w TOP naraz": **na jedną frazę-parasol kilkanaście URL-i pokrywających każdy przekrój intencji** (składka zdrowotna: per forma opodatkowania × moment roku × edge case × kalkulator). Wzorzec on-page wart skopiowania: **title ≠ H1 ≠ pierwszy H2** — trzy sformułowania tej samej intencji (title = exact match + rok; H1 = pytanie long tail; H2 = powtórka exact match); rok N i N-1 świadomie obok siebie w treści (łapie rozliczających wstecz); 3 tabele + „Przykład 1–4" z kwotami; widoczne FAQ z PAA (mają też markup `FAQPage` — tego nie kopiować, rich result wycofany); kalkulator jako typ strony z 4,6 tys. słów obudowy (H3 per miesiąc = sezonowe long taile w jednej stronie); **lead magnet jako hub klastra** (`/-formy-opodatkowania`: ebook „bez rejestracji" + dynamiczna lista ~20 artykułów — strona z 2023 wciąż zbiera świeże linki wewnętrzne); CTA kontekstowe dokładnie w miejscu, gdzie tekst pokazuje trudność ręcznego wyliczenia.

Słabości: **zero cytowań źródeł pierwotnych** (0 odwołań do ustaw, 0 „art. X", 0 linków do ISAP/KIS/ZUS — linki zewnętrzne tylko do wfirma.pl i poradnikpracownika.pl); strony autorów puste (sam listing, bez bio/zdjęcia/uprawnień), podpisy przypisane niezgodnie ze specjalizacją; mikrodane zamiast JSON-LD, `Person` z samym `name`; `datePublished` nadpisywane przy refreshu (znika historia); brak `<lastmod>` w sitemapach; gnijące refreshe (H3 „...w 2024?" wewnątrz artykułu „2026", kalkulator z title „2025/2024" w sierpniu 2026); puste strony kategorii (feed z 409 stronami paginacji); płaskie URL-e `/-slug` bez kontekstu (relikt, nie wzorzec). Ciekawostka: robots.txt zamyka SemrushBotowi, PerplexityBotowi, bingbotowi i mj12botowi wyłącznie `/categories/search` — reszta serwisu stoi otworem dla wszystkich, Google i GPTBot bez żadnych ograniczeń.

---

## Konkurencja doradczo-audytorska — bliżsi profilem niż SaaS-y

Dodane 2026-08-28. Pełne analizy: `_notes/konkurencja-grantthornton-pl.md`, `_notes/konkurencja-rsmpoland-pl.md`, `_notes/konkurencja-roedl-pl.md` (wszystkie badania 2026-08-28, metoda jak wyżej — struktura i sygnały on-page, bez danych o ruchu).

Ta trójka konkuruje z Mentzenem inaczej niż SaaS-y i wydawcy: to firmy usług profesjonalnych sprzedające zaufanie do ludzi z uprawnieniami, więc walka toczy się na E-E-A-T, a nie na wolumen. Wszyscy trzej mają to, czego piątce brakowało (realni eksperci, tytuły zawodowe, publikacje cykliczne) — i wszyscy trzej marnują to w warstwie technicznej lub maszynowej. Zastrzeżenie: Rödl gra częściowo w innym segmencie (niemiecki kapitał w Polsce, serwis trójjęzyczny ~40/30/30 PL/EN/DE), więc pokrycie fraz z Mentzenem jest tylko częściowe.

**Uwaga do rekomendacji `FAQPage` z notatek źródłowych:** czytać przez §Dane strukturalne — rich result wycofany 2026-05-07. Widoczny blok Q&A kopiować, nowego markupu `FAQPage` nie wdrażać.

### grantthornton.pl — taksonomia treści = taksonomia oferty

~5 100 URL-i (WordPress + Yoast): 3 330 publikacji PL, **246 stron usługowych** rozdrobnionych do pojedynczego zagadnienia, 314 profili pracowników. Najmocniejszy mechanizm w całym koszyku: **kategorie artykułów są lustrem hierarchii usług** (`/articles_categories/uslugi/doradztwo-podatkowe/podatek-vat/` ↔ `/usluga/doradztwo-podatkowe/`), więc linkowanie artykuł ↔ oferta generuje szablon, nie redaktor. Boks eksperta pod artykułem to jednocześnie CTA („Skontaktuj się": nazwisko + tytuł zawodowy + mail + **komórka**). Do tego jedyne w koszyku **cykle na danych własnych** (miesięczny raport o ofertach pracy z autorskiego systemu, marki „Purpurowy Informator", „Barometr prawa") — linkable assets: liczba w leadzie + metodologia + cytat imienny + stała częstotliwość, a sam raport to ~230 słów prozy. Rytm publikacji ~45–55 URL-i/mies., szczyty w styczniu i sierpniu.

Słabości do wykorzystania: puste `meta description` na stronach pieniężnych, zdublowany sufiks marki w `<title>`, `author` w JSON-LD bez `@id`/`url` (mimo istniejących stron ekspertów), okruszki tylko 2-poziomowe, hreflang na stronę główną zamiast na tłumaczenia, brak numerów uprawnień w profilach, zero linków do źródeł prawa w treści YMYL, **34% archiwum nietykane od 2018–2019** — publikują szybciej, niż porządkują.

### rsmpoland.pl — jakość strony i autorytet autorów, fatalna higiena

Najmniejszy z trójki (1 557 URL-i, Drupal), a mimo to najbliższy wzorca E-E-A-T: **179 profili ekspertów z numerami uprawnień** (np. „doradca podatkowy, numer uprawnień 10240", rok wpisu, staż, LinkedIn) i — **jako jedyny w całym koszyku ośmiu domen — `author` jako `Person` z `@id`/`url`** wskazującym na profil. Format artykułu najlepiej z całego koszyka ustawiony pod AI search: ramka „Kluczowe informacje" (3 punkty) na górze, H2-pytania (7/13 w przewodniku), FAQ + glosariusz + checklista w jednym tekście, trzy warianty tytułu (długi H1 / krótki frazowy `<title>` / osobny `og:title`). Śródtekstowy blok CTA linkuje do **podusług dobranych tematycznie** (kuracja redakcyjna, nie automat), strony usług segmentują po branży („Ulga B+R dla branży IT") i po świeżych regulacjach (Pillar 2, DAC7, KSeF, JPK_CIT).

Słabości do wykorzystania: **~33% serwisu to zombie-URL-e** po migracji (legacy `/pl/insights/` i spółka — 200 OK, self-canonical, dosłownie pusty `<title>| RSM Poland</title>`), sitemapa w 100% wskazuje wariant bez `www` (każdy wpis = 301), zero JSON-LD na stronach usług, profil eksperta nie listuje jego artykułów, brak jakiegokolwiek raportu branżowego („raporty" = obowiązkowe sprawozdania z przejrzystości), żywy podcast „Podatkowy GPS" w całości oddany Spotify (zero URL-i na własnej domenie), seria „Tax Alert" porzucona ok. 2015.

### roedl.pl — substancja mimo technikaliów

1 472 URL-e PL na SharePoincie; 77% to treść merytoryczna w klastrach zakodowanych w ścieżce (`/podatek-cit/`, `/ceny-transferowe/` — 72 artykuły w najsilniejszym żywym klastrze), pokrywających się 1:1 z liniami usługowymi. Wzorce warte kopiowania: model **„autor pod tekstem + ekspert jako kontakt"** (byline z tytułem zawodowym i osobny H2 „Kontakt" z osobą, do której się dzwoni), blok Q&A z 10 pytaniami jako H3, sekcja „Co to oznacza dla przedsiębiorców?" w alertach (konsekwencja operacyjna zamiast przedruku), **broszury roczne** („Ceny transferowe" rocznik po roczniku od 2020) i pięć formatów treści (artykuł, broszura, podcast, książka, wydarzenie) wokół tych samych klastrów. Bio ekspertów wzorcowe (rok wpisu na listę, certyfikat MF, staż, publikacje w „Rzeczpospolitej").

Słabości do wykorzystania: **zero danych strukturalnych na całej domenie** (żadnego `Person`, `Article`, `LocalBusiness` przy 6 biurach; autor i data widoczne tylko dla człowieka), profile pod generycznym `<title>` „Profil | Rödl" i `?PersonID=`, 287 URL-i ze spacjami/diakrytykami, rozdwojony klaster aktualności, martwe klastry (koronawirus — 78 URL-i) w sitemapie, literówki w URL-ach i nagłówkach (`due-dilligence`, `dokumetacja`), `meta keywords` wykładające konkurencji listę fraz docelowych. Rödl blokuje w robots.txt boty AI (ClaudeBot, anthropic-ai, CCBot) — wycina się z odpowiedzi generatywnych.

### Wnioski z koszyka doradczo-audytorskiego

1. **To jest właściwy punkt odniesienia dla E-E-A-T, nie SaaS-y.** Piątka z pierwszego koszyka strukturalnie nie może mieć doradców z uprawnieniami — ta trójka ma. Mimo to nikt z ośmiu nie pokazuje recenzji merytorycznej, nie wystawia numeru uprawnień w byline artykułu i nie łączy kompletu: widoczna ekspertyza + `Person` z `hasCredential` + `reviewedBy` + `author.@id`. Luka z TL;DR pkt 1 pozostaje otwarta — ale zamykać ją trzeba szybko, bo RSM jest o jeden krok (numery uprawnień + `@id`) od jej domknięcia.
2. **Nikt nie ma naraz architektury i autorytetu.** GT ma systemowe wiązanie treści z ofertą i słabe on-page; RSM ma najlepsze on-page i E-E-A-T przy fatalnej higienie i 1/3 serwisu w gruzach; Rödl ma substancję i zero warstwy maszynowej. Mentzen, startując z mniejszej skali, może mieć wszystkie trzy warstwy poprawne od pierwszego dnia — to jest realna przewaga małej domeny.
3. **Mechanizmy do przejęcia, których nie było w pierwszym koszyku:** taksonomia bloga = taksonomia usług (GT), cykl na danych własnych z imiennym cytatem (GT), ramka „Kluczowe informacje" + H2-pytania (RSM), model „autor + ekspert kontaktowy" (Rödl), publikacja roczna z hubem na jednym stałym URL-u (poprawiona wersja broszur Rödla).
4. **Higiena archiwum to systemowa słabość całej trójki** (GT 34% nietykane od 2018–19, RSM ~33% zombie, Rödl martwe klastry i hurtowy `lastmod` 2025). Norma dla Mentzena: cykl przeglądu/deindeksacji planowany razem z kalendarzem publikacji, nie po fakcie.

---

## Wnioski przekrojowe — reguły dla skilla SEO

### Architektura

- **Trzymaj trzy warstwy:** krótki pillar ofertowy (1 200–1 500 słów konkretu) w korzeniu domeny → hub/kompendium (5–7 tys. słów ze spisem treści i kotwicami) → klaster satelitów, **jedna wątpliwość = jeden URL**. Wyczerpywanie tematu deleguj do klastra, nie do strony ofertowej (Crido, inFakt).
- **Zepnij taksonomię bloga z taksonomią oferty** (Grant Thornton): kategoria wpisu odwzorowuje drzewo usług, więc linkowanie artykuł ↔ oferta generuje szablon, nie decyzja per tekst. Warunek: katalog usług rozdrobniony do poziomu zagadnienia (GT: 246 stron), żeby link z artykułu trafiał w podusługę dokładnie odpowiadającą problemowi, nie w ogólną ofertę. Jedna taksonomia treści i jeden typ autora od początku — nie trzy równoległe systemy jak u GT.
- **URL pillara niezależny od głębokości nawigacji:** `mentzen.pl/fundacja-rodzinna/`, nawet jeśli w menu siedzi 4 poziomy głęboko (crido.pl/ulga-b-r/).
- **Blog w podkatalogu tej samej domeny.** Rozproszenie na subdomeny działa tylko przy sile wydawcy klasy Infor i wymaga spięcia wspólnym `@id` w JSON-LD; dla kancelarii — podkatalog.
- **Obstawiaj frazy transakcyjno-informacyjne podwójnie:** artykuł blogowy + landing hybrydowy w korzeniu (treść kompendium + formularz po sekcjach + FAQ). To realnie daje 2–3 pozycje na wariantach frazy (infakt.pl/zalozenie-spolki-z-o-o-kompendium-wiedzy).
- **Rozbijaj frazy-parasole na siatkę intencji** (forma opodatkowania × moment × forma prawna × edge case), ale startuj od 2–3 fraz z realną przewagą merytoryczną, nie od całego pionu (mechanizm poradnikprzedsiebiorcy.pl).
- **Druga oś taksonomii `/tematy/<tag>/`** jako strony klastrowe linkowane z każdego artykułu — ale z własnym wstępem i unikalnym meta description, nie boilerplate jak u Infora.
- **Nie mnóż typów treści ponad potrzebę:** 4 osobne CPT blogowe Crido mają sens przy 5 400 URL-ach; przy mniejszej skali wystarczą kategorie. Warto wziąć tylko `hottopic` (landing na zmianę prawa) i `case_study`.
- **Nie zostawiaj pustych stron kategorii** (feed + paginacja jak `/podatki` u poradnikprzedsiebiorcy) — kategoria ma być hubem z definicją, mapą tematu i linkami do filarów.

### Świeżość i aktualizacje

Trzy modele w koszyku — wybór ma konsekwencje:

| Model | Kto | Ocena dla Mentzena |
|---|---|---|
| Ten sam URL, rok w title, zbiorczy refresh klastra | Crido (14.01.2026), inFakt (02.06.2026) | **Domyślny wybór.** Stabilny URL, historia linków, świeży snippet |
| 301 na nowy slug z rokiem + rozbudowa | ifirma (estoński CIT) | Opcja dla dużych przebudów top 10–20 artykułów; bezpieczniejsza alternatywa: zostaw slug, aktualizuj treść i `dateModified`, dodaj sekcję „zmiany w N+1" |
| Nowy artykuł zamiast aktualizacji | Infor (dateModified==datePublished 3/3) | **Nie kopiować** — bez autorytetu domeny Infora to czysta kanibalizacja |

Reguły:
- **Rok w `<title>` i og:title; nigdy w URL, nigdy w H1** (Crido: „Ulga B+R 2026" / `/ulga-b-r/` / „Ulga B+R").
- **Refresh klastra zbiorczo, w kalendarzu redakcyjnym** (start roku podatkowego + po każdej dużej nowelizacji), nie ad hoc. Refresh musi objąć całą treść — u poradnikprzedsiebiorcy nagłówek „...w 2024?" wisi w artykule „2026".
- **Wyświetlaj datę aktualizacji on-page** — „Aktualizacja: DD.MM.RRRR (pierwotnie: DD.MM.RRRR)" + krótki changelog. **Nikt z piątki tego nie robi**: Crido pokazuje datę sprzed 7 lat, inFakt chowa aktualizację w JSON-LD, poradnikprzedsiebiorcy nadpisuje datę publikacji. Rozdzielone `datePublished`/`dateModified` + `<lastmod>` w sitemapie.
- **Publikuj na etapie projektu ustawy** — jedyny mechanizm wyprzedzenia autorytetu domeny (Infor w sierpniu 2026 ma gotowy klaster „podatki 2027"). Doradcy widzą projekty wcześniej niż redakcje — przekuwać to w URL-e z datą. Do tego sekcja „zmiany N+1?" w każdym pillarze (ifirma) i podwójny cykl sezonowy: evergreen wcześnie + news domykający przed terminem (Infor).
- **Nie publikuj szybciej, niż porządkujesz archiwum.** Cała trójka doradczo-audytorska tego nie robi: GT — 34% archiwum nietykane od 2018–19, RSM — ~33% zombie-URL-i po migracji, Rödl — martwe klastry (koronawirus, polski ład) w sitemapie i hurtowo przestawiony `lastmod`. Norma: cykl rewizji lub deindeksacji (np. po 24 miesiącach) w kalendarzu redakcyjnym; przy migracji URL-i zawsze 301, nigdy stare adresy na 200 z self-canonical.

### E-E-A-T — tu jest cała przewaga kancelarii

Stan koszyka pierwotnego (0/5 ma którykolwiek z tych elementów): recenzja merytoryczna, numer uprawnień w byline, `reviewedBy`, `Person` z `hasCredential`. Autorzy: dziennikarz radiowy (inFakt), „księgowa i autorka tekstów" (ifirma), „oprac." redaktora (Infor), podpis przypisany niezgodnie ze specjalizacją (poradnikprzedsiebiorcy), konto agencji SEO `/author/widoczni_seo/` (Crido). Strony autorów Crido i poradnikprzedsiebiorcy są puste (sam listing).

Koszyk doradczo-audytorski podnosi poprzeczkę, ale jej nie zamyka: RSM ma numery uprawnień w profilach i jako jedyny `author.@id`; Rödl ma wzorcowe bio (rok wpisu na listę, certyfikat MF) — ale zero warstwy maszynowej; GT ma tytuły zawodowe i komórkę eksperta przy artykule — ale `author` bez `@id` i bez numerów uprawnień. **Recenzenta i `reviewedBy` nie ma nikt z ośmiu, numeru uprawnień w byline artykułu również** (RSM trzyma go tylko na profilu).

Normy:
- **Każdy artykuł YMYL: „Autor: X · Zweryfikował merytorycznie: Y, doradca podatkowy nr NNNNN · Stan prawny na: DD.MM.RRRR"** + `reviewedBy` w schemacie.
- **Pełne strony autorów jako aktywa SEO:** zdjęcie, bio kwalifikacyjne (nie marketingowe), numer wpisu na listę doradców/radców, specjalizacje, publikacje, LinkedIn; `Person` z `jobTitle`, `hasCredential`, `knowsAbout`, `worksFor`, `sameAs`. Profile z pełną listą publikacji budują topical authority per osoba (Infor: 340 tekstów Bilińskiego; GT domyka pętlę artykuł → ekspert → publikacje eksperta → usługa, RSM i Rödl tej listy nie mają — nie powtarzać). Wzorzec bio: Rödl/RSM (rok wpisu, certyfikaty, staż, dowody publikacji); wzorzec oprawy technicznej: `<title>` z nazwiskiem i tytułem zawodowym, czysty slug (nie `?PersonID=` jak Rödl).
- **`author` w JSON-LD zawsze jako `Person` z `@id` i `url`** wskazującym na profil, identyczny we wszystkich tekstach danej osoby — wzorzec RSM, jedyny poprawny w koszyku ośmiu domen; GT daje gołe `{"@type":"Person","name":"..."}` mimo istniejących stron ekspertów.
- **Boks autora jako moduł konwersji, nie tylko stopka** (GT: „Skontaktuj się" + tytuł zawodowy + mail + telefon) oraz — obok autorstwa, nie zamiast — blok „Kontakt" z ekspertem, do którego się dzwoni (Rödl). Byline daje E-E-A-T, blok kontaktowy daje lead.
- **Podpisz strony ofertowe człowiekiem** — blok „Twój doradca" ze zdjęciem i uprawnieniami; flagowy pillar Crido nie ma żadnego.
- **Sekcja „Podstawa prawna" z konkretem:** ustawa + artykuł + ustęp + link do ISAP + sygnatury interpretacji KIS/orzeczeń NSA. Infor podaje samą ustawę, poradnikprzedsiebiorcy — nic (0 odwołań do prawa w artykule o składce zdrowotnej). To różnica, którą czytają i Google, i modele językowe.
- **Podpis musi odpowiadać realnej specjalizacji** — inaczej strona autora działa przeciwko nam.
- **Odwróć model ekspercki Infora:** zamiast pozyskiwać komentarze (jak oni od Modzelewskiego, KIG), dostarczać je portalom (infor, pit.pl) — linki, cytowania nazwiska, obecność w klastrach konkurenta; wzorzec widoczny na pit.pl (radca z kancelarii Graś i Wspólnicy jako źródło merytoryczne portalu).

### Dane strukturalne

- **Jeden spójny `@graph` JSON-LD na stronę** (wzorzec implementacyjny: Infor; antywzorzec: trzy sprzeczne bloki inFaktu). Zestaw: `Article` (z `wordCount`, `articleSection`, `inLanguage`), `WebPage`, `BreadcrumbList`, `WebSite` z `SearchAction`, `Organization` (NAP, godziny, `sameAs` **z LinkedInem** — Crido go nie ma, a to główny kanał B2B), `Person` z pełnymi atrybutami.
- **`FAQPage`: nie wdrażaj nowego markupu.** FAQ rich results wyłączone 2026-05-07 ([SEJ, 2026-05-10](https://www.searchenginejournal.com/google-drops-faq-rich-results-from-search/574429/)) — szczegóły i tabela decyzyjna: technical-seo.md, content-onpage.md §5.4, ai-search.md. Widoczne sekcje FAQ zostają (UX, frazy z PAA, cytowalność w AI); istniejący markup nie szkodzi. Braku `FAQPage` u Crido i Infora nie traktuj jako przewagi do wzięcia, a mikrodanego akordeonu ifirmy nie kopiuj.
- `meta robots`: `max-snippet:-1, max-image-preview:large` (ifirma) — warunek pełnej ekspozycji w snippetach i AI Overviews.
- Kalkulatory: rozważyć `SoftwareApplication`/`WebApplication` — nikt w koszyku tego nie ma [do weryfikacji: realny wpływ na SERP].
- **Nie kopiować:** `AggregateRating` z głosowania czytelników na artykułach (inFakt — self-serving markup, ryzyko manual action); `NewsArticle` dla evergreenów bez statusu wydawcy (Infor); duplikaty typu Article+NewsArticle; mikrodane rozproszone w szablonie jako jedyna warstwa (poradnikprzedsiebiorcy).
- Higiena: walidacja dat (Crido ma `datePublished: 1790-11-17`), jeden H1 na stronę (kalkulator Crido ma dwa, gofin ma H1=nazwa serwisu), `Disallow` dla tagów i parametrów śledzących (wzorzec inFakt), canonical/noindex na filtrowanych archiwach `?categories=` (ryzyko duplikacji u Crido).

### Format artykułu, który rankuje w tej niszy

- **Liczby przed wykładnią:** tabele porównawcze z policzonymi kwotami, „Przykład 1–N" na konkretnych liczbach, progi opłacalności. To jest to, co Google cytuje w snippetach i co wyciąga AI Overviews. Infor — mimo pozycji — ma zero tabel; ifirma i poradnikprzedsiebiorcy mają i to widać w ich formatach topowych.
- **H2 jako pytania językiem problemu użytkownika** („Ile kosztuje...?", „Pułapka pierwszej faktury"), nie nazwą przepisu; jeden H3 = jedna decyzja czytelnika; procedury jako numerowane kroki „1…N".
- **Wzorzec trzech sformułowań:** title (exact match + rok) ≠ H1 (pytanie long tail) ≠ pierwszy H2 (powtórka exact match) — zero kosztu, trzy szanse dopasowania (poradnikprzedsiebiorcy). Rozjazd title/H1 ma być decyzją z `headline` w schemacie zgodnym z H1 (u Infora wyszedł z zaniedbania).
- Spis treści z kotwicami; sekcja autokwalifikacji na pillarach („Czy X jest dla Ciebie?" + twarde warunki — Crido); FAQ 4–6 pytań zdjętych z PAA na końcu; ramki „Ważne"; sekcja „zmiany N+1?".
- **Ramka „Kluczowe informacje" (3 punkty streszczenia) na samej górze artykułu** (RSM) — najtańszy materiał do wyciągnięcia przez AI Overviews i featured snippet. W alertach o zmianach prawa obowiązkowa sekcja **„Co to oznacza dla przedsiębiorców?"** (Rödl) — konsekwencja operacyjna zamiast streszczenia przepisu.
- Nazwy plików obrazów z frazą tematyczną i ofertową (RSM: `home-office-zaklad-podatkowy-doradztwo-podatkowe-dla-firm.jpg`).
- **Nie:** keyword stuffing w każdym nagłówku (inFakt — działa, ale bije w markę; Mentzen sprzedaje głosem), pełne zdania w `<h2>` jako styl (Crido), treść „przepisana z ustawy" bez stanowiska — interpretacja „co to znaczy dla ciebie" to przewaga, której portale strukturalnie nie dowiozą.

### Narzędzia i lead magnety

- **Kalkulatory jako osobny, trwały silnik ruchu i linków.** Priorytety wynikające z luk koszyka: „JDG czy spółka (z estońskim CIT)", „wypłata ze spółki: dywidenda vs wynagrodzenie vs świadczenia", „składka zdrowotna", „opodatkowanie fundacji rodzinnej", „koszt przekształcenia JDG→sp. z o.o.".
- **Wynik bez bramki** (Crido): liczba za darmo, formularz pod wynikiem. Kalkulator z obudową tekstową kilku tysięcy słów jako typ strony (poradnikprzedsiebiorcy) + linkowanie do narzędzi z każdego artykułu klastra (mechanizm stopki Infora, ale kuratorowany tematycznie).
- **Lead magnet jako hub klastra:** ebook/raport + dynamiczna lista artykułów klastra pod nim — strona latami zbiera świeże linki wewnętrzne (poradnikprzedsiebiorcy `/-formy-opodatkowania`).
- **Co najmniej jeden cykl na danych własnych** (Grant Thornton: miesięczny raport o ofertach pracy — ~230 słów prozy: konkretna liczba w leadzie + metodologia + imienny cytat + własna nazwa marki + własny URL kategorii). Jedyny mechanizm w całym koszyku generujący linki bez outreachu — media cytują, bo nie mają alternatywnego źródła. Kancelaria ma dane, których nie ma nikt: rozkład rodzajów spraw, czasy uzyskania interpretacji, statystyka rozstrzygnięć. Publikację roczną prowadzić na **jednym stałym URL-u huba z archiwum jako podstronami** (poprawiona wersja broszur Rödla, które rozpraszają autorytet na osobne roczniki) — i nie porzucać serii (RSM: „Tax Alert" martwy od ~2015; lepiej rzadszy rytm i dotrzymany).
- Kalkulator kancelarii ma kończyć się rekomendacją rozmowy z doradcą, nie samą liczbą — narzędzie jest kwalifikatorem leada, nie końcem ścieżki.

### Konwersja

- **Dwustronna pętla blog ↔ oferta:** kontekstowe linki z artykułów do stron usługowych + blok wpisów na stronach usługowych + FAQ z frazami long-tail na landingach (ifirma). Asymetria inFaktu (oferta nie linkuje do bloga, landing bez treści) to luka, nie wzorzec.
- **CTA kontekstowe w miejscu bólu** („to skomplikowane → oto kto to zrobi") — 1:1 od poradnikprzedsiebiorcy/inFaktu, ale **jedno mocne CTA zamiast pięciu banerów**: agresywność SaaS-owa (floating WhatsApp, 5 CTA na jednej stronie ofertowej) obniża postrzeganą jakość usługi premium. CTA kancelarii: konsultacja / wycena / kontakt z konkretnym doradcą.
- **Trzy progi zaangażowania** (Crido): newsletter → narzędzie/raport → formularz kwalifikujący (firma + stanowisko + dział — mniej leadów, ale od decydentów, routowalnych do zespołu).
- Case study osadzone bezpośrednio w pillarze; landing na każdą dużą zmianę prawa (wzorzec `hottopic`: co się zmienia → od kiedy → kogo dotyczy → usługi z „jak działamy"/„efekty") — polski rynek dostarcza kilka takich fal rocznie (KSeF, zmiany stawek, formy opodatkowania).
- Parametry atrybucji na linkach konwersyjnych + `Disallow` w robots.txt (inFakt) — mierzalność bez śmiecenia indeksu.

### Local SEO — jedna lokalizacja, pole prawie puste

Crido, Infor, poradnikprzedsiebiorcy: nie grają wcale. ifirma: 11 stron miastowych ze zmienionym tylko H1, bez `LocalBusiness`, z wrocławskim adresem. inFakt: ~445 programatycznych stron miejscowości (do wsi kilkusetosobowych włącznie) bez schema — dług, nie wzorzec. W koszyku doradczo-audytorskim: Rödl ma 6 biur i zero `LocalBusiness`; RSM ma 4 biura i zero schemy; jedynie Grant Thornton wstrzykuje na **każdą** stronę blok `Organization` + 7×`AccountingService` z pełnym `PostalAddress` per biuro — sygnał lokalny rozprowadzony po całym serwisie.

**Stan faktyczny Mentzena (potwierdzony 2026-08-28): jedna stacjonarna lokalizacja — Toruń, Grudziądzka 110-114/101.** Klienci mogą przyjść na konsultację stacjonarnie, ale większość korzysta z usług zdalnie. Oddziały Warszawa/Gdańsk/Poznań **nie istnieją stacjonarnie** — nie wolno budować pod nie stron „oddziałów" (to byłyby doorway pages klasy ifirma/inFakt, ryzyko algorytmiczne i wizerunkowe w YMYL).

Norma: **jedna realna lokalizacja = jedna mocna strona lokalna.** Strona biura w Toruniu z prawdziwą treścią, zespołem i NAP + `LegalService`/`LocalBusiness` spójne z Profilem Firmy w Google; wzorzec dystrybucji sygnału od GT (schema biura w szablonie całego serwisu, nie tylko na stronie kontaktu). Frazy „doradca podatkowy [inne miasto]" obsługiwać uczciwie: treścią o obsłudze zdalnej / ogólnopolskiej, nie fikcyjnymi adresami. Uwaga konkurencyjna: w Toruniu Grant Thornton ma dwa biura, w tym **Grudziądzka 46-48** — ta sama ulica co Mentzen; lokalny SERP toruński nie jest pusty.

### AI Overviews / crawlery

- Nie blokować crawlerów AI ani nietypowych UA (gofin zwraca 403 — wycina się z AI Overviews; poradnikprzedsiebiorcy zamyka PerplexityBotowi tylko `/categories/search`; Rödl blokuje w robots.txt ClaudeBot, anthropic-ai i CCBot — świadomie rezygnuje z obecności w odpowiedziach generatywnych). Koszt rośnie z każdym kwartałem.
- `max-snippet:-1`, `max-image-preview:large`, format „liczby + kroki + FAQ" — to jest to, co AI Overviews cytują.
- Uwaga operacyjna do badań konkurencji: crido.pl zwraca 403 na WebFetch, gofin.pl bywa blokujący dla nietypowych UA [do weryfikacji — przy kontroli 2026-08-28 gofin odpowiadał 200 na curl] — analizować curlem z przeglądarkowym `User-Agent` i `Accept-Language: pl-PL`.

---

## Zastosowanie dla mentzen.pl — priorytety

1. **Warstwa E-E-A-T (najwyższy zwrot, zerowa konkurencja):** byline z uprawnieniami + „Zweryfikował merytorycznie: ..., doradca podatkowy nr ..., stan prawny na ..." + pełne strony autorów + `Person` z `hasCredential`/`sameAs` + `reviewedBy` + `author.@id` do profilu. Nikt z ośmiu tego kompletu nie ma; SaaS-y i wydawcy strukturalnie mieć nie mogą, a koszyk doradczo-audytorski może — RSM jest najbliżej (numery uprawnień + `@id`) — więc tu liczy się tempo, nie sama decyzja.
2. **Klaster „podatki spółki z o.o."** — pillar + 15–25 satelitów (estoński CIT, ukryte zyski, dywidenda, wynagrodzenie zarządu, składka zdrowotna wspólnika, przekształcenie JDG→sp. z o.o.). Luka: ifirma nie ma pillara dla spółek, inFakt ma treść bez autorytetu merytorycznego. Rdzeń kompetencji Mentzena.
3. **Klaster „fundacja rodzinna"** — u Infora temat cienki (70 tekstów, głównie newsy z cudzymi komentarzami, bez kompendiów), a to temat najgłębszej praktyki i najwyższej wartości leada Mentzena. Pillar + satelity + kalkulator + tabele + widoczne FAQ (bez markupu `FAQPage`).
4. **Schema w szablonie (jednorazowy koszt):** spójny `@graph` — Article, BreadcrumbList, Person, Organization z LinkedInem (bez `FAQPage` — rich result wycofany 2026-05-07, zob. technical-seo.md); `max-snippet:-1`/`max-image-preview:large`; walidacja dat; audyt canonical/noindex.
5. **Format top artykułów:** tabele z kwotami, przykłady liczbowe, progi opłacalności, H2-pytania, kroki, FAQ z PAA, „Podstawa prawna" z ISAP i sygnaturami, sekcja „zmiany N+1".
6. **Kalkulator flagowy „JDG czy spółka (z estońskim CIT)"** bez bramki, z rekomendacją konsultacji — magnes linkowy i kwalifikator leada.
7. **Proces świeżości:** roczny/kwartalny zbiorczy refresh klastrów w kalendarzu redakcyjnym, rok w title (nie w URL/H1), widoczna data aktualizacji z changelogiem, `<lastmod>` w sitemapach, publikacje na etapie projektów ustaw + landingi `hottopic` na fale legislacyjne.
8. **Pętla konwersji:** przebudowa landingów usługowych (treść + FAQ + schema + linki w obie strony z blogiem), autokwalifikacja na pillarach, formularz kwalifikujący, trzy progi zaangażowania, jedno kontekstowe CTA.
9. **Local SEO dla jedynej realnej lokalizacji (biuro w Toruniu, Grudziądzka 110-114/101):** strona biura z zespołem i NAP + `LegalService` + Profil Firmy w Google, schema rozprowadzona w szablonie (wzorzec GT). Frazy innych miast — treścią o obsłudze zdalnej, **bez** fikcyjnych stron oddziałów (Warszawa/Gdańsk/Poznań nie istnieją stacjonarnie).
10. **PR ekspercki:** komentarze dla infor/pit.pl/gofin (linki + cytowania nazwiska), zamiast konkurowania z nimi wolumenem — 3–5 tekstów dziennie Infora i 26,5 tys. artykułów poradnikaprzedsiebiorcy są nie do dogonienia i nie trzeba ich doganiać.
11. **Jeden cykl na danych własnych kancelarii** (wzorzec GT: liczba w leadzie + metodologia + imienny cytat + stała częstotliwość + własna marka i URL huba) — np. statystyka spraw, czasy interpretacji, rozstrzygnięcia. Krótki (raport GT to ~230 słów prozy), ale regularny; linki przychodzą bez outreachu.

## Braki tej syntezy (do osobnych zadań)

- Profil linków przychodzących, realny ruch i pozycje wszystkich domen [do weryfikacji — Ahrefs/Senuto].
- Core Web Vitals konkurencji vs mentzen.pl (300 KB HTML i 242 linki u poradnikaprzedsiebiorcy sugerują pole do wygranej wydajnością) [do weryfikacji — PageSpeed].
- Obecność koszyka w AI Overviews [do weryfikacji].
- ~~Koszyk uzupełniający wskazany w analizie Crido: grantthornton.pl, rsmpoland.pl, roedl.pl~~ — zbadany 2026-08-28, zob. sekcję „Konkurencja doradczo-audytorska". Hipoteza potwierdzona częściowo: E-E-A-T merytoryczne (RSM, Rödl) mocniejsze niż u piątki, ale warstwa maszynowa i higiena techniczna słabe u całej trójki. Pozostaje [do weryfikacji]: ruch, pozycje i profil linków tej trójki (uwaga: `Crawl-delay: 10` dla Ahrefs/Semrush u Rödla może zaniżać dane).
- Stan obecny mentzen.pl (schema, meta robots, daty, strony autorów) — punkt zerowy do audytu przed wdrożeniem powyższych reguł.
