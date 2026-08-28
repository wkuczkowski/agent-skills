# roedl.pl — analiza SEO jako benchmark dla mentzen.pl

Data analizy: 2026-08-28
Domena: `www.roedl.pl` (polska część serwisu Rödl & Partner; ścieżka `/pl/`)

## Uwaga metodologiczna — ograniczenia tej analizy

**Rödl blokuje w `robots.txt` boty AI, w tym jawnie `ClaudeBot` i `anthropic-ai`:**

```
User-agent: CCBot        Disallow: /
User-agent: ClaudeBot    Disallow: /
User-agent: anthropic-ai Disallow: /
User-agent: Bytespider   Disallow: /
User-agent: FacebookBot  Disallow: /
User-agent: AhrefsBot    Crawl-delay: 10
User-agent: SemrushBot   Crawl-delay: 10
User-agent: MJ12bot      Crawl-delay: 10
User-agent: *            Crawl-delay: 2
```
Powyższe to wyciąg. Plik zawiera dodatkowo listę `Disallow` dla ścieżek technicznych (`/*Contact.aspx`, `/*ContactPersonEmail.aspx`, `/*roedlsearchresults.aspx`, `/*Authenticate.aspx`, `/*feedback.aspx`, `/help`, `/sites/search/`, `/*pagenotfounderror`, `/*accessdeniederror`, `/Podziekowanie`) oraz `Sitemap: https://www.roedl.pl/sitemap.xml`.

Źródło: `https://www.roedl.pl/robots.txt`, dostęp 2026-08-28. Treść pliku odtworzona przy weryfikacji 2026-08-28 — zgadza się co do joty. Dwa uzupełnienia: każda ścieżka `.aspx` występuje w pliku w dwóch wariantach wielkości liter (`/*Contact.aspx` i `/*contact.aspx`), a blok `User-agent: *` kończy się jawnym `Allow: /`.

W związku z tym analiza opiera się na **kilkunastu punktowych pobraniach publicznych stron** (nie na crawlu całej domeny) plus na sitemapie, którą Rödl sam publikuje w `robots.txt` do konsumpcji przez roboty. Nie prowadzono masowego scrapowania.

**Czego tu nie ma i czego nie wolno domniemywać:** brak jakichkolwiek danych o ruchu, widoczności, pozycjach i konwersji. Wszystkie wnioski dotyczą **struktury i sygnałów on-page**, nie skuteczności. Twierdzenia o skuteczności są oznaczone `[do weryfikacji]`.

**Status weryfikacji (2026-08-28, drugie przejście u źródła).** Potwierdzone niezależnie: treść `robots.txt`; 6 biur w Polsce (Gdańsk, Gliwice, Kraków, Poznań, Warszawa, Wrocław); pełna struktura nagłówków artykułu A wraz z blokiem 10 pytań Q&A; autorzy i daty publikacji wszystkich trzech artykułów (1 lipca 2026, 24 lipca 2026, 20 sierpnia 2026); brak JSON-LD na stronie głównej, artykule i profilu; profil `PersonID=88` = Monika Bartosiewicz z tytułem `Profil | Rödl`; zdublowana marka w `<title>` strony głównej; 6 kafli tematycznych na hubie „Warto wiedzieć"; brak rocznika 2026 broszury „Ceny transferowe"; literówka `dokumetacja`; komunikat wyszukiwarki ekspertów; HTTP 200 na ścieżce legacy `/pl-pl/`. Skorygowane lub oflagowane pozycje opisano w miejscu ich występowania.

Dodatkowo: `AhrefsBot` i `SemrushBot` dostają `Crawl-delay: 10`, co utrudnia zewnętrznym narzędziom pełne zbadanie tej domeny — dane o roedl.pl w Ahrefs/Semrush mogą być niekompletne. `[do weryfikacji]`

---

## Podsumowanie w jednym akapicie

Rödl to **odwrotność Crido**: treść merytoryczna i E-E-A-T są mocne i autentyczne (realni doradcy podatkowi z numerami na listach zawodowych, bio z datą wpisu, cykliczne broszury branżowe), natomiast **warstwa techniczna jest zaniedbana w stopniu, który w YMYL kosztuje**. Zero danych strukturalnych na całej domenie, daty i autorzy podani wyłącznie w warstwie widocznej (bez `datePublished`/`author`), 287 polskich URL-i ze spacjami i polskimi znakami, zdublowany segment ścieżki. To jest firma, która wygrywa **substancją mimo technikaliów**, nie dzięki nim. Dla mentzen.pl najcenniejsze do skopiowania są trzy rzeczy: model „autor pod tekstem + ekspert jako kontakt", blok Q&A na końcu tekstu i **cykl rocznych broszur** budujący powtarzalny sygnał świeżości. Najcenniejsze do NIE kopiowania — cała warstwa techniczna.

---

## 1. Stack i skala

| Element | Wartość |
|---|---|
| CMS | **Microsoft SharePoint** (ślady: `.aspx`, `/_Layouts/15/`, `MSOLayout_MakeInvisibleIfEmpty()`, `__REQUESTDIGEST`) |
| URL-e w sitemapie (wszystkie języki) | **3 609** |
| — polskich (`/pl/`) | **1 472** |
| — angielskich (`/en/`) | 1 091 |
| — niemieckich (`/de/`) | 1 045 |
| Biura w Polsce | 6 (Gdańsk, Gliwice, Kraków, Poznań, Warszawa, Wrocław) |

Źródło: `https://www.roedl.pl/sitemap.xml`, dostęp 2026-08-28. Sitemapa to płaski zbiór `<url>` (nie indeks), z `<lastmod>` i miejscami zagnieżdżonymi `<image:image>`; prefiksy `/pl/`, `/en/`, `/de/` potwierdzone.

