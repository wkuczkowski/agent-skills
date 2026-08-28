# Jak pisać Agent Skills — notatki pod przyszły skill SEO (mentzen.pl)

Data: 2026-08-28. Źródła na końcu pliku.

---

## 1. Czym jest skill i jak działa ładowanie

Skill to katalog z plikiem `SKILL.md` (frontmatter YAML + treść markdown), opcjonalnie z plikami pomocniczymi i skryptami. Mechanizm to **progressive disclosure** w trzech poziomach:

| Poziom | Co się ładuje | Kiedy | Koszt |
|---|---|---|---|
| 1. Metadane | `name` + `description` (+ `when_to_use`) | zawsze, w system prompcie | stały, na każdą turę |
| 2. `SKILL.md` | całe ciało pliku | gdy skill się odpali | jednorazowy, ale zostaje w kontekście |
| 3. `references/*.md`, `scripts/*` | pojedynczy plik | gdy model po niego sięgnie | zero do momentu odczytu |

Konsekwencje praktyczne:

- **Opis to jedyna rzecz, która decyduje o odpaleniu.** Reszta pliku nie ma wpływu na trigger. Skill, który się nie odpala, to prawie zawsze problem opisu, nie treści.
- **Treść `SKILL.md` zostaje w kontekście na kolejne tury** (Claude Code nie odczytuje pliku ponownie). Piszemy więc *standing instructions*, nie „zrób to raz". Każda linia to koszt powtarzalny.
- **Pliki w `references/` nie kosztują nic, dopóki nie zostaną odczytane.** To jest miejsce na obszerny materiał: pełne tabele, checklisty per-typ-strony, katalogi antywzorców.
- Po auto-kompakcji Claude Code doczepia z powrotem tylko pierwsze 5000 tokenów każdego wywołanego skilla, przy wspólnym budżecie 25 000 tokenów. Kolejny argument, żeby `SKILL.md` był krótki, a szczegóły siedziały w referencjach.

---

## 2. Struktura: co idzie do `SKILL.md`, a co do `references/`

### Drabina informacyjna (za `writing-for-agents`)

1. **Krok w pliku** — co agent robi, w kolejności. Poziom podstawowy.
2. **Referencja w pliku** — definicje i reguły konsultowane na żądanie. Płaski zbiór równorzędnych reguł to poprawny układ, nie zapach.
3. **Referencja odsunięta** — osobny plik za wskaźnikiem, ładowany dopiero gdy warunek się spełni.

**Test rozgałęzienia** (najczystsze kryterium): inline to, czego potrzebuje *każda* gałąź; za wskaźnik to, po co sięga *tylko część* gałęzi. Przykład SEO: reguły „nigdy nie zmyślaj danych" potrzebuje każdy przebieg → inline. Checklist dla strony kategorii usług potrzebuje jedna gałąź → `references/`.

### Reguły twarde z dokumentacji

- **`SKILL.md` poniżej 500 linii.** Powyżej — dziel.
- **Referencje maksymalnie jeden poziom w głąb od `SKILL.md`.** Zagnieżdżone odsyłacze (`SKILL.md` → `advanced.md` → `details.md`) powodują, że model podgląda pliki `head -100` zamiast czytać w całości i dostaje niepełną informację.
- **Plik referencyjny dłuższy niż 100 linii dostaje spis treści na górze.** Wtedy nawet częściowy odczyt pokazuje zakres.
- **Nazwy plików opisowe**: `references/on-page-checklist.md`, nie `docs/file2.md`. Ścieżki zawsze z ukośnikami w przód.
- **Organizacja per-domena, gdy konteksty się wykluczają.** Wzorzec BigQuery z dokumentacji: `reference/finance.md`, `reference/sales.md` — pytanie o przychody nie ładuje danych marketingowych.

### Wzorzec z `conversion-ux` (dobrze zrobiony skill, warto skopiować szkielet)

`SKILL.md` (69 linii) zawiera wyłącznie:

