# AEO / AI SEO — kurs Ahrefs (Samo Cerar)

- Tytuł: AI SEO Course for Beginners: Complete AEO Tutorial
- Kanał: Ahrefs
- URL: https://www.youtube.com/watch?v=uza9GX0E2mw
- Język transkryptu: en (napisy oryginalne)
- Długość: ok. 87 min, 4 moduły + wstęp. Notatka: 2026-08-28.

Uwaga metodologiczna do całości: prawie wszystkie liczby w kursie pochodzą z własnych badań Ahrefs i nie są recenzowane zewnętrznie, a co drugi krok wykonawczy jest pokazany na ich płatnym narzędziu (Brand Radar, Site Explorer, Content Explorer, Web Analytics). Merytoryka pod spodem jest sensowna i w większości da się odtworzyć bez Ahrefs (robots.txt, logi serwera, GA4, ręczne odpytywanie modeli), ale trzeba oddzielać obserwację od sprzedaży. Poniżej oznaczam: **[dane Ahrefs]**, **[opinia autora]**, **[praktyka powszechna]**.

---

## 1. Mechanika: skąd AI bierze treść

**Dwa źródła, dwie różne gry.** Trenowanie (statyczny snapshot internetu, aktualizowany co kilka miesięcy) i retrieval w czasie rzeczywistym (RAG — model wychodzi do sieci, pobiera kilkanaście stron, czyta i syntetyzuje). Wpływasz na jedno i drugie inaczej: na trening przez masową, spójną obecność nazwy w sieci, na retrieval przez klasyczne SEO. **[praktyka powszechna]**

**Rób: optymalizuj pod cały temat, nie pod pojedynczą frazę.** Powód to query fan-out: jeden prompt użytkownika model rozbija na kilkanaście podzapytań i odpytuje je równolegle. Badanie Seer Interactive/Nectiv cytowane w kursie: średnio 9–11 podzapytań na prompt, skrajnie do 28; tryb deep research ChatGPT potrafi wykonać 420 wyszukań na jedno pytanie. **[dane zewnętrzne cytowane przez Ahrefs]** Jeśli strona o temacie X pokrywa tylko podstawy, model dobierze cudzą stronę do podzapytań, których nie pokryłeś.

**Unikaj: traktowania zapytań fan-out jak nowej listy słów kluczowych.** Są syntetyczne, generowane w locie, niepowtarzalne między uruchomieniami i ponad 95% z nich ma zerowy wolumen wyszukiwań, bo żaden człowiek tak nie pisze. Traktuj je jako podgląd tego, jakie wątki model uważa za istotne przy danym pytaniu. **[opinia Ahrefs, mocno uzasadniona]**

**Cytowania są probabilistyczne, nie pozycyjne.** To samo pytanie zadane pięć razy da różne zestawy marek (temperatura + różne wyniki retrievalu). Nie istnieje „pozycja", istnieje rozkład prawdopodobieństwa. Konsekwencja praktyczna: raportuj widoczność jako udział w odpowiedziach z wielu powtórzeń, nigdy jako pojedynczy zrzut ekranu. **[praktyka powszechna]**

**Co podnosi prawdopodobieństwo cytowania** **[dane Ahrefs]**:
- konsensus — im więcej niezależnych źródeł mówi o marce to samo, tym chętniej model to powtarza,
- świeżość — treść cytowana przez AI jest średnio o 25,7% świeższa niż to, co rankuje w klasycznych wynikach,
- autorytet — nadal działa, ale słabiej niż się mówi (patrz sprzeczność niżej).

**Sprzeczność do odnotowania:** w module 1 pada „76% cytowań w AI Overviews pochodzi ze stron z top 10 Google", a w module 2 autor sam to koryguje: „nowsze badanie pokazuje, że to już bliżej 38%". Traktuj 76% jako liczbę nieaktualną. Wniosek trwały jest taki, że związek między rankingiem w Google a cytowaniem w AI istnieje, ale słabnie, a AI Overviews coraz częściej sięgają poza top 10 (YouTube, Reddit). 14% stron cytowanych w AI Overviews nie rankuje w top 100 Google w ogóle.

---

## 2. Platformy różnią się bardziej, niż się zakłada

**[dane Ahrefs]** Z 50 najczęściej cytowanych domen tylko 7 pojawia się na wszystkich trzech platformach (AI Overviews, ChatGPT, Perplexity) — 14% pokrycia.

