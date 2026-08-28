# Specyfika polskiego rynku SEO (kontekst: mentzen.pl, B2B, prawo/podatki/księgowość)

Research: 2026-08-28. Rynek: Polska, Google.pl.

Legenda zaufania źródeł:
- **[A]** źródło pierwotne / wysokozaufane (Google Search Central, How Search Works, StatCounter, Ahrefs, Search Engine Land, samorządy zawodowe)
- **[B]** źródło branżowe wtórne (blogi agencyjne PL, dokumentacja narzędzi) — traktuj jako hipotezę, nie jako fakt
- **[do weryfikacji]** twierdzenie, którego nie potwierdziłem w źródle pierwotnym

---

## 1. Google.pl: monopol i co z niego wynika

**Rób: planuj SEO wyłącznie pod Google, nie rozpraszaj budżetu na inne wyszukiwarki.**
StatCounter, lipiec 2026, Polska (wszystkie urządzenia): Google 89,46%, Bing 7,16%, Yandex 1,35%, DuckDuckGo 0,95%.
Źródło [A]: https://gs.statcounter.com/search-engine-market-share/all/poland (dane: lipiec 2026)

Dlaczego: przy ~90% udziału każda optymalizacja pod Bing/Yandex ma marginalny zwrot. Udział Bing rośnie jednak wolno i to on zasila część ekosystemu LLM — nie jest to powód do osobnej strategii, ale do niewykluczania Bing Webmaster Tools jako darmowego źródła danych o zapytaniach brandowych.

**Rób: traktuj Search Console + Bing Webmaster Tools jako komplet do inwentaryzacji fraz brandowych.**
Search Engine Land rekomenduje ten duet jako punkt wyjścia do identyfikacji zapytań brandowych.
Źródło [A]: https://searchengineland.com/branded-search-seo-452676 (Dan Taylor, 2025-02-27)

**[do weryfikacji]** Krążące w polskich publikacjach liczby „Google 98,49% na mobile / 95,2% ogółem" nie zgadzają się z bieżącym odczytem StatCounter (89,46%). Nie cytuj ich; jeśli potrzebna liczba, odczytaj StatCounter na dzień pisania tekstu.

---

## 2. AI Overviews a ruch na treści podatkowo-prawne

**Unikaj: budowania modelu ruchu opartego na CTR sprzed 2024 r. dla fraz informacyjnych.**
Ahrefs (aktualizacja badania): obecność AI Overview koreluje ze spadkiem CTR pozycji #1 o ~58%. Wcześniejsze badanie na 300 tys. fraz dawało ~34,5%. CTR pozycji #1 dla fraz informacyjnych spadł z 0,076 (XII 2023) do 0,039 (XII 2025); dla fraz z AIO z 0,073 do 0,016.
Źródła [A]: https://ahrefs.com/blog/ai-overviews-reduce-clicks-update/ oraz https://ahrefs.com/blog/ai-overviews-reduce-clicks/

**Rób: dziel frazy na „informacyjne (ryzyko AIO)" i „transakcyjne/lokalne/brandowe (odporne)" i licz KPI osobno dla obu koszyków.**
Dlaczego: „jak rozliczyć PIT-36" to klasyczna ofiara AIO; „kancelaria podatkowa Toruń" i „Mentzen abonament" to nie.

**Rób: mierz obecność w AIO ręcznie lub narzędziem — Search Console nie ma filtra ani metryki AI Overviews.** [B]
Źródło [B]: https://www.aivisible.pl/blog/jak-sledzic-widocznosc-w-google-ai-overviews (2026)

**[do weryfikacji]** Udział zapytań z AIO w polskim Google (spotykana liczba: ~24%) pochodzi z blogów agencyjnych, nie z danych Google ani Ahrefs. Nie cytuj bez własnego pomiaru na próbce fraz z branży.

---

## 3. Fleksja polska w keyword research

