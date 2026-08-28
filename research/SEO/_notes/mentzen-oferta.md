# mentzen.pl — oferta, segmenty klientów, język, ścieżki konwersji

Data badania: 2026-08-28. Źródło: publiczne strony mentzen.pl (sitemap Yoast, menu główne) oraz subdomeny.
Cel notatki: materiał wejściowy do skilla SEO — co firma sprzedaje i jak o tym pisze. To nie jest audyt techniczny.

## 1. Kim jest podmiot

- Kancelaria Mentzen Sp. z o.o., NIP 9562371350, REGON 52020126200000, KRS 0001071605.
- Jedno biuro: Amicus Business Park, ul. Grudziądzka 110-114/101, 87-100 Toruń.
- Telefon: +48 563 000 363. E-mail: kontakt@mentzen.pl. Źródło: https://mentzen.pl/dane-kontaktowe/
- Obsługa zdalna w całej Polsce (konsultacje online, panel klienta), lokalizacja toruńska jest tylko adresem rejestrowym/siedzibą — strona nie buduje przekazu lokalnego SEO ("kancelaria Toruń" nie występuje jako motyw).
- Model biznesowy: mieszanka abonamentów (subskrypcja miesięczna) i usług jednorazowych. Abonament jest prezentowany jako ścieżka domyślna.

## 2. Architektura serwisu (menu główne)

Menu dzieli ofertę na dwie grupy, co jest istotne dla mapowania intencji:

**Subskrypcje** (produkty powtarzalne, sprzedawane jak SaaS)
- Mentzen+ — https://mentzen.pl/mentzen-plus/
- Mentzen+ IT — https://mentzen.pl/mentzen-it/
- Mentzen Prime (księgowość) — https://mentzen.pl/mentzen-prime/
- Kadry Mentzena — https://mentzen.pl/kadry-mentzena/
- Ewidencja IP BOX — https://mentzen.pl/ewidencja-ip-box/
- Panel klienta (logowanie) — https://mentzen.pl/panel-klienta-logowanie/

**Usługi** (projektowe, sprzedawane przez konsultację)
- Zakładanie działalności — https://mentzen.pl/start-z-mentzenem-zakladanie-firmy/
- Doradztwo podatkowe (hub) — https://mentzen.pl/doradztwo-podatkowe/
- Doradztwo prawne (hub) — https://mentzen.pl/doradztwo-prawne/
- Księgowość — kieruje do Mentzen Prime

Poza tym: Blog https://mentzen.pl/blog/, Nasz zespół https://mentzen.pl/nasz-zespol/, Inwestorzy → corporate.mentzen.pl (osobny serwis, poza zakresem).

Struktura URL usług jest płaska: wszystko na pierwszym poziomie (`/vat/`, `/sukcesja/`), bez zagnieżdżenia w `/doradztwo-podatkowe/...`. Huby linkują do dzieci, ale hierarchia URL tego nie odzwierciedla. Kilka podstron (Fundacja Rodzinna, Sukcesja, Restrukturyzacja) jest współdzielonych przez oba huby — podatkowy i prawny.

## 3. Pełna lista usług z URL-ami

### 3.1 Subskrypcje

| Produkt | URL | Dla kogo | Cena publiczna |
|---|---|---|---|
| Mentzen+ Podatki | https://mentzen.pl/mentzen-plus/ | firmy, próg wg przychodu rocznego | 699 / 1299 / 2499 zł netto/mc |
| Mentzen+ Prawo i Podatki | https://mentzen.pl/mentzen-plus/ | jw., wariant rozszerzony | 999 / 1899 / 3199 zł netto/mc |
| Mentzen+ IT | https://mentzen.pl/mentzen-it/ | JDG w IT (programiści, PM, PO, Scrum master, DevOps, tester, architekt, UX/UI, analityk) | brak ceny na stronie, "Sprawdź dostępne pakiety" |
| Mentzen Prime (księgowość) | https://mentzen.pl/mentzen-prime/ | JDG, sp. c., sp. j., sp. z o.o., sp. k., fundacje i fundacje rodzinne, stowarzyszenia, S.A. | brak cennika, kalkulator wyceny |
| Kadry Mentzena | https://mentzen.pl/kadry-mentzena/ | firmy zatrudniające | brak cennika, kalkulator |
| Ewidencja IP BOX | https://mentzen.pl/ewidencja-ip-box/ | przedsiębiorcy z działalnością B+R korzystający z IP Box | brak ceny, przycisk "Kup teraz" |