`[do weryfikacji]` — wszystkie liczby w tej tabeli i w tabelach niżej (rozkład sekcji, klastry, rozkład `lastmod`, „287 URL-i ze spacjami") pochodzą z jednorazowego parsowania sitemapy i **nie zostały odtworzone przy weryfikacji** (ponowne pobranie zwróciło plik ucięty, ok. 900 wpisów). Traktować jako rząd wielkości, nie jako liczby twarde. Sygnał ostrzegawczy: rozkład sekcji polskich sumuje się do 1 458, a nie do deklarowanych 1 472.

**Serwis jest trójjęzyczny w proporcji ~40/30/30 PL/EN/DE.** To nie jest polska strona z tłumaczeniami — to niemiecka firma obsługująca niemiecki kapitał w Polsce, i struktura językowa to odzwierciedla. Dla mentzen.pl to sygnał, że Rödl gra w innym segmencie klienta (inwestor zagraniczny), a nie o polskiego przedsiębiorcę na tych samych frazach — konkurencja jest częściowa.

### Rozkład sekcji polskich

| Sekcja `/pl/…` | URL-e |
|---|---|
| `warto-wiedziec` | **1 135** |
| `uslugi` | 177 |
| `media` | 38 |
| `lokalizacje-i-kontakt` | 37 |
| `wydarzenia` | 25 |
| `o-nas` | 23 |
| `kariera` | 15 |
| `komu-doradzamy` | 8 |

**77% polskich URL-i to treść merytoryczna.** Stosunek treść:oferta wynosi ok. **6,4:1** (1135 do 177). To bardzo wysoki udział contentu jak na stronę firmy doradczej.

---

## 2. Architektura treści i klastry tematyczne

### Struktura URL artykułu

```
https://www.roedl.pl/pl/warto-wiedziec/warto-wiedziec/podatek-cit/<slug>
                        │              │              │
                        │              │              └─ klaster tematyczny
                        │              └─ ZDUBLOWANY segment (błąd)
                        └─ sekcja
```

Segment `warto-wiedziec` występuje **dwa razy** w każdym URL-u artykułu. To artefakt SharePointa (nazwa listy + nazwa strony), nie decyzja redakcyjna. Nie jest krytyczne, ale wydłuża URL i marnuje miejsce na słowo kluczowe.

### Klastry tematyczne (liczba polskich URL-i)

| Klaster | URL-e | Uwaga |
|---|---|---|
| `aktualności podatkowe` | **234** | ⚠ URL ze **spacją i polskimi znakami** |
| `koronawirus` | 78 | archiwalny, martwy temat |
| `ulgi-dotacje-psi` | 74 | |
| `ceny-transferowe` | **72** | najsilniejszy żywy klaster |
| `prawo-pracy` | 66 | |
| `odnawialne-zrodla-energii` | 56 | |
| `podatek-vat` | 50 | |
| `podatek-cit` | 42 | |
| `aktualnosci-podatkowe` | 33 | ⚠ **duplikat** klastra powyżej, poprawnie zapisany |
| `sse` | 32 | |
| `rodo` | 32 | |
| `podatek-pit` | 32 | |
| `polski-lad` | 27 | archiwalny |
| `clo-i-akcyza` | 27 | |
| `postepowania-sadowe-i-rozwiazywanie-sporow` | 25 | |
| `prawo-spolek-i-ma` | 21 | |
| `compliance` | 21 | |
| `whistleblowing` | 17 | |
| `podatek-od-nieruchomosci` | 15 | |
| `audyt-finansowy` | 15 | |
| `dotacje` | 14 | |
| `esg` | 13 | |
| `prawo-upadlosciowe` | 11 | |
| `apa` | 10 | |
| `ai-sztuczna-inteligencja` | 6 | nowy, rosnący |

Źródło: parsowanie `sitemap.xml`, dostęp 2026-08-28.

### Co Rödl robi tu DOBRZE

1. **Klaster = segment URL, nie tylko kategoria.** Temat jest zakodowany w ścieżce (`/podatek-cit/`, `/ceny-transferowe/`), więc Googlebot dostaje sygnał tematyczny z samego adresu. To ta sama zasada, którą stosuje Crido przez osobne custom post types.

2. **Klastry pokrywają się 1:1 z liniami usługowymi.** `ceny-transferowe` jako klaster treści ma odpowiednik w `/pl/uslugi/doradztwo-podatkowe/ceny-transferowe-i-dokumentacja-podatkowa`. Treść i oferta mówią tym samym słownikiem.

3. **Głębokość zamiast szerokości.** 72 artykuły o cenach transferowych to nie jest blog ogólnopodatkowy — to biblioteka tematyczna. Przy tak wąskim temacie taka gęstość buduje realną autorytatywność tematyczną.

4. **Hub „Warto wiedzieć" z 6 wyróżnionymi tematami + resztą zwiniętą do listy.** Strona `/pl/warto-wiedziec/` promuje sześć klastrów priorytetowych (aktualności prawno-podatkowe, ceny transferowe, księgowość i płace, postępowania sądowe i rozwiązywanie sporów, prawo pracy, ulgi/dotacje PSI — potwierdzone co do składu i liczby) i zwija resztę do listy „Pozostałe tematy". To świadome rozdanie link equity. Lista „Pozostałe tematy" liczy **28 pozycji** (potwierdzone przy weryfikacji 2026-08-28; wcześniejsze szacunki 26 i „ok. 40" były błędne).
   Źródło: `https://www.roedl.pl/pl/warto-wiedziec/`, dostęp 2026-08-28.

### Co Rödl robi tu ŹLE (i czego nie kopiować)

1. **287 polskich URL-i zawiera spacje lub polskie znaki diakrytyczne.** Przykłady:
   - `…/warto-wiedziec/aktualności podatkowe/wh-osc-pulapki-bledy-i-watpliwosci`
   - `…/rodo/nowelizacja-ustawy-o-krajowym-systemie-cyberbezpieczeństwa`
   - `…/ai-sztuczna-inteligencja/ai-w-przedsiębiorstwie-gotowe-rozwiazanie-czy-wlasny-system`

   Spacja w URL-u wymusza kodowanie `%20`, znaki diakrytyczne — punycode/percent-encoding. Efekt: brzydkie linki, ryzyko rozjazdu wersji zakodowanej i niezakodowanej, gorsza klikalność w SERP.

2. **Rozdwojony klaster: `aktualności podatkowe` (234 URL-e) i `aktualnosci-podatkowe` (33 URL-e).** Ten sam temat w dwóch różnych ścieżkach. To rozbija sygnał tematyczny i linkowanie wewnętrzne na dwa niepowiązane katalogi.

3. **Martwe klastry ciągną się w sitemapie.** `koronawirus` (78 URL-i) i `polski-lad` (27) to tematy nieaktualne od lat, wciąż zgłaszane do indeksacji. Rozcieńczają crawl budget i średnią jakość domeny.

### Świeżość — rozkład `lastmod`

| Rok | URL-e |
|---|---|
| 2025 | **1 939** |
| 2023 | 680 |
| 2024 | 618 |
| 2026 | 251 |
| 2022 | 68 |
| 2021 i starsze | 53 |

Skok w 2025 (1939 URL-i) sugeruje **masową migrację lub przebudowę serwisu w 2025 roku**, a nie realną aktualizację treści — `lastmod` został przestawiony hurtowo. `[do weryfikacji]` Dla mentzen.pl to przestroga: hurtowe przestawienie `lastmod` na wszystkich URL-ach to sygnał, który Google szybko dyskontuje.

---

## 3. Format artykułów — przeczytane teksty

Przeczytałem trzy artykuły z klastrów CIT i PIT.

### Artykuł A: „Zaliczki uproszczone w CIT – zasady stosowania i najnowsze orzecznictwo"