**Rób: pisz naturalnie, jedną odmianą w tytule i H1; nie wtłaczaj wszystkich przypadków do treści.**
Google: „Nasz zaawansowany system synonimów pozwala znaleźć trafne dokumenty, nawet jeśli nie zawierają dokładnie użytych przez Ciebie słów" oraz „systemy dopasowania językowego Google są dość zaawansowane i rozumieją, jak Twoja strona odnosi się do wielu zapytań, nawet jeśli nie używasz dokładnych terminów".
Źródła [A]: https://www.google.com/search/howsearchworks/how-search-works/ranking-results/ oraz https://developers.google.com/search/docs/fundamentals/seo-starter-guide

**Unikaj: powtarzania odmian tego samego słowa „na zapas".**
Google wprost: „Nadmierne powtarzanie tych samych słów (nawet w wariantach) męczy użytkowników, a keyword stuffing narusza zasady antyspamowe Google".
Źródło [A]: https://developers.google.com/search/docs/fundamentals/seo-starter-guide

**Rób: mimo powyższego rozdzielaj frazy, które różnią się znaczeniem, a nie tylko końcówką.**
Praktyczny podział dla polskiego:
- **Warianty fleksyjne** (`kancelaria podatkowa` / `kancelarii podatkowej` / `kancelarię podatkową`) — jedna strona docelowa, jedna intencja. Nie twórz osobnych podstron.
- **Warianty leksykalne** (`doradca podatkowy` / `doradztwo podatkowe` / `biuro rachunkowe` / `księgowość dla firm`) — inne intencje i inne SERP-y. Sprawdź SERP każdej z osobna zanim zdecydujesz o jednej czy kilku podstronach.
- **Formy pytające** (`jak`, `ile`, `kiedy`, `czy`) — osobny ruch informacyjny, naturalne miejsce w FAQ i blogu. [B]
Źródło [B]: https://semcore.pl/keyword-research/

**Rób: agreguj wolumeny wariantów fleksyjnych ręcznie przed priorytetyzacją.**
Dlaczego: narzędzia raportują odmiany jako osobne rekordy, więc surowy wolumen jednej formy zaniża realny potencjał tematu. Suma odmian bywa wielokrotnością formy mianownikowej. **[do weryfikacji — potwierdź na własnych danych z Senuto/GSC, nie znalazłem badania z liczbami dla PL]**

**Rób: kontroluj kanibalizację, bo w polskim jest łatwiejsza do przypadkowego wywołania.**
Kanibalizacja = kilka URL-i tej samej witryny widocznych na tę samą frazę; skutek to niestabilne pozycje i spadek ruchu. Senuto ma dedykowany raport kanibalizacji.
Źródło [B]: https://wiki.senuto.com/l/pl/analiza-widocznosci/analiza-widocznosci-raport-kanibalizacja-fraz
Dlaczego akurat w PL: bogata odmiana sprawia, że dwa teksty o „rozliczeniu ryczałtu" i „ryczałcie ewidencjonowanym" trafiają w ten sam klaster, choć autorom wydają się różne.

**Rób: URL-e slug bez polskich znaków, w mianowniku, krótkie** (`/doradztwo-podatkowe/`, nie `/doradztwa-podatkowego-dla-firm/`). **[do weryfikacji — to konwencja branżowa, nie wymóg Google; Google nie wymaga słów kluczowych w URL]**

---

## 4. Narzędzia i źródła wiedzy dla rynku PL

### Narzędzia

| Narzędzie | Do czego | Uwagi |
|---|---|---|
| **Senuto** (PL) | analiza widoczności, baza fraz PL, sezonowość, kanibalizacja, klastrowanie semantyczne | baza deklarowana: 14–19 mln fraz PL, codzienne odświeżanie pozycji w polskim Google [B] |
| **Semstorm** (PL) | analiza konkurencji, monitoring reklam tekstowych i PPC, audyt techniczny | [B] |
| **Surfer SEO** (PL, globalny produkt) | optymalizacja on-page treści na podstawie TOP wyników | [B] |
| **Ahrefs / Semrush** | backlinki, dane globalne, benchmarki | słabsze pokrycie długiego ogona PL niż Senuto [B] |
| **Google Search Console** | jedyne dane pierwsze: realne zapytania, CTR, pozycje | [A] |
| **Google Trends** | sezonowość — okno min. 12 mies., najlepiej 5 lat, żeby zobaczyć powtarzalne szczyty | [B] |

