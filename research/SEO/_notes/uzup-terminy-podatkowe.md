# Kalendarz terminów podatkowych pod content bloga B2B

Notatka researchowa pod skill SEO dla mentzen.pl (kancelaria: doradztwo podatkowe, prawo,
księgowość; WordPress; rynek PL; YMYL).

Data researchu: 2026-08-28.

## 0. Jak powstała ta notatka i co z tego wynika dla wiarygodności

Terminy potwierdzałem w tej kolejności: tekst ustawy z Dziennika Ustaw (ELI API Sejmu,
`api.sejm.gov.pl/eli`), potem podatki.gov.pl i biznes.gov.pl jako wykładnia MF, potem zus.pl
dla składek. Portale komercyjne (Infor, pit.pl) nie były potrzebne ani razu, więc ta notatka
nie zawiera ani jednego twierdzenia z takiego źródła. To istotne, bo blog kancelarii pisany
z portali dziedziczy ich błędy.

Podstawy prawne cytuję z aktualnych tekstów jednolitych:

| Ustawa | Tekst jednolity | Data ogłoszenia |
|---|---|---|
| PIT | Dz.U. 2026 poz. 592 | 30.04.2026 (obwieszczenie z 17.04.2026) |
| CIT | Dz.U. 2026 poz. 554 | 22.04.2026 (obwieszczenie z 27.03.2026) |
| VAT | Dz.U. 2025 poz. 775 | 16.06.2025 (obwieszczenie z 21.05.2025) |
| Ryczałt ewidencjonowany | Dz.U. 2025 poz. 843 | 27.06.2025 (obwieszczenie z 13.06.2025) |
| Ordynacja podatkowa | Dz.U. 2026 poz. 622 | 11.05.2026 (obwieszczenie z 22.04.2026) |
| Ustawa o rachunkowości | Dz.U. 2026 poz. 522 | 16.04.2026 (obwieszczenie z 30.03.2026) |
| Świadczenia opieki zdrowotnej | Dz.U. 2025 poz. 1461 | 24.10.2025 (obwieszczenie z 26.09.2025) |

Pliki PDF pobierałem przez `https://api.sejm.gov.pl/eli/acts/DU/{rok}/{poz}/text.pdf`.
Ten sam sposób nadaje się do cyklicznej weryfikacji terminów przed publikacją tekstu.

Jedna uwaga o podatki.gov.pl: serwis odsyła `301` z HTTPS na HTTP i część starszych sekcji
przekierowuje do archiwum `podatki-arch.mf.gov.pl`. Kalendarz „Ważne terminy" MF działa
wyłącznie na archiwalnej domenie i ładuje dane JavaScriptem, więc nie nadaje się jako
źródło do cytowania. Kalendarz na dole tej notatki złożyłem z przepisów, nie z tego widżetu.

---

## 1. Reguła przesunięcia terminu. Podatki i ZUS działają inaczej niż się wydaje

To pierwszy błąd, który wyłapuje czytelnik-księgowy i który psuje E-E-A-T całego tekstu.

Ordynacja podatkowa, art. 12 § 5:

> „Jeżeli ostatni dzień terminu przypada na sobotę lub dzień ustawowo wolny od pracy,
> za ostatni dzień terminu uważa się następny dzień po dniu lub dniach wolnych od pracy,
> chyba że ustawy podatkowe stanowią inaczej."

Źródło: Dz.U. 2026 poz. 622, art. 12 § 5 (weryfikowane w pobranym PDF).

Sobota liczy się tak samo jak niedziela i święto. Przesunięcie idzie na następny dzień
roboczy po całym ciągu dni wolnych, nie na kolejny dzień kalendarzowy.

**ZUS: identyczna reguła, ale opisana niespójnie na stronach rządowych.**
Strona ZUS mówi wprost:

> „Jeżeli koniec tego terminu przypada w święto (dzień ustawowo wolny od pracy),
> sobotę lub niedzielę, to za ostatni dzień terminu uważa się następny dzień roboczy."

Źródło: https://www.zus.pl/baza-wiedzy/skladki-wskazniki-odsetki/skladki/terminy-rozliczania-i-oplacania-skladek-na-ubezpieczenia-spoleczne (data na stronie: 2 marca 2022)

