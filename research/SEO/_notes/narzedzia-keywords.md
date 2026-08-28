# Dane o frazach i SERP dla rynku polskiego

Research na 2026-08-28. Kontekst: agent SEO (Claude Code) pracujący nad mentzen.pl, WordPress, branża prawo/podatki/księgowość B2B, rynek PL.

Wszystko oznaczone `[do weryfikacji]` pochodzi ze źródeł wtórnych albo ze strony, której nie udało się przeczytać w całości. Reszta ma link do dokumentacji producenta.

Krótka rekomendacja na wstępie, żeby nie trzeba było czytać całości: dla jednej witryny i małej skali sensowny zestaw to **Google Search Console API** (darmowe, prawdziwe dane o frazach mentzen.pl) plus **DataForSEO** (pay-as-you-go, bez abonamentu, oficjalny serwer MCP) jako źródło wolumenów i SERP-ów. **Senuto** dokładam wtedy, gdy liczy się głębia polskiej bazy i gotowe raporty widoczności, bo to jedyny z tej listy zbudowany wokół polskiego Google. Semrush i Ahrefs przy jednej witrynie B2B to koszt rzędu kilkuset dolarów miesięcznie za dane, które w polskim długim ogonie i tak są rzadsze niż w Senuto.

---

## 1. Co daje

### Senuto