Progi cenowe Mentzen+ (z modala cenowego, `data-podatki` / `data-prawo` w HTML):
- STANDARD — "Wybierz, gdy osiągasz przychody do 1 mln złotych w skali roku." → 699 zł netto (Podatki) / 999 zł netto (Prawo i Podatki)
- PREMIUM — "przychody do 10 mln złotych w skali roku" → 1299 / 1899 zł netto
- PRO — "przychody powyżej 10 mln złotych" → 2499 / 3199 zł netto

Zakres Mentzen+ Podatki wg strony: nielimitowane konsultacje podatkowe, odpowiedzi mailowe, analiza podatkowa z symulacją oszczędności, wsparcie przy zakładaniu działalności, baza wzorów dokumentów, webinary i newsletter, zniżki na inne usługi. Wariant Prawo i Podatki dokłada nielimitowane konsultacje prawne, comiesięczny audyt prawny, jedno wezwanie do zapłaty miesięcznie, szerszy dostęp do dokumentów.

### 3.2 Doradztwo podatkowe — hub i podstrony

Hub: https://mentzen.pl/doradztwo-podatkowe/ — nagłówek "Twoje podatki są w dobrych rękach".

- Optymalizacja podatkowa — https://mentzen.pl/optymalizacja-podatkowa/ — "Zwiększ Zyski, Zmniejsz Podatki!"
- Fundacja rodzinna — https://mentzen.pl/fundacja-rodzinna/ — "Myślisz o założeniu Fundacji Rodzinnej? Pomożemy Ci w tym!"
- CIT estoński — https://mentzen.pl/cit-estonski/ — "Obniżenie podatków jeszcze nigdy nie było tak proste!"
- Ceny transferowe — https://mentzen.pl/ceny-transferowe/ — dokumentacja TPR, local file, master file, polityka cen transferowych, audyt dokumentacji
- Restrukturyzacja działalności — https://mentzen.pl/restrukturyzacja-dzialalnosci/ — "Nowa struktura, lepsza efektywność"
- Sukcesja — https://mentzen.pl/sukcesja/ — "Zabezpiecz przyszłość swojej firmy"
- Kontrola i postępowanie podatkowe — https://mentzen.pl/kontrola-i-postepowanie-podatkowe/ — "Kontrola i postępowanie podatkowe - to stresujące doświadczenie, ale nie musi oznaczać problemów!"
- VAT — https://mentzen.pl/vat/ — "Zadbaj o prawidłowe rozliczenie"; audyt VAT, interpretacje indywidualne, WIS, weryfikacja JPK_VAT
- Kryptowaluty — https://mentzen.pl/kryptowaluty/ — "Inwestujesz w kryptowaluty? Pomożemy Ci z rozliczeniem sprzedaży!"; rozliczenia, wsparcie przy kontroli, AML przy wypłatach z giełd, zmiana rezydencji podatkowej

### 3.3 Doradztwo prawne — hub i podstrony

Hub: https://mentzen.pl/doradztwo-prawne/ — nagłówek "Doradztwo prawne dla Twojej firmy".

- Umowy szyte na miarę — https://mentzen.pl/umowy-szyte-na-miare/ — "Potrzebujesz umowy szytej na miarę? Z nami Twoje interesy są bezpieczne." (umowy o pracę, B2B, najem, regulaminy/OWU, NDA, zakaz konkurencji)
- Sprawy pracownicze — https://mentzen.pl/sprawy-pracownicze/ — "Profesjonalne wsparcie dla Twojego biznesu w zakresie prawa pracy!" (regulaminy, zwolnienia, spory, delegowanie, sygnaliści)
- Postępowania sądowe i windykacja — https://mentzen.pl/postepowania-sadowe-i-windykacja/ (gospodarcze, pracownicze, cywilne, administracyjne; windykacja od wezwania do egzekucji)
- Wsparcie ZUS — https://mentzen.pl/wsparcie-zus/ — "Profesjonalne wsparcie dla Twojego biznesu w zakresie relacji z ZUS!"
- Znaki towarowe — https://mentzen.pl/znaki-towarowe/ — "Chroń to, co Cię wyróżnia" (zgłoszenia, spory)
- Fundacja rodzinna, Sukcesja, Restrukturyzacja — te same URL-e co w dziale podatkowym

