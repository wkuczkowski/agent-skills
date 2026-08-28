# Local SEO dla firmy usługowej — co z tego ma sens dla modelu zdalnego z jednym biurem

Research pod skill SEO dla mentzen.pl (kancelaria prawo/podatki/księgowość, B2B, WordPress, rynek polski, YMYL). Data: 2026-08-28. Twierdzenia nieoznaczone `[do weryfikacji]` mają źródło przy sobie.

Stan faktyczny mentzen.pl (rozstrzygnięty — potwierdzony przez użytkownika 2026-08-28): jedna stacjonarna lokalizacja (Amicus Business Park, ul. Grudziądzka 110-114/101, 87-100 Toruń, tel. +48 563 000 363), w której **klienci mogą odbyć konsultację na miejscu**; większość klientów korzysta z usług zdalnie; oddziały Warszawa/Gdańsk/Poznań nie istnieją (wzmianki o nich w zewnętrznych wizytówkach to dane nieaktualne — do sprzątnięcia, sekcja 5). W serwisie zero stron lokalizacyjnych (`/oddzialy/`, `/doradca-podatkowy-warszawa/` itp. zwracają 404), brak schema `LocalBusiness`/`LegalService` (na home tylko JSON-LD Yoast: `Organization`, `WebSite`, `WebPage`, `BreadcrumbList`, `ImageObject`). Źródło: własne mapowanie serwisu + potwierdzenie użytkownika, 2026-08-28.

## TL;DR