Polski dostawca danych SEO. Baza słów kluczowych deklarowana jako 80 mln fraz dla Polski, aktualizowana codziennie, z wolumenem, CPC, sezonowością, frazami powiązanymi, klastrami tematycznymi i pytaniami ([senuto.com/pl/baza-slow-kluczowych](https://www.senuto.com/pl/baza-slow-kluczowych/)).

Poza bazą fraz: analiza widoczności domeny w polskim Google, monitoring pozycji, audyt SEO, wykrywanie kanibalizacji, analiza SERP-ów.

Najciekawsze dla tego projektu: Senuto ma **własny zdalny serwer MCP** pod `https://mcp.senuto.com/mcp`, reklamowany wprost jako działający z Claude Code, z 23 narzędziami w 7 kategoriach: widoczność (3), audyt SEO (5), słowa kluczowe (2, w tym klastrowanie semantyczne i pytania FAQ), historia (2), rank tracker (5), SERP (4), narzędzia pomocnicze (2) ([senuto.com/pl/mcp](https://www.senuto.com/pl/mcp/)). To znaczy, że część roboty integracyjnej jest już zrobiona i agent może dostać dane bez pisania klienta HTTP.

### DataForSEO

Hurtownia danych SEO sprzedawana wyłącznie przez API, bez interfejsu do klikania i bez abonamentu. Trzy grupy endpointów istotne tutaj:

- **SERP API** zwraca surowe wyniki wyszukiwania Google dla zadanej lokalizacji i języka, z pozycjami organicznymi i elementami SERP.
- **Keywords Data API / Google Ads** to przepakowany Keyword Planner. Zwraca `search_volume`, `competition`, `competition_index`, `cpc`, `low_top_of_page_bid`, `high_top_of_page_bid` i `monthly_searches` z rozbiciem na 12 miesięcy. Do 1000 fraz na jedno zapytanie, maks. 80 znaków i 10 słów na frazę ([docs.dataforseo.com, search_volume/live](https://docs.dataforseo.com/v3/keywords_data/google_ads/search_volume/live/)).
- **DataForSEO Labs** to ich własna baza fraz i zależności między nimi. `keyword_ideas` przyjmuje do 200 fraz zalążkowych i zwraca do 1000 wyników z `search_volume`, `competition`, `cpc`, `keyword_difficulty`, `search_intent`, `avg_backlinks_info`, `monthly_searches` i kategoriami produktowymi ([docs.dataforseo.com, keyword_ideas/live](https://docs.dataforseo.com/v3/dataforseo_labs/google/keyword_ideas/live/)).

Sam producent pisze, że Labs korzysta z własnej bazy, a Google Ads API z danych Google, i że frazy znalezione ich algorytmem są "just as relevant as those from Google Keyword Planner" ([help center](https://dataforseo.com/help-center/dataforseo-labs-api-vs-google-ads-api)). Traktowałbym to jako deklarację sprzedażową, nie pomiar.

### Semrush

Duży zestaw danych o frazach i domenach, API v3 (Analytics) i v4. Raporty frazowe: `phrase_this` (przegląd frazy), `phrase_related` (powiązane), `phrase_questions` (pytania), `phrase_fullsearch` (szerokie dopasowanie), `phrase_kdi` (keyword difficulty) ([developer.semrush.com](https://developer.semrush.com/api/v3/analytics/keyword-reports/)).

### Ahrefs

API v3 z sekcjami Site Explorer, Keywords Explorer, SERP Overview, Rank Tracker, Site Audit i Brand Radar. Keywords Explorer ma 6 endpointów pod `https://api.ahrefs.com/v3/keywords-explorer`: `overview`, `volume-history`, `volume-by-country`, `matching-terms`, `related-terms`, `search-suggestions` ([docs.ahrefs.com](https://docs.ahrefs.com/en/api/reference/keywords-explorer)).

Endpoint `overview` zwraca m.in. `clicks`, `cpc`, `cps`, `difficulty`, `global_volume`, `intents`, `parent_topic`, `parent_volume`, `serp_features`, `traffic_potential`, `volume`, `volume_desktop_pct`, `volume_mobile_pct`, `volume_monthly_history` ([get-overview](https://docs.ahrefs.com/en/api/reference/keywords-explorer/get-overview)). `traffic_potential` i `parent_topic` to metryki, których pozostali dostawcy nie mają w tej formie, i to jest realna przewaga Ahrefs.

### Google Keyword Planner przez Google Ads API

`KeywordPlanIdeaService.GenerateKeywordIdeas`, wywoływane po REST jako `POST https://googleads.googleapis.com/v{N}/customers/{customerId}:generateKeywordIdeas` ([dokumentacja metody](https://developers.google.com/google-ads/api/rest/reference/rest/v18/customers/generateKeywordIdeas)). To źródło, z którego pośrednio żyją wszystkie pozostałe narzędzia. Dane są od Google, więc nie ma pośrednika, ale są przedziałowe, nie punktowe, jeśli konto Google Ads nie wydaje pieniędzy `[do weryfikacji: nie znalazłem tego wprost w aktualnej dokumentacji, to obserwacja powszechna w branży]`.

### Darmowe alternatywy

- **Google Search Console API**. Najlepsze darmowe źródło danych o frazach dla mentzen.pl, bo to prawdziwe zapytania, kliknięcia, wyświetlenia, CTR i średnie pozycje dla tej konkretnej domeny. Żaden dostawca zewnętrzny tego nie zastąpi.
- **Google autocomplete** (`suggestqueries.google.com/complete/search?client=firefox&hl=pl&gl=pl&q=...`). Endpoint nieudokumentowany i nieobjęty żadną umową. Daje sugestie, nie daje wolumenów. `[do weryfikacji: brak oficjalnej dokumentacji, użycie automatyczne jest w szarej strefie regulaminu Google]`
- **Google Trends API (alpha)**. Google ogłosiło je w lipcu 2025 na blogu Search Central. Nie udało mi się odczytać treści wpisu, WebFetch zwracał samą nawigację, więc zakres danych i sposób zapisu na listę oczekujących zostawiam jako `[do weryfikacji]` ([wpis na blogu](https://developers.google.com/search/blog/2025/07/trends-api)).
- **AlsoAsked** i podobne narzędzia do "People Also Ask". Nie zweryfikowałem, wyczerpałem budżet wyszukiwań w tej sesji. `[do weryfikacji]` Funkcjonalnie zastępuje to endpoint `phrase_questions` w Semrush, narzędzie FAQ w Senuto MCP albo parsowanie bloku PAA z SERP API DataForSEO.

---

## 2. Dostęp i auth

| Narzędzie | Auth | Bariera wejścia |
|---|---|---|
| Senuto | token z `POST https://api.senuto.com/api/users/token`, ważny 30 dni | plan Advanced lub Prime, albo dodatek API |
| Senuto MCP | logowanie przez panel Senuto, serwer `https://mcp.senuto.com/mcp` | plan z MCP |
| DataForSEO | HTTP Basic, login i hasło z `app.dataforseo.com/api-access` | rejestracja, bez abonamentu |
| Semrush | parametr `?key=<key>` w URL | plan SEO Business + osobno kupione units |
| Ahrefs | klucz API (nagłówek), `https://api.ahrefs.com/v3/...` | plan Standard i wyżej |
| Google Ads API | OAuth2 + developer token + `login-customer-id` | wniosek o poziom dostępu, weryfikacja |
| Search Console API | OAuth2 (Google Cloud project) | konto GSC z dostępem do domeny |

Kilka szczegółów, które robią różnicę przy konfiguracji:

**Senuto.** Token żyje 30 dni, więc klient musi go cache'ować i odświeżać, a nie brać przy każdym wywołaniu. Dokumentacja siedzi pod `https://docs-api.senuto.com/` (stare `docs.senuto.com` przekierowuje). To aplikacja JavaScript, więc WebFetch nie odczyta z niej listy endpointów, trzeba otworzyć w przeglądarce. Proces autoryzacji opisuje [przewodnik na wiki Senuto](https://wiki.senuto.com/pl/articles/206536-api-przewodnik).

**Google Ads API to najtrudniejszy element całego zestawu.** Są cztery poziomy developer tokena ([Access Levels and Permissible Use](https://developers.google.com/google-ads/api/docs/api-policy/access-levels)):

| Poziom | Konta | Limit operacji na dobę |
|---|---|---|
| Test Account | tylko testowe | 15 000 |
| Explorer | testowe i produkcyjne | 2 880 na produkcji |
| Basic | testowe i produkcyjne | 15 000 |
| Standard | testowe i produkcyjne | bez limitu |

Explorer, nadawany czasem automatycznie po rejestracji, **blokuje całą grupę Planning, w tym `KeywordPlanIdeaService`**. Czyli token, który dostaje się bez wniosku, nie pozwala na keyword research. Trzeba złożyć wniosek o Basic albo Standard, deklarując permissible use "Researching keywords and recommendations".

Czy sam Basic wystarcza do `KeywordPlanIdeaService`, tego nie rozstrzygnąłem. Dwa odczyty tej samej strony Google dały sprzeczne odpowiedzi, a w lutym 2026 branża raportowała zaległości w rozpatrywaniu wniosków po wprowadzeniu poziomu Explorer. `[do weryfikacji: przeczytać sekcję Permissible Use na https://developers.google.com/google-ads/api/docs/api-policy/access-levels przed składaniem wniosku]` Czas rozpatrzenia to ok. 5 dni roboczych dla Basic i ok. 10 dla Standard `[do weryfikacji]`.

**Semrush.** Klucz jedzie w query stringu, więc trafia do logów każdego proxy po drodze. Warto owinąć to własnym klientem, który nigdy nie loguje pełnego URL-a.

---

## 3. Limity i koszty

### DataForSEO

Model pay-as-you-go, płacisz za wywołanie. Ceny z ich stron cennikowych:

**SERP API, Google Organic** ([cennik](https://dataforseo.com/pricing/serp/google-organic-serp-api)). Jeden "SERP" to 10 wyników.

| Tryb | Za 1 SERP | Za 1000 SERP | Czas |
|---|---|---|---|
| Standard queue | $0.0006 | $0.60 | ok. 5 min |
| Priority queue | $0.0012 | $1.20 | do 1 min |
| Live | $0.002 | $2.00 | do 6 s |

**Keywords Data, Google Ads** ([cennik](https://dataforseo.com/pricing/keywords-data/google-ads)). Rozliczenie za zadanie, nie za frazę, a w zadaniu mieści się do 1000 fraz.

| Tryb | Za zadanie | Za 1 mln fraz | Czas |
|---|---|---|---|
| Standard queue | $0.06 | $60 | 1 do 3 godzin |
| Live | $0.09 | $90 | do 7 s |

**DataForSEO Labs** ([cennik](https://dataforseo.com/pricing/dataforseo-labs/dataforseo-google-api)): $0.012 za zapytanie plus $0.00012 za każdy zwrócony element, czyli $132 za milion fraz. Zapytanie zwracające 1000 pozycji kosztuje $0.132. Historical Rank to $0.12 za zapytanie i $0.0012 za element. Parametr `include_clickstream_data` mnoży koszt razy dwa.

Praktyczny wniosek: przy tych stawkach realistyczny miesięczny research dla jednej witryny to **kilka dolarów**. Tysiąc SERP-ów w kolejce standardowej to 60 centów. Sprawdzenie wolumenu dla 5000 fraz przez Google Ads to 30 centów. To jest rząd wielkości niższy niż jakikolwiek abonament z tej listy.

Rejestracja jest darmowa, konto dostaje $1 kredytu na testy, minimalna wpłata to $50 `[do weryfikacji: pochodzi z podsumowania wyszukiwarki, nie z cennika]`.

### Senuto

Ceny netto z [cennika](https://www.senuto.com/pl/cennik/), stan na dziś `[do weryfikacji: odczytane przez WebFetch, potwierdzić w panelu]`:

| Plan | Miesięcznie | Rocznie | API |
|---|---|---|---|
| Lite | 107,50 zł | 1 290 zł | nie |
| Basic | 224,17 zł | 2 690,04 zł | nie |
| Advanced | 457,50 zł | 5 490 zł | tak |
| Prime | 866 zł | 10 392 zł | tak |

Dodatek API dla niższych planów to 199 zł miesięcznie. Dodatek MCP na planie Advanced to 69 zł miesięcznie, w Prime wliczony. Konkretnych limitów zapytań API [przewodnik na wiki](https://wiki.senuto.com/pl/articles/206536-api-przewodnik) nie podaje.

### Semrush

API wymaga planu **SEO Business** i to jest tylko przepustka. Sam plan daje zero units, units kupuje się osobno w paczkach 2 mln, 5 mln, 10 mln albo 20 mln ([API access](https://developer.semrush.com/api/v4/get-started/api-access/)). Cen paczek Semrush nie publikuje, odsyła do sprzedaży. Business kosztuje ok. $499,95 miesięcznie przy płatności miesięcznej `[do weryfikacji: źródła wtórne]`.

Koszty raportów frazowych w units za linię odpowiedzi ([keyword reports](https://developer.semrush.com/api/v3/analytics/keyword-reports/)):

| Raport | Units za linię |
|---|---|
| `phrase_this` | 10 |
| `phrase_fullsearch` | 20 |
| `phrase_related` | 40 |
| `phrase_questions` | 40 |
| `phrase_kdi` | 50 |

Dokumentacja podaje własny przykład: 1000 fraz dla 100 domen to 1 mln units na danych bieżących i 5 mln na historycznych. Przy jednej witrynie to przepalanie budżetu.

Czy w liście baz jest `pl`, tego nie potwierdziłem, dokumentacja pokazuje tylko "US, UK, Italy, etc." `[do weryfikacji: sprawdzić sekcję Databases w developer.semrush.com]`

### Ahrefs

Rozliczenie w units. Minimalny koszt dowolnego zapytania to **50 units**. Większość metryk to 1 unit na frazę, a `volume_monthly_history` kosztuje 2 units za każdy miesiąc historii, przy minimum 50 ([get-overview](https://docs.ahrefs.com/en/api/reference/keywords-explorer/get-overview)). Rank Tracker, Management i część endpointów publicznych units nie zużywają, są też darmowe zapytania testowe.

To minimum 50 units za zapytanie ma konkretne znaczenie dla agenta: odpytywanie po jednej frazie jest 50 razy droższe niż powinno. Trzeba batchować.

Plany z [cennika Ahrefs](https://ahrefs.com/pricing): Starter $29, Lite $129, Standard $249, Advanced $449, Enterprise $1499 miesięcznie. Dostęp do API strona pokazuje przy Standard, Advanced i Enterprise (to ostatnie z adnotacją "uncapped"). Źródła wtórne twierdzą, że na początku 2026 Ahrefs rozszerzył API na plan Lite i że jest osobna subskrypcja API od $500 miesięcznie. `[do weryfikacji: nie potwierdzone na stronie Ahrefs, ahrefs.com/api/pricing przekierowuje teraz na docs.ahrefs.com]`

### Google Ads API i Search Console API

Oba bez opłat za wywołania. Google Ads: limity operacji w tabeli wyżej, liczone w przesuwającym się oknie 24 godzin. Search Console API ([limity](https://developers.google.com/webmaster-tools/limits)): Search Analytics 1200 zapytań na minutę na witrynę, 40 000 na minutę i 30 mln na dobę na projekt. Search Analytics zwraca maks. 25 000 wierszy na zapytanie i wymaga stronicowania `[do weryfikacji]`.

---

## 4. Biblioteki i przykłady integracji

### DataForSEO

Oficjalny klient Python, `pip install dataforseo-client` ([github.com/dataforseo/PythonClient](https://github.com/dataforseo/PythonClient)):

```python
from dataforseo_client import configuration as dfs_config
configuration = dfs_config.Configuration(username='USERNAME', password='PASSWORD')
```

Oficjalny serwer MCP w TypeScript ([github.com/dataforseo/mcp-server-typescript](https://github.com/dataforseo/mcp-server-typescript)):

```bash
DATAFORSEO_LOGIN=... DATAFORSEO_PASSWORD=... npx dataforseo-mcp-server@latest
```

Serwer udostępnia cztery narzędzia: indeks dokumentacji, listę sekcji, wyszukiwanie w dokumentacji i generyczne uwierzytelnione zapytanie do API. To sensowna konstrukcja, bo agent najpierw czyta dokumentację endpointu, potem go wywołuje, zamiast zgadywać kształt żądania. Działa na stdio i po HTTP (`--mode http`, port 3000).

Dla mentzen.pl i tak zbudowałbym cienką warstwę własną na tym API, żeby wymusić cache i limity kosztów, ale MCP jest dobre do eksploracji.

### Google Ads

Oficjalna biblioteka Google, `pip install google-ads`, wersja 31.4.0 z 19 sierpnia 2026, Python od 3.9 do poniżej 3.15, Apache 2.0, wydawca Google LLC ([PyPI](https://pypi.org/project/google-ads/)). Dla Node istnieje `google-ads-api`, ale to biblioteka społeczności, nie Google. `[do weryfikacji: npm blokuje odczyt, sprawdzić właściciela i datę wydania przed instalacją, zgodnie z zasadą weryfikacji zależności]`

Biorąc pod uwagę, że Python ma tu klienta od Google, a Node nie, ta część zestawu prosi się o `uv` i Pythona.

### Senuto

Brak publicznie udokumentowanego SDK. Zwykły REST plus token z 30-dniowym życiem, więc kilkadziesiąt linijek własnego klienta. Alternatywnie zdalne MCP bez pisania czegokolwiek.

### Semrush i Ahrefs

Oba to zwykły REST bez oficjalnych SDK. Semrush zwraca domyślnie CSV z separatorem `;`, Ahrefs JSON.

---

## 5. Zastosowanie dla agenta SEO

Konkretne przepływy dla mentzen.pl. Wszystkie zakładają, że agent ma lokalne narzędzia CLI i pisze wyniki do plików w repo, żeby dało się je przejrzeć i wersjonować.

**Baza tematów z własnych danych.** Raz w tygodniu ciągnąć z Search Console API zapytania z ostatnich 90 dni, filtrować te z wyświetleniami powyżej progu a pozycją 8 do 25, i zestawiać z URL-em, który je zbiera. To lista tekstów do poprawienia, oparta na prawdziwych danych, kosztująca zero. Do tego dołożyć wolumen z DataForSEO Google Ads Search Volume (do 1000 fraz w jednym zadaniu za $0.06), żeby wiedzieć, o jaką stawkę toczy się gra.

**Wykrywanie kanibalizacji.** Ta sama tabela z GSC, tyle że grupowana po zapytaniu: jeśli jedno zapytanie zbiera wyświetlenia na trzech różnych URL-ach, mamy problem. W kancelaryjnym blogu z długą historią to prawdopodobnie najczęstsza przyczyna zatrzymania wzrostu. Senuto MCP ma do tego gotowe narzędzie w kategorii audytu, więc jeśli plan z MCP i tak jest wykupiony, warto porównać wynik obu metod.

**Klastrowanie i mapa treści.** Wziąć 20 do 50 fraz zalążkowych z podatków i prawa spółek, przepuścić przez DataForSEO Labs `keyword_ideas` (200 fraz zalążkowych na zapytanie, do 1000 wyników), odfiltrować po `search_volume` i `search_intent`, pogrupować. Koszt jednego takiego przebiegu to ok. $0.13. Senuto ma do tego klastrowanie semantyczne dostrojone do polskiego, i tu podejrzewam realną przewagę, bo grupowanie polskiej odmiany po podobieństwie tekstowym wychodzi kiepsko.

**Pytania do sekcji FAQ i pod nagłówki H2.** Trzy drogi: narzędzie FAQ w Senuto MCP, `phrase_questions` w Semrush (40 units za linię, drogo), albo wyciąganie bloku People Also Ask z SERP API DataForSEO za $0.0006 za wynik. Dla polskich fraz podatkowych ta trzecia droga jest najtańsza i daje to, co Google faktycznie pokazuje dziś.

**Monitoring SERP przed publikacją i po niej.** Przed napisaniem tekstu agent pobiera 10 wyników dla frazy głównej, sprawdza kto tam jest i jaki typ treści wygrywa. Dla prawa i podatków SERP-y są zdominowane przez serwisy typu poradnikprzedsiebiorcy, infor, ifirma i strony rządowe, więc bez tego kroku łatwo napisać tekst o złym formacie. Po publikacji ten sam pomiar co tydzień. Sto fraz razy cztery tygodnie to 24 centy miesięcznie w kolejce standardowej.

**Sezonowość.** Podatki są brutalnie sezonowe: PIT od lutego do końca kwietnia, CIT do końca marca, JPK i terminy miesięczne przez cały rok, zmiany przepisów od stycznia. Pole `monthly_searches` z DataForSEO Google Ads daje 12 miesięcy historii dla każdej frazy w tym samym zapytaniu, za które i tak płacisz. Agent może z tego zbudować kalendarz publikacji, który każe zacząć pisać o rozliczeniu PIT w grudniu, nie w marcu.

**Luki wobec konkurencji.** Ranked keywords konkurenta z Labs albo raport widoczności z Senuto, odjąć własne frazy z GSC, posortować po wolumenie. Senuto jest tu mocniejsze dla PL, bo ich baza widoczności powstała pod polski rynek.

**Czego nie robić.** Nie odpytywać Ahrefs po jednej frazie, bo minimum 50 units na zapytanie zamienia 100 pojedynczych wywołań w 5000 units zamiast setki. Nie włączać `include_clickstream_data` w DataForSEO odruchowo, bo to podwaja rachunek. Nie budować monitoringu na trybie Live tam, gdzie kolejka standardowa za jedną trzecią ceny odda wynik w 5 minut.

### Jakość danych dla polskiego

Uczciwie: twardych, niezależnych porównań dokładności dla języka polskiego nie znalazłem, a te które krążą w branży są robione przez samych dostawców. Więc poniżej rozumowanie, nie pomiar.

Senuto ma jedną przewagę, której nie da się podrobić: firma jest polska, baza budowana pod polski Google, deklarowane 80 mln fraz to dużo jak na jeden rynek, a klastrowanie i obsługa odmiany są robione dla tego języka. Dla długiego ogona w rodzaju "czy fundacja rodzinna płaci CIT od najmu" spodziewałbym się, że Senuto ma frazę, a globalny dostawca jej nie widzi.

DataForSEO i Google Ads API mają dane wprost od Google przez Keyword Planner, więc dla fraz, które Keyword Planner zna, wolumen jest tak dobry jak u każdego. Słabszą stroną jest to, że Keyword Planner grupuje bliskie warianty i podaje przedziały, a odmiana polska tworzy tych wariantów mnóstwo. Baza Labs dla Polski wymaga sprawdzenia, dokumentacja mówi o ok. 90 lokalizacjach i odsyła do pliku CSV, którego nie udało mi się pobrać. `[do weryfikacji: pobrać listę lokalizacji Labs i potwierdzić Polskę oraz rozmiar bazy PL]`

Semrush i Ahrefs mają najlepsze narzędzia analityczne i najgorszy stosunek ceny do przydatności przy jednej polskiej witrynie B2B. `traffic_potential` i `parent_topic` z Ahrefs są naprawdę użyteczne przy planowaniu treści, ale $249 miesięcznie plus units za coś, co przy tej skali będzie odpalane kilka razy w tygodniu, trudno obronić.

Dla DataForSEO potwierdzone parametry dla Polski to `location_name: "Poland"` i `language_code: "pl"` w endpointach Google Ads. Odczyt sugerował `location_code: 616`, ale DataForSEO używa identyfikatorów geo Google, gdzie Polska ma 2616, a USA w dokumentacji ma 2840. `[do weryfikacji: sprawdzić GET /v3/keywords_data/google_ads/locations z filtrem country "pl", zanim kod trafi do konfiguracji]` Ahrefs przyjmuje zwykłe ISO 3166-1 alpha-2, więc `country=pl`.

---

## 6. Wymagana konfiguracja po stronie użytkownika

Kolejność od najmniejszego wysiłku do największego.

**Search Console API, zacząć od tego.** Projekt w Google Cloud, włączona Search Console API, ekran zgody OAuth, dane klienta typu Desktop, jednorazowa autoryzacja kontem z dostępem do mentzen.pl w GSC i zapisany refresh token. Zero kosztów, dane własne, bez negocjacji z kimkolwiek.

**DataForSEO.** Rejestracja na `app.dataforseo.com`, odebranie loginu i hasła API z `app.dataforseo.com/api-access`, doładowanie konta. Zaczęłbym od kwoty minimalnej, bo przy tych stawkach starczy na miesiące. Warto od razu ustawić limit wydatków, jeśli panel to pozwala, żeby błąd w pętli agenta nie skończył się rachunkiem.

**Senuto, jeśli budżet pozwala.** Plan Advanced albo Prime, albo niższy plus dodatek API za 199 zł. Jeżeli agent ma z tego korzystać przez MCP, dołożyć dodatek MCP (69 zł na Advanced) i podłączyć `https://mcp.senuto.com/mcp` w konfiguracji Claude Code, autoryzując przez panel Senuto. Token REST-owy, gdyby jednak pisać własnego klienta, wygasa po 30 dniach i wymaga odświeżania.

**Google Ads API, najdłuższa ścieżka.** Potrzebne jest konto Google Ads (może być bez wydatków, ale wtedy dane będą przedziałowe), konto menedżerskie MCC do wygenerowania developer tokena, wniosek o poziom **Basic lub Standard** z deklaracją permissible use "Researching keywords and recommendations", i osobne dane OAuth2. Poziom Explorer, który Google nadaje czasem automatycznie, nie wystarczy, bo blokuje `KeywordPlanIdeaService`. Czas oczekiwania na rozpatrzenie liczy się w dniach roboczych, a w lutym 2026 branża raportowała zaległości. Wniosek warto złożyć wcześnie, nawet jeśli agent wystartuje na DataForSEO.

**Semrush albo Ahrefs, tylko jeśli któryś już jest opłacony.** Semrush wymaga planu SEO Business plus osobnego zakupu paczki units w Subscription info, potem klucz z `semrush.com/accounts/api-keys/active`. Ahrefs wymaga planu Standard lub wyżej i wygenerowania klucza API w ustawieniach.

**Sekrety.** Wszystkie klucze do zmiennych środowiskowych albo pliku poza repo, nigdy do CLAUDE.md ani do kodu. Klucz Semrush jedzie w query stringu, więc własny klient nie powinien logować pełnych URL-i.

---

## Źródła

- [Senuto: przewodnik po API](https://wiki.senuto.com/pl/articles/206536-api-przewodnik)
- [Senuto: MCP](https://www.senuto.com/pl/mcp/)
- [Senuto: cennik](https://www.senuto.com/pl/cennik/)
- [Senuto: baza słów kluczowych](https://www.senuto.com/pl/baza-slow-kluczowych/)
- [DataForSEO: cennik SERP Google Organic](https://dataforseo.com/pricing/serp/google-organic-serp-api)
- [DataForSEO: cennik Keywords Data Google Ads](https://dataforseo.com/pricing/keywords-data/google-ads)
- [DataForSEO: cennik Labs Google API](https://dataforseo.com/pricing/dataforseo-labs/dataforseo-google-api)
- [DataForSEO: Labs vs Google Ads API](https://dataforseo.com/help-center/dataforseo-labs-api-vs-google-ads-api)
- [DataForSEO: search_volume/live](https://docs.dataforseo.com/v3/keywords_data/google_ads/search_volume/live/)
- [DataForSEO: keyword_ideas/live](https://docs.dataforseo.com/v3/dataforseo_labs/google/keyword_ideas/live/)
- [DataForSEO: lokalizacje Google Ads](https://docs.dataforseo.com/v3/keywords_data/google_ads/locations/)
- [DataForSEO: oficjalny klient Python](https://github.com/dataforseo/PythonClient)
- [DataForSEO: oficjalny serwer MCP](https://github.com/dataforseo/mcp-server-typescript)
- [Semrush: dostęp do API](https://developer.semrush.com/api/v4/get-started/api-access/)
- [Semrush: raporty keyword i koszty units](https://developer.semrush.com/api/v3/analytics/keyword-reports/)
- [Semrush: dokumentacja podstawowa Analytics](https://developer.semrush.com/api/v3/analytics/basic-docs/)
- [Ahrefs: Keywords Explorer API](https://docs.ahrefs.com/en/api/reference/keywords-explorer)
- [Ahrefs: endpoint overview](https://docs.ahrefs.com/en/api/reference/keywords-explorer/get-overview)
- [Ahrefs: cennik](https://ahrefs.com/pricing)
- [Google Ads API: poziomy dostępu i permissible use](https://developers.google.com/google-ads/api/docs/api-policy/access-levels)
- [Google Ads API: generateKeywordIdeas](https://developers.google.com/google-ads/api/rest/reference/rest/v18/customers/generateKeywordIdeas)
- [Google Ads: oficjalna biblioteka Python](https://pypi.org/project/google-ads/)
- [Search Console API: limity](https://developers.google.com/webmaster-tools/limits)