Natomiast biznes.gov.pl w dwóch artykułach pomija sobotę („wypada w niedzielę lub święto")
— art. 00277 i 00287. Źródła: https://www.biznes.gov.pl/pl/portal/00277,
https://www.biznes.gov.pl/pl/portal/00287. Rozbieżność między dwoma serwisami rządowymi.
Wersja ZUS jest właściwa dla składek i tak należy pisać w treściach.

**Błąd w przykładzie na biznes.gov.pl, wart własnego akapitu w tekście.**
Artykuł 00236 podaje przykład: „jeżeli 30 kwietnia wypada w niedzielę, to deklarację PIT 36
możesz złożyć do 1 maja". 1 maja to Święto Pracy, dzień ustawowo wolny, więc termin
przesuwa się dalej. Źródło: https://www.biznes.gov.pl/pl/portal/00236 (aktualizacja 25.06.2026).
To dobry haczyk na tekst „kiedy naprawdę mija termin PIT" — pokazuje, że kancelaria czyta
przepis, a nie przepisuje poradnik.

Praktyczny skutek na najbliższe lata:

| Termin | 2027 | 2028 |
|---|---|---|
| 20 lutego (zmiana formy opodatkowania) | sobota → poniedziałek 22.02 | niedziela → poniedziałek 21.02 |
| 30 kwietnia (PIT roczny) | piątek, bez zmian | niedziela, a 1 maja wolne → wtorek 2.05 |
| 20 maja (roczna składka zdrowotna) | czwartek, bez zmian | sobota → poniedziałek 22.05 |
| 31 lipca (JPK ksiąg rachunkowych) | sobota → poniedziałek 2.08 | poniedziałek, bez zmian |
| 31 października | niedziela, 1 listopada wolne → wtorek 2.11 | wtorek, bez zmian |

---

## 2. Wybór i zmiana formy opodatkowania

Najważniejsze sprostowanie merytoryczne w całej notatce: **20 lutego nie jest terminem
ustawowym.** Ustawa mówi o 20. dniu miesiąca po miesiącu pierwszego przychodu. Dla większości
firm pierwszy przychód wypada w styczniu, więc wychodzi 20 lutego, i stąd wzięło się skrótowe
hasło z poradników. Firma, która pierwszy przychód roku osiągnie w marcu, ma czas do 20 kwietnia.

PIT, art. 9a ust. 2 (wybór podatku liniowego):

> „(...) do 20. dnia miesiąca następującego po miesiącu, w którym został osiągnięty pierwszy
> przychód z tego tytułu w roku podatkowym, albo do końca roku podatkowego, jeżeli pierwszy
> taki przychód został osiągnięty w grudniu tego roku podatkowego."

Źródło: Dz.U. 2026 poz. 592, art. 9a ust. 2.

Ustawa o ryczałcie, art. 9 ust. 1: identyczna konstrukcja dla wyboru ryczałtu.
Źródło: Dz.U. 2025 poz. 843, art. 9 ust. 1.

biznes.gov.pl potwierdza i podaje przykład liczbowy:

> „Pani Marta stosuje podatek liniowy. Pierwszy przychód w 2026 roku uzyskała w styczniu tego
> roku, ma zatem czas na zmianę z opodatkowania podatkiem liniowym na zasady ogólne
> do 20 lutego 2026 roku."

Źródło: https://www.biznes.gov.pl/pl/portal/00264

| Czynność | Termin | Źródło |
|---|---|---|
| Wybór lub zmiana: skala / liniowy / ryczałt | 20. dzień miesiąca po miesiącu pierwszego przychodu w roku; przy pierwszym przychodzie w grudniu — do końca roku | PIT art. 9a ust. 2; ryczałt art. 9 ust. 1 |
| Rezygnacja z karty podatkowej | do 20 stycznia roku kalendarzowego | https://www.biznes.gov.pl/pl/portal/00264 |
| Wybór ryczałtu estońskiego (ZAW-RD) | do końca pierwszego miesiąca pierwszego roku podatkowego objętego ryczałtem | CIT art. 28j ust. 1 pkt 7 (Dz.U. 2026 poz. 554) |

Wybór formy nie wymaga corocznego ponawiania. Milczenie oznacza kontynuację, a brak
jakiegokolwiek wyboru na starcie oznacza skalę podatkową.
Źródło: https://www.biznes.gov.pl/pl/portal/00263

---

## 3. Zaliczki na podatek dochodowy: wszystko na 20.

| Podatek | Zaliczka miesięczna | Zaliczka kwartalna | Ostatni okres roku | Podstawa |
|---|---|---|---|---|
| PIT (skala, liniowy) | do 20. dnia miesiąca za miesiąc poprzedni | do 20. dnia miesiąca po kwartale | do 20 stycznia roku następnego | PIT art. 44 ust. 6 |
| Ryczałt ewidencjonowany | do 20. dnia następnego miesiąca | do 20. dnia miesiąca po kwartale | do 20 stycznia roku następnego | Ryczałt art. 21 ust. 1 i 1a |
| CIT | do 20. dnia każdego miesiąca za miesiąc poprzedni | do 20. dnia miesiąca po kwartale | do 20. dnia pierwszego miesiąca następnego roku podatkowego | CIT art. 25 ust. 1a i 1c |

Kwartalne zaliczki PIT i CIT przysługują małym podatnikom i rozpoczynającym działalność
(PIT art. 44 ust. 3g, CIT art. 25 ust. 1b).
Wyboru nie zgłasza się w trakcie roku, wskazuje się go w zeznaniu rocznym za rok, w którym
się je stosowało. To samo dotyczy zaliczek uproszczonych.
Źródło: https://www.biznes.gov.pl/pl/portal/00264 (aktualizacja 2026)

Kwartalny ryczałt: limit 200 000 euro przychodu w roku poprzednim albo rozpoczęcie działalności.
Źródło: Dz.U. 2025 poz. 843, art. 21 ust. 1b.

---

## 4. Roczne zeznania PIT

PIT, art. 45 ust. 1:

> „(...) w terminie od dnia 15 lutego do dnia 30 kwietnia roku następującego po roku
> podatkowym. Zeznania złożone przed początkiem terminu uznaje się za złożone w dniu
> 15 lutego roku następującego po roku podatkowym."

Źródło: Dz.U. 2026 poz. 592, art. 45 ust. 1.

PIT-28 ma dziś ten sam termin co PIT-36. Stary termin lutowy nie obowiązuje i wciąż krąży
po starszych tekstach w sieci — warto to punktować w treściach evergreen.
Ustawa o ryczałcie, art. 21 ust. 2 pkt 2: „w terminie od dnia 15 lutego do dnia 30 kwietnia
roku następującego po roku podatkowym". Źródło: Dz.U. 2025 poz. 843.

| Formularz | Termin | Źródło |
|---|---|---|
| PIT-36, PIT-36L, PIT-37, PIT-38, PIT-39, PIT-28 | 15 lutego – 30 kwietnia | PIT art. 45 ust. 1; ryczałt art. 21 ust. 2 pkt 2 |
| Zapłata podatku z zeznania | 30 kwietnia | https://www.podatki.gov.pl/podatki-osobiste/pit/informacje-podstawowe |
| PIT-16A (składka zdrowotna od karty podatkowej) | do końca lutego | https://www.biznes.gov.pl/pl/portal/00236 |
| PIT-CFC | do końca 9. miesiąca po roku podatkowym | https://www.biznes.gov.pl/pl/portal/00236 |
| Danina solidarnościowa (DSF-1) i jej zapłata | do 30 kwietnia | PIT art. 30h ust. 4 |

Twój e-PIT udostępnia zeznania od 15 lutego. Brak działania podatnika przy PIT-37 i PIT-38
oznacza automatyczną akceptację 30 kwietnia.
Źródło: https://www.biznes.gov.pl/pl/portal/00236

Zwrot nadpłaty: 45 dni od złożenia elektronicznego, 3 miesiące od papierowego.
Źródło: https://www.podatki.gov.pl/podatki-osobiste/pit/informacje-podstawowe

---

## 5. CIT klasyczny

CIT, art. 27 ust. 1: zeznanie i zapłata podatku „do końca trzeciego miesiąca roku następnego".
Źródło: Dz.U. 2026 poz. 554.

Dla roku podatkowego równego kalendarzowemu to 31 marca. Dla roku przesuniętego liczy się
od jego końca, co warto powtarzać w treściach dla spółek z kapitałem zagranicznym.

| Obowiązek | Termin | Źródło |
|---|---|---|
| CIT-8, CIT-8AB i zapłata podatku | koniec 3. miesiąca roku następnego | CIT art. 27 ust. 1 |
| CIT-CFC i zapłata podatku | koniec 9. miesiąca roku następnego | CIT art. 27 ust. 2a |
| CIT-10Z | koniec 1. miesiąca roku następującego po roku powstania obowiązku | https://www.biznes.gov.pl/pl/portal/00236 |
| IFT-2R | koniec 3. miesiąca roku następującego po roku wypłat | CIT art. 26 ust. 3a |

---

## 6. CIT estoński

Terminy estońskie rozjeżdżają się z klasycznymi i to jest samodzielny temat contentowy.
Podatek od ukrytych zysków płaci się co miesiąc, a od podzielonego zysku raz w roku.

| Zdarzenie | Termin | Podstawa |
|---|---|---|
| ZAW-RD (wybór ryczałtu) | koniec pierwszego miesiąca pierwszego roku opodatkowania ryczałtem | CIT art. 28j ust. 1 pkt 7 |
| CIT-8E z załącznikiem CIT/EZ | do końca 3. miesiąca roku podatkowego, za rok poprzedni | CIT art. 28r ust. 1 |
| Ryczałt od podzielonego zysku i zysku na pokrycie strat | koniec 3. miesiąca roku po roku uchwały o podziale wyniku | CIT art. 28t ust. 1 pkt 1 |
| Ryczałt od rozdysponowanego dochodu z zysku netto | koniec 3. miesiąca roku po roku wypłaty | CIT art. 28t ust. 1 pkt 2 |
| Ryczałt od nieujawnionych operacji gospodarczych | koniec 3. miesiąca roku po roku, w którym należało zarachować | CIT art. 28t ust. 1 pkt 3 |
| Ryczałt od ukrytych zysków i wydatków niezwiązanych z działalnością | do 20. dnia miesiąca po miesiącu wypłaty, wydatku lub świadczenia | CIT art. 28t ust. 1 pkt 4 |
| Ryczałt od zmiany wartości składników majątku | do 20. dnia miesiąca po miesiącu przejęcia, przekształcenia lub aportu | CIT art. 28t ust. 1 pkt 5 |

Źródło ustawowe: Dz.U. 2026 poz. 554, art. 28r i 28t. Wykładnia MF potwierdza to samo:
https://www.podatki.gov.pl/podatki-firmowe/cit/cit-estonski/informacje-podstawowe-cit-estonski

Uwaga na sformułowanie art. 28r ust. 1. Przepis mówi „do końca trzeciego miesiąca roku
podatkowego", nie „roku następnego", ale deklaracja dotyczy poprzedniego roku. Dla roku
kalendarzowego wynik jest ten sam: 31 marca.

---

## 7. VAT i JPK_V7

| Obowiązek | Termin | Podstawa |
|---|---|---|
| Deklaracja VAT miesięczna (JPK_V7M) | do 25. dnia miesiąca za miesiąc poprzedni | VAT art. 99 ust. 1 |
| Deklaracja kwartalna (JPK_V7K, część deklaracyjna) | do 25. dnia miesiąca po kwartale | VAT art. 99 ust. 2 |
| Część ewidencyjna przy rozliczeniu kwartalnym | za pierwsze 2 miesiące kwartału osobno, do 25. dnia za miesiąc poprzedni; ewidencja trzeciego miesiąca idzie razem z JPK_V7K | https://www.biznes.gov.pl/pl/portal/00237 |
| Zapłata VAT | do 25. dnia miesiąca po miesiącu powstania obowiązku podatkowego | VAT art. 103 ust. 1 |
| VAT-R | przed pierwszą czynnością opodatkowaną | https://www.biznes.gov.pl/pl/portal/00236 |
| Podatek cukrowy | do 25. dnia następnego miesiąca | https://www.biznes.gov.pl/pl/portal/00252 |

Źródło ustawowe: Dz.U. 2025 poz. 775, art. 99 ust. 1–2 i art. 103 ust. 1.

Reguła przesunięcia jest tu opisana poprawnie na biznes.gov.pl:

> „Jeśli 25. dzień miesiąca wypada w sobotę lub dzień ustawowo wolny od pracy, wtedy masz
> czas do pierwszego dnia roboczego przypadającego po tych dniach."

Źródło: https://www.biznes.gov.pl/pl/portal/00237

---

## 8. JPK ksiąg (JPK_CIT, JPK_PIT). Termin zmienił się 1 lipca 2026

To najświeższa zmiana, jaką znalazłem, i jednocześnie najlepszy materiał na tekst, którego
nie ma jeszcze konkurencja. Ustawa z dnia 15 maja 2026 r. (Dz.U. 2026 poz. 779, ogłoszona
15.06.2026, w życie 1.07.2026) przesunęła termin przesyłania ksiąg rachunkowych z terminu
złożenia zeznania na późniejszy. Uwaga: nowy termin jest inaczej skonstruowany w każdej
z ustaw — w CIT to termin względny („koniec siódmego miesiąca po zakończeniu roku
podatkowego"), w PIT sztywna data kalendarzowa („do dnia 31 lipca"). Dla podatników PIT
wychodzi na to samo, bo ich rokiem podatkowym jest zawsze rok kalendarzowy, ale przy
cytowaniu przepisu nie wolno tych dwóch sformułowań mieszać.

W CIT nowelizacja podmienia w art. 9 ust. 1c odesłanie do terminu złożenia zeznania
z art. 27 ust. 1 **albo deklaracji z art. 28r ust. 1** na „do końca siódmego miesiąca
po zakończeniu roku podatkowego", a w art. 9 ust. 1e zamienia wyraz „trzeciego" na „siódmego".
Skreślone odesłanie do art. 28r ust. 1 oznacza, że zmiana obejmuje także podatników
estońskiego CIT — to warto wyłapać, bo rozdział 6 tej notatki dotyczy tej samej grupy.

W PIT nowe brzmienie art. 24a ust. 1e rozdziela dwa terminy:

> „1) do dnia upływu terminu złożenia zeznania, o którym mowa w art. 45 ust. 1 – w przypadku
> podatkowej księgi przychodów i rozchodów oraz ewidencji środków trwałych oraz wartości
> niematerialnych i prawnych,
> 2) do dnia 31 lipca – w przypadku ksiąg rachunkowych"