Źródła [B]: https://cluegroup.pl/narzedzia/porownanie-narzedzi-senuto-vs-semstorm-vs-surferseo-polskie-starcie-gigantow/ ; https://wiki.senuto.com/l/pl/analiza-widocznosci/analiza-widocznosci-raport-kanibalizacja-fraz ; https://www.senuto.com/pl/blog/jak-sprawdzic-popularnosc-slow-kluczowych/

**[do weryfikacji]** Twierdzenie „Senuto wykrywa średnio o 35% więcej polskich fraz niż Ahrefs dla typowej domeny .pl (testy Q1 2026)" pochodzi z bloga agencyjnego bez metodologii. Nie cytuj; jeśli decyzja o narzędziu jest kosztowna, zrób własne porównanie na mentzen.pl.

**Rób: jako podstawowe narzędzie do fraz PL bierz Senuto, jako źródło prawdy o ruchu — Search Console.**
Dlaczego: wolumeny w każdym narzędziu to estymaty; GSC to jedyne dane o tym, na co realnie wyświetla się mentzen.pl.

### Źródła wiedzy

- **[A]** Google Search Central (dokumentacja + blog): https://developers.google.com/search/docs
- **[A]** Search Quality Rater Guidelines (PDF, aktualizacja 2025-09-11): https://guidelines.raterhub.com/searchqualityevaluatorguidelines.pdf
- **[A]** Ahrefs Blog, Search Engine Land, Search Engine Journal, Moz — badania z danymi
- **[B]** polskie blogi: Senuto, Semstorm, agencje (eActive, widoczni, Semcore, Verseo). Użyteczne do kontekstu rynkowego i nazewnictwa; nie jako źródło liczb.
- Konferencje/branża PL do rozważenia jako źródło aktualności **[do weryfikacji — nie sprawdzałem harmonogramów]**

---

## 5. Konkurencyjność fraz podatkowo-prawnych w PL

**Rób: zakładaj, że ogólnopolskie frazy główne (`kancelaria prawna`, `doradztwo podatkowe`, `biuro rachunkowe`) są poza zasięgiem szybkiego zwrotu.**
Niemal każda kancelaria w dużym mieście inwestuje w widoczność, co podniosło koszt wejścia do TOP 3; pozycjonowanie na ogólną frazę krajową wymaga dużych zasobów i lat pracy. [B]
Źródła [B]: https://prawnymarketing.pl/pozycjonowanie-kancelarii/ ; https://weblymate.com/seo-lokalne-dla-prawnikow-jak-kancelaria-zdobywa-klientow-z-google-i-google-maps/

**Rób: priorytetyzuj trzy koszyki fraz ponad frazami prestiżowymi:**
1. **Specjalizacja + lokalizacja** (`doradca podatkowy Toruń`, `księgowość dla spółek Warszawa`) — mniejszy wolumen, wyższa gotowość do kontaktu.
2. **Frazy problemowe / długi ogon** (`estoński CIT dla spółki z o.o. warunki`, `czy fundacja rodzinna płaci CIT`) — trafiają w realny problem klienta B2B.
3. **Frazy transakcyjne i brandowe** — najwyższa konwersja, najniższe ryzyko AIO.
Źródło [B]: https://prawnymarketing.pl/pozycjonowanie-kancelarii/

