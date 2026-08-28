# AI Overviews w praktyce – jak odzyskać ruch (webinar Senuto + Vestigio)

- Tytuł: AI overviews w praktyce – jak odzyskać ruch? | Webinar Senuto i Vestigio
- Kanał: Senuto – kompleksowe wsparcie działań SEO
- URL: https://www.youtube.com/watch?v=MKwi0qmVSS8
- Prelegent: Szymon Parzych (agencja Vestigio), prowadzi Mateusz Żegrocki (Senuto)
- Język transkryptu: pl (auto-napisy, sporo błędów w nazwach własnych; liczby weryfikuj u źródła)
- Data publikacji: 2025-09-30, ok. 2,5 h. Notatka: 2026-08-28

Oznaczenia: **[opinia]** = teza prelegenta oparta na jego audytach lub przeczuciu; **[dane]** = liczba z badania (Senuto, Semrush, Surfer, HigherVisibility); **[standard]** = powszechna praktyka SEO, nic nowego.

---

## 1. Skala zjawiska — po co w ogóle działać

**[dane]** Z audytów prelegenta (kilka domen, w tym 3 z listy top 500 najczęściej cytowanych przez AIO wg raportu Senuto):

- 20–30% fraz, na które domena jest w top 3 organicznie, wywołuje już blok AI Overviews. W top 10 i top 100 podobnie (ponad 20% i niecałe 20%). Raport Senuto podaje 24% niezależnie od pozycji.
- Spadek kliknięć organicznych: 12–18% dla większości badanych domen, jeden wydawca stracił 60%. Raport Senuto: -1% kliknięć rok do roku w skali rynku (czerwiec 2025 vs 2024), CTR -34%.
- Zdobycie statusu źródła w AIO pozwala odzyskać **20–40% utraconego ruchu** z fraz objętych blokiem. Prelegent obniżył tę estymację z 61% podawanych na poprzednim webinarze — po kolejnych audytach na większych serwisach.
- Potencjał jest niewykorzystany: badane domeny nie mają statusu źródła dla 90–96% fraz, dla których mają pozycję organiczną i występuje blok AIO. Czyli nawet najczęściej cytowane polskie domeny są cytowane dla 4–10% swoich fraz.
- W jednym bloku AIO jest zwykle 6–8 źródeł, średnio z 6 unikalnych domen. Jest gdzie się rozpychać.

**[opinia]** Realistyczny cel to wskaźnik cytowań 20–30% fraz i to już bardzo dobry wynik. 100% jest nieosiągalne, tak jak pierwsza pozycja nigdy nie dawała 100% kliknięć.

**Efekt „paszczy krokodyla" w GSC.** Kliknięcia spadają, wyświetlenia rosną. Wzrost wyświetleń to artefakt: GSC liczy każdy link do domeny na SERP osobno — wynik organiczny, link w tekście AIO i link w panelu bocznym to trzy wyświetlenia. **Nie interpretuj rosnących wyświetleń jako rosnącego popytu.**

## 2. Zanim zaczniesz gasić pożar — zmierz właściwą metrykę

**Rób:** zanim wdrożysz cokolwiek naprawczego, sprawdź wpływ AIO na konwersje, leady i sprzedaż, nie na kliknięcia z bloga.
**Dlaczego:** **[dane]** 80% zapytań wywołujących AIO to zapytania informacyjne, zwykle z góry lejka (badanie Semrush). Zapytania transakcyjne: niecałe 6%. **[opinia]** Prelegent zna domeny, w których mimo spadku ruchu blogowego sprzedaż wzrosła.

**Unikaj:** paniki i szybkich trików po zobaczeniu czerwieni w GSC/Analytics.

**Rób:** licz wartość treści informacyjnych także poza ruchem — autorytet tematyczny dla całej witryny, przekazywanie kontekstu i mocy linkami wewnętrznymi do stron transakcyjnych, materiał do recyklingu (social, newsletter, wideo, podcast), lead magnety.

