# AI search / GEO / AEO — widoczność w odpowiedziach AI

Research pod skill SEO dla mentzen.pl (kancelaria: prawo, doradztwo podatkowe, księgowość; B2B; PL; Google.pl).
Data zebrania: 2026-08-28. Wszystkie twierdzenia z cytowaniem. Brak potwierdzenia = `[do weryfikacji]`.

---

## 0. TL;DR — co robić, czego nie

**Rób:**
1. Klasyczne SEO techniczne i treściowe — to nadal fundament, bo funkcje AI w Google działają na tym samym indeksie i tych samych systemach rankingowych.
2. Odpowiadaj wprost na pytanie w pierwszym akapicie sekcji, prozą, pod nagłówkiem sformułowanym jak pytanie klienta.
3. Publikuj własne dane: stawki, terminy, wyliczenia, statystyki z praktyki kancelarii, mini-badania. To najmocniej udokumentowana dźwignia w badaniach GEO.
4. Buduj obecność poza własną domeną (cytowania w mediach branżowych, LinkedIn, katalogi, rankingi). Większość cytowań AI wskazuje na źródła trzecie, nie na stronę marki.
5. Odblokuj crawl (robots.txt, brak gate'owania kluczowych treści), pilnuj `Organization`/`Person`/`Article` schema jako infrastruktury encji.
6. Mierz osobno: GSC → raport Generative AI, GA4 → własna grupa kanałów po regexie, logi serwera → boty AI.

**Unikaj:**
1. `llms.txt` jako inwestycji pod widoczność w AI — brak dowodów, że ktokolwiek to czyta.
2. Dosypywania schema.org "pod AI" w nadziei na cytowania — eksperyment kontrolowany nie wykazał efektu.
3. Cięcia treści na "chunki pod AI" — Google wprost mówi, że to niepotrzebne.
4. Kupowania/wymuszania "wzmianek" w sieci pod GEO — Google klasyfikuje to jako nieautentyczne.
5. Traktowania narzędzi GEO obiecujących "wewnętrzne metryki Google" jako wiarygodnych.
6. Publikowania treści prawno-podatkowych bez autora z realnymi kwalifikacjami. To YMYL.

---

## 1. Jak to działa mechanicznie

### 1.1. Google AI Overviews i AI Mode

Google opisuje własne funkcje generatywne jako nadbudowę nad istniejącym Search, a nie osobny system: „These features rely on AI techniques to highlight content from our Search index", z użyciem retrieval-augmented generation (RAG) i query fan-out.
— [Google's Guide to Optimizing for Generative AI Features](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide), aktualizacja 2026-07-10

Warunek wejścia jest minimalny i czysto techniczny: „a page must be indexed and eligible to be shown in Google Search with a snippet, fulfilling the Search technical requirements".
— [AI Features and Your Website](https://developers.google.com/search/docs/appearance/ai-features), aktualizacja 2025-12-10

**Konsekwencja normatywna:** jeśli strona jest wyłączona ze snippetów (`nosnippet`, `max-snippet:0`, `data-nosnippet` na kluczowym fragmencie), wypada z AI Overviews i AI Mode. Sprawdź, czy ktoś w przeszłości nie wstawił tych dyrektyw „dla bezpieczeństwa".

### 1.2. Query fan-out

Zapytanie użytkownika jest rozbijane na wiele podzapytań (definicje, pojęcia pokrewne, ograniczenia, przykłady, implikacje), każde odpytuje indeks osobno, a wyniki są łączone w jedną odpowiedź. Systemy preferują źródła, które obsługują wiele powiązanych pytań w obrębie jednego tematu.
— [Query fan-out in AI search](https://searchengineland.com/guide/query-fan-out), Search Engine Land (guide, aktualizowany)
— [What is Query Fan-Out?](https://ahrefs.com/blog/query-fan-out/), Ahrefs

**Konsekwencja normatywna:** buduj strony wyczerpujące temat wszerz (jedna strona = temat + sąsiednie podpytania), nie serię cienkich stron na warianty frazy. Przykład dla kancelarii: jedna solidna strona „Estoński CIT" pokrywająca warunki, wyłączenia, terminy, koszty wdrożenia, kiedy się nie opłaca — zamiast pięciu 400-słowych wpisów.

---

## 2. Skala zjawiska: zero-click i CTR

### 2.1. Zero-click

68,01% wyszukiwań w Google w USA (I–IV 2026) kończy się bez kliknięcia gdziekolwiek; w 2024 było 60,45%. Dane: SparkToro na clickstreamie Similarweb, panel desktop + mobile web, bez aplikacji mobilnej Google.
— [Google zero-click searches reach 68% in early 2026](https://searchengineland.com/google-zero-click-searches-2026-study-479717), SEL, 2026-06-09

Kategorie nadal generujące kliknięcia wg SparkToro: zapytania brandowe, lokalne i transakcyjne o wysokiej intencji.

**Konsekwencja normatywna dla kancelarii:** przesuwaj cel treści informacyjnych z „ruch" na „bycie zacytowanym + wzmianka marki", a KPI konwersyjne opieraj na zapytaniach brandowych, lokalnych i usługowych (te nadal klikają).

### 2.2. Spadek CTR przy AI Overviews

Trzy niezależne pomiary, zbieżne co do kierunku, rozbieżne co do siły:

| Badanie | Metoda | Wynik |
|---|---|---|
| Ahrefs, 2026-02-04 | 300 tys. fraz (150 tys. z AIO / 150 tys. bez), dane GSC, XII 2023 vs XII 2025 | CTR poz. 1 z AIO: −58%; poz. 2 −50,8%, poz. 3 −46,4%, poz. 10 −19,4% |
| Ahrefs, wcześniejsza edycja (IV 2025) | rok do roku | −34,5% |
| Agarwal & Sen (ISB / Carnegie Mellon), preprint SSRN IV 2026 | randomizowany eksperyment terenowy, wtyczka Chrome, 1 065 użytkowników US desktop, I–II 2026, prerejestracja AEA RCT | AIO na 42% zapytań; −38% kliknięć wychodzących na zapytaniach z AIO; zero-click 54% → 72%; satysfakcja użytkowników bez zmian |

— [Update: AI Overviews Reduce Clicks by 58%](https://ahrefs.com/blog/ai-overviews-reduce-clicks-update/), Ahrefs, 2026-02-04
— [Study Confirms Google AI Overviews Cut Organic Clicks 38%](https://www.searchenginejournal.com/ai-overviews-cut-organic-clicks-38-field-study-finds/573145/), SEJ, IV 2026

Preprint Agarwal & Sen nie przeszedł recenzji, a wyniki dla AI Mode autorzy sami nazywają eksploracyjnymi (wyższy odpływ uczestników, obchodzenie wtyczki).

**Wniosek:** przyjmij, że na zapytaniach informacyjnych z AIO tracisz rzędu 40–60% kliknięć względem stanu sprzed AIO, i że efekt sięga całej pierwszej dziesiątki, nie tylko pozycji 1.

---

## 3. Co realnie zwiększa szanse na bycie cytowanym

### 3.1. Najmocniejszy dowód: dane, cytaty, statystyki w treści

Oryginalna praca o GEO (Aggarwal i in., Princeton, arXiv 2311.09735) testowała edycje treści pod widoczność w silnikach generatywnych. Najskuteczniejsze zabiegi: dodanie cytowań źródeł, cytatów z autorytetów i statystyk — poprawa 30–40% na metryce Position-Adjusted Word Count, w niektórych zapytaniach powyżej 40%. Poprawa płynności i czytelności: 15–30%. Keyword stuffing nie działał.
— [GEO: Generative Engine Optimization](https://arxiv.org/pdf/2311.09735), arXiv 2311.09735

**Konsekwencja normatywna:** każdy istotny akapit merytoryczny powinien mieć zaczep faktograficzny: liczbę, datę, kwotę, sygnaturę, przepis, link do źródła. Dla kancelarii to naturalne — art. ustawy, interpretacja KIS, wyrok NSA, próg kwotowy.

### 3.2. Struktura i odpowiedź wprost

Analiza danych Ahrefs o schemacie prowadzi do wniosku, że „content structure, clear headings, and direct answers in prose may matter more for AI citation than markup structure".
— [SERP FAQ Removal & New Data Challenge Schema's AI Search Value](https://www.searchenginejournal.com/serp-faq-removal-new-data-challenge-schemas-ai-search-value/574993/), SEJ, 2026-05-16

Uwaga: to interpretacja komentatorska wywiedziona z braku efektu schemy, nie bezpośredni pomiar wpływu struktury. Sama teza „answer-first zwiększa cytowalność" nie ma w zebranych źródłach kontrolowanego eksperymentu. `[do weryfikacji]`

### 3.3. Obecność poza własną domeną

- Tylko 2,8% z 1 851 źródeł cytowanych w Google AI Mode, ChatGPT i Perplexity to strony należące do marek (analiza Shero Commerce). Gdy jednak AI wymieniało markę z nazwy, jej własna strona była cytowana w 31% przypadków.
- Marki są 6,5× częściej wzmiankowane dzięki obecności na stronach trzecich niż na własnej domenie; tylko 13% wzmianek AI pochodzi z domen marek; 81% cytowań w zestawieniach typu listicle pochodzi ze źródeł trzecich.
- Mniej niż 30% odpowiedzi AI jednocześnie wzmiankuje i cytuje tę samą markę. Wzmianka ≠ cytowanie i trzeba to mierzyć osobno.
— [AI Tools Recommend Brands But Cite Other Sites](https://www.searchenginejournal.com/ai-tools-recommend-brands-but-cite-other-sites-data-shows/587160/), SEJ
— [The latest data on AI citations vs. AI mentions](https://searchengineland.com/guide/ai-citations-vs-ai-mentions-data), SEL (guide zbiorczy, aktualizowany)

**Konsekwencja normatywna:** dla kancelarii PR branżowy (Rzeczpospolita/Prawo.pl/Puls Biznesu, rankingi kancelarii, wystąpienia, komentarze eksperckie) ma prawdopodobnie większy wpływ na bycie *polecanym* przez asystenta niż kolejny wpis blogowy. Traktuj to jako hipotezę roboczą opartą na danych z rynku US. `[do weryfikacji dla PL]`

### 3.4. E-E-A-T i YMYL — dla kancelarii to warunek brzegowy

Usługi prawne wpływają na zdrowie, źródło utrzymania i sytuację finansową, więc każda forma usług prawnych dotykających tych obszarów jest YMYL. Google przykłada większą wagę do E-E-A-T przy tematach YMYL.
— [Search Quality Rater Guidelines](https://guidelines.raterhub.com/searchqualityevaluatorguidelines.pdf), wersja 2025-09-11
— [Creating Helpful, Reliable, People-First Content](https://developers.google.com/search/docs/fundamentals/creating-helpful-content), Google Search Central
— [What is YMYL?](https://searchengineland.com/guide/ymyl), SEL

We wrześniu 2025 Google zaktualizował wytyczne dla oceniających: doprecyzowanie definicji YMYL i nowe przykłady dotyczące AI Overviews.
— [Google updates search quality raters guidelines...](https://searchengineland.com/google-updates-search-quality-raters-guidelines-adding-ai-overview-examples-ymyl-definitions-461908), SEL

**Konsekwencja normatywna:** każdy artykuł prawno-podatkowy ma mieć podpisanego autora z tytułem i uprawnieniami (doradca podatkowy nr wpisu, radca prawny), datę publikacji i datę ostatniej aktualizacji, oraz jawny stan prawny („stan na dzień…"). Treść generowana przez AI musi być zweryfikowana przez eksperta — Google wymaga jawności co do sposobu wytworzenia treści.

---

## 4. schema.org — faktyczny status

**Stanowisko Google:** „You don't need to create new machine readable files, AI text files, or markup to appear in these features. There's also no special schema.org structured data that you need to add."
— [AI Features and Your Website](https://developers.google.com/search/docs/appearance/ai-features), 2025-12-10

„Structured data isn't required for generative AI search", ale pozostaje przydatne dla kwalifikowalności do rich results.
— [AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide), 2026-07-10

**Dowód eksperymentalny:** Ahrefs śledził 1 885 stron, które dodały JSON-LD, każdą sparowaną ze stroną kontrolną bez schemy; okno 30 dni. Wynik: AI Mode +2,4% (nieistotne statystycznie), ChatGPT +2,2% (nieistotne), AI Overviews −4,6% (istotne, ale bez wiarygodnego przypisania przyczyny). Wcześniejsze badanie Search/Atlas (XII 2024) nie znalazło korelacji między pokryciem schemą a wskaźnikiem cytowań.
— [SERP FAQ Removal & New Data Challenge Schema's AI Search Value](https://www.searchenginejournal.com/serp-faq-removal-new-data-challenge-schemas-ai-search-value/574993/), SEJ, 2026-05-16

Ograniczenia badania Ahrefs: testowało tylko strony już widoczne w AI, zlało wszystkie typy schemy w jedną pulę, więc efekty per typ pozostają niezmierzone.

**Kontrapunkt:** John Mueller: „you're always welcome to use structured data to provide better machine-readable context for your pages, which may not always result in visible changes, but can still help our systems show your pages for relevant queries". Praktycy argumentują, że schema działa jak „mały wewnętrzny knowledge graph" — definicja encji, klarowność atrybutów, relacje (`offeredBy`, `worksFor`) w formacie `@graph`.
— [How schema markup fits into AI search — without the hype](https://searchengineland.com/schema-markup-ai-search-no-hype-472339), SEL, 2026-03-25, Aimee Jurenka

W tym samym artykule pada twierdzenie, że Google potwierdził w IV 2025, iż „structured data gives an advantage in search results" — nie udało się dotrzeć do pierwotnego źródła tej wypowiedzi. `[do weryfikacji]`

Google wygasił rich results dla FAQ w 2026 (po ograniczeniu ich w 2023 do stron rządowych i zdrowotnych), co jest kolejnym krokiem w wieloletnim ograniczaniu widocznych nagród za markup.
— tamże, SEJ 2026-05-16

**Rekomendacja dla mentzen.pl:** wdroż i utrzymuj `Organization`, `LegalService`/`ProfessionalService`, `Person` (autorzy z `worksFor`), `Article`/`BlogPosting`, `BreadcrumbList`, `WebSite`. Uzasadnienie: jednoznaczna identyfikacja encji kancelarii i jej ekspertów oraz kwalifikowalność do rich results — nie „więcej cytowań w AI". Nie inwestuj w `FAQPage` licząc na rich result. Nie dodawaj schemy niezgodnej z treścią widoczną (Google wymaga zgodności).

---

## 5. llms.txt — faktyczny status adopcji

**Zweryfikowane. Odpowiedź: nie działa, nie wdrażaj pod SEO/GEO.**

Badanie Ahrefs (137 210 domen, Ahrefs Web Analytics + Bot Analytics, maj 2026, weryfikacja że pliki są prawdziwym Markdownem a nie soft 404):
- 28% domen (38 360) publikuje poprawny `llms.txt`.
- **97% tych plików nie zostało pobranych ani razu w maju 2026.**
- Z 3% plików, które miały ruch: 96% żądań to automaty. Największa kategoria to narzędzia audytu SEO (21,7%), dalej niezidentyfikowane (14,9%), ogólne crawlery (13,1%), profilowanie technologii (11,6%). Agenci/infrastruktura AI 10,5%, narzędzia GEO/AEO 5,8%, crawlery treningowe AI 5,3%.
- Boty *wyszukiwania* AI (PerplexityBot itp.): 1,1% żądań.
- Zero żądań od botów AI o pliki nieistniejące — **systemy AI nie szukają tego pliku z własnej inicjatywy.**
- Zastrzeżenie autorów: klienci Ahrefs są bardziej techniczni niż przeciętna sieć, więc 28% to górna granica adopcji.
— [We Analyzed 137K Sites: 97% of llms.txt Files Never Get Read](https://ahrefs.com/blog/llmstxt-study/), Ahrefs, 2026-06-15

**Stanowisko Google:** o `llms.txt` i podobnych plikach — „Google Search itself doesn't use them", ani nie szkodzą, ani nie pomagają widoczności.
— [AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide), 2026-07-10

Sygnały mieszane: Google dodał `llms.txt` na wielu swoich stronach dokumentacji w XII 2025, po czym w ciągu doby wycofał go z dokumentacji Search; Chrome Lighthouse dostał check na `llms.txt`. John Mueller tłumaczył, że to „not done for search" — „temporary crutch, perhaps to save some tokens" dla narzędzi AI parsujących dokumentację deweloperską.
— [Google adds llms.txt check to Chrome Lighthouse](https://searchengineland.com/google-llms-txt-chrome-lighthouse-478246), SEL
— [Does llms.txt matter? We tracked 10 sites to find out](https://searchengineland.com/does-llms-txt-matter-467740), SEL

Jedyny przypadek użycia z realnym sygnałem: dokumentacja techniczna czytana przez agentów kodujących (Claude-Code był drugim botem po GPTBot wśród nazwanych narzędzi AI). Dla kancelarii nieistotne.

Jeśli mimo wszystko plik ma powstać: traktuj go jak kod (wersjonowanie, ograniczony dostęp do edycji), bo jest wektorem prompt injection.

---

## 6. Różnice między platformami

Podstawowy fakt: **cytowania nie są przenośne między platformami.** Ahrefs, czerwiec 2025: analiza ~76,7 mln AI Overviews, ~957 tys. promptów ChatGPT i ~953,5 tys. promptów Perplexity; z top 50 najczęściej wzmiankowanych domen tylko 7 stron pokrywało się na wszystkich trzech platformach (14% pokrycia, 86% rozbieżności).
— [86% of Top Mentioned Sources Are Not Shared Across AI Assistants](https://ahrefs.com/blog/top-mentioned-sources-are-not-shared-across-ai-assistants/), Ahrefs, 2025-06-12

Profile źródeł (tamże):
- **Google AI Overviews** — uznane serwisy autorytatywne (zdrowie, finanse, encyklopedyczne), własności Google, social media; słabo obecne media i e-commerce.
- **ChatGPT** — wydawcy i media spoza Google, mocno pod wpływem umów licencyjnych; słabe pokrycie tematów zdrowotnych. Najniższy udział cytowań first-party, poniżej 11%.
- **Perplexity** — szersze źródła międzynarodowe, silna reprezentacja marek regionalnych i lokalnych; brak platform społecznościowych i klasycznych mediów zachodnich. Wg innych danych Reddit to ponad 46% cytowań Perplexity, a dla zapytań B2B liczą się LinkedIn i G2.
- **Claude** — cytuje user-generated content 4× częściej niż konkurenci, częściej mniejsze wydawnictwa (za: SEL guide).

### 6.1. Sprzeczność w danych o pokryciu z rankingiem Google — rozstrzygnij ostrożnie

| Źródło | Zakres | Wynik |
|---|---|---|
| Ahrefs, 2025-08-11 | 15 tys. zapytań long-tail; ChatGPT, Gemini, Copilot, Perplexity | średnio **12%** cytowań w top 10 Google; Perplexity 28,6%, Gemini 8,6%, Copilot 8,2%, ChatGPT ~7%; ~80% cytowanych URL-i nie ma nawet w top 100 |
| Ahrefs, VII 2025 | tylko AI Overviews | **86%** cytowań pochodzi ze stron obecnych w top 100 Google; mediana pozycji cytowanych URL-i: 3; główny URL: mediana pozycji 2 |
| Semrush, VII 2025 | ponad 5 tys. fraz | Perplexity: 91% pokrycia domen / 82% URL z top 10; AI Overviews 86% / 67%; AI Mode 54% / 35% |
| Search Atlas, XI 2025 | 18 377 dopasowanych zapytań; GPT, Gemini, Perplexity | mediana pokrycia domen Perplexity z Google: ~25–30% |

Rozbieżność Perplexity 28,6% vs 91% jest zbyt duża, żeby ją pogodzić bez wglądu w metodyki (long-tail vs head, pokrycie URL vs domen, moment pomiaru). **Nie cytuj żadnej z tych liczb jako ustalonego faktu.** `[do weryfikacji]`

Bezpieczna synteza: **AI Overviews są mocno sprzężone z rankingiem organicznym Google; AI Mode słabiej; ChatGPT, Gemini i Copilot najsłabiej.** Kierunek ten jest zgodny we wszystkich czterech badaniach.
— [Only 12% of AI Cited URLs Rank in Google's Top 10](https://ahrefs.com/blog/ai-search-overlap/), Ahrefs, 2025-08-11
— [76% of AI Overview Citations Pull From the Top 10](https://ahrefs.com/blog/search-rankings-ai-citations/), Ahrefs
— [How Google's AI Mode Compares to Traditional Search and Other LLMs](https://www.semrush.com/blog/ai-mode-comparison-study/), Semrush
— [New Data Finds Gap Between Google Rankings And LLM Citations](https://www.searchenginejournal.com/new-data-finds-gap-between-google-rankings-and-llm-citations/561492/), SEJ

**Konsekwencja normatywna:** utrzymuj klasyczne pozycje w Google (to kupuje AI Overviews prawie za darmo), ale nie zakładaj, że top 3 w Google automatycznie daje cytowanie w ChatGPT — tam decyduje obecność w źródłach, które ChatGPT ufa i licencjonuje.

### 6.2. Dostępność w Polsce

Google ogłosił rozszerzenie AI Mode „in more than 35 new languages and over 40 new countries and territories", łącznie ponad 200 krajów i terytoriów, „including many across Europe". Wpis nie wymienia Polski ani polskiego z nazwy.
— [AI Mode in Google Search expands to more than 40 new areas](https://blog.google/products-and-platforms/products/search/ai-mode-expands-languages-locations/), blog.google, 2025-10-07

AI Overviews są dostępne w ponad 200 krajach i terytoriach oraz ponad 40 językach.
— [AI Overviews are now available in over 200 countries and territories](https://blog.google/products-and-platforms/products/search/ai-overview-expansion-may-2025-update/), blog.google

Polskie agencje (Delante, Whites, mbridge, Pikseo) raportują dostępność AI Overviews i AI Mode po polsku, ale to źródła niskozaufane wg przyjętego kryterium. Dokładna data i zakres uruchomienia w PL — `[do weryfikacji]` w oficjalnych komunikatach Google lub Search Console.

**Praktyczne:** zweryfikuj to empirycznie w GSC (raport Generative AI, wymiar Country = Poland) zamiast opierać się na doniesieniach.

---

## 7. Pomiar: skąd wiedzieć, czy to działa

### 7.1. Google Search Console — raport Generative AI

Google uruchomił 2026-06-03 dedykowane raporty Search Generative AI performance w GSC — osobno dla Search i dla Discover.
— [Introducing Search Generative AI performance reports in Search Console](https://developers.google.com/search/blog/2026/06/gen-ai-performance-reports), Google Search Central Blog, 2026-06-03

Co jest w raporcie: wyświetlenia (impressions) w funkcjach generatywnych, strony które się pojawiły, kraje, urządzenia, daty z granulacją godzinową/dzienną/tygodniową/miesięczną.
**Czego nie ma: danych o kliknięciach.**
Rollout jest etapowy — na podzbiór witryn. Sprawdź, czy mentzen.pl już go ma.

Znany błąd danych: błąd logowania spowodował zaniżenie wyświetleń w raporcie Generative AI od 2026-08-13. Nie interpretuj spadków z tego okresu jako realnych.
— [Google Search Console Generative AI Performance Report in Search data bug](https://searchengineland.com/google-search-console-generative-ai-performance-report-in-search-data-bug-485215), SEL

Uwaga na sprzeczność w dokumentacji: starsza strona `ai-features` (2025-12-10) mówi, że ruch z funkcji AI raportowany jest w zwykłym Performance report pod typem „Web". Nowszy raport Generative AI jest osobnym widokiem wyświetleń. Obie rzeczy mogą współistnieć (kliknięcia w Web, wyświetlenia w raporcie GenAI), ale to interpretacja. `[do weryfikacji]`

Google ostrzega wprost: „Be wary of third-party tools that promise ranking success or claim to use 'internal' Google metrics."
— [AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide), 2026-07-10

### 7.2. GA4 — własna grupa kanałów

Domyślny kanał „AI Assistant" w GA4 zaniża ruch, m.in. dlatego, że Perplexity w nim nie ma i ląduje w Referral. Zbuduj własną grupę kanałów.
— [How To Track AI Traffic In GA4 Without Undercounting It](https://www.searchenginejournal.com/ga4s-ai-assistant-channel-undercounts-your-ai-traffic-how-to-build-one-that-doesnt/580133/), SEJ
— [GA4 adds AI Assistant channel for referral tracking](https://www.semrush.com/blog/ga4-adds-ai-assistant-channel/), Semrush

Konfiguracja: Admin → Data display → Channel groups → nowa grupa → nowy kanał „AI Referral" → warunek `Source matches regex`:

```
(chatgpt\.com|openai\.com|perplexity\.ai|claude\.ai|gemini\.google\.com|copilot\.microsoft\.com|you\.com|search\.brave\.com|bard\.google\.com)
```

— [How to Track, Measure, and Boost AI Referral Traffic](https://www.semrush.com/blog/ai-referral-traffic/), Semrush
— [How to Track and Analyze Your AI Traffic](https://ahrefs.com/blog/track-analyze-ai-traffic/), Ahrefs, 2025-04-04

**Zasada budowy regexa:** trzymaj się rozpoznawalnych domen i hostów. Nie wrzucaj gołych tokenów typu `gpt`, bo złapią fałszywe dopasowania w innych źródłach.
— SEJ, jw.

### 7.3. Ograniczenia pomiaru (znaj je i komunikuj klientowi wewnętrznemu)

1. Wiele kliknięć z AI trafia do „Direct", bo platformy nie przekazują referrera.
2. Kliknięcia z Google AI Overviews są liczone jako zwykły ruch organiczny z google — nie da się ich wydzielić w GA4.
3. GSC pokazuje wyświetlenia w GenAI, ale nie kliknięcia; GA4 pokazuje kliknięcia, ale nie z AIO. Żadne narzędzie nie zamyka pełnego obrazu.
4. Ahrefs (IV 2025) szacował ruch z LLM-ów na ~0,1% całości i sam nazywał to prawdopodobnie mocno zaniżonym.
— Ahrefs 2025-04-04, jw.
— [Why GA4 alone can't measure the real impact of AI SEO](https://searchengineland.com/why-ga4-alone-cant-measure-the-real-impact-of-ai-seo-468387), SEL

### 7.4. Logi serwera

Trzecia warstwa: udział botów AI w crawlu (GPTBot, PerplexityBot, ClaudeBot, Google-Extended, OAI-SearchBot). Jeśli boty nie pobierają strony, nie może zostać zacytowana. Ahrefs pokazuje, że rozkład botów jest mierzalny w logach — ta sama metoda co w badaniu llms.txt.
— [Ahrefs llms.txt study](https://ahrefs.com/blog/llmstxt-study/), 2026-06-15

### 7.5. Metryki do śledzenia — proponowany zestaw

| Metryka | Źródło | Uwaga |
|---|---|---|
| Wyświetlenia w funkcjach GenAI (PL) | GSC raport Generative AI | brak kliknięć |
| Kliknięcia i CTR organiczne na frazach z AIO | GSC Performance | AIO nie do wydzielenia |
| Sesje z kanału „AI Referral" | GA4 własna grupa kanałów | zaniżone |
| Udział wzmianek marki vs cytowań domeny | narzędzie AI visibility | mierz osobno, patrz 3.3 |
| Crawl botów AI | logi serwera | warunek konieczny |

---

## 8. Strategia dla kancelarii B2B

### 8.1. Dlaczego to w ogóle priorytet — dane o zachowaniu kupujących B2B

Badanie Semrush, 622 poprawne odpowiedzi, III–IV 2026, respondenci: kadra zarządzająca, product/marketing/operations, zakupy; 54% to finalni decydenci, firmy od <50 do 5000+ pracowników.
— [How AI tools shape the B2B buying process](https://www.semrush.com/blog/how-ai-shapes-b2b-buying/), Semrush, 2026-07-08

- 84% badanych używa narzędzi AI w pracy; 69% codziennie.
- 66% regularnie używa AI do researchu dostawców i rozwiązań, 29% okazjonalnie.
- **92% potwierdza, że AI ukształtowało ich shortlistę dostawców** (45% znacząco).
- 75% ufa rekomendacjom AI całkowicie lub w większości.
- Etapy: 72% wczesny research, 62% porównywanie dostawców, 48% zawężanie shortlisty, 45% decyzja końcowa.
- Kategorie badane przez AI: agencje i usługodawcy 51%, SaaS 46%, **usługi finansowe i prawne 25%**.
- Co przykuwa uwagę w odpowiedzi AI: dopasowanie do przypadku użycia 53%, jasny szczegółowy opis 50%, wyartykułowane korzyści 38%. **Tylko 7% zwraca uwagę na dostawcę dlatego, że rozpoznaje nazwę.**

To ostatnie jest najważniejsze dla kancelarii: w kanale AI rozpoznawalność marki daje minimalną przewagę. Wygrywa jasny, konkretny opis tego, co robisz i dla kogo.

### 8.2. Plan działania dla mentzen.pl

**A. Warstwa techniczna (warunek wejścia)**
1. Audyt `robots.txt` i meta robots pod kątem `nosnippet` / `max-snippet` / `data-nosnippet` — usuń, jeśli występują na treściach merytorycznych. Bez snippetu nie ma AI Overviews.
2. Zdecyduj świadomie o `Google-Extended` (ogranicza wykorzystanie treści do trenowania i groundingu w innych systemach Google, poza samym Search) oraz o GPTBot/ClaudeBot/PerplexityBot. Blokada = brak cytowań w danym asystencie. Dla kancelarii budującej autorytet rekomendacja: nie blokować.
3. Kluczowa treść w HTML jako tekst, nie w obrazkach ani wyłącznie w JS. Google wprost: „Keep important content in text format".
   — [AI Features and Your Website](https://developers.google.com/search/docs/appearance/ai-features)
4. Schema zgodnie z sekcją 4.
5. Nie gate'uj treści eksperckich za formularzem. Systemy AI nie przechodzą przez formularze lead-genowe, więc gated content jest niewidoczny dla asystentów.
   — [The future of B2B authority building in the AI search era](https://searchengineland.com/b2b-authority-ai-search-era-456207), SEL

**B. Warstwa treści**
1. **Strony usług pisane pod dopasowanie do przypadku użycia**, nie pod ogólniki. Zamiast „kompleksowa obsługa prawna firm" → „dla kogo, jaki problem, jaki zakres, jaki tryb rozliczenia, jaki termin". Uzasadnienie: 53% kupujących B2B reaguje na dopasowanie do use case, 50% na jasny szczegółowy opis (Semrush 2026-07-08).
2. **Formuła odpowiedzi**: nagłówek H2/H3 sformułowany jak realne pytanie klienta → pierwszy akapit z bezpośrednią odpowiedzią w 2–3 zdaniach → rozwinięcie z podstawą prawną, liczbami i zastrzeżeniami.
3. **Własne liczby.** To najlepiej udokumentowana dźwignia (GEO paper, +30–40%). Dla kancelarii do wykorzystania: agregaty z własnej praktyki (ile trwa rejestracja spółki, ile trwa uzyskanie interpretacji indywidualnej, jaki odsetek wniosków przechodzi), krótkie ankiety wśród klientów, analiza publicznych danych KIS/MF z własnym komentarzem. Publikuj jako oddzielne, cytowalne opracowania.
4. **Aktualność.** Prawo podatkowe zmienia się co roku; wpisz i eksponuj datę aktualizacji oraz stan prawny. Dla treści YMYL to element trustu, nie kosmetyka.
5. **Jedna mocna strona na temat zamiast rozdrobnienia** — pod query fan-out (sekcja 1.2).
6. Wideo i grafiki tam, gdzie mają sens: Google zaleca wspieranie treści tekstowej wysokiej jakości obrazami i wideo.
   — [AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide)

**C. Warstwa poza domeną (prawdopodobnie najważniejsza dla „bycia polecanym")**
1. Komentarze eksperckie w mediach branżowych i ogólnobiznesowych.
2. Profile ekspertów na LinkedIn z merytorycznymi publikacjami (LinkedIn wskazywany jako mocne źródło dla zapytań B2B w Perplexity).
3. Obecność w rankingach i katalogach kancelarii, w profilach z pełnymi, spójnymi danymi (NIP, adres, specjalizacje).
4. Spójność danych encji wszędzie: ta sama nazwa, adres, opis specjalizacji. To karmi rozpoznanie encji.
5. Google Business Profile aktualny — Google wymienia go wprost wśród praktyk dla funkcji AI.
   — [AI Features and Your Website](https://developers.google.com/search/docs/appearance/ai-features)

**D. Czego nie robić**
- Nie kupuj i nie zlecaj masowych „wzmianek" pod GEO. Google: „Seeking inauthentic 'mentions' across the web isn't as helpful as it might seem", a skalowana produkcja treści łamie polityki spamowe.
  — [AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide)
- Nie rozdrabniaj treści na mikro-chunki: „There's no requirement to break your content into tiny pieces for AI to better understand it."
  — tamże
- Nie publikuj treści prawno-podatkowej wygenerowanej przez AI bez weryfikacji eksperta. Przytaczane dane: AI halucynuje treść orzeczeń w 75% przypadków.
  — [Can You Use AI To Write For YMYL Sites?](https://www.searchenginejournal.com/can-you-use-ai-to-write-for-ymyl/558945/), SEJ — liczba pochodzi z artykułu wtórnego, nie dotarłem do badania źródłowego `[do weryfikacji]`

---

## 9. Twierdzenia pojedynczego źródła — traktuj ostrożnie

Poniższe pojawiają się w zebranych materiałach, ale opierają się na jednym pomiarze lub na danych dostawcy narzędzia. Nie używaj ich jako argumentu decyzyjnego bez potwierdzenia.

- Marki cytowane w AI Overviews mają 35% wyższy CTR niż niecytowane. `[do weryfikacji]`
- Odwiedzający z AI konwertują ~4,4× lepiej niż z klasycznego organicznego (Semrush, VII 2025, próbka: ponad 500 tematów z branży digital marketing/SEO — wąska i nietransferowalna na usługi prawne). `[do weryfikacji dla B2B prawniczego]`
- Ruch z AI przewyższy ruch z klasycznego wyszukiwania w 2028 (projekcja Semrush dla tematów digital marketingowych). Projekcja, nie pomiar.
  — [We Studied the Impact of AI Search on SEO Traffic](https://www.semrush.com/blog/ai-search-seo-traffic-study/), Semrush, 2025-07-21
- Reddit jako źródło ponad 46% cytowań Perplexity. Wartość mocno zależna od kategorii zapytań i rynku; dla polskiego rynku prawniczego prawdopodobnie nieaktualna. `[do weryfikacji dla PL]`
- ChatGPT cytuje strony z pozycji 21+ w ~90% przypadków (Semrush) — trudne do pogodzenia z danymi o pokryciu z Bing top 10; patrz sprzeczności w 6.1. `[do weryfikacji]`

---

## 10. Luki do domknięcia w kolejnym researchu

1. Oficjalne potwierdzenie Google co do daty i zakresu uruchomienia AI Overviews i AI Mode w Polsce oraz po polsku.
2. Czy istnieją jakiekolwiek dane o zachowaniu AI Overviews na polskojęzycznych zapytaniach prawno-podatkowych (częstotliwość wyzwalania, typy cytowanych źródeł — czy dominują portale typu Infor, Prawo.pl, gov.pl).
3. Metodyki badań pokrycia Perplexity z Google (rozstrzygnięcie sprzeczności 28,6% vs 91%).
4. Czy Google faktycznie wypowiedział się w IV 2025, że structured data daje przewagę w wynikach — źródło pierwotne.
5. Realny efekt formuły answer-first na cytowalność — brak kontrolowanego eksperymentu w zebranych źródłach.
6. Stan raportu Generative AI w GSC dla mentzen.pl (czy konto jest objęte rolloutem).

---

## 11. Spis źródeł

**Google (pierwotne)**
- https://developers.google.com/search/docs/fundamentals/ai-optimization-guide — aktualizacja 2026-07-10
- https://developers.google.com/search/docs/appearance/ai-features — aktualizacja 2025-12-10
- https://developers.google.com/search/blog/2026/06/gen-ai-performance-reports — 2026-06-03
- https://developers.google.com/search/blog/2025/05/succeeding-in-ai-search — V 2025
- https://developers.google.com/search/docs/fundamentals/creating-helpful-content
- https://guidelines.raterhub.com/searchqualityevaluatorguidelines.pdf — wersja 2025-09-11
- https://blog.google/products-and-platforms/products/search/ai-mode-expands-languages-locations/ — 2025-10-07
- https://blog.google/products-and-platforms/products/search/ai-overview-expansion-may-2025-update/

**Badania**
- https://arxiv.org/pdf/2311.09735 — GEO: Generative Engine Optimization, Aggarwal i in., Princeton
- https://ahrefs.com/blog/llmstxt-study/ — 2026-06-15
- https://ahrefs.com/blog/ai-overviews-reduce-clicks-update/ — 2026-02-04
- https://ahrefs.com/blog/ai-overviews-reduce-clicks/ — IV 2025
- https://ahrefs.com/blog/ai-search-overlap/ — 2025-08-11
- https://ahrefs.com/blog/top-mentioned-sources-are-not-shared-across-ai-assistants/ — 2025-06-12
- https://ahrefs.com/blog/search-rankings-ai-citations/ — VII 2025
- https://ahrefs.com/blog/query-fan-out/
- https://ahrefs.com/blog/track-analyze-ai-traffic/ — 2025-04-04
- https://www.semrush.com/blog/how-ai-shapes-b2b-buying/ — 2026-07-08
- https://www.semrush.com/blog/ai-mode-comparison-study/ — VII 2025
- https://www.semrush.com/blog/ai-search-seo-traffic-study/ — 2025-07-21
- https://www.semrush.com/blog/ai-referral-traffic/
- https://www.semrush.com/blog/ga4-adds-ai-assistant-channel/

**Prasa branżowa**
- https://searchengineland.com/google-zero-click-searches-2026-study-479717 — 2026-06-09
- https://www.searchenginejournal.com/ai-overviews-cut-organic-clicks-38-field-study-finds/573145/ — IV 2026
- https://www.searchenginejournal.com/serp-faq-removal-new-data-challenge-schemas-ai-search-value/574993/ — 2026-05-16
- https://searchengineland.com/schema-markup-ai-search-no-hype-472339 — 2026-03-25
- https://searchengineland.com/does-llms-txt-matter-467740
- https://searchengineland.com/google-llms-txt-chrome-lighthouse-478246
- https://searchengineland.com/guide/query-fan-out
- https://searchengineland.com/guide/how-to-optimize-for-query-fan-out
- https://searchengineland.com/guide/ai-citations-vs-ai-mentions-data
- https://searchengineland.com/guide/ymyl
- https://searchengineland.com/b2b-authority-ai-search-era-456207
- https://searchengineland.com/why-ga4-alone-cant-measure-the-real-impact-of-ai-seo-468387
- https://searchengineland.com/google-search-console-generative-ai-performance-report-in-search-data-bug-485215
- https://searchengineland.com/google-updates-search-quality-raters-guidelines-adding-ai-overview-examples-ymyl-definitions-461908
- https://www.searchenginejournal.com/ai-tools-recommend-brands-but-cite-other-sites-data-shows/587160/
- https://www.searchenginejournal.com/new-data-finds-gap-between-google-rankings-and-llm-citations/561492/
- https://www.searchenginejournal.com/ga4s-ai-assistant-channel-undercounts-your-ai-traffic-how-to-build-one-that-doesnt/580133/
- https://www.searchenginejournal.com/can-you-use-ai-to-write-for-ymyl/558945/
