# Content i on-page SEO: intencje, keyword research, struktura treści, klastry tematyczne

Research pod skill SEO dla mentzen.pl (kancelaria prawo/podatki/księgowość, B2B, WordPress, rynek polski, YMYL).
Data: 2026-08-28. Styl: normatywny.

## TL;DR

1. **Każda fraza przechodzi test SERP przed napisaniem tekstu.** Google już rozstrzygnął, jaki typ, format i kąt treści zaspokaja intencję — publikowanie strony usługowej tam, gdzie rankują artykuły (i odwrotnie), to walka z formatem, którą się przegrywa ([Ahrefs](https://ahrefs.com/blog/search-intent/)).
2. **Treści podatkowe to wprost YMYL** — „filling out tax forms" jest w wytycznych asesorów przykładem tematu Financial Security. Tekst bez podpisanego autora z uprawnieniami, daty stanu prawnego i podstawy prawnej obniża Trust całej witryny; lepiej 40 tekstów sygnowanych niż 400 anonimowych ([General Guidelines, 2025-09-11](https://static.googleusercontent.com/media/guidelines.raterhub.com/en//searchqualityevaluatorguidelines.pdf)).
3. **Priorytetyzuj frazy po potencjale biznesowym, nie po wolumenie.** Fraza 70/mies. o ocenie biznesowej 3 bije frazę 3000/mies. o ocenie 0; w niszy podatkowej niski wolumen to norma, nie problem ([Ahrefs](https://ahrefs.com/blog/republishing-content/); [SEJ, B2B Keyword Research](https://www.searchenginejournal.com/b2b-keyword-research/428962/)).
4. **Kolejność publikacji: najpierw strony usługowe (dół lejka), potem jeden klaster blogowy naraz, dokończony do końca** — nie lista fraz sortowana po „łatwe + wolumen" (wg autora kursu Surfer Academy; wg Sama Dunninga z Breaking B2B).
5. **Jedna fraza główna / jedna intencja = jeden URL.** Kanibalizację diagnozuj w GSC (kilka URL-i zbiera kliknięcia na tę samą frazę); przy pokryciu frazy i intencji — konsoliduj i 301 ([Ahrefs](https://ahrefs.com/blog/keyword-cannibalization/); [Semrush](https://www.semrush.com/blog/keyword-cannibalization-guide/)).
6. **Struktura artykułu: odpowiedź na początku (answer-first/BLUF), TL;DR z konkretem u góry, opisowe H2/H3 (najlepiej pytania), sekcje samodzielne, tekst nasycony encjami.** Długość nie jest celem: korelacja liczby słów z cytowaniem w AI ≈ 0,04, a Google wprost wymienia pisanie „to a particular word count" jako sygnał treści pod wyszukiwarkę ([dane Ahrefs, kurs AEO](https://www.youtube.com/watch?v=uza9GX0E2mw); [Google, helpful content](https://developers.google.com/search/docs/fundamentals/creating-helpful-content)).
7. **FAQ rich results umarły 2026-05-07** — sekcje FAQ zostawiaj dla ludzi i modeli AI, ale nie planuj żadnych prac pod markup `FAQPage` z argumentem „poszerzy snippet" ([SEJ, 2026-05-10](https://www.searchenginejournal.com/google-drops-faq-rich-results-from-search/574429/)).
8. **Linkowanie wewnętrzne: 3–5 kontekstowych linków z body na artykuł, w tym co najmniej jeden do strony usługi; opisowe, różnicowane anchory; każda strona ≤3 kliknięcia od głównej** ([Ahrefs, 2026-03-10](https://ahrefs.com/blog/internal-links-for-seo/)).
9. **Aktualizacja archiwum to najwyżej zwrotny obszar dla kancelarii** — stan prawny starzeje treść z każdą nowelizacją. Aktualizuj realnie (nowy stan prawny, dane, przykłady), nigdy samą datą: Google wymienia podbijanie dat jako sygnał spamu, a testy praktyków potwierdzają, że sama zmiana daty nie działa ([Google](https://developers.google.com/search/docs/fundamentals/creating-helpful-content); wg Szymona Parzycha z webinaru Senuto/Vestigio).
10. **Zasady etyki zawodowej są NADRZĘDNE wobec zaleceń konwersyjnych** (sekcja 10a): publiczny cennik jest dozwolony u doradcy podatkowego (ZE art. 11 ust. 1) i radcy (§ 15 pkt 2 Regulaminu KIRP), zakazany przy treściach adwokackich (Zbiór § 23a ust. 3 lit. m); oferta „płacisz tylko za wynik" zakazana we wszystkich trzech reżimach; CTA i treści promocyjne nie mogą obiecywać skuteczności ani porównywać się z konkurencją.
11. **Zero-click to stan wyjściowy:** obecność AI Overview obniża CTR pozycji 1 o ~58% (badanie na 300 tys. fraz; wcześniejszą wartość ~34,5% z IV 2025 Ahrefs sam skorygował — cytuj wyłącznie update), a AIO dotyczą prawie wyłącznie fraz informacyjnych. Frazy „przegrane" przez AI obsługuj pod widoczność marki, a kliknięcia i leady zbieraj z fraz problemowych, porównawczych, cenowych i narzędziowych ([Ahrefs, 2026-02-04](https://ahrefs.com/blog/ai-overviews-reduce-clicks-update/); [dane Ahrefs, kurs AEO](https://www.youtube.com/watch?v=uza9GX0E2mw)).

---

## 1. Intencja wyszukiwania — pierwsza decyzja, przed frazami i narzędziami

**Rób:**

- Klasyfikuj każdą frazę do jednej z 4 intencji: **informacyjna, nawigacyjna, komercyjna** (porównania, „najlepszy", „ranking", „X vs Y" — etap oceny dostawcy), **transakcyjna** ([Semrush](https://www.semrush.com/blog/search-intent/); [Search Engine Land](https://searchengineland.com/guide/search-intent-seo)).
- **Test 30 sekund: wygoogluj frazę i zobacz, co rankuje.** SERP to odpowiedź Google na pytanie o intencję. Stosuj „3 C" ([Ahrefs](https://ahrefs.com/blog/search-intent/)):
  - *Content type* — artykuł, strona usługowa, narzędzie, kategoria?
  - *Content format* — poradnik, lista, porównanie, kalkulator?
  - *Content angle* — aktualność („zmiany 2026"), prostota, kompletność, koszt?
- Czytaj SERP features (People Also Ask, featured snippet, local pack) jako dodatkowy sygnał intencji i wskazówkę, który fragment sformatować pod wyciągnięcie ([Search Engine Land](https://searchengineland.com/optimize-search-intent-tips-430857)).
- W praktyce wystarczą **dwa kubełki: strony komercyjne i informacyjne** — drobniejsze typologie to zbędna komplikacja (wg Nathana Gotcha z kanału Gotch SEO). Osobna strona na osobny moment ścieżki („kontrola celno-skarbowa — pilna pomoc" vs „przegląd podatkowy spółki") działa i na SEO, i na konwersję.

**Unikaj:**

- Wystawiania strony usługowej na frazę informacyjną („jak rozliczyć estoński CIT") ani artykułu na frazę transakcyjną („biuro rachunkowe Toruń"). Niedopasowanie typu treści to najczęstsza przyczyna, dla której poprawna technicznie strona nie wchodzi do top10.
- Zgadywania intencji z brzmienia frazy. Frazy podatkowe bywają dwuznaczne („ulga na badania i rozwój" — raz SERP informacyjny, raz usługowy); sprawdzaj empirycznie.

---

## 2. Ramka YMYL — filtr dla każdej decyzji on-page

Szczegóły E-E-A-T są w osobnym pliku; tu tylko to, co bezpośrednio steruje treścią:

- „Trust is the most important member of the E-E-A-T family" — a strona YMYL „highly inexpert" ma być oceniona **Lowest** ([General Guidelines 2025-09-11, sekcje 3.4 i 4.5.2](https://static.googleusercontent.com/media/guidelines.raterhub.com/en//searchqualityevaluatorguidelines.pdf)). Konsekwencja: **żaden tekst podatkowy bez podpisanego autora z uprawnieniami (doradca podatkowy, radca prawny — z numerem wpisu w biogramie), podstawy prawnej i daty stanu prawnego.**
- Pokazuj **first-hand expertise**: konkretne sprawy, interpretacje indywidualne, wyroki NSA/WSA, praktyka z postępowań — nie parafrazę ustawy ([Google, helpful content, 2025-12-10](https://developers.google.com/search/docs/fundamentals/creating-helpful-content)).
- Sygnały „search engine-first", których Google każe unikać (tamże): masowa produkcja z „extensive automation", streszczanie cudzych treści bez wartości dodanej, pisanie do zadanej liczby słów, **sztuczne podbijanie dat**, wchodzenie w tematy poza kompetencją dla ruchu, clickbaitowe tytuły.
- Test people-first: czy istniejący odbiorca uznałby tekst za wartościowy, gdyby trafił na niego poza wyszukiwarką? (tamże)

---

## 3. Keyword research dla usług B2B

### 3.1 Zasady doboru

- **Akceptuj niskie wolumeny.** „B2B keyword research works best with precise targeting, low search volumes" — frazy specyficzne dają kwalifikowane leady, choć „look scary in the keyword research document" ([SEJ](https://www.searchenginejournal.com/b2b-keyword-research/428962/)). Najmocniej to samo mówi Sam Dunning (Breaking B2B): ludzie nie wpisują „doradca podatkowy dla e-commerce" dla zabawy — szukają, bo potrzebują usługi ([film](https://www.youtube.com/watch?v=N1m9hlMsMKg)).
- **Oceniaj każdą frazę czterema atrybutami** (framework BID/„sweet spot", zbieżny u Ahrefs i Surfer Academy): potencjał biznesowy (skala 0–3, gdzie 3 = usługa jest nieodzownym rozwiązaniem problemu z frazy), intencja (z SERP, nie z głowy), trudność (patrz niżej), popyt ([Ahrefs](https://ahrefs.com/blog/republishing-content/)).
- **Trudność sprawdzaj na poziomie konkretnych wyników, nie syntetycznego KD.** KD 39 może kryć w top10 domeny o DR 0–17 — cele trywialne. Zejdź do listy wyników i sprawdź profile linkowe (wg Nathana Gotcha; to samo zaleca kurs AEO Ahrefs). Semrush dodatkowo rozróżnia KD od Personal KD (trudność dla twojej domeny) ([Semrush](https://www.semrush.com/analytics/keywordoverview/)).
- **Filtr AI przed zatwierdzeniem frazy:** jeśli AI Overview daje kompletną odpowiedź, kliknięcia nie będzie — frazę targetuj pod wzmiankę marki, nie pod ruch. AIO pojawia się przy ~21% fraz ogółem, ale 58% zapytań pytających i 99,9% fraz wywołujących AIO ma intencję informacyjną [dane Ahrefs, kurs AEO](https://www.youtube.com/watch?v=uza9GX0E2mw). Kliknięcie wciąż jest do wzięcia przy zapytaniach wymagających działania: kalkulator, wzór, generator, checker.
- Frazy top-of-funnel nie są złe — są złe **bez ścieżki konwersji**. Case PandaDoc: „contract template" (300 tys./mies.) działa, bo za rankingiem stoi katalog szablonów → download → mail → nurture. „Fraza bez ścieżki konwersji to metryka próżności; fraza ze ścieżką to pipeline" (wg autora kursu Surfer Academy, [film](https://www.youtube.com/watch?v=7DRO4rEIHDk)).

**Unikaj:** budowania kalendarza na wolumenie (prowadzi wprost do „topics only because they seem trending" — sygnał spamowy wg Google) i ufania wolumenom narzędzi dla polskiej niszy — dane dla PL bywają zaokrąglane do zera przy realnym ruchu [do weryfikacji: brak twardego źródła na skalę zjawiska].

### 3.2 Money keyword matrix (wg Sama Dunninga z Breaking B2B)

Arkusz czterech kolumn, z których składa się frazy long-tail ([film](https://www.youtube.com/watch?v=N1m9hlMsMKg)):

1. **Nazwy usługi** — jak klienci ją nazywają, nie jak nazywa ją oferta („księgowość dla spółki z o.o.", nie „usługi rachunkowości finansowej").
2. **Money niches** — branże/segmenty, które już płacą dobrze i mało churnują.
3. **Konkurenci** — tylko gdy istnieją marki realnie wyszukiwane z nazwy; w PL to głównie oprogramowanie (inFakt, wFirma, ifirma) — frazy „inFakt czy biuro rachunkowe" to wysokointencyjne porównania.
4. **Problemy i objawy** — dolegliwości przed poznaniem nazwy rozwiązania. Dla kancelarii to najcenniejsza kolumna, bo problemy są ostre i nazwane: „kontrola celno-skarbowa co robić", „zajęcie rachunku", „utrata estońskiego CIT". To frazy o najwyższej intencji — człowiek jest w trybie zakupu usługi, tylko o tym nie wie.

Macierz wypełniaj z działem sprzedaży i obsługi klienta — oni znają realne sformułowania. Warianty wyczerpuj do końca; macierz generuje dziesiątki podobnych fraz, więc pilnuj reguły **jedna strona na intencję, nie na frazę**.

Modyfikator „best" po polsku działa słabiej; jego funkcję przejmują **frazy cenowe i rankingowe**: „ile kosztuje doradca podatkowy", „cennik biura rachunkowego", „ranking biur rachunkowych", „opinie".

### 3.3 Źródła fraz (od najmocniejszego)

1. **Google Search Console** — realne zapytania, którymi ludzie już trafiają do serwisu; najdokładniejsze pojedyncze źródło (wg autora kursu Surfer Academy).
2. **Język klientów:** pytania z konsultacji, maile, komentarze pod wideo, grupy przedsiębiorców — long-tail „w słowach, którymi ludzie opisują problem" ([Search Engine Land](https://searchengineland.com/guide/long-tail-keywords-seo)).
3. Autouzupełnianie Google, People Also Ask, „People also search for".
4. Sitemapa własna i konkurenta wrzucona do LLM z prośbą o luki treściowe; „30 pytań przed zakupem tej usługi" jako lista zalążkowa.
5. Frazy 3+ wyrazowe z seed keywords, odsiewane po intencji ([Ahrefs](https://ahrefs.com/blog/long-tail-keywords/)).

---

## 4. Klastry tematyczne i priorytetyzacja

**Model:** pillar obejmuje temat szeroko, clustery rozwijają podtematy; linkowanie pillar ↔ cluster plus poziome między clusterami. Google nie używa terminu „topical authority" — najbliższe kryteria to „Does the site have a primary purpose or focus?" i treść pisana przez kogoś, kto „demonstrably knows the topic well". Klastry są **sposobem realizacji** tych kryteriów, nie osobnym czynnikiem rankingowym [do weryfikacji: brak potwierdzenia Google, że to wyodrębniony sygnał] ([helpful content](https://developers.google.com/search/docs/fundamentals/creating-helpful-content); [Ahrefs](https://ahrefs.com/blog/internal-links-for-seo/)).

**Kolejność pracy (zbieżna u trzech niezależnych autorów — Surfer Academy, Breaking B2B, Gotch SEO):**

1. **Najpierw money pages, szeroko** — strony usług pod frazy transakcyjne/komercyjne. Typowy błąd: 10 tekstów informacyjnych i zdziwienie, że nikt nie kupuje — ruch nie ma gdzie konwertować.
2. **Potem JEDEN klaster podpięty do money page, budowany do końca**, zanim zacznie się następny. Mechanizm: Google nie ocenia strony w izolacji — przy „jak przejść kontrolę podatkową" sprawdza, czy serwis pokrywa też pytania sąsiednie tej samej osoby. Pokrycie pełnej ścieżki ułatwia rankowanie na każdą frazę klastra, także trudne.
3. Cienka strona (kilka podstron, zero bloga) to punkt startu, nie problem — rozbudowa serwisu jest konsekwencją strategii fraz, nie warunkiem wstępnym (wg Sama Dunninga).

**Autorytet tematyczny to głębokość, nie szerokość.** Kevin Indig (cyt. w filmie Semrush): HubSpot stracił ruch przez oddalenie się od tematów rdzeniowych. Dla kancelarii: lepiej dominować w wąskim wycinku (spory z KAS, estoński CIT) niż mieć po dwa teksty w piętnastu obszarach ([film](https://www.youtube.com/watch?v=VOb_QjlrgpE)).

**Unikaj:**

- Sztywnych silosów zakazujących linków między klastrami ([Ahrefs](https://ahrefs.com/blog/internal-links-for-seo/)).
- Publikowania clusterów szybciej, niż da się je sygnować nazwiskiem eksperta — w YMYL wolumen bez Trustu działa przeciw witrynie.
- Progów typu „15–25 clusterów na pillar" — pochodzą z blogów agencyjnych, bez źródła pierwotnego [do weryfikacji — nie opieraj na tym planu].

### Kanibalizacja

- Diagnoza w GSC: Performance → klik na frazę → jeśli kliknięcia zbiera więcej niż jeden URL, to sygnał ([Semrush](https://www.semrush.com/blog/keyword-cannibalization-guide/); [Search Engine Land](https://searchengineland.com/guide/keyword-cannibalization)).
- Ta sama fraza i intencja → **konsoliduj**: najlepsze fragmenty do silniejszego URL-a (po klikach i profilu linków), drugi 301 ([Ahrefs](https://ahrefs.com/blog/keyword-cannibalization/)).
- Różna intencja → nie konsoliduj; rozdziel frazy i popraw anchory.
- Słownictwo podatkowe mocno się zazębia („CIT estoński" = „ryczałt od dochodów spółek" — jedna intencja, jedna strona); rozdział fraz pilnuj w arkuszu, zanim tekst powstanie (wg Macieja Cherubina, [film](https://www.youtube.com/watch?v=yUp_Emk8KAo)).

---

## 5. Struktura artykułu eksperckiego

### 5.1 Szkielet

- **Kolejność pisania: tytuł → pełna struktura H2/H3 → dopiero treść.** Struktura nagłówków jest planem encji i tezą tekstu (wg Macieja Cherubina). Przed konspektem przeczytaj top 5 wyników: co pokrywają (minimum wejścia), czego nie pokrywają (twoja szansa), jaka jest średnia długość (benchmark, nie cel), czy ktoś bierze unikalny kąt (wg autora kursu Surfer Academy).
- Jeden H1; hierarchia H2/H3; nagłówki opisowe, **najlepiej w formie pytań klientów, z bezpośrednią odpowiedzią w pierwszym–drugim zdaniu pod nagłówkiem** — podstawa prawna i zastrzeżenia dopiero potem (wg Szymona Parzycha z webinaru Senuto/Vestigio, [film](https://www.youtube.com/watch?v=MKwi0qmVSS8)). „Paragraphs and sections, along with headings that provide clear structure" to jedyna rekomendacja formatowania w oficjalnym przewodniku Google pod AI ([AI optimization guide, 2026-07-10](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide)).
- **Answer-first / BLUF / odwrócona piramida:** odpowiedź na pytanie z tytułu pada na samym początku („Ile kosztuje X?" → najpierw widełki, potem szczegóły). TL;DR u góry z konkretem: kto, od kiedy, jaki termin, jaka stawka. Wstęp to skondensowana pigułka, nie literackie wprowadzenie — są przesłanki, że boty na podstawie pierwszych akapitów decydują, czy przetwarzać resztę strony (wg Parzycha; wg Cherubina; potwierdzone kierunkowo w kursie AEO Ahrefs — modele ważą początek fragmentu wyżej).
- **Treść atomowa:** jedna sekcja / jeden akapit = jedna samodzielna idea, zrozumiała po wycięciu z kontekstu. Test: przeczytaj sekcję H2 w oderwaniu; jeśli bez kontekstu nie ma sensu — przepisz. Unikaj zaimków „to", „tamto" — używaj nazw ([kurs AEO Ahrefs](https://www.youtube.com/watch?v=uza9GX0E2mw)).
- **Jedna intencja na artykuł.** Nie upychaj całej wiedzy o temacie w jednym tekście (wg Parzycha).
- Klikalny spis treści; podstawa prawna + widoczny „stan prawny na DD.MM.RRRR" — to konkret, który adresuje kryterium błędów faktograficznych i jest chętnie cytowany.

### 5.2 Encje zamiast zagęszczania frazy

Przestań mierzyć tekst liczbą wystąpień frazy; mierz liczbą i trafnością informacji. Encja = coś na hasło w Wikipedii; algorytmy modelują rzeczywistość relacjami encji (wg Macieja Cherubina; to samo jako zasada „entity-rich writing" w kursie AEO Ahrefs). Dla kancelarii encjami są: ustawy i ich skróty, organy (KAS, US, ZUS, KRS), formy opodatkowania, terminy ustawowe, sygnatury i linie orzecznicze, systemy (KSeF, JPK, e-Doręczenia), role zawodowe. **Strona o kontroli podatkowej, która nie wspomina o czynnościach sprawdzających, korekcie deklaracji i czynnym żalu, jest tematycznie płytka niezależnie od liczby wystąpień „kontrola podatkowa".** Tezę Cherubina, że fraza „może w ogóle nie paść", traktuj ostrożnie — frazę trzymaj w title, H1 i pierwszym akapicie, encje dokładaj wokół.

Styl (wg Parzycha, spójne z Ahrefs): konkretne liczby zamiast „znacząco", ton oznajmujący bez asekuracji, krótkie zdania podmiot–orzeczenie–dopełnienie, terminologia fachowa wyjaśniana przy pierwszym użyciu, wyróżniki oferty mierzalne.

### 5.3 Formaty i długość

- **Formaty chętnie cytowane przez AIO:** listy (78% odpowiedzi AIO zawiera listę — badanie Surfera cyt. przez Parzycha), tabele (**każdy wiersz musi być samodzielnym faktem**, bo model wyciąga pojedyncze wiersze), instrukcje krok po kroku, porównania X vs Y, FAQ.
- **Długość nie jest sygnałem:** korelacja liczby słów z cytowaniem 0,04; 53,4% cytowanych stron ma <1000 słów [dane Ahrefs]. Google: nie pisz do zadanej liczby słów. Mediana czołówki to benchmark kompletności, nie target.
- Nie rozbijaj artykułu na mikro-URL-e — Google odradza „tiny pieces" ([AI optimization guide](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide)).
- Multimodalnie: zamiast kolejnego artykułu wzmocnij istniejący infografiką/wideo z transkrypcją, opisowym `alt` i `figure`/`figcaption` (wg Parzycha).

### 5.4 FAQ — status po deprecjacji

FAQ rich results **przestały pojawiać się w Google Search 2026-05-07**; raporty i wsparcie narzędzi wygaszone do sierpnia 2026 ([SEJ, 2026-05-10](https://www.searchenginejournal.com/google-drops-faq-rich-results-from-search/574429/); tło: [Google, 2023-08](https://developers.google.com/search/blog/2023/08/howto-faq-changes)). Rób: sekcje FAQ z realnymi pytaniami klientów (wartość dla ludzi i modeli, miejsce na subpytania). Istniejący markup `FAQPage` może zostać — nie szkodzi. Unikaj: nowych wdrożeń `FAQPage` „bo snippet" i generycznego FAQ doklejanego do każdego artykułu.

### 5.5 Information gain — fosa treściowa

Człowiek dokłada to, czego AI nie ma (wg autora kursu Surfer Academy — najmocniejsza teza materiału): doświadczenie z realnych spraw, opinia, dane własne, cytaty ekspertów (nie muszą być własne), konkretne przykłady z liczbami. Poprzeczka: nie „dobrze", tylko wyraźnie lepiej niż istniejące wyniki na tę frazę — większość stron nie dostaje ruchu, bo jest taka sama jak wszystko inne. Google wprost premiuje „original information, research, or analysis" i „insights beyond the obvious". Proces produkcyjny: szkic AI → redakcja przez człowieka-praktyka; **nie „humanizuj" tekstu AI kolejnym AI** (wg Nathana Gotcha). Autorskie frameworki nazywaj marką i definiuj wprost — inaczej modele wchłoną je jako wiedzę ogólną bez przypisania ([kurs AEO Ahrefs](https://www.youtube.com/watch?v=uza9GX0E2mw)).

---

## 6. Title, meta description, H1, URL

### Title ([Google, Title links, 2025-12-10](https://developers.google.com/search/docs/appearance/title-link))

- Opisowy, zwięzły, fraza z przodu; marka raz, na początku albo na końcu, po separatorze (`-`, `:`, `|`). **Google nie podaje limitu znaków** — dokumentacja mówi wprost „there's no limit on how long a `<title>` element can be", więc „~60 znaków" to heurystyka narzędziowa, nie reguła; tytuł jest ucinany do szerokości urządzenia.
- Różnicuj wg typu strony: poradnik — opisowo i wprost; strona usługowa — fraza + korzyść (wg autora kursu Surfer Academy).
- Unikaj: keyword stuffingu, **boilerplate'u w szablonie WordPressa** („%tytuł% | Kancelaria — Doradztwo podatkowe, prawo, księgowość" na każdym URL-u), dat, których nie utrzymasz (nieaktualny tytuł to jeden z powodów przepisania go przez Google).

### Meta description ([Google, Snippets, 2026-04-20](https://developers.google.com/search/docs/appearance/snippet))

- Unikalna per strona, pisana jak copy: konkret, czynność, co czytelnik dostanie. To **kandydat** na snippet, nie gwarancja — Google używa jej, gdy opisuje stronę lepiej niż treść. Brak oficjalnego limitu znaków.
- Unikaj: duplikatów i listy fraz zamiast zdania.
- `data-nosnippet` zakładaj na disclaimery prawne, żeby nie wypierały merytoryki ze snippetu — ale audytuj, czy dyrektywy `nosnippet`/`max-snippet` nie siedzą gdzieś „na wszelki wypadek", bo blokują też obecność w AI Overviews (wg Parzycha).

### H1 i URL

- Jeden H1, zgodny z title (nie musi być identyczny); **nie** nazwa firmy na podstronach usługowych — traci się główny sygnał tematyczny.
- URL krótki, opisowy, z frazą, w czytelnej hierarchii; bez losowych ciągów z CMS-a.

---

## 7. Linkowanie wewnętrzne

„Jeden z najlepszych stosunków nakładu do efektu w całym SEO" (wg autora kursu Surfer Academy) — to fizyczna realizacja klastra.

**Rób** ([Ahrefs, 2026-03-10](https://ahrefs.com/blog/internal-links-for-seo/); [Google](https://developers.google.com/search/docs/crawling-indexing/links-crawlable); [Search Engine Land](https://searchengineland.com/internal-links-seo-best-practices-examples-tips-448047)):

- 3–5 linków kontekstowych z body na artykuł; linki w treści > nawigacja > stopka.
- **Z każdego artykułu co najmniej jeden link do właściwej strony usługi** — sygnał SEO i ścieżka konwersji naraz; kluczowe linki i CTA wyżej na stronie (wyższa klikalność).
- Opisowe, różnicowane anchory („rozliczenie estońskiego CIT" / „warunki ryczałtu od dochodów spółek" — nie ten sam anchor wszędzie).
- Z mocnych stron do słabych; każda strona ≤3 kliknięcia od głównej; **linkuj ze starych wpisów do nowych** — boty odwiedzają stare i tam znajdą nowy URL (wg Parzycha).

**Unikaj:** stron osieroconych, linków 4XX, `nofollow` wewnętrznie, anchorów „kliknij tutaj", nawigacji tylko w JavaScripcie, sztywnych silosów.

---

## 8. Strony usługowe vs blog

| Wymiar | Strona usługi | Artykuł blogowy |
|---|---|---|
| Intencja | transakcyjna / komercyjna | informacyjna / komercyjna |
| Cel | konwersja | edukacja, zasięg, Trust |
| Fraza | „doradca podatkowy [miasto]", „księgowość spółki z o.o." | „jak rozliczyć [X]", „[podatek] zmiany 2026" |
| Rola w linkowaniu | odbiorca linków | nadawca linków do usług |

- Landing wystawiaj tylko na frazy z komponentem transakcyjnym w SERP; celuj w niższe wolumeny, bo landing jest dochodowy ([Ahrefs, Landing Page SEO](https://ahrefs.com/blog/landing-page-seo/)).
- **Buduj wąskie podstrony pod wąskie intencje** zamiast jednej ogólnej „obsługi podatkowej": osobno kontrola celno-skarbowa, wdrożenie KSeF, estoński CIT, ceny transferowe — widoczność + konwersja, bo treść mówi o sytuacji odwiedzającego (wg Macieja Cherubina). Ale nie rozbijaj jednej oferty na wiele cienkich URL-i — Google potrafi wypromować blogowy tekst ponad rozproszoną stronę komercyjną ([SEJ, Ask An SEO](https://www.searchenginejournal.com/ask-an-seo-how-do-i-balance-content-that-converts-with-content-that-builds-brand-authority/566261/)).
- Elementy strony usługi, które konwertują (wg Sama Dunninga, zbieżne z [Search Engine Land](https://searchengineland.com/landing-pages-seo-conversions-447672)): diagnoza problemu → jego koszt biznesowy → obraz stanu po rozwiązaniu; opinie i case'y wplecione w stronę (nie w osobnej zakładce); FAQ; **jawny cennik lub widełki** (w polskiej branży prawniczej rzadkość = przewaga + odpowiedź na frazy „ile kosztuje"); **jawne „dla kogo nie jesteśmy"** — odsiewa złe leady i buduje wiarygodność. Uwaga: opinie, case'y i cennik podlegają twardym ograniczeniom etyki zawodowej z sekcji 10a — przy treściach adwokackich opinie i publiczny cennik odpadają w całości.
- Planuj serwis arkuszem optymalizacyjnym: jedna podstrona = jeden wiersz (URL, intencja, frazy przypisane wyłącznie tu, title, H1, cel konwersji) i 10–15 powtarzalnymi widokami sekcji; struktura i treść przed grafiką, sekcja rozprowadzająca ruch blisko góry, żadnych ścian tekstu „nagłówek + 10 paragrafów" (wg Macieja Cherubina).
- Strona kategorii oferty („Doradztwo podatkowe"): zwięzły opis z encjami **nad** listą usług, linki w dół do usług — nie artykuł o podatkach; artykuł należy na blog (wg Cherubina).
- Treść informacyjną organizuj jako **bazę wiedzy / stale aktualizowane węzły tematyczne** z kategoriami, nie strumień newsów; blog newsowy zostaw na faktyczne zmiany legislacyjne, gdzie data ma wartość (wg Dale'a Daviesa i Charliego Marchanta z Exposure Ninja, [film](https://www.youtube.com/watch?v=vrGLaJOAKas)).
- WordPress: usługi jako *pages* w hierarchii, blog jako *posts* ([Search Engine Land](https://searchengineland.com/wordpress-pages-vs-posts-385682)).

---

## 9. Aktualizacja, konsolidacja, pruning, sezonowość

Najwyżej zwrotny obszar dla kancelarii — archiwum podatkowe starzeje się z każdą nowelizacją, nie po 2–5 latach.

**Rób:**

- Kandydatów wybieraj po danych: spadek ruchu 12-mies. + niska trudność (Ahrefs sugeruje KD ≤ 40) + wysoki potencjał biznesowy ([Ahrefs, 2025-10-30](https://ahrefs.com/blog/republishing-content/)). Wariant „sleeper pages": strony ze spadkiem ruchu **i** przyzwoitą liczbą linków — najszybsza droga do widoczności, bo autorytet już jest ([kurs AEO Ahrefs](https://www.youtube.com/watch?v=uza9GX0E2mw)).
- **Rozróżnij problem treści od problemu autorytetu:** jeśli wyprzedzające strony mają wyższy UR/więcej linków, przepisywanie nic nie da (Ahrefs).
- Zmieniaj realnie: nowy stan prawny i liczby, luki względem top10, information gain, tytuł/meta/linkowanie. Cykl aktualizacji artykułów o stawkach, limitach i terminach planuj **przed** wejściem przepisów w życie (wg Parzycha).
- Konsoliduj duplikaty (jeden mocny URL + 301); treści o przepisach uchylonych usuwaj (410) lub przekierowuj. Pruning poprawia średni obraz jakości i budżet crawlowania; wyjątek — treść, która konwertuje, zostaje ([Semrush](https://www.semrush.com/blog/content-pruning/); [SEJ](https://www.searchenginejournal.com/content-pruning-seo/375066/); wg Parzycha).
- **Świeżość jest silnym sygnałem w AI:** 89,7% najczęściej cytowanych przez ChatGPT stron zaktualizowano w 2025, 76% w ostatnich 30 dniach [dane Ahrefs]; URL-e cytowane przez AI są średnio o 25,7% świeższe niż w klasycznym SERP ([Ahrefs](https://ahrefs.com/blog/republishing-content/)).
- **Sezonowość — publikuj 4–6 tygodni przed szczytem, nie w jego trakcie** (wg Exposure Ninja; ich case sezonowy: +280% przychodu organicznego). Polski kalendarz: styczeń–kwiecień (PIT, wybór formy opodatkowania do 20 lutego), koniec roku (zamknięcie ksiąg, planowanie formy na kolejny rok), okresy sprawozdawcze spółek, terminy wdrożeniowe przepisów (KSeF, składka zdrowotna) — sezony sztuczne, ale przewidywalne z dużym wyprzedzeniem.

**Unikaj:**

- **Podbijania samej daty** — sygnał spamowy wg Google ([helpful content](https://developers.google.com/search/docs/fundamentals/creating-helpful-content)); Parzych testował: nie działa; Ahrefs ostrzega przed „binary trust signal" przy nadużyciu. Zamiast tego dopisz stan prawny i nową liczbę.
- Aktualizowania stron świeżo po dużej zmianie — daj im czas na stabilizację.

Raportowane efekty (przykłady, nie gwarancje): +302% ruchu po przepisaniu artykułu, 3× wzrost cytowań w AI po dużej aktualizacji (Ahrefs, 2025-10-30); wzrost kliknięć HubSpot po usunięciu ~3000 starych wpisów (SEJ).

---

## 10. CTA i konwersja treści blogowych

- **CTA dopasowane do tematu artykułu i etapu lejka**, nie ogólne „Skontaktuj się": artykuł o estońskim CIT → „Sprawdź, czy Twoja spółka spełnia warunki — bezpłatna analiza wstępna" ([SEJ, CTAs for B2B](https://www.searchenginejournal.com/how-to-write-ctas-for-b2b-a-call-to-action-guide-for-businesses-with-10-examples/457392/)).
- Konkretny czasownik („Umów konsultację podatkową"), nie „Wyślij"; rozważ pierwszą osobę („Zamów moją analizę").
- Nie żądaj dużego zaangażowania (audyt, rozmowa) przy treści czysto definicyjnej — kupujący B2B robią research na długo przed kontaktem, treść ma obsłużyć różne etapy ([Backlinko](https://backlinko.com/hub/content/b2b)).
- Testuj jedną zmienną naraz ([Semrush](https://www.semrush.com/blog/lead-generation-strategies/)); CTA i kluczowe linki wyżej na stronie ([Ahrefs](https://ahrefs.com/blog/internal-links-for-seo/)).
- **Metryka sukcesu treści to zapytania ofertowe, nie sesje.** Blog podatkowy jest z natury top-of-funnel, czyli tam, gdzie uderza AIO — zanim ktokolwiek zaproponuje przebudowę, zestaw ruch vs formularze kontaktowe w tym samym okresie (wg Parzycha; to samo mówią Exposure Ninja: mierz kwalifikowane konwersje, nie ruch).
- Zastrzeżenie „nie stanowi porady prawnej/podatkowej" — wymóg praktyki i element Trustu; wstawiaj w `data-nosnippet`, żeby nie wypierało merytoryki ze snippetu (General Guidelines 4.5.3).
- Granice treści CTA i przekazu cenowego wyznacza etyka zawodowa — sekcja 10a, nadrzędna wobec wszystkich zaleceń wyżej.

---

## 10a. Ograniczenia etyki zawodowej — NADRZĘDNE wobec zaleceń konwersyjnych

Reguły z tej sekcji mają pierwszeństwo przed każdym zaleceniem z sekcji 8 i 10: taktyka konwersyjna sprzeczna z zasadami etyki nie wchodzi do skilla niezależnie od skuteczności. Podstawy (stan na 2026-08-28, teksty pierwotne): Zasady etyki doradców podatkowych (ZE, t.j. uchwała KRDP 40/2026), ustawa o doradztwie podatkowym (u.d.p.), Kodeks Etyki Radcy Prawnego (KERP, t.j. 2023) z Regulaminem wykonywania zawodu, Zbiór Zasad Etyki Adwokackiej (Zbiór, t.j. 2022). Treść sygnowaną nazwiskiem ocenia kodeks autora; treść o kancelarii jako całości — reżim najostrzejszy dla danego twierdzenia (adwokacki, jeśli w zespole jest adwokat). Pełne omówienie z cytatami: `_notes/uzup-etyka-doradcy.md`, `_notes/uzup-etyka-prawnicy.md`.

**Cenniki**

- DOZWOLONE (doradca podatkowy): publiczny cennik lub widełki wręcz wspierają obowiązek określenia zasad wynagrodzenia przed rozpoczęciem usługi (ZE art. 11 ust. 1; u.d.p. art. 41a ust. 1 — wynagrodzenie ustala umowa z klientem).
- DOZWOLONE (radca prawny): § 15 pkt 2 Regulaminu KIRP dopuszcza podawanie zasad kształtowania wynagrodzenia, w tym jego formy i wysokości.
- ZAKAZANE (adwokat): stawki wolno podać wyłącznie na życzenie klienta lub w ofercie skierowanej do potencjalnego klienta (Zbiór § 23a ust. 3 lit. m) — publiczny cennik na ogólnodostępnej stronie nie mieści się w tym trybie.
- WARUNKOWE (treść przekazu cenowego): cena z zakresem świadczenia, informacją o VAT i kosztach dodatkowych — cena wywoławcza bez obowiązkowych dopłat wprowadza w błąd (ZE art. 9 lit. a; u.z.n.k. art. 16 ust. 1 pkt 2; u adwokata § 23a ust. 4).
- ZAKAZANE (wszyscy): model „płacisz tylko od wyniku" / „no win, no fee" (ZE art. 11 ust. 3; KERP art. 36 ust. 3; Zbiór § 50) — komunikować wolno wyłącznie model mieszany: wynagrodzenie podstawowe plus ewentualna premia za pomyślny wynik. ZAKAZANE też porównania cenowe z innymi doradcami i kancelariami („taniej niż konkurencja", „najniższe stawki w mieście" — ZE art. 9b ust. 2 lit. a; KERP art. 32 ust. 1 pkt 7).

**CTA i treści promocyjne**

- ZAKAZANE: obiecywanie lub gwarantowanie skuteczności i wywoływanie nieuzasadnionych oczekiwań co do wyniku (ZE art. 9b ust. 2 lit. b; KERP art. 32 ust. 1 pkt 5 lit. e; Zbiór § 23b ust. 2 lit. a). CTA „odzyskamy Twój VAT" odpada; „sprawdź, czy Twoja spółka spełnia warunki" jest w porządku.
- ZAKAZANE: sugerowanie znajomości i dojść w organach (ZE art. 9b ust. 2 lit. c; KERP art. 32 ust. 1 pkt 5 lit. a; Zbiór § 23b ust. 2 lit. b) oraz superlatywy porównawcze — „najlepsza kancelaria", własny ranking z sobą na czele (ZE art. 9b ust. 2 lit. a; KERP art. 32 ust. 1 pkt 7; Zbiór § 23b ust. 2 lit. c).
- WARUNKOWE: promocje czasowe i „bezpłatna analiza" — bez fałszywej ograniczonej dostępności i fałszywego „gratis" (u.p.n.p.r. art. 7 pkt 7 i 20); nagłówki bez przedstawiania grup przedsiębiorców w niekorzystnym świetle i bez straszenia — „fiskus zabierze Ci wszystko" narusza ZE art. 5b ust. 1, spójnie z QRG 5.2 o sensacyjnych tytułach.
- WARUNKOWE (adwokat): informowanie ma być „nieukierunkowane na udzielenie adwokatowi konkretnego zlecenia" (Zbiór § 23a ust. 1 lit. e) — przy treściach sygnowanych przez adwokata CTA ogranicz do neutralnego kontaktu, bez presji sprzedażowej.
- ZAKAZANE: cold mailing i niezamówione oferty w obu reżimach prawniczych (KERP art. 32 ust. 1 pkt 6; Zbiór § 23b ust. 1 i 4) — lead nurturing buduj wyłącznie na zgodach.

---

## 11. Zastosowanie dla mentzen.pl

1. **Źródło prawdy: mapa fraza → intencja → typ URL** (blog / strona usługi / strona zespołu / FAQ), sprawdzana przy każdym nowym tekście; jedna fraza główna = jeden URL. Typologia robocza [do weryfikacji w danych GSC]:

| Typ | Wzorzec | Intencja | Docelowy URL |
|---|---|---|---|
| Definicyjna | „czym jest [ulga]" | informacyjna | blog / pillar |
| Proceduralna | „jak rozliczyć [X]", „termin [Y]" | informacyjna | blog / cluster |
| Zmiana prawa | „[podatek] zmiany 2026" | informacyjna, sezonowa | blog aktualizowany |
| Porównawcza | „[A] czy [B]", „ile kosztuje [X]" | komercyjna | blog + mocne CTA |
| Usługowa | „doradca podatkowy [miasto]" | transakcyjna | strona usługi |
| Problemowa | „kontrola podatkowa co robić" | transakcyjna/pilna | strona usługi + case |

2. **Frazy definicyjne oddaj AIO bez żalu** (obecność marki, nie ruch); leady zbieraj z fraz problemowych, porównawczych, cenowych i z narzędzi (kalkulator estońskiego CIT, kalkulator składki zdrowotnej — modyfikatory „kalkulator/wzór/checker" wciąż dają kliknięcia).
3. **Listicle „ranking kancelarii" z sobą na pierwszym miejscu — nie rób.** W YMYL to widoczny konflikt interesów, a porównywanie się z innymi doradcami i kancelariami jest wprost zakazane (ZE art. 9b ust. 2 lit. a; KERP art. 32 ust. 1 pkt 7; Zbiór § 23b ust. 2 lit. c — sekcja 10a). Bezpieczny odpowiednik o tej samej intencji porównawczej: „księgowość online vs biuro rachunkowe vs księgowa in-house dla spółki z o.o.", „kiedy doradca podatkowy, a kiedy wystarczy księgowa" (przełożenie rady Sama Dunninga).
4. **Przewaga nie do skopiowania przez content farmy:** clustery na realnych sprawach, interpretacjach i wyrokach z sygnaturami (encje + Trust naraz), tabele porównawcze form opodatkowania z samowystarczalnymi wierszami, agregowane i anonimizowane dane własne z postępowań — nigdy przypadki identyfikowalne.
5. **Rytm roczny:** kalendarz sezonów podatkowych (sekcja 9) + rejestr treści: fraza, URL, autor, data stanu prawnego, data następnego przeglądu. Cykl przeglądu archiwum sprzężony z kalendarzem legislacyjnym, nie z kalendarzem redakcyjnym.
6. **Checklista per tekst:** SERP-test frazy → GSC-test kanibalizacji → autor z uprawnieniami → H1 + struktura nagłówków przed treścią → odpowiedź i TL;DR na górze → podstawa prawna + stan prawny → information gain → 3–5 linków wewnętrznych (≥1 do usługi) → unikalny title bez boilerplate'u → CTA pod temat i etap → filtr etyki zawodowej (sekcja 10a) → wpis do rejestru → monitoring Generative AI performance report w GSC.

---

## Rozbieżności między źródłami

- **Długość treści:** Surfer Academy każe traktować średnią długość top wyników jako benchmark; Ahrefs pokazuje zerową korelację długości z cytowaniem, Google odradza pisanie do liczby znaków. Rozstrzygnięcie: mediana czołówki = wskaźnik kompletności tematu, nigdy target objętości.
- **Fraza w treści:** Cherubin twierdzi, że przy dobrych encjach fraza może nie paść wcale; praktyka bezpieczna dla polskiego B2B — fraza w title, H1 i pierwszym akapicie, encje wokół.
- **FAQ:** źródła sprzed maja 2026 zalecają markup `FAQPage` dla rich resultów — nieaktualne; sekcje FAQ zostają wyłącznie ze względu na użytkowników i systemy generatywne.
- **Tempo efektów:** „90 dni przy KD 0–10" Sama Dunninga dotyczy rynku anglojęzycznego bez YMYL; dla prawa i podatków w Polsce zakładaj dłużej — kierunek (wąskie frazy branżowo-lokalne rankują szybciej) pozostaje słuszny.

## Źródła główne

Google (pierwotne): [helpful content, 2025-12-10](https://developers.google.com/search/docs/fundamentals/creating-helpful-content) · [AI optimization guide, 2026-07-10](https://developers.google.com/search/docs/fundamentals/ai-optimization-guide) · [Title links, 2025-12-10](https://developers.google.com/search/docs/appearance/title-link) · [Snippets, 2026-04-20](https://developers.google.com/search/docs/appearance/snippet) · [Link best practices](https://developers.google.com/search/docs/crawling-indexing/links-crawlable) · [Quality Rater Guidelines PDF, 2025-09-11](https://static.googleusercontent.com/media/guidelines.raterhub.com/en//searchqualityevaluatorguidelines.pdf)

Branżowe: Ahrefs ([search intent](https://ahrefs.com/blog/search-intent/), [long-tail](https://ahrefs.com/blog/long-tail-keywords/), [internal links, 2026-03-10](https://ahrefs.com/blog/internal-links-for-seo/), [republishing, 2025-10-30](https://ahrefs.com/blog/republishing-content/), [cannibalization](https://ahrefs.com/blog/keyword-cannibalization/), [landing pages](https://ahrefs.com/blog/landing-page-seo/), [AIO −58% CTR, update 2026-02-04](https://ahrefs.com/blog/ai-overviews-reduce-clicks-update/)) · SEJ ([FAQ deprecation, 2026-05-10](https://www.searchenginejournal.com/google-drops-faq-rich-results-from-search/574429/), [B2B keyword research](https://www.searchenginejournal.com/b2b-keyword-research/428962/), [B2B CTAs](https://www.searchenginejournal.com/how-to-write-ctas-for-b2b-a-call-to-action-guide-for-businesses-with-10-examples/457392/), [pruning](https://www.searchenginejournal.com/content-pruning-seo/375066/)) · Semrush ([intent](https://www.semrush.com/blog/search-intent/), [cannibalization](https://www.semrush.com/blog/keyword-cannibalization-guide/), [pruning](https://www.semrush.com/blog/content-pruning/)) · Search Engine Land ([intent](https://searchengineland.com/guide/search-intent-seo), [long-tail](https://searchengineland.com/guide/long-tail-keywords-seo), [internal links](https://searchengineland.com/internal-links-seo-best-practices-examples-tips-448047), [WP pages vs posts](https://searchengineland.com/wordpress-pages-vs-posts-385682)) · [Backlinko B2B](https://backlinko.com/hub/content/b2b)

Wideo: [Surfer Academy — SEO & AI SEO Course 2026](https://www.youtube.com/watch?v=7DRO4rEIHDk) · [Senuto/Vestigio (Szymon Parzych) — AI Overviews w praktyce, 2025-09-30](https://www.youtube.com/watch?v=MKwi0qmVSS8) · [Sam Dunning, Breaking B2B — money keyword matrix](https://www.youtube.com/watch?v=N1m9hlMsMKg) · [Maciej Cherubin — planowanie treści: struktura, widoki, encje](https://www.youtube.com/watch?v=yUp_Emk8KAo) · [Ahrefs (Samo Cerar) — kurs AEO](https://www.youtube.com/watch?v=uza9GX0E2mw) · [Nathan Gotch — Local SEO 2026](https://www.youtube.com/watch?v=bfBwk2KK9jc) · [Exposure Ninja — strategia SEO 2026, 2025-10-17](https://www.youtube.com/watch?v=vrGLaJOAKas) · [Semrush — 12 Brand Authority Signals](https://www.youtube.com/watch?v=VOb_QjlrgpE)
