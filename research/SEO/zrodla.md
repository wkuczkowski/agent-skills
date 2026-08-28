# Bibliografia researchu SEO — mentzen.pl

Wszystkie źródła użyte w 38 notatkach z `_notes/` (research prowadzony 2026-08-28; tego samego dnia
uzupełniony o notatki `uzup-*` — terminy podatkowe i etyka zawodowa ze źródeł pierwotnych — trzy
analizy konkurencji doradczo-audytorskiej i audyt kanałów YouTube), pogrupowane
tematycznie i zdeduplikowane. Ten plik ma jedno zadanie: **powiedzieć przyszłemu skillowi SEO,
któremu źródłu wolno wierzyć na słowo, które trzeba odświeżyć przed użyciem, a którego nie wolno
cytować w ogóle.** Nie streszcza treści — od tego są notatki tematyczne.

Data zestawienia: 2026-08-28.

---

## TL;DR

1. **Kanon to ~30 dokumentów Google + QRG** (plus 9 postów blogowych Search Central i pomoc Google).
   Wszystko inne w tej bibliografii jest komentarzem do nich albo dokumentacją narzędzi.
   Jeśli skill ma cytować jedno źródło na twierdzenie o tym, „czego
   chce Google", ma to być dokument z sekcji 2, nie blog z sekcji 4.
2. **Search Quality Rater Guidelines (2025-09-11, 182 s.) to najbardziej przekrojowy dokument
   researchu** — powołuje się na niego 5 z 38 notatek. Krąży pod dwoma URL-ami (mirror
   `static.googleusercontent.com` i `guidelines.raterhub.com`); **używaj tego drugiego jako
   kanonicznego, pierwszego jako fallbacku** — patrz sekcja 2.
3. **Dokumentacja Google jest datowana i ruchoma** (2025-12-10, 2026-04-15, 2026-06-15, 2026-07-10,
   2026-07-22, 2026-08-28 — zależnie od strony). Skill **musi pobierać ją na żywo, nie cache'ować
   liczb i cytatów**. Ta bibliografia notuje datę, na jaką każdy dokument był czytany.