- **Google AI Overviews**: preferują serwisy uznane i encyklopedyczne, zdrowie, finanse, plus własne usługi Google. Sam YouTube to ok. 5,6% wszystkich cytowań. Reddit mocno obecny.
- **ChatGPT**: ciąży ku wydawcom i mediom (Reddit, Wikipedia, Amazon, Forbes, Business Insider, Wired). Mediana DR najczęściej cytowanych stron to 90, częściowo przez umowy licencyjne OpenAI. Pokrycie z top 10 Google tylko 8–10%.
- **Perplexity**: najbliżej klasycznego Google, 28,6% cytowań ze stron z top 10. Jeśli już dobrze rankujesz, tu zobaczysz efekt najszybciej.
- **Google AI Mode**: mimo że to ten sam Google, pokrycie cytowań z AI Overviews wynosi 13,7%, przy 86% podobieństwa semantycznego odpowiedzi. Najczęściej cytowana domena to YouTube, z dużą przewagą; Quorę cytuje 3,5× częściej niż AI Overviews, mocno sięga po Facebooka i Instagram.

**Rób: wybierz platformę świadomie, po dwóch kryteriach** — udział w rynku (Google + ChatGPT to zdecydowana większość ruchu) oraz pokrycie z tym, co już robisz w SEO. **[opinia autora]** Nie potrzebujesz osobnych strategii dla każdej platformy, ale potrzebujesz świadomej kolejności: pod ChatGPT gra się wzmiankami w mediach o wysokim DR, pod AI Overviews — YouTube i Reddit.

---

## 3. Trzy typy widoczności

1. **Cytowanie z linkiem** — najłatwiejsze do zmierzenia, daje ruch.
2. **Wzmianka bez linku** — użytkownik poznaje nazwę i szuka jej później sam. Rekomendacja ustna w skali.
3. **Brak obecności.**

**[dane Ahrefs]** Tylko ok. 28% wzmianek marki zawiera link. Rozbicie: Perplexity 51,6%, AI Mode 36,8%, ChatGPT 26,9%, AI Overviews 10,7%. Ale po zważeniu wolumenem: linki pojawiają się przy zapytaniach o największym zasięgu (Perplexity — 78% wyświetleń mimo 51% wzmianek; Gemini — 71% wyświetleń przy 16,8% wzmianek).

**Kluczowa liczba całego kursu:** w badaniu 75 000 marek to wzmianki marki w sieci (branded web mentions) miały najsilniejszą korelację z widocznością w AI Overviews — 0,664, wyżej niż backlinki, DR i liczba domen odsyłających. Wzmianka na stronie z dużą liczbą linków przychodzących: 0,7. **[dane Ahrefs]** To ta sama seria badań, którą mam już odnotowaną w `web-link-building.md`.

**Konsekwencja: wzmianka bez linku nie jest odpadem.** Do klasycznego rankingu nie przekazuje sygnału (stanowisko Google), do widoczności w AI ma wartość samodzielną. Prowadź te dwa cele osobno.

---

## 4. Brand gap analysis (moduł 2, krok pierwszy)

**Rób to przed jakąkolwiek optymalizacją**, żeby mieć punkt odniesienia.

**Krok 1: zmapuj encje marki.** Nazwa główna, submarki, nazwy produktów, autorskie funkcje, autorskie metryki, marki osobiste ludzi z firmy. Każda ma własny profil widoczności i każdą analizujesz osobno. Następnie połącz każdą encję z tematami i atrybutami, z którymi ma być kojarzona — modele nie rozumieją nazw same z siebie, wnioskują znaczenie z tego, jak marka jest opisywana. Szybka metoda: wyszukaj powtarzające się przymiotniki i określenia używane obok nazwy i kategorii.

**Krok 2: zbierz metryki bazowe** — wzmianki, cytowania, wyświetlenia, AI share of voice, w rozbiciu na platformy. Filtry, które faktycznie coś dają: odpowiedzi, które wymieniają markę, ale nie linkują do strony (to gotowa lista celów), zapytania, gdzie konkurent jest wymieniony, a ty nie, oraz konkretne tematy przypisywane komuś innemu.

**Krok 3: sześć rodzajów luk** (framework Despiny Gavoyannis z Ahrefs) **[opinia/framework autorski]**:
- **luka widoczności** — pojawiasz się rzadziej niż konkurenci,
- **luka narracyjna** — AI opisuje markę inaczej, niż chcesz być pozycjonowany (np. „tania alternatywa" zamiast „premium"),
- **luka tematyczna** — tematy, z którymi powinieneś być kojarzony, a nie jesteś,
- **luka formatu** — AI cytuje określone formaty (poradniki, wideo, recenzje, porównania), a ty ich nie produkujesz,
- **luka wzmianek zewnętrznych** — zestawienia, serwisy recenzenckie, fora i publikacje wymieniające konkurencję bez ciebie,
- **luka popytu** — zapytania w twojej przestrzeni, przy których nazwa marki nigdy nie pada.