**Rób: traktuj każdą stronę o podatkach i prawie jako YMYL i inwestuj w sygnały E-E-A-T.**
Search Quality Rater Guidelines zaliczają bezpieczeństwo finansowe (zarządzanie pieniędzmi, podatki, kredyty, inwestycje) do YMYL; przy YMYL każdy element E-E-A-T staje się krytyczny, a Trust jest fundamentem. E-E-A-T nie jest bezpośrednim czynnikiem rankingowym i Google nie przypisuje „E-E-A-T score".
Źródła [A/B]: https://guidelines.raterhub.com/searchqualityevaluatorguidelines.pdf (2025-09-11) — opis wtórny: https://www.seozoom.com/google-search-quality-rater-guidelines/

Konkretnie dla mentzen.pl:
- podpisuj artykuły imieniem, nazwiskiem i tytułem zawodowym autora (doradca podatkowy nr wpisu, radca prawny, adwokat);
- strony autorów z realnym biogramem, publikacjami, numerem wpisu na listę;
- data publikacji i data aktualizacji na każdym tekście podatkowym (przepisy się zmieniają);
- widoczne dane firmy: KRS, NIP, adres, kontakt — sygnały Trust.

**Rób: dane strukturalne LegalService / AccountingService / Organization + Person dla autorów.** **[do weryfikacji — sprawdź w dokumentacji Google Search Central, które typy mają wsparcie w wynikach wzbogaconych; sam typ schema.org nie gwarantuje rich resulta]**

---

## 6. Sezonowość fraz podatkowych: kalendarz treści

**Rób: publikuj i aktualizuj treści sezonowe 4–8 tygodni przed szczytem zainteresowania.**
Dlaczego: indeksacja i wspinaczka w rankingu trwa; tekst opublikowany 25 kwietnia nie zdąży na szczyt PIT-owy.

Powtarzalne szczyty w polskim kalendarzem podatkowym (do wykorzystania jako szkielet planu treści):