**Test proponowany przez prelegenta [opinia]:** przygotuj 20–30 artykułów wspierających jeden istotny typ produktu/usługi, wstaw w nich panele i linki do stron transakcyjnych, obserwuj pozycje i ruch stron transakcyjnych. Wariant „odważny" (usunięcie istniejących artykułów i obserwacja) — odradzany bez konsultacji.

**Dla kogo sytuacja jest zła:** wydawcy i wszyscy, dla których walutą jest sam ruch informacyjny. Tu odzyskanie ruchu przez status źródła nie wystarczy (20–40% odzysku × 20–30% realnego wskaźnika cytowań). **Rekomendacja: dywersyfikacja kanałów i monetyzacji** — Google Discover i News (na razie nietknięte przez syntezę AI, potrafią dać kilkukrotnie większy ruch niż Search), YouTube, podcasty, social, newsletter, społeczność (Patreon, grupy FB, Discord), afiliacja zamiast samych reklam.

## 3. Czy czaty i wyszukiwarki AI zastąpią utracony ruch? Nie

**[dane]** HigherVisibility (sierpień 2025): ponad 60% użytkowników w USA deklaruje korzystanie z AI do szukania informacji (luty 2025: ok. 40%). Ale w tym samym badaniu udział Google trzyma się na poziomie ~95%, a narzędzia AI to 38% (czerwiec 2025). Ludzie używają wielu kanałów równolegle, więc **wzrost użycia AI nie oznacza równoważnego odpływu z Google**.

**[dane]** Tylko 6% odpowiedzi ChatGPT zawiera linki do źródeł zewnętrznych; średnia dla chatbotów to 14%. Czaty chcą zatrzymać użytkownika u siebie.

**[opinia] Wniosek:** LLM-y to na dziś kanał dodatkowy i addytywny, nie substytut. Nie planuj budżetu w oparciu o założenie, że optymalizacja pod LLM-y zwróci ruch utracony w AIO. Ale zacznij już teraz, bo w perspektywie kilku lat kanał będzie istotny.

**Podział systemów (przydatny przy diagnozie, dlaczego gdzieś nas nie ma):**
- Czaty (ChatGPT) — priorytet: szybkość. Domyślnie z wiedzy wewnętrznej, internet przeszukują niechętnie, źródła pokazują najgorzej.
- Wyszukiwarki AI (Perplexity) — priorytet: aktualność. Domyślnie przeszukują sieć i mocno eksponują źródła. **Najlepszy cel do walki o status źródła.**
- AI Overviews — hybryda: indeks Google + przeszukiwanie sieci.

## 4. Jak działa retrieval (mechanika, z której wynikają taktyki)

Analogia prelegenta: asystent naukowy z biblioteką.

1. Rozbicie zapytania użytkownika na serię **subpytań** badawczych.
2. Wstępna selekcja „książek" (stron, wideo, wątków na forach) — część odrzucana bez otwierania.
3. **Chunking** — wyciąganie z wybranych stron samowystarczalnych fragmentów (definicja, akapit, wiersz tabeli, punkt listy) na „fiszki".
4. Synteza odpowiedzi z fiszek. Część fiszek nie trafia do odpowiedzi.
5. Bibliografia = linki do źródeł.

Konsekwencje, o których warto pamiętać:
- **Proces nie jest deterministyczny.** To samo pytanie zadane kilka razy daje różne zestawy źródeł (powtarzalność źródeł na poziomie kilkudziesięciu procent). Nie ma gwarancji stałego statusu źródła. Nie raportuj klientowi „jesteśmy w AIO na frazę X" jako stanu trwałego.
- Systemy personalizują odpowiedzi po historii czatów/wyszukiwań/przeglądania.
- W jednym bloku może się pojawić kilka URL-i z tej samej domeny, jeśli masz spójny klaster treści.
- Cztery dźwignie: (a) reputacja autora i marki, (b) treść łatwa do zacytowania, (c) dostępność techniczna, (d) kompletne pokrycie tematu.