### 3.4 Wejście dla nowych firm

- Start z Mentzenem — https://mentzen.pl/start-z-mentzenem-zakladanie-firmy/ — "Załóż z nami przyszłą wielką firmę"
  - Pakiet: godzinna konsultacja podatkowa o wyborze formy, założenie JDG, wsparcie podatkowe przez pierwszy miesiąc, poradnik startowy.
  - Cena publiczna: 699 zł brutto (jedyna jednorazowa usługa z jawną ceną na stronie).
  - Istnieje regulamin promocji "Księgowość na start" powiązany ze Start z Mentzenem: https://mentzen.pl/regulamin-promocji-ksiegowosc-na-start-start-z-mentzenem/
- Zakładanie spółek przez S24 — osobny regulamin (https://mentzen.pl/regulamin-swiadczenia-uslug-zakladania-spolek-w-portalu-s24/) i osobna subdomena spolki.mentzen.pl (nie odpowiada poprawnie certyfikatem, nie udało się pobrać treści).

### 3.5 Usługi widoczne tylko w regulaminach / panelu (nie w menu)

Ślady oferty, której nie ma w nawigacji — warto sprawdzić przy planowaniu SEO, czy to produkty żywe:
- "Mentzen Nieruchomości" — https://mentzen.pl/regulamin-uslugi-mentzen-nieruchomosci/ (w HTML kalkulatora Prime są też pola `realestatebasic`…`realestatevip`)
- "Mentzen+ Prawo i Podatki za 2 zł" (promocja wejściowa) — https://mentzen.pl/regulamin-swiadczenia-uslugi-mentzen-podatki-prawo-2zl/
- "Legalny Mentzen" w pakietach 2 h / 4 h / 6 h — osobne panele Calendesk linkowane z /panel-klienta-logowanie/
- ulgi.mentzen.pl — osobna aplikacja do ulg podatkowych wymieniona w regulaminie konsultacji (strona to SPA, treść nie renderuje się w fetchu)

## 4. Dla kogo — segmenty klientów

Cały serwis jest B2B, adresowany do właściciela firmy, nie do działu. Segmentacja jest trzyosiowa:

1. **Po wielkości/przychodzie** — jawna w cenniku Mentzen+: do 1 mln, do 10 mln, powyżej 10 mln zł przychodu rocznie. To najczystszy sygnał, kogo firma uważa za swojego klienta: od mikrofirmy po średnią.
2. **Po etapie życia firmy** — zakładam działalność (Start z Mentzenem) → prowadzę (subskrypcje) → restrukturyzuję / przekazuję (Restrukturyzacja, Sukcesja, Fundacja rodzinna).
3. **Po branży/formie** — jedyna wyodrębniona branża to IT (Mentzen+ IT wymienia konkretne role zawodowe). Poza tym: inwestorzy krypto (osobna podstrona), firmy z transakcjami z podmiotami powiązanymi (ceny transferowe, progi 10 mln / 2 mln / 2,5 mln zł), spółki z prostą strukturą właścicielską (CIT estoński).

Mentzen Prime wymienia obsługiwane formy prawne wprost: JDG, spółka cywilna, jawna, z o.o., komandytowa, fundacja i fundacja rodzinna, stowarzyszenie, S.A.; KPiR i księgi handlowe.

Segment nieobsługiwany w komunikacji: konsument/osoba prywatna. Wyjątkiem są kryptowaluty i częściowo fundacja rodzinna, gdzie odbiorcą jest osoba fizyczna z majątkiem.

## 5. Jak firma opisuje usługi — język

Wzorzec jest powtarzalny na wszystkich podstronach usługowych i wygląda na świadomy szablon.

**Otwarcie: ból klienta w drugiej osobie.** Nagłówki są zdaniami do czytelnika, często z pytaniem albo trybem rozkazującym: "Twoje podatki są w dobrych rękach", "Zwiększ Zyski, Zmniejsz Podatki!", "Zabezpiecz przyszłość swojej firmy", "Chroń to, co Cię wyróżnia", "Myślisz o założeniu Fundacji Rodzinnej? Pomożemy Ci w tym!". Prawie nigdy nie ma nagłówka rzeczownikowego typu "Usługi w zakresie X" — wyjątki to "Ceny transferowe" i "Postępowania sądowe i windykacja", czyli tematy najbardziej techniczne.

**Formuła "zamiast X, rób Y".** Np. na doradztwie prawnym: "Zamiast topić się w morzu przepisów i papierów, skup się na rozwoju swojej firmy". Na IP BOX: "Zdejmujemy z Ciebie ciężar skomplikowanych formalności, abyś mógł skupić się na tym, co najważniejsze". Na ZUS: "Ubezpieczenia społeczne to zmora każdego przedsiębiorcy". Punkt wyjścia to zawsze uciążliwość, nie kompetencja kancelarii.

**Merytoryka jako dowód, nie jako treść główna.** Liczby i przepisy pojawiają się, ale w roli uwiarygodnienia: na /cit-estonski/ efektywna stawka 20% vs 26,3% dla małego podatnika i 25% vs 34,4% dla pozostałych; na /ceny-transferowe/ progi 10 mln / 2 mln / 2,5 mln zł; na /vat/ nazwy instrumentów (interpretacja indywidualna, WIS, JPK_VAT); na /doradztwo-prawne/ skróty RODO, BHP, AML, ZUS i odwołanie do Global Business Complexity Index. Proporcja to mniej więcej 70% korzyść / 30% merytoryka. Głębokie treści eksperckie są zepchnięte na bloga, nie na strony usługowe.

**Ton humorystyczny na stronie głównej, nieobecny na podstronach.** Home ma linie w rodzaju "Nic tak na świecie nie pachnie, jak zapach niepłaconych podatków o poranku" i satyryczny opis księgowych. Podstrony usługowe są już całkiem konwencjonalne. To rozjazd rejestru: marka jest rozpoznawalna z prowokacyjnego tonu, ale konwersyjne podstrony tego nie kontynuują.

**Personalizacja przez ekspertów.** Wiele podstron przypisuje konkretną osobę do tematu: Joanna Zawadzka (ZUS, prawo pracy, znaki towarowe), Konrad Tonkiewicz (VAT, sukcesja), Kacper Boroń (sukcesja, restrukturyzacja), Aleksandra Biskup (ZUS), Jakub Nowogórski (restrukturyzacja). CTA brzmi wtedy "umów konsultację z…" zamiast anonimowego formularza.

**Dowód społeczny.** Na home i /nasz-zespol/: 6–7 imiennych opinii klientów (m.in. Adrian Gorzycki, Maciej Wieczorek, Adrian Zdunczyk) oraz pasek logotypów mediów: Wprost, Rzeczpospolita, Strefa Inwestorów, Money, Business Insider. Jest deklaracja "najszybciej rozwijająca się kancelaria w Polsce". Brak liczb typu "X klientów", "Y lat na rynku", brak nagród. Zespół prezentowany imiennie, ok. 20+ osób z podziałem na działy (podatki, prawo, księgowość, kadry, administracja, IT); tytuły "doradca podatkowy" i "radca prawny" są eksponowane.

## 6. CTA i ścieżki kontaktu

Trzy równoległe ścieżki, obecne praktycznie na każdej podstronie:

**1. Rezerwacja konsultacji przez Calendesk.**
- Strona zbiorcza: https://mentzen.pl/konsultacje-online/ — nagłówek "Porozmawiajmy", instrukcja "wybierz usługę, doradcę i termin, aby omówić swój temat – resztą zajmiemy się my".
- Technicznie: iframe `https://doradztwo.mentzen.pl/` (Calendesk), z własnym stylowaniem i przyciskiem "Otwórz kalendarz na pełnym ekranie". Sam kalendarz to SPA, treść niedostępna bez JS — co ma znaczenie dla indeksacji oferty konsultacji.
- Tematy do wyboru w kalendarzu: Doradztwo podatkowe, Doradztwo prawne, Spółki, Księgowość, Kryptowaluty, Subskrypcje, Ulgi podatkowe, Ewidencja IP BOX, Inne.
- Warianty CTA na podstronach: "Umów konsultację", "Otwórz kalendarz", "Zarezerwuj swoją konsultację już dziś", "Umów się na spotkanie z naszym ekspertem", "Rozlicz krypto bez stresu! Umów się na konsultację."

**2. Formularz kontaktowy (Contact Form 7 / wpcf7).**
- Najczęstsza formuła: "Wypełnij formularz, a my zajmiemy się Twoją sprawą indywidualnie i kompleksowo" (ceny transferowe, CIT estoński, umowy, sprawy pracownicze, postępowania sądowe).
- Warianty przycisku: "Skontaktuj się z nami", "Wyślij formularz", "Zapytaj o niezobowiązującą ofertę", "Chcę niezobowiązującą ofertę".
- Formularz zawiera wybór tematu + zgody RODO na kontakt mailowy i telefoniczny.

**3. Telefon i e-mail w stopce każdej strony:** +48 563 000 363, kontakt@mentzen.pl.

**Ścieżki zakupowe (self-service):**
- Mentzen+: modal cenowy z przyciskami "Wybierz swój pakiet" / "Wybieram pakiet", zdarzenia GA `purchase_click`.
- Mentzen Prime i Kadry: kalkulator wyceny ("Sprawdź", "Dostępne pakiety") — cena wychodzi dopiero po podaniu formy prawnej, liczby dokumentów, procedur szczególnych (proporcja VAT, OSS, VAT marża, CIT estoński) i liczby zatrudnionych.
- Ewidencja IP BOX: "Kup teraz" / "Subskrybuj ewidencję IP BOX".
- Płatności: Stripe, Gpay, Przelewy24 (wg regulaminu konsultacji).

**Panel klienta** — https://mentzen.pl/panel-klienta-logowanie/ — cała obsługa posprzedażowa siedzi na Calendesku, osobna instancja na produkt:
- subskrypcje.mentzen.pl (Mentzen+ Podatki)
- mentzen-prawo.calendesk.net, mentzen-prawo-i-podatki.calendesk.net
- mentzen-pakiet.calendesk.net (pakiet 2 zł)
- mentzen-it.calendesk.net (Mentzen+ IT i IP BOX)
- mentzenprime.calendesk.net (księgowość)
- legalnymentzen2/4/6.calendesk.net (pakiety godzinowe)

## 7. Cennik — co jest jawne, a co nie

Jawne:
- Mentzen+ — pełna macierz 6 cen (699/1299/2499 zł netto Podatki; 999/1899/3199 zł netto Prawo i Podatki, miesięcznie), z jawnym kryterium progu przychodowego.
- Start z Mentzenem — 699 zł brutto za pakiet założycielski.
- Konsultacja wideo — regulamin określa czas 60 minut, ale nie cenę ("Informacja o wynagrodzeniu jest dostępna pod adresem www.mentzen.pl w momencie zakupu Usług"). Cena widoczna dopiero w kalendarzu Calendesk, więc dla robota niewidoczna.

Nieujawnione na stronie: Mentzen Prime, Kadry, Mentzen+ IT, Ewidencja IP BOX (kalkulator lub "sprawdź pakiety"), oraz wszystkie usługi projektowe (optymalizacja, fundacja rodzinna, CIT estoński, ceny transferowe, sukcesja, restrukturyzacja, VAT, kontrole, krypto, umowy, ZUS, znaki towarowe, windykacja) — tam zawsze wycena indywidualna po kontakcie.

## 8. Blog i treści — stan na 2026-08-28

- 522 wpisy w post-sitemap, 33 dodatkowe w interpretacje-sitemap, 1 osierocony wpis w praktyka-sitemap (https://mentzen.pl/blog/praktyka/copywriting-pisanie-tekstow/, lastmod 2021 — pozostałość po starszym typie treści).
- Podział kategorii w URL-ach: `doradztwo-podatkowe` 306, `doradztwo-prawne` 107, `inne` 77, `ksiegowosc` 32. Kategorie bloga odwzorowują działy oferty, co daje naturalne mapowanie treść → usługa.
- Struktura URL: `/blog/{kategoria}/{slug}/`, np. https://mentzen.pl/blog/doradztwo-podatkowe/zyski-sprzed-przeksztalcenia-a-estonski-cit/
- Osobny typ treści `interpretacje` — omówienia interpretacji i wyroków, np. https://mentzen.pl/blog/interpretacje/podatek-u-zrodla-z-pelnym-odliczeniem-wazny-wyrok-nsa/ , https://mentzen.pl/blog/interpretacje/studia-podyplomowe-w-kosztach-firmy/
- Tempo: 45 wpisów z lastmod 2026, 75 z 2025; najświeższy 2026-08-27. Publikacja regularna, kilka wpisów miesięcznie.
- Nagłówek bloga: "Wiedza podatkowa na wyciągnięcie ręki".
- Tematyka wpisów jest wyraźnie bardziej ekspercka niż strony usługowe i mocno pokrywa się z gorącymi tematami podatkowymi: klauzula GAAR, estoński CIT, fundacja rodzinna, przekwalifikowanie B2B na umowę o pracę, prop trading, OKI, IP Box. Przykłady: https://mentzen.pl/blog/doradztwo-podatkowe/10-lat-klauzuli-gaar-czy-jest-lepiej/ , https://mentzen.pl/blog/doradztwo-podatkowe/przekwalifikowanie-b2b-umowa-o-prace-pit-pracownik/ , https://mentzen.pl/blog/doradztwo-podatkowe/opodatkowanie-prop-tradingu/

## 9. Obserwacje przydatne przy budowie skilla SEO

- Blog i strony usługowe mówią dwoma różnymi językami. Blog jest merytoryczny i to on realnie łapie długi ogon; strony usługowe są sprzedażowe i ubogie w treść pod frazy problemowe. Skill powinien to rozdzielać: inny szablon dla strony usługowej, inny dla wpisu.
- Kategorie bloga = działy oferty, więc mapowanie wpis → strona usługowa → CTA jest wykonalne mechanicznie.
- Płaskie URL-e usług oznaczają brak sygnału hierarchicznego; huby `/doradztwo-podatkowe/` i `/doradztwo-prawne/` niosą całe linkowanie wewnętrzne do dzieci.
- Trzy podstrony (fundacja rodzinna, sukcesja, restrukturyzacja) obsługują dwie intencje naraz — podatkową i prawną. Przy pracy nad frazami trzeba zdecydować, którą stronę pod co optymalizować, żeby nie kanibalizować.
- Kluczowe elementy oferty (ceny Prime/Kadry, kalendarz konsultacji, panele) są za JS-em lub na subdomenach Calendesku, więc dla wyszukiwarki nie istnieją. Konwersja jest tam, gdzie treści nie ma.
- Jedyna jawnie zdefiniowana branża to IT. To model do ewentualnego powielenia, jeśli skill ma proponować strony branżowe.
- Firma sprzedaje głównie abonament, więc frazy transakcyjne warto rozpatrywać nie tylko jako "usługa X cena", ale jako "stała obsługa prawno-podatkowa / księgowość dla spółki z o.o." itd.

## 10. Czego nie udało się sprawdzić

- spolki.mentzen.pl — błąd certyfikatu (self signed) przy pobraniu, treść nieznana.
- ulgi.mentzen.pl oraz doradztwo.mentzen.pl (kalendarz) — SPA, treść nie renderuje się bez JS; ceny konsultacji i lista usług w kalendarzu pozostają nieustalone.
- corporate.mentzen.pl (sekcja Inwestorzy) — poza zakresem zadania.
- Statusu produktów "Mentzen Nieruchomości", pakietu 2 zł i "Legalny Mentzen" nie da się potwierdzić z samego serwisu — są tylko w regulaminach i panelu logowania, bez stron ofertowych.