**Krok 4: priorytetyzacja.** Każda luka domyka się jednym z trzech działań: **fix** (poprawa istniejącej strony), **build** (nowa treść), **influence** (działanie off-site, outreach). Wagi: ile popytu to otwiera, czy wzmacnia wiarygodność marki, czy zwiększa szansę na cytowanie. Zaczynaj od strony, która już rankuje i wymaga tylko aktualizacji — niski nakład, wysoki potencjał.

**Rób to samo dla konkurentów.** Ich zapytania brandowe pokazują tematy, które użytkownicy i modele wiążą z nimi mocniej niż z tobą. Częsty przypadek: masz funkcję, ale nie masz treści, która ją z marką łączy.

---

## 5. Research słów kluczowych i promptów

**Formuła BID przy każdej frazie** **[praktyka powszechna, nazwa Ahrefs]**:
- **B — business potential**: czy pozycja 1 realnie pomoże firmie. „Co to jest espresso" ma wolumen, ale zerową intencję zakupową.
- **I — intent**: wygoogluj frazę i zobacz, co rankuje. Jeśli w top same strony e-commerce, blog tam nie wejdzie.
- **D — difficulty**: sprawdź DR i liczbę domen odsyłających stron z top 10. Kilka niskich DR w top 10 to dobry znak.

**Krok dodatkowy: filtr AI.** Zanim zatwierdzisz frazę, zapytaj, czy AI potrafi w pełni zaspokoić użytkownika. Jeśli AI Overview daje kompletną odpowiedź, kliknięcia nie będzie i frazę należy targetować inaczej (przez wzmiankę w odpowiedzi, nie przez ranking). **[dane Ahrefs]** AI Overviews pojawiają się przy ok. 21% wszystkich fraz, ale przy 58% zapytań pytających, 46% zapytań 7+ słów, a 99,9% fraz wywołujących AI Overview ma intencję informacyjną.

**Gdzie kliknięcie wciąż jest do wzięcia:** zapytania wymagające zrobienia czegoś. Modyfikatory: kalkulator, checker, generator, narzędzie, wzór/szablon, wyszukiwarka, planer. Plus filtr intencji transakcyjnej. **[praktyka powszechna]** AI nie zastąpi narzędzia, którego trzeba użyć.

**Frazy „przegrane" przez AI targetuj inaczej — pod wzmiankę.** **[dane Ahrefs]** 43,8% wszystkich cytowanych stron to zestawienia (listicle). Autor tłumaczy to tak: listy pomagają modelowi zbudować konsensus, bo obecność marki na wielu listach to wiele niezależnych rekomendacji **[opinia autora]**. Praktyczny filtr zapytań do polowania: `best`, `top`, `versus`, `review`, `alternative` (po polsku: „najlepszy", „ranking", „porównanie", „opinie", „alternatywa dla").

**Ponad 45% cytowań zmienia się przy odświeżeniu AI Overviews, a to dzieje się średnio co 2 dni.** To nie jest ćwiczenie jednorazowe. **[dane Ahrefs]**