URL: `https://www.roedl.pl/pl/warto-wiedziec/warto-wiedziec/podatek-cit/zaliczki-uproszczone-w-cit-zasady-stosowania-i-najnowsze-orzecznictwo`, dostęp 2026-08-28.

**Objętość:** ~1 020 słów w obszarze `<main>`.

**Struktura nagłówków:**
```
H1  Zaliczki uproszczone w CIT – zasady stosowania i najnowsze orzecznictwo
H2  Wprowadzenie
H2  Istota zaliczek uproszczonych
H2  Warunki stosowania
H2  Najnowsze orzecznictwo – terminowość wpłat jako warunek skutecznego wyboru
H2  Podsumowanie
H2  Q&A – zaliczki uproszczone w CIT
    H3  1. Na czym polegają zaliczki uproszczone w CIT?
    H3  2. Kto może skorzystać z uproszczonych zaliczek w CIT?
    H3  3. Czy trzeba zgłaszać wybór tej metody?
    H3  4. Czy zaliczki trzeba wpłacać terminowo?
    H3  5. Co się dzieje w przypadku spóźnienia z wpłatą zaliczki?
    H3  6. Czy trzeba stosować tę metodę przez cały rok?
    H3  7. Czy uproszczone zaliczki wpływają na rozliczenie roczne?
    H3  8. Czy uproszczone zaliczki są korzystne dla firm?
    H3  9. Jakie jest ryzyko stosowania tej metody?
    H3  10. Jak ograniczyć ryzyko sporu z fiskusem?
H2  Kontakt
```

**To jest wzorzec wart skopiowania.** Trzy rzeczy działają tu razem:

- **Korpus ~600 słów w 5 sekcjach H2** — zwięźle, bez waty, każda sekcja odpowiada na jedno pytanie praktyka.
- **Blok Q&A z 10 pytaniami jako H3** — to jawna gra pod featured snippets, „People Also Ask" i odpowiedzi generatywne. Każde H3 jest sformułowane jako pełne pytanie w języku naturalnym, dokładnie tak, jak użytkownik je wpisuje.
- **H2 „Kontakt" z konkretnym ekspertem** zamyka tekst — o tym niżej.

**Uwaga krytyczna:** blok Q&A **nie jest oznaczony schematem `FAQPage`**. Rödl zbudował idealną strukturę pod rich results i nie podpiął danych strukturalnych. To dosłownie darmowy CTR leżący na stole.

### Artykuł B: „JPK_CIT: korzystna zmiana stanowiska dla spółek posiadających zagraniczne oddziały"

URL: `…/warto-wiedziec/warto-wiedziec/podatek-cit/jpk-cit-korzystna-zmiana-stanowiska-dla-spolek-posiadajacych-zagraniczne-oddzialy`, dostęp 2026-08-28.

```
H1  JPK_CIT: korzystna zmiana stanowiska dla spółek posiadających zagraniczne oddziały
H2  Co to oznacza dla przedsiębiorców?
H2  Kontakt
```