Źródło: Dz.U. 2026 poz. 779, art. 1 pkt 5 i art. 2 pkt 1 (weryfikowane w pobranym PDF).

Praktycznie dla roku kalendarzowego: PKPiR i ewidencja środków trwałych do 30 kwietnia,
księgi rachunkowe do 31 lipca.

Ta sama ustawa zamienia w art. 38ab ust. 1 pkt 2 lit. b ustawy CIT wyrazy „23 %" na „27 %".
To nie jest podwyżka: pkt 2 nadal ustala stawkę 23%, a lit. b tylko opisuje rok następujący
po roku, w którym zastosowano stawkę z pkt 1, czyli 27% — nowelizacja prostuje więc błędne
odesłanie. Przepis dotyczy stawki z art. 19 ust. 1 pkt 4 ustawy CIT, czyli banków
spółdzielczych i SKOK-ów, a nie całego sektora bankowego.
Źródło: Dz.U. 2026 poz. 779, art. 2 pkt 2; Dz.U. 2026 poz. 554, art. 38ab ust. 1
i art. 19 ust. 1 pkt 3–4.

[do weryfikacji] Za który rok podatkowy nowy siedmiomiesięczny termin stosuje się po raz
pierwszy. Brak przepisu przejściowego potwierdzony na pełnym tekście: ustawa ma tylko pięć
artykułów, z czego art. 4 utrzymuje w mocy przepisy wykonawcze, a art. 5 określa wejście
w życie — żaden nie rozstrzyga pierwszego rocznika. Dla roku 2025 termin z dotychczasowego
brzmienia minął przed 1 lipca 2026, więc pytanie jest realne, a nie akademickie.
Czego brakuje: wykładni organu. Sprawdziłem, że nie ma jej ani na podatki.gov.pl pod
`/jpk`, `/jpk/jpk-ksiegi-rachunkowe` i `/podatki-firmowe/cit/jpk-cit` (wszystkie `404`),
ani w artykule biznes.gov.pl 00236, który tematu JPK ksiąg w ogóle nie podejmuje.
Do zamknięcia potrzebny jest komunikat, objaśnienia podatkowe albo interpretacja ogólna MF —
w tej sesji limit WebSearch był wyczerpany, więc nie dało się ich wyszukać po treści.
Do czasu potwierdzenia nie publikować zdania o tym, od którego rocznika obowiązuje 31 lipca.

