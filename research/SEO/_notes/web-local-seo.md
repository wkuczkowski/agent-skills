# Local SEO dla firmy usługowej z biurami w kilku miastach

Notatki robocze pod skill SEO dla mentzen.pl (kancelaria: prawo, doradztwo podatkowe, księgowość; B2B; Polska, Google.pl).
Data zebrania: 2026-08-28. Styl: normatywnie — "rób X / unikaj Y" + krótkie uzasadnienie.

Oznaczenie `[do weryfikacji]` = twierdzenie, którego nie potwierdziłem w źródle wysokozaufanym.

---

## 1. Jak Google opisuje ranking lokalny

Google podaje trzy czynniki rankingu lokalnego:

- **Trafność (relevance)** — "how well a Business Profile matches what someone is searching for"
- **Odległość (distance)** — "how far each business is from the customer who's searching"
- **Rozpoznawalność (prominence)** — "how well-known a business is"

Źródło: [Google Business Profile Help — Improve your local ranking on Google](https://support.google.com/business/answer/7091)

Wnioski normatywne z tej samej strony:

- **Uzupełnij profil do końca** (pełny adres, godziny, kategoria, atrybuty). Google: kompletne dane pomagają dopasować profil do zapytań.
- **Odpowiadaj na opinie.** Google: "positive reviews and helpful replies can help your business stand out".
- **Nie kupuj pozycji.** Google wprost: "There's no way to request or pay for a better local ranking on Google". Odrzucaj oferty "gwarantowanego miejsca w local packu".
- Odległość jest czynnikiem, na który nie masz wpływu poza fizyczną lokalizacją biura. Dlatego **nie da się zająć local packu w mieście, w którym nie masz realnego biura** — to determinuje całą strategię wielolokalizacyjną.

### Wagi czynników wg badania branżowego

Whitespark, *Local Search Ranking Factors 2026* (opublikowane 2025-11-06, ankieta wśród 47 specjalistów local SEO):

Top 10 dla **Local Pack / Maps**:
1. Główna kategoria GBP
2. Bliskość punktu wyszukiwania (proximity)
3. Słowa kluczowe w nazwie firmy w GBP
4. Fizyczny adres w mieście z zapytania
5. Firma otwarta w momencie wyszukiwania
6. Wysokie oceny liczbowe w Google
7. Adres wyświetlany w GBP
8. Kategorie dodatkowe GBP
9. Liczba natywnych opinii Google
10. Poprawne ustawienie pinezki na mapie

Top 10 dla **wyników organicznych zlokalizowanych**:
1. Dedykowana strona per usługa
2. Trafność geograficzna słów kluczowych
3. Jakość/autorytet linków przychodzących
4. Słowa kluczowe w title landing page'a
5. Linki z domen branżowych
6. Struktura linkowania wewnętrznego
7. Trafność tematyczna słów kluczowych
8. Słowa kluczowe w nagłówkach landing page'a
9. Wąska specjalizacja tematyczna serwisu
10. Słowa kluczowe w anchor tekstach linków przychodzących

Whitespark w edycji 2026: "the biggest changes we're seeing are an increased importance of review signals and behavioural signals"; po raz pierwszy dodano kategorię widoczności w AI Search oraz sygnały społecznościowe.

Źródło: [Whitespark — Local Search Ranking Factors 2026](https://whitespark.ca/local-search-ranking-factors/), 2025-11-06

**Uwaga metodologiczna:** to badanie ankietowe (opinie ekspertów), nie eksperyment. Traktuj jako priorytetyzację pracy, nie jako dowód przyczynowości. Konkretne procenty wag krążące po blogach wtórnych (np. "GBP 32%, opinie 20%, on-page 15%") pochodzą z wykresu w raporcie i nie udało mi się ich odczytać bezpośrednio ze źródła — `[do weryfikacji]`.

**Implikacja dla kancelarii:** pierwsza trójka czynników Local Pack (kategoria, odległość, nazwa) jest w dużej mierze poza optymalizacją treściową. Realne dźwignie to: poprawna kategoria główna per biuro, realny adres w mieście docelowym, opinie (ilość + ocena + świeżość), aktywność profilu. Dla organiki: osobna strona per usługa i realna, unikalna treść lokalna.

---

## 2. Google Business Profile — twarde zasady kwalifikacji

Wszystko poniżej cytuję z [Guidelines for representing your business on Google](https://support.google.com/business/answer/3038177). To jest dokument, którego złamanie kończy się zawieszeniem profilu, więc traktuj go jako warunek brzegowy, nie jako "dobrą praktykę".

### Kwalifikacja lokalizacji

- Firma musi mieć "a physical location that customers can visit, or travels to customers where they are". **Nie twórz profilu dla miasta, w którym nie masz ani biura, ani obsługi na miejscu.**
- **Nie używaj wirtualnego biura.** Google: "If your business rents a physical mailing address but doesn't operate out of that location, also known as a virtual office, that location isn't eligible for a Business Profile". Skrzynki pocztowe i P.O. box też są wykluczone.
- **Coworking tylko warunkowo.** Google: "Businesses can't list an office at a co-working space unless that office maintains clear signage, receives customers at the location during business hours, and is staffed during business hours by your business staff". Trzy warunki łącznie: oznakowanie, przyjmowanie klientów, własny personel w godzinach otwarcia.
- **Wymagane trwałe oznakowanie:** "Businesses showing their address on Google should maintain permanent fixed signage of their business name at the address".

Egzekwowanie w branży prawniczej jest zaostrzone; wirtualne biura kancelarii bywają usuwane z Map — źródło wtórne: [PaperStreet — Virtual Offices and Google](https://www.paperstreet.com/blog/virtual-offices-and-google-these-two-dont-mix/) `[do weryfikacji co do skali]`.

### Nazwa firmy

- "Your name should reflect your business's real-world name, as used consistently on your storefront, website, stationery."
- Zakazane w nazwie: hasła marketingowe, kody sklepów, numery telefonu, adresy URL, godziny otwarcia, nieistotne terminy prawne.
- **Nie dopisuj miasta ani usługi do nazwy** ("Kancelaria X Warszawa doradztwo podatkowe"), nawet jeśli badania (Whitespark: keywords in GBP title = #3 czynnik) sugerują, że to działa. To jawne naruszenie wytycznych i jedna z głównych przyczyn zawieszeń. Źródło wtórne o wzmożonej egzekucji w 2026: [Digital Applied](https://www.digitalapplied.com/blog/local-seo-march-2026-core-update-gbp-optimization-guide) `[do weryfikacji]`.
- Wyjątek jest tylko jeden: jeśli miasto **jest** częścią realnej nazwy używanej na szyldzie i w dokumentach.

### Kategorie

- "Use as few categories as possible to describe your overall core business."
- Kategoria ma uzupełniać zdanie "This business **IS** a", nie "HAS a". Czyli: *Kancelaria prawna*, *Doradca podatkowy*, *Biuro rachunkowe* — nie *Usługa*, nie *Konsultant*.
- Wybieraj kategorię szczegółową zamiast ogólnej.
- **Nie używaj kategorii jako słów kluczowych** ani do opisu udogodnień.
- Kategoria główna to najsilniejszy czynnik wg Whitespark — poświęć jej realny research: sprawdź, jakich kategorii używają firmy z top 3 local packa dla docelowych fraz w każdym mieście osobno.

### Telefon i strona

- "The phone number must be under the direct control of the business"; nie może przekierowywać do call center ani usług zewnętrznych.
- Telefon i strona mają reprezentować **konkretną lokalizację**, nie centralę. Czyli: numer lokalny per biuro i link do strony tego biura, nie do strony głównej.

### Godziny

- Podawaj rzeczywiste godziny obsługi klienta.
- Google: niektóre typy firm nie powinny podawać godzin (hotele, szkoły, kina, firmy działające wyłącznie na umówione spotkania). Kancelaria pracująca stacjonarnie z recepcją — podaj godziny; oddział czysto "na umówione spotkanie" — rozważ pominięcie. `[do weryfikacji, czy kancelaria mieści się w wyjątku]`
- Uzupełniaj **godziny świąteczne**. "Firma otwarta w momencie wyszukiwania" to wg Whitespark #5 czynnik Local Pack; profil oznaczony jako zamknięty traci widoczność w tym momencie.

### Firmy usługowe bez punktu obsługi (SAB)

- "should hide your business address from customers"
- Obszar obsługi "shouldn't extend farther than about 2 hours of driving time" od bazy.
- **Nie ustawiaj obszaru obsługi na całą Polskę** dla każdego biura — to rozmywa sygnał i jest sprzeczne z wytyczną.

---

## 3. Kilka biur i kilku prawników — konfiguracja profili

Z tych samych wytycznych Google:

- **Jeden profil na lokalizację.** "Do not create more than one page for each location of your business, either in a single account or multiple accounts."
- **Ta sama nazwa we wszystkich lokalizacjach** w obrębie kraju, chyba że realne oznakowanie się różni.
- **Działy (departments)** mogą mieć osobne profile, ale muszą mieć odrębne nazwy i **inną kategorię główną** niż firma matka. Dla kancelarii: potencjalnie sensowne rozdzielenie "kancelaria prawna" / "biuro rachunkowe" pod jednym adresem, jeśli to realnie odrębne działy z odrębnymi nazwami. Sprawdź, czy nie wygląda to na duplikat pod tym samym adresem.
- **Praktycy indywidualni** (prawnicy, doradcy podatkowi) mogą mieć własny profil, jeśli "operate in a public-facing role" i "can be contacted directly at the verified location during stated hours". Personel wsparcia (asystenci, recepcja, marketing) — nie.

Praktyczne konsekwencje:

- **Rób** osobny profil dla każdego realnego biura, z lokalnym numerem, lokalnym adresem, własnymi zdjęciami i własną stroną lokalizacji jako URL.
- **Rób** profile praktyków tylko dla osób, które faktycznie przyjmują klientów pod danym adresem i mają bezpośredni kontakt. Każdy taki profil musi mieć wyróżniki (własny numer lub oznaczenie), inaczej Google może go scalić z profilem firmy — źródło wtórne: [Market My Market](https://www.marketmymarket.com/legal-marketing/law-firm-google-business-profile-optimization/) `[do weryfikacji]`.
- **Unikaj** tworzenia profilu-widma pod adresem, gdzie nikt nie siedzi. Weryfikacja wideo i kontrola współdzielonych adresów są coraz częstsze.
- **Rób** porządek w uprawnieniach: konto firmowe jako właściciel, zespół lokalny jako menedżer. Przy 10+ lokalizacjach dostępna jest weryfikacja masowa i grupy firm — źródło wtórne: [SEJ — The Complete Guide To Local SEO For Multiple Locations](https://www.searchenginejournal.com/local-seo-multiple-locations/370704/), Dan Taylor, aktualizacja 2026-06-18.

---

## 4. Opinie — pozyskiwanie zgodne z zasadami

To jest obszar o najwyższym ryzyku regulacyjnym: jednocześnie polityka Google i polskie prawo konsumenckie.

### Co Google wprost zakazuje

Z [Prohibited & restricted content — Maps User Generated Content Policy](https://support.google.com/contributionpolicy/answer/7400114):

- **Fake engagement:** "Content that is not based on a real experience or does not accurately represent the location or product in question."
- **Opinie opłacone:** "Reviews or ratings that have been paid for, directly or in kind."
- **Zachęty:** "Content that has been posted due to an incentive offered by a business - such as payment, discounts, free goods and/or services." Zakazane jest "Offer incentives ... in exchange for posting any review or revision or removal of a negative review".
- **Kwoty dla pracowników:** "Merchants requesting that staff solicit a certain number of reviews".
- **Sterowanie treścią opinii:** "Merchants requesting that staff solicit reviews that include specific content, including content that identifies a staff member." Czyli **nie proś klientów o wymienienie w opinii imienia opiekuna sprawy**.
- **Presja na miejscu:** merchants "should not require or pressure users to leave ratings or write reviews while on the premises, nor should they request that specific content be included".
- **Podszywanie się:** zakaz treści publikowanych w celu podszycia się pod osobę lub organizację. Czyli: nie pisz opinii z kont pracowników ani rodziny.

Co jest **dozwolone**: "Solicit or encourage the posting of content that does represent a genuine experience, without offering incentives."

### Review gating

Praktyka "najpierw ankieta wewnętrzna, do Google kierujemy tylko zadowolonych" jest opisywana jako naruszenie (selektywne pozyskiwanie opinii). Nie znalazłem tego sformułowanego wprost jako osobny punkt w cytowanej polityce Google — źródła wtórne: [Birdeye — Google review policy](https://birdeye.com/blog/google-review-policy/), [Applause](https://www.applausehq.com/blog/googles-rules-for-incentivizing-reviews). `[do weryfikacji w oficjalnym tekście polityki]`. Zalecenie mimo to: **nie filtruj, kogo prosisz o opinię, po przewidywanej ocenie.**

### Sankcje po stronie Google

Usuwanie opinii, wstrzymanie przyjmowania nowych, zawieszenie profilu, publiczny baner ostrzegawczy dla konsumentów przy profilu. Źródła wtórne: [Birdeye](https://birdeye.com/blog/google-review-policy/), [Stan Ventures](https://www.stanventures.com/news/google-maps-implements-stricter-policy-on-fake-reviews-144/). `[do weryfikacji szczegółów mechaniki banera]`

### Warstwa prawna w Polsce

Dyrektywa Omnibus (wdrożona w Polsce od 2023-01-01) nakłada na przedsiębiorcę obowiązek weryfikacji, czy opinie pochodzą od osób, które faktycznie skorzystały z produktu lub usługi, oraz obowiązek poinformowania, czy i jak taka weryfikacja jest prowadzona. Publikowanie fałszywych opinii lub zlecanie ich to nieuczciwa praktyka rynkowa. Sankcja: do 10% obrotu z poprzedniego roku za naruszenie zbiorowych interesów konsumentów, dodatkowo do 2 mln zł dla osoby zarządzającej.

Źródła: [Prawo.pl — Dyrektywa omnibus a fałszywe opinie w sieci](https://www.prawo.pl/prawo/dyrektywa-omnibus-a-falszywe-opinie-w-sieci,516032.html), [Poradnik Przedsiębiorcy](https://poradnikprzedsiebiorcy.pl/-dyrektywa-omnibus-a-publikacja-opinii-o-produktach-i-uslugach)

Ważne zastrzeżenie dla kancelarii B2B: przepisy o nieuczciwych praktykach rynkowych chronią **konsumentów**. Zakres ich zastosowania do relacji czysto B2B (klient = spółka) jest ograniczony, ale kancelaria obsługująca też osoby fizyczne prowadzące działalność i klientów indywidualnych powinna traktować obowiązek jako obowiązujący. **Skonsultuj zakres wewnętrznie — to nie jest ustalenie SEO.** `[do weryfikacji prawnej]`

Praktyczne minimum, jeśli publikujecie opinie na stronie mentzen.pl:
- Opisz w regulaminie lub na stronie z opiniami, **czy i jak** weryfikujecie, że opinia pochodzi od realnego klienta.
- Nie usuwaj selektywnie opinii negatywnych z własnego serwisu, jeśli deklarujesz, że publikujesz wszystkie.
- Nie agreguj ocen z Google do widżetu na stronie w sposób sugerujący własny, niezależny system ocen.

### Proces pozyskiwania — co robić

- **Rób** prośbę o opinię jako standardowy, neutralny krok po zamknięciu sprawy: krótki mail lub SMS z bezpośrednim linkiem do formularza opinii profilu **właściwego biura**.
- **Rób** to konsekwentnie i ciągle. Świeżość opinii jest wskazywana jako rosnący czynnik (Whitespark 2026: wzrost wagi sygnałów opinii). Lepszy stały strumień 2–4 opinii miesięcznie per biuro niż jednorazowa kampania na 50.
- **Rozkładaj opinie na biura.** Opinie są przypisane do profilu lokalizacji; 200 opinii na centrali nie pomoże oddziałowi w innym mieście.
- **Unikaj** konkursów, rabatów, gadżetów, "za opinię 10% zniżki", tabletów z formularzem na recepcji, kwot dla pracowników.
- **Unikaj** proszenia o wymienienie nazwiska prawnika w treści opinii.

### Odpowiadanie na opinie

- **Odpowiadaj na wszystkie**, także pozytywne. Google wprost łączy "helpful replies" z wyróżnieniem się profilu ([support.google.com/business/answer/7091](https://support.google.com/business/answer/7091)).
- **Odpowiadaj szybko** i z jednego, spójnego tonu; ustal szablon ramowy, ale nie kopiuj odpowiedzi słowo w słowo — powtarzalne odpowiedzi wyglądają na automat.
- **Nie ujawniaj w odpowiedzi żadnych informacji o sprawie klienta.** Dla kancelarii to nadrzędne nad SEO: tajemnica adwokacka/radcowska i tajemnica doradcy podatkowego obowiązują też w publicznej odpowiedzi na opinię. Bezpieczna formuła przy negatywnej opinii: podziękowanie, brak potwierdzania ani zaprzeczania, że osoba była klientem, zaproszenie do kontaktu offline. `[do weryfikacji z działem compliance / etyki zawodowej]`
- **Nie wpychaj słów kluczowych** do odpowiedzi. Odpowiedź jest dla czytelnika, a nadmiar fraz wygląda sztucznie.
- Opinie ewidentnie fałszywe lub naruszające politykę zgłaszaj przez mechanizm Google zamiast wdawać się w polemikę.

---

## 5. Strony lokalizacji i strony "miasto × usługa" — jak nie zrobić doorway pages

### Definicja Google

Z [Google Search Essentials — Spam policies](https://developers.google.com/search/docs/essentials/spam-policies) (aktualizacja 2026-08-28):

> "Doorway abuse is when sites or pages are created to rank for specific, similar search queries. They lead users to intermediate pages that aren't as useful as the final destination."

Przykłady wskazane przez Google obejmują wiele stron kierowanych na konkretne regiony lub miasta, które przepychają użytkownika w inne miejsce.

Powiązana polityka:

> "Scaled content abuse is when many pages are generated for the primary purpose of manipulating search rankings and not helping users."

Obejmuje to generowanie wielu stron niskiej wartości narzędziami AI. **Wygenerowanie 200 stron "usługa + miasto" z podmienioną nazwą miasta mieści się w obu definicjach jednocześnie.**

### Test decyzyjny: czy w ogóle tworzyć stronę dla danego miasta

Twórz stronę tylko, gdy spełnione są łącznie:
1. Masz w tym mieście **realną obecność** — biuro, zespół, klientów.
2. Strona jest **celem końcowym** użytkownika: da się z niej załatwić sprawę (kontakt, umówienie, cennik, dane biura), a nie tylko przejść do centralnego formularza.
3. Masz **materiał na unikalną treść**: własny zespół z tego biura, realizacje, specyfikę rynku lokalnego.

Kryteria zbieżne u Miriam Ellis: [Search Engine Land — Service area pages](https://searchengineland.com/guide/service-area-pages), aktualizacja 2025-11-27, oraz u Dana Taylora: [SEJ](https://www.searchenginejournal.com/local-seo-multiple-locations/370704/), 2026-06-18.

**Dla kancelarii oznacza to:** rób strony biur dla miast, w których faktycznie są biura. Nie rób siatki "doradztwo podatkowe [dowolne miasto powiatowe]".

### Co musi zawierać dobra strona lokalizacji

Za Search Engine Land (Ellis, 2025-11-27) i SEJ (Taylor, 2026-06-18):

- URL zawierający miasto i usługę, np. `/biura/warszawa/` lub `/doradztwo-podatkowe/wroclaw/`
- Title tag ok. 55–70 znaków z miastem i usługą; meta description 150–160 znaków z korzyścią dla klienta
- H1 wskazujący usługę i miasto
- **Pełne dane NAP tego biura** + telefon lokalny, mail, mapa, wskazówki dojazdu, parking, dostępność
- **Zdjęcia rzeczywiste** — budynek, wnętrze, zespół tego biura. Nie stock.
- **Zespół tego biura**: sylwetki prawników i doradców z tego adresu, z uprawnieniami i specjalizacjami (to jednocześnie sygnał E-E-A-T dla treści YMYL)
- **Usługi realnie świadczone w tym biurze** — jeśli oddział nie robi obsługi celnej, nie wypisuj jej
- **Dowody lokalne**: case studies, realizacje, opinie klientów z tego miasta, lokalne wyróżnienia, patronaty, wystąpienia
- **FAQ specyficzne dla lokalizacji**: właściwość miejscowa urzędu skarbowego, sąd rejestrowy, lokalne terminy, dojazd
- **Jasne CTA** i kompletne kanały kontaktu

Taylor: około połowa strony może być wspólną treścią marki, druga połowa musi być realnie lokalna. To użyteczna heurystyka proporcji `[do weryfikacji jako reguła — to opinia autora, nie stanowisko Google]`.

### Architektura i linkowanie

- Model hub-and-spoke: strona-katalog biur (hub) → strony poszczególnych biur (spoke), z powrotnym linkowaniem.
- **Linkuj strony lokalizacji z nawigacji lub z wyraźnej sekcji "Nasze biura"**, nie chowaj ich wyłącznie za wyszukiwarką oddziałów (JS-owy store locator bywa niecrawlowalny).
- Umieść je w sitemapie XML i w mapie HTML.
- Linkuj z GBP i z katalogów **bezpośrednio na stronę danego biura**, nie na stronę główną (Ellis).
- Krzyżowo linkuj ze stron usługowych i z artykułów blogowych dotyczących lokalnych tematów.

### Relacja "usługa" × "miasto"

- Whitespark 2026 wskazuje **dedykowaną stronę per usługa** jako czynnik #1 dla organiki lokalnej. Zbuduj najpierw kompletną, mocną warstwę stron usługowych (prawo, doradztwo podatkowe, księgowość, i ich podusługi), a dopiero potem warstwę lokalizacji.
- **Unikaj macierzy pełnej** usługa × miasto. Przy 3 biurach i 20 usługach macierz daje 60 stron, z których większość nie ma unikalnej treści. Bezpieczniejszy wzorzec: strony usług (globalne, mocne) + strony biur (lokalne, bogate) + ewentualnie **pojedyncze** strony usługa×miasto tam, gdzie jest realny popyt i realna specyfika lokalna.
- Waliduj popytem: sprawdź w narzędziu słów kluczowych, czy fraza "doradca podatkowy [miasto]" ma w ogóle wolumen w Polsce dla danego miasta. Brak wolumenu = brak powodu do tworzenia strony.

### Anty-wzorce

- Podmiana nazwy miasta w tym samym szablonie
- Strona lokalizacji bez adresu i telefonu, z samym formularzem kierującym do centrali
- Lista 100 miast "obsługiwanych" jako osobne podstrony
- Generowanie opisów lokalnych przez LLM bez lokalnych faktów (mieści się w "scaled content abuse")
- Ukrywanie stron lokalizacji przed użytkownikami, a pokazywanie robotom

---

## 6. NAP i spójność danych

**NAP = Name, Address, Phone.** Zasada: **jeden kanoniczny format zapisu na lokalizację, identyczny wszędzie.**

Co ustandaryzować i zapisać w dokumencie źródłowym (jedna tabela per biuro):
- Pełna nazwa prawna i nazwa handlowa (ta, która idzie do GBP)
- Adres w jednym formacie (skrót "ul." albo jego brak, numer lokalu zawsze tak samo, kod pocztowy z myślnikiem)
- Telefon w jednym formacie (np. `+48 XX XXX XX XX`)
- Adres e-mail per lokalizacja
- URL strony tej lokalizacji
- Godziny otwarcia
- NIP/REGON/KRS — dla polskich katalogów i dla wiarygodności

Zasady:
- **Rób** dokładnie ten sam NAP w: GBP, stopce/stronie biura, schema LocalBusiness, katalogach, mediach społecznościowych, wizytówkach Bing Places i Apple Business Connect.
- **Rób** aktualizację wszystkich miejsc jednocześnie przy przeprowadzce lub zmianie numeru. Rozjazd danych po zmianie adresu to najczęstsza przyczyna spadków.
- **Unikaj** numerów przekierowujących do call center (naruszenie wytycznych GBP) i numerów śledzących (call tracking) jako głównego numeru w GBP; jeśli używasz call trackingu, główny numer w GBP musi zostać numerem realnym. `[do weryfikacji — Google historycznie dopuszczał numer trackingowy jako primary przy numerze realnym jako additional; sprawdź aktualny stan]`
- **Unikaj** publikowania adresu biura, jeśli to biuro nie kwalifikuje się wg wytycznych GBP.

Cytowania (citations) w Polsce — sensowna baza startowa: Panorama Firm, pkt.pl, Aleo, Firmy.net, Targeo, Biznesfinder, plus branżowe: listy KIDP (doradcy podatkowi), listy okręgowych rad adwokackich i izb radców prawnych, Rejestr.io / KRS-owe agregatory. Źródła wtórne: [WeNet — ranking katalogów](https://wenet.pl/blog/reklama-w-internecie-ranking-top-25-miejsc-do-lokalnej-promocji-firmy/), [Ranktracker — przewodnik po local SEO w Polsce](https://www.ranktracker.com/pl/blog/a-complete-guide-for-doing-local-seo-in-poland/). `[do weryfikacji aktualności listy i wartości poszczególnych katalogów]`

Priorytet: **wpisy branżowe i regionalne (izby, organizacje gospodarcze, lokalne media) > masowe katalogi**. Masowe katalogi mają dziś głównie wartość spójnościową, nie linkową — teza powtarzana w źródłach wtórnych, brak potwierdzenia u Google `[do weryfikacji]`.

Uwaga o statystykach typu "53% wyższe pozycje dzięki spójnemu NAP" krążących po blogach: nie znalazłem dla nich pierwotnego badania. Nie używaj takich liczb w materiałach.

---

## 7. Schema LocalBusiness

Dokumentacja: [Google Search Central — Local business (LocalBusiness) structured data](https://developers.google.com/search/docs/appearance/structured-data/local-business), aktualizacja 2025-12-10.

### Wymagane

- `name`
- `address` jako `PostalAddress` z `streetAddress`, `addressLocality`, `addressRegion`, `postalCode`, `addressCountry`

### Rekomendowane

- `geo` (`GeoCoordinates`, `latitude`/`longitude`) — Google wymaga **co najmniej 5 miejsc po przecinku**
- `telephone` z numerem kierunkowym kraju i miasta
- `url` — pełny, działający link do strony **tej lokalizacji**
- `priceRange` — max 100 znaków
- `openingHoursSpecification` — `dayOfWeek`, `opens`/`closes` w formacie `hh:mm:ss`; `validFrom`/`validThrough` (`YYYY-MM-DD`) dla okresów sezonowych; `00:00`–`23:59` dla całodobowych, `00:00`–`00:00` dla dnia zamkniętego

### Typ: użyj podtypu

Zamiast generycznego `LocalBusiness` użyj najbardziej szczegółowego podtypu:
- kancelaria prawna → [`LegalService`](https://schema.org/LegalService) (podtyp `ProfessionalService`)
- biuro rachunkowe / doradztwo podatkowe → `AccountingService`
- w razie wątpliwości przy jednym podmiocie łączącym usługi: `ProfessionalService` z `department` na podtypy

Google dopuszcza zagnieżdżanie `department` z podtypami `LocalBusiness`; nazwa działu powinna zawierać nazwę firmy plus nazwę działu.

Uwaga: `areaServed` nie występuje w dokumentacji Google dla tego typu rich resultu, choć istnieje w schema.org. Możesz go użyć jako informacji dla parserów, ale nie licz na wpływ na rich result. Źródło: powyższa dokumentacja Google + [schema.org](https://schema.org/LegalService).

### Opinie i oceny w schema — twarda zasada

Z [Review snippet structured data](https://developers.google.com/search/docs/appearance/structured-data/review-snippet), aktualizacja 2026-07-24:

> "If the entity that's being reviewed controls the reviews about itself, their pages that use `LocalBusiness` or any other type of `Organization` structured data are ineligible for star review feature."

Dotyczy to również opinii wstawianych przez widżety firm trzecich osadzone na własnej stronie.

Dodatkowo: "Don't aggregate reviews or ratings from other websites" — **nie przepisuj oceny z Google do `aggregateRating` na własnej stronie.**

Wniosek normatywny: **nie oznaczaj własnych opinii o kancelarii przez `aggregateRating`/`review` licząc na gwiazdki.** Ryzyko manual action przy zerowym zysku. Opinie zbieraj w Google (tam się liczą do local packa) i publikuj na stronie jako zwykłą treść, bez markupu ocen o samym sobie.

### Reszta warstwy strukturalnej

- `Organization` tylko na stronie głównej / stronie "o nas"; `LocalBusiness` (podtyp) na stronach poszczególnych biur — rekomendacja: [SEJ, Taylor, 2026-06-18](https://www.searchenginejournal.com/local-seo-multiple-locations/370704/)
- `sameAs` z linkami do zweryfikowanego profilu GBP, LinkedIn, profili branżowych
- Waliduj każdą stronę w Rich Results Test i w Search Console (raport o elementach strukturalnych). Częsty błąd wdrożeniowy to niekompletny `PostalAddress` i brak `geo`.

---

## 8. Local pack dla usług profesjonalnych — specyfika

- **Local pack dla fraz prawno-podatkowych jest wysoce zależny od odległości.** Zapytanie "doradca podatkowy" z centrum Warszawy pokaże inne firmy niż to samo zapytanie z Ursusa. Nie oczekuj jednej "pozycji"; mierz widoczność siatką geograficzną (grid rank tracking), nie pojedynczym pomiarem.
- **Frazy YMYL.** Doradztwo prawne i podatkowe to obszar Your Money or Your Life. Sygnały E-E-A-T (autorzy z uprawnieniami, aktualizacje treści, cytowanie źródeł prawa) mają większe znaczenie niż w innych branżach — teza szeroko przyjęta w branży w oparciu o Search Quality Rater Guidelines. Nie zdążyłem zweryfikować bezpośrednio w aktualnej wersji SQRG `[do weryfikacji — pobrać aktualne Search Quality Rater Guidelines i sprawdzić rozdział o YMYL]`.
- **Local Services Ads / Google Screened** (weryfikacja licencji dla prawników) — o ile mi wiadomo niedostępne w Polsce `[do weryfikacji]`.
- **AI Overviews i AI Mode zmieniają local pack.** Źródła wtórne raportują, że w części zapytań pakiet trzech firm bywa zastępowany krótszą listą generowaną przez AI, z mniejszą liczbą firm i bez części elementów konwersyjnych. Źródła: [OnPurpose Media](https://onpurposemedia.com/ai-overview-local-packs-impacting-visibility/), [Digital Applied](https://www.digitalapplied.com/blog/local-seo-core-updates-gbp-strategy-may-2026). `[do weryfikacji — brak potwierdzenia w oficjalnych materiałach Google; traktować jako obserwację branżową, nie fakt]`
- Whitespark 2026 dodał kategorię "AI Search Visibility"; wg opisu raportu trzy z pięciu najważniejszych czynników w tej kategorii to sygnały cytowań i encji. Praktyczny wniosek: **spójne, szeroko rozsiane, zgodne dane o firmie (encja) zyskują na znaczeniu względem klasycznej optymalizacji on-page.** Źródło: [Whitespark](https://whitespark.ca/local-search-ranking-factors/), 2025-11-06.

---

## 9. Checklista wdrożeniowa (skrót do skilla)

**Per biuro, jednorazowo:**
- [ ] Profil GBP zweryfikowany, nazwa bez dopisków, kategoria główna dobrana po analizie konkurencji w tym mieście
- [ ] 2–3 trafne kategorie dodatkowe, żadnych kategorii "na słowo kluczowe"
- [ ] Lokalny numer telefonu pod bezpośrednią kontrolą biura
- [ ] URL w GBP → strona tego biura, nie strona główna
- [ ] Adres kwalifikowany (trwałe oznakowanie, obsługa klientów, własny personel w godzinach otwarcia)
- [ ] Pinezka na mapie ustawiona na wejście do budynku
- [ ] Usługi wpisane w GBP, opis firmy, atrybuty
- [ ] 10+ własnych zdjęć (budynek, wejście, wnętrze, zespół)
- [ ] Strona lokalizacji z unikalną treścią wg sekcji 5
- [ ] `LegalService`/`AccountingService` schema na stronie lokalizacji, walidowana
- [ ] NAP wpisany do dokumentu kanonicznego i rozsiany po katalogach

**Cyklicznie:**
- [ ] Godziny świąteczne uzupełniane z wyprzedzeniem
- [ ] Prośba o opinię po każdej zamkniętej sprawie, bez zachęt, link do właściwego profilu
- [ ] Odpowiedź na każdą opinię w ciągu 48 h, bez ujawniania szczegółów sprawy
- [ ] Post w GBP i nowe zdjęcia — regularnie (część źródeł wtórnych sugeruje ≥1×/miesiąc, część ≥1×/tydzień; brak potwierdzenia u Google `[do weryfikacji]`)
- [ ] Audyt spójności NAP raz na kwartał
- [ ] Monitoring pozycji siatką geograficzną, nie pojedynczym pomiarem

**Czerwone linie — nigdy:**
- Słowa kluczowe lub miasto w nazwie firmy w GBP
- Wirtualne biuro, skrytka pocztowa, adres współdzielony bez własnego personelu
- Rabaty, upominki, konkursy lub kwoty pracownicze za opinie
- Filtrowanie, kogo prosimy o opinię, po przewidywanej ocenie
- Prośba o wymienienie nazwiska pracownika w treści opinii
- Generowana masowo siatka stron "usługa + miasto" bez realnej obecności
- `aggregateRating` z własnymi opiniami lub ocenami przepisanymi z Google

---

## 10. Luki do domknięcia w kolejnej iteracji

- Aktualne **Search Quality Rater Guidelines** — rozdział YMYL i E-E-A-T pod kątem usług prawno-podatkowych (nie pobrałem)
- Oficjalne stanowisko Google w sprawie **review gating** (czy jest sformułowane wprost)
- Aktualna polityka Google wobec **numerów call tracking** w GBP
- Dostępność **Google Screened / Local Services Ads** w Polsce
- Faktyczne **wagi procentowe** z wykresu Whitespark 2026
- Zakres obowiązku **weryfikacji opinii z dyrektywy Omnibus** przy kliencie B2B — do ustalenia z prawnikami wewnętrznie
- Wpływ **zasad etyki zawodowej** (adwokaci, radcowie prawni, doradcy podatkowi) na treść odpowiedzi na opinie i na materiały lokalne — do ustalenia wewnętrznie

---

## Źródła

Wysokozaufane (dokumentacja i pomoc Google):
- [Improve your local ranking on Google](https://support.google.com/business/answer/7091)
- [Guidelines for representing your business on Google](https://support.google.com/business/answer/3038177)
- [Prohibited & restricted content — Maps User Generated Content Policy](https://support.google.com/contributionpolicy/answer/7400114)
- [Google Search Essentials — Spam policies](https://developers.google.com/search/docs/essentials/spam-policies) (2026-08-28)
- [Local business (LocalBusiness) structured data](https://developers.google.com/search/docs/appearance/structured-data/local-business) (2025-12-10)
- [Review snippet structured data](https://developers.google.com/search/docs/appearance/structured-data/review-snippet) (2026-07-24)
- [schema.org/LegalService](https://schema.org/LegalService)

Branżowe (uznane wydawnictwa):
- [Whitespark — Local Search Ranking Factors 2026](https://whitespark.ca/local-search-ranking-factors/) (2025-11-06)
- [Search Engine Land — Service area pages](https://searchengineland.com/guide/service-area-pages), Miriam Ellis (2025-11-27)
- [Search Engine Journal — The Complete Guide To Local SEO For Multiple Locations](https://www.searchenginejournal.com/local-seo-multiple-locations/370704/), Dan Taylor (2026-06-18)

Polski kontekst prawny:
- [Prawo.pl — Dyrektywa omnibus a fałszywe opinie w sieci](https://www.prawo.pl/prawo/dyrektywa-omnibus-a-falszywe-opinie-w-sieci,516032.html)
- [Poradnik Przedsiębiorcy — Dyrektywa Omnibus a publikacja opinii](https://poradnikprzedsiebiorcy.pl/-dyrektywa-omnibus-a-publikacja-opinii-o-produktach-i-uslugach)

Pomocnicze / niższe zaufanie (oznaczone w tekście jako `[do weryfikacji]`):
- [Birdeye — Google review policy](https://birdeye.com/blog/google-review-policy/)
- [Applause — Google's rules for incentivizing reviews](https://www.applausehq.com/blog/googles-rules-for-incentivizing-reviews)
- [Stan Ventures — Google Maps stricter policy on fake reviews](https://www.stanventures.com/news/google-maps-implements-stricter-policy-on-fake-reviews-144/)
- [Market My Market — Law Firm GBP Optimization](https://www.marketmymarket.com/legal-marketing/law-firm-google-business-profile-optimization/)
- [PaperStreet — Virtual Offices and Google](https://www.paperstreet.com/blog/virtual-offices-and-google-these-two-dont-mix/)
- [Digital Applied — Local SEO after March 2026 core update](https://www.digitalapplied.com/blog/local-seo-march-2026-core-update-gbp-optimization-guide)
- [OnPurpose Media — AI Overview local packs](https://onpurposemedia.com/ai-overview-local-packs-impacting-visibility/)
- [WeNet — ranking katalogów lokalnych](https://wenet.pl/blog/reklama-w-internecie-ranking-top-25-miejsc-do-lokalnej-promocji-firmy/)
- [Ranktracker — local SEO w Polsce](https://www.ranktracker.com/pl/blog/a-complete-guide-for-doing-local-seo-in-poland/)