| Okres | Temat | Uwaga |
|---|---|---|
| **styczeń** | zmiany podatkowe na nowy rok, składka zdrowotna, wybór formy opodatkowania | co roku najwyższy pik „zmiany w podatkach [rok]" [do weryfikacji — potwierdź w Google Trends] |
| **do 20 lutego** | zmiana formy opodatkowania na dany rok | [do weryfikacji — termin potwierdź w ustawie/podatki.gov.pl] |
| **15 lutego – 30 kwietnia** | sezon PIT: rozliczenie roczne, ulgi, Twój e-PIT, PIT-36/37/28/38 | okno składania zeznań za rok poprzedni [do weryfikacji — potwierdzić na podatki.gov.pl; fetch strony zwrócił 404/redirect] |
| **do 31 marca** | CIT-8 za rok poprzedni | [do weryfikacji — termin ustawowy „koniec 3. miesiąca po roku podatkowym", nie potwierdziłem w źródle pierwotnym] |
| **czerwiec–lipiec** | sprawozdania finansowe, zatwierdzenie i złożenie do KRS | [do weryfikacji] |
| **25. dzień każdego miesiąca** | JPK_V7 / VAT | plik JPK_V7 składa się do 25. dnia miesiąca po okresie rozliczeniowym [B] — https://www.comarchbetterfly.pl/poradnik-przedsiebiorcy/terminy-podatkowe-2026/ |
| **2026: JPK CIT / JPK_PD** | nowe obowiązki raportowe, terminy przedłużone | termin JPK w podatkach dochodowych przedłużony do końca 7. miesiąca po roku podatkowym (dla 2026: 31 lipca) [B] — https://www.pit.pl/jednolity-plik-kontrolny/termin-skladania-jpk-cit-i-pit-2026-zostanie-przedluzony |
| **listopad–grudzień** | optymalizacja końca roku, decyzje o formie opodatkowania na przyszły rok | [do weryfikacji] |

**WAŻNE: wszystkie terminy z tej tabeli przed użyciem w skillu potwierdź na podatki.gov.pl.** Nie udało mi się pobrać oficjalnych stron (podatki.gov.pl zwracał przekierowania i 404 przez WebFetch). Publikowanie błędnego terminu podatkowego na stronie kancelarii to ryzyko wizerunkowe większe niż utrata pozycji.

**Rób: jedna stała strona-hub per temat sezonowy, aktualizowana co rok — nie nowy URL co rok.**
Dlaczego: strona z historią i linkami zewnętrznymi wraca na pozycje szybciej niż świeży URL. Wyjątek: gdy stan prawny zmienił się na tyle, że stara treść byłaby myląca — wtedy nowa strona + przekierowanie lub jasne oznaczenie archiwum.

**Rób: używaj sezonowości w Senuto / Google Trends z oknem 5 lat, szukając powtarzalnych szczytów w tych samych miesiącach.** [B]
Źródło [B]: https://webmetric.com/wiedza/sezonowosc-slow-kluczowych-jak-ja-wykorzystac-w-dzialaniach-seo/

---

## 7. Wyszukiwanie brandowe: „Mentzen" jako nazwisko publiczne

### Kontekst faktograficzny

Sławomir Mentzen: doradca podatkowy i przedsiębiorca, właściciel Kancelarii Mentzen, prezes Mentzen S.A. (debiut na NewConnect w marcu 2024), jednocześnie jeden z najbardziej rozpoznawalnych polityków Konfederacji i kandydat na prezydenta RP. Kancelaria świadczy doradztwo podatkowe, prawne i księgowość dla MŚP w modelu abonamentowym; przychody 21,5 mln zł w 2023 r. przy ponad 3300 obsługiwanych firmach.
Źródła: https://mycompanypolska.pl/artykul/slawomir-mentzen-rozkreca-biznes-jego-kancelaria-zarobila-ponad-21-mln-zl/14025 ; https://rynekprawniczy.pl/2024/04/08/kancelaria-podatkowo-prawna-konfederackiego-polityka-wchodzi-na-gielde/ ; https://mentzen.pl/

### Dlaczego to ma znaczenie dla SEO

Zapytanie brandowe to każde zapytanie zawierające nazwę firmy lub marki, samodzielnie albo z modyfikatorem (lokalizacja, usługa). Ranking dla zapytań brandowych jest kluczowy dla kontroli nad tym, jak marka jest przedstawiana — a firmy dzielące nazwę z inną encją mają z tym problem, bo wyszukiwarka może priorytetyzować inne znaczenie.
Źródło [A]: https://searchengineland.com/branded-search-seo-452676 (2025-02-27)

To jest dokładnie sytuacja mentzen.pl: nazwa marki i nazwisko polityka to ta sama encja leksykalna, ale dwie różne encje w rozumieniu Google, z dwiema różnymi intencjami użytkownika.

### Szanse

**Rób: eksploatuj wolumen brandowy — to najtańszy i najodporniejszy na AIO ruch, jaki firma ma.**
Wolumen zapytań brandowych to czysty proxy siły marki: im większa świadomość, tym więcej osób szuka nazwy wprost. [B]
Źródło [B]: https://www.airops.com/blog/what-is-branded-search

**Rób: buduj i broń pozycji na kombinacje brand + usługa** (`Mentzen księgowość`, `Mentzen abonament`, `Mentzen cennik`, `Mentzen opinie`, `kancelaria Mentzen kontakt`). Każda z nich powinna mieć dedykowaną, jednoznaczną stronę docelową.
Dlaczego: te frazy mają najwyższą intencję zakupową w całym portfelu i zerową konkurencję poza samą marką.

**Rób: zadbaj o encję w Grafie Wiedzy — spójne NAP (nazwa, adres, telefon) na stronie, w GBP, w KRS, na profilach branżowych; Organization schema z `sameAs` do zweryfikowanych profili.**
Dlaczego: Google rankuje zapytania brandowe m.in. przez rozpoznanie encji i dane strukturalne. [B]
Źródło [B]: https://thatware.co/brand-entity-seo/

**Rób: rozpoznawalność nazwiska jako akcelerator linków i wzmianek.** Wzmianki medialne o osobie przekładają się na wzmianki o marce — pilnuj, żeby publikacje o kancelarii linkowały do mentzen.pl, a nie tylko do profili społecznościowych.

### Ryzyka

**Ryzyko 1: rozjazd intencji.** Zapytanie „Mentzen" ma dominującą intencję polityczno-newsową, nie usługową. SERP na samo nazwisko będzie zajęty przez Wikipedię, portale informacyjne i materiały wyborcze — mentzen.pl nie wygra go w całości i nie powinien tego celem czynić.
**Rób: nie optymalizuj strony głównej pod gołe „Mentzen". Optymalizuj pod `kancelaria Mentzen`, `Mentzen doradztwo podatkowe`, `Mentzen księgowość`.**
Dlaczego: walka o zapytanie o innej intencji marnuje zasoby i psuje dopasowanie strony do zapytania.

**Ryzyko 2: SERP brandowy zajęty przez osoby trzecie.** Search Engine Land wprost: strony trzecie mogą zajmować wyniki brandowe przez nieaktualne treści, wzmianki konkurencji i podsumowania AI zawierające nieścisłości o marce.
Źródło [A]: https://searchengineland.com/branded-search-seo-452676
W przypadku Mentzen to ryzyko jest podwyższone: w SERP-ie brandowym pojawiają się materiały o sporach prawnych i politycznych (pozew PO, przegrana przed NSA w sprawie kosztów), które nie dotyczą usług kancelarii, ale użytkownik na zapytaniu `Mentzen opinie` je zobaczy.
Źródła: https://www.bankier.pl/wiadomosc/PO-sklada-pozew-wobec-Slawomira-Mentzena-W-tle-dezinformacja-8942622.html ; https://www.money.pl/firma/mentzen-sam-sobie-zle-doradzil-w-kwestii-podatkow-wlasnie-przegral-w-sadzie-7318009512069248a.html
**Rób: monitoruj SERP brandowy (co najmniej `Mentzen`, `Mentzen opinie`, `kancelaria Mentzen`) i utrzymuj własne, aktualne strony odpowiadające na te zapytania — opinie klientów, case'y, FAQ, strona „o kancelarii".**
**Unikaj: prób usuwania lub wypierania treści krytycznych metodami SEO wykraczającymi poza publikowanie własnych, prawdziwych materiałów.**

**Ryzyko 3: polaryzacja i wolumen szumu.** Ruch brandowy w okresach kampanii wyborczych rośnie, ale jest to ruch o intencji politycznej, nie zakupowej. Sam prezes wskazywał w liście do akcjonariuszy, że kampania wyborcza wpływa na biznes spółki.
Źródło: https://www.rp.pl/biznes/art41749801-o-slabych-wynikach-i-kampanii-wyborczej-czyli-list-slawomira-mentzena-do-akcjonariuszy
**Rób: w raportowaniu SEO segmentuj ruch brandowy na „usługowy" (brand + modyfikator usługowy) i „newsowy" (gołe nazwisko, brand + nazwisko + temat polityczny), żeby skoki polityczne nie zafałszowały KPI.**
Dlaczego: bez segmentacji wzrost sesji w kampanii wygląda jak sukces SEO, choć nie generuje leadów B2B.

**Ryzyko 4: przeniesienie ryzyka reputacyjnego osoby na markę usługową w kontekście YMYL.** Trust jest fundamentem E-E-A-T, a strony o podatkach są YMYL.
**Rób: opieraj E-E-A-T strony na zespole i tytułach zawodowych, nie wyłącznie na jednym nazwisku.** Dlaczego: dywersyfikuje sygnały wiarygodności i uniezależnia widoczność merytoryczną od cyklu newsowego wokół jednej osoby.

---

## 8. Ograniczenia zawodowe wpływające na treści SEO

**Rób: różnicuj treści według zawodu, który je firmuje.**
- **Adwokaci**: uchwała NRA z 26 maja 2023 r. zniosła całkowity zakaz reklamy kancelarii adwokackich, wprowadzając w zamian ograniczenia co do treści informacji handlowych. [B]
- **Radcowie prawni**: Kodeks Etyki (uchwała 3/2014) dopuszcza „informowanie o wykonywaniu zawodu"; w praktyce pojęcie zrównane z reklamą, ale z zakazem reklamy nachalnej i niezgodnej z godnością zawodu. [B]
- **Zakaz dla obu**: publikowanie informacji o prowadzonych sprawach, obsługiwanych klientach i stawkach za usługi. [B]
- **Doradcy podatkowi**: nie podlegają tym ograniczeniom w takim zakresie jak adwokaci i radcowie. [B]
Źródła [B]: https://www.ibif.pl/blog/strategie-marketingowe/koniec-zakazu-reklamy-adwokackiej-na-czym-polegaja-nowe-zasady ; https://radcaprawny.kirp.pl/aktualnosci/zakaz-reklamy-w-kodeksach-etycznych-radcow-prawnych-i-lekarzy/ ; https://kidp.pl/aktualnosciall.php/10/5940

**[do weryfikacji]** Powyższe streszczenia pochodzą z opracowań wtórnych. Przed wdrożeniem treści typu „nasze sukcesy", „referencje klientów" czy publiczny cennik usług prawnych potwierdź stan w aktualnym Zbiorze Zasad Etyki Adwokackiej / Kodeksie Etyki Radcy Prawnego i skonsultuj z compliance kancelarii.

**Praktyczna konsekwencja dla skilla:** frazy transakcyjne typu `kancelaria prawna cennik` czy strony z case studies są łatwiejsze do obsłużenia po stronie doradztwa podatkowego i księgowości niż po stronie usług adwokackich/radcowskich. Planuj architekturę treści z tym rozróżnieniem.

---

## 9. Lokalne SEO (uzupełniająco)

**Rób: utrzymuj zweryfikowany profil Google Business Profile dla każdej lokalizacji, z poprawną kategorią, pełnym NAP, godzinami, zdjęciami i regularnymi postami; nazwa w GBP musi odpowiadać nazwie z KRS/szyldu.** [B]
Źródła [B]: https://bigbrains.pl/porady-marketingowe/seo-lokalne-biura-rachunkowego-w-google-maps/ ; https://lokalnytop.pl/blog/seo-lokalne-dla-prawnikow-przewodnik-jak-zyskac-klientow-z-wyszukiwarki

**[do weryfikacji]** Liczby krążące w polskich publikacjach („brak w TOP 3 Local Pack = utrata ~44% kliknięć", „GBP to 32% sygnałów rankingowych Local Pack", „7-krotnie większa szansa na Local Pack") pochodzą z blogów agencyjnych bez podanej metodologii. Nie cytuj ich w skillu; jeśli potrzebne, poszukaj oryginału w badaniach Whitespark / BrightLocal.

---

## 10. Lista rzeczy do domknięcia przed napisaniem skilla

1. Potwierdzić wszystkie terminy podatkowe na podatki.gov.pl (WebFetch nie przeszedł — spróbować przez przeglądarkę lub inny URL).
2. Zweryfikować, jakie typy danych strukturalnych dla usług prawnych/księgowych Google faktycznie wspiera w wynikach wzbogaconych (Search Central, dokumentacja typów).
3. Zmierzyć na własnej próbce fraz branżowych, jaki odsetek generuje AI Overviews w polskim Google.
4. Zweryfikować deklarowaną wielkość bazy fraz Senuto (14 vs 19 mln) na stronie producenta.
5. Potwierdzić stan zasad reklamy w aktualnych kodeksach etyki NRA i KIRP (źródła pierwotne, nie blogi).
6. Zrobić inwentaryzację obecnego SERP-u brandowego dla `Mentzen`, `Mentzen opinie`, `kancelaria Mentzen` jako punkt odniesienia.
