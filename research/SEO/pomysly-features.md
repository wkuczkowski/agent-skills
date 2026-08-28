# Backlog features agenta SEO: co budujemy po GSC

Stan na 2026-08-28. Kontekst: skill SEO dla mentzen.pl (kancelaria doradztwo podatkowe / prawo /
księgowość B2B, WordPress + Divi + Yoast, rynek polski, YMYL). Plik jest rejestrem pomysłów
odłożonych świadomie, a nie listą życzeń. Rzeczy, które budujemy teraz, opisuje
`narzedzia-agenta.md`; bibliografię i rangi zaufania źródeł opisuje `zrodla.md`.

Każdy pomysł ma ten sam format: co daje, nakład, zależności i wymagana konfiguracja, kiedy wrócić do
tematu, status decyzji użytkownika. Twierdzenia bez cytowania są moim szacunkiem i są tak oznaczone.
Niepotwierdzone zostają z `[do weryfikacji]`.

## Decyzje użytkownika z 2026-08-28

1. Najpierw CLI do Google Search Console. Reszta czeka.
2. SERP jest drugi w kolejce.
3. Przy SERP użytkownik chce wybrać między gotowym SERP API a własnym rozwiązaniem i prosi o
   rzetelne porównanie obu. Temu poświęcona jest sekcja 1, bo to jedyna decyzja w tym pliku, która
   ma konsekwencje prawne dla kancelarii, a nie tylko budżetowe.
4. Badanie cytowań AI dla polskich fraz podatkowych odłożone do czasu, aż będzie dostęp do GSC.

---

## 1. SERP: gotowe API kontra własny scraper

To jest jedna decyzja z dwoma wariantami, dlatego opisuję ją razem, a nie jako dwa osobne pomysły.

### 1.0 Co w ogóle daje warstwa SERP

Bez niej agent zna wyłącznie własne dane z GSC, czyli wie, na co wchodzą ludzie, ale nie wie, kto
stoi obok w wynikach ani jak wygląda strona wyników. Warstwa SERP odblokowuje cztery rzeczy naraz:
rank tracking (sekcja 2), przegląd SERP przed pisaniem tekstu (sekcja 3), wykrywanie AI Overview i
featured snippetów na frazach podatkowych oraz monitoring wzmianek przez endpoint newsowy
(sekcja 7). Wszystkie cztery czekają na tę samą decyzję.

### 1.1 Wariant A: gotowe SERP API

**Co daje.** Ustrukturyzowany JSON z wynikami Google dla frazy, lokalizacji i języka. Dostawca bierze
na siebie proxy, obchodzenie zabezpieczeń, parsowanie zmieniającego się HTML-a i utrzymanie tego w
czasie. My piszemy klienta HTTP i warstwę historii.

Ceny sprawdzone bezpośrednio u dostawców 2026-08-28. Kolumna „realny koszt" liczy nasz scenariusz
bazowy: 200 fraz mierzonych raz w tygodniu, czyli około 800 zapytań miesięcznie.