---

## 9. Składki ZUS: 5, 15, 20

Deklaracja rozliczeniowa i zapłata składek idą w tym samym terminie.

| Płatnik | Termin | Źródło |
|---|---|---|
| Jednostki budżetowe i samorządowe zakłady budżetowe | do 5. dnia następnego miesiąca | zus.pl, terminy rozliczania i opłacania składek |
| Płatnicy posiadający osobowość prawną (sp. z o.o., SA, spółdzielnie, fundacje, stowarzyszenia) | do 15. dnia następnego miesiąca | jw. |
| Pozostali: JDG, spółki osobowe (jawna, partnerska, komandytowa, komandytowo-akcyjna), płatnicy składek za siebie | do 20. dnia następnego miesiąca | jw. |

Źródło: https://www.zus.pl/baza-wiedzy/skladki-wskazniki-odsetki/skladki/terminy-rozliczania-i-oplacania-skladek-na-ubezpieczenia-spoleczne

Termin obejmuje ubezpieczenia społeczne, zdrowotne, Fundusz Pracy, Fundusz Solidarnościowy,
FGŚP i Fundusz Emerytur Pomostowych.
Źródło: https://www.biznes.gov.pl/pl/portal/00287

Dodatkowo ZUS IWA: do 31 stycznia, dla płatników opłacających składkę wypadkową.
Źródło: https://www.biznes.gov.pl/pl/portal/00287