1. **Local SEO w klasycznym sensie (local pack w wielu miastach) jest dla mentzen.pl niedostępne z definicji.** Google wprost: odległość jest czynnikiem rankingu lokalnego, nie da się go kupić ani obejść, a profil firmy wymaga realnej lokalizacji z obsługą klientów ([support.google.com/business/answer/7091](https://support.google.com/business/answer/7091), [answer/3038177](https://support.google.com/business/answer/3038177)). Bez biura w Warszawie nie ma local packa w Warszawie. Kropka.
2. **Nie buduj siatki stron "usługa + miasto".** Bez realnej obecności to jednocześnie doorway abuse i scaled content abuse wg polityki spamowej Google ([developers.google.com/search/docs/essentials/spam-policies](https://developers.google.com/search/docs/essentials/spam-policies), stan 2026-08-28). Przy YMYL ryzyko kary jest wyższe niż w zwykłych usługach.
3. To, co z local SEO **realnie ma sens**, mieści się w czterech punktach: (a) jeden dopracowany profil GBP dla Torunia — **w pełni kwalifikowalny, nie graniczny**: biuro realnie przyjmuje klientów na konsultacje stacjonarne (potwierdzone przez użytkownika 2026-08-28), więc warunek „realnej lokalizacji z obsługą klientów" z [answer/3038177](https://support.google.com/business/answer/3038177) jest spełniony wprost, (b) ciągły, zgodny z zasadami proces zbierania opinii Google, (c) jedna porządna strona biura z schema `LegalService`/`AccountingService`, (d) spójny NAP i spójna encja firmy w całej sieci.
4. **Opinie Google przestały być tematem "tylko lokalnym".** AI Mode wyciąga profil GBP z opiniami przy pytaniach porównawczych i reputacyjnych ("która kancelaria podatkowa w Toruniu...") (wg Exposure Ninja, podcast "Dojo", 2025-10-17). To — obok realnych konsultacji stacjonarnych w Toruniu, dla których local pack i opinie są zwykłą ścieżką pozyskania klienta — powód, by kancelaria obsługująca większość klientów zdalnie i tak dbała o GBP.
5. **Czerwone linie, których skill nie może nigdy zaproponować:** miasto lub fraza w nazwie firmy w GBP; rabat/prezent/konkurs za opinię; premie dla pracowników za liczbę opinii; prośba o wymienienie nazwiska pracownika w opinii; filtrowanie proszonych o opinię po przewidywanej ocenie; `aggregateRating` z własnych opinii na własnej stronie; strony miast bez biura. Każda z nich narusza opublikowaną politykę Google (źródła w sekcjach niżej).
6. Opinie w Polsce to też **prawo, nie tylko polityka Google**: dyrektywa Omnibus (od 2023-01-01) wymaga informowania, czy i jak weryfikuje się, że opinie pochodzą od realnych klientów; fałszywe opinie to nieuczciwa praktyka rynkowa z sankcją do 10% obrotu `[do weryfikacji — cytowane źródła nie podają wysokości sankcji]` ([prawo.pl](https://www.prawo.pl/prawo/dyrektywa-omnibus-a-falszywe-opinie-w-sieci,516032.html)).
7. Wg Whitespark (ankieta 47 ekspertów, 2025-11-06) najsilniejsze czynniki local packa to kategoria główna GBP, bliskość punktu wyszukiwania i słowa kluczowe w nazwie firmy w GBP (fizyczny adres w mieście zapytania to #4), czyli rzeczy poza treścią. Dla organiki zlokalizowanej czynnik #1 to dedykowana strona per usługa ([whitespark.ca/local-search-ranking-factors](https://whitespark.ca/local-search-ranking-factors/)). Wniosek dla mentzen.pl: energia idzie w strony usług, nie w strony miast.
8. Frazy "doradca podatkowy [miasto]" spoza Torunia zostaw konkurencji z biurami. Zamiast tego celuj w ogólnopolskie frazy zdalne ("księgowość online", "doradztwo podatkowe online", "biuro rachunkowe dla spółki z o.o.") plus frazy branżowe, gdzie geografia nie gra roli.
9. **Opinie mają drugi reżim obok polityki Google: etykę zawodową doradców podatkowych** (`_notes/uzup-etyka-doradcy.md`). Opinia wystawiona samodzielnie przez klienta w GBP to wypowiedź klienta w cudzym serwisie; gdy kancelaria aktywnie o nią prosi, osadza ją na własnej stronie lub cytuje w reklamie, staje się przekazem doradcy [częściowo interpretacja]. Wtedy: uprzednia zgoda klienta na imienne użycie (tajemnica co do faktu współpracy — ZE art. 6 ust. 1, u.d.p. art. 37 ust. 1), bez kwot i obietnic skuteczności (ZE art. 9b ust. 2 lit. b), bez sugestii znajomości w urzędach (lit. c), bez selekcji samych pochwał (ZE art. 9 lit. a; u.p.n.p.r. art. 7 pkt 26). Szczegóły w sekcji 3.

## 1. Werdykt: ile local SEO wchodzi do skilla

Ranking lokalny stoi na trzech czynnikach: trafność, odległość, rozpoznawalność ([support.google.com/business/answer/7091](https://support.google.com/business/answer/7091)). Odległość jest nieoptymalizowalna inaczej niż fizycznym biurem, a wytyczne GBP wykluczają wirtualne biura i adresy bez własnego personelu ([support.google.com/business/answer/3038177](https://support.google.com/business/answer/3038177): wirtualne biuro "isn't eligible for a Business Profile"; coworking tylko z oznakowaniem, przyjmowaniem klientów i własnym personelem w godzinach otwarcia). W branży prawniczej egzekwowanie jest zaostrzone, wirtualne adresy kancelarii bywają usuwane z Map ([PaperStreet](https://www.paperstreet.com/blog/virtual-offices-and-google-these-two-dont-mix/) `[do weryfikacji co do skali]`).

Dlatego skill SEO dla mentzen.pl powinien traktować local SEO jako **wąski, domknięty moduł "Toruń + reputacja"**, a nie jako oś strategii:

- **Rób:** GBP dla biura w Toruniu, opinie, strona biura, schema, spójność NAP/encji. To całość.
- **Unikaj:** wszystkiego, co udaje obecność w miastach, gdzie biura nie ma: profili GBP pod cudzymi/wirtualnymi adresami, stron miast, "obszaru obsługi: cała Polska" (wytyczna Google dla firm typu SAB: obszar obsługi nie powinien przekraczać ok. 2 h jazdy od bazy, [answer/3038177](https://support.google.com/business/answer/3038177)).
- Popyt lokalny z innych miast obsługuj **ogólnopolskimi stronami usług** i pozycjonowaniem modelu zdalnego. Wg Sama Dunninga (kanał Breaking B2B, notatka video-N1m9hlMsMKg): wąskie frazy branżowe ("biuro rachunkowe dla spółki z o.o.", "księgowość dla IT") rankują szybciej i konwertują lepiej niż czołowe frazy "usługa + duże miasto"; miasto to tylko jeden z możliwych modyfikatorów macierzy fraz i dla firmy zdalnej najsłabszy.

Wyjątek do rozważenia w przyszłości: jeśli firma kiedyś otworzy drugie biuro, cały playbook wielolokalizacyjny (osobny profil per biuro, osobna strona per biuro, lokalny numer, opinie przypisane do lokalizacji) wchodzi do gry. Do tego czasu skill nie powinien go proponować.

## 2. Profil GBP dla Torunia — jedyny, ale kompletny

Wytyczne twarde (złamanie = ryzyko zawieszenia profilu), wszystkie z [Guidelines for representing your business on Google](https://support.google.com/business/answer/3038177):

- **Nazwa = realna nazwa z szyldu i dokumentów.** Zakaz dopisków typu miasto, usługa, slogan. Uwaga na rozjazd źródeł: Whitespark podaje słowa kluczowe w nazwie GBP jako czynnik #3 local packa, i to jest prawda empiryczna, ale jednocześnie jawne naruszenie wytycznych i częsta przyczyna zawieszeń. Skill ma tego nigdy nie sugerować. Ten sam konflikt dotyczy rady Nathana Gotcha (kanał Nathan Gotch, film "Free Local SEO Course: My Full 2026 System") o rejestrowaniu domeny i nazwy "[miasto][branża]": w Polsce dochodzą regulacje korporacyjne nazw kancelarii, nie przenosić.
- **Kategoria główna po researchu konkurencji, nie z sufitu.** Sprawdź kategorie trzech firm z topu local packa dla "doradca podatkowy Toruń", "biuro rachunkowe Toruń", "kancelaria prawna Toruń" i ustaw tę, która realnie opisuje działalność. Wg Gotcha to najtańsza pojedyncza dźwignia w całym local SEO, a kategoria główna to czynnik #1 wg Whitespark. Kategoria dopełnia zdanie "ta firma JEST", nie "ma": *Doradca podatkowy* / *Kancelaria prawna* / *Biuro rachunkowe*. 2-3 kategorie dodatkowe, żadnych kategorii jako słów kluczowych.
- **Telefon pod bezpośrednią kontrolą firmy** (bez call center), URL profilu prowadzący do strony biura/kontaktu, nie do home. Pinezka na wejściu do budynku, godziny realne plus świąteczne (profil "zamknięty w momencie wyszukiwania" traci widoczność; czynnik #5 wg Whitespark).
- **Zdjęcia własne:** budynek, wejście, wnętrze, zespół. Nie stock.
- **Profile praktyków** (konkretni doradcy) są dozwolone tylko dla osób publicznie przyjmujących klientów pod tym adresem, z bezpośrednim kontaktem. Skoro konsultacje stacjonarne w Toruniu realnie się odbywają, taki profil jest formalnie możliwy dla doradcy, który faktycznie przyjmuje tam klientów — ale musi mieć wyróżnik (własny numer), inaczej Google go scali z profilem firmy ([Market My Market](https://www.marketmymarket.com/legal-marketing/law-firm-google-business-profile-optimization/) `[do weryfikacji]`). Domyślnie: jeden profil firmowy wystarczy.
- Jeden profil na lokalizację, nigdy więcej.

Kwalifikowalność profilu jest bezsporna: biuro w Toruniu realnie przyjmuje klientów na konsultacje stacjonarne (potwierdzone przez użytkownika 2026-08-28), więc to nie jest przypadek graniczny adresu wirtualnego ani czystego SAB. Wartość profilu wykracza przy tym poza klientów lokalnych: profil GBP z opiniami jest zaciągany przez AI Mode przy pytaniach porównawczych i reputacyjnych o firmę (wg Exposure Ninja; obserwacja branżowa, nie dokumentacja Google `[do weryfikacji]`). GBP to też encja-kotwica: potwierdza w danych Google, że firma istnieje, gdzie i pod jaką nazwą.

## 3. Opinie — proces, polityka Google, prawo polskie

Największy pojedynczy zasób local SEO dostępny dla firmy zdalnej. Whitespark 2026 raportuje rosnącą wagę sygnałów opinii; wysokie oceny w Google (#6) i liczba natywnych opinii Google (#9) są w top 10 local packa, świeżość opinii jest wskazywana jako czynnik rosnący, ale poza tą dziesiątką ([whitespark.ca](https://whitespark.ca/local-search-ranking-factors/), 2025-11-06). W polskim B2B prawno-podatkowym czołówka lokalna ma zwykle kilkadziesiąt opinii, nie tysiące, więc dogonienie lidera jest osiągalne.

### Co robić

- Prośba o opinię jako **standardowy krok po zamknięciu sprawy** (trigger w systemie kancelaryjnym / po rozliczeniu rocznym), mail lub SMS z bezpośrednim linkiem do formularza opinii. Wg Gotcha: sposób na więcej opinii to częstsze proszenie, plus cotygodniowy follow-up przypisany do konkretnej osoby. Lepszy stały strumień 2-4 opinii miesięcznie niż jednorazowa kampania.
- Cel liczbowy ustawiaj na **lidera** lokalnego packa, nie na średnią z top 3 (Gotch).
- **Odpowiadaj na każdą opinię**, także pozytywną; Google wprost łączy "helpful replies" z wyróżnieniem profilu ([answer/7091](https://support.google.com/business/answer/7091)). W odpowiedzi nigdy nie potwierdzaj ani nie zaprzeczaj, że osoba była klientem, i nie ujawniaj niczego o sprawie — to nie jest już kwestia otwarta: sam fakt współpracy jest objęty tajemnicą zawodową (ZE art. 6 ust. 1; u.d.p. art. 37 ust. 1 — katalog wyłączeń zamknięty, bez przesłanki zgody), a jej naruszenie grozi odpowiedzialnością dyscyplinarną i karną (art. 266 § 1 k.k.); szczegóły: `_notes/uzup-etyka-doradcy.md`, sekcje 4.2 i 5. Tajemnica zawodowa jest nadrzędna wobec SEO. Fałszywe opinie zgłaszaj przez mechanizm Google zamiast polemiki.

### Czego nie robić — polityka Google

Z [Maps User Generated Content Policy](https://support.google.com/contributionpolicy/answer/7400114):

- Zakaz opinii opłaconych i **jakichkolwiek zachęt** ("payment, discounts, free goods and/or services"), także za usunięcie negatywnej opinii.
- Zakaz **kwot i premii dla pracowników** za pozyskane opinie ("Merchants requesting that staff solicit a certain number of reviews").
- Zakaz sterowania treścią, w tym proszenia o **wymienienie imienia pracownika** w opinii ("solicit reviews that include specific content, including content that identifies a staff member").
- Zakaz opinii z kont pracowników i rodziny (fake engagement, podszywanie się).
- Review gating (kierowanie do Google tylko zadowolonych) jest zakazany wprost w oficjalnej polityce, w sekcji o manipulowaniu ocenami: "Merchants should not... Discourage or prohibit negative reviews, or selectively solicit positive reviews from customers" ([support.google.com/contributionpolicy/answer/7400114](https://support.google.com/contributionpolicy/answer/7400114), sprawdzone 2026-08-28). Nie filtruj proszonych po przewidywanej ocenie.

**Rozbieżność źródeł do zapamiętania przez skill:** Gotch rekomenduje premie i wewnętrzną rywalizację dla pracowników za opinie oraz prośbę o imię obsługującego w treści. Obie rady wprost łamią cytowaną politykę Google. Tu wygrywa dokument Google, nie praktyk.

### Warstwa etyki zawodowej doradców podatkowych

Źródło: `_notes/uzup-etyka-doradcy.md` (teksty pierwotne: Zasady etyki doradców podatkowych — uchwała KRDP 40/2026, u.d.p.; 2026-08-28). Rozróżnienie nośne: opinia wystawiona przez klienta **samodzielnie** w GBP to wypowiedź klienta w serwisie osoby trzeciej — doradca jej nie „przekazuje". Gdy kancelaria (a) aktywnie o opinię prosi, (b) osadza ją na własnej stronie lub w reklamie, (c) publikuje jej ocenę w schema — staje się ona informacją przekazywaną przez doradcę i obowiązują ograniczenia ZE [częściowo interpretacja — brak przepisu wprost o platformach opinii]. Konsekwencje dla procesu opinii:

- **Prośba o opinię**: dopuszczalna jako indywidualna, neutralna prośba bez zachęt i bez sugerowania treści — czyli dokładnie w granicach, których i tak wymaga polityka Google. `[do weryfikacji]` Notatka etyczna zostawia otwarte, czy aktywne zachęcanie do wystawiania opinii w GBP mieści się w „przekazywaniu wykazu klientów" (ZE art. 9b ust. 2 lit. d) — do rozstrzygnięcia trzymać się formuły prośby indywidualnej po zamknięciu sprawy.
- **Imienne użycie opinii poza GBP** (osadzenie na mentzen.pl, cytat w reklamie): wymaga **uprzedniej zgody danego klienta**, klient po kliencie — fakt współpracy jest objęty tajemnicą zawodową (ZE art. 6 ust. 1; u.d.p. art. 37 ust. 1), a ZE art. 9b ust. 2 lit. d stosuje się przez analogię. Zgoda pisemna (zalecenie dowodowe), wąska, odwoływalna; zgoda zbiorcza w regulaminie nie wystarcza. Opinia anonimowa („klient z branży e-commerce") — dopuszczalna, jeśli klient nie jest identyfikowalny pośrednio.
- **Treść cytowanych opinii**: bez kwot i deklaracji wyniku („odzyskali dla nas 300 tys.") — cytat użyty promocyjnie jest przekazem doradcy i narusza zakaz obiecywania skuteczności (ZE art. 9b ust. 2 lit. b); bez sugestii osobistych relacji z organami (lit. c), nawet jeśli napisał to klient.
- **Dobór opinii do publikacji**: selekcja wyłącznie pochwał przy ukrywaniu reszty gryzie się z wymogiem obiektywizmu i wyważenia (ZE art. 9 lit. a) i ryzykuje kwalifikację jako zniekształcanie opinii konsumentów (u.p.n.p.r. art. 7 pkt 26).
- **Opinie kupione lub pisane przez zespół**: zakazane podwójnie — polityka Google oraz u.p.n.p.r. art. 7 pkt 25–26 i ZE art. 9b ust. 1 i 3; odpowiedzialność obejmuje też agencję prowadzącą profil na rzecz kancelarii („reklama prowadzona na jego rzecz").
- **Obowiązek aktywny**: ZE art. 9b ust. 4 każe podjąć czynności, gdy osoba trzecia rozpowszechnia informacje o kancelarii z naruszeniem zasad — podstawa do stałego monitoringu wizytówek i katalogów (spina się z porządkowaniem nieaktualnych wpisów o nieistniejących oddziałach, sekcja 5).

### Warstwa prawna (Polska)

Dyrektywa Omnibus (w PL od 2023-01-01): obowiązek weryfikacji, czy opinie pochodzą od realnych klientów, i poinformowania, czy i jak ta weryfikacja działa; zlecanie fałszywych opinii to nieuczciwa praktyka rynkowa; sankcje do 10% obrotu plus do 2 mln zł dla zarządzającego `[do weryfikacji — obu liczb nie ma w cytowanych niżej źródłach; prawo.pl to tekst z 2022 sprzed wejścia przepisów w życie]` ([prawo.pl](https://www.prawo.pl/prawo/dyrektywa-omnibus-a-falszywe-opinie-w-sieci,516032.html), [poradnikprzedsiebiorcy.pl](https://poradnikprzedsiebiorcy.pl/-dyrektywa-omnibus-a-publikacja-opinii-o-produktach-i-uslugach)). Przepis chroni konsumentów; zakres przy kliencie czysto B2B jest ograniczony, ale kancelaria obsługuje też JDG, więc bezpiecznie zakładać, że obowiązuje `[do weryfikacji prawnej, wewnętrznie]`. Minimum przy publikowaniu opinii na mentzen.pl (sekcje "Mówili o nas" na stronach usług): opisz, czy i jak weryfikujecie autentyczność, nie usuwaj selektywnie negatywnych, nie przedstawiaj ocen z Google jako własnego systemu.

### Schema a opinie — twarda zasada

Strona używająca `LocalBusiness`/`Organization` **nie kwalifikuje się do gwiazdek** za opinie o samej sobie: "If the entity that's being reviewed controls the reviews about itself... ineligible for star review feature". Dodatkowo zakaz agregowania ocen z innych serwisów ([Review snippet structured data](https://developers.google.com/search/docs/appearance/structured-data/review-snippet), 2026-07-24). Czyli: opinii klientów na mentzen.pl **nie oznaczać** przez `aggregateRating`/`review`. Publikować jako zwykłą treść; markup to ryzyko manual action przy zerowym zysku.

## 4. Strona biura i schema

mentzen.pl nie ma dziś ani strony biura (jest tylko `/dane-kontaktowe/`), ani żadnego markupu lokalnego. Docelowo jedna strona biura w Toruniu, zbudowana wg kryteriów dobrej strony lokalizacji (Miriam Ellis, [Search Engine Land](https://searchengineland.com/guide/service-area-pages), 2025-11-27; Dan Taylor, [SEJ](https://www.searchenginejournal.com/local-seo-multiple-locations/370704/), 2026-06-18):

- Pełny NAP, mapa, dojazd, parking; realne zdjęcia biura i zespołu; sylwetki doradców z uprawnieniami (jednocześnie sygnał E-E-A-T pod YMYL); usługi realnie dostępne na miejscu; FAQ lokalne (właściwość US, sąd rejestrowy); jasne CTA. To ma być cel końcowy użytkownika, nie przelotka do formularza.
- Link do niej z nawigacji lub stopki, w sitemapie, i **z profilu GBP bezpośrednio na nią**, nie na home (Ellis).

Schema na tej stronie ([Local business structured data](https://developers.google.com/search/docs/appearance/structured-data/local-business), 2025-12-10):

- Podtyp zamiast generycznego `LocalBusiness`: `LegalService` dla kancelarii, `AccountingService` dla księgowości; przy jednym podmiocie łączącym oba `ProfessionalService` z zagnieżdżonym `department` (nazwa działu = nazwa firmy + dział).
- Wymagane: `name`, pełny `PostalAddress`. Rekomendowane: `geo` (min. 5 miejsc po przecinku), `telephone` z kierunkowym, `url` strony biura, `openingHoursSpecification` (`hh:mm:ss`), `priceRange` (max 100 znaków).
- `Organization` zostaje na home; `sameAs` do GBP, LinkedIn, profili KIDP/izb. Walidacja w Rich Results Test.
- `areaServed` istnieje w schema.org, ale nie występuje w dokumentacji rich resultu Google; można użyć informacyjnie, bez oczekiwań.

Uwaga kalibrująca: wg Samo Cerara z kursu AEO Ahrefs (kanał Ahrefs) brak potwierdzonych danych, że schema poprawia cytowalność w AI; dodać jako dobry nawyk, nie inwestować w to czasu kosztem treści i opinii.

## 5. NAP i spójność encji

Zasada: jeden kanoniczny zapis nazwy, adresu i telefonu, identyczny wszędzie (GBP, stopka, strona biura, schema, katalogi, social, Bing Places, Apple Business Connect). Utrzymuj dokument źródłowy z kanonicznym NAP + NIP/KRS i aktualizuj wszystkie miejsca naraz przy każdej zmianie. Konkretne zadanie na start: zewnętrzne wizytówki wymieniające nieistniejące oddziały (Warszawa/Gdańsk — np. wpis na trojmiasto.pl) zawierają dane sprzeczne ze stanem faktycznym (jedyna lokalizacja: Toruń, potwierdzone przez użytkownika 2026-08-28) — wystąpić o korektę lub usunięcie; wspiera to także obowiązek reagowania na treści osób trzecich z ZE art. 9b ust. 4 (sekcja 3). Krążące liczby typu "53% wyższe pozycje dzięki spójnemu NAP" nie mają pierwotnego badania, nie używać.

Dla firmy zdalnej ten punkt awansował z higieny na realną dźwignię, bo karmi systemy AI:

- Whitespark 2026 dodał kategorię "AI Search Visibility"; wśród najważniejszych jej czynników są sygnały cytowań i encji ([whitespark.ca](https://whitespark.ca/local-search-ranking-factors/)).
- Eksperyment Ahrefs z fikcyjną marką: Gemini i Perplexity powtarzały rozsiane nieprawdy w 37-39% odpowiedzi; ChatGPT poniżej 7%, bo w 84% cytował oficjalne FAQ marki (wg Cerara, kurs AEO Ahrefs). Wniosek: publikuj konkretne, oficjalne dane o firmie (zespół, numery wpisów na listy zawodowe, adres, model rozliczeń) w formie łatwej do wyciągnięcia, bo model wybierze cudzy konkret zamiast twojego ogólnika.
- Wg autora filmu "12 Brand Authority Signals That Make AI Recommend You": identyczny dwu-trzyzdaniowy opis firmy (specjalizacja, adres, NIP) skopiowany na wszystkie profile to najtańsza pozycja z całej listy sygnałów autorytetu i najczęściej zaniedbana.

Katalogi po polsku: priorytet mają wpisy branżowe i regionalne (lista KIDP, izby adwokackie/radcowskie, izby gospodarcze, lokalne media biznesowe) nad masowymi katalogami (Panorama Firm, pkt.pl, Aleo), które dziś dają głównie spójność, nie linki `[do weryfikacji wartości poszczególnych katalogów]`. Nie ma polskiego odpowiednika G2 dla usług prawnych; rolę platform recenzyjnych przejmują rejestry izb i rankingi (Rzeczpospolita, Legal 500), więc obecność tam liczy się podwójnie: reputacja + źródło dla AI (wg "12 Brand Authority Signals").

## 6. Czego nie robić: strony miast i doorway pages

Definicje z [polityki spamowej Google](https://developers.google.com/search/docs/essentials/spam-policies) (stan 2026-08-28): doorway abuse to strony tworzone pod podobne zapytania, prowadzące użytkownika przez pośrednika zamiast do celu; scaled content abuse to masowe generowanie stron pod rankingi, w tym narzędziami AI. Siatka "doradztwo podatkowe + [miasto]" z podmienioną nazwą miasta łapie się na obie polityki naraz.

Test decyzyjny przed utworzeniem jakiejkolwiek strony lokalnej (za Ellis i Taylorem), wszystkie trzy warunki łącznie: realna obecność w mieście; strona jest celem końcowym (da się z niej załatwić sprawę); jest materiał na unikalną treść (zespół, realizacje, specyfika). mentzen.pl spełnia to wyłącznie dla Torunia.

Gotch w swoim systemie wybiera "agresywną" strukturę URL z miastem w każdym adresie usługowym i uczciwie nazywa to kompromisem. Dla kancelarii zdalnej z jednym biurem ta rada się nie przenosi: strony pod miasta bez fizycznej obecności to podręcznikowe doorway pages, a przy YMYL ryzyko jest większe niż w usługach konsumenckich. Kolejność jest odwrotna i zgodna z Whitespark (strona per usługa = czynnik #1 organiki lokalnej): najpierw kompletna warstwa mocnych stron usługowych bez miasta, potem jedna strona biura. Ewentualną pojedynczą stronę usługa×miasto tworzyć tylko przy potwierdzonym wolumenie i realnej lokalnej specyfice, czyli w praktyce najwyżej dla Torunia.

## 7. Local pack, YMYL i AI — kontekst pomiarowy

- Local pack fraz prawno-podatkowych jest silnie zależny od punktu wyszukiwania; widoczność mierzy się siatką geograficzną (grid rank tracking), nie jedną pozycją. Dla mentzen.pl dotyczy to tylko Torunia.
- Frazy usług prawno-podatkowych to YMYL: sygnały E-E-A-T (imienny autor z numerem wpisu, data stanu prawnego, cytowanie źródeł prawa) ważą więcej niż w innych branżach `[do weryfikacji w aktualnych Search Quality Rater Guidelines]`.
- Local Services Ads / Google Screened (weryfikacja licencji prawników) niedostępne w Polsce: lista krajów Google obejmuje USA, Kanadę, Wielką Brytanię, Irlandię, Niemcy, Austrię, Szwajcarię, Francję, Belgię, Włochy i Hiszpanię, bez Polski ([support.google.com/localservices/answer/6224841](https://support.google.com/localservices/answer/6224841), sprawdzone 2026-08-28).
- Źródła wtórne raportują zastępowanie local packa krótszymi listami generowanymi przez AI w części zapytań ([OnPurpose Media](https://onpurposemedia.com/ai-overview-local-packs-impacting-visibility/)) `[do weryfikacji, brak potwierdzenia Google]`. Kierunek jest spójny z resztą: reputacja i encja > pozycja w packu.
- Lokalny YouTube ("[temat podatkowy] [miasto]") stoi pusty wg Gotcha; dla firmy zdalnej sensowniejszy jest wariant bez miasta, ogólnopolski, bo taka jest jej strefa sprzedaży.

## 8. Zastosowanie dla mentzen.pl — lista dla skilla

Priorytety w kolejności wykonania:

1. **GBP Toruń:** zweryfikować istnienie i stan profilu; kategoria główna po analizie top 3 local packa w Toruniu; nazwa bez dopisków; URL profilu → strona biura (po jej powstaniu); godziny + świąteczne; 10+ własnych zdjęć; usługi i opis wypełnione.
2. **Proces opinii:** trigger po zamknięciu sprawy/rozliczeniu rocznym, link bezpośredni, zero zachęt, zero kwot pracowniczych, zero sugerowania treści; odpowiedź na każdą opinię w 48 h bez potwierdzania statusu klienta i bez ujawniania czegokolwiek o sprawie (tajemnica zawodowa: ZE art. 6 ust. 1, u.d.p. art. 37 ust. 1); każde imienne użycie opinii poza GBP tylko za uprzednią zgodą tego klienta, bez cytatów z kwotami/wynikami (ZE art. 9b ust. 2 lit. b); nota o weryfikacji autentyczności opinii przy sekcjach "Mówili o nas" (Omnibus).
3. **Strona biura Toruń** (rozbudowa `/dane-kontaktowe/` albo nowy URL): NAP, dojazd, zdjęcia, zespół z uprawnieniami, FAQ lokalne, CTA; schema `LegalService`/`AccountingService` (lub `ProfessionalService` + `department`), walidacja; `sameAs` do GBP i profili branżowych. Bez `aggregateRating`.
4. **Encja:** kanoniczny dokument NAP + jeden opis firmy skopiowany na wszystkie profile (LinkedIn, KIDP, izby, katalogi); Bing Places i Apple Business Connect; uzupełnić luki informacyjne o firmie oficjalnym konkretem (skład zespołu, numery wpisów, model rozliczeń), bo to z tego czerpią modele AI.
5. **Frazy lokalne:** w keyword researchu traktować "usługa + miasto ≠ Toruń" jako frazy poza zasięgiem local packa; obsługiwać intencję lokalną frazami zdalnymi ("księgowość online", "biuro rachunkowe dla spółki z o.o.") i frazami branżowymi bez geografii. "Doradca podatkowy Toruń" i pochodne: tak, przez GBP + stronę biura.

Zakazy do zaszycia w skillu (skill ma je odrzucać, nawet jeśli użytkownik poprosi):

- strony/landingi miast bez biura, macierz usługa×miasto, miasto w slugach usług
- drugi profil GBP, profil pod adresem wirtualnym/coworkiem bez personelu, obszar obsługi "cała Polska"
- słowa kluczowe lub "Toruń" w nazwie w GBP
- jakiekolwiek zachęty, premie i sterowanie treścią opinii; review gating
- `aggregateRating`/`review` markup dla opinii o samej kancelarii; przepisywanie ocen z Google na stronę
- imienna opinia lub logotyp klienta na stronie/w reklamie bez uprzedniej zgody tego klienta (ZE art. 6 ust. 1; ZE art. 9b ust. 2 lit. d)
- cytowanie w materiałach kancelarii opinii z kwotami, deklaracjami wyniku lub sugestią znajomości w urzędach (ZE art. 9b ust. 2 lit. b–c)
- potwierdzanie lub zaprzeczanie w odpowiedziach na opinie, że autor jest klientem (tajemnica zawodowa: ZE art. 6 ust. 1, u.d.p. art. 37 ust. 1)

## 9. Luki do domknięcia

- Aktualne Search Quality Rater Guidelines: rozdział YMYL/E-E-A-T pod usługi prawno-podatkowe
- Oficjalne stanowisko Google ws. numerów call tracking w GBP (review gating: domknięte — polityka zakazuje wprost, sekcja 3)
- Wysokość sankcji z ustawy wdrażającej Omnibus (10% obrotu / 2 mln zł) — potwierdzić w tekście ustawy, nie w artykułach branżowych
- Zakres obowiązków z Omnibus przy kliencie B2B — do ustalenia wewnętrznie, nie researchem SEO. Ograniczenia etyki zawodowej: domknięte dla doradców podatkowych w `_notes/uzup-etyka-doradcy.md` (wplecione w sekcje 3 i 8; dla radców i adwokatów osobno: `_notes/uzup-etyka-prawnicy.md`); otwarta pozostaje kwestia, czy aktywne zachęcanie do opinii w GBP mieści się w „przekazywaniu wykazu klientów" (ZE art. 9b ust. 2 lit. d) `[do weryfikacji]`
- Faktyczny stan profilu GBP kancelarii (nie badałem profilu, tylko serwis)

## Źródła

Dokumentacja Google:
- [Improve your local ranking on Google](https://support.google.com/business/answer/7091)
- [Guidelines for representing your business on Google](https://support.google.com/business/answer/3038177)
- [Maps UGC Policy — Prohibited & restricted content](https://support.google.com/contributionpolicy/answer/7400114)
- [Search Essentials — Spam policies](https://developers.google.com/search/docs/essentials/spam-policies) (2026-08-28)
- [Local business structured data](https://developers.google.com/search/docs/appearance/structured-data/local-business) (2025-12-10)
- [Review snippet structured data](https://developers.google.com/search/docs/appearance/structured-data/review-snippet) (2026-07-24)

Branżowe:
- [Whitespark — Local Search Ranking Factors 2026](https://whitespark.ca/local-search-ranking-factors/) (2025-11-06; ankieta ekspercka, nie eksperyment)
- [Search Engine Land — Service area pages](https://searchengineland.com/guide/service-area-pages), Miriam Ellis (2025-11-27)
- [SEJ — Local SEO For Multiple Locations](https://www.searchenginejournal.com/local-seo-multiple-locations/370704/), Dan Taylor (2026-06-18)

Prawo polskie:
- [Prawo.pl — Dyrektywa omnibus a fałszywe opinie](https://www.prawo.pl/prawo/dyrektywa-omnibus-a-falszywe-opinie-w-sieci,516032.html)
- [Poradnik Przedsiębiorcy — Omnibus a publikacja opinii](https://poradnikprzedsiebiorcy.pl/-dyrektywa-omnibus-a-publikacja-opinii-o-produktach-i-uslugach)

Etyka zawodowa:
- `_notes/uzup-etyka-doradcy.md` — Zasady etyki doradców podatkowych (uchwała KRDP 40/2026) i u.d.p., teksty pierwotne, 2026-08-28; stamtąd wszystkie cytowania ZE/u.d.p. w sekcjach 3, 5, 8 i 9

Stan faktyczny lokalizacji: potwierdzenie użytkownika, 2026-08-28 (jedno biuro Toruń, konsultacje stacjonarne możliwe, większość klientów zdalnie, brak oddziałów).

Wideo (opinie praktyków, przypisane w tekście):
- Nathan Gotch, "Free Local SEO Course: My Full 2026 System", kanał Nathan Gotch — https://www.youtube.com/watch?v=bfBwk2KK9jc
- Exposure Ninja, strategia SEO 2026 (AI Mode a GBP) — https://www.youtube.com/watch?v=vrGLaJOAKas
- Samo Cerar, kurs AEO Ahrefs (schema a AI, eksperyment z dezinformacją) — https://www.youtube.com/watch?v=uza9GX0E2mw
- "12 Brand Authority Signals That Make AI Recommend You" (spójność encji, platformy recenzyjne) — https://www.youtube.com/watch?v=VOb_QjlrgpE
- Sam Dunning, "I Built a Full B2B SEO Strategy in 11 Minutes (LIVE)", kanał Breaking B2B (macierz money keywords, miasto jako modyfikator) — https://www.youtube.com/watch?v=N1m9hlMsMKg

Niższe zaufanie, oznaczone `[do weryfikacji]` w tekście: [PaperStreet](https://www.paperstreet.com/blog/virtual-offices-and-google-these-two-dont-mix/), [Market My Market](https://www.marketmymarket.com/legal-marketing/law-firm-google-business-profile-optimization/), [OnPurpose Media](https://onpurposemedia.com/ai-overview-local-packs-impacting-visibility/).