Korekta: pierwotnie zapisano tu trzeci H2 („Dyrektor Krajowej Informacji Skarbowej zmienił swoje wcześniejsze stanowisko…"). Weryfikacja 2026-08-28 pokazuje, że to lead artykułu, a nie nagłówek — artykuł B ma tylko dwa H2. Tym samym alert jest strukturalnie jeszcze uboższy, niż wynikało z pierwszej wersji notatki.

Format „alertu": interpretacja organu + sekcja **„Co to oznacza dla przedsiębiorców?"**. Ta druga sekcja to sedno — Rödl nie streszcza interpretacji, tylko **tłumaczy ją na konsekwencję operacyjną**. To jest różnica między treścią doradczą a przedrukiem z Dziennika Ustaw.

Meta description też jest napisana pod konsekwencję, nie pod temat:
> „Dyrektor KIS potwierdził, że dane zagranicznych samobilansujących się oddziałów nie muszą być ujmowane w JPK_CIT i JPK_KR_PD, ograniczając obowiązki raportowe spółek. Więcej »"

### Artykuł C: „Zmiany w PIT, ryczałcie i CIT – co oznaczają dla podatników i przedsiębiorców?"

URL: `…/warto-wiedziec/warto-wiedziec/podatek-pit/zmiany-w-pit-ryczalcie-i-cit-co-oznaczaja-dla-podatnikow-i-przedsiebiorcow`, dostęp 2026-08-28.

```
H1  Zmiany w PIT, ryczałcie i CIT – co oznaczają dla podatników i przedsiębiorców?
H2  Kontakt
```

~1 010 słów **bez ani jednego śródtytułu H2 w treści**. Ściana tekstu. To pokazuje, że **standard redakcyjny Rödla jest niespójny** — obok wzorcowo ustrukturyzowanego artykułu A stoi tekst bez struktury nagłówkowej. `[do weryfikacji]` — czy blok Q&A z artykułu A to nowy standard wdrażany stopniowo, czy jednorazowy eksperyment; sprawdzone 3 artykuły to za mała próba.

### Wspólne cechy formatu

| Cecha | Obserwacja |
|---|---|
| Docelowa długość | ~1 000 słów (bardzo spójne w 3 z 3 tekstów) |
| Tytuł strony | `<H1 artykułu> \| Rödl` |
| Meta description | pisana pod korzyść/konsekwencję, kończy się `Więcej »` |
| Zakończenie | zawsze H2 „Kontakt" z imiennym ekspertem |
| Autor | **jest** — imienny podpis pod tekstem, z tytułem zawodowym (A: Maria Wośkowiak-Adamczyk, doradca podatkowy; B: „Autorzy: Paweł Gąsak, Barbara Strycharczyk"; C: Monika Spotowska, adwokat, doradca podatkowy, Associate Partner) |
| Data publikacji | **jest** — widoczna pod tekstem (A: 1 lipca 2026, B: 24 lipca 2026, C: 20 sierpnia 2026) |
| `datePublished` / `article:published_time` w kodzie | brak trafień przy przeszukaniu HTML `[do weryfikacji]` |
| Dane strukturalne | **brak** |

**Wzorzec `Więcej »` w meta description jest konsekwentny w 3 z 3 zbadanych stron** — to celowy zabieg pod CTR (strzałka sugeruje kontynuację i przyciąga wzrok w SERP).

`[do weryfikacji]` — **cała warstwa `<head>` w tej notatce opiera się na jednym przebiegu.** Przy weryfikacji 2026-08-28 ponowne pobrania nie zwróciły treści `meta description` ani `meta keywords` na żadnej z badanych stron (najpewniej dlatego, że narzędzie konwertuje stronę do tekstu i gubi znaczniki z `<head>`). Nie podważa to cytowanych treści, ale znaczy, że **nie zostały one odtworzone niezależnie**. Dotyczy: meta description artykułów B i strony głównej, wzorca `Więcej »`, obu list `meta keywords` w sekcji niżej oraz `canonical` na profilu i ścieżkach legacy. Do potwierdzenia na surowym HTML.

### `meta keywords` — nieoczekiwane źródło wywiadu

Rödl wciąż wypełnia znacznik `meta keywords`, który Google ignoruje od 2009 roku. Dla nas to **jawnie wyłożona lista fraz docelowych**:

Artykuł A:
```
zaliczki uproszczone CIT, CIT zaliczki uproszczone zasady, art 25 ust 6 CIT wyjaśnienie,
jak działają zaliczki uproszczone CIT, CIT-8 zaliczki uproszczone,
terminowość zaliczek CIT orzecznictwo
```

Artykuł C:
```
zmiany w pit, zmiany cit, ryczałt, nowa skala podatkowa, reforma podatkowa,
nowe profi podatkowe, PIT, CIT
```

Dwie obserwacje. Po pierwsze — w artykule A frazy są **długoogonowe i pytaniowe** („jak działają…", „art 25 ust 6 CIT wyjaśnienie"), co potwierdza, że blok Q&A jest zaplanowany pod konkretne zapytania, a nie dopisany na oślep. Po drugie — literówka `nowe profi podatkowe` (zamiast „progi") pokazuje, że pole nie przechodzi korekty. Nie szkodzi SEO, ale **daje konkurencji darmowy wgląd w strategię słów kluczowych**. Mentzen nie powinien powtarzać tego błędu.

---

## 4. Sygnały E-E-A-T

To jest najmocniejsza strona Rödla — **merytorycznie**.

### Model „autor pod tekstem + ekspert jako kontakt"

Artykuły **mają imienny podpis autora** pod tekstem, z tytułem zawodowym — potwierdzone w 3 z 3 zbadanych artykułów (Maria Wośkowiak-Adamczyk; „Autorzy: Paweł Gąsak, Barbara Strycharczyk"; Monika Spotowska, adwokat, doradca podatkowy, Associate Partner). Autor bywa inną osobą niż ekspert kontaktowy: w artykule A tekst podpisuje Maria Wośkowiak-Adamczyk, a w bloku „Kontakt" stoi Monika Bartosiewicz.

Oprócz podpisu każdy artykuł kończy się blokiem:

> **Kontakt**
> Monika Bartosiewicz
> doradca podatkowy
> Partner
> [Wyślij zapytanie] [Profil]

Źródło: artykuł A, dostęp 2026-08-28.

To świadoma decyzja: obok autora tekstu stoi **osoba, do której zadzwonisz w tej sprawie**. Dla firmy doradczej to konwersyjnie mocne — zamienia czytelnika w lead w miejscu największego zaufania.

**Luka jest nie w warstwie widocznej, tylko w maszynowej.** Google w wytycznych dla oceniających jakość przypisuje wagę autorstwu treści YMYL. Rödl podaje autora i datę czytelnikowi, ale nie wystawia ich w danych strukturalnych (`author`, `datePublished`) — więc wyszukiwarka nie ma jak jednoznacznie powiązać artykułu o CIT z osobą mającą uprawnienia doradcy podatkowego. Rödl ma ekspertyzę i deklaruje ją ludziom, a nie maszynom.

### Profile ekspertów — bogata treść, fatalna oprawa

Profil: `https://www.roedl.pl/pl/o-nas/kim-jestesmy/profil?PersonID=88`, dostęp 2026-08-28.

**Treść bio (cytat):**
> doradca podatkowy, Partner, Warszawa
> – wpisana na listę doradców podatkowych od 2008 roku,
> – posiada certyfikat Ministra Finansów uprawniający do usługowego prowadzenia ksiąg rachunkowych,
> – zarządza kilkunastoosobowym zespołem specjalistów,
> – koordynuje międzynarodowe projekty podatkowe m.in. tax compliance,
> – kieruje grupą roboczą CIT w Rödl & Partner,
> – przeprowadza wdrożenia JPK i KSeF,
> – członkini grupy prawa międzynarodowego VAT,
> – autorka i współautorka licznych publikacji książkowych i prasowych, w tym współautorka broszur podatkowych wydawanych dla dziennika „Rzeczpospolita",
> – w RÖDL od 2013 roku

(Ostatnia pozycja — staż w firmie — została pominięta w pierwszej wersji cytatu; uzupełniona przy weryfikacji 2026-08-28. To dodatkowy sygnał E-E-A-T: ciągłość zatrudnienia.)

To jest **wzorcowe bio E-E-A-T**: tytuł zawodowy, **rok wpisu na listę** (weryfikowalne uprawnienie), certyfikat MF, zakres odpowiedzialności, staż w firmie, dowód publikacji w uznanym medium. Każdy element to sprawdzalny sygnał kompetencji, a nie marketingowy ogólnik.

Obecność linku do LinkedIn na profilu — pierwotnie wymieniona jako element bio — **nie została potwierdzona** przy ponownym pobraniu. `[do weryfikacji]`

**A teraz oprawa techniczna tego samego profilu:**

| Element | Wartość | Ocena |
|---|---|---|
| `<title>` | `Profil \| Rödl` | ✗ generyczny — identyczny dla **wszystkich** ekspertów |
| `meta description` | `Monika Bartosiewicz` | ✗ samo imię i nazwisko |
| `canonical` | `…/profil?PersonID=88` | ✗ parametr zapytania zamiast czytelnego sluga |
| schema `Person` | brak | ✗ |
| JSON-LD | 0 bloków | ✗ |

**To jest najważniejsza obserwacja całej analizy.** Rödl ma zespół realnych ekspertów z weryfikowalnymi uprawnieniami — i publikuje ich pod tytułem „Profil | Rödl" bez żadnego znacznika `Person`. Strona eksperta nie ma szans wyrankować na jego własne nazwisko, bo tytuł go nie zawiera. Cała ta ekspertyza jest **niewidoczna dla maszyn**.

### Wyszukiwarka ekspertów

`https://www.roedl.pl/pl/wyszukiwarka-ekspertow/`, dostęp 2026-08-28.

Filtry: linia usługowa (ok. 140 obszarów — potwierdzone przy weryfikacji 2026-08-28; pierwotne „ponad 150" zawyżone), biuro (6 miast: Gdańsk, Gliwice, Kraków, Poznań, Warszawa, Wrocław), alfabetycznie po nazwisku. Karta eksperta pokazuje nazwisko, **tytuł zawodowy** (doradca podatkowy / radca prawny / biegły rewident), stanowisko, biuro, języki.

Dobre: tytuły zawodowe są eksponowane systemowo, nie okazjonalnie. Języki na karcie to sensowny sygnał dla klienta zagranicznego.
Złe: strona bez aktywnego filtra nie renderuje listy („Proszę wprowadzić swoje hasło wyszukiwania"), więc **nie ma statycznej, indeksowalnej listy wszystkich ekspertów**. Liczby ekspertów nie dało się ustalić. `[do weryfikacji]`

### Pozostałe sygnały zaufania

Sekcja `/pl/komu-doradzamy/` zawiera osobne strony: `dlaczego-my/rankingi`, `dlaczego-my/nagrody`, `dlaczego-my/izby-i-stowarzyszenia`, `zaufali-nam`, `nasi-klienci`.
Źródło: sitemap, dostęp 2026-08-28.

Rozdzielenie rankingów, nagród i członkostw w izbach na **trzy osobne URL-e** to dobra praktyka — każdy typ dowodu zaufania dostaje własną stronę, którą można linkować z ofert.

---

## 5. Dane strukturalne — sekcja krytyczna

Zbadano kod źródłowy trzech typów stron:

| Strona | Bloki JSON-LD | Microdata |
|---|---|---|
| Strona główna `/pl/` | **0** | brak |
| Artykuł (CIT, zaliczki uproszczone) | **0** | brak |
| Profil eksperta (`PersonID=88`) | **0** | brak |

Dostęp 2026-08-28.

**Rödl nie ma żadnych danych strukturalnych na całej domenie.** Brakuje:

- `Organization` / `ProfessionalService` + `LocalBusiness` dla 6 biur
- `Article` / `NewsArticle` z `datePublished`, `dateModified`, `author`
- `Person` dla ekspertów (mimo istnienia bogatych bio z uprawnieniami)
- `FAQPage` dla bloku Q&A, który jest **już zbudowany** w HTML
- `BreadcrumbList` (mimo istnienia breadcrumbów wizualnych)

### Pozostałe defekty techniczne

**Brak daty publikacji w znacznikach maszynowych.** Przeszukanie HTML artykułu A pod kątem `datePublished`, `dateModified`, `article:published_time` nie zwróciło trafienia. `[do weryfikacji]` — natomiast **data publikacji jest widoczna dla czytelnika** pod tekstem każdego z 3 zbadanych artykułów (1 lipca 2026, 24 lipca 2026, 20 sierpnia 2026), więc pierwotna teza, że daty istnieją wyłącznie na listingach, jest błędna. Realny brak to sama warstwa maszynowa: bez `datePublished`/`dateModified` wyszukiwarka nie dostaje sygnału świeżości, mimo że treść go niesie.

**Pusty H1 na stronie głównej.** `[do weryfikacji]` — trzy odczyty, trzy różne wyniki. Pierwotna ekstrakcja nagłówków z `/pl/` zwróciła `['', 'Warto wiedzieć', 'Wydarzenia']`; drugie pobranie wyrenderowało pierwszy H1 jako `We pave the way. Worldwide.` (link z claimem); trzecie (2026-08-28) nie znalazło **żadnego** H1. Prawdopodobnie H1 zawiera grafikę/link bez węzła tekstowego, przez co parsery rozjeżdżają się w odczycie. Do rozstrzygnięcia na surowym HTML, nie na konwersji do tekstu — a to, że wynik zależy od parsera, samo w sobie jest sygnałem ostrzegawczym.

**Zdublowana marka w tytule strony głównej:**
> `Rödl | Usługi audytorskie | Doradztwo prawne | Doradztwo podatkowe | Księgowość | Rödl`

„Rödl" pojawia się na początku i na końcu. Tytuł jest też przeładowany — pięć członów oddzielonych pipe'ami.

**Meta description strony głównej z ptaszkami:**
> `Audyt ✓ Consulting ✓ Prawo ✓ Podatki ✓ Nowe technologie ✓ Cyberbezpieczeństwo | Sprawdź usługi Rödl »`

Znaki ✓ przyciągają wzrok w SERP, ale to lista kategorii bez propozycji wartości.

**Hreflang:** zaimplementowany poprawnie dla `pl`/`en`/`de` z wzajemnymi odsyłaczami, ale **brak `x-default`**.

**Duplikaty na ścieżkach legacy.** Stary format URL `/pl-pl/pl/warto-wiedziec/…` zwraca **HTTP 200** (nie przekierowanie) — potwierdzone 2026-08-28 na `https://www.roedl.pl/pl-pl/pl/warto-wiedziec/optymalizacja-podatkowa/Pages/default.aspx`. Ścieżki `/pl-pl/` nie występują w sitemapie (0 wystąpień).

Dwie poprawki wobec pierwszej wersji:
- Teza, że sytuację „ratuje poprawny `canonical` wskazujący na czysty URL `/pl/…`", **nie została potwierdzona** — ponowne pobranie strony legacy nie zwróciło znacznika `canonical`. Może to być artefakt konwersji do tekstu (znaczniki z `<head>` bywają gubione), więc do rozstrzygnięcia na surowym HTML. `[do weryfikacji]`
- Ścieżki legacy nie są tylko biernie żywe: **serwis sam do nich linkuje z bieżących stron**. Na `/pl/uslugi/doradztwo-podatkowe/` nagłówek „Optymalizacja podatkowa – doradztwo" prowadzi do `/pl-pl/pl/warto-wiedziec/optymalizacja-podatkowa/Pages/default.aspx`, a link do OWU do `/pl-pl/pl/Documents/owu/…` (potwierdzone 2026-08-28). To gorszy stan niż opisany pierwotnie: linkowanie wewnętrzne aktywnie zasila równoległą ścieżkę.

Przekierowanie 301 byłoby czystsze niż utrzymywanie dwóch żywych ścieżek.

**Wniosek:** to jest domena, na której **działa wyłącznie treść**. Cała warstwa maszynowej interpretacji jest pusta.

---

## 6. Łączenie treści z ofertą

To Rödl robi dobrze i konsekwentnie, w obie strony.

### Z oferty do treści

Strona `/pl/uslugi/doradztwo-podatkowe/`, dostęp 2026-08-28. ~1 200–1 500 słów.

```
H1  Doradztwo podatkowe
H2  Zapytaj nas o szczegóły oferty
H2  Optymalizacja podatkowa – doradztwo
H2  Doradztwo podatkowe – nasze usługi
H2  Bieżące doradztwo podatkowe – nasze usługi
H2  Ogólne warunki świadczenia usług
H2  Usługi doradztwa podatkowego – dlaczego warto korzystać?
H2  Doradztwo podatkowe dla osób fizycznych
H2  Kontakt
H2  Warto wiedzieć          ← moduł wciągający artykuły
```

(H2 „Kontakt" został pominięty w pierwszej wersji listy; uzupełniony przy weryfikacji 2026-08-28 — potwierdza, że wzorzec „H2 Kontakt na końcu" obejmuje też strony ofertowe, nie tylko artykuły.)

Na stronie **czterech imiennych partnerów z tytułami zawodowymi**: Katarzyna Judkowiak (doradca podatkowy, Partner), Monika Bartosiewicz (doradca podatkowy, Partner), Anna Harasimowicz (biegły rewident, Partner), Dominika Tyczka-Szyda (doradca podatkowy, Partner). Każde nazwisko z przyciskiem zapytania i linkiem do profilu.

**Moduł „Warto wiedzieć" na dole strony ofertowej** wciąga najnowsze artykuły z powiązanego klastra. Kluczowa różnica wobec Crido: u Crido widget CRIDOTEKA wrzuca artykuły **z całego serwisu**, często niepowiązane tematycznie (prawo pracy pod ulgą B+R). U Rödla moduł jest **wąsko tematyczny**.

Na stronie cen transferowych (`/pl/uslugi/doradztwo-podatkowe/ceny-transferowe-i-dokumentacja-podatkowa`, ~800–900 słów) moduł podaje **trzy artykuły o cenach transferowych i odcinek podcastu „Ceny transferowe – obowiązki dokumentacyjne"**. Dwa formaty treści, jeden temat, jedna strona ofertowa — wiązanie klastra z ofertą działa, choć węziej, niż zakładała pierwsza wersja notatki.

Korekta: pierwotnie przypisano temu modułowi także broszurę „Ceny transferowe – 2024". Weryfikacja 2026-08-28 nie znalazła broszury w module — są tam wyłącznie trzy artykuły (19 maja 2026, 28 maja 2025, 21 października 2024) i podcast. Warto też odnotować, że moduł **nie podaje wyłącznie najnowszych tekstów** — najstarsza pozycja ma dwa lata.

*(Drobiazg: na tej stronie literówka w nagłówku — „Ceny transferowe i **dokumetacja** podatkowa – nasze usługi".)*

### Struktura oferty

177 polskich URL-i w `/uslugi/`, rozkład głębokości: 1 URL na poziomie 2, 46 na poziomie 3, **104 na poziomie 4**, 26 na poziomie 5.

Hierarchia jest logiczna i głęboka:
```
/uslugi/doradztwo-podatkowe/
  ├── podatki-dochodowe/{cit, ulga-b-r}
  ├── podatki-obrotowe/{vat, krajowy-system-e-faktur, podatek-od-czynnosci-cywilnoprawnych-pcc}
  ├── podatki-majatkowe/{podatek-od-nieruchomosci, podatek-od-spadkow-i-darowizn, podatek-od-srodkow-transportu}
  ├── ceny-transferowe-i-dokumentacja-podatkowa
  ├── estonski-cit
  ├── optymalizacja-podatkowa
  ├── miedzynarodowe-planowanie-podatkowe
  ├── doradztwo-transakcyjne-i-restrukturyzacje
  ├── postepowania-podatkowe
  ├── due-dilligence-i-przeglady-podatkowe          ← literówka: "dilligence"
  ├── vat-compliance-w-e-commerce
  ├── ulgi-i-dotacje-dla-innowacyjnych-przedsiebiorcow
  ├── procedury-nalezytej-starannosci-dla-transakcji-rajowych
  ├── podatek-od-przerzuconych-dochodow
  └── clo-i-akcyza
```

**Kontrast z Crido, wart odnotowania:** Crido wyciąga strony o najwyższej intencji zakupowej do korzenia domeny (`crido.pl/ulga-b-r/`), Rödl trzyma je głęboko (`roedl.pl/pl/uslugi/doradztwo-podatkowe/podatki-dochodowe/ulga-b-r`). Rödl ma czystszą, bardziej semantyczną hierarchię; Crido ma krótsze URL-e i płytszą ścieżkę kliknięć od strony głównej. `[do weryfikacji]` — które podejście wygrywa na frazach komercyjnych, nie da się rozstrzygnąć bez danych o pozycjach.

**Uwaga:** URL `due-dilligence` zawiera literówkę (poprawnie: „due diligence"). Przy trzech znalezionych literówkach (`dokumetacja`, `dilligence`, `nowe profi`) widać, że **korekta nie jest częścią procesu publikacji**. Dla domeny YMYL literówka w nazwie usługi to sygnał niedbałości.

---

## 7. Publikacje cykliczne

Tu Rödl ma przewagę, której nie mają SaaS-y księgowe.

### Broszury roczne — powtarzalny cykl

`/pl/media/nasze-publikacje/broszury/`, dostęp 2026-08-28.

**Seria „Ceny transferowe" wydawana rok po roku:**
- `ceny-transferowe-2021-i-2022`
- `ceny-transferowe-2022`
- `ceny-transferowe-2023`
- `ceny-transferowe-2024`
- `ceny-transferowe-2025`
- (wcześniej: `abc-cen-transferowych-w-roku-2020`, `dokumentacja-cen-transferowych-2020` — nie widoczne już na stronie broszur, prawdopodobnie w `/broszury/archiwum` `[do weryfikacji]`)

**Stan na 2026-08-28: najnowszy rocznik to 2025 — edycji 2026 jeszcze nie ma.** Cykl jest więc roczny, ale nie kalendarzowo punktualny; przy ocenie „powtarzalnego sygnału świeżości" warto o tym pamiętać.

**Seria „Fotowoltaika w Polsce":** `fotowoltaika-w-polsce-2022`, `fotowoltaika-w-polsce-2023`.

Pozostałe broszury tematyczne: `fundacja-rodzinna`, `prawo-holdingowe-w-polsce`, `spolki-nieruchomosciowe`, `raportowanie-schematow-podatkowych`, `wartosc-nieruchomosci-w-zmieniajacym-sie-swiecie`, `polands-tax-exemption-regime-for-foreign-investment-and-pension-funds` (po angielsku — pod inwestora zagranicznego), plus archiwum pod `/broszury/archiwum`.

**Dlaczego to działa:** seria roczna daje **przewidywalny, powtarzalny powód do publikacji i do linkowania**. Co roku powstaje nowy URL, nowy asset do promocji, nowy pretekst do maila i do wzmianki w mediach branżowych. To buduje sygnał „ta firma jest referencyjnym źródłem w cenach transferowych" w sposób, którego pojedynczy artykuł nie zbuduje.

**Wada implementacji:** każdy rocznik to osobny URL, więc **autorytet rozprasza się na 5+ adresów** zamiast kumulować się na jednym, aktualizowanym co roku. Crido stosuje odwrotną taktykę na stronach filarowych — jeden URL, rok w tytule, `dateModified` aktualizowane rocznie. Dla broszur-PDF-ów rozdzielenie ma sens archiwalny, ale **hub `/broszury/` powinien być stroną filarową**, której archiwum podlega.

### Newsletter Polska

`/pl/media/nasze-publikacje/newsletter/`, dostęp 2026-08-28.

Jeden newsletter marki: **„Newsletter Polska"**, zakres: „zmiany w podatkach, prawie i gospodarce" (cytat ze strony potwierdzony).

**Korekta:** wcześniejsza teza o „publicznym, indeksowalnym archiwum wydań newslettera" **nie ma pokrycia w źródle**. Weryfikacja strony nie wykazała listy wydań newslettera. Datowana lista widoczna na tej stronie (od 27.08.2026 wstecz do 28.05.2025) to moduł **„Warto wiedzieć" z artykułami**, a nie archiwum wysyłek — te same daty co w hubie aktualności. Czy Rödl publikuje archiwum wydań pod innym adresem — nie ustalono. `[do weryfikacji]`

Częstotliwość wysyłki nie jest podana na stronie. `[do weryfikacji]`
Zakres danych zbieranych w formularzu zapisu — nie ustalono. `[do weryfikacji]`

Osobny URL `/newsletter/pozostanmywkontakcie` sugeruje dedykowaną ścieżkę lead-nurturingową. `[do weryfikacji]`

### Pozostałe formaty cykliczne

| Format | URL | Uwaga |
|---|---|---|
| Podcasty | `/pl/media/podcasty/` | linkowane z odpowiednich stron usług |
| Książki | `/pl/media/nasze-publikacje/ksiazki/` | najmocniejszy sygnał autorytetu |
| Rödl w mediach | `/pl/media/roedl-und-partner-w-mediach/` + roczniki 2016–2019 | dowód cytowalności |
| Wydarzenia | `/pl/wydarzenia/` + `archiwum-wydarzen/2017…2023` | webinary/szkolenia |
| Szkolenia na zamówienie | `/pl/wydarzenia/szkolenie-na-zamowienie/` | np. „Indywidualne warsztaty Pillar 2 na danych klienta" |
| Kalkulator APA | `/pl/kalkulator-apa` | **jedyne narzędzie interaktywne w serwisie** |

**Pięć formatów treści** (artykuł, broszura, podcast, książka, wydarzenie) wokół tych samych klastrów tematycznych. To buduje autorytet szerzej niż sam blog.

**Kalkulator APA** to jedyne narzędzie w całej domenie. Narzędzia interaktywne są zwykle najlepszymi magnesami linkowymi — Rödl ma tu jedno i najwyraźniej go nie rozwija.

---

## 8. Rödl vs Crido — tabela porównawcza

| Wymiar | Rödl | Crido |
|---|---|---|
| CMS | SharePoint | WordPress + Yoast |
| URL-e (PL) | 1 472 | ~5 400 |
| Separacja klastrów | segment URL | osobne custom post types |
| Strony filarowe | głęboko w `/uslugi/…` | w korzeniu domeny |
| Długość artykułu | ~1 000 słów | zróżnicowana |
| Blok Q&A/FAQ | tak (niekonsekwentnie) | nie zaobserwowano |
| Ekspert przy treści | byline + osobny blok „Kontakt" | strony autorów `/author/…` |
| Bio z uprawnieniami | **bardzo mocne** | słabsze |
| Dane strukturalne | **zero** | słabe |
| Data w kodzie artykułu | widoczna dla czytelnika, **brak w znacznikach** | `datePublished` + `dateModified` |
| Publikacje cykliczne | **broszury roczne, książki, podcast** | raporty (97 URL-i) |
| Linkowanie oferta→treść | wąskie, tematyczne | szerokie, automatyczne, rozmyte |
| Wielojęzyczność | PL/EN/DE równolegle | głównie PL |

**Obie firmy mają słabe dane strukturalne.** To potwierdza wniosek z analizy Crido: **w polskim segmencie doradztwa podatkowego dane strukturalne są niezagospodarowanym polem**. Mentzen może tu zbudować przewagę niskim kosztem.

---

## 9. Wnioski dla mentzen.pl

### Do skopiowania — wysoki priorytet

**1. Blok Q&A na końcu artykułu, oznaczony schematem `FAQPage`.**
Rödl zbudował wzorcowy blok (10 pytań jako H3, sformułowanych w języku naturalnym) i **nie podpiął `FAQPage`**. Mentzen powinien zrobić jedno i drugie. To pojedyncza zmiana, która działa równocześnie na featured snippets, „People Also Ask" i odpowiedzi generatywne AI. Standard redakcyjny: 8–10 pytań, każde jako pełne zdanie pytające, odpowiedź 40–60 słów.

**2. Bio ekspertów w standardzie Rödla — ale z `Person` schema.**
Wzorzec do naśladowania: tytuł zawodowy + **rok wpisu na listę zawodową** + certyfikaty + zakres odpowiedzialności + dowody publikacji + LinkedIn. To są weryfikowalne fakty, a nie marketing. Rödl ma to wszystko i marnuje pod tytułem „Profil | Rödl".
Mentzen powinien: `<title>` = `Imię Nazwisko — doradca podatkowy | Mentzen`, czytelny slug zamiast `?PersonID=`, `meta description` streszczające specjalizację, `schema.org/Person` z `jobTitle`, `hasCredential`, `sameAs` do LinkedIn i do wpisu na liście KIDP.

**3. Ekspert jako kontakt pod każdym artykułem.**
Blok „Kontakt" z imieniem, tytułem zawodowym, przyciskiem zapytania i linkiem do profilu. Konwersyjnie mocniejszy niż sam byline.
**Obok autorstwa, nie zamiast niego.** Mentzen powinien mieć **jedno i drugie**: widoczny byline z autorem (sygnał E-E-A-T dla YMYL) i blok kontaktowy na dole (konwersja). Rödl ma oba — i to jest właśnie wzorzec do skopiowania; traci dopiero na tym, że żadnego z nich nie wystawia w `author` w danych strukturalnych.

**4. Cykl publikacji rocznej.**
Rödl wydaje „Ceny transferowe" rocznik po roczniku od 2020 (najnowszy potwierdzony: 2025). Mentzen powinien wybrać **jeden temat, w którym chce być źródłem referencyjnym**, i wydawać go corocznie. Przewidywalny cykl daje powtarzalny powód do publikacji, promocji i pozyskiwania linków.
**Implementacja lepsza niż u Rödla:** jeden stały URL huba (np. `/raporty/ceny-transferowe/`) aktualizowany co roku, z archiwum roczników jako podstrony. Autorytet kumuluje się na jednym adresie zamiast rozpraszać na pięć.

**5. Publiczne, indeksowalne archiwum newslettera.**
Treść wysłana mailem pracuje drugi raz w wyszukiwarce, zamiast ginąć w skrzynkach. Koszt wdrożenia niemal zerowy. **Uwaga: to rekomendacja z teorii, nie skopiowany wzorzec** — weryfikacja nie potwierdziła, żeby Rödl takie archiwum publikował (patrz sekcja 7). `[do weryfikacji]`

**6. Sekcja „Co to oznacza dla przedsiębiorców?" w alertach.**
Nie streszczaj interpretacji — tłumacz ją na konsekwencję operacyjną. To odróżnia treść doradczą od przedruku przepisu i jest dokładnie tym, czego szuka użytkownik wpisujący „co zmienia się w…".

**7. Wąsko tematyczny moduł „Warto wiedzieć" na stronach ofertowych.**
Rödl podaje na stronie cen transferowych trzy artykuły + broszurę + podcast — wszystko o cenach transferowych. Crido wrzuca automatycznie cokolwiek z całego serwisu i rozmywa trafność. **Kuracja tematyczna bije automat.**

### Do uniknięcia — czego Rödl uczy negatywnie

**1. Zero danych strukturalnych to zmarnowana ekspertyza.**
Rödl ma najlepsze bio ekspertów w tej grupie porównawczej i żadnego znacznika `Person`. Ma gotowy blok Q&A i żadnego `FAQPage`. Ma 6 biur i żadnego `LocalBusiness`. Dla mentzen.pl minimum: `Organization`, `LocalBusiness` per biuro, `Article` z `author`/`datePublished`/`dateModified`, `Person` dla ekspertów, `FAQPage` dla Q&A, `BreadcrumbList`.

**2. Data i autor widoczne dla człowieka, niewidoczne dla maszyny.**
Rödl podaje pod tekstem i datę, i imiennego autora z tytułem zawodowym — ale ani jedno, ani drugie nie trafia do danych strukturalnych. Sygnał, który redakcja już wytworzyła, ginie w drodze do wyszukiwarki. Mentzen: widoczna data publikacji **i** data ostatniej aktualizacji, plus `datePublished`/`dateModified`/`author` w JSON-LD. Dołożenie warstwy maszynowej do istniejącej warstwy widocznej to najtańsza możliwa poprawka.

**3. Higiena URL-i.**
287 polskich URL-i Rödla zawiera spacje lub polskie znaki. Klaster „aktualności podatkowe" jest rozbity na dwie ścieżki (234 + 33 URL-e). Mentzen: wyłącznie małe litery, myślniki, bez diakrytyków, **jeden slug na jeden temat**, walidacja przy publikacji.

**4. Literówki w URL-ach i nagłówkach.**
`due-dilligence`, `dokumetacja`, `nowe profi podatkowe`. W YMYL, gdzie oceniana jest staranność, literówka w nazwie usługi działa przeciwko zaufaniu. Korekta musi być krokiem publikacji, a URL po opublikowaniu jest trudny do naprawy.

**5. Nie zostawiać martwych klastrów w sitemapie.**
`koronawirus` (78 URL-i) i `polski-lad` (27) są nieaktualne od lat. Mentzen powinien mieć proces wygaszania: przekierowanie do aktualnego odpowiednika albo jawne oznaczenie treści jako archiwalnej.

**6. Nie wypełniać `meta keywords`.**
Google ignoruje ten znacznik od 2009 roku, a Rödl wykłada w nim konkurencji pełną listę fraz docelowych. Zero korzyści, realny koszt wywiadowczy.

**7. Nie przestawiać `lastmod` hurtowo.**
1 939 URL-i Rödla ma `lastmod` z 2025 roku — to wygląda na skutek migracji, nie na aktualizację treści. `lastmod` powinien odzwierciedlać realną zmianę merytoryczną.

**8. Pusty H1 na stronie głównej i marka zdublowana w `<title>`.**
Podstawowe błędy on-page. Warto sprawdzić, czy mentzen.pl ich nie powiela.

### Wniosek strategiczny

Rödl i Crido — dwaj najbliżsi profilem konkurenci — **oba mają słabe dane strukturalne**. Rödl podaje autorstwo i daty czytelnikowi, ale nie wystawia ich maszynowo, mimo posiadania realnie mocnej kadry eksperckiej.

To definiuje lukę dla mentzen.pl: **przewaga nie leży w tym, żeby napisać więcej artykułów, tylko w tym, żeby jako jedyny w tej grupie poprawnie opisać maszynowo posiadaną ekspertyzę.** Przy YMYL, gdzie Google waży E-E-A-T najmocniej, poprawnie wdrożone `Person` + `Article` z autorstwem + `FAQPage` + widoczne daty aktualizacji to przewaga osiągalna w tygodnie, a nie w lata — i taka, której żaden z dwóch głównych konkurentów obecnie nie ma.

Rezerwa: powyższe dotyczy sygnałów on-page. Bez danych o pozycjach, ruchu i profilu linkowym nie da się orzec, jak duża część widoczności Rödla wynika z marki i linków, a jaka ze struktury treści. Przed podjęciem decyzji budżetowych warto zestawić to z analizą widoczności w Ahrefs/Semrush — pamiętając, że `Crawl-delay: 10` w robots.txt Rödla może zaniżać kompletność tych danych. `[do weryfikacji]`

---

## Źródła

Wszystkie dostępy: 2026-08-28.

- `https://www.roedl.pl/robots.txt`
- `https://www.roedl.pl/sitemap.xml`
- `https://www.roedl.pl/pl/`
- `https://www.roedl.pl/pl/warto-wiedziec/`
- `https://www.roedl.pl/pl/warto-wiedziec/aktualnosci-podatkowe/`
- `https://www.roedl.pl/pl/warto-wiedziec/warto-wiedziec/podatek-cit/zaliczki-uproszczone-w-cit-zasady-stosowania-i-najnowsze-orzecznictwo`
- `https://www.roedl.pl/pl/warto-wiedziec/warto-wiedziec/podatek-cit/jpk-cit-korzystna-zmiana-stanowiska-dla-spolek-posiadajacych-zagraniczne-oddzialy`
- `https://www.roedl.pl/pl/warto-wiedziec/warto-wiedziec/podatek-pit/zmiany-w-pit-ryczalcie-i-cit-co-oznaczaja-dla-podatnikow-i-przedsiebiorcow`
- `https://www.roedl.pl/pl/uslugi/doradztwo-podatkowe/`
- `https://www.roedl.pl/pl/uslugi/doradztwo-podatkowe/ceny-transferowe-i-dokumentacja-podatkowa`
- `https://www.roedl.pl/pl/o-nas/kim-jestesmy/profil?PersonID=88`
- `https://www.roedl.pl/pl/wyszukiwarka-ekspertow/`
- `https://www.roedl.pl/pl/media/nasze-publikacje/newsletter/`