## 5. Fundament: klasyczne SEO wciąż decyduje

**[dane]** Raport Senuto: ponad 60% cytowań w AIO w Polsce pochodzi ze stron, które miały już pozycję w top 10 organicznie; średnia pozycja cytowanego źródła to 6–7. 21% źródeł pochodzi spoza top 100.

**[opinia] Rób:** gros budżetu SEO trzymaj na tradycyjnych fundamentach, bo działają na 100% fraz, a AIO dotyczy ~30%. Działania pod AIO traktuj jako rozszerzenie, nie zamiennik.
**Unikaj:** przenoszenia całego fokusu na „GEO/AEO" kosztem podstaw.

## 6. Marka i autorytet — dać się poznać systemom AI

**Rób: buduj obecność poza własną witryną.** AIO preferuje różnorodne źródła (średnio 6 unikalnych domen na blok), więc własną domeną nie zajmiesz całego bloku. Kanały wskazane przez prelegenta: Reddit i fora branżowe (UGC bywa cytowany, bo daje autentyczny, nielukrowany kontekst), Wikipedia i Wikidata, YouTube, social media, publikacje gościnne i sponsorowane, wypowiedzi eksperckie w mediach branżowych, dzielenie się własnymi badaniami/raportami z dziennikarzami.

**Taktyka nieoczywista [opinia]:** sprawdź, jakie domeny są dziś źródłami AIO dla twoich ważnych fraz (albo weź listę top 500 z raportu Senuto), wybierz te tematycznie powiązane i opublikuj tam dopieszczony artykuł pod twoją frazę. Zyskujesz podwójnie: link do siebie plus widoczność treści, którą sam napisałeś, na cudzej, chętnie cytowanej domenie. Wiele portali link buildingowych pozwala też **edytować istniejący, już cytowany artykuł** i dopisać wzmiankę.

**Rób: walcz o obecność w rankingach i porównaniach.** AI lubi te formaty, bo odpowiadają intencji „pomóż mi wybrać". Cel: maksymalna liczba wzmianek marki w cudzych zestawieniach + własne rankingi (bezpieczniej: rankingi własnych produktów/wariantów niż „najlepsze firmy w branży, numer 1 my").

**Rób: link nie jest warunkiem koniecznym.** Sama wzmianka marki w wielu miejscach zwiększa szansę, że model wskaże cię w zestawieniu czy porównaniu.

**Rób: wejdź do źródeł wiedzy, z których systemy czerpią.** Knowledge Graph Google (jest formularz zgłoszenia encji, jeśli cię tam nie ma), Profil Firmy w Google maksymalnie wypełniony, Wikipedia i Wikidata (ostrożnie i zgodnie z regulaminem tych serwisów).

**Rób: pilnuj spójności danych o marce** we wszystkich źródłach — ta sama nazwa, te same dane teleadresowe, te same opisy usług. To sygnał „to wszystko jesteśmy my".

**Rób: aktywnie zarządzaj narracją o marce.** AI potrafi wyciągnąć jeden pięcioletni negatywny komentarz z forum i podać go jako fakt. Nie skasujesz tego, ale możesz przeważyć skalę: zbieraj pozytywne opinie, wchodź do zestawień, zdobywaj recenzje na portalach, bądź obecny w mediach branżowych i na forach. Uważaj też na subtelny wariant: konkurent publikuje „obiektywne" porównanie, w którym twój produkt wypada blado — AI to zacytuje.

## 7. E-E-A-T: autorzy, dane własne, źródła

**Rób: dostarczaj wiedzę, której model nie ma.** Autorski raport, komentarz ekspercki własnego pracownika, recenzja poparta własnym testem, webinar, podcast.
**Unikaj:** setnego artykułu przepisującego top 10 Google. Nie ma powodu, żeby system wybrał akurat ciebie.