4. **Osiem filmów YouTube, siedem z nich ma konflikt interesów** (Surfer, Ahrefs, Semrush, Senuto,
   Gotch SEO, Exposure Ninja, agencja Cherubina sprzedają narzędzie albo usługę). Ich tezy wchodzą do
   skilla wyłącznie z atrybucją („wg X z kanału Y") i nigdy jako liczba bez źródła pierwotnego.
5. **Trzy grupy badań z twardymi danymi**, na których opiera się cała warstwa AI search: Ahrefs
   (llms.txt, AI Overviews, pokrycie cytowań), Semrush (AI Mode, ruch referalny), arXiv 2311.09735
   (GEO, Princeton; praca przyjęta na KDD 2024). Wszystkie wtórne wobec Google, żadne recenzowane
   poza pracą GEO — i Ahrefs sam koryguje własną liczbę w obrębie jednego kursu (76% → 38% cytowań
   AIO z top 10; moduł 1 vs moduł 2 kursu `uza9GX0E2mw`).
6. **Sekcja 11 to lista zakazu.** Polskie blogi agencyjne z liczbami bez metodologii („44% kliknięć",
   „GBP to 32% sygnałów", „+30% ruchu z topical authority") nie wchodzą do skilla w żadnej formie.
7. **Sekcja 12 mapuje 15 twierdzeń `[do weryfikacji]` na konkretne pytania** (sześć domkniętych
   2026-08-28: zakres Indexing API, wielkość bazy Senuto, wsparcie rich resultów dla `LegalService`,
   polityki o self-serving reviews, a notatkami `uzup-*` — **terminy podatkowe** i **etyka
   zawodowa**). Z dawnych blokerów wdrożeniowych została wyłącznie retencja i limity GSC API —
   do zweryfikowania empirycznie zaraz po uzyskaniu dostępu do właściwości mentzen.pl.
8. **Sekcja 13 to gotchas dostępowe** — sześć zasobów, na których standardowy fetch przewraca się w
   przewidywalny sposób. To oszczędza agentowi cyklu prób i błędów przy każdym uruchomieniu.
9. **Nowa warstwa [A] dla prawa polskiego** (sekcje 7.1–7.2): teksty jednolite ustaw pobierane
   z ELI API Sejmu (`https://api.sejm.gov.pl/eli/acts/DU/{rok}/{poz}/text.pdf`) plus teksty
   pierwotne samorządów (KIDP, KIRP, NRA). Terminy podatkowe i granice etyczne marketingu cytować
   wyłącznie stamtąd — portale komercyjne nie były do tego potrzebne ani razu.

---

## 1. Konwencja rang zaufania

| Ranga | Co to znaczy | Jak używać w skillu |
|---|---|---|
| **[A]** | Źródło pierwotne: dokumentacja Google, pomoc Google, dokumentacja producenta API, publikacja naukowa | Cytuj wprost. Twierdzenie oparte na [A] nie wymaga hedge'owania. |
| **[B]** | Branżowe z ujawnioną metodologią i danymi: Ahrefs, Search Engine Land, Search Engine Journal, Semrush, Whitespark, Backlinko | Cytuj z nazwiskiem/wydawcą i datą. Liczby podawaj jako „wg badania X", nie jako fakt. |
| **[C]** | Wtórne, marketingowe, blogi agencyjne, materiały producentów wtyczek | Wolno użyć wyłącznie do kontekstu rynkowego i nazewnictwa. **Nigdy jako źródło liczby.** |

Reguła nadrzędna: **gdy [B] lub [C] mówi coś o Google, sprawdź w [A].** W tym researchu ta zasada
wyłapała co najmniej dwa fałszywe twierdzenia (rzekoma sekcja „Authors" w Search Central z lutego
2026; rzekome 67% serwisów YMYL dotkniętych December 2025 core update) — oba opisane w sekcji 11.

---

## 2. Kanon — źródła pierwotne Google

Rdzeń całego researchu. Kolumna „akt." to data ostatniej aktualizacji odczytana przy pobraniu.

| Dokument | URL | Akt. | Czego dotyczy | Notatki |
|---|---|---|---|---|
| **Search Quality Rater General Guidelines** (PDF, 182 s.) | `https://guidelines.raterhub.com/searchqualityevaluatorguidelines.pdf` (mirror: `https://static.googleusercontent.com/media/guidelines.raterhub.com/en//searchqualityevaluatorguidelines.pdf`) | 2025-09-11 | Definicja YMYL (2.3), E-E-A-T (3.4), reputacja (3.3.1–3.3.5), progi Lowest/Low (4.5–5.6), treści AI i scaled content abuse (4.6.5–4.6.7), High/Highest (7.0, 8.3) | eeat-ymyl, content-onpage, link-building, ai-search, polski-rynek |
| Creating helpful, reliable, people-first content | `https://developers.google.com/search/docs/fundamentals/creating-helpful-content` | 2025-12-10 | Pytania „Who / How / Why", byline autora, ujawnianie roli AI, zdanie o tym, że E-E-A-T **nie jest** czynnikiem rankingowym | eeat-ymyl, content-onpage, ai-search |
| Guide to optimizing for generative AI features | `https://developers.google.com/search/docs/fundamentals/ai-optimization-guide` | 2026-07-10 | Oficjalne stanowisko Google o widoczności w AI; zdanie, że **nie trzeba** llms.txt ani markdownu, by pojawić się w funkcjach generatywnych | ai-search, content-onpage |
| AI features and your website | `https://developers.google.com/search/docs/appearance/ai-features` | 2025-12-10 | Jak AI Overviews / AI Mode dobierają źródła; sterowanie przez `nosnippet`, `max-snippet`, `data-nosnippet` | ai-search |
| Google Search's core updates | `https://developers.google.com/search/docs/appearance/core-updates` | 2025-12-10 | Procedura analizy po core update; „several months" na odbudowę; instrukcja porównywania okien czasowych | eeat-ymyl, local-seo |
| Page experience | `https://developers.google.com/search/docs/appearance/page-experience` | 2025-12-10 | CWV w rankingu; kluczowe zdanie, że **poza CWV pozostałe elementy page experience nie podbijają pozycji** (HTTPS, interstitiale) | technical-seo |
| Spam policies (Search Essentials) | `https://developers.google.com/search/docs/essentials/spam-policies` | 2026-08-28 | Link spam, site reputation abuse, doorway pages — podstawa oceny taktyk linkowych i stron pod lokalizacje | link-building, local-seo |
| Article structured data (best practices dla `author`) | `https://developers.google.com/search/docs/appearance/structured-data/article` | 2025-12-10 | Osobne pole `author` per osoba, `type` + `url`/`sameAs`, **tylko imię i nazwisko w `author.name`** | eeat-ymyl, technical-seo |
| Local business structured data | `https://developers.google.com/search/docs/appearance/structured-data/local-business` | 2025-12-10 | Wymagane `address`+`name`, rekomendowane `geo`/`openingHours`; nakaz używania najwęższego podtypu | local-seo, technical-seo |
| Organization structured data | `https://developers.google.com/search/docs/appearance/structured-data/organization` | 2026-04-15 | `logo`, `sameAs`, `contactPoint`, `vatID`; wystarczy na home lub „O nas" | technical-seo |
| BreadcrumbList | `https://developers.google.com/search/docs/appearance/structured-data/breadcrumb` | 2025-12-10 | ≥2 `ListItem`; zalecenie odwzorowania ścieżki użytkownika, nie struktury URL | technical-seo |
| ProfilePage / Person | `https://developers.google.com/search/docs/appearance/structured-data/profile-page` | 2025-12-10 | `mainEntity`; Google wymienia „employee pages on company websites" jako poprawny przypadek | technical-seo |
| Review snippet structured data | `https://developers.google.com/search/docs/appearance/structured-data/review-snippet` | 2026-07-24 | Zasady dla ocen i recenzji — warunek brzegowy przed wdrożeniem `AggregateRating` | local-seo, technical-seo |
| **FAQPage (deprecated)** | `https://developers.google.com/search/docs/appearance/structured-data/faqpage` | — | Notka: funkcja **znika z wyników od 2026-05-07**. Kluczowy fakt „nie buduj już strategii FAQ pod rich results" | technical-seo |
| Search gallery (lista typów rich results) | `https://developers.google.com/search/docs/appearance/structured-data/search-gallery` | 2026-06-15 | Lista **bez FAQ i bez `Service`** (potwierdzone u źródła 2026-08-28) — czyli test, czy dany typ w ogóle ma szansę na rich result. Liczba pozycji `[do weryfikacji]`: notatka podaje 25, ponowne pobranie 2026-08-28 dało 28 | technical-seo |
| Structured data general policies | `https://developers.google.com/search/docs/appearance/structured-data/sd-policies` | 2026-07-10 | Oznaczaj tylko treść widoczną; brak gwarancji wyświetlenia; manual action za naruszenie | technical-seo |
| robots.txt intro | `https://developers.google.com/search/docs/crawling-indexing/robots/intro` | 2025-12-10 | robots.txt steruje crawlem, nie indeksowaniem | technical-seo |
| Block indexing (`noindex`) | `https://developers.google.com/search/docs/crawling-indexing/block-indexing` | 2025-12-10 | `noindex` przez meta / `X-Robots-Tag`; zakaz łączenia z `Disallow` | technical-seo |
| Consolidate duplicate URLs (canonical) | `https://developers.google.com/search/docs/crawling-indexing/consolidate-duplicate-urls` | 2026-07-10 | Hierarchia siły: 301 > canonical > sitemap; canonical to podpowiedź | technical-seo |
| Build and submit a sitemap | `https://developers.google.com/search/docs/crawling-indexing/sitemaps/build-sitemap` | 2026-07-08 | Limity 50 MB / 50 000 URL; `lastmod` tylko wiarygodny; `priority`/`changefreq` ignorowane | technical-seo |
| Managing crawl budget for large sites | `https://developers.google.com/search/docs/crawling-indexing/large-site-managing-crawl-budget` | 2026-07-22 | Progi adresatów (1 mln+ / 10 tys.+ stron); `noindex` **nie** oszczędza budżetu | technical-seo |
| Troubleshoot crawling errors (soft 404) | `https://developers.google.com/search/docs/crawling-indexing/troubleshoot-crawling-errors` | — | Definicja soft 404 — dotyczy pustych archiwów tagów i `/?s=` w WP | technical-seo |
| Link best practices (crawlable links) | `https://developers.google.com/search/docs/crawling-indexing/links-crawlable` | — | Linki muszą być `<a href>` w HTML — dotyczy paginacji i „load more" | content-onpage, technical-seo |
| Pagination and incremental page loading | `https://developers.google.com/search/docs/specialty/ecommerce/pagination-and-incremental-page-loading` | 2025-12-10 | Self-canonical na każdej stronie paginacji; **strona 1 nie jest kanoniczna dla serii**; `rel=next/prev` nieużywane | technical-seo |
| Control your title links | `https://developers.google.com/search/docs/appearance/title-link` | 2025-12-10 | Jak Google przepisuje title — punkt wyjścia do reguł on-page | content-onpage |
| Control your snippets | `https://developers.google.com/search/docs/appearance/snippet` | 2026-04-20 | `max-snippet`, `data-nosnippet` — sterowanie tym, co trafia do SERP i do AI | content-onpage, ai-search |
| A guide to Google Search ranking systems | `https://developers.google.com/search/docs/appearance/ranking-systems-guide` | — | Potwierdzenie, że helpful content system nie jest już osobnym systemem | eeat-ymyl |
| SEO Starter Guide | `https://developers.google.com/search/docs/fundamentals/seo-starter-guide` | — | Baza pojęciowa; punkt odniesienia dla rynku PL | polski-rynek |
| Search Central — documentation updates (changelog) | `https://developers.google.com/search/updates` | — | **Narzędzie weryfikacji**, nie źródło treści: tu sprawdzasz, czy plotka o zmianie w dokumentacji jest prawdziwa | eeat-ymyl |
| Search Status Dashboard — historia update'ów | `https://status.search.google.com/products/rGHU1u87FJnkP6W2GwMi/history` | na żywo | Kanoniczna lista core i spam updates z datami i długością rolloutu | eeat-ymyl |
| How Search Works — ranking results | `https://www.google.com/search/howsearchworks/how-search-works/ranking-results/` | — | Ogólny opis rankingu | polski-rynek |

**Blog Google Search Central** (posty, nie dokumentacja — datowane raz i już niezmienne):

| Post | URL | Data | Po co |
|---|---|---|---|
| Introducing Search Generative AI performance reports in Search Console | `https://developers.google.com/search/blog/2026/06/gen-ai-performance-reports` | 2026-06-03 | Jedyne oficjalne źródło o raporcie AI w GSC — podstawa pomiaru widoczności w AI |
| Succeeding in AI search | `https://developers.google.com/search/blog/2025/05/succeeding-in-ai-search` | 2025-05 | Stanowisko Google przed powstaniem dedykowanego guide'u |
| Mobile-first is here | `https://developers.google.com/search/blog/2023/10/mobile-first-is-here` | 2023-10-31 | Zakończenie mobile-first indexing — konsekwencja: treść ukryta na mobile nie istnieje |
| Changes to HowTo and FAQ rich results | `https://developers.google.com/search/blog/2023/08/howto-faq-changes` | 2023-08 | Pierwsze ograniczenie FAQ; tło dla wycofania w 2026 |
| Site reputation abuse — policy update | `https://developers.google.com/search/blog/2024/11/site-reputation-abuse` | 2024-11 | Granica dla treści sponsorowanych i gościnnych na własnej domenie |
| Search Analytics hourly data | `https://developers.google.com/search/blog/2025/04/san-hourly-data` | 2025-04 | Wymiar `HOUR` w GSC API |
| Performance data deep dive | `https://developers.google.com/search/blog/2022/10/performance-data-deep-dive` | 2022-10 | Jak GSC agreguje i anonimizuje dane |
| AI Mode expands to 40+ areas | `https://blog.google/products-and-platforms/products/search/ai-mode-expands-languages-locations/` | 2025-10-07 | Zasięg AI Mode — istotne przy ocenie ekspozycji rynku PL |
| AI Overviews in 200+ countries | `https://blog.google/products-and-platforms/products/search/ai-overview-expansion-may-2025-update/` | 2025-05 | Zasięg AI Overviews |

**Pomoc Google** (support.google.com) — dla GBP, Trends, GSC i GA4:

- `https://support.google.com/business/answer/7091` [A] — Improve your local ranking (relevance /
  distance / prominence). Podstawa całej notatki local-seo.
- `https://support.google.com/business/answer/3038177` [A] — Guidelines for representing your business.
  **Dokument, którego złamanie kończy się zawieszeniem profilu** — warunek brzegowy, nie porada.
- `https://support.google.com/contributionpolicy/answer/7400114` [A] — polityka treści użytkowników
  (opinie): zakaz zachęt, zakaz fałszywych recenzji.
- `https://support.google.com/webmasters/answer/9044175` [A] — raport ręcznych działań.
- `https://support.google.com/webmasters/answer/16984139`, `/16983858`, `/16908024`, `/12917991`,
  `/12917675`, `/12918484`, `/7687615` [A] — raporty GSC: skuteczność, dane generatywne, indeksacja,
  limity i anonimizacja zapytań.
- `https://support.google.com/trends/answer/4365533` [A] — skąd pochodzą dane Trends (próbkowanie,
  normalizacja) — kluczowe przy interpretacji.
- `https://support.google.com/trends/answer/3076011` [A] — Trending now + eksport RSS.
- `https://support.google.com/analytics/answer/9305788` [A] — nadawanie uprawnień w GA4.

---

## 3. Branżowe wysokozaufane [B] — pogrupowane tematycznie

### 3.1 AI search / GEO / AEO

Najgęściej obsadzony obszar researchu i najszybciej starzejący się. **Każda liczba stąd wymaga daty
przy cytowaniu.**

| Źródło | URL | Data | Co wnosi |
|---|---|---|---|
| Ahrefs — 137 tys. serwisów, 97% plików llms.txt nigdy nieodczytanych | `https://ahrefs.com/blog/llmstxt-study/` | 2026-06-15 | Najmocniejszy argument przeciw llms.txt; cytowany w 3 notatkach |
| Ahrefs — AI Overviews reduce clicks by 58% (update) | `https://ahrefs.com/blog/ai-overviews-reduce-clicks-update/` | 2026-02-04 | Aktualizacja wcześniejszego badania |
| Ahrefs — AI Overviews reduce clicks (oryginał) | `https://ahrefs.com/blog/ai-overviews-reduce-clicks/` | 2025-04 | Wersja pierwotna — **cytuj tylko update, nie oryginał** |
| Ahrefs — only 12% of AI-cited URLs rank in Google top 10 | `https://ahrefs.com/blog/ai-search-overlap/` | 2025-08-11 | Rozjazd między rankingiem a cytowaniem |
| Ahrefs — 76% of AI Overview citations from top 10 | `https://ahrefs.com/blog/search-rankings-ai-citations/` | 2025-07 | **Liczba nieaktualna** — sam Ahrefs koryguje ją później do ~38%; trzymać wyłącznie jako historię |
| Ahrefs — 86% top mentioned sources not shared across AI assistants | `https://ahrefs.com/blog/top-mentioned-sources-are-not-shared-across-ai-assistants/` | 2025-06-12 | Platformy AI mają rozłączne zbiory źródeł |
| Ahrefs — query fan-out | `https://ahrefs.com/blog/query-fan-out/` | — | Mechanika rozbijania promptu na podzapytania |
| Ahrefs — how to track and analyze AI traffic | `https://ahrefs.com/blog/track-analyze-ai-traffic/` | 2025-04-04 | Praktyka pomiaru w GA4 i logach |
| Ahrefs — AI brand visibility correlations (75 tys. marek) | `https://ahrefs.com/blog/ai-brand-visibility-correlations/` | 2025-12-12 | Co koreluje z widocznością w AI |
| Ahrefs — domain link metrics are poor mention predictors | `https://ahrefs.com/blog/domain-link-metrics-not-good-mention-predictors/` | — | DR/UR słabo przewidują wzmianki w AI |
| Semrush — AI Mode vs traditional search | `https://www.semrush.com/blog/ai-mode-comparison-study/` | 2025-07 | Porównanie AI Mode z klasycznym SERP |
| Semrush — impact of AI search on SEO traffic | `https://www.semrush.com/blog/ai-search-seo-traffic-study/` | 2025-07-21 | Wpływ na ruch |
| Semrush — how AI tools shape B2B buying | `https://www.semrush.com/blog/how-ai-shapes-b2b-buying/` | 2026-07-08 | **Najbliższe kontekstowi mentzen.pl** — zachowanie kupujących B2B |
| Semrush — AI referral traffic / GA4 AI Assistant channel | `https://www.semrush.com/blog/ai-referral-traffic/`, `https://www.semrush.com/blog/ga4-adds-ai-assistant-channel/` | — | Pomiar ruchu z asystentów |
| SEL — zero-click reaches 68% in early 2026 | `https://searchengineland.com/google-zero-click-searches-2026-study-479717` | 2026-06-09 | Skala zero-click |
| SEL — how schema markup fits into AI search, without the hype | `https://searchengineland.com/schema-markup-ai-search-no-hype-472339` | 2026-03-25, Aimee Jurenka | Kalibracja oczekiwań wobec schema |
| SEL — Google adds llms.txt check to Chrome Lighthouse | `https://searchengineland.com/google-llms-txt-chrome-lighthouse-478246` | — | Kontekst do sporu o llms.txt |
| SEL — does llms.txt matter? 10 sites tracked | `https://searchengineland.com/does-llms-txt-matter-467740` | — | Test empiryczny |
| SEL — GSC generative AI performance report data bug | `https://searchengineland.com/google-search-console-generative-ai-performance-report-in-search-data-bug-485215` | — | **Ostrzeżenie przed ufaniem świeżym danym z raportu AI w GSC** |
| SEL — why GA4 alone can't measure AI SEO | `https://searchengineland.com/why-ga4-alone-cant-measure-the-real-impact-of-ai-seo-468387` | — | Ograniczenia pomiaru |
| SEL — the future of B2B authority building in the AI search era | `https://searchengineland.com/b2b-authority-ai-search-era-456207` | — | Strategia B2B |
| SEL — guides: query fan-out, how to optimize for query fan-out, AI citations vs mentions, YMYL | `https://searchengineland.com/guide/query-fan-out`, `/guide/how-to-optimize-for-query-fan-out`, `/guide/ai-citations-vs-ai-mentions-data`, `/guide/ymyl` | aktualizowane | Guide'y SEL są **żywe** — przy cytowaniu podawaj datę dostępu, nie publikacji |
| SEJ — AI Overviews cut organic clicks 38% (field study) | `https://www.searchenginejournal.com/ai-overviews-cut-organic-clicks-38-field-study-finds/573145/` | 2026-04 | Niezależne potwierdzenie skali |
| SEJ — AI tools recommend brands but cite other sites | `https://www.searchenginejournal.com/ai-tools-recommend-brands-but-cite-other-sites-data-shows/587160/` | — | Rozdzielenie „wzmianka" od „cytowanie" |
| SEJ — gap between Google rankings and LLM citations | `https://www.searchenginejournal.com/new-data-finds-gap-between-google-rankings-and-llm-citations/561492/` | — | jw. |
| SEJ — SERP FAQ removal challenges schema's AI value | `https://www.searchenginejournal.com/serp-faq-removal-new-data-challenge-schemas-ai-search-value/574993/` | 2026-05-16 | Spina wycofanie FAQ z tezą o schema |
| SEJ — Google drops FAQ rich results from search | `https://www.searchenginejournal.com/google-drops-faq-rich-results-from-search/574429/` | 2026-05-10 | Relacja z wycofania |
| SEJ — how to track AI traffic in GA4 without undercounting | `https://www.searchenginejournal.com/ga4s-ai-assistant-channel-undercounts-your-ai-traffic-how-to-build-one-that-doesnt/580133/` | — | Regexy do własnej grupy kanałów |
| SEJ — can you use AI to write for YMYL sites? | `https://www.searchenginejournal.com/can-you-use-ai-to-write-for-ymyl/558945/` | — | **Liczba w artykule jest wtórna, badania źródłowego nie odnaleziono** `[do weryfikacji]` |
| SEJ — Google says llms.txt is purely speculative | `https://www.searchenginejournal.com/google-says-llms-txt-is-purely-speculative-for-now/577576/` | — | Wypowiedź Gary'ego Illyesa |
| Search Engine Roundtable — Google, AI, llms.txt | `https://www.seroundtable.com/google-ai-llms-txt-39607.html` | — | jw., drugie potwierdzenie |
| Stan Ventures — schema markup has no meaningful impact on AI citations | `https://www.stanventures.com/news/schema-markup-has-no-meaningful-impact-on-ai-citations-7231/` | [C] | **Omówienie eksperymentu Ahrefs na 1 885 stronach; oryginalny URL badania nie został potwierdzony** `[do weryfikacji]` |

### 3.2 E-E-A-T i YMYL

- [B] `https://searchengineland.com/google-updates-search-quality-raters-guidelines-adding-ai-overview-examples-ymyl-definitions-461908` — 2025-09-11.
  Relacja z wrześniowej aktualizacji QRG; zawiera cytat rzecznika Google („This update makes no change
  to our rating guidance"). Cytowane w 3 notatkach.
- [B] `https://searchengineland.com/google-eeat-misconceptions-437445` — Ryan Jones, 2024-02-13.
  **Najważniejsze źródło demitologizujące**: E-E-A-T nie jest czynnikiem rankingowym, badge'e „expert
  reviewed" bez weryfikowalności nie dają przewagi, cytat Danny'ego Sullivana o bio autorów.
- [B] `https://ahrefs.com/blog/eeat-audit/` — Despina Gavoyannis, red. Ryan Law, 2026-02-10.
  „E-E-A-T Audit: 220+ Markers" — najbardziej operacyjna lista sygnałów; źródło cytatu Muellera
  („You can't sprinkle some experiences on your web pages").
- [B] `https://ahrefs.com/blog/google-quality-raters-guidelines/` — omówienie QRG (skrót do nawigacji
  po 182-stronicowym PDF-ie).
- [C] `https://www.seozoom.com/google-search-quality-rater-guidelines/` — opis wtórny QRG, użyty
  wyłącznie jako pomocniczy w notatce o rynku PL.

### 3.3 Content, on-page, architektura treści

- [B] `https://ahrefs.com/blog/republishing-content/` — Louise Linehan, 2025-10-30. Kiedy odświeżać,
  a kiedy zostawić treść.
- [B] `https://ahrefs.com/blog/internal-links-for-seo/` — Chris Haines, 2026-03-10.
- [B] `https://ahrefs.com/blog/search-intent/`, `/long-tail-keywords/`, `/keyword-cannibalization/`,
  `/landing-page-seo/` — kanon Ahrefs dla intencji, długiego ogona, kanibalizacji i stron docelowych.
- [B] `https://searchengineland.com/internal-links-seo-best-practices-examples-tips-448047`,
  `/guide/search-intent-seo`, `/guide/long-tail-keywords-seo`, `/guide/keyword-cannibalization`,
  `/optimize-search-intent-tips-430857`, `/refreshing-content-drive-traffic-453280`,
  `/landing-pages-seo-conversions-447672`, `/wordpress-pages-vs-posts-385682`.
- [B] `https://www.semrush.com/blog/search-intent/`, `/keyword-cannibalization-guide/`,
  `/content-pruning/`, `/how-to-choose-long-tail-keywords/`, `/lead-generation-strategies/`.
- [B] `https://www.searchenginejournal.com/b2b-keyword-research/428962/`,
  `/how-to-write-ctas-for-b2b-a-call-to-action-guide-for-businesses-with-10-examples/457392/`,
  `/content-pruning-seo/375066/`,
  `/ask-an-seo-how-do-i-balance-content-that-converts-with-content-that-builds-brand-authority/566261/`.
- [B] `https://backlinko.com/hub/content/b2b` — B2B content marketing.

**Uwaga z notatki content-onpage:** wyszukiwania o „topical authority" zwróciły niemal wyłącznie blogi
agencyjne z niepotwierdzonymi statystykami. Żadne z nich nie weszło do researchu — patrz sekcja 12.

### 3.4 Linki i digital PR

- [B] `https://searchengineland.com/backlinks-seo-importance-442529` — James Allen, 2025-07-24.
- [B] `https://searchengineland.com/links-google-search-ranking-factor-gary-illyes-432422` — Illyes o
  spadku wagi linków.
- [B] `https://www.searchenginejournal.com/google-needs-very-few-links/514494/` — „Google Confirms Links
  Are Not That Important".
- [B] `https://www.searchenginejournal.com/brand-mentions-googles-algorithm/439801/` — wzmianki marki.
- [B] `https://www.searchenginejournal.com/ai-search-link-building-resolve-spa/567924/` — Michael
  Johnson, 2026-03-11.
- [B] `https://backlinko.com/digital-pr-strategies` — 2026-07-21, 6 strategii digital PR pod AI.
- [B] `https://searchengineland.com/guide/digital-pr-for-seo` — guide zbiorczy, w tym unlinked mentions.
- [B] `https://www.searchenginejournal.com/google-says-disavow-tool-not-part-of-normal-site-maintenance/543861/`
  i `/google-says-disavow-links/568928/` — stanowisko Google o disavow (czyli: prawie nigdy).
- [B] `https://searchengineland.com/branded-search-seo-452676` — 2025-02-27. **Kluczowe dla mentzen.pl**:
  ryzyko SERP-u brandowego zajętego przez osoby trzecie przy marce dzielącej nazwę z inną encją.

### 3.5 Local SEO

- [B] `https://whitespark.ca/local-search-ranking-factors/` — 2025-11-06. Local Search Ranking Factors
  2026; edycja 2026 dodała kategorię „AI Search Visibility".
- [B] `https://searchengineland.com/guide/service-area-pages` — Miriam Ellis, 2025-11-27. Kryteria
  sensownej strony pod obszar obsługi (vs. doorway).
- [B] `https://www.searchenginejournal.com/local-seo-multiple-locations/370704/` — Dan Taylor,
  aktualizacja 2026-06-18. Wielolokalizacyjność, uprawnienia, `Organization` vs `LocalBusiness`.
- [A] `https://schema.org/LegalService` — definicja typu (podtyp `ProfessionalService`).

### 3.6 Techniczne SEO i WordPress

- [B] `https://web.dev/articles/vitals` — 2024-10-31, statusy metryk CWV (INP „stable").
- [B] `https://www.searchenginejournal.com/google-completes-switch-to-mobile-first-indexing/499810/`.
- [B] `https://searchengineland.com/soft-404s-indexing-issues-traffic-collapse-477116` — soft 404 jako
  przyczyna załamania ruchu.
- [B] `https://perfmatters.io/docs/interaction-to-next-paint/` — praktyczna diagnostyka INP.
- [B] `https://www.searchenginejournal.com/wordpress-seo/wordpress-seo-checklist-get-ready-for-site-launch/`.
- [C] `https://www.joinindexed.com/blog/technical-seo-for-wordpress-the-settings-plugins-and-fixes-that-actually-matter`,
  `https://redshaw.consulting/rsc-seo-suite/wordpress-seo/wordpress-foundations/wordpress-seo-problems/`
  — praktyka porządkowania archiwów WP; **brak potwierdzenia w dokumentacji Google co do siły efektu.**
- [C] `https://yoast.com/choosing-the-right-wordpress-seo-plugin-for-your-business-yoast-vs-rank-math/`,
  `https://rankmath.com/blog/best-schema-markup-plugins-for-wordpress/` — **materiały producentów,
  stronnicze z definicji.** Porównanie wtyczek oparte na nich wymaga weryfikacji na aktualnych wersjach.

---

## 4. Badania i publikacje naukowe [A]

| Publikacja | URL | Po co w researchu |
|---|---|---|
| GEO: Generative Engine Optimization (Aggarwal i in., Princeton) | `https://arxiv.org/pdf/2311.09735` | **Jedyne recenzowalne źródło warstwy GEO.** Podstawa tezy, że cytowanie własnych danych, statystyk i cytatów podnosi widoczność w odpowiedziach generatywnych |
| Reliability of Google Trends (PMC8186442) | `https://www.ncbi.nlm.nih.gov/pmc/articles/PMC8186442/` | Powtarzalność i szum w danych Trends |
| The (mis)use of Google Trends data in the social sciences (Social Science Research) | `https://www.sciencedirect.com/science/article/pii/S0049089X24001212` | Systematyczne błędy interpretacji Trends |
| Econbrowser — can Google Trends data be replicated? | `https://econbrowser.com/archives/2017/06/guest-contribution-can-google-trends-data-be-replicated` | Problem niepowtarzalności próbki między odczytami |
| medRxiv — walidacja Trends (preprint) | `https://www.medrxiv.org/content/10.1101/2020.12.29.20248969.full.pdf` | jw., kontekst metodologiczny |

**Konsekwencja dla skilla:** każdy odczyt Google Trends jest próbką. Nie porównuj odczytów z różnych
dni jako liczb — porównuj kształt krzywej i powtarzalność szczytów w oknie 5 lat.

---

## 5. Dokumentacja narzędzi i API [A]

### 5.1 Google Search Console API

`https://developers.google.com/webmaster-tools/v1/searchanalytics/query` · `/v1/searchanalytics` ·
`/v1/urlInspection.index/inspect` · `/v1/urlInspection.index/UrlInspectionResult` · `/v1/sitemaps` ·
`/v1/api_reference_index` · `/v1/prereqs` · `/v1/how-tos/authorizing` · `/v1/how-tos/all-your-data` ·
**limity**: `https://developers.google.com/webmaster-tools/limits` (1 200 QPM per site dla Search
Analytics) · zakresy OAuth `https://www.googleapis.com/auth/webmasters[.readonly]` ·
`https://console.cloud.google.com/apis/library/searchconsole.googleapis.com`.

Indexing API (i dlaczego **odpada** dla bloga): `https://developers.google.com/search/apis/indexing-api/v3/quickstart`
i `/v3/prereqs` — obsługuje wyłącznie `JobPosting` i `BroadcastEvent`.

BigQuery bulk export: `https://cloud.google.com/bigquery/pricing` `[do weryfikacji — aktualnych stawek
nie udało się pobrać]`.

Źródła wtórne o limitach GSC — **wszystkie oznaczone `[do weryfikacji]`, bo liczb nie ma w oficjalnej
dokumentacji**: `https://www.lumar.io/blog/industry-news/google-update-search-console-api-now-includes-16-months-of-data/`
(retencja 16 miesięcy), `https://seotesting.com/google-search-console/data-limitations/`,
`https://weld.app/blog/google-search-console-50k-limit` i
`https://www.analyticsedge.com/blog/download-over-25000-rows-from-google-search-console-api/`
(sufit ~50 000 wierszy/dobę), `https://searchengineland.com/google-search-analytics-api-gains-hourly-break-down-for-past-10-days-454167`
(dane godzinowe).

Biblioteki i wrappery [C/A mieszane, oceniane po aktywności repo]:
`https://pypi.org/project/searchconsole/` · `https://github.com/joshcarty/google-searchconsole` ·
`https://www.npmjs.com/package/googleapis` · `https://googleapis.dev/nodejs/googleapis/latest/searchconsole/classes/Searchconsole.html` ·
`https://www.jcchouinard.com/searchconsole-api-wrapper-python/` ·
`https://www.codeconcisely.com/posts/how-to-use-google-search-console-api-in-node-js/` ·
`https://stateful.com/blog/google-search-console-nodejs` ·
`https://trevorfox.com/2024/07/google-search-console-tables-in-bigquery/`.
CLI/MCP (przegląd, jakość zróżnicowana): `github.com/awkoy/gsc-cli`, `/benedict2310/gsc-cli`,
`/jpreagan/gsc-cli`, `/ivankristianto/google-search-console-cli`, `/ahonn/mcp-server-gsc`,
`/surendranb/google-search-console-mcp`, `/garethcull/search-console-mcp`,
`/acamolese/google-search-console-mcp`, `/ncosentino/google-search-console-mcp`,
`/sarahpark/google-search-console-mcp`, `/Shin-sibainu/google-search-console-mcp-server`.

### 5.2 Google Analytics 4 — Data API v1

`https://developers.google.com/analytics/devguides/reporting/data/v1` (+ `/basics`, `/advanced`,
`/quotas`, `/api-schema`, `/changelog`, `/quickstart-client-libraries`,
`/reporting-data-expectations` — sampling, kardynalność, progowanie) ·
`/rest/v1beta/properties/runReport` · `/rest/v1beta/properties/getMetadata` ·
`/rest/v1beta/PropertyQuota` · Admin API `/config/admin/v1/rest/v1beta/accountSummaries/list` ·
zakres `https://www.googleapis.com/auth/analytics.readonly`.
Klienty: `https://pypi.org/project/google-analytics-data/` ·
`https://www.npmjs.com/package/@google-analytics/data` ·
`https://googleapis.dev/python/analyticsdata/latest/data_v1beta/beta_analytics_data.html` ·
oficjalny MCP: `https://github.com/googleanalytics/google-analytics-mcp`.

### 5.3 Google Trends

Oficjalne: `https://developers.google.com/search/blog/2025/07/trends-api` (ogłoszenie alfy) ·
`https://developers.google.com/search/apis/trends` (formularz dostępu — **wciąż alfa po ponad roku**) ·
`https://trends.google.com/trending/rss?geo=PL` (darmowy RSS dla Polski).
Biblioteki: `https://github.com/GeneralMills/pytrends` — **zarchiwizowany od kwietnia 2025**, issues
`#492`, `#561`, `#625` dokumentują rozpad; aktywne forki `https://github.com/flack0x/trendspyg`,
`https://github.com/sdil87/trendspy` (`https://pypi.org/project/trendspy/`),
`https://github.com/yiromo/pytrends-modern`.
Pośrednicy płatni: `https://serpapi.com/google-trends-api` ·
`https://docs.dataforseo.com/v3/keywords_data/google_trends/explore/live/` ·
`https://meetglimpse.com/google-trends-api/`.
Prasa: `https://ppc.land/google-opens-alpha-testing-for-new-trends-api-targeting-developers-and-journalists/` ·
`https://www.searchenginejournal.com/google-trends-api-alpha-launching-breaking-news/551935/`.

### 5.4 Dane o frazach i SERP

- **Senuto** (jedyny z listy zbudowany wokół polskiego Google): `https://wiki.senuto.com/pl/articles/206536-api-przewodnik` ·
  `https://www.senuto.com/pl/mcp/` + endpoint `https://mcp.senuto.com/mcp` · `https://www.senuto.com/pl/cennik/` ·
  `https://www.senuto.com/pl/baza-slow-kluczowych/` · raport kanibalizacji
  `https://wiki.senuto.com/l/pl/analiza-widocznosci/analiza-widocznosci-raport-kanibalizacja-fraz` ·
  [C] `https://www.senuto.com/pl/blog/jak-sprawdzic-popularnosc-slow-kluczowych/`,
  [C] `https://mateuszkozlowski.pl/blog/skuteczne-pozycjonowanie-stron-w-google-przy-uzyciu-senuto-api/`.
  **Rozbieżność rozstrzygnięta:** Senuto deklaruje **80 mln fraz** dla Polski (sprawdzone na
  `senuto.com/pl/baza-slow-kluczowych/`, 2026-08-28) — tak samo jak notatka narzedzia-keywords.
  Podawane w notatce polski-rynek „14–19 mln" jest nieaktualne, nie cytować.
- **DataForSEO** (rekomendowany pay-as-you-go): `https://dataforseo.com/apis/serp-api` + `/pricing` ·
  `https://docs.dataforseo.com/v3/serp/google/organic/live/advanced/` ·
  `/v3/keywords_data/google_ads/search_volume/live/` · `/v3/keywords_data/google_ads/locations/` ·
  `/v3/dataforseo_labs/google/keyword_ideas/live/` · `/v3/ai_optimization-overview/`,
  `/v3/ai_optimization-llm_mentions-overview/`, `/v3/ai_optimization-llm_responses-overview/` ·
  cenniki `/pricing/serp/google-organic-serp-api`, `/pricing/keywords-data/google-ads`,
  `/pricing/dataforseo-labs/dataforseo-google-api`, `/pricing/backlinks/backlinks`,
  `/pricing/ai-optimization/llm-responses`, `/pricing/ai-optimization/llm-scraper` ·
  `https://dataforseo.com/help-center/dataforseo-labs-api-vs-google-ads-api` ·
  `https://app.dataforseo.com/api-access` · klient `https://github.com/dataforseo/PythonClient` ·
  **oficjalny MCP** `https://github.com/dataforseo/mcp-server-typescript`.
- **Serper**: `https://serper.dev/` (+ [C] `https://apiserpent.com/blog/serper-pricing-credits-explained`).
- **SerpApi**: `https://serpapi.com/pricing`, `/integrations` (+ [C] `https://apiserpent.com/blog/serpapi-pricing-explained`).
- **Semrush API**: `https://developer.semrush.com/api/v4/get-started/api-access/` ·
  `https://developer.semrush.com/api/v3/analytics/keyword-reports/` · `/v3/analytics/basic-docs/`.
- **Ahrefs API**: `https://docs.ahrefs.com/` · `/en/api/reference/keywords-explorer` (+ `/get-overview`) ·
  `/en/api/docs/limits-consumption` · `https://ahrefs.com/pricing`, `https://ahrefs.com/api/pricing`.
- **Google Ads API** (darmowe wolumeny, ale z warunkiem): `https://developers.google.com/google-ads/api/docs/api-policy/access-levels`
  (poziomy dostępu i „permissible use") · `/rest/reference/rest/v18/customers/generateKeywordIdeas` ·
  `https://pypi.org/project/google-ads/`.
- **Google Programmable Search / Custom Search**: `https://developers.google.com/custom-search/v1/overview`.
- **Bing Webmaster Tools API** (darmowe dane o linkach): `https://learn.microsoft.com/en-us/bingwebmaster/getting-started` ·
  `/bingwebmaster/api-protocols` · `https://learn.microsoft.com/en-us/dotnet/api/microsoft.bing.webmaster.api.interfaces.iwebmasterapi.getlinkcounts` ·
  `https://learn.microsoft.com/en-us/answers/questions/5939109/...` · endpoint `https://ssl.bing.com/webmaster/api.svc/json/`.
- [C] `https://busyless.space/seo-apis/moz` — porównanie API Moz.
- [C] `https://www.surmado.com/blog/best-ai-visibility-tools-2026` — przegląd narzędzi AI visibility.
- **Brand24** `https://brand24.com/api/` — **strona zwraca 404**; istnienie i zakres API `[do weryfikacji]`.

### 5.5 Wydajność, crawl, indeksacja

- PageSpeed Insights API: `https://developers.google.com/speed/docs/insights/v5/get-started` ·
  `/v5/reference/pagespeedapi/runpagespeed` · endpoint `https://www.googleapis.com/pagespeedonline/v5/runPagespeed` ·
  `https://github.com/GoogleChromeLabs/psi`.
- CrUX API (dokąd Google przenosi dane polowe z PSI): `https://developer.chrome.com/docs/crux/api` ·
  `/docs/crux/history-api` · endpointy `https://chromeuxreport.googleapis.com/v1/records:queryRecord`,
  `:queryHistoryRecord`.
- Lighthouse / LHCI: `https://github.com/GoogleChrome/lighthouse` ·
  `https://github.com/GoogleChrome/lighthouse-ci/blob/main/docs/getting-started.md` ·
  `https://unlighthouse.dev/` (skan całej witryny).
- Crawlery: `https://advertools.readthedocs.io/en/master/advertools.spider.html` (+ `/readme.html`) ·
  `https://www.screamingfrog.co.uk/seo-spider/user-guide/general/`.
- Walidacja schema: `https://schema.org/docs/validator.html` ·
  `https://github.com/schemaorg/schemaorg/blob/main/docs/validator.md`.
- IndexNow: `https://www.indexnow.org/documentation` · `https://www.indexnow.org/searchengines.json` ·
  `https://api.indexnow.org/indexnow` · `https://www.bing.com/indexnow` · `https://www.bing.com/webmasters`.
- WordPress / Yoast REST: `https://developer.wordpress.org/rest-api/reference/posts/` ·
  `/rest-api/using-the-rest-api/authentication/` · `https://developer.yoast.com/customization/apis/rest-api/`.

---

## 6. Filmy YouTube

Osiem materiałów. **Siedmiu autorów sprzedaje narzędzie lub usługę omawianą w filmie** — to nie
dyskwalifikuje treści, ale wymusza atrybucję i oddzielenie metody od autopromocji. Notatki robią to
konsekwentnie znacznikami `[opinia]` / `[dane]` / `[promocja narzędzia]`.

| Tytuł | Kanał / autor | URL | Język | Konflikt interesów | Co wnosi do skilla |
|---|---|---|---|---|---|
| The Complete SEO & AI SEO Course for 2026 | Surfer Academy (@SurferSEO) | `https://www.youtube.com/watch?v=7DRO4rEIHDk` | en | **Tak** — producent Surfera | Rama „SEO to fundament widoczności w AI"; test intencji w 30 s; framework keyword sweet spot. **Zero treści o YMYL, E-E-A-T i rynku PL.** Liczby w filmie podane bez źródeł |
| AI SEO Course for Beginners: Complete AEO Tutorial (~87 min) | Ahrefs, Samo Cerar | `https://www.youtube.com/watch?v=uza9GX0E2mw` | en | **Tak** — Ahrefs | Najlepszy przegląd mechaniki AI search: trening vs retrieval, query fan-out, probabilistyczność cytowań, różnice między AIO / ChatGPT / Perplexity / AI Mode. **Sam koryguje w module 2 własną liczbę z modułu 1** (76% → 38%) |
| AI overviews w praktyce – jak odzyskać ruch? (webinar, ~2,5 h) | Senuto; prelegent Szymon Parzych (Vestigio), prowadzi Mateusz Żegrocki | `https://www.youtube.com/watch?v=MKwi0qmVSS8` | pl (auto-napisy) | **Tak** — Senuto | **Jedyne źródło z danymi o AI Overviews dla polskiego SERP-u.** Efekt „paszczy krokodyla" w GSC; realistyczny cel cytowań 20–30% fraz; 20–40% odzysku ruchu. Auto-napisy psują nazwy własne — liczby weryfikować u źródła |
| 12 Brand Authority Signals That Make AI Recommend You | Semrush | `https://www.youtube.com/watch?v=VOb_QjlrgpE` | en | **Tak** — Semrush | 12 sygnałów autorytetu z przypisanym narzędziem pomiaru; teza (za Gianluką Fiorellim), że popytu nawigacyjnego nie da się sfałszować. Bezpośrednio przekłada się na pomiar marki „Mentzen" |
| Free Local SEO Course: My Full 2026 System | Nathan Gotch (Gotch SEO) | `https://www.youtube.com/watch?v=bfBwk2KK9jc` | en | **Tak** — Rankability | Model earned / owned / rented; kategoria główna GBP jako najtańsza dźwignia. Rynek USA, B2C usługowe — przenosi się częściowo |
| I Built a Full B2B SEO Strategy in 11 Minutes (LIVE) | Sam Dunning — Breaking B2B | `https://www.youtube.com/watch?v=N1m9hlMsMKg` | en | Umiarkowany | **Najbliższy sytuacji kancelarii.** „Money keyword matrix" (4 kolumny: nazwy oferty × nisze × konkurenci × problemy) dla usługi drogiej i rzadko kupowanej |
| How To Create a Cutting Edge SEO Strategy for 2026 (podcast „Dojo", odc. 7) | Exposure Ninja — Dale Davies i Charlie Marchant | `https://www.youtube.com/watch?v=vrGLaJOAKas` | en (auto), publ. 2025-10-17 | **Tak** — agencja | Piramida SEO (techniczne → on-site → off-site); **rozdzielanie spadków ruchu na informacyjne i komercyjne przed wyciąganiem wniosków**; case DSLD Mortgage (za dużo niekwalifikowanych leadów) |
| Jak planować treści pod SEO i AI? Poznaj prosty, ale skuteczny proces | Maciej Cherubin | `https://www.youtube.com/watch?v=yUp_Emk8KAo` | pl (auto) | **Tak** — agencja SEO | Kolejność: struktura → arkusz optymalizacyjny → widoki → grafika → treść. Arkusz „jedna podstrona = jeden wiersz" jest gotowy do użycia. **Mimo tytułu nie omawia klastrów tematycznych** |

Kanały własne klienta (obiekt badania, nie źródło): `https://www.youtube.com/@kancelariamentzen`
i osobisty `https://www.youtube.com/@SlawomirMentzen` — zaudytowane w `_notes/mentzen-youtube.md`;
inwentarz i metodologia w sekcji 9.

---

## 7. Polski kontekst: rynek, prawo zawodowe, opinie

### 7.1 Prawo i samorządy zawodowe — rozstrzygnięte u źródeł pierwotnych [A]

**Status: bloker nr 2 z sekcji 12 domknięty 2026-08-28** dwiema notatkami:
`_notes/uzup-etyka-doradcy.md` (doradcy podatkowi) i `_notes/uzup-etyka-prawnicy.md` (radcowie
prawni i adwokaci). Werdykty weszły do `eeat-ymyl.md` §7a, `link-building.md` §4a i `local-seo.md`.

Źródła pierwotne [A] (wszystkie dostęp 2026-08-28):

- **KIDP:** Zasady etyki doradców podatkowych, t.j. — załącznik do uchwały KRDP nr 40/2026
  z 13.04.2026 (PDF:
  `https://kidp.pl/storage/files/prawo/prawo-korporacyjne/zasady_etyki_doradcow_podatkowych_40_2026.pdf`;
  **linkować do strony** `https://kidp.pl/dla-doradcow/podstawy-prawne/prawo-korporacyjne`, bo adres
  PDF zmienia się z numerem uchwały — patrz sekcja 13) + ustawa o doradztwie podatkowym
  (t.j. Dz.U. 2021 poz. 2117 ze zm.; uchylenie ustawowego zakazu reklamy: Dz.U. 2010 nr 122 poz. 826,
  w życie 7.08.2010).
- **KIRP:** Kodeks Etyki Radcy Prawnego, t.j. uchwała Prezydium KRRP nr 884/XI/2023 + Regulamin
  wykonywania zawodu (uchwała KRRP nr 124/XI/2022), oba w życie 1.01.2023 —
  `https://kirp.pl/kodeks-etyki-radcy-prawnego/` (PDF:
  `https://kirp.pl/wp-content/uploads/2023/02/kodeks-etyki-radcy-prawnego-i-regulamin-wykonywania-zawodu.pdf`).
- **NRA:** Zbiór Zasad Etyki Adwokackiej i Godności Zawodu, t.j. z 22.09.2022 —
  `https://nra.pl/szukaj-dokumenty/dokument/1021/` (uchwała 66/2022: `…/dokument/1017/`; plan prac
  nad zmianą zasad reklamy: `…/dokument/1083/`).
- **Ustawy powszechne przez ELI API Sejmu** (`https://api.sejm.gov.pl/eli/acts/DU/{rok}/{poz}/text.pdf`):
  prawo prasowe (Dz.U. 2018 poz. 1914), u.z.n.k. (Dz.U. 2026 poz. 85), u.p.n.p.r. (Dz.U. 2023
  poz. 845).

Najważniejsze rozstrzygnięcia (pełne tabele werdyktów w notatkach): reklama doradców podatkowych
jest **dozwolona warunkowo od 7.08.2010** — materiały zakładające zakaz są nieaktualne; u adwokatów
**zakaz reklamy obowiązuje nadal** (§ 23 Zbioru — „nowelizacja dopuszczająca reklamę" nie istnieje,
dopuszczone jest wyłącznie „informowanie" z zamkniętym katalogiem kanałów i treści); radcowie prawni
po 1.01.2023 są najliberalniejsi. Dla treści opisującej kancelarię jako całość obowiązuje reżim
najsurowszy — w praktyce adwokacki: opinie klientów, publiczny cennik i publikacje płatne
**zakazane**, gdy treść dotyczy adwokatów. Kwestie nadal sporne (m.in. relacja zgody klienta do
tajemnicy ustawowej, rankingi, Google Ads u adwokatów) są oznaczone `[do weryfikacji]` w notatkach —
i tylko te kierować do samorządu/compliance. Praktyczny wniosek z pierwszego przebiegu — frazy
transakcyjne i case studies łatwiejsze po stronie doradztwa podatkowego i księgowości niż
adwokackiej/radcowskiej — **potwierdzony u źródła**.

Streszczenia wtórne [C] zebrane w pierwszym przebiegu — zdegradowane do kontekstu rynkowego,
**nie cytować jako podstawy normatywnej**:

- `https://www.prawo.pl/prawnicy-sady/radca-prawny-a-reklama-zmiany-w-kodeksie-etyki` — zmiany w KERP
  dotyczące informowania o wykonywaniu zawodu.
- `https://www.ibif.pl/blog/strategie-marketingowe/koniec-zakazu-reklamy-adwokackiej-na-czym-polegaja-nowe-zasady`
  — uchwała NRA z 2023-05-26 znosząca zakaz reklamy adwokackiej.
- `https://radcaprawny.kirp.pl/aktualnosci/zakaz-reklamy-w-kodeksach-etycznych-radcow-prawnych-i-lekarzy/`.
- `https://kidp.pl/aktualnosciall.php/10/5940` — stanowisko KIDP (doradcy podatkowi nie podlegają tym
  ograniczeniom w takim zakresie).
- `https://www.prawo.pl/prawo/dyrektywa-omnibus-a-falszywe-opinie-w-sieci` oraz
  `https://poradnikprzedsiebiorcy.pl/-dyrektywa-omnibus-a-publikacja-opinii-o-produktach-i-uslugach`
  — obowiązki weryfikacji opinii wynikające z dyrektywy Omnibus.

### 7.2 Podatki: kalendarz i terminy — rozstrzygnięte u źródeł pierwotnych [A]

**Status: bloker nr 1 z sekcji 12 domknięty 2026-08-28** notatką `_notes/uzup-terminy-podatkowe.md`:
wszystkie terminy potwierdzone w tekstach jednolitych z Dz.U. pobieranych przez **ELI API Sejmu**
(`https://api.sejm.gov.pl/eli/acts/DU/{rok}/{poz}/text.pdf`) — PIT Dz.U. 2026 poz. 592, CIT Dz.U.
2026 poz. 554, VAT Dz.U. 2025 poz. 775, ryczałt Dz.U. 2025 poz. 843, Ordynacja podatkowa Dz.U. 2026
poz. 622, ustawa o rachunkowości Dz.U. 2026 poz. 522, świadczenia zdrowotne Dz.U. 2025 poz. 1461,
nowelizacja JPK ksiąg Dz.U. 2026 poz. 779, rozporządzenie o informacjach podatkowych Dz.U. 2024
poz. 1452 — z wykładnią MF/ZUS na działających podstronach:

- [A] `podatki.gov.pl`: `/ceny-transferowe`, `/podatki-osobiste/pit/informacje-podstawowe`,
  `/podatki-firmowe/cit/cit-klasyczny/...` i `/cit-estonski/...`, `/e-sprawozdania-finansowe`,
  informator TPR (`/media/bcbjtt3u/informator-tpr-2024.pdf` — nazwa pliku myli, to wydanie szóste,
  X 2025), KSeF: `http://ksef.podatki.gov.pl/etapy-wdrozenia-ksef/`.
- [A] `biznes.gov.pl`: artykuły 00236, 00237, 00245, 00252, 00263, 00264, 00274, 00277, 00287
  (uwaga: art. 00236 zawiera błędny przykład przesunięcia terminu na 1 maja, a art. 00277/00287
  pomijają sobotę w regule przesunięcia — rozbieżności opisane w notatce).
- [A] `zus.pl`: terminy rozliczania i opłacania składek
  (`https://www.zus.pl/baza-wiedzy/skladki-wskazniki-odsetki/skladki/terminy-rozliczania-i-oplacania-skladek-na-ubezpieczenia-spoleczne`).

Portale komercyjne (Infor, pit.pl, comarchbetterfly.pl) **nie były potrzebne ani razu** — wcześniej
zebrane linki [C] zastąpione, nie cytować. Ten sam mechanizm ELI służy do cyklicznej weryfikacji
terminów przed publikacją każdego tekstu.

Pozostałe `[do weryfikacji]` w notatce (nie blokują skilla, blokują pojedyncze treści): pierwszy
rocznik siedmiomiesięcznego terminu JPK ksiąg (brak przepisu przejściowego w Dz.U. 2026 poz. 779),
dzienna data ORD-U w wykładni organu (30 listopada wynika z wyliczenia z przepisów, nie z komunikatu
MF), rzeczywista sezonowość zapytań w Google.pl.

### 7.3 Rynek i marka [B/C]

- [A] `https://gs.statcounter.com/search-engine-market-share/all/poland` — udział wyszukiwarek w PL.
- [B] `https://searchengineland.com/branded-search-seo-452676` — patrz 3.4.
- [C] `https://prawnymarketing.pl/pozycjonowanie-kancelarii/`,
  `https://weblymate.com/seo-lokalne-dla-prawnikow-jak-kancelaria-zdobywa-klientow-z-google-i-google-maps/`,
  `https://lokalnytop.pl/blog/seo-lokalne-dla-prawnikow-przewodnik-jak-zyskac-klientow-z-wyszukiwarki`,
  `https://bigbrains.pl/porady-marketingowe/seo-lokalne-biura-rachunkowego-w-google-maps/` — kontekst
  konkurencyjności fraz kancelaryjnych. **Tylko nazewnictwo i kierunek, żadnych liczb.**
- [C] `https://www.airops.com/blog/what-is-branded-search`, `https://thatware.co/brand-entity-seo/`,
  `https://webmetric.com/wiedza/sezonowosc-slow-kluczowych-jak-ja-wykorzystac-w-dzialaniach-seo/`,
  `https://semcore.pl/keyword-research/`,
  `https://cluegroup.pl/narzedzia/porownanie-narzedzi-senuto-vs-semstorm-vs-surferseo-polskie-starcie-gigantow/`,
  `https://www.aivisible.pl/blog/jak-sledzic-widocznosc-w-google-ai-overviews`.
- [C] o systemach wymiany linków (SWL) w PL: `https://delante.pl/swl-a-seo/`,
  `https://seo-www.pl/blog/system-wymiany-linkow-swl-co-to-jest-jak-dziala-i-dlaczego-lepiej-go-unikac/`.
- [C] katalogi lokalne PL: `https://wenet.pl/blog/reklama-w-internecie-ranking-top-25-miejsc-do-lokalnej-promocji-firmy/`,
  `https://www.ranktracker.com/pl/blog/a-complete-guide-for-doing-local-seo-in-poland/`.

### 7.4 Marka Mentzen w mediach (materiał do analizy SERP-u brandowego)

Te źródła nie uczą SEO — **są dowodem na to, co zobaczy użytkownik szukający marki**, i dlatego wchodzą
do skilla jako baseline SERP-u brandowego:
`https://rejestr.io/krs/1071605/kancelaria-mentzen` · `https://www.money.pl/gielda/spolki-gpw/plmntzn00014` ·
`https://www.trojmiasto.pl/Kancelaria-Mentzen-o94675.html` ·
`https://en.wikipedia.org/wiki/S%C5%82awomir_Mentzen` ·
`https://mycompanypolska.pl/artykul/slawomir-mentzen-rozkreca-biznes-jego-kancelaria-zarobila-ponad-21-mln-zl/14025` ·
`https://rynekprawniczy.pl/2024/04/08/kancelaria-podatkowo-prawna-konfederackiego-polityka-wchodzi-na-gielde/` ·
`https://www.rp.pl/biznes/art41749801-o-slabych-wynikach-i-kampanii-wyborczej-czyli-list-slawomira-mentzena-do-akcjonariuszy` ·
`https://www.bankier.pl/wiadomosc/PO-sklada-pozew-wobec-Slawomira-Mentzena-W-tle-dezinformacja-8942622.html` ·
`https://www.money.pl/firma/mentzen-sam-sobie-zle-doradzil-w-kwestii-podatkow-wlasnie-przegral-w-sadzie-7318009512069248a.html`.

---

## 8. Konkurencja — serwisy analizowane bezpośrednio

Osiem analiz zrobionych **na żywo, na surowym HTML i sitemapach** (curl + parsowanie JSON-LD,
nagłówków, linków); trzy domeny doradczo-audytorskie dodane 2026-08-28 (synteza: `konkurencja.md`,
sekcja „Konkurencja doradczo-audytorska"). Wspólne ograniczenie: **żadna nie miała dostępu do danych
o ruchu** — wnioski opisują strukturę, nie widoczność.

| Serwis | Rola w benchmarku | Wejścia analizy |
|---|---|---|
| **crido.pl** | Doradztwo podatkowe premium; benchmark **architektury**: ~5 400 URL-i w osobnych CPT per obszar praktyki (`post_taxes` 3 188, `post_business` 737, `post_law` 459, `threads`, `case_study`, `report`, `hottopic`, `grant`, `training`), krótka strona pillar `crido.pl/ulga-b-r/` na szczycie klastra. **Ich E-E-A-T i dane strukturalne są słabe — to luka dla Mentzena** | `crido.pl`, `crido.pl/ulga-b-r/`, sitemapy, JSON-LD |
| **ifirma.pl** | SaaS księgowy z GPW; benchmark treści podatkowej bez doradców na pierwszym planie | `https://www.ifirma.pl/blog/`, `/blog/cit-estonski-w-spolce-z-o-o-w-2026-.../`, `/blog/aktualnosci/estonski-cit-.../` (301), `/blog/podatek-cit-.../`, `/blog/spolki/`, `/ksiegowosc-dla-spolek/`, `/jednoosobowa-dzialalnosc-gospodarcza-kompendium/`, `/author/dorota-lesak/`, `/rzetelne-biuro-rachunkowe-w-warszawie/`, `/sitemap_index.xml` (+ post/page/author), `/robots.txt` |
| **infakt.pl** | SaaS + biuro rachunkowe (grupa Visma); benchmark **stron księgowych jako encji lokalnych** | `/blog/jak-zalozyc-spolke-z-o-o-krok-po-kroku/`, `/blog/spolka-z-o-o-kompendium-krok-po-kroku/`, `/blog/ksiegowosc-spolki-z-o-o-kompendium/`, `/blog/kategoria/spolki/`, `/blog/author/maciej-sztykiel/`, `/zalozenie-spolki-z-o-o-kompendium-wiedzy`, `/wygodne-zakladanie-spolki/`, `/ksiegowosc-dla-spolek/`, `/ksiegowi/` (+ `/warszawa`, profil księgowej), `/kalkulatory/`, `/robots.txt`, `/ksiegowi/sitemap.xml` |
| **poradnikprzedsiebiorcy.pl** | Content marketing wFirma.pl (siostrzany `poradnikpracownika.pl`); benchmark **portalu-lejka** i kalkulatorów | `/-wyliczenie-skladki-zdrowotnej-u-ryczaltowca`, `/-nowy-polski-lad-skladka-zdrowotna-...`, `/-formy-opodatkowania`, `/kalkulator-skladki-zdrowotnej`, `/kalkulatory`, `/podatki`, `/autor/dorociak-katarzyna`, `/dolacz-do-ekspertow`, `/robots.txt`, `/sitemap.xml` |
| **infor.pl** (+ `ksiegowosc.infor.pl`, z odniesieniem do gofin.pl i pit.pl) | Wydawnictwo; benchmark **sieci subdomen spiętej jednym JSON-LD** (`https://www.infor.pl/#website`, `#organization` jako `NewsMediaOrganization`) i dwuosiowej taksonomii | `ksiegowosc.infor.pl/wiadomosci/7524215,...`, `/podatki/7496316,...`, `/podatki/kpir/abc-kpir/7514477,...`, `/tematy/podatki-2026/`, `/tematy/fundacja-rodzinna/`, `/podatki/`, `/ksef/`, `/vademecum/`, `https://www.infor.pl/eksperci/38,Slawomir-Bilinski.html`, `https://www.gofin.pl/firma/17,2,305,210541,...`, `https://www.pit.pl/aktualnosci/jak-krok-po-kroku-zalozyc-spolke-z-o-o-przez-internet-1008854` |
| **grantthornton.pl** | Audyt + doradztwo B2B, najbliżej profilem; benchmark **spięcia taksonomii treści z taksonomią usług** (~5 100 URL-i: 3 330 publikacji PL, 246 stron usług, 314 stron pracowników) i raportów cyklicznych z danych własnych („Purpurowy Informator", „Barometr prawa"). Warstwa techniczna zaniedbana (puste meta description, `author` bez `@id`) — luka dla Mentzena. **Gotcha W3TC opisany w sekcji 13** | `grantthornton.pl`, `robots.txt`, `sitemap_index.xml` + 14 sitemap, `/publikacja/...` (artykuł ekspercki + raport cykliczny), `/usluga/doradztwo-podatkowe/`, `/pracownik/elzbieta-cybulska/`, `/autorzy/krzysztof-jeromin/`, `/artykuly/`, `/articles_categories/uslugi/raporty/` |
| **rsmpoland.pl** | Sieć audytorska (Drupal, nie WordPress); benchmark **E-E-A-T autorów**: 179 profili ekspertów z numerami uprawnień, jedyny z ósemki z poprawnym `author.@id` w JSON-LD; higiena fatalna (~33% zombie-URL-i, ~394 puste `<title>`, sitemapa w 100% na 301). Notatka dwukrotnie zweryfikowana adwersaryjnie | `rsmpoland.pl`, `robots.txt`, `sitemap.xml` (1 557 URL-i), `/pl/blog/...` (hub + 2 artykuły), `/pl/uslugi/doradztwo-podatkowe`, `/pl/zespol/piotr-liss`, `/pl/raporty`, `/pl/multimedia/podcast`, 26 URL-i legacy przez `curl -I`, Spotify („Podatkowy GPS"), `rsm.global/poland/{en,de}` |
| **roedl.pl** | Niemiecka sieć audyt+podatki+prawo (konkurencja częściowa — profil: inwestor zagraniczny); benchmark **klastrów tematycznych i publikacji cyklicznych** (broszury roczne, newsletter, 5 formatów wokół klastrów), wzorcowe bio ekspertów. Antywzorzec techniczny: **zero JSON-LD na całej domenie**, URL-e ze spacjami i diakrytykami | `roedl.pl`, `robots.txt`, `sitemap.xml`, `/pl/warto-wiedziec/...` (hub + 3 artykuły), `/pl/uslugi/doradztwo-podatkowe/` (+ ceny transferowe), profil eksperta (`/pl/o-nas/kim-jestesmy/profil?PersonID=88`), `/pl/wyszukiwarka-ekspertow/`, `/pl/media/nasze-publikacje/newsletter/` |

---

## 9. mentzen.pl — inwentarz zbadanych zasobów (stan 2026-08-28)

Nie są to „źródła" w sensie bibliograficznym, tylko **stan wyjściowy, do którego skill będzie się
odnosił**. Zebrane w pięciu notatkach (`mentzen-struktura`, `mentzen-oferta`, `mentzen-blog`,
`mentzen-brand`, `mentzen-youtube`).

**Infrastruktura:** `https://mentzen.pl/robots.txt` (24 linie, **zero realnych dyrektyw** — same
komentarze Cloudflare Content Signals, brak `Sitemap:`) · `https://mentzen.pl/sitemap_index.xml`
(= `/sitemap.xml`, Yoast, 5 pozycji: `post-sitemap.xml` 522 URL-e, `page-sitemap.xml`,
`praktyka-sitemap.xml`, `interpretacje-sitemap.xml`, `et_code_snippet_type-sitemap.xml`) ·
`https://mentzen.pl/wp-json/` (REST otwarty bez autoryzacji; namespace'y m.in. `yoast/v1`,
`redirection/v1`, `mentzen-blog/v2`, `mentzen/v1`, `wordfence-login-security/v1`) ·
IndexNow `https://mentzen.pl/{klucz}.txt` (istnienia nie da się stwierdzić bez klucza).
Stack: WordPress + Divi + Yoast SEO 28.3 + Cloudflare + Calendesk.

**Oferta — subskrypcje:** `/mentzen-plus/` · `/mentzen-it/` · `/mentzen-prime/` · `/kadry-mentzena/` ·
`/ewidencja-ip-box/` · `/panel-klienta-logowanie/`.
**Oferta — usługi:** `/start-z-mentzenem-zakladanie-firmy/` · `/doradztwo-podatkowe/` ·
`/doradztwo-prawne/` · `/konsultacje-online/` · `/cit-estonski/` · `/fundacja-rodzinna/` ·
`/ceny-transferowe/` · `/optymalizacja-podatkowa/` · `/kontrola-i-postepowanie-podatkowe/` ·
`/restrukturyzacja-dzialalnosci/` · `/sukcesja/` · `/sprawy-pracownicze/` ·
`/postepowania-sadowe-i-windykacja/` · `/umowy-szyte-na-miare/` · `/znaki-towarowe/` ·
`/kryptowaluty/` · `/vat/` · `/wsparcie-zus/` · `/nieruchomosci` (regulamin) · `/dane-kontaktowe/` ·
`/nasz-zespol/` · `/regulaminy/`.
**Blog:** `https://mentzen.pl/blog/` — 523 wpisy, 39 kategorii, 1547 tagów (dane z REST API).
Kategorie główne: `/blog/category/doradztwo-podatkowe/` (197), `/doradztwo-prawne/` (84),
`/doradztwo-podatkowe/ulgi-podatkowe/` (48), `/ksiegowosc/` (32, **0 wpisów z 2025–2026**),
`/inne/` (25), `/doradztwo-podatkowe/vat/` (24, 0 z 2025–2026),
`/doradztwo-podatkowe/optymalizacja-podatkowa/` (18), `/doradztwo-podatkowe/cit-estonski/` (11),
`/doradztwo-prawne/spolki/`, `/doradztwo-prawne/fundacja-rodzinna/`, `/inne/kadry/`,
`/inne/kryptowaluty/`. Archiwa autorów: m.in. `/blog/author/kacper-boron/`, `/blog/author/szymon-mackiewicz/`.
**Subdomeny:** `https://corporate.mentzen.pl/` · `https://szkolenia.mentzen.pl/` ·
`https://doradztwo.mentzen.pl/` · `https://appworks.pl/`.
**Kanały YouTube** (audyt `_notes/mentzen-youtube.md`; dane z bezpośredniego pobrania
`ytInitialData`/`ytInitialPlayerResponse` przez curl + RSS + Google Autocomplete, 2026-08-28 —
limit WebSearch był wtedy wyczerpany): firmowy `@kancelariamentzen` (ID `UCe-iBx3MWE3B4wAAdC3BDQw`,
491 filmów, 16,8 tys. subskrybentów, 6,2 mln wyświetleń, ostatnia publikacja 2026-06-13, odcięty od
domeny — brak `sameAs`, zero embedów w sprawdzonych artykułach) i osobisty `@SlawomirMentzen`
(1,09 mln subskrybentów, 389,7 mln wyświetleń, **zero linków do mentzen.pl**). Pozycje
w wyszukiwarce YouTube traktować jako migawki — dwa odczyty tego samego dnia dały różne wyniki.
Otwarte `[do weryfikacji]` z audytu: wolumeny fraz (DataForSEO/Senuto), udział Shorts
w wyświetleniach kanału, obecność filmów w wynikach wideo Google, ruch YouTube→mentzen.pl w GA4.
**Uwaga na duplikat hosta:** część URL-i występuje w notatkach zarówno jako `mentzen.pl`, jak i
`www.mentzen.pl` (np. `/ulga-br/`, `/ulga-ip-box-dla-programistow/`, `/ceny-transferowe/`,
`/roczne-zeznanie-podatkowe-przedsiebiorcy/`, `/podatek-liniowy-czy-skala-podatkowa-co-wybrac/`).
**Do sprawdzenia jako pierwsze zadanie techniczne skilla.**

---

## 10. Dokumenty i pliki lokalne (warstwa „jak napisać skill")

- [A] `https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices` — Skill
  authoring best practices.
- [A] `https://code.claude.com/docs/en/skills` — Extend Claude with skills (Claude Code).
- [A] `https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills`.
- [A] `https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents`.
- Lokalnie: `/home/wkuczkowski/.claude/skills/writing-for-agents/SKILL.md` + `SKILL-MECHANICS.md`.
- Lokalnie (wzorzec struktury do naśladowania): `/home/wkuczkowski/projects/skills/skills/conversion-ux/`
  (`SKILL.md` + `references/`).

---

## 11. Źródła odrzucone — lista zakazu

Te materiały **pojawiły się w wyszukiwaniach i nie weszły do notatek**. Zapisuję je, żeby przyszły
agent nie „odkrył" ich ponownie i nie wciągnął do skilla.

| Co odrzucono | Dlaczego |
|---|---|
| Blogi agencyjne o „topical authority" z liczbami typu „+30% ruchu", „2,5× dłuższe utrzymanie pozycji", „15–25 clusterów na pillar" | Brak metodologii, brak źródła pierwotnego. Odrzucone w całości w notatce content-onpage |
| Polskie liczby o local pack: „brak w TOP 3 = utrata ~44% kliknięć", „GBP to 32% sygnałów rankingowych", „7-krotnie większa szansa na Local Pack" | Blogi agencyjne bez metodologii. Jeśli liczba jest potrzebna — szukać oryginału u Whitespark lub BrightLocal |
| Twierdzenie, że Google dodało sekcję „Authors" do Search Central 2026-02-01 i że autorstwo stało się bezpośrednim czynnikiem rankingowym przed March 2026 core update | **Prawdopodobnie nieprawdziwe.** Sprawdzenie changelogu `developers.google.com/search/updates` nie wykazało takiego wpisu. Nie opierać skilla na tym |
| „December 2025 core update: 67% serwisów YMYL odnotowało zmiany widoczności" | Jedno niskozaufane źródło, brak danych pierwotnych |
| Liczby o TTFB WordPressa („z 800 ms+ do <200 ms" po zmianie hostingu) | Blogi branżowe, nie źródła wysokozaufane. Kierunek prawdziwy, liczba niecytowalna |
| llms.txt jako argument sprzedażowy wtyczek SEO | Google nie używa pliku; 97% plików nigdy nieodczytanych (Ahrefs 2026-06-15) |

---

## 12. Twierdzenia `[do weryfikacji]` — mapa na źródła

Uporządkowane wg tego, jak mocno blokują decyzję.

**Blokują wdrożenie — wszystkie domknięte 2026-08-28:**

1. ~~**Terminy podatkowe w kalendarzu treści.**~~ **Rozstrzygnięte 2026-08-28:** wszystkie terminy
   potwierdzone w tekstach jednolitych z Dz.U. przez ELI API Sejmu, z wykładnią MF/ZUS na
   działających podstronach (sekcja 7.2; pełne podstawy prawne, tabela zbiorcza i kalendarz
   contentowy: `_notes/uzup-terminy-podatkowe.md`). Po drodze sprostowania: „20 lutego" nie jest
   terminem ustawowym (ustawowo: 20. dzień miesiąca po miesiącu pierwszego przychodu), a JPK ksiąg
   rachunkowych ma od 1.07.2026 nowy termin 31 lipca (Dz.U. 2026 poz. 779). W notatce zostają trzy
   `[do weryfikacji]`: pierwszy rocznik nowego terminu JPK ksiąg, dzienna data ORD-U w wykładni
   organu, sezonowość zapytań. Zasada „publikacja błędnego terminu to większe ryzyko niż utrata
   pozycji" pozostaje w mocy — stąd weryfikacja przez ELI przed każdą publikacją.
2. ~~**Etyka adwokacka i radcowska a zbieranie oraz publikowanie opinii klientów, case studies
   i cenników.**~~ **Rozstrzygnięte 2026-08-28** w źródłach pierwotnych KIDP/KIRP/NRA (sekcja 7.1;
   `_notes/uzup-etyka-doradcy.md`, `_notes/uzup-etyka-prawnicy.md`): u adwokatów opinie klientów,
   publiczny cennik i publikacje płatne zakazane; u doradców podatkowych dozwolone warunkowo;
   radcowie pośrodku; dla treści mieszanej reżim najsurowszy. Werdykty weszły jako bright lines do
   `eeat-ymyl.md` §7a i `link-building.md` §4a. Do compliance/samorządu kierować już tylko kwestie
   oznaczone w notatkach `[do weryfikacji]` (m.in. relacja zgody klienta do tajemnicy ustawowej,
   rankingi, Google Ads u adwokatów).
3. ~~Czy Google dokumentuje wsparcie rich resultów dla `LegalService` / `AccountingService` /
   `Attorney`.~~ **Rozstrzygnięte 2026-08-28:** nie. Dokumentacja LocalBusiness wymienia jako przykłady
   podtypów tylko Restaurant, DaySpa, HealthClub, Electrician, Plumber, Locksmith i Pharmacy, a `Service`
   nie występuje w Search Gallery (2026-06-15) — te typy są poprawne dla schema.org, ale bez gwarancji
   rich resultu. Szczegóły w `technical-seo.md` §4.3 i §11; skill nie obiecuje z nich efektu w SERP.
4. ~~Polityki Google o self-serving reviews i `AggregateRating`.~~ **Rozstrzygnięte 2026-08-28:**
   klauzula zacytowana wprost z dokumentacji Review snippet w `technical-seo.md` §4.3 (strona z
   `LocalBusiness`/`Organization` jest „ineligible for star review feature" dla opinii o samej sobie);
   konsekwencja wdrożeniowa opisana w `local-seo.md` — opinii klientów na mentzen.pl nie oznaczać
   markupem `aggregateRating`/`review`.

**Blokują pomiar:**

5. Retencja 16 miesięcy i sufit ~50 000 wierszy/dobę w GSC API — **liczb nie ma w oficjalnej
   dokumentacji Google**, tylko w źródłach wtórnych (sekcja 5.1). Zweryfikować empirycznie na
   właściwości mentzen.pl.
6. Aktualne stawki BigQuery dla bulk exportu (`https://cloud.google.com/bigquery/pricing`) —
   nie pobrane.
7. ~~Czy Indexing API rozszerzyło zakres poza `JobPosting`/`BroadcastEvent`.~~ **Rozstrzygnięte
   2026-08-28:** nie. Dokumentacja nadal mówi „The Indexing API can only be used to crawl pages with
   either `JobPosting` or `BroadcastEvent` embedded in a `VideoObject`". Odpada dla bloga.
8. ~~Deklarowana wielkość bazy fraz Senuto: 14 czy 19 mln.~~ **Rozstrzygnięte 2026-08-28:** producent
   deklaruje 80 mln fraz PL (patrz sekcja 5.4).
9. Cennik i kredyty Serper (1 kredyt do 10 wyników, 2 dla 11–100; ~$1 → $0,30 / 1000 zapytań);
   przechodzenie niewykorzystanych zapytań SerpApi na kolejny miesiąc. Źródła: apiserpent [C].
10. Istnienie, zakres i cennik API Brand24 (`https://brand24.com/api/` → 404). Podtrzymane jako
    otwarte w `pomysly-features.md` §14.

**Kalibrują oczekiwania, nie blokują:**

11. Oryginalny URL badania Ahrefs „schema vs. cytowania AI" (1 885 stron) — dostępne tylko omówienia.
12. „Site reputation policy adjustment, 2026-08-28" — **treść zmiany sprawdzona 2026-08-28** w
    changelogu Search Central: wpis dotyczy modyfikacji sposobu egzekwowania polityki **na obszarze
    EOG** (szczegóły w towarzyszącym poście na blogu). **Dotyczy treści sponsorowanych i gościnnych
    na domenie kancelarii** i — jako zmiana dla EOG — bezpośrednio rynku polskiego; przeczytać post
    blogowy przed zaproponowaniem jakiejkolwiek publikacji gościnnej.
13. Czy pole „reviewed on" / „zweryfikował" jest w ogóle sygnałem dla Google (prawdopodobnie nie — to
    zabezpieczenie merytoryczne, nie ranking hack).
14. Zakres obowiązkowych danych identyfikacyjnych podmiotu na stronie WWW w prawie polskim (kwestia
    prawna, nie SEO).
15. Aktualne zachowanie Yoast / Rank Math / SEOPress w zakresie grafu schema — porównanie oparte na
    źródłach o średnim zaufaniu i materiałach producentów.

Nierozstrzygnięte luki tematyczne z notatki eeat-ymyl (budżet wyszukiwań wyczerpał się po 7
zapytaniach): polski SERP dla fraz podatkowych, wpływ AI Overviews na zapytania prawno-podatkowe w PL
(**częściowo pokryty później przez webinar Senuto — sekcja 6**), wytyczne local SEO dla kancelarii
(**pokryte przez notatkę local-seo**).

**Status decyzji użytkownika (2026-08-28):** kolejność budowy narzędzi rozstrzygnięta — najpierw
CLI do GSC, warstwa SERP druga w kolejce (przed nią decyzja: gotowe SERP API vs własny scraper —
jedyna decyzja backlogu z konsekwencjami prawnymi dla kancelarii), badanie cytowań AI dla polskich
fraz podatkowych odłożone do uzyskania dostępu do GSC. Porównanie wariantów SERP i pełny backlog:
`pomysly-features.md` (sekcje 1 i 14).

---

## 13. Gotchas dostępowe

Sześć rzeczy, które w tym researchu przewróciły pobieranie (albo audyt) w powtarzalny sposób. Skill
powinien mieć je wpisane w procedurę, żeby nie odkrywać ich za każdym razem.

| Zasób | Objaw | Obejście |
|---|---|---|
| `crido.pl` | **403 na WebFetch** (blokada nietypowych UA po WAF) | `curl` z przeglądarkowym `User-Agent` + `Accept-Language: pl-PL`. Prawdopodobnie dotyczy innych serwisów za WAF-em |
| `https://www.podatki.gov.pl/` | 301 z HTTPS na HTTP; część starszych sekcji przekierowuje do archiwum `podatki-arch.mf.gov.pl`; kalendarz „Ważne terminy" ładuje dane JS-em i deklaruje rocznik 2025 | Działają konkretne podstrony (`/ceny-transferowe`, `/podatki-osobiste/pit/informacje-podstawowe`, `/podatki-firmowe/cit/...`); terminów i tak nie cytować z widżetu, tylko z Dz.U. przez ELI API `https://api.sejm.gov.pl/eli/acts/DU/{rok}/{poz}/text.pdf` — sekcja 7.2 |
| `https://brand24.com/api/` | 404 | Zapytać dostawcę bezpośrednio |
| `grantthornton.pl` (W3 Total Cache) | Przy pierwszym pobraniu potrafi zwrócić skróconą wersję strony (121 KB zamiast 665 KB) bez boksu autora i CTA — fałszuje audyt E-E-A-T; zjawisko nie odtworzyło się przy powtórce, więc jest nieprzewidywalne | Pobierać dwukrotnie i porównywać rozmiar odpowiedzi przed wyciąganiem wniosków — dotyczy też innych serwisów za agresywnym cache |
| Zasady etyki KIDP | Jedyny nośnik to PDF pod adresem zawierającym numer uchwały — URL zmieni się przy kolejnym tekście jednolitym | Linkować do strony `https://kidp.pl/dla-doradcow/podstawy-prawne/prawo-korporacyjne`, nie do PDF-u |
| QRG (PDF) | Dwa równoległe URL-e | Kanoniczny `https://guidelines.raterhub.com/searchqualityevaluatorguidelines.pdf`; mirror `static.googleusercontent.com/media/guidelines.raterhub.com/en//searchqualityevaluatorguidelines.pdf` (uwaga na podwójny ukośnik w ścieżce). Weryfikacja wersji: metadane PDF (`CreationDate`, `Pages: 182`) + nagłówek „General Guidelines — September 11, 2025" |

---

## Zastosowanie dla mentzen.pl

**Rób — wpisz sekcję 2 jako jedyne dopuszczalne źródło twierdzeń o Google.** Skill ma odsyłać do
dokumentu z kanonu albo mówić „to jest praktyka branżowa, nie stanowisko Google". Trzeciej opcji nie
ma. *Dlaczego:* w tym researchu dwa krążące „fakty" o Google okazały się nieweryfikowalne, a jeden
prawdopodobnie zmyślony (sekcja 11).

**Rób — traktuj daty przy dokumentacji jako datę ważności, nie metadaną.** Dokumenty Google mają w tym
zestawieniu daty od 2023-08 do 2026-08-28. Skill uruchamiany za pół roku musi je pobrać ponownie,
zanim zacytuje liczbę albo cytat. *Dlaczego:* FAQPage w ciągu trzech lat przeszedł drogę od pełnego
rich resultu, przez ograniczenie do serwisów rządowych i zdrowotnych, po całkowite wycofanie
(2026-05-07). Każda rada oparta na wersji sprzed roku byłaby dziś szkodliwa.

**Rób — przy każdej tezie z filmu podawaj autora i jego interes.** „Wg Samo Cerara z kanału Ahrefs",
„wg Szymona Parzycha (Vestigio) na webinarze Senuto". *Dlaczego:* siedem z ośmiu filmów sprzedaje
narzędzie omawiane w materiale, a jeden autor koryguje własną liczbę w obrębie tego samego kursu.

**Rób — użyj webinaru Senuto (`MKwi0qmVSS8`) jako jedynego źródła danych o AI Overviews dla polskiego
SERP-u**, z zastrzeżeniem, że transkrypt to auto-napisy i liczby wymagają sprawdzenia u źródła.
*Dlaczego:* wszystkie pozostałe badania AI search w tej bibliografii są anglojęzyczne i mierzone na
rynku US/UK. Polskiego odpowiednika po prostu nie ma.

**Rób — terminy podatkowe i granice etyczne cytuj wyłącznie z notatek `uzup-*` i ich źródeł
pierwotnych.** Dwa z trzech dawnych blokerów sekcji 12 są domknięte 2026-08-28
(`uzup-terminy-podatkowe.md` — Dz.U. przez ELI API Sejmu; `uzup-etyka-doradcy.md` +
`uzup-etyka-prawnicy.md` — teksty KIDP/KIRP/NRA); wcześniej domknięto wsparcie rich resultów dla
typów prawniczych i polityki o `AggregateRating` (`technical-seo.md` §4.3 i §11). Otwarta pozostaje
retencja i limity GSC API — zweryfikować empirycznie zaraz po uzyskaniu dostępu do właściwości
mentzen.pl. *Dlaczego:* skill zbudowany na niepotwierdzonym terminie podatkowym generuje treść,
która szkodzi kancelarii bardziej, niż pomaga jej widoczność — dlatego przed publikacją terminy
weryfikuje się w Dz.U., nie w portalach.

**Unikaj — dopisywania nowych polskich blogów agencyjnych do warstwy faktograficznej.** Sekcja 11
opisuje, co i dlaczego już raz odrzucono. *Dlaczego:* w polskim SEO liczby krążą między blogami bez
źródła; ta bibliografia ma zapobiec temu, żeby przy kolejnym researchu wróciły tylnymi drzwiami.

**Unikaj — cache'owania tej bibliografii jako listy „sprawdzonych linków".** To zdjęcie stanu na
2026-08-28. Największą wartością jest tu podział na rangi, lista zakazu i mapa niedomkniętych
twierdzeń — nie same adresy.