| Dostawca | Model | Cena jednostkowa | Realny koszt przy 800 SERP/mies. | Źródło |
| --- | --- | --- | --- | --- |
| **DataForSEO** Standard | pay-as-you-go, min. wpłata $50 `[do weryfikacji]` | $0.0006/SERP, czyli $0.60/1000 | ok. **$0.48** | [cennik](https://dataforseo.com/pricing/serp/google-organic-serp-api) |
| DataForSEO Live | jw. | $0.002/SERP | ok. $1.60 | jw. |
| **Serper** | przedpłacone kredyty | ok. $1/1000 `[do weryfikacji]` | ok. $0.80 `[do weryfikacji]` | [serper.dev](https://serper.dev/), 2500 zapytań gratis bez karty |
| **Bright Data** SERP API | pay-as-you-go | $1.50/1000 zapytań | ok. $1.20 | [cennik](https://brightdata.com/pricing/serp) |
| Bright Data plan | abonament | $499/mies. za 380 tys. zapytań, potem $1.30/1000 | nieadekwatny | jw. |
| **Oxylabs** SERP Scraper API | abonament od $49/mies. | Google bez renderowania JS $0.60–$1.00/1000 zależnie od planu | min. **$49/mies.** | [strona produktu](https://oxylabs.io/products/scraper-api/serp) |
| **SearchApi.io** Developer | abonament | $40/mies. za 10 tys., czyli $4/1000 | min. **$40/mies.** | [cennik](https://www.searchapi.io/pricing) |
| **SerpApi** Starter | abonament | $25/mies. za 1000 zapytań | min. **$25/mies.** | [cennik](https://serpapi.com/pricing) |
| **Scale SERP** | abonament | $23/mies. za 1000 zapytań przy płatności rocznej | min. **$23/mies.** | [cennik](https://trajectdata.com/pricing/scaleserp-api) |

Wniosek z tabeli jest brutalnie prosty: przy naszej skali cena za zapytanie nie ma znaczenia, ma
znaczenie wyłącznie to, czy dostawca wymaga abonamentu. DataForSEO, Serper i Bright Data w trybie
pay-as-you-go kosztują mniej niż dolar miesięcznie. Reszta zaczyna się od 23 do 49 dolarów za
przepustowość, której nie wykorzystamy w ułamku. Za te pieniądze kupujemy nie dane, tylko gwarancje
i osłonę prawną, i tak trzeba te plany oceniać.

Darmowe progi na testy: Serper 2500 zapytań bez karty ([serper.dev](https://serper.dev/),
2026-08-28), Bright Data 5000 rekordów miesięcznie bez karty
([cennik](https://brightdata.com/pricing/serp), 2026-08-28), Oxylabs 2000 wyników w triale
jednorazowym ([strona produktu](https://oxylabs.io/products/scraper-api/serp), 2026-08-28), SerpApi
250 zapytań miesięcznie w planie Free ([cennik](https://serpapi.com/pricing), 2026-08-28), Scale
SERP 125 zapytań miesięcznie ([cennik](https://trajectdata.com/pricing/scaleserp-api), 2026-08-28),
SearchApi.io 100 zapytań na start ([cennik](https://www.searchapi.io/pricing), 2026-08-28). Cały
prototyp da się zbudować i zwalidować za zero złotych, u trzech dostawców równolegle.

**Ryzyko ToS po stronie dostawcy.** Tu trzeba być precyzyjnym, bo od tego zależy cała decyzja.
Warunki korzystania z usług Google obowiązujące od 2026-07-30 zakazują wprost „using automated means
to access content from any of our services in violation of the machine-readable instructions on our
web pages (for example, robots.txt files that disallow crawling, training, or other activities)"
([policies.google.com/terms](https://policies.google.com/terms), sprawdzone 2026-08-28). Plik
`https://www.google.com/robots.txt` w trzeciej linii zawiera `Disallow: /search` (sprawdzone curl-em
2026-08-28). Czyli: automatyczne pobieranie wyników wyszukiwania Google jest niezgodne z warunkami
Google wprost z ich treści, nie na zasadzie interpretacji. To dotyczy każdego, kto to robi, łącznie z
dostawcami SERP API.

Co daje wariant z dostawcą: to dostawca prowadzi ruch do Google, utrzymuje infrastrukturę i ponosi
konsekwencje operacyjne. My kupujemy dane na podstawie umowy z dostawcą. Ryzyko nie znika, przenosi
się i zmienia charakter. Nadal odbieramy i wykorzystujemy dane pozyskane wbrew warunkom Google, i to
jest pytanie do compliance kancelarii, nie do mnie.

Dwaj dostawcy sprzedają to jako produkt. SerpApi reklamuje „U.S. Legal Shield provides up to
$2 million in coverage for the scraping and parsing of search engine data, as long as your use of the
data or service is not illegal", dostępne od planu Production ($150/mies.) w górę
([cennik](https://serpapi.com/pricing), 2026-08-28). SearchApi.io wymienia ochronę prawną do 2 mln
USD na wyższych planach ([cennik](https://www.searchapi.io/pricing), 2026-08-28). Obie osłony są
konstrukcjami amerykańskimi i warunkowane legalnością naszego użycia. Jak to działa dla polskiego
podmiotu, nie sprawdziłem i nie znalazłem żadnego materiału, który by to opisywał `[do weryfikacji]`.
Moja opinia: sprzedawanie polisy na ryzyko, którego zakres nie jest zdefiniowany dla naszej
jurysdykcji, jest głównie argumentem sprzedażowym, ale przy kancelarii nawet argument sprzedażowy
warto przeczytać w oryginale przed podpisaniem.

Dodatkowo SerpApi ma „ZeroTrace Mode", czyli brak przechowywania parametrów zapytań, samych zapytań i
wyników po stronie dostawcy, dostępny na planach Cloud ([cennik](https://serpapi.com/pricing),
2026-08-28). Dla kancelarii to jedyna funkcja z całej tabeli, która dotyka poufności, ale nasze
zapytania to frazy podatkowe, nie sprawy klientów, więc na dziś nie jest to argument. Zostaje jako
rzecz do zapamiętania, gdyby kiedyś ktoś chciał wrzucać do SERP API zapytania markowe klientów.

**Nakład.** Klient HTTP, mapowanie odpowiedzi na własny model, zapis do SQLite, licznik budżetu.
Dzień do dwóch dni pracy dla pierwszego dostawcy (mój szacunek), plus godziny na kolejnych, jeśli
zrobimy to za jednym interfejsem.

**Zależności i wymagana konfiguracja.** Konto u dostawcy, klucz albo login i hasło do `.env` poza
repo z `chmod 600`. Przy DataForSEO pierwsza wpłata (w notatkach $50, kwoty nie ma na publicznych stronach cennika,
potwierdzić przy zakładaniu konta `[do weryfikacji]`) i twardy limit wydatków ustawiony w panelu. Parametry
dla Polski: `location_name: "Poland"`, `language_code: "pl"`; kod `location_code` zweryfikować przez
`/v3/keywords_data/google_ads/locations`, bo w notatkach krąży 2616 i 616 `[do weryfikacji]`.

**Kiedy wrócić.** Zaraz po tym, jak CLI do GSC odda pierwsze raporty i powstanie lista 150–300 fraz
filarowych. Bez tej listy SERP API mierzy przypadkowe rzeczy.

**Status.** Wariant nierozstrzygnięty, decyzja użytkownika oczekiwana. Kolejność wdrożenia
zdecydowana 2026-08-28: po GSC.

### 1.2 Wariant B: własne rozwiązanie

**Co daje.** Dokładnie to samo co wariant A, plus niezależność od dostawcy i brak przekazywania
komukolwiek listy naszych fraz. To drugie jest realną zaletą i nie chcę jej bagatelizować: lista
monitorowanych fraz kancelarii to informacja handlowa.

**Z czego się składa.** Headless Chrome (Playwright), pula proxy z rotacją IP, obsługa captcha,
parser HTML wyników Google, warstwa wykrywania, że parser przestał działać, oraz cała reszta tego, co
w wariancie A robi dostawca.

**Nakład.** Mój szacunek, nie liczba z badania: tydzień pracy na wersję, która działa, i stały koszt
utrzymania rzędu kilku godzin miesięcznie plus awaryjne naprawy po każdej zmianie po stronie Google.
Google zmienia układ wyników i mechanizmy antybotowe bez zapowiedzi. Najgorszy tryb awarii nie polega
na tym, że skrypt się wywala, tylko na tym, że parser cicho zwraca zero wyników organicznych, a agent
raportuje spadek pozycji, którego nie ma. Ochrona przed tym to osobna warstwa testów kanarkowych na
frazach o znanym wyniku.

**Koszty bezpośrednie.** Proxy rezydencjalne u Oxylabs kosztują $6/GB w planie Starter i $4/GB w
planie Advanced ([cennik](https://oxylabs.io/products/residential-proxy-pool), 2026-08-28).
Rozwiązywanie captcha: reCAPTCHA v2 od 4,49 do 13,49 zł za 1000 sztuk, Cloudflare Turnstile 6,49 zł
za 1000 ([2captcha.com/pricing](https://2captcha.com/pricing), 2026-08-28, ceny podane w złotówkach).
Przyjmując mój szacunek, że jedna strona wyników Google z renderowaniem to 0,3–1 MB transferu, 1000
SERP-ów kosztuje 0,3–1 GB, czyli $1,2–$6 w samym transferze. To jest dwu- do dziesięciokrotność ceny
DataForSEO Standard ($0.60/1000), zanim policzymy captcha i zanim policzymy godzinę pracy.

**Ryzyko ToS bezpośrednio po naszej stronie.** To jest sedno i dlatego zostawiam to na koniec. W tym
wariancie to my łamiemy warunki Google, z naszej infrastruktury, i nie ma między nami a Google
żadnego pośrednika. Cytat i `Disallow: /search` z sekcji 1.1 obowiązują tu bez żadnego złagodzenia.
Do tego dochodzi drugi zapis: te same warunki zakazują „spamming, hacking, or bypassing our systems
or protective measures" ([policies.google.com/terms](https://policies.google.com/terms), 2026-08-28).
Rozwiązywanie captcha przez zewnętrzny serwis jest obchodzeniem środka zabezpieczającego w
najbardziej dosłownym możliwym sensie. To nie jest szara strefa, to jest wprost opisany przypadek.

Trzy konsekwencje, które przy kancelarii ważą inaczej niż przy sklepie internetowym:

1. **Reputacyjna.** Kancelaria doradztwa podatkowego utrzymująca farmę proxy i konto w serwisie
   rozwiązywania captcha to materiał na tekst, który sam się pisze. Ryzyko nie jest duże, ale koszt
   realizacji jest nieproporcjonalny do oszczędności rzędu kilku dolarów miesięcznie.
2. **Zawodowa.** Podmiot doradzający klientom w sprawach zgodności działa w sposób jawnie niezgodny z
   warunkami usługi, z której korzysta. Ocena, czy i jak to wpływa na obowiązki zawodowe, należy do
   compliance kancelarii, nie do agenta i nie do mnie.
3. **Dane osobowe.** Wyniki wyszukiwania zawierają nazwiska i dane osób. Przechowywanie zrzutów
   SERP-ów w lokalnej bazie to przetwarzanie danych osobowych i pytanie o podstawę prawną, zwłaszcza
   przy zapytaniach markowych. `[do weryfikacji]` z compliance. Przy wariancie A to samo pytanie
   dotyczy przechowywania po naszej stronie, ale pozyskaniem zajmuje się dostawca, który ma to
   opisane w swojej umowie.

Osobne pytanie, którego nie zweryfikowałem: jak do tego ma się wyjątek na eksplorację tekstów i
danych z dyrektywy DSM 2019/790 i jego wdrożenie w polskim prawie autorskim, oraz czy zastrzeżenie w
`robots.txt` liczy się jako skuteczny sprzeciw uprawnionego. To jest pytanie, na które kancelaria
odpowie lepiej niż jakikolwiek research SEO. `[do weryfikacji]`

**Zależności i wymagana konfiguracja.** Konto u dostawcy proxy, konto w serwisie captcha, Playwright
z Chromium na maszynie, harmonogram uruchomień, zestaw fraz kanarkowych do wykrywania cichej awarii
parsera. Nic z tego nie wymaga otwartych portów, więc reguła sieciowa z CLAUDE.md nie jest tu
naruszona.

**Kiedy wrócić.** Realnie: gdy dostawcy przestaną wystarczać. Dwa scenariusze, w których to się
dzieje. Pierwszy, gdy potrzebujemy danych, których żaden dostawca nie oddaje, na przykład zrzutów
konkretnego widgetu w polskim SERP-ie. Drugi, gdy lista monitorowanych fraz stanie się na tyle
wrażliwa, że nie chcemy jej wysyłać na zewnątrz. Ani jedno, ani drugie nie jest dziś prawdą.

**Status.** Użytkownik prosi o rzetelny opis, decyzji brak.

### 1.3 Moja rekomendacja

Zacząć od DataForSEO w trybie Standard, z twardym limitem wydatków. Powód nie jest cenowy, choć
cenowo też wygrywa: jedno konto pokrywa SERP-y, wolumeny z Keyword Plannera, `ranked_keywords`,
backlinki i widoczność w LLM-ach, czyli cztery inne pozycje z tego backlogu, i każda z nich osobno
kosztowałaby kolejny abonament.

Ważniejsza rzecz. Własne rozwiązanie **nie rozwiązuje problemu ToS, tylko przenosi go z dostawcy na
kancelarię i przy okazji dokłada obchodzenie zabezpieczeń, które warunki Google wymieniają osobno**.
Jeśli motywacją do wariantu B jest ostrożność prawna, to wariant B robi dokładnie odwrotnie niż
zamierzano. Jeśli motywacją jest niezależność albo poufność listy fraz, to jest to argument uczciwy,
ale kosztuje tydzień pracy, kilkadziesiąt dolarów miesięcznie i stałe utrzymanie.

Tani hedge, który proponuję niezależnie od wyboru: napisać jeden cienki interfejs dostawcy SERP z
jedną implementacją. Zmiana dostawcy staje się wtedy zmianą w `.env`, a nie przepisaniem narzędzia,
i drzwi do wariantu B zostają otwarte bez płacenia za nie dziś.

---

## 2. Rank tracking (`serp-snapshot`)

**Co daje.** Cotygodniowy zrzut pozycji dla koszyka fraz, top 10 konkurencji i obecności AI Overview,
featured snippetu oraz PAA. Wyjście to wyłącznie diff: zmiany większe niż 3 pozycje, wejścia i
wyjścia z top 10, nowe AI Overview. Rank tracking nie jest produktem do kupienia, jest pętlą lista
fraz → SERP API → SQLite → diff, a cała wartość siedzi w liście fraz i w tym, co agent zrobi ze
zmianą.

**Nakład.** Pół dnia ponad to, co powstanie w sekcji 1, bo klient HTTP i tabela historii są wspólne.

**Zależności.** Decyzja z sekcji 1. Lista 150–300 fraz filarowych. Tabele `keywords`,
`serp_snapshots`, `rankings` w SQLite. Reguła sezonowości z Trends, żeby agent nie diagnozował
lutowego wzrostu fraz PIT-owych jako sukcesu wdrożenia.

**Kiedy wrócić.** Natychmiast po sekcji 1.

**Status.** Brak osobnej decyzji, wchodzi razem z SERP.

---

## 3. `serp-inspect` przed pisaniem tekstu

**Co daje.** Na żądanie, w trybie Live, pełne 20 wyników plus PAA dla jednej frazy. To krok
poprzedzający napisanie artykułu. SERP-y prawno-podatkowe w Polsce są zdominowane przez
poradnikprzedsiebiorcy.pl, infor.pl, ifirma.pl i strony rządowe (analizy konkurencji w
`konkurencja.md`), więc bez tego kroku łatwo napisać poprawny tekst w formacie, który na tej frazie
nie wygrywa.

**Nakład.** Kilka godzin ponad sekcję 1.

**Zależności.** Sekcja 1, tryb Live u wybranego dostawcy.

**Kiedy wrócić.** Razem z sekcją 2. To najtańsza i najczęściej używana komenda z całej warstwy SERP.

**Status.** Brak osobnej decyzji.

---

## 4. Badanie cytowań AI dla polskich fraz podatkowych

**Co daje.** Odpowiedź na pytanie, czy Mentzen jest wymieniany, gdy ktoś pyta ChatGPT albo Gemini o
kancelarię podatkową dla spółki z o.o., o estoński CIT albo o fundację rodzinną. Dwa strumienie
danych. Pierwszy to raport Generative AI performance w GSC, czyli AI Overviews i AI Mode
([pomoc Google](https://support.google.com/webmasters/answer/16984139)); dostępność tego raportu
przez API jest niepotwierdzona, trzeba sprawdzić empirycznie wymiarem `searchAppearance`
`[do weryfikacji]`. Drugi to stały zestaw 30–50 polskich promptów puszczanych przez DataForSEO LLM
Responses ($0.0002 za zadanie w kolejce Standard plus koszt modelu,
[cennik](https://dataforseo.com/pricing/ai-optimization/llm-responses), 2026-08-28). Metryka: udział
promptów ze wzmianką marki oraz lista domen cytowanych zamiast nas, bo to jest gotowy plan
linkbuildingu.

Dwa ostrzeżenia do wpisania w narzędzie. Raport AI w GSC miał udokumentowany błąd w danych: błąd
logowania zaniżał wyświetlenia dla danych od 2026-08-13, co Google potwierdziło jako problem
raportowania, a nie spadek widoczności
([Search Engine Land](https://searchengineland.com/google-search-console-generative-ai-performance-report-in-search-data-bug-485215),
2026-08-17),
więc świeżym liczbom nie ufać bez porównania z poprzednim tygodniem. Cytowania w LLM-ach są
probabilistyczne, ten sam prompt zadany dwa razy da inne źródła, więc pojedynczy pomiar nic nie
znaczy i liczy się wyłącznie seria.

**Nakład.** Dzień na warstwę techniczną. Znacznie więcej na napisanie sensownego zestawu promptów,
bo to jest praca merytoryczna z użytkownikiem, nie kodowanie.

**Zależności.** Dostęp do GSC (stąd odłożenie). Konto DataForSEO. Twarda granica z CLAUDE.md: do
promptów nigdy nie trafiają treści spraw ani dane klientów kancelarii. Zapytanie „co wiesz o
kancelarii Mentzen" jest w porządku, zapytanie zawierające kontekst sprawy nie jest.

**Kiedy wrócić.** Po uruchomieniu CLI do GSC i po zebraniu 4–8 tygodni historii, żeby było do czego
porównywać. Wcześniej nie ma sensu, bo powstanie pomiar bez punktu odniesienia.

**Status.** Odłożone decyzją użytkownika 2026-08-28 do czasu dostępu do GSC.

---

## 5. Bulk export GSC do BigQuery

To jedyna pozycja w tym pliku, przy której zmieniam zdanie względem `narzedzia-agenta.md`, gdzie
napisałem, że przy jednym serwisie to przerost formy. Po sprawdzeniu dokumentacji uważam inaczej i
poniżej piszę dlaczego.

**Co daje.** Codzienny eksport danych o skuteczności do trzech tabel: `searchdata_site_impression`,
`searchdata_url_impression` i `ExportLog`
([dokumentacja tabel](https://support.google.com/webmasters/answer/12917991), 2026-08-28). Klucz
siedzi w tym, że eksport zawiera **wiersze dla zapytań zanonimizowanych, oznaczone flagą, z polem
`query` ustawionym na `null`**: „Rare queries (called anonymized queries) are marked with this bool.
The query field will be null when it's true to protect the privacy of users making the query" (tamże). API Search Analytics
takich wierszy nie zwraca w ogóle, przez co sumy wierszy nigdy nie zgadzają się z totalem.

Dla mentzen.pl to trafia dokładnie w opisany wcześniej problem: niszowe frazy prawno-podatkowe B2B to
typowy materiał na anonimizację, więc każdy raport „ile fraz mamy w TOP 10" liczony z API jest
systematycznie zaniżony o nieznaną wartość. Bulk export zamienia tę nieznaną wartość w liczbę, którą
da się raportować. Nie odzyskuje treści zapytań, ale mówi, jaki procent wyświetleń jest poza zasięgiem
raportów, a to zupełnie inna jakość rozmowy o danych.

**Drugi powód, ważniejszy niż pierwszy.** Eksport nie robi backfillu. Dokumentacja mówi wprost: „If
you want to see historical data that precedes your initial setup, use the Search Console API or the
reports" ([instrukcja konfiguracji](https://support.google.com/webmasters/answer/12917675),
2026-08-28). Pierwszy eksport wykonuje się w ciągu 48 godzin i obejmuje dane od dnia włączenia w
przód. Czyli każdy tydzień zwłoki to bezpowrotnie utracony tydzień pełnych danych. To jest opcja,
która wygasa codziennie, a kosztuje kilkanaście minut konfiguracji.

**Nakład.** Kilkanaście minut na włączenie. Zapytania SQL do tych tabel to osobna praca, ale można ją
odłożyć dowolnie długo, bo dane odkładają się same.

**Koszt.** Praktycznie zero, ale nie formalnie zero. BigQuery: zapytania on-demand $6.25 za TiB, przy
czym pierwszy 1 TiB miesięcznie jest darmowy; aktywne magazynowanie logiczne $0.000031507 za GiB na
godzinę, przy czym pierwsze 10 GiB miesięcznie jest darmowe
([cennik BigQuery](https://cloud.google.com/bigquery/pricing), 2026-08-28). Serwis z 523 wpisami
zmieści się w darmowym progu z zapasem rzędów wielkości. Warunek formalny jest inny: projekt musi
mieć **włączony billing**, czyli podpiętą kartę. To jest decyzja użytkownika, nie techniczna.

Uwaga eksploatacyjna z dokumentacji: Search Console domyślnie trzyma tabele bezterminowo i Google
sam zaleca ustawienie wygasania partycji (tamże). Warto to zrobić od razu, żeby za dwa lata nie
odkryć rosnącego rachunku za magazynowanie.

**Zależności i wymagana konfiguracja.** Projekt Google Cloud z włączonym billingiem, włączone API
BigQuery i BigQuery Storage, nadanie kontu `search-console-data-export@system.gserviceaccount.com`
ról „BigQuery Job User" i „BigQuery Data Editor", a następnie w Search Console: Ustawienia → Zbiorczy
eksport danych, podanie ID projektu, nazwy zbioru danych (zawsze zaczyna się od `searchconsole`) i
lokalizacji ([instrukcja](https://support.google.com/webmasters/answer/12917675), 2026-08-28).

**Kiedy wrócić.** Przy zakładaniu projektu GCP pod CLI do GSC, czyli w kroku 1 istniejącej roadmapy.
Nie później. Włączyć nawet wtedy, gdy przez pół roku nikt do tych tabel nie zajrzy.

**Status.** Nieomawiane z użytkownikiem. Rekomenduję podniesienie tego tematu przy konfiguracji GCP,
bo koszt zwłoki jest nieodwracalny, a koszt włączenia to kwadrans i zgoda na billing.

---

## 6. Lighthouse CI i warstwa Core Web Vitals

**Co daje.** Lighthouse CI odpowiada na pytanie „czy ten commit pogorszył wydajność". My tego pytania
nie mamy, bo mentzen.pl to WordPress z Divi, a nie repozytorium z pipeline'em wdrożeniowym, do
którego mamy dostęp. Właściwe pytanie brzmi „które z 800 podstron mają słaby LCP i czy poprawiło się
po zmianie", a na nie odpowiadają Unlighthouse (przekrojowy skan) i CrUX History API (40 tygodni
danych z pola, aktualizacja w poniedziałki).

**Stan narzędzi, sprawdzony przez API GitHuba 2026-08-28.** Lighthouse CI: ostatnie wydanie v0.15.1 z
2025-06-26, ostatni push 2026-03-27, repozytorium nie jest zarchiwizowane, ale tempo prac wyraźnie
spadło. Dla porównania sam Lighthouse: v13.4.1 z 2026-07-20, push z dnia sprawdzenia. Unlighthouse:
v0.18.0 z 2026-06-29, push 2026-08-14, projekt żywy.

**Nakład.** Unlighthouse to konfiguracja i pojedynczy przebieg, godziny. CrUX History to prosty
klient HTTP, pół dnia. Lighthouse CI wymagałby najpierw stworzenia pipeline'u, którego nie ma, więc
liczę go w tygodniach i odrzucam.

**Zależności.** Node 22+ dla Unlighthouse. Klucz API Google ograniczony do CrUX i PSI. Twardy limit
CrUX to 150 zapytań na minutę na projekt GCP, nie do podniesienia, więc przy skanie 800 URL-i razy
dwa formaty urządzenia potrzebny jest throttling. Dashboard, jeśli w ogóle, wiązać na `127.0.0.1`
zgodnie z CLAUDE.md.

**Kiedy wrócić.** Po punkcie 4 istniejącej roadmapy, czyli po GA4. Wcześniej nie warto, bo wyniki
wydajnościowe bez danych o ruchu prowadzą do optymalizowania podstron, których nikt nie odwiedza.
Jedna rzecz do wpisania w narzędzie od razu: CrUX to średnia krocząca z 28 dni, więc efekt wdrożenia
widać po około czterech tygodniach i agent nie może ogłaszać sukcesu po trzech dniach.

**Status.** Nieomawiane. Moja rekomendacja: Lighthouse CI odpuścić, Unlighthouse plus CrUX History
zbudować.

---

## 7. Monitoring wzmianek marki

**Co daje.** Cotygodniowa lista nowych wzmianek o marce z podziałem na linkujące i nielinkujące. Te
drugie są najtańszym zadaniem linkbuildingowym, jakie istnieje, bo tekst już powstał i wystarczy
poprosić o link. Przy marce osobowej, dzielonej z politykiem, dochodzi drugi wymiar: to jest
monitoring reputacyjny, nie tylko SEO, i część wyników nie będzie miała z kancelarią nic wspólnego.
Narzędzie musi to rozdzielać, inaczej utonie w szumie politycznym.

**Wariant DIY.** Zapytania markowe do endpointu news albo search w SERP API z `gl=pl` i `hl=pl`,
deduplikacja po URL-u, diff względem poprzedniego tygodnia. Koszt pomijalny, bo to kilkadziesiąt
zapytań miesięcznie. Pokrywa to, co indeksuje Google, i nie pokrywa mediów społecznościowych.

**Wariant komercyjny.** Brand24 jest polskim dostawcą i pokrywa social media, których wariant DIY nie
dotknie. Problem: strona `https://brand24.com/api/` zwraca 404 (sprawdzone 2026-08-28), podobnie
`/api-docs/`, `/help/api/` i `/features/api/`; subdomena `api.brand24.com` odpowiada, ale przekierowuje
na stronę główną. Nie znalazłem publicznej dokumentacji API. Istnienie, zakres i cennik `[do
weryfikacji]` bezpośrednio u dostawcy, mailem albo przez formularz.

**Nakład.** Wariant DIY: pół dnia po zbudowaniu sekcji 1. Wariant komercyjny: godzina na integrację
plus czas na rozmowę handlową i decyzję budżetową.

**Zależności.** Sekcja 1 dla wariantu DIY. Lista wariantów nazwy marki do monitorowania, w tym
odmiana i błędy pisowni.

**Kiedy wrócić.** Po sekcji 1, jako mały dodatek do warstwy SERP. Brand24 dopiero wtedy, gdy wariant
DIY pokaże, że social media faktycznie są luką, a nie gdy z góry założymy, że są.

**Status.** Nieomawiane. Decyzja o Brand24 jest budżetowa i należy do użytkownika.

---

## 8. IndexNow i Bing Webmaster Tools

**Co daje.** IndexNow to zgłaszanie zmian do Binga, Copilota i kilku mniejszych wyszukiwarek, ale
**nie do Google**, którego nie ma na liście uczestników
([searchengines.json](https://www.indexnow.org/searchengines.json), 2026-08-28). Nie przyspieszy więc
indeksacji tam, gdzie mamy 89% ruchu. Bing Webmaster Tools daje za darmo `GetQueryStats` i dane o
linkach jako kontrolę krzyżową dla GSC.

Czy to się opłaca, zależy od jednej liczby. Bing ma 7,16% udziału w rynku wyszukiwarek w Polsce
(Google 89,46%, Yandex 1,35%, DuckDuckGo 0,95%; Statcounter, lipiec 2026, sprawdzone 2026-08-28,
[źródło](https://gs.statcounter.com/search-engine-market-share/all/poland)). To nie jest zero. Do
tego indeks Binga zasila asystentów AI Microsoftu, więc realna wartość jest wyższa niż sam udział w
ruchu wyszukiwarki. Nadal jest to jednak jedna dwunasta tego, co daje Google.

**Nakład.** Najtańsza droga to oficjalna wtyczka IndexNow od Microsoftu w WordPressie, czyli
kwadrans, zero kodu, zero utrzymania. Własny poller ma sens tylko przy selektywnym zgłaszaniu i nie
widzę dla niego uzasadnienia. API Bing Webmaster Tools to pół dnia, z zastrzeżeniem: są zgłoszenia
pustych odpowiedzi z `GetLinkCounts`
([Microsoft Answers](https://learn.microsoft.com/en-us/answers/questions/5939109/bing-webmaster-tools-api-getlinkcounts-and-geturll)),
więc przed budowaniem czegokolwiek trzeba to sprawdzić empirycznie, a nie założyć, że działa.

**Zależności.** Weryfikacja mentzen.pl w Bing Webmaster Tools i klucz API. Klucz IndexNow wystawiony
pod `https://mentzen.pl/{klucz}.txt`. Dostęp do panelu WP do instalacji wtyczki.

**Kiedy wrócić.** Wtyczkę IndexNow można włączyć kiedykolwiek, choćby przy okazji porządków
technicznych z kroku 2 roadmapy, bo kosztuje kwadrans. API Binga dopiero po tym, jak GSC pokaże, ile
ruchu faktycznie przychodzi z Binga na mentzen.pl, bo dane ogólnorynkowe Statcountera nie muszą
odpowiadać profilowi odwiedzających kancelarię.

**Status.** Nieomawiane.

---

## 9. `backlinks-diff`

**Co daje.** Miesięczny diff profilu linków: nowe i utracone domeny linkujące, nowe anchory. Nowe
anchory są tu ważniejsze niż liczba linków, bo nagły przyrost anchorów komercyjnych z przypadkowych
domen to sygnał negatywnego SEO, a przy marce publicznej to scenariusz mniej abstrakcyjny niż zwykle.

**Nakład.** Pół dnia. Endpoint Backlinks w DataForSEO kosztuje $0.024 za zadanie plus $0.000036 za
wiersz ([cennik](https://dataforseo.com/pricing/backlinks/backlinks), 2026-08-28), czyli grosze przy
miesięcznej częstotliwości.

**Zależności.** Konto DataForSEO, czyli sekcja 1 w wariancie A. Tabela `backlinks` w SQLite.

**Kiedy wrócić.** Po sekcji 1, ale bez pośpiechu. Profil linków zmienia się w skali tygodni i
miesięcy, więc codzienne pobieranie to marnowanie zapytań, a pierwszy diff i tak wymaga dwóch
pomiarów odległych o miesiąc.

**Status.** Nieomawiane.

---

## 10. Mapa sezonowości i reguła sezon kontra problem

**Co daje.** Branża podatkowa jest sezonowa w stopniu, który psuje każdą naiwną analizę spadków: PIT
od lutego do kwietnia, CIT w marcu, zmiany przepisów od stycznia. Bez warstwy sezonowości agent
regularnie będzie diagnozował normalny majowy spadek fraz PIT-owych jako awarię i proponował naprawy
czegoś, co nie jest zepsute.

Reguła do wpisania na sztywno: Trends spada i GSC spada podobnie oznacza sezon i nietykanie strony;
Trends płaski lub rosnący przy spadającym GSC oznacza problem SEO i eskalację; Trends rosnący przy
płaskim GSC oznacza niewykorzystaną szansę.

Druga rzecz to mapa sezonowości koszyka fraz filarowych, czyli pięcioletnie szeregi tygodniowe i
profil roczny per fraza, z których powstaje kalendarz publikacji ustawiony 4–6 tygodni przed startem
sezonu. Tańszy substytut, który dostajemy przy okazji: `monthly_searches` z DataForSEO Google Ads
daje 12 miesięcy wstecz i przychodzi razem z wolumenami, za które i tak płacimy.

**Nakład.** Reguła sezon kontra problem to kilka godzin, bo to logika wpięta w istniejący raport
spadków. Pełna mapa sezonowości to dzień plus praca merytoryczna nad koszykiem fraz.

**Zależności.** Historia w GSC, czyli minimum kilka tygodni po uruchomieniu kroku 1. Źródło danych
Trends: `trendspyg` na start (limit około 8–10 sesji Explore na 15 minut, potem 429 na ponad pół
godziny, więc cache jest obowiązkowy) albo DataForSEO Google Trends produkcyjnie ($0.011 za zadanie
live, maksymalnie 5 fraz na request). `pytrends` jest zarchiwizowany (potwierdzone przez API GitHuba
2026-08-28, ostatni push 2024-08-10; sama data archiwizacji 2025-04-17 `[do weryfikacji]`, API jej
nie zwraca) i nie wolno go instalować.

**Kiedy wrócić.** Reguła sezon kontra problem: przy pierwszym raporcie spadków z GSC, czyli
praktycznie od razu, bo bez niej ten raport wprowadza w błąd. Mapa sezonowości: przy planowaniu
kalendarza treści, najlepiej jesienią, żeby zdążyć przed styczniowym szczytem.

**Status.** Nieomawiane.

---

## 11. Warstwa zapisu do WordPressa

**Co daje.** Możliwość, żeby agent zapisywał szkice i poprawki tytułów SEO oraz meta description
zamiast wyłącznie raportować, co należy poprawić. Bez tego każda rekomendacja on-page kończy się
ręcznym przepisywaniem przez człowieka, co przy 523 wpisach jest wąskim gardłem.

**Nakład.** Sam klient REST to godziny. Problemem jest konfiguracja i jedna pułapka. API Yoasta jest
wyłącznie do odczytu: „The Yoast REST API is currently read-only, and doesn't currently support
`POST` or `PUT` calls to update the data"
([Yoast dev](https://developer.yoast.com/customization/apis/rest-api/), 2026-08-28). Zachowanie przy
POST na `yoast_head_json`, czyli czy leci błąd, czy pole jest po cichu ignorowane, sprawdzić na
instancji `[do weryfikacji]`. Obejście to standardowy mechanizm WordPressa, nie ścieżka opisana przez
Yoasta: zarejestrować klucze meta przez `register_post_meta(..., show_in_rest => true)` w motywie
potomnym albo wtyczce `[do weryfikacji]`, czyli zmiana w kodzie na produkcji, której agent nie zrobi
sam.

**Zależności i wymagana konfiguracja.** Osobne konto agenta w WP z minimalną rolą, nigdy konto
administratora. Application Password (WP 5.6+). Snippet `register_post_meta` wdrożony przez człowieka.
Test na lokalnej kopii `local-mentzen` przed dotknięciem produkcji. Do sprawdzenia, czy Wordfence
Login Security nie blokuje Application Passwords przy włączonym 2FA `[do weryfikacji]`. Dokładne
nazwy kluczy meta Yoasta, prawdopodobnie `_yoast_wpseo_title` i `_yoast_wpseo_metadesc`, potwierdzić
na instancji `[do weryfikacji]`.

**Twarda granica.** Zapis wyłącznie jako `status: "draft"`. Kancelaria publikuje treści o skutkach
prawnych i człowiek zatwierdza każdą z nich. To nie jest ustawienie do zmiany „gdy zaufamy agentowi".

**Kiedy wrócić.** Gdy agent zacznie generować rekomendacje on-page szybciej, niż człowiek nadąża je
wdrażać. Wcześniej to rozwiązanie problemu, którego nie ma.

**Status.** Nieomawiane.

---

## 12. Reszta backlogu, krótko

Pozycje, przy których pełny format byłby dłuższy niż treść.

| Pomysł | Co daje | Nakład | Kiedy wrócić |
| --- | --- | --- | --- |
| **Audyt URL Inspection** | Stan indeksacji dla kilkuset artykułów: verdict, canonical Google kontra nasz, `lastCrawlTime` | Pół dnia, ale wymaga licznika dziennego i cache, bo kwota to 2000 zapytań na dobę na property i agent w pętli wypali ją w minutę | Po CLI do GSC, jako osobna komenda z twardym limitem |
| **Senuto** | Polska baza fraz (deklarowane 80 mln `[do weryfikacji]`, liczby nie ma w cenniku) i klastrowanie dostrojone do polskiej odmiany, plus własny serwer MCP `https://mcp.senuto.com/mcp` | Godziny na integrację, koszt 457,50 zł/mies. netto za plan Advanced, w którym dostęp do MCP jest już wliczony; dodatek MCP za 69 zł/mies. dokupuje się tylko do tańszych planów (cennik startuje od 107,50 zł za Lite; [cennik](https://www.senuto.com/pl/cennik/), 2026-08-28) | Dopiero gdy GSC i DataForSEO działają i widać konkretnie, czego brakuje. Decyzja budżetowa użytkownika |
| **Google Ads API (Keyword Planner)** | Darmowe wolumeny wprost od Google | Konfiguracja jest najdłuższa w całym zestawie: konto MCC, developer token, wniosek o poziom Basic. Poziom Explorer blokuje `KeywordPlanIdeaService` | Wniosek złożyć wcześnie i równolegle, bo nic nie kosztuje i trwa tygodnie. Budować dopiero po przyznaniu dostępu |
| **Alfa Trends API** | Oficjalne dane Trends bez limitów scrapingu | Sam wniosek to kwadrans. Po ponad roku to nadal alfa za formularzem, więc nie planować pod to architektury | Wniosek równolegle z powyższym, z konkretnym opisem zastosowania |
| **Screaming Frog headless** | Crawl z renderowaniem JS, czego advertools nie robi | Licencja £199/rok | Tylko gdy advertools okaże się niewystarczający, czyli gdy coś istotnego na mentzen.pl renderuje się w JS |
| **Endpointy `redirection/v1`** | Automatyczne dodawanie 301 przy zmianie slugów | Godziny, ale endpointy niepotwierdzone `[do weryfikacji]` | Razem z warstwą zapisu (sekcja 11) |

---

## 13. Czego świadomie nie budujemy

Zapisuję to, żeby przy kolejnym przeglądzie backlogu nikt nie odkrył tych pomysłów ponownie.

- **Indexing API Google do przyspieszania indeksacji bloga.** Obsługuje wyłącznie `JobPosting` i
  `BroadcastEvent` osadzony w `VideoObject`, co potwierdzono ponownie 2026-08-28
  ([dokumentacja](https://developers.google.com/search/apis/indexing-api/v3/quickstart)). Nie ma
  ścieżki, którą dałoby się to obejść legalnie.
- **Google Custom Search JSON API.** Zamknięte dla nowych klientów, wyłączenie 1 stycznia 2027
  ([dokumentacja](https://developers.google.com/custom-search/v1/overview)). Nie budować niczego.
- **Ahrefs albo Semrush kupowane wyłącznie dla API.** Ahrefs od $249/mies. plus units, z minimum 50
  units za każde zapytanie ([limity](https://docs.ahrefs.com/en/api/docs/limits-consumption)). Przy
  naszej skali to kilkaset dolarów miesięcznie za dane, które DataForSEO odda za kilka dolarów rocznie.
- **Osobny SaaS do rank trackingu albo platforma AI visibility za $499/mies.** Duplikują pętlę SERP
  API → SQLite → diff, którą i tak musimy mieć, żeby odpowiadać na pytanie „co się zmieniło".
- **Generyczne serwery MCP do GSC.** Żaden z siedmiu znalezionych nie ma backingu instytucjonalnego, a
  klucz z dostępem do danych firmowych nie powinien przechodzić przez niesprawdzony kod społecznościowy.
  Oficjalne MCP (Google Analytics, DataForSEO, Senuto) zostają do eksploracji ad hoc, nie do
  powtarzalnych zadań, bo nie zapisują historii.
- **llms.txt.** Google z pliku nie korzysta, a 97% opublikowanych plików nie odebrało w maju 2026 ani
  jednego żądania od jakiegokolwiek bota („97% of llms.txt files receive zero traffic in May 2026";
  o tych plikach: „No bots. No humans. Nothing fetched them at all"; próba to ok. 38 tys. domen z
  poprawnym plikiem, [badanie Ahrefs](https://ahrefs.com/blog/llmstxt-study/), 2026-06-15).

---

## 14. Otwarte pytania, które blokują pozycje z tego pliku

Uporządkowane wg tego, co blokują.

1. **Wybór wariantu SERP i akceptacja ryzyka ToS** (sekcja 1). Blokuje pozycje 1, 2, 3, 7 i 9.
   Decyzja użytkownika po przeczytaniu sekcji 1, w razie potrzeby z compliance kancelarii.
2. **Zgoda na włączenie billingu w projekcie GCP** (sekcja 5). Blokuje bulk export, a koszt zwłoki
   jest nieodwracalny, bo eksport nie robi backfillu.
3. **Podstawa prawna przechowywania zrzutów SERP zawierających dane osobowe** `[do weryfikacji]`
   z compliance. Dotyczy obu wariantów z sekcji 1, ale przy wariancie własnym dotyczy też pozyskania,
   nie tylko przechowywania.
4. **Wyjątek TDM z dyrektywy DSM i jego polskie wdrożenie a zastrzeżenie w `robots.txt`**
   `[do weryfikacji]`. Pytanie do kancelarii, nie do researchu SEO. Istotne wyłącznie dla wariantu
   własnego.
5. **Dostępność raportu Generative AI przez API GSC** (wymiar `searchAppearance`) `[do weryfikacji]`.
   Blokuje pierwszy strumień z sekcji 4. Sprawdzić empirycznie zaraz po uzyskaniu dostępu do GSC.
6. **Istnienie, zakres i cennik API Brand24** `[do weryfikacji]` u dostawcy. Blokuje wariant
   komercyjny z sekcji 7.
7. **Skuteczność „U.S. Legal Shield" SerpApi i analogicznej ochrony SearchApi.io dla podmiotu z
   Polski** `[do weryfikacji]`. Nie blokuje niczego, ale zmienia ocenę planów abonamentowych, które
   dziś odrzucam jako za drogie.
8. **Realny udział Binga w ruchu mentzen.pl** (sekcja 8). Nie blokuje wtyczki IndexNow, którą można
   włączyć od razu, blokuje sensowność budowania klienta API Bing Webmaster Tools.