**Rób: podpisuj treści konkretną osobą** z imienia i nazwiska, w widocznym miejscu (przy H1 albo bezpośrednio pod artykułem), z krótkim biogramem **podkreślającym ekspertyzę**, linkującym do dedykowanej strony autora.
**Unikaj:** biogramów-prozy budujących sympatię („uwielbia kawę i górskie wędrówki"). Nie niosą żadnej informacji o kompetencji.
**Dobry biogram zawiera:** specjalizację, lata i rodzaj doświadczenia, publikacje zewnętrzne, z kim współpracował w danej tematyce, wystąpienia konferencyjne.
**Strona autora:** personalia w H1, zdjęcie, pełna biografia jak wyżej, listing linków do wszystkich tekstów autora. Ten listing ma też funkcję czysto techniczną — patrz sekcja 11.

**Rób: cytuj wiarygodne źródła i podawaj konkretne liczby.** „Wzrost o 34% w porównaniu z II kw. 2025 wg raportu X" zamiast „znacząco wzrosło".

**Rób: zbieraj i pokazuj opinie użytkowników na własnej witrynie** (z moderacją, najlepiej weryfikacją po zakupie/usłudze). To dodatkowy, inny w tonie content na tej samej stronie.

**[opinia, słabe dane] Zaangażowanie użytkowników.** W audycie topowego polskiego e-commerce technologicznego prelegent zobaczył korelację: im więcej głosów miał artykuł, tym wyższy wskaźnik cytowań w AIO; podobnie dla średniej oceny — z wyjątkiem ocen 5.0, gdzie wskaźnik spadał, bo serwis dorzucał nowym artykułom po dwie–trzy piątki na start. Sam prelegent zaznacza, że to może być korelacja, nie przyczynowość. **Nie nabijaj ocen sztucznie.**

## 8. Aktualność treści i content pruning

**[dane z audytów prelegenta, wykres]** Wskaźnik cytowań w funkcji wieku treści ma cztery fazy:
1. Treści bardzo świeże — cytowane najrzadziej (systemy jeszcze do nich nie dotarły, brak zbudowanego zaufania).
2. Faza wzrostu — wskaźnik szybko rośnie.
3. Złoty środek — autorytet zbudowany, treść wciąż aktualna, najwyższe wskaźniki.
4. Faza śmierci — treść przestaje być satysfakcjonującym źródłem. W najlepszym audycie po ~5 latach, w innych domenach już po 2 latach.

**Rób:** nie pompuj całego budżetu contentowego w nowe teksty. Odświeżaj najważniejsze artykuły tak często, jak ich temat tego wymaga. Zmierz sam, jak szybko dezaktualizują się treści w twojej branży.
**Unikaj: samej zmiany daty publikacji.** Prelegent to testował — nie działa. Aktualizacja musi wnosić wartość: nowe dane, cytaty ekspertów, świeże przykłady.

**Rób: content pruning.** Usuwaj artykuły niezwiązane tematycznie, bez ruchu i bez potencjału. Powody: (a) lepszy średni obraz jakości witryny, (b) crawlery mają skończony budżet — chudszy, skondensowany blog to większa szansa, że roboty w ogóle dotrą do wartościowych treści.
**Wyjątek:** jeśli treść konwertuje, zostaw ją, nawet gdy nie pasuje tematycznie. Kolejność narzędzi: usunięcie z 410, przekierowanie 301 tam, gdzie jest sensowny cel, noindex jako jedna z opcji.

## 9. Struktura i formatowanie — jak zostać zacytowanym

Zasada: projektuj treść jako zestaw klocków, które łatwo znaleźć, zrozumieć i wyciąć.

**Nagłówki** [standard, ale ze wzmocnieniem]: H1 jeden na stronę, dalej logiczna hierarchia H2/H3. Każdy nagłówek opisowy, **najlepiej w formie pytania**, z frazą kluczową. Pod każdym nagłówkiem **bezpośrednia, zwięzła odpowiedź w pierwszym, najdalej drugim zdaniu**; rozwinięcie, dane, cytaty i przykłady dopiero potem.

**Wstęp pod H1** — według prelegenta najbardziej niedoceniana rzecz. Zamiast literackiego wprowadzenia i budowania „empatycznej więzi" daj skondensowaną pigułkę: czego czytelnik dowie się z tekstu. Istnieją przesłanki, że roboty na podstawie pierwszych akapitów decydują, czy w ogóle warto poświęcić zasoby na resztę strony.

**Odwrócona piramida:** najpierw treść odpowiadająca na główną intencję, potem podrozdziały pod intencje uzupełniające, w kolejności malejącej istotności. Nie zakopuj kluczowej informacji na dole.

**Jedna intencja na artykuł.** Cel to intencja użytkownika, nie fraza. Nie upychaj całej wiedzy o temacie w jednym tekście.

**Formaty chętnie cytowane [dane z audytów]:** listy, rankingi, porównania, instrukcje krok po kroku, tabele, FAQ. **[dane]** Badanie Surfera: 78% odpowiedzi AIO zawiera listę (uporządkowaną, nieuporządkowaną albo obie).
**Rób:** najpierw ustal intencję, potem dobierz format z tej puli. Nie pisz „bezkształtnego artykułu".

**Tabele — według prelegenta złoto w kontekście AIO.** Warunek: **każdy wiersz musi być samodzielnym faktem**, zrozumiałym w oderwaniu od reszty tabeli, bo model często wyciąga jeden wiersz, nie całą tabelę.

**FAQ** — idealna struktura pytanie–odpowiedź, plus miejsce na subpytania, które nie zmieściły się w głównych nagłówkach.

**Samowystarczalne akapity i zdania.** Jeden akapit = jedna wyodrębniona idea, gęsta informacyjnie: jasne, autorytatywne stwierdzenie poparte danymi lub cytatem. Unikaj zaimków i odniesień w rodzaju „to", „tamto", „ten pierwszy" — używaj konkretnych nazw i encji, żeby zdanie działało wyrwane z kontekstu.
**Unikaj:** anegdot i opisów sytuacyjnych typowych dla artykułów SEO.

**Spis treści** w artykule, klikalny.

**Lista najważniejszych wniosków** (TL;DR) na początku albo na końcu — na tyle dobra, żeby ktoś, kto przeczyta tylko ją, wiedział, o co chodzi.

**Semantyczny HTML:** `<table>`, `<ul>`/`<ol>`, `<blockquote>`, `<strong>`, `<figure>` + `<figcaption>`. **[opinia]** Może pomóc, na pewno nie zaszkodzi.

**Multimodalność:** w AIO pojawiają się już obrazy, zaczynają pojawiać się wideo. Zamiast wydawać drugie tyle na kolejny artykuł, wzmocnij istniejący infografiką lub wideo — ten sam materiał obsłuży YouTube i newsletter. Wymogi: opisowy `alt` zawierający frazę powiązaną z grafiką, `figure`/`figcaption`, transkrypcja do wideo.

## 10. Język i styl

- **Konkrety i dane zamiast ogólników.** „Dużo", „często", „znacząco" nie nadają się do cytowania.
- **Ton autorytatywny.** Zdania oznajmujące, bez asekuracji („wydaje się", „chyba", „w pewnym sensie").
- **Parafrazuj i używaj synonimów.** Model operuje znaczeniami, nie frazami. Ujmij ten sam problem kilkoma konstrukcjami, których używają użytkownicy — szersza sieć na warianty zapytań.
- **Nasycaj tekst encjami** (marki, osoby, wydarzenia, ustawy, normy, nagrody — wszystko, co mogłoby mieć hasło w encyklopedii). Encje istnieją w Knowledge Graph i pomagają osadzić tekst w kontekście.
- **Trójki semantyczne:** podmiot–orzeczenie–dopełnienie. Prosta struktura odpowiadająca formatowi grafów wiedzy; ułatwia modelowi odczytanie relacji między encjami.
- **Wyróżniki oferty pisz mierzalnie.** Konkretna, weryfikowalna liczba jest cytowana chętniej niż deklaracja jakości.
- **Terminologia fachowa: używaj, ale wyjaśniaj** krótko przy pierwszym wystąpieniu. Podwójna korzyść: sygnał ekspertyzy plus dodatkowe encje.
- **Unikaj:** nadmuchiwania tekstu do limitu znaków narzucanego przez narzędzia contentowe i komplikowania zdań dla objętości.

Kryterium końcowe [opinia]: dobra treść to taka, po której użytkownik myśli „już wiem wszystko, czego chciałem". Nie triki pod algorytm — to nie kolejna aktualizacja algorytmu, tylko zmiana rzeczywistości.

## 11. Fundamenty techniczne

- **Wszystkie istotne treści w HTML.** Roboty AI **nie wykonują JavaScriptu**. Treść doładowywana po stronie klienta dla nich nie istnieje — widzą pustą stronę. SSR albo statyczne serwowanie dla: treści artykułu, opisów produktów, dokumentacji, `title`, `meta description`, nawigacji. Po stronie klienta zostaw tylko rzeczy nieistotne dla cytowania (widżet czatu, liczniki).
- **robots.txt:** nie blokuj crawlerów AI dostępu do sekcji z ważną treścią.
- **Uważaj na blokady poza robots.txt:** kod odpowiedzi musi być 200, brak agresywnych blokad po stronie serwera/hostingu/WAF. Prewencyjnie można whitelistować oficjalne zakresy IP zaufanych botów. Brak `noindex`.
- **Dyrektywy `nosnippet`, `data-nosnippet`, `max-snippet`** — blokują generowanie fragmentów, a przez to obecność w AI Overviews. Zweryfikuj, czy nie zostały gdzieś wdrożone „na wszelki wypadek".
- **Mapy witryny** XML i HTML, podzielone według typu stron, 500–1000 URL-i na plik, spięte indeksem sitemap, zgłoszone.
- **Architektura i linkowanie wewnętrzne:** każda strona osiągalna w maksymalnie 3 kliknięciach, klastrowanie tematyczne, opisowe anchory, kontekst wokół linka też powiązany tematycznie ze stroną docelową. Linkuj **ze starych wpisów do nowych** — boty i tak odwiedzają stare, więc tam znajdą nowy URL i dodadzą go do kolejki.
- **Page speed** — crawlery mają skończony budżet czasu; wolny serwer to ryzyko pobrania fragmentu treści albo rezygnacji z wizyty.
- **Kod 304** dla treści niezmienionych — oszczędza budżet crawlowania na dużym blogu.
- **Dane strukturalne** [wg Google] nie wpływają bezpośrednio na AIO, wpływają pośrednio przez tradycyjny search, a Google nie wyklucza wzrostu ich roli. Najważniejsze: `Organization` i `Person`/`Author`. Jeśli wdrażasz, wypełniaj maksymalnie wszystkie sensowne właściwości i zagnieżdżaj, nie tylko wymagane minimum.
- **Przekierowania:** usuwaj zbędne łańcuchy, dla trwale usuniętych stron bez wartości daj 410.
- Strony autorów i sekcje „powiązane wpisy" pełnią też rolę wewnętrznych hubów linkowych ułatwiających indeksację — przy 3 kategoriach i 20 autorach strony autorów mogą być krótszą drogą do URL-i niż paginacja kategorii.

---

## 12. Co z tego jest szczególnie istotne dla B2B prawo / podatki / księgowość w Polsce

Wnioski moje, na bazie materiału; prelegent nie omawiał tej branży wprost (odpowiadał na pytania z edukacji, franczyzy i przemysłu).

**Straty będą duże, ale bolą mniej, niż pokazuje GSC.** Blog podatkowy to niemal w całości treść informacyjna z góry lejka — czyli dokładnie ten typ zapytań, w który uderza AIO (80% wywołań). Metryką sukcesu musi być liczba zapytań ofertowych i podpisanych umów, nie sesje na artykule o KSeF. **Zanim ktokolwiek zaproponuje przebudowę bloga, pokaż zestawienie: ruch vs formularze kontaktowe w tym samym okresie.**

**YMYL działa na waszą korzyść.** Systemy AI minimalizują ryzyko podania szkodliwej informacji, a prawo i podatki to najostrzejszy przypadek. Sygnały autorstwa liczą się tu bardziej niż w e-commerce:
- Każdy artykuł podpisany doradcą podatkowym / radcą prawnym z imienia i nazwiska przy H1.
- Biogram z numerem wpisu na listę (KIDP, OIRP, NRA), specjalizacją, publikacjami i wystąpieniami — nie „pasjonat prawa gospodarczego".
- Dedykowane strony autorów z pełnym listingiem tekstów.
- Schema `Person` powiązana z `Organization`.

**Aktualność jest tu twardym wymogiem, nie optymalizacją.** Faza śmierci treści w prawie i podatkach przychodzi z każdą nowelizacją, nie po 2–5 latach. Priorytet budżetu: cykl aktualizacji artykułów o stawkach, limitach, terminach i obowiązkach (JPK, KSeF, składka zdrowotna, estoński CIT) wyprzedzający wejście przepisów w życie. **Nie zmieniaj samej daty — dopisz stan prawny i nową liczbę.** Warto dodawać widoczne „stan prawny na DD.MM.RRRR", bo to konkret, który model chętnie cytuje.

**Unikalne dane, których nikt inny nie ma.** To najsilniejsza dźwignia z całego webinaru i akurat kancelaria ma czym strzelać: własne statystyki z postępowań, ankiety wśród klientów-przedsiębiorców, komentarze do projektów ustaw w dniu publikacji, koszty i czas wdrożeń u klientów. Uwaga na dane klientów — agreguj i anonimizuj, nigdy nie publikuj przypadków identyfikowalnych.

**Formaty pod cytowanie, które pasują do tej branży:**
- Tabele porównawcze form opodatkowania, progów, limitów, stawek — z wierszami samowystarczalnymi („Ryczałt 8,5%: usługi X, limit przychodu 2 mln EUR, brak odliczenia kosztów").
- Instrukcje krok po kroku (zgłoszenie do KSeF, zmiana formy opodatkowania, rejestracja VAT-UE).
- Rozbudowane FAQ pod subpytania („czy X można zaliczyć do kosztów", „do kiedy złożyć Y", „co grozi za Z").
- Nagłówki w formie pytań klientów, z odpowiedzią w pierwszym zdaniu pod nagłówkiem — potem dopiero podstawa prawna i zastrzeżenia.
- Cytowanie źródeł pierwotnych (Dz.U., interpretacje KIS, wyroki NSA) z sygnaturami. To encje i sygnał wiarygodności naraz.

**Klastry tematyczne zamiast luźnych wpisów.** Jedna intencja na artykuł, klaster wokół każdego obszaru praktyki, linkowanie kontekstowe z treści informacyjnych do stron usługowych. To ta sama mechanika, która daje kilka URL-i z jednej domeny w jednym bloku AIO.

**Obecność poza własną domeną.** AIO chce 6 różnych domen. Dla kancelarii realne: komentarze eksperckie w mediach gospodarczych i prawniczych, publikacje w portalach branżowych chętnie cytowanych przez AIO (sprawdź listę top 500 z raportu Senuto pod kątem serwisów prawno-podatkowych), obecność w zestawieniach i rankingach kancelarii, Wikidata dla kancelarii i wspólników, aktywność na LinkedIn i YouTube.

**Ryzyko reputacyjne warto monitorować.** Jeden negatywny wątek na forum przedsiębiorców może trafić do odpowiedzi AI o kancelarii jako fakt. Regularnie pytaj czaty i AIO o nazwę kancelarii i sprawdzaj, z czego zbudowały odpowiedź.

**Techniczne minimum do sprawdzenia od razu:** czy treść artykułów jest w HTML (nie doładowywana JS-em), czy gdzieś nie siedzi `nosnippet`, czy WAF nie blokuje botów AI, czy strony autorów istnieją i linkują do tekstów.

---

## 13. Odpowiedzi na pytania branżowe z końcówki webinaru (skrót)

- **Szkoła językowa — pisać tylko o nauce języka czy też dla rodziców?** Rdzeń biznesu w pierwszej kolejności. Tematy poboczne tak, jeśli starczy zasobów na kompletne klastry i jeśli masz w nich realną ekspertyzę. Spinaj je linkami kontekstowymi z rdzeniem i grupuj w osobne kategorie. Im dalej od rdzenia, tym ważniejszy E-E-A-T.
- **Franczyza ogólnopolska, podstrony filii?** Priorytet: silna marka i siła domeny głównej (przenosi się na podstrony lokalne). Do tego spójność danych (te same adresy, te same nazwy oddziałów wszędzie), maksymalnie wypełnione wizytówki, rozbudowane podstrony lokalizacji, pozytywne opinie o oddziałach. Blog drugorzędny, sensowny gdy wspiera cele lokalne lub rekrutację franczyzobiorców.
- **Przemysł/nowe technologie?** Regularne odświeżanie kluczowych artykułów o nowe normy, parametry, wyniki badań, case studies. Rankingi zawsze z najnowszymi produktami. Nowe treści budowane na unikalnych danych i wypowiedziach ekspertów. Klastry na każdą gałąź przemysłu, tagi jako encje technologiczne, tabele do porównań technicznych.
- **Treści edukacyjne, które się nie starzeją?** Recykling do innych formatów, dokładanie nowych formatów do istniejących tekstów, linkowanie wewnętrzne, UGC, nowe przykłady, świeże trendy, nowe cytaty i wyniki badań.
- **Rozrastający się blog i problemy z indeksacją?** 304 dla niezmienionych, klastrowanie tematyczne, sekcje „powiązane wpisy", strony autorów jako huby linkowe, linkowanie ze starych wpisów do nowych, regularny content pruning, page speed, sitemapy, czyszczenie przekierowań (410 dla trwale usuniętych).
- **Czy cytowanie źródeł nie wzmacnia konkurencji?** Bezpośredniego konkurenta nie linkuj. Źródło celujące w inną intencję (raport, badanie) linkuj bez obaw.

## 14. Czego w materiale brakuje / na co uważać

- Prezentacja opiera się w dużej mierze na **reverse engineeringu** i kilku audytach jednego eksperta — sam to zaznacza. Liczby z jego audytów (fazy życia treści, korelacja z ocenami) traktuj jako hipotezy do zweryfikowania na własnych danych, nie jako stałe rynkowe.
- Auto-transkrypcja przekręca liczby i nazwy (np. „-1% kliknięć" w kontekście, w którym wynik raportu Senuto był prawdopodobnie dwucyfrowy). **Zanim zacytujesz którąkolwiek liczbę na zewnątrz, sprawdź ją w raporcie Senuto o AI Overviews i w badaniach Semrush/Surfer/HigherVisibility.**
- Brak konkretów o AI Mode Google, o `llms.txt` i o umowach licencyjnych z dostawcami LLM — webinar z września 2025 tego nie obejmuje.