Podział 15 / 20 to dobry temat sam w sobie. Spółka z o.o. płaci do 15., a jej wspólnik
prowadzący JDG do 20. Mylą to nawet biura rachunkowe.

---

## 10. Roczne rozliczenie składki zdrowotnej

Ustawa o świadczeniach opieki zdrowotnej wiąże ten termin z terminem PIT, a nie z konkretną datą:

> „2k. Kwota, o której mowa w ust. 2j, wykazywana jest w dokumencie rozliczeniowym składanym
> za miesiąc, w którym upływa termin złożenia zeznania, o którym mowa w art. 45 ust. 1 ustawy
> z dnia 26 lipca 1991 r. o podatku dochodowym od osób fizycznych.
> 2ka. Ubezpieczony (...) przekazuje roczne rozliczenie składek w dokumencie rozliczeniowym,
> o którym mowa w ust. 2k.
> 2l. Dopłata (...) następuje w terminie płatności składek za miesiąc, o którym mowa w ust. 2k."

Źródło: Dz.U. 2025 poz. 1461, art. 81 ust. 2k, 2ka, 2l.

Rozwinięcie dla roku kalendarzowego: rozliczenie wchodzi do dokumentu za kwiecień, a ten
przedsiębiorca składa i opłaca do 20 maja. Dopłata idzie w tym samym terminie.

Wniosek o zwrot nadpłaty: ZUS przygotowuje go na profilu PUE, a złożyć można wyłącznie
w terminie miesiąca od upływu terminu złożenia PIT. Wniosek po terminie zostaje bez rozpoznania.
Źródło: Dz.U. 2025 poz. 1461, art. 81 ust. 2m–2o.

ZUS zwraca nadpłatę w terminie 3 miesięcy od upływu terminu złożenia zeznania.
Źródło: Dz.U. 2025 poz. 1461, art. 81 ust. 2s; https://www.biznes.gov.pl/pl/portal/00277

Ten „miesiąc na wniosek, inaczej przepada" to najmocniejszy pojedynczy hak contentowy
w całym maju. Konsekwencja jest twarda i mierzalna w pieniądzu.

---

## 11. Sprawozdania finansowe

| Etap | Termin | Podstawa |
|---|---|---|
| Sporządzenie | nie później niż 3 miesiące od dnia bilansowego (31 marca przy roku kalendarzowym) | ustawa o rachunkowości art. 52 ust. 1 |
| Zatwierdzenie przez organ zatwierdzający | nie później niż 6 miesięcy od dnia bilansowego (30 czerwca) | art. 53 ust. 1 |
| Złożenie do KRS | 15 dni od dnia zatwierdzenia | art. 69 ust. 1 |
| Złożenie do Szefa KAS: podatnicy CIT niewpisani do rejestru przedsiębiorców KRS | 15 dni od zatwierdzenia | https://www.biznes.gov.pl/pl/portal/00245 |
| Złożenie do Szefa KAS: podatnicy PIT prowadzący księgi rachunkowe | w terminie złożenia zeznania rocznego | jw. |
| Oświadczenie o braku obowiązku (sp. jawna osób fizycznych i partnerska, przychody netto ze sprzedaży towarów i produktów za poprzedni rok obrotowy poniżej 2,5 mln euro) | 6 miesięcy od dnia kończącego rok obrotowy | ustawa o rachunkowości art. 70a |

Źródło ustawowe: Dz.U. 2026 poz. 522, art. 52 ust. 1, art. 53 ust. 1, art. 69 ust. 1.

Sankcje warto podawać konkretnie, bo to podnosi wartość tekstu: grzywna albo ograniczenie
wolności kierownika jednostki od miesiąca do 2 lat, tryb przymuszający KRS z wezwaniem
w ciągu 7 dni, a przy braku sprawozdań za 2 kolejne lata obrotowe sąd z urzędu orzeka
o rozwiązaniu podmiotu bez postępowania likwidacyjnego.
Źródło: https://www.biznes.gov.pl/pl/portal/00245

Bezpłatne złożenie przez Portal Rejestrów Sądowych działa tylko wtedy, gdy zgłoszenia dokona
osoba uprawniona do reprezentacji z numerem PESEL ujawnionym w KRS. Inaczej płatny KRS-Z30
za 140 zł przez S24. Źródło: jw.

---

## 12. Ceny transferowe

Trzy różne terminy w trzech różnych miesiącach, co czyni z tego wdzięczny temat na osobny
cykl wpisów dla spółek z grup kapitałowych.

| Obowiązek | Termin | Podstawa |
|---|---|---|
| Lokalna dokumentacja cen transferowych (local file) | koniec 10. miesiąca po zakończeniu roku podatkowego | CIT art. 11k ust. 1 |
| Informacja TPR-C / TPR-P | koniec 11. miesiąca po zakończeniu roku podatkowego | CIT art. 11t ust. 1 |
| Grupowa dokumentacja cen transferowych (master file) | koniec 12. miesiąca po zakończeniu roku podatkowego | CIT art. 11p ust. 1 |

Źródło ustawowe: Dz.U. 2026 poz. 554. Potwierdzenie MF dla TPR:

> „Informację TPR składa się do właściwego dla podatnika naczelnika urzędu skarbowego
> w terminie do końca jedenastego miesiąca po zakończeniu roku podatkowego."

Źródło: https://www.podatki.gov.pl/ceny-transferowe (data publikacji 6.02.2026, aktualizacja 12.08.2026)

Dla roku kalendarzowego: 31 października, 30 listopada, 31 grudnia.

Progi dokumentacyjne: 10 mln zł dla transakcji towarowych i finansowych, 2 mln zł dla
usługowych i pozostałych. Master file dotyczy grup o skonsolidowanych przychodach powyżej
200 mln zł w poprzednim roku obrotowym.
Źródło: CIT art. 11k ust. 2 i art. 11p ust. 1 pkt 2.

