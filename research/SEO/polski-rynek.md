# Specyfika polskiego rynku SEO i fraz podatkowych

Destylat researchu z 2026-08-28 pod skill SEO dla mentzen.pl (kancelaria prawo/podatki/księgowość, B2B, WordPress, YMYL, rynek polski). Twierdzenia bez potwierdzenia w źródle pierwotnym oznaczone [do weryfikacji].

## TL;DR

1. Planuj wyłącznie pod Google — 89,46% rynku w Polsce (StatCounter, lipiec 2026); Bing (7,16%) obsługuj tylko biernie przez Bing Webmaster Tools jako drugie źródło danych o zapytaniach brandowych.
2. Frazy podatkowe informacyjne („jak rozliczyć…", „czym jest KSeF") to główna ofiara AI Overviews — ~80% zapytań wywołujących AIO jest informacyjnych, CTR pozycji #1 dla fraz z AIO spadł do 0,016. Dziel portfel fraz na koszyk „informacyjny (ryzyko AIO)" i „transakcyjny/lokalny/brandowy (odporny)" i licz KPI osobno.
3. Fleksja: warianty odmiany (`kancelaria podatkowa`/`kancelarii podatkowej`) = jedna strona; warianty leksykalne (`doradca podatkowy` vs `biuro rachunkowe`) = sprawdź SERP każdego z osobna. Narzędzia raportują odmiany osobno — agreguj wolumeny ręcznie przed priorytetyzacją.
4. Ogólnopolskie frazy prestiżowe (`doradztwo podatkowe`, `kancelaria prawna`) są poza zasięgiem szybkiego zwrotu. Priorytet: specjalizacja+lokalizacja, frazy problemowe („kontrola celno-skarbowa co robić") i brand+usługa.
5. Polski odpowiednik anglosaskiego „best X" to frazy cenowe i porównawcze: „ile kosztuje doradca podatkowy", „cennik biura rachunkowego", „inFakt czy biuro rachunkowe" — jednoznacznie zakupowe, obsługuj je dedykowanymi stronami.
6. Kalendarz podatkowy = darmowy kalendarz treści: publikuj 4–8 tygodni przed szczytem (PIT do 30 kwietnia, CIT-8 do 31 marca, zmiana formy opodatkowania — potocznie „do 20 lutego", ustawowo 20. dzień miesiąca po miesiącu pierwszego przychodu, KSeF, koniec roku). Jedna strona-hub na temat, aktualizowana co rok — nie nowy URL. Terminy potwierdzaj w tekstach jednolitych z Dz.U. (api.sejm.gov.pl/eli), nie w portalach; pełne podstawy prawne w `_notes/uzup-terminy-podatkowe.md`.
7. Brand „Mentzen" to dwie encje w jednej nazwie: polityk i kancelaria. Nie optymalizuj pod gołe „Mentzen"; buduj strony pod „kancelaria Mentzen", „Mentzen księgowość", „Mentzen cennik" i segmentuj ruch brandowy na usługowy vs newsowy, żeby kampanie wyborcze nie fałszowały KPI.
8. Treści adwokackie/radcowskie podlegają ograniczeniom etyki zawodowej (zakaz informowania o klientach, sprawach, stawkach); doradcy podatkowi mają większą swobodę. Cenniki i case studies planuj po stronie doradztwa podatkowego i księgowości.
9. Narzędzia: Senuto do fraz PL (najlepsze pokrycie długiego ogona, raport kanibalizacji), GSC jako jedyne źródło prawdy o realnym ruchu; wolumeny z każdego narzędzia to estymaty.
10. Klasyczne SEO wciąż jest fundamentem widoczności w AIO: ponad 60% cytowań AIO w Polsce pochodzi ze stron będących już w top 10 organicznie (raport Senuto, za webinarem Senuto/Vestigio).

---

## 1. Google.pl: monopol

**Rób: całą strategię pod Google, zero budżetu na inne wyszukiwarki.**
StatCounter, lipiec 2026, Polska: Google 89,46%, Bing 7,16%, Yandex 1,35%, DuckDuckGo 0,95%.
Źródło: https://gs.statcounter.com/search-engine-market-share/all/poland (odczyt 2026-08)

**Rób: podłącz Bing Webmaster Tools obok Search Console** — nie dla pozycji w Bing, ale jako darmowe drugie źródło do inwentaryzacji zapytań brandowych i dlatego, że Bing zasila część ekosystemu LLM.
Źródło: https://searchengineland.com/branded-search-seo-452676 (Dan Taylor, 2025-02-27)

**Unikaj: cytowania krążących w polskich publikacjach liczb „Google 98,49% mobile / 95,2% ogółem"** — nie zgadzają się z bieżącym StatCounter. Jeśli skill potrzebuje liczby, odczytuj StatCounter na dzień pisania.

## 2. AI Overviews na polskim rynku: skala i konsekwencje

Dane globalne (Ahrefs): obecność AIO koreluje ze spadkiem CTR pozycji #1 o ~58% (badanie na 300 tys. fraz: 150 tys. z AIO i 150 tys. bez; wcześniejsze badanie z kwietnia 2025 dawało ~34,5%). CTR #1 dla fraz informacyjnych bez AIO spadł z 0,076 (XII 2023) do 0,039 (XII 2025); dla fraz z AIO z 0,073 do 0,016.
Źródła: https://ahrefs.com/blog/ai-overviews-reduce-clicks-update/ ; https://ahrefs.com/blog/ai-overviews-reduce-clicks/

Dane polskie — wg Szymona Parzycha (Vestigio) na webinarze Senuto (2025-09-30, https://www.youtube.com/watch?v=MKwi0qmVSS8; liczby z auto-transkrypcji, przed cytowaniem na zewnątrz sprawdź w raporcie Senuto o AIO):

- Raport Senuto: ~24% fraz z pozycją organiczną wywołuje blok AIO, niezależnie od pozycji; spadek kliknięć w audytach Parzycha 12–18% (skrajnie: wydawca -60%).
- Ok. 80% zapytań wywołujących AIO to zapytania informacyjne, transakcyjne <6% (badanie Semrush).
- Ponad 60% cytowań w AIO w Polsce pochodzi ze stron już będących w top 10 organicznie; średnia pozycja cytowanego źródła 6–7. Wniosek Parzycha: gros budżetu trzymaj na klasycznych fundamentach SEO, działania pod AIO to rozszerzenie, nie zamiennik.
- Status źródła w AIO odzyskuje 20–40% utraconego ruchu z fraz objętych blokiem; realistyczny wskaźnik cytowań to 20–30% fraz. Proces retrieval nie jest deterministyczny — nie raportuj „jesteśmy w AIO na frazę X" jako stanu trwałego.
- Efekt „paszczy krokodyla" w GSC: kliknięcia spadają, wyświetlenia rosną, bo GSC liczy każdy link na SERP osobno (wynik organiczny + link w AIO + panel boczny = 3 wyświetlenia). **Nie interpretuj rosnących wyświetleń jako rosnącego popytu.**

**Rób: dziel frazy na koszyki i licz KPI osobno.** „Jak rozliczyć PIT-36" to klasyczna ofiara AIO; „kancelaria podatkowa Toruń", „Mentzen abonament" — nie. Wg kursu Ahrefs AEO (Samo Cerar, notatka video-uza9GX0E2mw): zapytania podatkowo-prawne informacyjne dostaną AIO niemal zawsze — nie rezygnuj z tych treści, ale przestaw ich cel z kliknięcia na wzmiankę/cytowanie i wiarygodność.

**Rób: zanim wdrożysz działania naprawcze, zestaw ruch blogowy z formularzami kontaktowymi w tym samym okresie** (wg Parzycha: znane są domeny, gdzie ruch blogowy spadł, a sprzedaż wzrosła). Metryką sukcesu bloga podatkowego są zapytania ofertowe, nie sesje.

**Rób: mierz obecność w AIO narzędziem lub ręcznie — GSC nie ma filtra AI Overviews.** Udział AIO na własnej próbce fraz branżowych zmierz samodzielnie przed przyjęciem liczby 24% [do weryfikacji na własnych danych].

**Zapytania odporne na AIO — wg kursu Ahrefs najbezpieczniejsza inwestycja w tej branży:** narzędziowe i generujące działanie: kalkulator składki zdrowotnej, kalkulator ryczałt/liniowy/skala, wzory dokumentów, terminarz podatkowy, checklisty wdrożeń. AI nie wykona obliczenia na danych użytkownika ani nie wygeneruje mu dokumentu; to jednocześnie magnesy na linki. Warunek YMYL: jawna metodologia, podstawa prawna, data aktualizacji, zastrzeżenie „to nie porada w indywidualnej sprawie".

## 3. Fleksja polska w keyword research

**Rób: pisz naturalnie, jedną odmianą w tytule i H1.** Google deklaruje, że systemy dopasowania rozumieją stronę względem wielu zapytań bez użycia dokładnych terminów; powtarzanie odmian „na zapas" to keyword stuffing.
Źródła: https://www.google.com/search/howsearchworks/how-search-works/ranking-results/ ; https://developers.google.com/search/docs/fundamentals/seo-starter-guide

**Rób: rozróżniaj trzy klasy wariantów** (praktyczny podział, źródło branżowe: https://semcore.pl/keyword-research/):

- **fleksyjne** (`kancelaria podatkowa` / `kancelarii podatkowej` / `kancelarię podatkową`) — jedna intencja, jedna strona; nigdy osobne podstrony;
- **leksykalne** (`doradca podatkowy` / `doradztwo podatkowe` / `biuro rachunkowe` / `księgowość dla firm`) — inne intencje, inne SERP-y; przed decyzją o liczbie stron sprawdź SERP każdej frazy z osobna;
- **pytające** (`jak`, `ile`, `kiedy`, `czy`) — osobny ruch informacyjny, miejsce w FAQ i na blogu.

**Rób: agreguj wolumeny wariantów fleksyjnych ręcznie przed priorytetyzacją** — narzędzia raportują odmiany jako osobne rekordy, więc wolumen jednej formy zaniża potencjał tematu. Suma odmian bywa wielokrotnością formy mianownikowej [do weryfikacji na danych Senuto/GSC — brak badania z liczbami dla PL].

**Rób: kontroluj kanibalizację — w polskim łatwiej ją wywołać przypadkiem.** Bogata odmiana i synonimia sprawiają, że teksty o „rozliczeniu ryczałtu" i „ryczałcie ewidencjonowanym" trafiają w ten sam klaster, choć autorom wydają się różne. Senuto ma dedykowany raport kanibalizacji (dokumentacja: https://wiki.senuto.com — deep link podany w notatkach zwraca 404, odczyt 2026-08). Reguła domyślna: jedna strona na intencję, nie na frazę.

**Rób: slugi bez polskich znaków, w mianowniku, krótkie** (`/doradztwo-podatkowe/`, nie `/doradztwa-podatkowego-dla-firm/`). To konwencja branżowa, nie wymóg Google [do weryfikacji jako wymóg; jako konwencja — bezpieczna].

## 4. Specyfika fraz podatkowo-prawnych: intencja i język klienta

**Rób: buduj macierz fraz od dołu lejka** (wg autora filmu o money keyword matrix, notatka video-N1m9hlMsMKg: „ludzie nie szukają nazwy usługi dla zabawy — szukają, bo jej potrzebują"; niskie wolumeny akceptuj świadomie). Przełożenie na kancelarię:

- **Nazwy usług:** `doradca podatkowy`, `biuro rachunkowe`, `obsługa księgowa`, `audyt podatkowy`, `ceny transferowe`. Uwaga na rozjazd języka: przedsiębiorca wpisuje „księgowość dla spółki z o.o.", nie „usługi w zakresie rachunkowości finansowej".
- **Nisze (największa przewaga):** forma prawna i branża jako modyfikatory z realnym wolumenem i niską konkurencją — spółka z o.o., fundacja rodzinna, estoński CIT, IT/kontraktorzy B2B, e-commerce, transport, budowlanka, gabinety medyczne, spółki z kapitałem zagranicznym. Trzeci wymiar: miasto.
- **Konkurenci:** w Polsce frazami porównawczymi o wysokiej intencji są zestawienia z oprogramowaniem („inFakt czy biuro rachunkowe", „księgowość online czy księgowa") — nie tylko z innymi kancelariami.
- **Problemy i objawy — najwyższa intencja w całym portfelu:** „kontrola celno-skarbowa co robić", zajęcie rachunku, wezwanie z KAS, utrata prawa do estońskiego CIT, odpowiedzialność członka zarządu za zaległości spółki, przekształcenie JDG w spółkę, sukcesja. Człowiek wpisujący te frazy jest w trybie zakupu usługi, choć jeszcze o tym nie wie.

**Rób: frazy cenowe traktuj jako polski substytut „best".** Modyfikator „best" po polsku działa słabo; realne odpowiedniki komercyjne to „ile kosztuje doradca podatkowy", „cennik biura rachunkowego", „ranking biur rachunkowych", „[marka] opinie". Obsługuj je dedykowanymi stronami z widełkami cen — w polskiej branży prawniczej jawny cennik to rzadkość, więc przewaga konwersyjna.

**Unikaj: listicle'a „najlepsze kancelarie" pisanego przez kancelarię.** W YMYL to widoczny konflikt interesów plus ryzyko z zasad etyki zawodowej. Bezpieczny odpowiednik zachowujący intencję porównawczą: porównanie **modeli obsługi**, nie podmiotów — „księgowość online vs biuro rachunkowe vs księgowa in-house dla spółki z o.o.", „kiedy doradca podatkowy, a kiedy wystarczy księgowa".

**Rób: przed pisaniem sprawdź typ treści faworyzowany w SERP dla frazy** (landing vs artykuł vs zestawienie) — publikowanie landing page'a tam, gdzie Google promuje zestawienia, to przegrana walka z formatem niezależnie od jakości tekstu.

**Rób: dopasowuj typ strony do intencji — osobna strona na osobną intencję.** Wg Nathana Gotcha (Local SEO 2026, notatka video-bfBwk2KK9jc): „kontrola celno-skarbowa — pomoc pilna" to inna strona niż „obsługa księgowa spółki z o.o." (inna pilność, inny decydent, inny formularz). Typowy błąd kancelarii: jedna strona „Usługi" z listą wypunktowaną i bogaty blog o nowelizacjach — kolejność powinna być odwrotna: najpierw strony usług pod frazy transakcyjne, potem komentarze do ustaw.

## 5. Konkurencyjność i priorytetyzacja

**Rób: zakładaj, że ogólnopolskie frazy główne (`kancelaria prawna`, `doradztwo podatkowe`, `biuro rachunkowe`) wymagają lat i dużych zasobów** — niemal każda kancelaria w dużym mieście inwestuje w widoczność. Źródło branżowe: https://prawnymarketing.pl/pozycjonowanie-kancelarii/

**Rób: priorytetyzuj trzy koszyki ponad frazami prestiżowymi:**
1. specjalizacja + lokalizacja (`doradca podatkowy Toruń`, `księgowość dla spółek Warszawa`) — mniejszy wolumen, wyższa gotowość do kontaktu;
2. frazy problemowe / długi ogon (`estoński CIT dla spółki z o.o. warunki`, `czy fundacja rodzinna płaci CIT`);
3. transakcyjne i brandowe — najwyższa konwersja, najniższe ryzyko AIO.

**Rób: oceniaj konkurencję po sile domen faktycznie rankujących, nie po wskaźniku keyword difficulty.** Sama reguła pochodzi od Gotcha (przykład z filmu: KD 39, a w topie domeny z DR 0, 1 i 17 — rynek USA). Obserwacja, że w polskich frazach lokalnych prawno-podatkowych czołówka bywa bardzo słaba domenowo, to przełożenie autora notatki, nie teza Gotcha [do weryfikacji na własnej próbce fraz].

**Ramy czasowe:** teza „ranking w 90 dni przy KD 0–10" pochodzi z rynku anglojęzycznego bez YMYL — dla podatków w Polsce zakładaj dłużej (YMYL podnosi poprzeczkę reputacyjną). Kierunek pozostaje słuszny: wąskie frazy branżowo-lokalne rankują znacznie szybciej niż „doradca podatkowy Warszawa".

**Rób: każdą stronę o podatkach traktuj jako YMYL.** Search Quality Rater Guidelines (2025-09-11, https://guidelines.raterhub.com/searchqualityevaluatorguidelines.pdf) zaliczają podatki do bezpieczeństwa finansowego; przy YMYL Trust jest fundamentem. Minimum dla mentzen.pl: podpis autora z tytułem zawodowym i numerem wpisu (KIDP/OIRP/NRA), strony autorów z biogramem i listą publikacji, data publikacji + data aktualizacji + „stan prawny na DD.MM.RRRR", widoczne KRS/NIP/adres. Szczegóły E-E-A-T w osobnym pliku researchu (eeat-ymyl).

**Przewaga treściowa na zalanym rynku:** rynek treści podatkowych w PL to głównie przepisane komunikaty MF. Fosa, którą kancelaria ma z urzędu: dane własne (statystyki postępowań, czasy kontroli, koszty wdrożeń — agregowane i anonimizowane, nigdy identyfikowalne przypadki klientów), sygnatury interpretacji KIS i wyroków NSA, stanowisko eksperta, komentarz do projektu ustawy w dniu publikacji. Konkret i liczba wygrywają ze streszczeniem ustawy — także w cytowaniach AI.

## 6. Sezonowość: kalendarz podatkowy jako kalendarz treści

**Rób: publikuj/aktualizuj treść sezonową 4–8 tygodni przed szczytem.** Materiał opublikowany przed terminem łapie research; opublikowany w terminie łapie panikę i mniejszy wolumen (wg Exposure Ninja, notatka video-vrGLaJOAKas — ich case sezonowy: +280% przychodu organicznego z publikacji tuż przed sezonem, z trafieniami do AIO i Google Discover).

Terminy w tabeli zweryfikowane w tekstach jednolitych z Dz.U. (research 2026-08-28, pełne podstawy prawne i cytaty: `_notes/uzup-terminy-podatkowe.md`). Przed publikacją odświeżaj weryfikację przez `api.sejm.gov.pl/eli/acts/DU/{rok}/{poz}/text.pdf` — kalendarz „Ważne terminy" MF działa tylko na archiwalnej domenie, ładuje dane skryptem i nie nadaje się ani do cytowania, ani jako link wychodzący. Błędny termin podatkowy na stronie kancelarii to ryzyko wizerunkowe większe niż utrata pozycji.

| Okno publikacji | Temat | Termin (szczyt) i podstawa |
|---|---|---|
| 1–15 grudnia | wybór formy opodatkowania na nowy rok (symulacje skala/liniowy/ryczałt/estoński); ZAW-RD dla wchodzących w estoński CIT od stycznia; obowiązki płatnika: PIT-4R, PIT-11, ZUS IWA | 20 lutego / 31 stycznia (CIT art. 28j ust. 1 pkt 7; PIT art. 38 ust. 1a, art. 42g ust. 1) |
| 1–20 stycznia | zmiana formy opodatkowania krok po kroku; zmiany podatkowe nowego roku | „20 lutego" to skrót poradnikowy: ustawowo 20. dzień miesiąca po miesiącu pierwszego przychodu w roku (PIT art. 9a ust. 2; ryczałt art. 9 ust. 1) — pierwszy przychód w marcu daje termin 20 kwietnia; dobry hak na tekst |
| 1–15 lutego | CIT-8 i zapłata CIT, sporządzenie sprawozdania finansowego, IFT-2R | 31 marca przy roku kalendarzowym (CIT art. 27 ust. 1: koniec 3. miesiąca; UoR art. 52 ust. 1) |
| 15 lutego – 5 marca | sezon PIT: PIT-36/36L/37/28/38/39, ulgi, Twój e-PIT; danina solidarnościowa | 30 kwietnia; okno składania od 15 lutego (PIT art. 45 ust. 1; DSF-1: PIT art. 30h ust. 4); PIT-28 ma ten sam termin — stary termin lutowy wciąż krąży po sieci, punktuj to |
| 20 marca – 10 kwietnia | roczne rozliczenie składki zdrowotnej: dopłata i wniosek o zwrot nadpłaty | 20 maja (dokument za kwiecień; ustawa zdrowotna art. 81 ust. 2k–2l); wniosek o zwrot tylko miesiąc od terminu PIT, po terminie bez rozpoznania (art. 81 ust. 2m–2o) — najmocniejszy hak maja |
| 20 marca – 10 kwietnia | zatwierdzenie sprawozdania, uchwały wspólników, podział zysku | 30 czerwca (UoR art. 53 ust. 1) |
| 1–15 maja | złożenie sprawozdania do KRS: PRS, S24, sankcje | 15 dni od zatwierdzenia (UoR art. 69 ust. 1); przy zatwierdzeniu 30.06 → 15 lipca |
| 1–15 czerwca | JPK ksiąg rachunkowych po zmianie z 1.07.2026 | 31 lipca (Dz.U. 2026 poz. 779: w PIT sztywna data, w CIT koniec 7. miesiąca po roku podatkowym — nie mieszać sformułowań); [do weryfikacji] pierwszy rocznik objęty nowym terminem — brak przepisu przejściowego, do potwierdzenia wykładnią MF przed publikacją |
| 1–15 września | lokalna dokumentacja cen transferowych (local file), progi | 31 października (CIT art. 11k ust. 1: koniec 10. miesiąca) |
| 1–15 października | informacja TPR i ORD-U: kto podpisuje, zwolnienia | TPR: 30 listopada (CIT art. 11t ust. 1, potwierdzone na podatki.gov.pl/ceny-transferowe); ORD-U: 30 listopada wynika z wyliczenia (§ 3 ust. 3 rozporządzenia, Dz.U. 2024 poz. 1452 + Ordynacja art. 12 § 3), [do weryfikacji] brak potwierdzenia dziennej daty przez MF |
| 1–15 listopada | grupowa dokumentacja cen transferowych (master file) | 31 grudnia (CIT art. 11p ust. 1: koniec 12. miesiąca) |
| 1–15 listopada 2026 | KSeF dla najmniejszych firm (sprzedaż fakturowana ≤ 10 tys. zł/mies.) | 1 stycznia 2027 (mapa drogowa MF, ksef.podatki.gov.pl/etapy-wdrozenia-ksef); to komunikat, nie przepis — podstawę ustawową dołóż przed publikacją; etapy 1.02.2026 i 1.04.2026 już minęły — nadają się na „co się zmieniło", nie „przygotuj się" |
| cały rok, cykl miesięczny | terminarz serwisowy na najbliższy miesiąc | 20. dnia: zaliczki PIT/CIT/ryczałt, ZUS dla JDG i spółek osobowych; 25.: JPK_V7 i VAT; 15.: ZUS płatników z osobowością prawną |

**Rób: w tekstach o terminach cytuj regułę przesunięcia poprawnie.** Sobota liczy się jak dzień wolny, a termin przechodzi na pierwszy dzień roboczy po całym ciągu dni wolnych (Ordynacja podatkowa art. 12 § 5, Dz.U. 2026 poz. 622); dla składek ZUS reguła identyczna (zus.pl). Nawet biznes.gov.pl ma tu błędy (art. 00236 przesuwa termin PIT na 1 maja — dzień ustawowo wolny; art. 00277 i 00287 pomijają sobotę) — pokazanie tego w treści to tani dowód, że kancelaria czyta przepis, a nie przepisuje poradnik.

**Rób: jedna stała strona-hub per temat sezonowy, aktualizowana co rok — nie nowy URL co rok.** Strona z historią i linkami wraca na pozycje szybciej niż świeży URL. Wyjątek: zmiana stanu prawnego czyniąca starą treść mylącą — wtedy nowa strona + przekierowanie lub jawne archiwum.

**Rób: aktualizując, nie zmieniaj samej daty — dopisz stan prawny i nowe liczby** (wg Parzycha/Senuto: „stan prawny na DD.MM.RRRR" to konkret, który modele chętnie cytują; w prawie i podatkach faza śmierci treści przychodzi z każdą nowelizacją, nie po latach).

**Rób: sezonowość weryfikuj w Google Trends / Senuto z oknem 5 lat**, szukając powtarzalnych szczytów w tych samych miesiącach. Okna publikacji w tabeli wyliczono wstecz od terminów ustawowych, nie z danych o wolumenie [do weryfikacji — krzywe dla co najmniej: „zmiana formy opodatkowania", „PIT termin", „CIT-8", „składka zdrowotna rozliczenie roczne", „sprawozdanie finansowe KRS", „TPR termin"]. Nawet jeśli szczyt zapytań wypada tuż przed terminem, publikacja 6–8 tygodni wcześniej pozostaje właściwa — daje czas na indeksację przed falą.

## 7. Brand „Mentzen": jedna nazwa, dwie encje

Kontekst: Sławomir Mentzen to jednocześnie doradca podatkowy (Kancelaria Mentzen, Mentzen S.A. na NewConnect od 2024 — wniosek o wprowadzenie akcji planowany na koniec maja 2024, debiut zapowiadany na połowę 2024; ponad 21 mln zł przychodu w 2023, +28% r/r; liczba obsługiwanych firm „ponad 3300" [do weryfikacji — nie ma jej w cytowanych źródłach]) i jeden z najbardziej rozpoznawalnych polityków. Nazwa marki i nazwisko polityka to jedna encja leksykalna, ale dwie encje w rozumieniu Google, z różnymi intencjami.
Źródła: https://mycompanypolska.pl/artykul/slawomir-mentzen-rozkreca-biznes-jego-kancelaria-zarobila-ponad-21-mln-zl/14025 ; https://rynekprawniczy.pl/2024/04/08/kancelaria-podatkowo-prawna-konfederackiego-polityka-wchodzi-na-gielde/

### Szanse

- **Ruch brandowy to najtańszy i najodporniejszy na AIO ruch w portfelu.** Buduj i broń kombinacji brand+usługa: `Mentzen księgowość`, `Mentzen abonament`, `Mentzen cennik`, `Mentzen opinie`, `kancelaria Mentzen kontakt` — każda z dedykowaną, jednoznaczną stroną docelową (inaczej ruch nawigacyjny rozjeżdża się na LinkedIn i katalogi — wg filmu Semrush o 12 sygnałach autorytetu marki, notatka video-VOb_QjlrgpE).
- **Monitoruj zapytania współwystępujące** (brand + fraza niebrandowa: „Mentzen estoński CIT", „kancelaria Mentzen KSeF") w GSC — filtr na nazwę, regex wykluczający czysto brandowe. Wg tego samego filmu: to najlepszy dostępny miernik, czy marka skleiła się z tematem; mierz osobno nazwę kancelarii i nazwiska doradców.
- **Encja w Grafie Wiedzy:** spójne NAP na stronie, w GBP, KRS i profilach branżowych; Organization schema z `sameAs`.
- **Rozpoznawalność nazwiska jako akcelerator linków:** pilnuj, żeby publikacje o kancelarii linkowały do mentzen.pl, nie tylko do profili społecznościowych.

### Ryzyka

- **Rozjazd intencji.** Gołe „Mentzen" ma dominującą intencję polityczno-newsową; SERP zajmą Wikipedia i portale. **Nie optymalizuj strony głównej pod gołe nazwisko** — optymalizuj pod `kancelaria Mentzen`, `Mentzen doradztwo podatkowe`.
- **SERP brandowy zajęty przez osoby trzecie** (Search Engine Land: nieaktualne treści i przypadkowe pliki rankujące na frazy brandowe, nieścisłości w podsumowaniach AI — https://searchengineland.com/branded-search-seo-452676). Przy „Mentzen opinie" użytkownik zobaczy materiały o sporach politycznych i prawnych niedotyczących usług. **Rób:** monitoruj SERP dla `Mentzen`, `Mentzen opinie`, `kancelaria Mentzen`; utrzymuj własne strony odpowiadające na te zapytania (opinie klientów, case'y, FAQ, „o kancelarii"). Regularnie pytaj też czaty AI o nazwę kancelarii i sprawdzaj, z czego budują odpowiedź — jeden negatywny wątek na forum może trafić do odpowiedzi AI jako fakt (wg Parzycha/Senuto). **Unikaj:** wypierania treści krytycznych metodami wykraczającymi poza publikowanie własnych, prawdziwych materiałów.
- **Szum wyborczy w KPI.** Ruch brandowy w kampaniach rośnie, ale to intencja polityczna, nie zakupowa. **Segmentuj raportowanie:** „usługowy" (brand + modyfikator usługowy) vs „newsowy" (gołe nazwisko, tematy polityczne). Bez tego skok sesji w kampanii wygląda jak sukces SEO, choć nie generuje leadów.
- **Ryzyko reputacyjne jednej osoby w YMYL.** Opieraj E-E-A-T na zespole i tytułach zawodowych wielu doradców, nie wyłącznie na jednym nazwisku — uniezależnia widoczność merytoryczną od cyklu newsowego. Wg kursu Ahrefs: nazwisko każdego doradcy to osobna encja z własnym profilem widoczności i własną analizą luk; szczególnie kosztowna jest luka narracyjna (AI opisuje kancelarię jako „biuro rachunkowe dla mikrofirm", gdy pozycjonuje się w sporach podatkowych).
- **Nie skaluj linków szybciej niż rośnie popyt brandowy** — nadmiar linków wobec realnego wolumenu wyszukiwań nazwy to sygnał przeSEOwania (hipoteza z patentu Google, wg filmu Semrush; traktuj jako ostrożnościową).

## 8. Ograniczenia zawodowe wpływające na treści

**Rób: różnicuj treści według zawodu, który je firmuje.** Stan wg opracowań wtórnych [do weryfikacji w źródłach pierwotnych przed wdrożeniem]:

- **Adwokaci:** uchwała NRA z 26 maja 2023 zniosła całkowity zakaz reklamy, ale wprowadziła ograniczenia treści informacji handlowych.
- **Radcowie prawni:** Kodeks Etyki dopuszcza „informowanie o wykonywaniu zawodu" z zakazem reklamy nachalnej.
- **Obaj:** ograniczenia dotyczące informacji o prowadzonych sprawach i klientach — wg opisu uchwały NRA referencje i opisy spraw wymagają **pisemnej zgody klienta**, a informacja handlowa nie może być porównawcza, wartościująca ani wprowadzająca w błąd (nie jest to zakaz bezwarunkowy, jak podają skróty w blogach branżowych).
- **Doradcy podatkowi:** nie podlegają ograniczeniom w tym zakresie.
Źródła: https://www.ibif.pl/blog/strategie-marketingowe/koniec-zakazu-reklamy-adwokackiej-na-czym-polegaja-nowe-zasady ; https://radcaprawny.kirp.pl/aktualnosci/zakaz-reklamy-w-kodeksach-etycznych-radcow-prawnych-i-lekarzy/ ; https://kidp.pl/aktualnosciall.php/10/5940

**Praktyczna konsekwencja dla architektury treści:** cenniki, case studies, „nasze sukcesy" i rankingowe taktyki wzmiankowe planuj po stronie doradztwa podatkowego i księgowości; po stronie usług adwokackich/radcowskich każdą taką treść konsultuj z compliance. Przed wdrożeniem potwierdź stan w aktualnym Zbiorze Zasad Etyki Adwokackiej i Kodeksie Etyki Radcy Prawnego.

## 9. Narzędzia dla rynku PL

| Narzędzie | Rola | Uwagi |
|---|---|---|
| **Google Search Console** | jedyne źródło prawdy: realne zapytania, CTR, pozycje | brak metryki AIO; pamiętaj o artefakcie wyświetleń (sekcja 2) |
| **Senuto** | podstawowe do fraz PL: baza ponad 18 mln fraz (deklaracja producenta, senuto.com, odczyt 2026-08), sezonowość, kanibalizacja, raport top 500 domen cytowanych w AIO | najlepsze pokrycie długiego ogona PL [teza branżowa] |
| **Semstorm** | konkurencja, monitoring reklam | |
| **Ahrefs / Semrush** | backlinki, benchmarki globalne | słabsze pokrycie długiego ogona PL niż Senuto [do weryfikacji własnym porównaniem] |
| **Google Trends** | sezonowość, okno 5 lat | |
| **Bing Webmaster Tools** | drugie źródło zapytań brandowych | |

Twierdzenie „Senuto wykrywa o 35% więcej polskich fraz niż Ahrefs" pochodzi z bloga bez metodologii — nie cytuj; przy kosztownej decyzji zrób własne porównanie na mentzen.pl.

Źródła wiedzy do skilla: Google Search Central (https://developers.google.com/search/docs), Search Quality Rater Guidelines, blogi Ahrefs/Search Engine Land/Moz (badania z danymi). Polskie blogi agencyjne (Senuto, Semstorm, eActive, widoczni, Semcore, Verseo) — użyteczne do kontekstu i nazewnictwa, **nie jako źródło liczb**.

**Specyfika ekosystemu cytowań PL [do weryfikacji własnym badaniem]:** dane Ahrefs o platformach (udział YouTube w AIO, mediana DR w ChatGPT, rola Reddita) dotyczą rynku anglojęzycznego. W polskich zapytaniach podatkowych zestaw autorytatywnych źródeł jest inny — serwisy urzędowe (podatki.gov.pl, ZUS, sejm.gov.pl) i portale branżowe (Infor, Prawo.pl, pit.pl). Reddit/Quora mają w polskim B2B marginalne znaczenie; sprawdź zamiast tego grupy FB/LinkedIn, fora księgowe i Wykop metodą „które strony forów rankują w top 5 na frazy z niszy". Zrób własne badanie domen cytowanych w AIO/ChatGPT dla polskich zapytań podatkowych — bez niego priorytety opierają się na danych z innego rynku.

## 10. Zastosowanie dla mentzen.pl — decyzje dla skilla

1. **Segmentacja portfela fraz na starcie każdej analizy:** (a) informacyjne z ryzykiem AIO, (b) transakcyjne/problemowe, (c) lokalne, (d) brandowe usługowe, (e) brandowe newsowe. KPI i rekomendacje osobno per koszyk; wzrost wyświetleń w GSC nigdy nie jest sam w sobie sygnałem sukcesu.
2. **Reguła kanibalizacji:** jedna strona na intencję; warianty fleksyjne nigdy nie uzasadniają nowej podstrony; warianty leksykalne rozstrzyga porównanie SERP-ów.
3. **Priorytet treściowy:** strony usług pod frazy „forma prawna/branża + usługa" i frazy problemowe przed kolejnymi komentarzami do nowelizacji. Frazy cenowe („ile kosztuje…", „cennik…") obsłużone stronami z widełkami — po stronie doradztwa podatkowego/księgowości, nie usług adwokackich.
4. **Kalendarz:** generuj plan publikacji z kalendarza podatkowego z wyprzedzeniem 4–8 tygodni; huby sezonowe aktualizowane w miejscu, z „stan prawny na…"; każdy termin podatkowy w publikowanej treści weryfikowany w tekście jednolitym z Dz.U. (api.sejm.gov.pl/eli), z podatki.gov.pl/biznes.gov.pl jako wykładnią pomocniczą — nie odwrotnie.
5. **Brand:** monitoring SERP i odpowiedzi AI dla `Mentzen`, `Mentzen opinie`, `kancelaria Mentzen`; dedykowane strony pod brand+usługa; raportowanie ruchu brandowego z podziałem usługowy/newsowy; E-E-A-T rozłożony na cały zespół doradców.
6. **Zapytania odporne na AIO jako inwestycja pierwszego wyboru:** kalkulatory, wzory, terminarze — z metodologią, podstawą prawną i zastrzeżeniem prawnym.
7. **Do domknięcia przed finalizacją skilla:** wsparcie Google dla typów danych strukturalnych LegalService/AccountingService, pomiar udziału AIO na własnej próbce fraz, stan kodeksów etyki NRA/KIRP w źródłach pierwotnych, inwentaryzacja SERP-u brandowego jako baseline, badanie domen cytowanych w AIO/ChatGPT dla polskich fraz podatkowych. Terminy podatkowe domknięte w `_notes/uzup-terminy-podatkowe.md`; otwarte pozostają wyłącznie: pierwszy rocznik nowego terminu JPK ksiąg (31 lipca), dzienna data ORD-U w wykładni MF, rzeczywista sezonowość wyszukiwań.
