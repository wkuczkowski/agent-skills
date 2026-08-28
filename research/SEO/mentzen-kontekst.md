# Mentzen.pl — kontekst serwisu dla skilla SEO

Stan na 2026-08-28. Wszystkie obserwacje pochodzą z badania serwisu z tego dnia (sitemapy Yoast, HTML stron, REST API WordPressa `/wp-json/wp/v2/`), o ile nie zaznaczono inaczej. To opis stanu faktycznego i wnioski operacyjne dla skilla — nie audyt z listą napraw.

## TL;DR

1. Serwis jest **mały po stronie oferty (17 stron usług + 5 produktów subskrypcyjnych), duży po stronie bloga (523 wpisy + 33 interpretacje)** — a te dwie warstwy są niemal niepołączone linkami: 77% wpisów nie ma ani jednego linku wewnętrznego w treści.
2. Model biznesowy to **abonament jako ścieżka domyślna** (Mentzen+ 699–3199 zł netto/mc wg progów przychodu klienta) plus usługi projektowe wyceniane po kontakcie. Skill ma myśleć frazami „stała obsługa prawno-podatkowa / księgowość dla spółki z o.o.", nie tylko „usługa X cena".
3. **Konwersja domyka się poza domeną i za JS-em**: kalendarz Calendesk (iframe SPA na `doradztwo.mentzen.pl`), kalkulatory wyceny Prime/Kadry, panele klienta. Dla robota cennik konsultacji i pół oferty nie istnieje — cel strony trzeba oceniać po CTA, nie po widocznej transakcji.
4. **Jedna lokalizacja stacjonarna: Toruń** (potwierdzone przez użytkownika, 2026-08-28): klienci mogą przyjść na konsultację na miejscu, większość korzysta z usług zdalnie; oddziały Warszawa/Gdańsk/Poznań nie istnieją. W serwisie brak stron miast i `LocalBusiness`. Lokalne SEO to wyłącznie domknięty moduł „Toruń + reputacja" (`local-seo.md`) — stron miast skill nie proponuje nigdy.
5. **Fraza „mentzen" ma mieszaną intencję** (polityk Sławomir Mentzen dominuje SERP). Osią mapy fraz brandowych są modyfikatory i nazwy produktów: „kancelaria mentzen", „mentzen+ cena", „mentzen prime", „start z mentzenem" — unikalne encje bez kolizji z polityką.
6. **E-E-A-T jest silne na poziomie autora, słabe na poziomie organizacji**: imienne autorstwo z tytułami zawodowymi, `Person` w schemacie, archiwa autorów, sygnatury interpretacji KIS w treści — ale brak „O nas", brak `sameAs`/`founder`/adresu w `Organization`, brak liczb w social proof.
7. Wzorzec redakcyjny bloga do zachowania: **H2 jako pytania klienta** + cytowanie przepisów i sygnatur. Do zmiany w nowych treściach: mediana 479 słów (2025+), płaska struktura H2, prawie brak FAQ, brak linków kontekstowych.
8. Ton marki jest celowo żartobliwy i **gra przeciw kategorii** — skill nie prostuje tonu, ale rozdziela role: żart w leadzie, fraza w nagłówku.
9. Nieporządki strukturalne, o których skill musi wiedzieć zanim coś zaproponuje: podwójne archiwa kategorii (`/blog/<kat>/` vs `/blog/category/<kat>/`), kolizja sluga podkategorii z wpisem, osierocone CPT `interpretacje` (33 URL-e bez linku z nawigacji), pusty robots.txt bez `Sitemap:`.
10. Regulaminy zdradzają usługi bez stron sprzedażowych (Mentzen Nieruchomości, spółki S24, pakiet „za 2 zł", Legalny Mentzen) — gotowa lista kandydatów na nowe landingi, ale status produktów [do weryfikacji] u użytkownika.
11. **YouTube: duże, porzucone i odcięte aktywo** — kanał firmowy @kancelariamentzen (491 filmów, 6,2 mln wyświetleń) milczy od 2026-06-13 i jest niepowiązany z domeną (brak `sameAs`, zero embedów w artykułach, 1/60 opisów linkuje do bloga), a kanał osobisty Sławomira Mentzena (1,09 mln subskrybentów) nie linkuje do mentzen.pl wcale. Szczegóły i priorytety: sekcja 8.

## 1. Firma i model biznesowy

Kancelaria Mentzen sp. z o.o. (NIP 9562371350, KRS 0001071605), doradztwo podatkowe + prawne + księgowość, wyłącznie B2B, rynek polski, jedna lokalizacja stacjonarna: Amicus Business Park, ul. Grudziądzka 110-114/101, 87-100 Toruń (https://mentzen.pl/dane-kontaktowe/, 2026-08-28; potwierdzone przez użytkownika 2026-08-28). Klienci mogą przyjść na konsultację stacjonarnie w Toruniu; większość korzysta z usług zdalnie, obsługa obejmuje całą Polskę. Oddziały w innych miastach nie istnieją. Część grupy Mentzen S.A. — relacje inwestorskie na osobnym serwisie https://corporate.mentzen.pl/ (linkowany z menu, poza zakresem skilla; zapytania „mentzen akcje", „mentzen S.A." kieruj tam, nie na mentzen.pl).

Dwa modele sprzedaży, oba w menu głównym:

- **Subskrypcje** (sprzedawane jak SaaS, eksponowane jako pierwsze): Mentzen+ (/mentzen-plus/), Mentzen+ IT (/mentzen-it/), Mentzen Prime — księgowość (/mentzen-prime/), Kadry Mentzena (/kadry-mentzena/), Ewidencja IP BOX (/ewidencja-ip-box/).
- **Usługi projektowe** (sprzedawane przez konsultację): huby /doradztwo-podatkowe/ i /doradztwo-prawne/ + 14 podstron, plus wejście dla nowych firm /start-z-mentzenem-zakladanie-firmy/ (699 zł brutto — jedyna jednorazowa usługa z jawną ceną).

Segmentacja klientów jest trzyosiowa i skill powinien ją odwzorować przy doborze fraz:

1. **Przychód** — jawna w cenniku Mentzen+: Standard do 1 mln, Premium do 10 mln, Pro powyżej 10 mln zł/rok (699/1299/2499 zł netto Podatki; 999/1899/3199 zł Prawo i Podatki) — https://mentzen.pl/mentzen-plus/ (2026-08-28). Klient docelowy: od mikrofirmy po średnią.
2. **Etap życia firmy** — zakładanie (Start z Mentzenem) → prowadzenie (subskrypcje) → restrukturyzacja/sukcesja/fundacja rodzinna.
3. **Branża** — jedyna wyodrębniona to IT (Mentzen+ IT wymienia role: programista, PM, PO, DevOps, tester, UX/UI…). To gotowy szablon, gdyby skill proponował strony branżowe.

Poza komunikacją: konsument/osoba prywatna (wyjątki: kryptowaluty, częściowo fundacja rodzinna).

**Cennik widoczny dla robota**: pełna macierz Mentzen+ i 699 zł za Start z Mentzenem. Wszystko inne — kalkulatory (Prime, Kadry) albo wycena indywidualna. Cena konsultacji jest tylko w kalendarzu Calendesk, czyli niewidoczna dla wyszukiwarki.

## 2. Struktura serwisu i URL-e

Stack: WordPress + Divi, Yoast SEO, Cloudflare. Tylko `pl-PL`, brak hreflang. 601 URL-i w sitemapach (https://mentzen.pl/sitemap_index.xml, 2026-08-28):

| Typ | Wzorzec | Liczba |
|---|---|---:|
| Wpisy bloga | `/blog/<kat>[/<podkat>[/…]]/<slug>/` | 522 |
| Strony | `/<slug>/` (płaskie, 1 segment) | 44 |
| Interpretacje (CPT) | `/blog/interpretacje/<slug>/` | 33 |
| Praktyka (CPT martwy) + snippet Divi | — | 2 |

Fakty, które zmieniają decyzje skilla:

- **robots.txt nie zawiera ani jednej dyrektywy** (tylko komentarze Cloudflare Content Signals) — brak `Sitemap:`, brak blokad. Sitemapy trzeba znajdować konwencją (`/sitemap_index.xml`).
- **URL-e usług są płaskie** (`/vat/`, `/cit-estonski/`, `/sukcesja/`) — brak sygnału hierarchii w URL-u; całe linkowanie hub→dziecko niosą strony /doradztwo-podatkowe/ i /doradztwo-prawne/.
- **Trzy strony obsługują dwie intencje naraz**: /fundacja-rodzinna/, /sukcesja/, /restrukturyzacja-dzialalnosci/ są linkowane z obu hubów (podatkowego i prawnego). Przy optymalizacji fraz skill musi jawnie zdecydować, którą intencję dana strona targetuje, żeby nie kanibalizować.
- **Menu miesza osie oferty**: pozycja „Księgowość" w Usługach prowadzi do produktu /mentzen-prime/, nie do strony usługowej. Nie istnieje usługowa strona księgowości.
- **Brak warstwy pośredniej** między hubem a wpisem: zero stron pillar/„wszystko o…", brak breadcrumbów widocznych na wpisie.
- **Brak zasobów lead-magnet jako stron**: 404 na /faq/, /baza-wiedzy/, /kalkulatory/, /wzory-dokumentow/, /webinary/. Kalkulatory istnieją, ale osadzone w stronach usług (np. kalkulator estońskiego CIT na https://mentzen.pl/cit-estonski/).
- **16 z 44 stron to regulaminy** — i to one ujawniają usługi bez landingów: Mentzen Nieruchomości (https://mentzen.pl/regulamin-uslugi-mentzen-nieruchomosci/; w kalkulatorze Prime są pola `realestatebasic…realestatevip`), zakładanie spółek S24, promocja „Mentzen+ Podatki-Prawo za 2 zł", „Księgowość na start", pakiety godzinowe „Legalny Mentzen" (tylko w panelu logowania). Status tych produktów [do weryfikacji] — zanim skill zaproponuje dla nich landingi, potwierdź u użytkownika, że produkty żyją.
- **Zero stron lokalizacyjnych** (404 na /biuro-rachunkowe-torun/, /doradca-podatkowy-warszawa/ itd.) — zgodnie ze stanem faktycznym: jedyna stacjonarna lokalizacja to Toruń, oddziały Warszawa/Gdańsk/Poznań **nie istnieją** (potwierdzone przez użytkownika 2026-08-28). Zewnętrzne wizytówki, które je wymieniają (np. https://www.trojmiasto.pl/Kancelaria-Mentzen-o94675.html), oraz wzmianka o zespole w Poznaniu w biogramie na stronie zespołu zawierają dane nieaktualne — do sprzątnięcia w ramach spójności NAP/encji (`local-seo.md`, sekcja 5). Lokalne SEO ogranicza się do modułu „Toruń + reputacja" z `local-seo.md`.
- **Osierocone sekcje**: CPT `interpretacje` — 33 krótkie (400–500 słów) omówienia wygranych spraw z sygnaturami i podpisem specjalisty, czyli de facto case studies — nie ma żadnego linku z nawigacji ani z /blog/ (archiwum `/blog/interpretacje/` → 404), ostatnia aktualizacja 2025-02. Najsilniejszy dowód kompetencji w serwisie jest niewidoczny dla użytkownika.
- **Podwójne archiwa kategorii**: wpisy żyją pod `/blog/<kat>/<slug>/`, ale nawigacja linkuje archiwa pod `/blog/category/<kat>/`; oba wzorce zwracają 200 z tym samym `<title>`. Dodatkowo `/blog/doradztwo-podatkowe/ulgi-podatkowe/` robi 301 na konkretny wpis (kolizja slugów). Archiwa kategorii mają `noindex, follow` — pełnią funkcję nawigacyjną, nie są landingami SEO. Skill, generując linki do kategorii, używa wariantu `/blog/category/…` (ten bez kolizji) i nie traktuje archiwów jako celów pozycjonowania.
- **Dane strukturalne**: home ma tylko Yoast-owe `Organization` (bez `sameAs`, `founder`, `address`, `telephone`), `WebSite`, `WebPage`, `BreadcrumbList`, `ImageObject`. Brak `LegalService`/`AccountingService`/`LocalBusiness`, brak `Service`, brak `FAQPage` mimo sekcji FAQ na stronach subskrypcji, brak `Person` na /nasz-zespol/.

## 3. Strony usługowe — wzorzec treści

Powtarzalny szablon (przykład: https://mentzen.pl/cit-estonski/, ~1800–2000 słów): H1 z hasłem sprzedażowym → korzyści → formularz → kwalifikacja leada („Kto będzie mógł skorzystać…") → zakres pomocy → kalkulator → blok linków do bloga → dowód społeczny („Mówili o nas" — zdjęcia przedsiębiorców z 2022 r., „Pisali o nas" — logotypy Wprost, Rzeczpospolita, Business Insider, Money, Strefa Inwestorów) → trzech imiennych specjalistów z „Umów konsultację".

Język stron usługowych: ból klienta w drugiej osobie („Zwiększ Zyski, Zmniejsz Podatki!", „Zabezpiecz przyszłość swojej firmy"), formuła „zamiast X, rób Y", proporcja ~70% korzyść / 30% merytoryka. Liczby i przepisy występują jako uwiarygodnienie (np. na /cit-estonski/ stawki efektywne 20% vs 26,3%), nie jako treść główna. Nagłówki prawie nigdy nie są rzeczownikowo-frazowe — **strony usługowe są ubogie w treść pod frazy problemowe; długi ogon łapie blog**. FAQ jest tylko na stronach subskrypcji, cen brak poza subskrypcjami.

Wniosek dla skilla: **utrzymuj dwa osobne szablony treści** — sprzedażowy dla strony usługowej (gdzie fraza główna musi wejść do H1/H2 bez zabijania stylu) i merytoryczny dla wpisu. Nie przenoś poradnikowej głębi na strony usług ani sprzedażowego tonu na bloga.

## 4. Blog

Źródło: REST API WP (523 wpisy z pełną treścią) + HTML wpisów, 2026-08-28.

**Skala i taksonomia**: 523 wpisy, 39 kategorii (drzewo do 3 poziomów), 1547 tagów (~3/wpis, w większości jednorazowe — bez wartości nawigacyjnej; jest tag operacyjny `wylaczonewyszukiwanie`, 38 wpisów). Rozkład głównych kategorii w URL-ach: doradztwo-podatkowe 306, doradztwo-prawne 107, inne 77, księgowość 32. **Kategorie bloga z grubsza odwzorowują działy oferty** — wspólne nazwy z landingami mają: VAT, CIT estoński, optymalizacja podatkowa, fundacja rodzinna, kryptowaluty; reszta (ulgi-podatkowe, ryczałt, nowy-lad, zatrudnienie) nie mapuje się 1:1 na slugi usług. 9 kategorii ma 0 wpisów (m.in. „Składka zdrowotna", „Spółka komandytowa", „Doradztwo podatkowe dla budownictwa") — taksonomię zaprojektowano szerzej, niż ją zapełniono. Brak archiwów tagów.

**Tematy (2025–2026)**: przekwalifikowanie B2B ↔ umowa o pracę i kompetencje PIP, fundacja rodzinna, CIT estoński, klauzula GAAR, ulgi B+R/IP Box, KSeF 2026, kadry, nisze przychodowe (prop trading, krypto, najem). Dominuje **treść reaktywna** — wywołana zmianą prawa, interpretacją KIS lub wyrokiem NSA. Evergreeny poradnikowe pochodzą z 2021–2022 i nie są aktualizowane (np. https://mentzen.pl/blog/doradztwo-podatkowe/skladka-zdrowotna-wszystko-co-powinienes-wiedziec/ z 2022 r. nadal operuje płacą minimalną 3010 zł). Zero treści lifestyle i zero polityki — profil tematyczny jest wąski i czysty; skill go pilnuje.

**Tempo**: szczyt 2022 (~12 wpisów/mc, 141 rocznie), spadek do 3–4/mc w 2026 (28 wpisów do 28.08); mediana odstępu 5 dni, najdłuższa przerwa 37 dni. Kadencja nieregularna.

**Typowy wpis** (pomiary na całym korpusie): mediana 549 słów (479 dla 2025+), mediana 4 × H2, H3 praktycznie brak — płaska struktura. Schemat: lead bez nagłówka (często definicja) → 4–6 H2 **sformułowanych jako pytania klienta** („Kto płaci zaliczki?", „Gdzie fiskus poluje najczęściej?") → baner graficzny CTA do /mentzen-plus/ wklejony po 2.–4. sekcji (bez alt-tekstu; w 40 ze 115 wpisów 2025+) → H2 „Podsumowanie"/„Co warto zapamiętać?" → opcjonalnie lista sygnatur interpretacji KIS lub wideo z YouTube (53 wpisy). FAQ mają tylko 3 wpisy, żaden nie ma schematu `FAQPage`. Tabele: 9 wpisów w całym korpusie. Metadane: mediana title 72 znaki, meta description 139 znaków, konsekwentnie zakończone wezwaniem („Sprawdź zasady opodatkowania.").

**Slugi i tytuły** bywają żartobliwe i oderwane od fraz („piekne-paznokcie-w-kosztach-uzyskania-przychodu", „inni-lekarze-go-nienawidza-placi-85-ryczaltu-od-przychodow"); wpisy z 2026 r. są chłodniejsze i bardziej „interpretacyjne" niż publicystyka 2022–2024.

**Autorzy i E-E-A-T**: 144 autorów na 523 wpisy; od 2025 aktywnych 42, nikt nie przekracza 6 wpisów — rozproszenie tak duże, że nikt nie buduje sygnatury tematycznej. Co działa: box autora ze zdjęciem, nazwiskiem i konkretnym stanowiskiem z tytułem zawodowym („Radca Prawny, Dział Doradztwa Prawnego"), CTA „Umów konsultację" z deep-linkiem Calendesk do tej osoby, schema `Person` z archiwum autora (indeksowalne, np. https://mentzen.pl/blog/author/kacper-boron/), cytowanie źródeł pierwotnych (artykuły ustaw, sygnatury KIS z datami, wyroki NSA) — najmocniejszy sygnał eksperckości. Czego brakuje: biogramów w boxie, linku z boxu do archiwum autora i profilu na /nasz-zespol/, `Person.description` dla ~1/3 autorów (w tym większości aktywnych w 2025–2026), `sameAs` przy `Person`.

**Nawigacja bloga**: lista wpisów na /blog/ renderuje się po stronie klienta — HTML serwera nie zawiera ani jednego linku do wpisu. `/blog/page/2/` ma canonical na `/blog/`. Breadcrumbs w schemacie tylko dwupoziomowe (Strona główna → tytuł wpisu, bez ogniwa „Blog" i bez kategorii; sprawdzone na 3 wpisach 2026-08-28).

## 5. Linkowanie wewnętrzne — najsłabszy element

W treści 523 wpisów jest łącznie **158 linków wewnętrznych** (0,3/wpis, mediana 0); **403 wpisy (77%) nie linkują nigdzie**, wśród 2025+ nadal 61%. Z tych 158 większość to ten sam baner do /mentzen-plus/ (36) i /mentzen-prime/ (6). Landingi /fundacja-rodzinna/, /vat/, /kryptowaluty/, /sukcesja/, /optymalizacja-podatkowa/, /kontrola-i-postepowanie-podatkowe/ **nie dostają z treści bloga ani jednego linku**, mimo że mają na blogu dedykowane serie wpisów. Linkowanie wpis→wpis jest szczątkowe — serie (3 wpisy o GAAR, 3 o przekwalifikowaniu B2B) nie linkują się nawzajem. Sekcja „Zobacz również" pod wpisem to **stała, identyczna lista 4 wpisów na całym blogu** (kategoria „Najczęściej czytane"), nie rekomendacje. W treści siedzą stare adresy na `www.mentzen.pl` sprzed migracji, URL-e ze spacją/łamaniem wiersza i literówka `/blog/doardztwo-prawne/`.

To jest **pierwszy i najtańszy kierunek pracy skilla**: przy każdym nowym lub edytowanym wpisie dodawaj linki kontekstowe (z frazy w zdaniu, nie baner) do właściwego landinga usługowego i do wpisów z tej samej serii. Mapowanie kategoria→usługa jest gotowe i mechaniczne.

## 6. Marka, brand search, ton

**Sławomir Mentzen** jest rozpoznawalny przede wszystkim jako polityk (Konfederacja, kandydat prezydencki 2025) i twórca internetowy. Serwis opiera na nazwisku całą architekturę nazewniczą (Mentzen+, Mentzen Prime, Start z Mentzenem, Kadry Mentzena), ale **osoby prawie nie eksponuje**: na home zero wzmianek, jedyne miejsce to żartobliwa karta „Prezes Zarządu" na /nasz-zespol/ bez dorobku zawodowego, z przyciskami „Obserwuj" prowadzącymi do jego prywatnych profili (facebook.com/slawomirmentzen, instagram.com/slawomirmentzen, youtube.com/c/SławomirMentzen). Home i strony usługowe nie mają żadnych linków do social mediów, ale stopka wpisu blogowego linkuje do profili kancelarii (Facebook, Instagram, X, https://www.youtube.com/@kancelariamentzen) — sprawdzone 2026-08-28. `Organization` bez `sameAs` i `founder` — dowiązanie firmy do osoby istnieje wyłącznie przez nazwę.

Konsekwencje dla skilla:

- Traktuj „mentzen" i „sławomir mentzen" jako **frazy o mieszanej intencji zdominowane przez encję-polityka** — mentzen.pl konkuruje tam o pojedyncze miejsce, nie o ekran. Nie raportuj ich jako zwykłego brandu.
- Oś mapy fraz brandowych to **modyfikatory i nazwy produktów**: „kancelaria mentzen", „mentzen opinie", „mentzen+ cena", „mentzen prime księgowość", „mentzen ip box", „start z mentzenem". Nazwy produktów są czystymi encjami bez kolizji z polityką.
- Zapytania inwestorskie („mentzen akcje", „mentzen S.A.") należą do corporate.mentzen.pl — rozdziel je w mapie fraz.
- „mentzen opinie" miesza opinie o polityku i o usłudze, a serwis nie ma własnej strony z opiniami klientów — nie ma czym wypełnić tej przestrzeni. Social proof w serwisie to twarze i nazwiska (karuzela przedsiębiorców z 2022 r., logotypy mediów bez linków), **zero liczb** („X klientów", „X lat", oceny) — dowód osobowy zamiast liczbowego.

**Ton**: home jest pastiszowo-żartobliwy („Nic tak na świecie nie pachnie, jak zapach niepłaconych podatków o poranku…"), strony usługowe konwencjonalno-doradcze, blog rzeczowy. Brak strony „O nas" (`/o-nas/` → 404). Ton jest wyróżnikiem marki i **skill go nie prostuje** — reguła robocza: żart może żyć w leadzie i slugach kampanijnych, ale nagłówki sekcyjne (H2) i metadane niosą frazę. Obecnie H1/H2 na home i landingach niosą prawie zero fraz — to świadomy koszt stylu, który skill kompensuje w treści, nie przez zmianę głosu marki.

## 7. Zastosowanie dla skilla SEO — reguły operacyjne

1. **Dwa szablony treści, nigdy jeden**: strona usługowa (sprzedażowa, fraza w H1/H2, FAQ + `FAQPage`, kwalifikacja leada) vs wpis blogowy (merytoryczny, H2-pytania, sygnatury i przepisy, link kontekstowy do usługi). Nie mieszaj rejestrów.
2. **Każdy wpis linkuje**: minimum jeden kontekstowy link do landinga usługowego wg mapy kategoria→usługa i linki do wpisów z tej samej serii. To domyślne zachowanie przy każdej edycji, nie osobny projekt.
3. **Klastry zamiast pojedynczych wpisów**: istniejące serie (GAAR, B2B/PIP, fundacja rodzinna, CIT estoński, KSeF) traktuj jako zalążki klastrów; brakuje stron filarowych — warstwy „wszystko o X" między hubem usługi a wpisami.
4. **Frazy transakcyjne pod model abonamentowy**: „stała obsługa prawna firmy", „księgowość dla spółki z o.o. cena", „doradca podatkowy abonament" — nie tylko frazy per-usługa.
5. **Kanibalizacja**: przed optymalizacją fundacji rodzinnej, sukcesji i restrukturyzacji ustal, czy strona targetuje intencję podatkową czy prawną; przed dodaniem treści sprawdź, czy tematu nie obsługuje już wpis, interpretacja lub landing.
6. **YMYL**: podatki/prawo to pełny YMYL — każda nowa treść z imiennym autorem posiadającym tytuł zawodowy, z przepisami i sygnaturami; dociążaj `Person` (description, `sameAs`, link box→archiwum→zespół) i `Organization` (sameAs, address, founder, typ LegalService/AccountingService) — dziś to najsłabsze ogniwo E-E-A-T.
7. **Aktualizacja przed produkcją nowego**: evergreeny 2021–2022 z nieaktualnymi kwotami mają istniejące URL-e i historię — odświeżenie ich zwykle bije nowy wpis. Daty modyfikacji w schemacie odzwierciedlają realne edycje — utrzymuj to.
8. **Licz się z niewidzialnością konwersji**: ceny konsultacji, kalkulatory i kalendarz są w SPA/iframe poza indeksem. Oceniając stronę, patrz na CTA (Calendesk deep-link, formularz CF7, telefon), nie szukaj transakcji w HTML; kluczowe informacje ofertowe (ceny, zakres) muszą trafiać do indeksowalnego HTML-a landinga, jeśli mają rankować.
9. **Nie ruszaj bez pytania**: lokalne SEO poza modułem „Toruń + reputacja" z `local-seo.md`, ton marki, landingi dla produktów znanych tylko z regulaminów (status [do weryfikacji]), scalanie podwójnych archiwów kategorii (zmiana strukturalna, nie treściowa).
10. **Higiena przy okazji edycji**: poprawiaj napotkane linki na `www.mentzen.pl`/z literówkami, dodawaj alt do banerów CTA, nie twórz nowych tagów (1547 istniejących to szum), nie publikuj do pustych kategorii bez decyzji o ich sensie.
11. **Interpretacje jako zasób**: 33 case studies z sygnaturami to najlepszy materiał dowodowy serwisu — przy pracy nad treścią z danego tematu linkuj do pasującej interpretacji; propozycję przywrócenia sekcji do nawigacji zgłoś użytkownikowi jako osobną decyzję.

## 8. Kanał YouTube

Źródło: `_notes/mentzen-youtube.md` (audyt 2026-08-28, dane z bezpośredniego pobrania stron YouTube; tam pełne liczby, metodologia i flagi [do weryfikacji]).

- **Skala**: kanał firmowy @kancelariamentzen (ID `UCe-iBx3MWE3B4wAAdC3BDQw`, zał. 2020) — 491 filmów (238 long-form), 16,8 tys. subskrybentów, 6,2 mln wyświetleń, 9 playlist. Tematyka pokrywa się z korpusem bloga (formy opodatkowania, składka zdrowotna, fundacja rodzinna, CIT estoński, IP Box, KSeF, VAT, spółki, krypto) — prowadzona jednak całkowicie niezależnie od serwisu.
- **Kanał porzucony**: ostatnia publikacja 2026-06-13 (potwierdzona RSS-em); tempo spadło z ok. 5 filmów/mc w 2025 r. do 1–3. Przed wznowieniem publikacji zbudować bufor nagrań.
- **Zasięg robią newsjacking i marka, nie evergreen**: mediana ok. 1 tys. wyświetleń, 47% katalogu poniżej tysiąca; poradniki o wysokim potencjale frazowym siedzą w 400–1500 wyświetleń.
- **Optymalizacja leży w warstwie tekstowej**: tytuły rozjeżdżają się z frazami realnie wyszukiwanymi (oba trafienia kanału w top 15 wyszukiwarki YouTube to filmy z frazą dosłownie w tytule — problemem jest nazewnictwo, nie autorytet); opisy to samo CTA bez akapitu streszczającego; 1/60 opisów linkuje do artykułu na blogu; zero tagów od 2025-03; rozdziały w 28% filmów; napisy wyłącznie ASR (dobrej jakości).
- **Izolacja od domeny — najważniejsze ustalenie**: serwis nie linkuje do kanału nigdzie poza stopką wpisu blogowego, zero embedów w sprawdzonych artykułach, brak `sameAs` w `Organization`. Kanał osobisty Sławomira Mentzena (1,09 mln subskrybentów, 389,7 mln wyświetleń; poz. 1 w wyszukiwarce YouTube na „fundacja rodzinna podatki" i „cit estoński") nie ma **żadnego** linku do mentzen.pl. Wzmianki na YouTube korelują najsilniej ze wszystkich badanych sygnałów z widocznością marki w AI (Ahrefs 2025, Spearman 0,712–0,740 — za `_notes/mentzen-youtube.md`, sekcja 6; korelacja, nie przyczynowość).
- **Priorytety wg stosunku efektu do kosztu** (sekcja 7 notatki): (A) link do mentzen.pl na kanale osobistym — decyzja właścicielska, wyłącznie do zaproponowania użytkownikowi; (B) przywrócenie kadencji; (C) tytuły pod frazy (fraza na początku, hook po myślniku, seria na końcu); (D) nowy szablon opisu (streszczenie z frazą, link do bliźniaczego artykułu, rozdziały, „stan prawny na dzień"); (E) `sameAs` + embedy z `VideoObject` na blogu; (F) transkrypcje ASR → wpisy blogowe (najtańsze źródło treści; narzędzie w `~/projects/yt-transcript`).

Reguła operacyjna dla skilla: przy każdym nowym lub aktualizowanym wpisie sprawdź, czy istnieje film o tym temacie — jeśli tak, osadź go w artykule i dodaj link zwrotny w opisie filmu. Para film+artykuł dziś praktycznie nie istnieje (1/60) i to jest najtańsze domknięcie pętli treść–wideo.

## Źródła

Notatki źródłowe (badanie 2026-08-28, pełne dane i metodologia):
`_notes/mentzen-struktura.md` (sitemapy, robots.txt, IA), `_notes/mentzen-oferta.md` (oferta, cennik, CTA), `_notes/mentzen-blog.md` (REST API WP, 523 wpisy, pomiary treści), `_notes/mentzen-brand.md` (marka, brand search, social proof), `_notes/mentzen-youtube.md` (kanał YouTube: katalog, wyświetlenia, optymalizacja, powiązanie z domeną).

Fakt potwierdzony przez użytkownika (2026-08-28): jedna stacjonarna lokalizacja — Toruń, Grudziądzka 110-114/101; konsultacje stacjonarne możliwe, większość klientów zdalnie; oddziały Warszawa/Gdańsk/Poznań nie istnieją.

Kluczowe URL-e serwisu: https://mentzen.pl/ · https://mentzen.pl/sitemap_index.xml · https://mentzen.pl/blog/ · https://mentzen.pl/mentzen-plus/ · https://mentzen.pl/doradztwo-podatkowe/ · https://mentzen.pl/doradztwo-prawne/ · https://mentzen.pl/nasz-zespol/ · https://mentzen.pl/dane-kontaktowe/ · https://corporate.mentzen.pl/