Informacji TPR nie podpisze pełnomocnik, chyba że jest adwokatem, radcą prawnym, doradcą
podatkowym albo biegłym rewidentem. Podpisuje kierownik jednostki.
Źródło: https://www.podatki.gov.pl/ceny-transferowe

Publikacja MF pod adresem `informator-tpr-2024.pdf` (nazwa pliku myli — dokument cytuje się
jako „TPR Informacja o cenach transferowych – pytania i odpowiedzi, wydanie szóste,
październik 2025, MF" i dotyczy wzorów TPR-C(5) i TPR-P(5) za rok podatkowy rozpoczynający się
po 31.12.2022, a nie „edycji za 2024 r.") potwierdza 11 miesięcy także dla skróconego roku
podatkowego: „Informacja TPR za skrócony rok podatkowy, obejmujący okres do zamknięcia ksiąg
rachunkowych, powinna zostać złożona w terminie 11 miesięcy po zakończeniu skróconego roku
podatkowego" (połączenie, pytanie 14) oraz to samo przy zmianie formy prawnej z zamknięciem
ksiąg (pytanie 15).
Źródło: https://www.podatki.gov.pl/media/bcbjtt3u/informator-tpr-2024.pdf, s. 9–10

---

## 13. ORD-U, IFT i informacje płatnika

| Formularz | Termin | Podstawa |
|---|---|---|
| ORD-U (umowy z nierezydentami) | 11 miesięcy licząc od zakończenia roku podatkowego, czyli 30 listopada przy roku kalendarzowym | § 3 ust. 3 rozporządzenia MF w sprawie informacji podatkowych, Dz.U. 2024 poz. 1452; sposób liczenia: Ordynacja art. 12 § 3 |
| IFT-2R (wypłaty na rzecz nierezydentów, CIT) | koniec 3. miesiąca roku następującego po roku wypłat | CIT art. 26 ust. 3a |
| IFT-1R (wypłaty na rzecz nierezydentów, PIT) | do końca lutego roku następującego po roku podatkowym | PIT art. 42 ust. 2 pkt 2 |
| PIT-4R do urzędu skarbowego | do końca stycznia | PIT art. 38 ust. 1a |
| PIT-11 do urzędu skarbowego | do końca stycznia | PIT art. 42g ust. 1 pkt 1 |
| PIT-11 do podatnika | do końca lutego | PIT art. 42g ust. 1 pkt 2 |

Uwaga o dacie ORD-U: rozporządzenie mówi o „terminie jedenastu miesięcy, licząc od zakończenia
tego roku podatkowego" (§ 3 ust. 3, brzmienie zweryfikowane co do słowa w tekście jednolitym
Dz.U. 2024 poz. 1452), a nie o konkretnym dniu. Przy liczeniu z Ordynacji (art. 12 § 3:
terminy w miesiącach kończą się z upływem dnia odpowiadającego dniowi początkowemu, a gdyby
takiego dnia nie było — w ostatnim dniu miesiąca) od 31 grudnia wychodzi 30 listopada,
tak samo jak przy TPR. [do weryfikacji] pozostaje wyłącznie potwierdzenie tej dziennej daty
przez organ: brakuje komunikatu lub objaśnień MF, które podawałyby „30 listopada" wprost.
W dwóch kolejnych sesjach limit WebSearch był wyczerpany, a formularza ORD-U nie ma na liście
formularzy interaktywnych MF, więc data nadal pochodzi z wyliczenia z przepisów, nie z wykładni
organu. Do zamknięcia potrzebny jest jeden trafiony adres na podatki.gov.pl albo wyszukiwarka.

ORD-U ma wyłączenie, które warto opisać osobno. Podmioty obowiązane do złożenia informacji
o cenach transferowych są zwolnione z ORD-U, z wyjątkiem tych, które realizują transakcje
z podmiotami z rajów podatkowych (art. 11k ust. 2a CIT, art. 23w ust. 2a PIT).
Źródło: Ordynacja podatkowa art. 82 § 1c, Dz.U. 2026 poz. 622.

---

## 14. Tabela zbiorcza. Rok kalendarzowy jako rok podatkowy

| Data | Co wypada |
|---|---|
| 20 stycznia | zaliczka PIT/CIT/ryczałt za grudzień lub IV kwartał; rezygnacja z karty podatkowej |
| 31 stycznia | PIT-4R i PIT-11 do US; ZUS IWA; CIT-10Z; ZAW-RD dla wybierających estoński CIT od stycznia |
| 15 lutego | start składania zeznań PIT; udostępnienie Twój e-PIT |
| 20 lutego | zmiana formy opodatkowania, jeśli pierwszy przychód był w styczniu |
| koniec lutego | PIT-11 do podatnika; IFT-1R; PIT-16A |
| 31 marca | CIT-8 i zapłata CIT; CIT-8E; IFT-2R; sporządzenie sprawozdania finansowego |
| 30 kwietnia | PIT-36/36L/37/28/38/39 i zapłata; danina solidarnościowa; JPK_PIT dla PKPiR i ewidencji ŚT; sprawozdanie finansowe podatników PIT do Szefa KAS |
| 20 maja | roczne rozliczenie składki zdrowotnej w dokumencie za kwiecień i dopłata |
| koniec maja | ostatni moment na wniosek o zwrot nadpłaty składki zdrowotnej (miesiąc od terminu PIT) |
| 30 czerwca | zatwierdzenie sprawozdania finansowego |
| 15 lipca | złożenie sprawozdania do KRS, jeśli zatwierdzono 30 czerwca |
| 31 lipca | JPK ksiąg rachunkowych (nowy termin od 1.07.2026) |
| 31 października | lokalna dokumentacja cen transferowych |
| 30 listopada | informacja TPR; ORD-U |
| 31 grudnia | grupowa dokumentacja cen transferowych |
| 20. każdego miesiąca | zaliczki PIT/CIT, ryczałt, składki ZUS dla JDG i spółek osobowych, estoński CIT od ukrytych zysków |
| 25. każdego miesiąca | JPK_V7 i zapłata VAT; podatek cukrowy |
| 15. każdego miesiąca | składki ZUS dla płatników z osobowością prawną |