**Prompt research — czym się różni.** Ludzie nie wpisują fraz, tylko prowadzą rozmowę z pełnym kontekstem („prowadzę małą agencję, jakiej platformy użyć"). To samo pytanie zadane na 10 sposobów da 10 różnych zestawów marek. Nie da się optymalizować pod pojedynczy prompt — buduje się widoczność w całym temacie.

---

## 6. Treść, którą AI cytuje

**Długość nie ma znaczenia.** **[dane Ahrefs]** Analiza 174 000 stron cytowanych w AI Overviews: korelacja liczby słów z cytowaniem 0,04, czyli zero. 53,4% cytowanych stron ma poniżej 1000 słów. Przestań pisać 3000 słów dla samej długości.

**Świeżość ma znaczenie duże.** **[dane Ahrefs]** 89,7% najczęściej cytowanych przez ChatGPT stron zaktualizowano w 2025, a 76% w ciągu ostatnich 30 dni. Treść nietknięta od pół roku jest na starcie w gorszej pozycji. **Unikaj podmiany samej daty publikacji** — jest wykrywalna; aktualizacja musi dotyczyć treści.

**Formaty, które działają**: zestawienia, treść oparta na własnych danych i liczbach (modele chętnie cytują konkretne liczby), porównania X vs Y (odwzorowują sposób, w jaki ludzie pytają AI).

**Cztery zasady pisania** — autor podkreśla, że nie istnieje osobny „format pod AI"; piszesz dla ludzi, a modele uczyły się tego, co ludzie uznają za wartościowe. **[opinia autora, ale zbieżna z praktyką]**

1. **BLUF (bottom line up front).** Każda sekcja zaczyna się od odpowiedzi, nie od tła. Uzasadnienie: ludzie czytają wzorem F (początek dokładnie, środek pobieżnie), a modele też ważą początek i koniec fragmentu wyżej niż środek. Wniosek zakopany w trzecim akapicie przepada dla obu.
2. **Treść atomowa.** Każda sekcja musi bronić się samodzielnie, bo systemy AI tną tekst na fragmenty i nie kontrolujesz, gdzie przetną. Test: weź dowolną sekcję H2, przeczytaj ją w oderwaniu od reszty; jeśli bez kontekstu nie ma sensu, przepisz.
3. **Pisanie nasycone encjami.** Zamiast „to narzędzie pomaga w SEO" pisz „Keywords Explorer od Ahrefs pokazuje frazy o niskiej trudności i wysokim potencjale ruchu". Modele rozumieją tekst przez encje i relacje między nimi; im konkretniej, tym więcej materiału do wykorzystania.
4. **Prosto i oznajmująco.** Krótkie zdania, układ podmiot-orzeczenie-dopełnienie, jedna myśl na zdanie. Jeśli zdanie wymaga dwóch przeczytań, jest za złożone.

**Etykietuj własne koncepcje nazwą marki.** **[opinia autora, wartościowa]** Modele spłaszczają oryginalność: autorski framework, o którym mówisz tylko ty, zostanie wchłonięty jako wiedza ogólna bez przypisania. Obrona: nazwij go nazwą firmy („macierz oceny treści Ahrefs"), zdefiniuj wprost i rozprowadź szeroko — blog, social media, podcasty, fora. Im więcej miejsc z nazwą przy koncepcji, tym trudniej ją zanonimizować.

**Odświeżaj „sleeper pages".** Strony, które kiedyś dobrze rankowały i powoli spadły. Mają już linki i autorytet, potrzebują tylko aktualizacji, a świeżość jest silnym sygnałem — to najszybsza droga do widoczności w AI. Jak znaleźć: raport najlepszych stron, sortowanie po zmianie ruchu, szukasz stron ze spadkiem ruchu **i** przyzwoitą liczbą linków. **Weryfikuj przyczynę:** jeśli strona nigdy nie miała domen odsyłających, to problem z linkami, nie z treścią, i aktualizacja nic nie da.

---

## 7. Zdobywanie wzmianek — trzy poziomy

**Tier 1: redakcyjna treść osób trzecich.** Publikacje branżowe, serwisy recenzenckie, zestawienia i porównania na uznanych blogach, recenzje na YouTube. Najtrudniejsze i najcenniejsze, bo to dokładnie ten typ stron, z których modele czerpią (43,8% cytowań ChatGPT to listy i porównania).

**Nie czekaj, aż strona zacznie być cytowana.** Celuj w strony, które już mają dużo linków i pokrywają twój temat — jest duża szansa, że zostaną cytowane później. Metoda wyszukiwania: `title:najlepsze` / `title:ranking` / `title:porównanie` w twojej niszy, minus nazwa własnej marki, filtr na liczbę domen odsyłających. To daje listę zestawień, na których cię brakuje.

**Tier 2: treść użytkowników i społeczności.** Reddit, Quora, fora niszowe. Reddit jest jednym z najczęściej cytowanych źródeł ChatGPT i jednym z fundamentalnych źródeł treningowych. **Unikaj spamowania nazwą — to działa przeciw tobie.** Rób: znajdź wątki, w których twoja wiedza realnie odpowiada na pytanie, i odpowiedz merytorycznie. Sposób na znalezienie wartościowych wątków bez płatnego narzędzia: wpisz reddit.com (lub polskie forum) do analizy fraz organicznych, filtr na pozycje w top 5, filtr `include` z terminami twojej niszy.

**Tier 3: własne właściwości.** Dodatkowe domeny, kanał YouTube, podcast, LinkedIn. Wszystko to jest indeksowane i może być źródłem dla AI. Zasada ogólna: im więcej miejsc, gdzie marka pojawia się pozytywnie i tematycznie trafnie, tym więcej przykładów uczących.

**Regularny audyt wzmianek.** Wzmianki znikają — strony są aktualizowane, listy odświeżane, marka wypada bez powiadomienia. Śledź trend liczby wzmianek i badaj spadki. Śledź też sentyment i poprawność. **Kolejność naprawy błędnej informacji: najpierw opublikuj u siebie treść, która wprost jej zaprzecza, potem wystąp do wydawcy o korektę.** Im szybciej, tym mniej czasu model ma na nauczenie się błędu.

---

## 8. YouTube

**[dane Ahrefs]** YouTube to najczęściej cytowana domena w AI Overviews, a wzmianki na YouTube mają korelację 0,737 z widocznością w ChatGPT — najsilniejszą ze wszystkich badanych czynników. Powód strukturalny: GPT-4 był trenowany na ponad milionie godzin transkrypcji z YouTube. To platforma, z której AI nie tylko cytuje, ale się uczy.

**Rób „search hits", nie „viral hits".** **[opinia autora]** Wirale dają skok wyświetleń i umierają, gdy algorytm wyczerpie zainteresowanych. Film wyszukiwalny daje stały ruch z Google i YouTube miesiącami, a skoro Google już go rankuje na frazę, jest duża szansa, że AI Overviews go zacytują. Dodatkowo tytuły filmów wyszukiwalnych są jednoznaczne, a wirali — nie.

Jak znaleźć tematy: sprawdź, na jakie frazy filmy YouTube rankują w Google (analiza fraz organicznych dla `youtube.com/watch`), filtr na top 3, filtr `include` z terminami niszy.

**Checklista filmu, który ma rankować:**
1. **Tytuł zawiera frazę wyszukiwaną.** To nie miejsce na kreatywność ani clickbait. Kreatywność idzie do miniatury: tytuł obsługuje frazę, miniatura sprzedaje kliknięcie.
2. **Opis to prawdziwe streszczenie filmu**, z frazą docelową w pierwszych dwóch linijkach. Czyta to Google, AI i widzowie.
3. **Znaczniki czasu.** Zamieniają się w rozdziały YouTube, a rozdziały mogą pojawiać się w Google przy konkretnych zapytaniach. Dwie minuty pracy za dodatkową widoczność.
4. **Wypowiedz frazę w filmie.** Google rozumie treść audio i wideo (potwierdzone przez Liz Reid, VP of Search). Sama fraza w tytule nie wystarcza.
5. **Dopasuj format do tego, co już rankuje.** Jeśli w wynikach dominują tutoriale, rób tutorial; jeśli zestawienia, rób zestawienie. To dopasowanie do intencji, nie kapitulacja.

Każdy opublikowany film jest potencjalnym materiałem treningowym, nawet jeśli nie zostanie zacytowany od razu.

---

## 9. Techniczne AEO — sześć kontroli

**1. robots.txt — najczęstszy blocker, 5 minut roboty.** **[dane Ahrefs]** Ok. 5,9% ze 140 mln zbadanych witryn blokuje GPTBot. Boty do sprawdzenia: `GPTBot` i `OAI-SearchBot` (OpenAI), `ClaudeBot` (Anthropic), `Google-Extended` (Google). Częsta przyczyna nieświadomej blokady: odziedziczone szablony, stare konfiguracje oraz **Cloudflare, gdzie funkcja „instruct AI bot traffic with robots.txt" jest domyślnie włączona** i sama dopisuje reguły blokujące trening. Wejdź na `twojadomena.pl/robots.txt` i sprawdź te cztery nazwy.

**2. LLMs.txt — nie priorytetyzuj.** **[opinia autora, ale zgodna ze stanem faktycznym]** Propozycja standardu bez wsparcia żadnego dużego dostawcy: OpenAI nie używa, Anthropic publikuje własny plik, ale nie potwierdził, że jego crawlery go czytają, Google nie zaadaptował. Nie zaszkodzi, ale robots.txt jest tym plikiem, który realnie działa.

**3. JavaScript.** Gemini i Copilot renderują JS, **crawler ChatGPT nie**. Jeśli treść wczytuje się przez JS (SPA, część aplikacji React/Angular), ChatGPT widzi pustą skorupę. Rozwiązanie: server-side rendering. Test: wyłącz JavaScript w przeglądarce i odwiedź własną stronę; jeśli treść znika, masz problem.

**4. Szybkość ładowania — ważniejsza niż w klasycznym SEO.** Przy retrievalu w czasie rzeczywistym model pobiera, parsuje i tnie stronę na bieżąco; zbyt wolna strona bywa odrzucona, zanim w ogóle zostanie oceniona. Zoptymalizowane Core Web Vitals załatwiają większość sprawy.

**5. Czysta struktura HTML.** Poprawna hierarchia nagłówków (H1 tytuł, H2 sekcje główne, H3 podsekcje), sekcje uporządkowane, akapity po jednej myśli. Modele parsują treść, idąc za strukturą HTML, i mogą pociąć tekst na dowolnej granicy nagłówka — dlatego zasady z sekcji 6 (BLUF, atomowość, encje) są też wymogiem technicznym, nie tylko stylistycznym.

**6. Schema/dane strukturalne — dowody mieszane.** **[opinia autora]** Brak potwierdzonych danych, że schema poprawia szanse na cytowanie przez AI. Nie szkodzi, jeśli już masz — nie usuwaj; przy nowej stronie dodanie właściwych typów to dobry nawyk. Nie inwestuj w schema czasu przeznaczonego na powyższe punkty.

**Bonus: halucynowane URL-e.** **[dane Ahrefs]** Asystenci AI kierują na strony 404 2,87× częściej niż wyszukiwarka Google; ChatGPT prowadzi w tej statystyce (ok. 1% klikniętych URL-i to 404). Rób: sprawdź w analityce strony 404 dostające ruch z referrerów AI i ustaw przekierowanie na najbliższą realnie istniejącą stronę. To odzyskiwanie ruchu, który inaczej ginie.

---

## 10. Pomiar — trzy filary

**Filar 1: ruch odsyłający z AI.** Ustaw w GA4 własną grupę kanałów: Administracja → Wyświetlanie danych → Grupy kanałów, skopiuj domyślną, dodaj kanał „AI traffic" z regexem na źródło obejmującym `chatgpt.com`, `perplexity`, `gemini.google.com`, `copilot.microsoft.com`, `claude.ai`, `deepseek.com`. Raport: Pozyskiwanie → Pozyskiwanie ruchu → wybierz nową grupę.

**Traktuj tę liczbę jako zaniżoną.** Nie wszystkie platformy przekazują referrer: linki źródłowe ChatGPT tak, ale linki w treści na kontach płatnych mają `noreferrer` i są niewidoczne; Claude przekazuje; Perplexity przekazuje w wersji web, ale nie w aplikacji desktopowej; Copilot przekazuje w web, ale nie w Windows; Grok nie przekazuje wcale. Reszta ląduje jako ruch bezpośredni.

Co z tym robić: (a) sprawdź, które strony dostają ruch z AI — to są strony, które AI już poleca, trzymaj je aktualne i z jasnym CTA, bo jeśli strona nie była ruszana od roku, AI przestanie ją polecać; (b) sprawdź, które ważne strony **nie** dostają ruchu z AI mimo że powinny — problem może być w treści, w crawlowaniu albo temat po prostu jeszcze nie jest obsługiwany.

**Filar 2: aktywność botów AI.** Boty odwiedzają strony znacznie częściej niż ludzie, więc strony, które odwiedzają najintensywniej, są najlepszymi kandydatami na cytowania. Dwa typy: boty treningowe (GPTBot, Google-Extended) i boty wyszukująco-cytujące (ChatGPT-User, OAI-SearchBot) — te drugie realnie generują ruch. Źródło danych: logi serwera (darmowo, jeśli masz do nich dostęp) albo integracja z Cloudflare. Sygnał do wyłapania: bot cytujący, który wielokrotnie uderza w jedną stronę, oraz ważne strony, do których boty nie zaglądają w ogóle (problem z wykrywalnością — linkowanie wewnętrzne, struktura).

**Filar 3: samodzielna atrybucja — najważniejsza i najprostsza.** Duża część wpływu AI nie pojawia się w analityce w ogóle: ktoś pyta ChatGPT, dostaje nazwę, wpisuje ją w pasek adresu (ruch bezpośredni) albo googluje (ruch organiczny). Jedyny sposób to zapytać. **Dodaj pytanie „skąd o nas wiesz" do formularza rejestracji, procesu zakupowego albo ankiety po transakcji, z opcjami: asystent AI (ChatGPT, Perplexity, Claude) oraz wyszukiwarka z AI (AI Overviews).** Autor mówi wprost: jeśli masz zrobić tylko jedną rzecz z tego modułu, zrób tę. **[opinia autora — moim zdaniem słuszna i najtańsza z całego kursu]**

**[dane Ahrefs]** U nich ok. 3% konwersji w ostatnim roku pochodziło z AI wg deklaracji użytkowników, a odwiedzający z AI konwertują 23× lepiej niż z wyszukiwania organicznego. Nie dowiedzieliby się o tym bez pytania.

---

## 11. Czy to się opłaca i w jakim rytmie pracować

**Uczciwe postawienie sprawy przez autora:** ruch z AI to średnio ok. 0,25% całego ruchu witryny, a Google wysyła ok. 210× więcej ruchu niż wszystkie platformy AI razem. Patrząc na sam wolumen, można uznać, że nie warto.

**Argument za:** jakość ruchu. Ahrefs — konwersja 23× wyższa niż z organicznego. Vercel — 10% konwersji z ruchu AI. Tally — AI jako największy kanał pozyskania, wzrost ARR o milion dolarów. Mechanizm jest zrozumiały: model już wyjaśnił użytkownikowi, dlaczego jesteś dobrym dopasowaniem, więc ruch przychodzi wstępnie zakwalifikowany. Do tego dynamika: ruch z AI wzrósł 9,7× rok do roku, ChatGPT o 85% od stycznia i wysyła już więcej ruchu niż Reddit czy LinkedIn. **[dane Ahrefs i przypadki firm — bez niezależnej weryfikacji]**

**Argument właściwy** **[opinia autora]**: prawdziwą wartością nie jest klikalny ruch, tylko świadomość marki budowana wewnątrz rozmowy z AI. Każda rekomendacja to wyświetlenie, którego wcześniej nie było, i najczęściej kończy się późniejszym wyszukaniem nazwy albo rozpoznaniem marki w innym kanale. Nie da się tego zmierzyć precyzyjnie, tak samo jak nie da się przypisać sprzedaży do konkretnego billboardu.

**Plan na pierwszy tydzień** (kolejność autora, moim zdaniem trafna):
1. Sprawdź robots.txt pod kątem botów AI — 5 minut, najczęstsza blokada techniczna.
2. Ustaw analitykę: kanał AI w GA4 + pytanie „skąd o nas wiesz" w formularzu.
3. Zaktualizuj 5–10 najważniejszych stron pod świeżość — nowe dane, aktualne przykłady, nie sama data.
4. Zrób brand gap analysis i zapisz stan wyjściowy.
5. Wytypuj 10 stron, na których zdobycie wzmianki dałoby największy efekt.

**Rytm stały:** miesięczny przegląd czterech wskaźników (AI share of voice, domeny cytujące, pokrycie tematyczne, sentyment wzmianek) i kwartalny głębszy audyt konkurencji.

**Podatność AI na dezinformację — eksperyment Ahrefs.** Stworzyli fikcyjną markę premium i rozsiali trzy sprzeczne źródła (blog, Reddit, Medium), po czym odpytali osiem platform. Gemini i Perplexity powtarzały nieprawdę w 37–39% odpowiedzi (fałszywi założyciele, miasta, historie cenowe, podane jako fakty). ChatGPT okazał się odporniejszy: poniżej 7%, przy czym w 84% odpowiedzi cytował oficjalne FAQ marki.

Wniosek operacyjny, najmocniejszy z całego kursu: **wypełnij każdą lukę informacyjną o swojej firmie treścią oficjalną i konkretną.** FAQ odpowiadające wprost na typowe pytania, konkretne liczby, daty i fakty zamiast ogólników. Gdy model wybiera między prawdą ogólnikową a fikcją konkretną, wybiera fikcję konkretną.

---

## 12. Co z tego dla B2B: prawo, podatki, księgowość w Polsce

Filtr własny — kurs jest pisany pod SaaS i e-commerce, poniżej to, co przenosi się na kancelarię doradczą i co przenosi się słabo.

**Przenosi się najmocniej:**

- **Zapytania podatkowo-prawne to prawie w całości intencja informacyjna**, a 99,9% fraz wywołujących AI Overview jest informacyjnych. To znaczy, że ta kategoria treści dostanie AI Overview niemal zawsze i klasyczny ruch z „czym jest KSeF", „jak rozliczyć…", „termin złożenia…" będzie topniał najszybciej. Nie rezygnuj z tych treści — przestaw ich cel z kliknięcia na wzmiankę i wiarygodność.
- **Zapytania narzędziowe zostają twoje.** Kalkulator wynagrodzeń, kalkulator składki zdrowotnej, wzór umowy, checklista wdrożenia KSeF, terminarz podatkowy. AI nie wykona za użytkownika obliczenia na jego danych ani nie wygeneruje mu dokumentu. To najbezpieczniejsza inwestycja w ruch organiczny w tej branży i jednocześnie naturalny magnes na linki.
- **Wzmianki bez linku jako cel osobny.** Kancelaria już generuje komentarze eksperckie w mediach; korelacja 0,664 dla wzmianek marki oznacza, że komentarz w Prawo.pl czy Rzeczpospolitej bez linku dofollow nadal buduje widoczność w AI. To zmienia rachunek opłacalności reactive PR — dotąd oceniany głównie przez pryzmat linku.
- **Marki osobiste ekspertów mają własny profil widoczności.** W doradztwie to jest kluczowe: nazwisko doradcy podatkowego, jego specjalizacja i tematy, z którymi ma być kojarzony, to osobna encja do zmapowania i osobna analiza luk. Zbieżne z YMYL i z tym, czego szukają Quality Raterzy (reputacja autora, nie tylko strony).
- **Sześć rodzajów luk przekłada się wprost.** Luka narracyjna jest tu szczególnie kosztowna: jeśli AI opisuje kancelarię jako „biuro rachunkowe dla mikrofirm", a pozycjonujesz się w sporach podatkowych, tracisz zapytania o wyższej wartości.
- **Świeżość ma w tej branży sens merytoryczny, nie tylko algorytmiczny.** Przepisy się zmieniają, a modele preferują treść zaktualizowaną w ostatnich 30 dniach. Strony o KSeF, składce zdrowotnej, JPK, estońskim CIT itd. wymagają cyklu aktualizacyjnego wpisanego w proces — i to jest jednocześnie obrona przed cytowaniem nieaktualnego stanu prawnego z twojej własnej strony.
- **Odporność na dezinformację przez konkret.** Eksperyment z fikcyjną marką ma bezpośrednie przełożenie: publikuj konkretne, oficjalne informacje o kancelarii (skład zespołu, numery wpisów na listy zawodowe, specjalizacje, lokalizacje, model rozliczeń) w formie łatwej do wyciągnięcia. Ogólnikowe „doświadczony zespół profesjonalistów" przegrywa z konkretną fikcją.
- **Zasada atomowości i BLUF pasuje do treści prawnej.** Sekcja musi bronić się bez kontekstu, bo model wytnie ją i zacytuje osobno. To zresztą argument redakcyjny sam w sobie: fragment o terminie przedawnienia wyrwany z kontekstu ustawy jest ryzykiem merytorycznym, więc każda sekcja powinna nieść własne zastrzeżenia i podstawę prawną.

**Przenosi się słabiej lub wymaga ostrożności:**

- **Reddit i Quora** mają w polskim B2B prawno-podatkowym marginalne znaczenie w porównaniu z rynkiem anglojęzycznym. Odpowiedniki do sprawdzenia: grupy branżowe na Facebooku i LinkedIn, fora księgowe (np. społeczności wokół portali kadrowo-płacowych), wątki na Wykopie. Metoda z kursu (sprawdź, które strony forum rankują w Google w top 5 na frazy z niszy) działa dla dowolnej domeny i tu też ją stosuj.
- **Zestawienia typu „best X"** w polskim rynku prawniczym istnieją (rankingi kancelarii Rzeczpospolitej, Forbes, Legal 500, Chambers), ale są to rankingi redakcyjne z własnym procesem kwalifikacji, a nie listy do zdobycia outreachem. Nakład jest inny niż przy zestawieniach SaaS. Za to obecność w nich jest wysoce cytowalna.
- **Reklamowanie się przez doradców podatkowych, radców i adwokatów podlega ograniczeniom zasad etyki zawodowej.** Zanim wdrożysz taktyki „zdobądź wzmiankę na liście najlepszych" albo aktywność w wątkach społecznościowych z rekomendacją własnych usług, zweryfikuj to z zasadami odpowiedniego samorządu. Kurs Ahrefs tego wymiaru nie zna.
- **Dane Ahrefs o platformach dotyczą rynku anglojęzycznego.** Udziały cytowań (YouTube 5,6% w AI Overviews, mediana DR 90 w ChatGPT) nie muszą odwzorowywać zapytań po polsku, gdzie zestaw autorytatywnych źródeł jest inny i gdzie duże znaczenie mają serwisy urzędowe (podatki.gov.pl, ZUS, sejm.gov.pl, Infor, Prawo.pl). **Do zweryfikowania własnym badaniem:** jakie domeny są faktycznie cytowane w AI Overviews i ChatGPT dla polskich zapytań podatkowo-prawnych.
- **YouTube z korelacją 0,737** jest bardzo mocnym argumentem, ale w tej branży wymaga zgody na twarz eksperta i zaplecza produkcyjnego. Tania wersja: nagrywaj webinary i wystąpienia, które i tak się odbywają, publikuj z porządnym tytułem zawierającym frazę, streszczeniem w opisie i znacznikami czasu. Punkt „wypowiedz frazę w filmie" jest istotny, bo transkrypcja jest tym, co model czyta.

**Co zrobiłbym w pierwszej kolejności dla mentzen.pl:** robots.txt (5 min), pytanie „skąd o nas wiesz" w formularzach kontaktowych, cykl aktualizacji stron o zmieniających się przepisach, mapa encji obejmująca markę i nazwiska ekspertów, oraz własne badanie cytowanych domen dla polskich zapytań podatkowych, bo bez niego reszta priorytetów opiera się na danych z innego rynku.