1. Ramę myślową w jednym akapicie („każdy element zadaje użytkownikowi pytanie").
2. **Uporządkowaną procedurę** (6 kroków, kolejność jest tezą skilla: najpierw usuń tarcie, dopiero potem perswazja).
3. **Non-negotiables** — twarde zakazy obowiązujące niezależnie od briefu.
4. **Dyscyplinę dowodową** — tabelę gradacji `(measured)` / `(mechanism)` / `(untested)` / `(legal)` z instrukcją, jak wolno mówić o każdej klasie.
5. **Indeks referencji** — tabela plik → na jakie pytanie odpowiada, z jawnym „Read the file for the surface you are working on. Do not read all of them."
6. **Kalibrację** — opis domyślnego, rozpoznawalnego wyjścia AI w tej domenie, z instrukcją traktowania go jako już wydanego.

Osiem plików w `references/` po 64–127 linii, każdy jedna powierzchnia lub jedna funkcja przekrojowa (`principles`, `anti-patterns`, `evidence`, `bright-lines` + pięć per-ekran).

To jest bezpośrednio przenośne na SEO.

---

## 3. Zasady pisania

### Zwięzłość

- **Domyślne założenie: model już to wie.** Dokładaj tylko kontekst, którego model nie ma. Test dla każdego zdania: czy to zmienia zachowanie względem domyślnego? Jeśli nie — to **no-op**, kasujemy całe zdanie, nie skracamy.
- Nie wyjaśniaj, czym jest PDF, meta description ani canonical. Wyjaśniaj, co jest specyficzne dla mentzen.pl i czego model sam nie ustali.
- **Środowisko też jest źródłem prawdy.** Dokument, który powtarza to, co widać w `package.json`, w strukturze katalogów albo w `--help`, jest cache'em — opłaca się tylko wtedy, gdy odczyt jest kosztowny. Cache'uj niepisaną konwencję, powód decyzji i pułapkę, której żaden config nie zdradza.

### Normatywność

- **Pisz, co robić, nie co jest.** Ekspozycja nie steruje zachowaniem.
- **Prompt pozytywny, nie negacja.** Sterowanie zakazem wciąga zakazane zachowanie do kontekstu i czyni je *bardziej* dostępnym. „Nie pisz keyword stuffingu" aktywuje pojęcie; „każde wystąpienie frazy musi wynikać ze zdania, które i tak byś napisał" celuje w zachowanie docelowe. Zakaz zostaje tylko jako twarda barierka, której nie da się sformułować pozytywnie — i wtedy w parze z celem pozytywnym.
- **Stopnie swobody dobierz do kruchości zadania.** Wysoka swoboda (tekst, heurystyki) tam, gdzie wiele dróg prowadzi do celu; niska (dokładny skrypt, dokładna komenda) tam, gdzie operacja jest krucha i kolejność ma znaczenie. Analogia z dokumentacji: wąska kładka nad przepaścią kontra otwarte pole.
- **Właściwa wysokość lotu** (z „effective context engineering"): nie koduj kruchej logiki if-else, ale też nie zostawiaj mglistych ogólników. Cel: „minimalny zbiór informacji, który w pełni określa oczekiwane zachowanie" — minimalny nie znaczy krótki.
- **Kryteria ukończenia.** Każdy krok kończy się warunkiem, po którym agent poznaje, że skończył. Warunek ma być sprawdzalny („każda podstrona z listy ma przypisaną frazę główną") i wymagający — mgliste („zrozumienie osiągnięte") zachęca do przedwczesnego zakończenia.
- **Słowa wiodące.** Zwięzły termin, który model już zna z pretrainingu, powtarzany jako token (nigdy jako zdanie), zakotwicza cały obszar zachowania w minimalnej liczbie tokenów. Jeśli triada jest rozpisana w trzech miejscach albo wskaźnik zużywa zdanie na jedną ideę — jest miejsce na słowo wiodące. Wymyślony termin nie rekrutuje żadnych priorów, więc najpierw szukaj istniejącego.
- **Spójna terminologia.** Jeden termin przez cały skill: zawsze „fraza kluczowa", nie na przemian „słowo kluczowe" / „keyword" / „zapytanie".
- **Przykłady wejście→wyjście** biją opisy tam, gdzie liczy się styl wyjścia. Dawaj kanoniczne, różnorodne przykłady, nie listę przypadków brzegowych.

### Pisanie opisu (trigger)

Opis to **context pointer**: mówi, czym jest materiał, i wylicza **gałęzie**, które mają go odpalić.

- **Trzecia osoba, zawsze.** „Optymalizuje treści pod wyszukiwarki…", nie „Pomogę Ci…" ani „Możesz tego użyć do…". Opis trafia do system promptu; niespójna osoba psuje wykrywanie.
- **Dwie części: co robi + kiedy użyć.** Konkretne terminy, których użytkownik faktycznie użyje.
- **Kluczowy przypadek użycia na początku.** W Claude Code `description` + `when_to_use` są przycinane na 1536 znakach, a przy wielu skillach listing jest dodatkowo skracany (budżet 1% okna kontekstu; `/doctor` pokazuje koszt).
- **Jeden trigger na gałąź.** Synonimy opisujące tę samą gałąź to jedna gałąź napisana dwa razy. Zwijaj.
- **Tnij tożsamość, którą niesie samo ciało pliku.**
- Limity: `name` do 64 znaków, tylko małe litery/cyfry/myślniki, bez słów „anthropic" i „claude". `description` do 1024 znaków (limit platformy), bez tagów XML.
- Nazewnictwo: forma gerundialna (`optimizing-content`) albo fraza rzeczownikowa (`seo-content`). Unikaj `helper`, `utils`, `tools`, `data`.

### Wybór trybu wywołania (Claude Code)

- **Model-invoked** (domyślnie): agent może odpalić sam, inne skille mogą sięgnąć. Cena: stałe obciążenie kontekstu opisem.
- **User-invoked** (`disable-model-invocation: true`): tylko człowiek przez `/nazwa`. Zero obciążenia kontekstu, ale człowiek musi pamiętać, że skill istnieje.

Model-invocation wybieraj tylko wtedy, gdy agent naprawdę musi sięgnąć sam albo inny skill musi go dosięgnąć. Dla SEO: **model-invoked**, bo chodzi o to, żeby odpalił się przy pisaniu i przeglądaniu treści bez proszenia.

Inne przydatne pola frontmattera w Claude Code: `when_to_use` (dodatkowe frazy triggerujące), `paths` (globy ograniczające auto-ładowanie do plików pasujących — sensowne, gdy skill ma dotyczyć tylko katalogu z treścią bloga), `allowed-tools`, `context: fork` (uruchomienie w subagencie), `argument-hint`.

---

## 4. Antywzorce

### Struktura i kontekst

| Antywzorzec | Dlaczego zawodzi |
|---|---|
| Przeładowany `SKILL.md` | blokuje progressive disclosure; wszystko ładuje się zawsze |
| Zagnieżdżone referencje (2+ poziomy) | model podgląda zamiast czytać, dostaje niepełną treść |
| Monolityczna referencja z wykluczającymi się kontekstami | ładuje niepotrzebne domeny |
| Mgliste `name`/`description` | fałszywe negatywy (nigdy nie odpala) albo fałszywe pozytywy (odpala wszędzie) |
| Niejasna intencja przy skryptach | model nie wie, czy uruchomić, czy czytać jako referencję |
| **Rozpełzanie się** (sprawl) | dokument po prostu za długi, nawet gdy każda linia jest żywa; uwaga się rozrzedza |
| **Duplikacja** | to samo znaczenie w dwóch miejscach — koszt utrzymania i sztuczne wywindowanie rangi na drabinie |
| **Osad** (sediment) | martwe warstwy, bo dodawanie wydaje się bezpieczne, a usuwanie ryzykowne |
| **Rozproszenie** | definicja, reguły i zastrzeżenia jednego pojęcia w różnych miejscach zamiast pod jednym nagłówkiem |

### Treść

- **Informacje wrażliwe na czas** („przed sierpniem 2025 użyj…"). Zamiast tego sekcja „stare wzorce" w `<details>`.
- **Za dużo opcji.** „Możesz użyć A, B, C albo D" dezorientuje. Daj domyślne rozwiązanie plus jedno wyjście awaryjne dla znanego przypadku brzegowego.
- **Zakładanie, że narzędzie jest zainstalowane.**
- **Ścieżki w stylu Windows.**
- **Voodoo constants** — wartości bez uzasadnienia. Jeśli autor nie wie, skąd liczba, model tym bardziej nie ustali.
- **Odsyłanie problemu do modelu w skryptach** zamiast obsłużenia błędu.

### Antywzorzec merytoryczny (lekcja z `conversion-ux`, krytyczna dla SEO)

`conversion-ux` ma osobny plik `references/evidence.md` z listą statystyk, których **nigdy nie wolno powtarzać**, bo są zmyślone albo źle zacytowane, mimo że krążą w branży. SEO ma dokładnie ten sam problem, w jeszcze większej skali: „idealna długość tekstu to 1890 słów", „gęstość frazy 1–2%", „Google karze za duplicate content", „domain authority to metryka Google", „bounce rate jest czynnikiem rankingowym". Analogiczny plik jest w skillu SEO obowiązkowy.

---

## 5. Ewaluacja i iteracja

Kolejność z dokumentacji — **ewaluacje przed dokumentacją**:

1. Uruchom agenta na reprezentatywnych zadaniach **bez** skilla. Zapisz konkretne porażki i brakujący kontekst.
2. Zbuduj co najmniej trzy scenariusze testowe pokrywające te luki.
3. Zmierz baseline bez skilla.
4. Napisz **minimalną** treść, która te luki zamyka.
5. Iteruj: uruchom, porównaj z baseline, popraw.

Pętla dwóch instancji: „Claude A" pomaga pisać i refaktoryzować skill, „Claude B" (świeża sesja z załadowanym skillem) go używa na realnych zadaniach. Obserwacje z B wracają do A jako konkret („zapomniał odfiltrować konta testowe, mimo że skill o tym mówi — może to za mało wyeksponowane?").

Na co patrzeć podczas obserwacji:

- **Nieoczekiwane ścieżki eksploracji** — czyta pliki w kolejności, której nie przewidziałeś? Struktura nie jest tak intuicyjna, jak zakładałeś.
- **Nieodwiedzone odsyłacze** — nie idzie za linkiem do ważnego pliku? Odsyłacz musi być wyraźniejszy.
- **Nadmierne poleganie** — czyta ten sam plik za każdym razem? Ta treść powinna być w `SKILL.md`.
- **Ignorowana treść** — nigdy nie sięga po plik? Jest zbędny albo źle zasygnalizowany.

Testuj na wszystkich modelach, na których skill będzie działać (Haiku potrzebuje więcej prowadzenia, Opus mniej tłumaczenia).

Rozstrzyganie sporu o no-op: dwie osoby, które kłócą się, czy zdanie jest no-opem, kłócą się o domyślne zachowanie modelu. Rozstrzyga uruchomienie, nie dyskusja.

---

## 6. Szkic struktury przyszłego skilla SEO dla mentzen.pl

Propozycja wstępna. Do weryfikacji przez ewaluacje (krok 1 z sekcji 5) — najpierw zobaczyć, gdzie agent bez skilla realnie zawodzi na treściach mentzen.pl.

### Nazwa i frontmatter

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

Uwagi: model-invoked (bez `disable-model-invocation`), trzecia osoba, kluczowy przypadek na początku, gałęzie rozdzielone i nieduplikowane. Rozważyć `paths` ograniczające auto-ładowanie do katalogu z treścią, jeśli skill ma być projektowy, a nie osobisty.

### Sekcje `SKILL.md` (cel: pod 200 linii)

1. **Rama w jednym akapicie.** Jedna teza, która porządkuje resztę. Kandydat: *strona odpowiada na zapytanie albo nie istnieje* — intencja wyszukiwania jest jednostką pracy, nie fraza.

2. **Procedura, w kolejności.** Kolejność jest tezą, tak jak w `conversion-ux`:
   1. Nazwij zapytanie i intencję. Jedno zdanie: czego szuka osoba, która ma trafić na tę stronę, i co uzna za odpowiedź.
   2. Sprawdź, czy strona na tę intencję już istnieje w serwisie (kanibalizacja). Jedna intencja, jeden URL.
   3. Zamknij blokery techniczne, które są sprawdzalne: indeksowalność, canonical, status, tytuł, meta description, H1, wydajność.
   4. Napisz odpowiedź, potem dobierz frazy do tego, co i tak zostało napisane. Nie odwrotnie.
   5. Podepnij stronę do struktury: linkowanie wewnętrzne, anchor teksty, breadcrumbs, dane strukturalne.
   6. Nazwij, czego nie dało się ustalić bez danych (GSC, Ahrefs, log serwera) i co by to rozstrzygnęło.

3. **Non-negotiables.** Twarde reguły niezależne od briefu. Kandydaci:
   - Nigdy nie zmyślaj liczb: wolumenów wyszukiwań, pozycji, ruchu, liczby linków. Jeśli wzorzec wymaga liczby, której nie ma — oznacz slot jako dane do dostarczenia.
   - Nigdy nie wstawiaj frazy do zdania, którego nie napisałbyś bez niej.
   - Nigdy nie proponuj techniki, która jest naruszeniem wytycznych Google dotyczących spamu (cloaking, ukryty tekst, doorway pages, kupowane linki, skalowane nadużycie treści).
   - Nigdy nie obiecuj pozycji ani terminu efektu.
   - Treść prawna i podatkowa: nigdy nie zmieniaj sensu merytorycznego dla frazy. Optymalizacja dotyczy formy, nie stanu prawnego.
   - Nigdy nie publikuj tekstu, którego nie zatwierdził autor merytoryczny (spójne z istniejącą checklistą autorów bloga).

4. **Dyscyplina dowodowa.** Tabela gradacji, wzorowana na `conversion-ux`, ale dopasowana do SEO:

   | Tag | Znaczenie | Jak o tym mówić |
   |---|---|---|
   | `(dokumentacja)` | wprost w dokumentacji Google Search Central | reguła, z odesłaniem |
   | `(zmierzone)` | badanie z podaną metodą i próbą | reguła plus liczba i denominator |
   | `(korelacja)` | badania korelacyjne branżowe | nigdy jako przyczynowość |
   | `(folklor)` | powtarzane, nigdy niezmierzone | tylko jako hipoteza, nigdy jako „best practice" |
   | `(prawne)` | RODO, prawo prasowe, wymogi zawodowe | nienegocjowalne |

5. **Indeks referencji.** Tabela plik → na jakie pytanie odpowiada, z jawnym „przeczytaj plik dla powierzchni, nad którą pracujesz; nie czytaj wszystkich" i z zamknięciem „zawsze kończ na `anti-patterns.md`".

6. **Kalibracja.** Opis domyślnego wyjścia AI w SEO, traktowanego jako już wydane: wstęp „W dzisiejszych czasach…", nagłówki H2 będące dosłownie frazą z narzędzia, sekcja FAQ doklejona bez powodu, akapit podsumowujący powtarzający wstęp, listy bez treści, „kompleksowe rozwiązanie". Każdy z tych elementów musi być zasłużony briefem albo zastąpiony tym, czego czytelnik faktycznie szuka.

### Pliki `references/`

Jeden poziom w głąb, każdy ze spisem treści jeśli przekroczy 100 linii.

| Plik | Na jakie pytanie odpowiada |
|---|---|
| `intent-and-keywords.md` | Jak ustalić intencję zapytania, jak dobrać frazę główną i wspierające, jak wykryć kanibalizację, kiedy fraza nie zasługuje na osobną stronę |
| `on-page.md` | Tytuł, meta description, H1–H3, struktura treści, obrazy i alt, długość, linkowanie wewnętrzne i anchor teksty |
| `technical.md` | Indeksowalność, canonical, robots, mapa strony, przekierowania, Core Web Vitals, paginacja, parametry URL, hreflang jeśli dojdzie wersja obcojęzyczna |
| `structured-data.md` | Schema.org dla treści mentzen.pl: Article, Person (autorzy), Organization, FAQPage, BreadcrumbList, LegalService — z warunkami, kiedy dany typ wolno użyć |
| `eeat-and-ymyl.md` | Treści podatkowe i prawne to YMYL: autorstwo, biogramy, źródła, daty aktualizacji, rozgraniczenie porady od informacji. Wiąże się z wtyczką `mentzen-autorzy` i drugim autorem |
| `content-quality.md` | Co odróżnia tekst, który odpowiada, od tekstu, który wypełnia. Redakcja pod czytelnika, nie pod parser |
| `local-and-brand.md` | Frazy brandowe, wyniki dla nazwiska i marki, profil firmy, sygnały lokalne — jeśli okaże się istotne dla kancelarii |
| `measurement.md` | Co da się zmierzyć, czego nie. Jak czytać GSC, jakie liczby są bez sensu, kiedy „przetestuj to" jest realną radą, a kiedy szumem |
| `myths.md` | Odpowiednik `evidence.md`: statystyki i reguły SEO, których nigdy nie powtarzać, z gotowym zamiennikiem dla każdej |
| `anti-patterns.md` | Katalog: jak wygląda → dlaczego zawodzi → co zrobić zamiast. Zamknięcie każdego przebiegu |

### Czego świadomie nie robić w wersji pierwszej

- Nie pisać wszystkich dziesięciu referencji naraz. Zacząć od `anti-patterns.md`, `myths.md` i `on-page.md` — to trzy pliki, które zamykają najczęstsze porażki. Reszta dochodzi wtedy, gdy obserwacja pokaże realną lukę.
- Nie kopiować do skilla tego, co model już wie o SEO. Wartość jest w: kontekście mentzen.pl (WordPress, wtyczka autorów, checklista autorów bloga, YMYL podatkowe), w gradacji dowodowej i w liście mitów.
- Nie wstawiać skryptów, dopóki nie będzie deterministycznej operacji do wykonania. Gdy dojdzie (audyt techniczny listy URL-i, walidacja danych strukturalnych) — `scripts/` z jawnym „uruchom", nie „przeczytaj".

---

## 7. Checklista przed wydaniem skilla

Za dokumentacją, z uzupełnieniami z `writing-for-agents`:

- [ ] `description` konkretny, w trzeciej osobie, z kluczowym przypadkiem na początku, zawiera „co robi" i „kiedy użyć"
- [ ] Jeden trigger na gałąź, bez synonimów tej samej gałęzi
- [ ] `SKILL.md` poniżej 500 linii (cel: poniżej 200)
- [ ] Odsyłacze do referencji jeden poziom w głąb
- [ ] Pliki referencyjne powyżej 100 linii mają spis treści
- [ ] Terminologia spójna w całym skillu
- [ ] Brak informacji wrażliwych na czas poza sekcją „stare wzorce"
- [ ] Przykłady konkretne, nie abstrakcyjne
- [ ] Kroki mają sprawdzalne kryteria ukończenia
- [ ] Każde zdanie przechodzi test no-opa (zmienia zachowanie względem domyślnego)
- [ ] Reguły sformułowane pozytywnie; zakazy tylko tam, gdzie są twardą barierką
- [ ] Ścieżki z ukośnikami w przód
- [ ] Co najmniej trzy ewaluacje, zmierzony baseline bez skilla
- [ ] Przetestowane na modelach, na których będzie używany

---

## Źródła

- [Skill authoring best practices — Claude Platform Docs](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices)
- [Equipping agents for the real world with Agent Skills — Anthropic Engineering](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills)
- [Extend Claude with skills — Claude Code Docs](https://code.claude.com/docs/en/skills)
- [Effective context engineering for AI agents — Anthropic Engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- `/home/wkuczkowski/.claude/skills/writing-for-agents/SKILL.md` oraz `SKILL-MECHANICS.md`
- `/home/wkuczkowski/projects/skills/skills/conversion-ux/` (SKILL.md + references/)