---

## 15. Kalendarz contentowy

Założenie z briefu: publikacja lub odświeżenie 6–8 tygodni przed szczytem zainteresowania.
Poniższe okna wyliczyłem wstecz od terminów ustawowych. Kolumna „szczyt" to termin, nie
zmierzony wolumen wyszukiwań.

[do weryfikacji] Rzeczywista sezonowość zapytań w Google.pl. W tej sesji wyczerpał się limit
WebSearch, więc nie zweryfikowałem krzywych z Google Trends ani Keyword Plannera. Przed
zamrożeniem harmonogramu trzeba sprawdzić co najmniej: „zmiana formy opodatkowania",
„PIT termin", „CIT-8", „składka zdrowotna rozliczenie roczne", „sprawozdanie finansowe KRS",
„TPR termin". Moje podejrzenie jest takie, że szczyt zapytań wypada bliżej terminu niż
6 tygodni przed nim, bo przedsiębiorcy szukają pod presją, a nie z wyprzedzeniem. Jeśli tak,
publikacja 6–8 tygodni wcześniej i tak jest właściwa, bo daje Google czas na indeksację
i wstępne pozycjonowanie przed falą.

| Okno publikacji | Temat | Szczyt (termin) |
|---|---|---|
| 1–15 grudnia | wybór formy opodatkowania na nowy rok, symulacje skala / liniowy / ryczałt / estoński | 20 lutego |
| 1–15 grudnia | ZAW-RD i wejście w estoński CIT od stycznia | 31 stycznia |
| 1–15 grudnia | obowiązki płatnika: PIT-4R, PIT-11, ZUS IWA | 31 stycznia |
| 1–20 stycznia | zmiana formy opodatkowania krok po kroku, termin liczony od pierwszego przychodu | 20 lutego |
| 1–20 stycznia | co się zmienia w podatkach w nowym roku | styczeń–luty |
| 1–15 lutego | CIT-8, sporządzenie sprawozdania finansowego, IFT-2R | 31 marca |
| 15 lutego – 5 marca | PIT roczny dla przedsiębiorcy, PIT-28, ulgi, wspólne rozliczenie | 30 kwietnia |
| 15 lutego – 5 marca | danina solidarnościowa: kto podlega, jak liczyć | 30 kwietnia |
| 1–15 marca | JPK_PIT i JPK_CIT: co i kiedy wysłać po zmianie z lipca 2026 | 30 kwietnia i 31 lipca |
| 20 marca – 10 kwietnia | roczne rozliczenie składki zdrowotnej, dopłata i wniosek o zwrot | 20 maja |
| 20 marca – 10 kwietnia | zatwierdzanie sprawozdania, uchwały wspólników, podział zysku | 30 czerwca |
| 1–15 maja | złożenie sprawozdania do KRS: PRS, S24, KRS-Z30, sankcje | 15 lipca |
| 1–15 czerwca | JPK ksiąg rachunkowych do 31 lipca | 31 lipca |
| 1–15 września | lokalna dokumentacja cen transferowych, progi, analiza porównawcza | 31 października |
| 1–15 października | informacja TPR i ORD-U: kto podpisuje, jak wysłać, zwolnienia | 30 listopada |
| 1–15 listopada | master file | 31 grudnia |
| 1–15 listopada 2026 | KSeF dla najmniejszych firm (sprzedaż fakturowana ≤ 10 tys. zł miesięcznie), które tracą zwolnienie | 1 stycznia 2027 |
| cały rok, cykl miesięczny | terminarz na najbliższy miesiąc jako krótki wpis serwisowy | co miesiąc |

Cztery uwagi do harmonogramu.

**Grudzień jest najgęstszy i najbardziej niedoceniony.** Wybór formy opodatkowania to
najwyżej wyceniana decyzja roku dla klienta B2B, a treści na ten temat publikuje się zwykle
w styczniu, kiedy walka o SERP już trwa. Wejście w grudniu daje przewagę indeksacyjną.

**Teksty o cenach transferowych nie mają konkurencji o takiej jakości jak PIT.** Wolumen jest
mniejszy, ale intencja transakcyjna nieporównanie wyższa, a to dokładnie profil klienta
kancelarii. Trzy terminy w trzech kolejnych miesiącach dają naturalny cykl jesienny.

**Nowelizacja JPK ksiąg z 1 lipca 2026 to okno, które się zamyka.** Póki treści konkurencji
podają stary termin, tekst z poprawnym stanem prawnym i cytatem z Dz.U. 2026 poz. 779 ma
szansę zająć pozycję na dłużej.

**Odświeżanie zamiast pisania od nowa.** Terminy się nie zmieniają, zmieniają się kwoty,
limity i formularze. Rok do roku wystarczy podmiana liczb, dat przykładowych i przypisów,
pod warunkiem że tekst od początku ma daty w osobnych, łatwych do znalezienia miejscach,
a nie wplecione w zdania.

---

## 16. Czego nie udało się potwierdzić

- **Sezonowość wyszukiwań.** Limit WebSearch wyczerpany w tej sesji. Cały rozdział 15 opiera
  się na terminach ustawowych, nie na danych o wolumenie.
