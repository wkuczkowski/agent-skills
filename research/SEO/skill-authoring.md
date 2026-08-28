# Jak zbudować skill SEO: praktyki Agent Skills + proponowana struktura

Research pod przyszły skill SEO (Claude Code) dla mentzen.pl. Data: 2026-08-28. Źródła z URL-ami na końcu; odwołania do lokalnych plików (`writing-for-agents`, `conversion-ux`) po ścieżce.

## TL;DR

1. Skill to katalog z `SKILL.md` (frontmatter + markdown) i opcjonalnym `references/`. Ładowanie jest trzypoziomowe: opis zawsze w kontekście, ciało `SKILL.md` po odpaleniu, pliki referencyjne dopiero przy odczycie. Pisz pod ten mechanizm, nie pod czytelnika-człowieka ([docs](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices), 2026-08-28).
2. O odpaleniu skilla decydują metadane z frontmattera — `name` i `description`, przy czym praktycznie całą robotę wykonuje `description`. Skill, który się nie odpala, ma problem opisu, nie treści. Opis: trzecia osoba, „co robi" + „kiedy użyć", kluczowy przypadek na początku, jeden trigger na gałąź.
3. Trzymaj `SKILL.md` krótko (rekomendacja z dokumentacji: poniżej 500 linii „for optimal performance", nie limit walidacji; realny cel: poniżej 200). Ciało zostaje w kontekście na kolejne tury, więc każda linia to koszt powtarzalny. Obszerny materiał idzie do `references/`, maksymalnie jeden poziom w głąb.
4. Test rozgałęzienia rozstrzyga podział: inline to, czego potrzebuje każdy przebieg (np. „nigdy nie zmyślaj liczb"); za wskaźnik to, po co sięga tylko część przebiegów (np. checklista strony kategorii) (`writing-for-agents`).
5. Domyślne założenie: model już zna SEO. Do skilla trafia tylko to, co zmienia zachowanie względem domyślnego. Zdanie, które tego nie robi, jest no-opem i wylatuje w całości. Wartość skilla SEO to kontekst mentzen.pl, gradacja dowodowa i lista mitów, nie definicja meta description.
6. Formułuj pozytywnie. Zakaz wciąga zakazane zachowanie do kontekstu i czyni je bardziej dostępnym; zostaje tylko jako twarda barierka, w parze z celem pozytywnym.
7. Każdy krok procedury kończy się sprawdzalnym kryterium ukończenia („każda podstrona ma przypisaną frazę główną"), nie mglistym („zrozumienie osiągnięte").
8. Ewaluacje przed pisaniem: najpierw uruchom agenta bez skilla na realnych zadaniach mentzen.pl, zapisz porażki, zbuduj minimum 3 scenariusze, zmierz baseline, dopiero potem pisz minimalną treść zamykającą luki ([best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices), 2026-08-28).
9. SEO ma wyjątkowo dużo branżowego folkloru („1890 słów", „gęstość frazy 1–2%", „bounce rate to czynnik rankingowy"). Skill musi mieć plik `myths.md` z listą twierdzeń, których nigdy nie powtarzać, na wzór `references/evidence.md` z `conversion-ux`.
10. Nie pisz dziesięciu referencji naraz. Start: `anti-patterns.md`, `myths.md`, `on-page.md`. Reszta dochodzi, gdy obserwacja pokaże realną lukę.

## 1. Mechanika ładowania i jej konsekwencje

Progressive disclosure działa w trzech poziomach ([platform docs](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices), 2026-08-28):

| Poziom | Co się ładuje | Kiedy | Koszt |
|---|---|---|---|
| 1 | `name` + `description` (+ `when_to_use`) | zawsze, w system prompcie | stały, każda tura |
| 2 | całe ciało `SKILL.md` | gdy skill się odpali | jednorazowy, zostaje w kontekście |
| 3 | `references/*.md`, `scripts/*` | gdy model sięgnie po plik | zero do momentu odczytu |

Wnioski normatywne:

- Pisz `SKILL.md` jako standing instructions, nie jednorazowe polecenie. Claude Code nie odczytuje pliku ponownie; treść siedzi w kontekście na kolejne tury (z zastrzeżeniem auto-kompakcji niżej).
- Wrzucaj pełne tabele, checklisty per-typ-strony i katalogi antywzorców do `references/`. Kosztują zero, dopóki nieodczytane.
- Po auto-kompakcji Claude Code doczepia z powrotem tylko pierwsze 5000 tokenów każdego wywołanego skilla, przy wspólnym budżecie 25 000 tokenów ([Claude Code docs](https://code.claude.com/docs/en/skills), 2026-08-28). Najważniejsze rzeczy muszą być na górze `SKILL.md`, a szczegóły w referencjach.
- Gdy skill się nie odpala, poprawiaj opis, nie treść. Reszta pliku nie ma wpływu na trigger.

## 2. Podział treści: co inline, co za wskaźnikiem

Drabina informacyjna (za `writing-for-agents`): (1) krok w pliku, czyli co agent robi w kolejności; (2) referencja w pliku, konsultowana na żądanie (płaski zbiór równorzędnych reguł to poprawny układ, nie zapach); (3) referencja odsunięta do osobnego pliku za wskaźnikiem.

**Test rozgałęzienia** to najczystsze kryterium podziału: inline to, czego potrzebuje każda gałąź; za wskaźnik to, po co sięga tylko część gałęzi. Dla SEO: reguła „nigdy nie zmyślaj wolumenów" obowiązuje w każdym przebiegu, więc inline; checklista danych strukturalnych dla FAQPage dotyczy jednej gałęzi, więc `references/`.

Reguły twarde z dokumentacji ([best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices), 2026-08-28):

- `SKILL.md` poniżej 500 linii; powyżej dziel.
- Referencje maksymalnie jeden poziom w głąb. Przy zagnieżdżeniu (`SKILL.md` → `advanced.md` → `details.md`) model podgląda pliki przez `head -100` zamiast czytać w całości i pracuje na niepełnej informacji.
- Plik referencyjny dłuższy niż 100 linii dostaje spis treści na górze, żeby częściowy odczyt pokazywał zakres.
- Nazwy opisowe (`references/on-page-checklist.md`, nie `docs/file2.md`), ścieżki z ukośnikami w przód.
- Organizuj per-domena, gdy konteksty się wykluczają (wzorzec BigQuery z dokumentacji: `reference/finance.md` vs `reference/sales.md`). Dla SEO: pytanie o dane strukturalne nie powinno ładować reguł link buildingu.

### Szkielet wart skopiowania: `conversion-ux`

Lokalny skill `/home/wkuczkowski/projects/skills/skills/conversion-ux/` to sprawdzony wzorzec. `SKILL.md` ma 69 linii i zawiera wyłącznie: (1) ramę myślową w jednym akapicie, (2) uporządkowaną procedurę, gdzie kolejność kroków jest tezą skilla, (3) non-negotiables, (4) dyscyplinę dowodową jako tabelę gradacji z instrukcją, jak wolno mówić o każdej klasie twierdzeń, (5) indeks referencji z jawnym „przeczytaj plik dla powierzchni, nad którą pracujesz; nie czytaj wszystkich", (6) kalibrację, czyli opis domyślnego, rozpoznawalnego wyjścia AI w tej domenie z instrukcją traktowania go jako już wydanego. Osiem referencji po 64–127 linii, każda jedna powierzchnia lub jedna funkcja przekrojowa. Ten szkielet przenosi się na SEO jeden do jednego.

## 3. Zasady pisania treści

### Zwięzłość

- Zakładaj, że model już to wie. Test dla każdego zdania: czy zmienia zachowanie względem domyślnego? Jeśli nie, kasuj całe zdanie, nie skracaj go. Spór o to, czy zdanie jest no-opem, rozstrzyga uruchomienie, nie dyskusja.
- Nie wyjaśniaj, czym jest canonical ani meta description. Wyjaśniaj to, czego model sam nie ustali: niepisane konwencje mentzen.pl, powody decyzji, pułapki, których żaden config nie zdradza.
- Środowisko jest źródłem prawdy. Dokument powtarzający to, co widać w strukturze katalogów albo w `--help`, to cache, opłacalny tylko przy kosztownym odczycie (`writing-for-agents`).

### Normatywność

- Pisz, co robić, nie co jest. Ekspozycja nie steruje zachowaniem.
- Prompt pozytywny. „Nie pisz keyword stuffingu" aktywuje pojęcie; „każde wystąpienie frazy musi wynikać ze zdania, które i tak byś napisał" celuje w zachowanie docelowe. Zakaz zostawiaj tylko jako twardą barierkę niedającą się sformułować pozytywnie, i wtedy w parze z celem pozytywnym.
- Stopnie swobody dobieraj do kruchości: wysoka swoboda (heurystyki) tam, gdzie wiele dróg prowadzi do celu; niska (dokładna komenda, skrypt) tam, gdzie operacja jest krucha i kolejność ma znaczenie ([best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices), 2026-08-28; analogia z dokumentacji: wąska kładka nad przepaścią kontra otwarte pole). Cel: minimalny zbiór informacji w pełni określający oczekiwane zachowanie; minimalny nie znaczy krótki ([effective context engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents), 2026-08-28).
- Każdy krok kończ sprawdzalnym i wymagającym kryterium ukończenia. Mgliste zachęca do przedwczesnego zakończenia; „każda podstrona z listy ma przypisaną frazę główną" wymusza pełny przebieg.
- Używaj słów wiodących: zwięzłych terminów z pretrainingu, powtarzanych jako token, które kotwiczą cały obszar zachowania (przykład dla SEO: „intencja" jako jednostka pracy zamiast rozpisywania za każdym razem, czego szuka użytkownik). Wymyślony termin nie rekrutuje priorów, więc najpierw szukaj istniejącego.
- Jedna terminologia przez cały skill: zawsze „fraza kluczowa", nigdy na przemian „słowo kluczowe" / „keyword" / „zapytanie".
- Tam, gdzie liczy się styl wyjścia, przykłady wejście→wyjście biją opisy. Dawaj kanoniczne, różnorodne przykłady, nie listę przypadków brzegowych.

### Opis (trigger) i frontmatter

Opis to context pointer: mówi, czym jest materiał, i wylicza gałęzie, które mają go odpalić. Reguły:

- Trzecia osoba („Optymalizuje treści…", nie „Pomogę Ci…"). Opis trafia do system promptu; niespójna osoba psuje wykrywanie.
- Dwie części: co robi + kiedy użyć, terminami, których użytkownik faktycznie użyje.
- Kluczowy przypadek na początku. W Claude Code `description` + `when_to_use` są przycinane na 1536 znakach, a przy wielu skillach listing jest dodatkowo skracany do budżetu 1% okna kontekstu; `/doctor` pokazuje koszt ([Claude Code docs](https://code.claude.com/docs/en/skills), 2026-08-28).
- Jeden trigger na gałąź; synonimy tej samej gałęzi zwijaj. Tnij tożsamość, którą niesie samo ciało pliku.
- Limity platformy: `name` do 64 znaków (małe litery, cyfry, myślniki, bez „anthropic" i „claude"), `description` do 1024 znaków; żadne z tych pól nie może zawierać tagów XML. Nazwa: gerundialna (`optimizing-content`) albo rzeczownikowa (`seo-content`); unikaj `helper`, `utils`, `tools`, `data`.

Tryb wywołania: wybieraj model-invoked (domyślny) tylko wtedy, gdy agent naprawdę musi sięgać sam; inaczej `disable-model-invocation: true` i zero obciążenia kontekstu. Dla skilla SEO: model-invoked, bo ma się odpalać przy pisaniu i redakcji treści bez proszenia. Rozważ `paths` (globy ograniczające auto-ładowanie do katalogu z treścią), jeśli skill będzie projektowy. Inne pola: `when_to_use`, `allowed-tools`, `context: fork`, `argument-hint` ([Claude Code docs](https://code.claude.com/docs/en/skills), 2026-08-28).

## 4. Antywzorce

Struktura i kontekst:

| Antywzorzec | Dlaczego zawodzi |
|---|---|
| Przeładowany `SKILL.md` | blokuje progressive disclosure; wszystko ładuje się zawsze |
| Zagnieżdżone referencje (2+ poziomy) | model podgląda zamiast czytać |
| Monolityczna referencja z wykluczającymi się kontekstami | ładuje niepotrzebne domeny |
| Mgliste `name`/`description` | nigdy nie odpala albo odpala wszędzie |
| Niejasna intencja przy skryptach | model nie wie: uruchomić czy czytać |
| Sprawl | dokument za długi nawet przy żywych liniach; uwaga się rozrzedza |
| Duplikacja | jedno znaczenie w dwóch miejscach; koszt utrzymania i sztucznie zawyżona ranga |
| Osad (sediment) | martwe warstwy, bo dodawanie wydaje się bezpieczne, a usuwanie ryzykowne |
| Rozproszenie | definicja, reguły i zastrzeżenia jednego pojęcia w różnych miejscach zamiast pod jednym nagłówkiem |

Treść: informacje wrażliwe na czas trzymaj w sekcji „stare wzorce" w `<details>`, nie w głównym tekście. Dawaj jedno domyślne rozwiązanie plus jedno wyjście awaryjne, nie menu „A, B, C albo D". Bez voodoo constants (wartość bez uzasadnienia; jeśli autor nie wie, skąd liczba, model tym bardziej nie ustali). Skrypty mają obsługiwać błędy, nie odsyłać problemu do modelu. Nie zakładaj, że narzędzie jest zainstalowane.

Antywzorzec merytoryczny, krytyczny dla SEO: `conversion-ux` utrzymuje `references/evidence.md` z listą statystyk, których nigdy nie wolno powtarzać, bo są zmyślone albo źle zacytowane, mimo że krążą w branży. SEO ma ten problem w większej skali: „idealna długość tekstu to 1890 słów", „gęstość frazy 1–2%", „Google karze za duplicate content", „domain authority to metryka Google", „bounce rate jest czynnikiem rankingowym". Analogiczny plik (`myths.md`) jest w skillu SEO obowiązkowy, z gotowym zamiennikiem dla każdego mitu.

## 5. Ewaluacja i iteracja

Kolejność z dokumentacji: ewaluacje przed dokumentacją. Pięć kroków poniżej pochodzi z [best practices](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices) (2026-08-28); [Anthropic Engineering](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) formułuje tę samą zasadę ogólnie („start with evaluation", 2026-08-28).

1. Uruchom agenta na reprezentatywnych zadaniach bez skilla. Zapisz konkretne porażki i brakujący kontekst.
2. Zbuduj co najmniej trzy scenariusze testowe pokrywające te luki.
3. Zmierz baseline bez skilla.
4. Napisz minimalną treść zamykającą luki.
5. Iteruj: uruchom, porównaj z baseline, popraw.

Pętla dwóch instancji: „Claude A" pisze i refaktoryzuje skill, „Claude B" (świeża sesja z załadowanym skillem) używa go na realnych zadaniach; obserwacje z B wracają do A jako konkret.

Sygnały do obserwacji podczas testów:

- Nieoczekiwane ścieżki eksploracji → struktura nie jest tak intuicyjna, jak zakładałeś.
- Nieodwiedzone odsyłacze → wskaźnik musi być wyraźniejszy.
- Ten sam plik czytany za każdym razem → ta treść powinna być w `SKILL.md`.
- Plik, po który model nigdy nie sięga → zbędny albo źle zasygnalizowany.

Testuj na wszystkich modelach, na których skill będzie działać: Haiku potrzebuje więcej prowadzenia, Opus mniej tłumaczenia.

## Zastosowanie dla mentzen.pl: proponowana struktura skilla

Propozycja wstępna. [do weryfikacji] przez ewaluacje z sekcji 5: najpierw zobaczyć, gdzie agent bez skilla realnie zawodzi na treściach mentzen.pl, dopiero potem zatwierdzić zestaw plików.

### Frontmatter

```yaml
---
name: seo-mentzen
description: >
  Optymalizuje i recenzuje treści pod wyszukiwarki dla mentzen.pl — wpisy blogowe,
  strony usług, tytuły, meta description, nagłówki, linkowanie wewnętrzne, dane
  strukturalne. Używaj przy pisaniu lub redakcji tekstu na stronę, przy planowaniu
  tematu pod frazę, przy audycie istniejącej podstrony i gdy tekst „jest dobry",
  a nie zdobywa ruchu.
---
```

Model-invoked (bez `disable-model-invocation`), trzecia osoba, kluczowy przypadek na początku, gałęzie rozdzielone i nieduplikowane. Rozważyć `paths` ograniczające auto-ładowanie do katalogu z treścią, jeśli skill ma być projektowy, a nie osobisty.

### Sekcje `SKILL.md` (cel: pod 200 linii)

1. **Rama w jednym akapicie.** Kandydat na tezę porządkującą: *strona odpowiada na zapytanie albo nie istnieje*; jednostką pracy jest intencja wyszukiwania, nie fraza.
2. **Procedura, w kolejności** (kolejność jest tezą, jak w `conversion-ux`):
   1. Nazwij zapytanie i intencję: jedno zdanie o tym, czego szuka osoba, która ma trafić na stronę, i co uzna za odpowiedź.
   2. Sprawdź, czy strona na tę intencję już istnieje w serwisie (kanibalizacja). Jedna intencja, jeden URL.
   3. Zamknij sprawdzalne blokery techniczne: indeksowalność, canonical, status, tytuł, meta description, H1, wydajność.
   4. Napisz odpowiedź, potem dobierz frazy do tego, co i tak zostało napisane. Nie odwrotnie.
   5. Podepnij stronę do struktury: linkowanie wewnętrzne, anchor teksty, breadcrumbs, dane strukturalne.
   6. Nazwij, czego nie dało się ustalić bez danych (GSC, Ahrefs, logi serwera) i co by to rozstrzygnęło.
3. **Non-negotiables**, niezależne od briefu:
   - Nigdy nie zmyślaj liczb: wolumenów, pozycji, ruchu, liczby linków. Brakującą liczbę oznacz jako slot na dane do dostarczenia.
   - Nigdy nie wstawiaj frazy do zdania, którego nie napisałbyś bez niej.
   - Nigdy nie proponuj techniki naruszającej wytyczne spamowe Google (cloaking, ukryty tekst, doorway pages, kupowane linki, skalowane nadużycie treści).
   - Nigdy nie obiecuj pozycji ani terminu efektu.
   - Treść prawna i podatkowa: optymalizacja dotyczy formy, nigdy sensu merytorycznego ani stanu prawnego.
   - Nigdy nie publikuj tekstu bez zatwierdzenia przez autora merytorycznego (spójne z istniejącą checklistą autorów bloga w `~/projects/BLOG`).
4. **Dyscyplina dowodowa**, tabela gradacji na wzór `conversion-ux`, dopasowana do SEO:

   | Tag | Znaczenie | Jak o tym mówić |
   |---|---|---|
   | `(dokumentacja)` | wprost w Google Search Central | reguła, z odesłaniem |
   | `(zmierzone)` | badanie z metodą i próbą | reguła plus liczba i denominator |
   | `(korelacja)` | branżowe badania korelacyjne | nigdy jako przyczynowość |
   | `(folklor)` | powtarzane, nigdy niezmierzone | tylko jako hipoteza, nigdy jako „best practice" |
   | `(prawne)` | RODO, prawo prasowe, wymogi zawodowe | nienegocjowalne |
5. **Indeks referencji**: tabela plik → pytanie, z jawnym „przeczytaj plik dla powierzchni, nad którą pracujesz; nie czytaj wszystkich" i zamknięciem „zawsze kończ na `anti-patterns.md`".
6. **Kalibracja**: opis domyślnego wyjścia AI w SEO, traktowanego jako już wydane; wstęp „W dzisiejszych czasach…", H2 będące dosłownie frazą z narzędzia, FAQ doklejone bez powodu, podsumowanie powtarzające wstęp, „kompleksowe rozwiązanie". Każdy element musi być zasłużony briefem albo zastąpiony tym, czego czytelnik faktycznie szuka.

### Pliki `references/`

Jeden poziom w głąb; spis treści przy przekroczeniu 100 linii.

| Plik | Na jakie pytanie odpowiada |
|---|---|
| `intent-and-keywords.md` | intencja zapytania, fraza główna i wspierające, wykrywanie kanibalizacji, kiedy fraza nie zasługuje na osobną stronę |
| `on-page.md` | tytuł, meta description, H1–H3, struktura treści, obrazy i alt, linkowanie wewnętrzne, anchor teksty |
| `technical.md` | indeksowalność, canonical, robots, sitemap, przekierowania, Core Web Vitals, paginacja, parametry URL, hreflang przy wersji obcojęzycznej |
| `structured-data.md` | Schema.org dla mentzen.pl: Article, Person (autorzy), Organization, FAQPage, BreadcrumbList, LegalService, z warunkami użycia każdego typu |
| `eeat-and-ymyl.md` | treści podatkowe/prawne jako YMYL: autorstwo, biogramy, źródła, daty aktualizacji, rozgraniczenie porady od informacji; wiąże się z wtyczką `mentzen-autorzy` |
| `content-quality.md` | co odróżnia tekst, który odpowiada, od tekstu, który wypełnia; redakcja pod czytelnika, nie pod parser |
| `local-and-brand.md` | frazy brandowe, wyniki dla nazwiska i marki, profil firmy, sygnały lokalne [do weryfikacji, czy istotne dla kancelarii] |
| `measurement.md` | co da się zmierzyć, jak czytać GSC, które liczby są bez sensu, kiedy „przetestuj to" jest radą, a kiedy szumem |
| `myths.md` | twierdzenia SEO, których nigdy nie powtarzać, z gotowym zamiennikiem dla każdego |
| `anti-patterns.md` | katalog: jak wygląda → dlaczego zawodzi → co zamiast; zamknięcie każdego przebiegu |

### Czego świadomie nie robić w wersji pierwszej

- Nie pisać wszystkich dziesięciu referencji naraz. Start: `anti-patterns.md`, `myths.md`, `on-page.md`; te trzy zamykają najczęstsze porażki. Reszta dochodzi, gdy obserwacja pokaże lukę.
- Nie kopiować do skilla wiedzy ogólnej o SEO. Wartość jest w kontekście mentzen.pl (WordPress, wtyczka autorów, checklista autorów bloga, YMYL podatkowe), w gradacji dowodowej i w liście mitów.
- Nie wstawiać skryptów, dopóki nie pojawi się deterministyczna operacja (audyt techniczny listy URL-i, walidacja danych strukturalnych). Gdy dojdzie: `scripts/` z jawnym „uruchom", nie „przeczytaj".

## Checklista przed wydaniem skilla

- [ ] `description` konkretny, trzecia osoba, kluczowy przypadek na początku, „co robi" + „kiedy użyć"
- [ ] Jeden trigger na gałąź, bez synonimów tej samej gałęzi
- [ ] `SKILL.md` poniżej 500 linii (cel: poniżej 200)
- [ ] Referencje jeden poziom w głąb
- [ ] Pliki referencyjne powyżej 100 linii ze spisem treści
- [ ] Terminologia spójna w całym skillu
- [ ] Informacje wrażliwe na czas tylko w sekcji „stare wzorce"
- [ ] Przykłady konkretne, nie abstrakcyjne
- [ ] Kroki ze sprawdzalnymi kryteriami ukończenia
- [ ] Każde zdanie przechodzi test no-opa
- [ ] Reguły pozytywne; zakazy tylko jako twarde barierki
- [ ] Ścieżki z ukośnikami w przód
- [ ] Minimum trzy ewaluacje, zmierzony baseline bez skilla
- [ ] Przetestowane na modelach, na których będzie używany

## Źródła

- Skill authoring best practices, Claude Platform Docs: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices (dostęp 2026-08-28)
- Equipping agents for the real world with Agent Skills, Anthropic Engineering: https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills (dostęp 2026-08-28)
- Extend Claude with skills, Claude Code Docs: https://code.claude.com/docs/en/skills (dostęp 2026-08-28)
- Effective context engineering for AI agents, Anthropic Engineering: https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents (dostęp 2026-08-28)
- Lokalnie: `/home/wkuczkowski/.claude/skills/writing-for-agents/SKILL.md` + `SKILL-MECHANICS.md`; `/home/wkuczkowski/projects/skills/skills/conversion-ux/` (SKILL.md + references/)
