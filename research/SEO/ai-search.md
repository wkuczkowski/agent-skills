# AI search / GEO: widoczność w AI Overviews, ChatGPT i Perplexity

Research pod skill SEO dla mentzen.pl (kancelaria prawo/podatki/księgowość, B2B, WordPress, rynek polski, YMYL). Stan na 2026-08-28. Twierdzenia bez potwierdzenia oznaczone `[do weryfikacji]`.

---

## TL;DR

1. **Fundamentem widoczności w AI jest klasyczne SEO.** AI Overviews i AI Mode działają na indeksie i systemach rankingowych Google; jedyny warunek wejścia to indeksacja i kwalifikowalność do snippetu ([Google, AI Features and Your Website](https://developers.google.com/search/docs/appearance/ai-features), 2025-12-10). Dyrektywa `nosnippet`/`max-snippet:0` wyklucza z AIO — sprawdź, czy nikt jej nie wstawił "na wszelki wypadek".
2. **Najlepiej udokumentowana dźwignia cytowalności to konkret w treści**: statystyki, cytaty autorytetów, źródła — poprawa widoczności w silnikach generatywnych o 30–40% w badaniu Princeton ([arXiv 2311.09735](https://arxiv.org/pdf/2311.09735)). Keyword stuffing nie działa.
3. **Większość cytowań i wzmianek AI pochodzi ze stron trzecich, nie z domeny marki** (tylko 2,8% cytowanych źródeł to strony marek — [SEJ](https://www.searchenginejournal.com/ai-tools-recommend-brands-but-cite-other-sites-data-shows/587160/); wzmianki 6,5× częściej z third-party — [SEL guide](https://searchengineland.com/guide/ai-citations-vs-ai-mentions-data)). PR branżowy, rankingi i zestawienia ważą więcej niż kolejny wpis blogowy, gdy celem jest bycie *polecanym*.
4. **Platformy się nie pokrywają**: z top 50 cytowanych domen tylko 7 wspólnych dla AIO, ChatGPT i Perplexity ([Ahrefs, 2025-06-12](https://ahrefs.com/blog/top-mentioned-sources-are-not-shared-across-ai-assistants/)). AIO ~ ranking Google; ChatGPT ~ media/licencje; Perplexity ~ najbliżej klasycznego SERP-a.
5. **Zapytania informacyjne tracą kliknięcia masowo**: 68% wyszukiwań w US bez kliknięcia ([SEL, 2026-06-09](https://searchengineland.com/google-zero-click-searches-2026-study-479717)), CTR pozycji 1 przy AIO −58% ([Ahrefs, 2026-02-04](https://ahrefs.com/blog/ai-overviews-reduce-clicks-update/)); 99,9% fraz wywołujących AIO ma intencję informacyjną (dane Ahrefs z kursu AEO). Cel treści informacyjnych przestaw z "ruch" na "wzmianka + cytowanie"; konwersje opieraj na zapytaniach brandowych, lokalnych, usługowych i narzędziowych (kalkulatory, wzory). Ruch na treściach newsowych o zmianach prawa broni Google Discover/News — kanały na razie nietknięte przez syntezę AI (sekcja 2).
6. **Świeżość jest silnym sygnałem**: 89,7% najczęściej cytowanych przez ChatGPT stron zaktualizowano w 2025 r., 76% w ostatnich 30 dniach (dane Ahrefs). Sama podmiana daty publikacji nie działa i jest wykrywalna — aktualizacja musi zmieniać treść.
7. **Nie inwestuj w `llms.txt`** (97% plików nigdy nie pobranych, boty AI nie szukają go z własnej inicjatywy — [Ahrefs, 2026-06-15](https://ahrefs.com/blog/llmstxt-study/); Google: "Google Search itself doesn't use them") **ani w schemę "pod AI"** (eksperyment kontrolowany bez efektu — [SEJ, 2026-05-16](https://www.searchenginejournal.com/serp-faq-removal-new-data-challenge-schemas-ai-search-value/574993/)). Schema zostaje jako infrastruktura encji (`Organization`, `Person`, `Article`), nie jako dźwignia cytowań.
8. **YMYL to warunek brzegowy**: treści prawno-podatkowe bez podpisanego autora z uprawnieniami (nr wpisu KIDP/OIRP), daty aktualizacji i stanu prawnego nie mają czego szukać ani w Google, ani w AI ([Search Quality Rater Guidelines](https://guidelines.raterhub.com/searchqualityevaluatorguidelines.pdf), 2025-09-11).
9. **Pomiar wymaga trzech warstw naraz**: GSC raport Generative AI (wyświetlenia, bez kliknięć; [Google, 2026-06-03](https://developers.google.com/search/blog/2026/06/gen-ai-performance-reports)), własna grupa kanałów AI w GA4 (zaniżona), logi serwera (boty AI). Czwarta, najtańsza: pytanie "skąd o nas wiesz" w formularzu kontaktowym.
10. **Cytowania są probabilistyczne i niestabilne** (ponad 45% cytowań AIO zmienia się przy odświeżeniu, średnio co 2 dni — dane Ahrefs). Raportuj widoczność jako udział w wielu powtórzeniach zapytań, nigdy jako pojedynczy zrzut ekranu.

---

## 1. Mechanika — z niej wynikają wszystkie taktyki

**RAG + query fan-out.** Google opisuje funkcje generatywne jako nadbudowę nad Search: retrieval-augmented generation na istniejącym indeksie ([AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide), 2026-07-10). Zapytanie użytkownika jest rozbijane na wiele podzapytań (średnio 9–11 na prompt, skrajnie 28; deep research ChatGPT do 420 wyszukań — badanie Seer Interactive/Nectiv cytowane w kursie AEO Ahrefs), każde odpytuje indeks osobno, wyniki są sklejane w odpowiedź ([SEL, query fan-out guide](https://searchengineland.com/guide/query-fan-out)).

**Chunking.** Systemy tną wybrane strony na samowystarczalne fragmenty (akapit, wiersz tabeli, punkt listy) i syntezują odpowiedź z fragmentów, nie z całych stron — tak to opisuje Szymon Parzych (Vestigio) na webinarze Senuto ([youtube.com/watch?v=MKwi0qmVSS8](https://www.youtube.com/watch?v=MKwi0qmVSS8), 2025-09-30).

**Proces nie jest deterministyczny.** To samo pytanie da różne zestawy źródeł; personalizacja po historii dodatkowo różnicuje wyniki. Wg Samo Cerara (kurs AEO, kanał Ahrefs, [youtube.com/watch?v=uza9GX0E2mw](https://www.youtube.com/watch?v=uza9GX0E2mw)): nie istnieje "pozycja", istnieje rozkład prawdopodobieństwa.

**Dwie osobne gry**: trening modelu (statyczny snapshot sieci — wpływasz przez masową, spójną obecność nazwy) i retrieval na żywo (wpływasz przez klasyczne SEO). Wg Cerara ponad 95% zapytań fan-out ma zerowy wolumen wyszukiwań — nie traktuj ich jako nowej listy keywordów, tylko jako podgląd wątków, które model uważa za istotne.

**Konsekwencje normatywne:**
- Buduj strony wyczerpujące temat wszerz (jedna solidna strona "Estoński CIT": warunki, wyłączenia, terminy, koszty, kiedy się nie opłaca), nie serię cienkich wpisów na warianty frazy — pod fan-out.
- Pisz sekcje atomowe: każda sekcja H2 musi bronić się wyrwana z kontekstu, bo model przetnie tekst tam, gdzie chce. Test Cerara: przeczytaj sekcję w oderwaniu; jeśli bez kontekstu nie ma sensu, przepisz.
- Nie raportuj "jesteśmy w AIO na frazę X" jako stanu trwałego.

## 2. Skala zjawiska i co z niej wynika

- 68,01% wyszukiwań Google w US (I–IV 2026) kończy się bez kliknięcia; w 2024: 60,45% (SparkToro/Similarweb — [SEL, 2026-06-09](https://searchengineland.com/google-zero-click-searches-2026-study-479717)). Klikają nadal: zapytania brandowe, lokalne, transakcyjne.
- CTR pozycji 1 przy obecności AIO: −58% (XII 2023 vs XII 2025, 300 tys. fraz); efekt sięga całej pierwszej dziesiątki (poz. 10: −19,4%) — [Ahrefs, 2026-02-04](https://ahrefs.com/blog/ai-overviews-reduce-clicks-update/). Niezależny randomizowany eksperyment terenowy (Agarwal & Sen, preprint SSRN, IV 2026): −38% kliknięć wychodzących na zapytaniach z AIO, zero-click 54%→72%, satysfakcja użytkowników bez zmian ([SEJ](https://www.searchenginejournal.com/ai-overviews-cut-organic-clicks-38-field-study-finds/573145/)). Przyjmij roboczo: na frazach informacyjnych z AIO tracisz 40–60% kliknięć.
- **Dane polskie** (wg Parzycha, webinar Senuto, na bazie audytów i raportu Senuto — auto-transkrypcja, liczby zweryfikuj w raporcie Senuto przed cytowaniem na zewnątrz): ~24% fraz wywołuje blok AIO niezależnie od pozycji; spadki kliknięć 12–18% dla większości domen; status źródła w AIO odzyskuje 20–40% utraconego ruchu; nawet najczęściej cytowane polskie domeny mają status źródła dla zaledwie 4–10% swoich fraz — jest gdzie się rozpychać (blok AIO ma zwykle 6–8 źródeł z ~6 domen).
- **Efekt "paszczy krokodyla" w GSC** (wg Parzycha): kliknięcia spadają, wyświetlenia rosną, bo GSC liczy każdy link na SERP osobno (wynik organiczny + link w AIO + panel boczny = 3 wyświetlenia). Nie interpretuj rosnących wyświetleń jako rosnącego popytu.
- LLM-y nie zastąpią utraconego ruchu: tylko 6% odpowiedzi ChatGPT zawiera linki zewnętrzne, średnia dla chatbotów 14% (dane przywołane na webinarze Senuto). Ruch z LLM to dziś ~0,1–0,25% całości (Ahrefs), ale konwertuje wielokrotnie lepiej (Ahrefs deklaruje 23×; próbki wąskie, `[do weryfikacji dla B2B prawniczego]`). Kanał addytywny, nie substytut.
- Zachowanie kupujących B2B (Semrush, 622 respondentów, III–IV 2026 — [semrush.com/blog/how-ai-shapes-b2b-buying](https://www.semrush.com/blog/how-ai-shapes-b2b-buying/)): 92% deklaruje, że AI ukształtowało ich shortlistę dostawców; 66% regularnie używa AI do researchu dostawców; usługi finansowe i prawne badane przez AI przez 25%. **Tylko 7% zwraca uwagę na dostawcę przez rozpoznawalność nazwy** — wygrywa jasny opis "dla kogo, jaki problem, jaki zakres" (53% reaguje na dopasowanie do use case, 50% na szczegółowy opis).

**Google Discover i News — kanał obronny wobec zero-click.** Wg Parzycha (webinar Senuto, [youtube.com/watch?v=MKwi0qmVSS8](https://www.youtube.com/watch?v=MKwi0qmVSS8)) dla witryn, których walutą jest ruch informacyjny, sam status źródła w AIO nie wystarczy (20–40% odzysku × 20–30% realnego wskaźnika cytowań) — rekomenduje dywersyfikację, z Discover i News na czele: **na razie nietknięte przez syntezę AI, potrafią dać kilkukrotnie większy ruch niż Search**. Zbieżny case Exposure Ninja ([youtube.com/watch?v=vrGLaJOAKas](https://www.youtube.com/watch?v=vrGLaJOAKas), 2025-10-17): sezonowe poradniki publikowane przed szczytem popytu trafiały równolegle do cytowań AI i do Discover; jeden materiał z Discover dał **+940% ruchu w szczycie sezonu**. Dla reaktywnego bloga o zmianach prawa (nowelizacje, KSeF, terminy) to naturalne dopasowanie — świeża treść newsowa z datą, czyli dokładnie ta, której zero-click zabiera najwięcej kliknięć. Zastrzeżenie: Discover jest niesterowalny i zmienny — Google wprost opisuje ten ruch jako mniej przewidywalny niż Search ([Google, Discover and your website](https://developers.google.com/search/docs/appearance/google-discover)); traktuj go jako dźwignię dodatkową, nie podstawę prognoz ruchu.

## 3. Co realnie zwiększa szanse na cytowanie

### 3.1. Konkret faktograficzny (najmocniejszy dowód)

Badanie GEO (Aggarwal i in., Princeton, [arXiv 2311.09735](https://arxiv.org/pdf/2311.09735)): dodanie cytowań źródeł, cytatów autorytetów i statystyk poprawia widoczność w silnikach generatywnych o 30–40%; poprawa płynności 15–30%; keyword stuffing — zero. **Norma: każdy istotny akapit ma zaczep faktograficzny** — liczbę, datę, kwotę, sygnaturę, przepis. Dla kancelarii naturalne: art. ustawy, interpretacja KIS, wyrok NSA, próg kwotowy.

Zbieżny wniosek z eksperymentu Ahrefs z fikcyjną marką (za Cerarem): Gemini i Perplexity powtarzały rozsianą dezinformację w 37–39% odpowiedzi; ChatGPT poniżej 7%, bo w 84% przypadków cytował oficjalne FAQ marki. **Gdy model wybiera między prawdą ogólnikową a fikcją konkretną, wybiera fikcję konkretną** — wypełnij każdą lukę informacyjną o firmie treścią oficjalną i konkretną (skład zespołu, numery wpisów, specjalizacje, model rozliczeń).

### 3.2. Struktura: answer-first, atomowość, formaty

Zgodne zalecenia z wielu źródeł (Ahrefs, Senuto/Vestigio, komentarz SEJ), ale **bez kontrolowanego eksperymentu na cytowalność** `[do weryfikacji]`:

- **BLUF / odwrócona piramida**: nagłówek H2/H3 jak realne pytanie klienta → bezpośrednia odpowiedź w 1–2 pierwszych zdaniach → dopiero potem podstawa prawna, dane, zastrzeżenia. Wstęp pod H1 jako pigułka, nie literackie wprowadzenie (wg Parzycha są przesłanki, że roboty na podstawie pierwszych akapitów decydują, czy czytać resztę).
- **Formaty chętnie cytowane** (dane z audytów Vestigio + Surfer: 78% odpowiedzi AIO zawiera listę; Ahrefs: 43,8% cytowań ChatGPT to zestawienia/porównania): listy, rankingi, tabele, instrukcje krok po kroku, FAQ. Tabele — warunek: **każdy wiersz samodzielnym faktem** ("Ryczałt 8,5%: usługi X, limit 2 mln EUR, brak kosztów"), bo model wyciąga jeden wiersz.
- **Język**: zdania oznajmujące bez asekuracji, konkretne liczby zamiast "znacząco", encje zamiast zaimków ("to", "tamto" nie działają w wyrwanym fragmencie), terminologia fachowa z krótkim wyjaśnieniem przy pierwszym użyciu, trójki podmiot–orzeczenie–dopełnienie.
- **Długość nie koreluje z cytowaniem** (Ahrefs, 174 tys. stron: korelacja 0,04; 53,4% cytowanych stron poniżej 1000 słów). Nie pisz 3000 słów dla objętości. Google wprost: nie ma potrzeby cięcia treści na "chunki pod AI" ([AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide)).

### 3.3. Świeżość

89,7% najczęściej cytowanych przez ChatGPT stron zaktualizowano w 2025 r., 76% w ostatnich 30 dniach; treść cytowana przez AI średnio o 25,7% świeższa niż top klasycznego rankingu (dane Ahrefs, kurs AEO). Wg Parzycha wskaźnik cytowań ma fazy życia: świeża treść cytowana najrzadziej → wzrost → złoty środek → śmierć (w prawie/podatkach faza śmierci przychodzi z nowelizacją, nie po latach). **Obaj niezależnie potwierdzają: sama podmiana daty publikacji nie działa** — aktualizacja musi wnosić nowe dane, przykłady, stan prawny. Najszybsza droga do widoczności wg Cerara: odświeżanie "sleeper pages" — stron z linkami i historią, które spadły (jeśli strona nigdy nie miała linków, aktualizacja nie pomoże — problem jest gdzie indziej).

### 3.4. Obecność poza własną domeną

- Tylko 2,8% z 1 851 cytowanych źródeł (AI Mode, ChatGPT, Perplexity) to strony marek; wzmianki 6,5× częściej dzięki stronom trzecim; 81% cytowań w listicle'ach ze źródeł trzecich ([SEJ](https://www.searchenginejournal.com/ai-tools-recommend-brands-but-cite-other-sites-data-shows/587160/), [SEL guide](https://searchengineland.com/guide/ai-citations-vs-ai-mentions-data)).
- Najsilniejsza korelacja z widocznością w AIO w badaniu 75 tys. marek: **branded web mentions, 0,664** — wyżej niż backlinki, DR i domeny odsyłające; wzmianka na mocno linkowanej stronie: 0,7 (dane Ahrefs, za Cerarem). **Wzmianka bez linku jest pełnowartościowym celem** — do klasycznego rankingu nie przekazuje sygnału, do widoczności w AI ma wartość samodzielną. Prowadź te dwa cele osobno; mniej niż 30% odpowiedzi AI jednocześnie wzmiankuje i cytuje tę samą markę, więc mierz wzmianki i cytowania osobno.
- Taktyka Parzycha: sprawdź, jakie domeny są źródłami AIO dla twoich fraz (lista top 500 z raportu Senuto), i publikuj artykuły gościnne na tych tematycznie powiązanych — link + widoczność własnej treści na chętnie cytowanej domenie; część portali pozwala edytować już cytowany artykuł i dopisać wzmiankę.
- **Kolejność naprawy błędnej informacji o marce** (Cerar): najpierw opublikuj u siebie treść wprost jej zaprzeczającą, potem występuj do wydawcy o korektę — im szybciej, tym mniej czasu model ma na nauczenie się błędu.
- **Unikaj kupowania/wymuszania masowych wzmianek**: Google klasyfikuje "seeking inauthentic mentions" jako nieautentyczne, a skalowana produkcja treści łamie polityki spamowe ([AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide)).

### 3.5. E-E-A-T / YMYL

Usługi prawne i podatkowe to YMYL — Google przykłada tu większą wagę do E-E-A-T ([Rater Guidelines](https://guidelines.raterhub.com/searchqualityevaluatorguidelines.pdf), wersja 2025-09-11, z nowymi przykładami dot. AI Overviews). Norma: autor z imienia i nazwiska przy H1, biogram z numerem wpisu (KIDP/OIRP/NRA), specjalizacją, publikacjami — **nie "pasjonat prawa gospodarczego"** (wg Parzycha biogramy-proza nie niosą informacji o kompetencji); dedykowane strony autorów z listingiem tekstów (pełnią też rolę hubów linkowania wewnętrznego); jawny stan prawny "na dzień DD.MM.RRRR"; treść AI zweryfikowana przez eksperta przed publikacją.

## 4. Różnice między platformami

Z top 50 najczęściej cytowanych domen tylko 7 wspólnych dla AIO, ChatGPT i Perplexity — 86% rozbieżności ([Ahrefs, 2025-06-12](https://ahrefs.com/blog/top-mentioned-sources-are-not-shared-across-ai-assistants/), próbka: ~76,7 mln AIO, ~957 tys. promptów ChatGPT, ~953 tys. Perplexity).

| Platforma | Profil źródeł | Jak grać |
|---|---|---|
| AI Overviews | uznane serwisy (zdrowie, finanse, encyklopedie), własności Google, YouTube (~5,6% cytowań), Reddit | klasyczny ranking Google + YouTube |
| AI Mode | tylko 13,7% pokrycia cytowań z AIO mimo 86% podobieństwa odpowiedzi; YouTube dominuje, Quora, social | jak AIO, ale nie zakładaj przenoszenia |
| ChatGPT | wydawcy i media (mediana DR 90), umowy licencyjne OpenAI; pokrycie z top 10 Google tylko ~8–10% | wzmianki w mediach o wysokim autorytecie |
| Perplexity | najbliżej klasycznego SERP-a; eksponuje źródła najmocniej; dla B2B: LinkedIn, G2 | najszybszy zwrot z istniejących pozycji; wg Parzycha najlepszy cel do walki o status źródła |
| Claude | UGC 4× częściej niż konkurenci, mniejsze wydawnictwa (SEL guide) | — |

**Sprzeczność w danych o pokryciu z rankingiem Google** — nie cytuj żadnej pojedynczej liczby jako faktu `[do weryfikacji]`: Ahrefs (long-tail) daje Perplexity 28,6% pokrycia z top 10, Semrush — 91% pokrycia domen; dla AIO od 38% do 86% zależnie od badania i daty ([Ahrefs](https://ahrefs.com/blog/ai-search-overlap/), [Semrush](https://www.semrush.com/blog/ai-mode-comparison-study/), [SEJ](https://www.searchenginejournal.com/new-data-finds-gap-between-google-rankings-and-llm-citations/561492/)). Bezpieczna synteza, zgodna kierunkowo we wszystkich badaniach: **AIO mocno sprzężone z rankingiem organicznym, AI Mode słabiej, ChatGPT/Gemini/Copilot najsłabiej**. Dla Polski raport Senuto: ponad 60% cytowań AIO ze stron z top 10, średnia pozycja cytowanego źródła 6–7, 21% spoza top 100 (za Parzychem).

**Dostępność w PL**: Google deklaruje AIO w 200+ krajach i 40+ językach; komunikat o ekspansji AI Mode nie wymienia Polski z nazwy ([blog.google, 2025-10-07](https://blog.google/products-and-platforms/products/search/ai-mode-expands-languages-locations/)). Zweryfikuj empirycznie w GSC (raport Generative AI, Country = Poland), nie na doniesieniach agencji. `[do weryfikacji]`

**Wszystkie dane platformowe pochodzą z rynku anglojęzycznego.** Reddit (46% cytowań Perplexity wg jednego badania) i Quora mają w polskim B2B prawno-podatkowym znaczenie marginalne; polskie odpowiedniki do sprawdzenia: grupy FB/LinkedIn, fora księgowe, Wykop. Zestaw domen cytowanych dla polskich zapytań podatkowych (gov.pl, Infor, Prawo.pl?) wymaga własnego badania — bez niego priorytety opierają się na danych z innego rynku. `[do weryfikacji dla PL]`

## 5. schema.org i llms.txt — nie inwestuj pod cytowania

**Schema.** Google: "There's also no special schema.org structured data that you need to add" ([AI Features](https://developers.google.com/search/docs/appearance/ai-features), 2025-12-10). Eksperyment Ahrefs (1 885 stron z parami kontrolnymi, 30 dni): AI Mode +2,4% (nieistotne), ChatGPT +2,2% (nieistotne), AIO −4,6% (istotne, bez wiarygodnej przyczyny) — [SEJ, 2026-05-16](https://www.searchenginejournal.com/serp-faq-removal-new-data-challenge-schemas-ai-search-value/574993/). Rich results dla FAQ wygaszone w 2026. **Rekomendacja**: utrzymuj `Organization`, `LegalService`, `Person` (autorzy z `worksFor`), `Article`, `BreadcrumbList` — jako identyfikację encji i kwalifikowalność do rich results, nie jako dźwignię cytowań; wypełniaj właściwości maksymalnie i zagnieżdżaj (`@graph`), nie dodawaj schemy niezgodnej z treścią widoczną. Nie wdrażaj `FAQPage` licząc na rich result.

**llms.txt.** Zweryfikowane, nie działa: 97% ze zbadanych 38 tys. plików nie zostało pobranych ani razu w ciągu miesiąca; boty wyszukiwania AI to 1,1% żądań; zero żądań o pliki nieistniejące, czyli systemy AI nie szukają go z własnej inicjatywy ([Ahrefs, 137 tys. domen, 2026-06-15](https://ahrefs.com/blog/llmstxt-study/)). Google: "Google Search itself doesn't use them" ([AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide), 2026-07-10). Cerar (Ahrefs) to potwierdza: żaden duży dostawca nie wspiera. Jedyny realny use case: dokumentacja techniczna dla agentów kodujących — dla kancelarii nieistotny. Jeśli plik mimo wszystko powstanie, traktuj go jak kod (wersjonowanie), bo jest wektorem prompt injection.

## 6. Warstwa techniczna — warunki konieczne

1. **robots.txt — najczęstszy blocker, 5 minut roboty** (Cerar: ~5,9% ze 140 mln witryn blokuje GPTBot, często nieświadomie — odziedziczone szablony, **Cloudflare z domyślnie włączonym "instruct AI bot traffic with robots.txt"**). Sprawdź: `GPTBot`, `OAI-SearchBot`, `ClaudeBot`, `PerplexityBot`, `Google-Extended`. Blokada = brak cytowań w danym asystencie; dla kancelarii budującej autorytet: nie blokować.
2. **Brak `nosnippet` / `max-snippet:0` / `data-nosnippet`** na treściach merytorycznych — wykluczają z AIO.
3. **Treść w HTML, nie w JS.** Crawler ChatGPT nie renderuje JavaScriptu (Gemini i Copilot tak) — treść doładowywana klientem to dla niego pusta strona. Test: wyłącz JS w przeglądarce i wejdź na stronę. Google: "Keep important content in text format" ([AI Features](https://developers.google.com/search/docs/appearance/ai-features)).
4. **Nie gate'uj treści eksperckich za formularzem** — systemy AI nie przechodzą przez lead-geny ([SEL](https://searchengineland.com/b2b-authority-ai-search-era-456207)).
5. **Szybkość i kody odpowiedzi**: przy retrievalu na żywo wolna strona bywa odrzucona przed oceną; WAF/hosting nie może blokować botów AI; 304 dla treści niezmienionych oszczędza crawl budget (webinar Senuto).
6. **Halucynowane URL-e**: asystenci AI kierują na 404 2,87× częściej niż Google (dane Ahrefs). Monitoruj 404 z referrerów AI i przekierowuj na najbliższą istniejącą stronę.
7. **Linkowanie wewnętrzne**: każda strona w ≤3 kliknięciach, klastry tematyczne, linki ze starych wpisów do nowych (boty i tak odwiedzają stare), strony autorów jako huby.

## 7. Pomiar

Żadne pojedyncze narzędzie nie zamyka obrazu: GSC pokazuje wyświetlenia w GenAI bez kliknięć, GA4 kliknięcia bez AIO (kliknięcia z AIO wpadają do zwykłego organica), a duża część wpływu AI kończy się wpisaniem nazwy w pasek adresu (Direct) albo wygooglowaniem marki.

1. **GSC — raport Generative AI** (od 2026-06-03, rollout etapowy — sprawdź, czy mentzen.pl jest objęty): wyświetlenia w funkcjach generatywnych, strony, kraje; **bez kliknięć** ([Google](https://developers.google.com/search/blog/2026/06/gen-ai-performance-reports)). Uwaga: błąd zaniżający wyświetlenia od 2026-08-13 — nie interpretuj spadków z tego okresu ([SEL](https://searchengineland.com/google-search-console-generative-ai-performance-report-in-search-data-bug-485215)).
2. **GA4 — własna grupa kanałów** (domyślny kanał "AI Assistant" zaniża, Perplexity ląduje w Referral — [SEJ](https://www.searchenginejournal.com/ga4s-ai-assistant-channel-undercounts-your-ai-traffic-how-to-build-one-that-doesnt/580133/)): Admin → Channel groups → kanał "AI Referral", `Source matches regex`: `(chatgpt\.com|openai\.com|perplexity\.ai|claude\.ai|gemini\.google\.com|copilot\.microsoft\.com|deepseek\.com|you\.com)`. Trzymaj się pełnych domen, nie gołych tokenów typu `gpt`. Liczba i tak zaniżona: linki w treści ChatGPT na kontach płatnych mają `noreferrer`, Perplexity nie przekazuje referrera z aplikacji desktopowej, Grok wcale (za Cerarem).
3. **Logi serwera — boty AI**: rozróżniaj treningowe (GPTBot, Google-Extended) od wyszukująco-cytujących (ChatGPT-User, OAI-SearchBot) — te drugie zwiastują ruch. Wyłapuj ważne strony, do których boty nie zaglądają wcale (problem wykrywalności).
4. **Samodzielna atrybucja — najtańsza i najważniejsza** (wg Cerara: "jeśli masz zrobić jedną rzecz, zrób tę"): pytanie "skąd o nas wiesz" w formularzu kontaktowym z opcjami "asystent AI (ChatGPT, Perplexity, Claude)" i "wyszukiwarka z AI". Ahrefs tą metodą odkrył, że ~3% ich konwersji pochodzi z AI.
5. **Ręczny monitoring widoczności**: co miesiąc zestaw pytań, które zadałby klient ("jaka kancelaria podatkowa specjalizuje się w cenach transferowych"), zadawany w oknie incognito (autor kursu "The Complete SEO & AI SEO Course for 2026", Surfer Academy, [youtube.com/watch?v=7DRO4rEIHDk](https://www.youtube.com/watch?v=7DRO4rEIHDk), ostrzega, że ChatGPT personalizuje po historii); notuj: czy marka pada, jak jest opisana, kto obok. Wielokrotne powtórzenia, nie pojedynczy zrzut. Google ostrzega przed narzędziami obiecującymi "wewnętrzne metryki Google" ([AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide)).

**Metryka biznesowa nadrzędna** (Parzych): zanim ktokolwiek zaproponuje przebudowę bloga po spadkach, zestaw ruch vs formularze kontaktowe w tym samym okresie — 80% wywołań AIO to góra lejka, a znane są przypadki wzrostu sprzedaży mimo spadku ruchu blogowego.

## 8. Zastosowanie dla mentzen.pl

**Kolejność wdrożenia** (nakład rosnąco):

1. **Audyt techniczny (dzień 1)**: robots.txt pod kątem GPTBot/OAI-SearchBot/ClaudeBot/PerplexityBot/Google-Extended; `nosnippet`/`max-snippet`; czy treść artykułów jest w czystym HTML (WordPress zwykle tak, ale sprawdź widżety/tabsy JS); czy WAF/hosting nie blokuje botów AI; 404 z referrerów AI.
2. **Pomiar (tydzień 1)**: grupa kanałów AI w GA4; sprawdzenie, czy GSC mentzen.pl ma raport Generative AI (i czy pokazuje impresje dla Poland — to zarazem rozstrzyga pytanie o dostępność AIO/AI Mode w PL); pytanie "skąd o nas wiesz" w formularzu kontaktowym; bazowy ręczny audyt widoczności po polsku (pytania usługowe i rekomendacyjne w ChatGPT/Perplexity/Google, incognito, kilka powtórzeń).
3. **E-E-A-T (miesiąc 1)**: każdy artykuł podpisany doradcą/radcą przy H1; biogramy z numerem wpisu, specjalizacją, publikacjami; strony autorów z listingiem tekstów; schema `Person` powiązana z `Organization`; widoczne "stan prawny na DD.MM.RRRR".
4. **Cykl aktualizacji zamiast samych nowych treści**: kalendarz odświeżania stron o stawkach, limitach, terminach (KSeF, składka zdrowotna, estoński CIT, JPK) wyprzedzający wejście przepisów w życie; content pruning wpisów bez ruchu, potencjału i związku tematycznego (410 lub 301).
5. **Discover/News dla treści reaktywnych (koszt bliski zeru)**: wpisy o zmianach przepisów publikuj z wyprzedzeniem względem terminu — wg Exposure Ninja materiał 4–6 tygodni przed datą łapie research, w dniu wejścia panikę — z dużą grafiką (`max-image-preview:large`, obraz ≥1200 px — [Google Discover](https://developers.google.com/search/docs/appearance/google-discover)), konkretnym tytułem bez clickbaitu i podpisanym autorem (E-E-A-T działa też tu); monitoruj zakładkę Discover w GSC. Uzasadnienie i zastrzeżenia: sekcja 2.
6. **Refaktoryzacja treści pod cytowalność**: konsolidacja cienkich wpisów w jedną mocną stronę na temat; nagłówki-pytania z odpowiedzią w pierwszym zdaniu; tabele porównawcze form opodatkowania z samowystarczalnymi wierszami; FAQ pod subpytania; sygnatury i przepisy jako zaczepy faktograficzne w każdym akapicie.
7. **Własne dane jako program**: agregaty z praktyki (czas rejestracji spółki, czas uzyskania interpretacji, odsetek wygranych sporów), ankiety wśród klientów-przedsiębiorców, komentarze do projektów ustaw w dniu publikacji. Publikuj jako oddzielne, cytowalne opracowania. Dane klientów wyłącznie zagregowane i zanonimizowane.
8. **Off-site pod wzmianki** (prawdopodobnie największa dźwignia "bycia polecanym", `[do weryfikacji dla PL]`): komentarze eksperckie w Rzeczpospolitej/Prawo.pl/Puls Biznesu (wzmianka bez linku ma samodzielną wartość — korelacja 0,664); rankingi kancelarii (Rzeczpospolita, Forbes — redakcyjne, ale wysoce cytowalne); spójne dane encji wszędzie (nazwa, NIP, specjalizacje); Wikidata dla kancelarii i wspólników; Google Business Profile; LinkedIn ekspertów. Mapuj osobno encje: marka kancelarii + nazwiska ekspertów (każda ma własny profil widoczności — framework luk Cerara: widoczność / narracja / temat / format / wzmianki / popyt). Uwaga: taktyki rekomendacyjne weryfikuj z zasadami etyki KIDP/OIRP.
9. **Frazy narzędziowe jako obrona ruchu**: kalkulatory (składka zdrowotna, wynagrodzenia), wzory dokumentów z zastrzeżeniem "to nie porada prawna", checklisty, terminarze — AI nie wykona obliczenia na danych użytkownika; to jedyna kategoria informacyjna odporna na zero-click i naturalny magnes na linki (zbieżne: Cerar i mechanizm "PandaDoc" z kursu SEO 2026: wzór → mail → sekwencja → kontakt).
10. **YouTube — tania wersja**: nagrywaj webinary, które i tak się odbywają; tytuł z frazą, fraza wypowiedziana w filmie (transkrypcja jest tym, co model czyta), znaczniki czasu. Uzasadnienie: YouTube to najczęściej cytowana domena w AIO, korelacja wzmianek YT z widocznością w ChatGPT 0,737 (dane Ahrefs, rynek EN).
11. **Monitoring reputacji**: regularnie pytaj asystentów o nazwę kancelarii i nazwiska wspólników — AI potrafi podać stary negatywny wątek z forum jako fakt (Parzych); przeważaj skalą pozytywnych źródeł, luki informacyjne wypełniaj oficjalnym konkretem.

**Czego nie robić**: llms.txt jako inwestycja; `FAQPage` schema pod rich result; cięcie treści na mikro-chunki; kupowanie masowych wzmianek; publikacja treści AI bez weryfikacji eksperta (przy pytaniach o sedno rozstrzygnięcia sądu modele halucynują w co najmniej 75% przypadków, a dla zapytań prawnych ogółem 69–88% — Dahl, Magesh, Suzgun, Ho, Stanford RegLab/HAI, [arXiv 2401.01301](https://arxiv.org/abs/2401.01301), za [SEJ](https://www.searchenginejournal.com/can-you-use-ai-to-write-for-ymyl/558945/)); przenoszenie budżetu z fundamentów SEO na "GEO" (AIO dotyczy ~30% fraz, klasyczny ranking — 100%; zbieżna opinia Parzycha i Nathana Gotcha, [youtube.com/watch?v=bfBwk2KK9jc](https://www.youtube.com/watch?v=bfBwk2KK9jc): "w ChatGPT nie ma czego optymalizować — widoczność to wypadkowa całości marketingu").

## 9. Luki do domknięcia

1. Oficjalne potwierdzenie zakresu AIO/AI Mode w Polsce — rozstrzygalne empirycznie przez GSC mentzen.pl.
2. Jakie domeny są cytowane w AIO/ChatGPT dla polskich zapytań podatkowo-prawnych (gov.pl? Infor? Prawo.pl?) — własne badanie, warunek sensownej priorytetyzacji off-site.
3. Rozstrzygnięcie sprzeczności metodyk pokrycia Perplexity/AIO z rankingiem Google (28,6% vs 91%).
4. Efekt answer-first na cytowalność — brak kontrolowanego eksperymentu; obecnie konsensus praktyków, nie dowód.
5. Liczby z webinaru Senuto (auto-transkrypcja) — weryfikacja w raporcie Senuto o AI Overviews przed cytowaniem na zewnątrz.
6. Konwersja ruchu AI w B2B prawniczym (deklarowane 4,4–23× pochodzi z prób digital-marketingowych).