- **Pierwszy rok stosowania siedmiomiesięcznego terminu JPK ksiąg.** Opisane w rozdziale 8.
- **Dzienna data ORD-U przy roku kalendarzowym.** 30 listopada wynika z wyliczenia z § 3 ust. 3
  rozporządzenia i art. 12 § 3 Ordynacji, nie z komunikatu MF. Opisane w rozdziale 13.
- **Kalendarz „Ważne terminy" MF.** Widżet na https://podatki-arch.mf.gov.pl/inne-narzedzia/wazne-terminy/
  ładuje dane skryptem i w treści strony deklaruje „terminarz podatkowy na 2025 rok" mimo
  sierpnia 2026. Serwis zapowiada modernizację zakładki. Nie nadaje się jako źródło do cytowania
  ani jako link wychodzący z bloga.
- **KSeF — ustalone, wcześniejszy opis był błędny.** Adres https://www.podatki.gov.pl/ksef
  nie odpowiada `200`, tylko `301` na https://ksef.podatki.gov.pl/, a daty wdrożenia leżą
  na podstronie „Etapy wdrożenia KSeF 2.0" (http://ksef.podatki.gov.pl/etapy-wdrozenia-ksef/,
  aktualizacja 29.05.2026). Mapa drogowa MF, cytowana dosłownie:
  „1 lutego 2026 r. udostępnienie KSeF 2.0 — KSeF 2.0 dla dużych przedsiębiorców
  i odbierających faktury"; „1 kwietnia 2026 r. KSeF obowiązkowy dla pozostałych
  przedsiębiorstw — KSeF 2.0 obowiązkowy dla wszystkich przedsiębiorców oraz wystawiających
  faktury, z wyjątkiem najmniejszych firm, których łączna sprzedaż dokumentowana jest fakturami
  ≤ 10 tys. zł miesięcznie"; „1 stycznia 2027 r. KSeF obowiązkowy dla wszystkich — KSeF 2.0
  obowiązkowy dla wcześniej zwolnionych przedsiębiorców (do 10 tys. zł miesięcznie)".
  Do kalendarza contentowego wchodzi z tego jeden twardy termin przyszły: 1 stycznia 2027,
  z oknem publikacji w okolicach listopada 2026. Dwa pozostałe etapy są już za nami i nadają
  się na treści typu „co się zmieniło", nie „przygotuj się". Ustawowej podstawy tych dat nie
  sprawdzałem w Dz.U. — przed publikacją tekstu warto ją dołożyć, bo mapa drogowa to komunikat,
  nie przepis.
- **Terminy dla podatników z rokiem podatkowym innym niż kalendarzowy** podałem wyłącznie jako
  wzór liczenia od końca roku podatkowego. Przykładów liczbowych nie weryfikowałem.

## 17. Źródła

Akty prawne (PDF z api.sejm.gov.pl/eli):
- Ustawa o PIT, Dz.U. 2026 poz. 592
- Ustawa o CIT, Dz.U. 2026 poz. 554
- Ustawa z 15 maja 2026 r. zmieniająca PIT, CIT i ryczałt, Dz.U. 2026 poz. 779
- Ustawa o VAT, Dz.U. 2025 poz. 775
- Ustawa o zryczałtowanym podatku dochodowym, Dz.U. 2025 poz. 843
- Ordynacja podatkowa, Dz.U. 2026 poz. 622
- Ustawa o rachunkowości, Dz.U. 2026 poz. 522
- Ustawa o świadczeniach opieki zdrowotnej finansowanych ze środków publicznych, Dz.U. 2025 poz. 1461
- Rozporządzenie MF w sprawie informacji podatkowych, Dz.U. 2024 poz. 1452

podatki.gov.pl (dostęp 2026-08-28):
- https://www.podatki.gov.pl/podatki-osobiste/pit/informacje-podstawowe
- https://www.podatki.gov.pl/podatki-firmowe/cit/cit-klasyczny/informacje-podstawowe-cit-klasyczny
- https://www.podatki.gov.pl/podatki-firmowe/cit/cit-estonski/informacje-podstawowe-cit-estonski
- https://www.podatki.gov.pl/ceny-transferowe (publikacja 6.02.2026, aktualizacja 12.08.2026)
- https://www.podatki.gov.pl/media/bcbjtt3u/informator-tpr-2024.pdf — „TPR Informacja
  o cenach transferowych – pytania i odpowiedzi", wydanie szóste, MF, październik 2025
  (nazwa pliku sugeruje rocznik 2024, sam dokument nie)
- https://www.podatki.gov.pl/e-sprawozdania-finansowe
- http://ksef.podatki.gov.pl/etapy-wdrozenia-ksef/ (aktualizacja 29.05.2026); uwaga:
  https://www.podatki.gov.pl/ksef zwraca `301` na https://ksef.podatki.gov.pl/

biznes.gov.pl (dostęp 2026-08-28):
- 00236 Jak składać deklaracje podatkowe: PIT, CIT, VAT i inne (aktualizacja 25.06.2026)
- 00237 Jak składać JPK_VAT z deklaracją
- 00245 Kto i kiedy musi składać sprawozdania finansowe
- 00252 Podatek cukrowy
- 00263 Ryczałt od przychodów ewidencjonowanych
- 00264 Najważniejsze zasady opodatkowania według skali podatkowej
- 00274 Jakie składki na ubezpieczenia społeczne płaci przedsiębiorca do ZUS (aktualizacja 11.02.2026)
- 00277 Jak opłacać składki ZUS
- 00287 Jak składać deklaracje i raporty ZUS: ZUS DRA, ZUS RCA i RSA

zus.pl (dostęp 2026-08-28):
- https://www.zus.pl/baza-wiedzy/skladki-wskazniki-odsetki/skladki/terminy-rozliczania-i-oplacania-skladek-na-ubezpieczenia-spoleczne
