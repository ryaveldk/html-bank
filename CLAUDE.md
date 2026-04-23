# HTML_bank — vidensbank om post-production

Personlig teknisk vidensbank som statiske HTML-sider på GitHub Pages.
Dokumenterer tekniske dybdedyk, beslutninger og workflows fra post-afdelingen
på Metronome / Nordisk Film TV.

- **Live:** <https://ryaveldk.github.io/html-bank>
- **Repo:** <https://github.com/ryaveldk/html-bank>
- **Lokal:** `~/Documents/GitHub/html-bank/`

---

## Om Ryan (ejeren)

- Head of post-production, Metronome / Nordisk Film TV
- **Ikke udvikler** — teknisk orienteret men arbejder GUI-baseret (GitHub Desktop, ikke terminal)
- Foretrækker direkte intellektuel sparring over affirmation
- Vil have ærlig feedback hvis en idé ikke er god, eller han går i den forkerte retning
- Målgruppe for vidensbanken: tekniske kolleger der skal forstå et emne fra bunden — eller ham selv om 6 måneder

---

## Filstruktur og navngivning

```
html-bank/
├── index.html                    ← forside (liste af kapitler)
├── kapitel-01-embeddings.html    ← Visuelle embeddings & VisualScanner
├── kapitel-02-rag.html           ← RAG & token-økonomi
├── kapitel-03-fcp-nfs.html       ← FCP Libraries & NFS
├── kapitel-skabelon.html         ← skabelon til nye kapitler (ikke linket fra forsiden)
├── workflow.html                 ← end-to-end arbejdsgang (user-facing)
└── CLAUDE.md                     ← denne fil
```

**Navngivning:** `kapitel-NN-kortnavn.html` hvor `NN` er nul-padded fortløbende
(04, 05, …) og `kortnavn` er kortfattet emne. Konventionen sikrer alfabetisk
sortering og let filgenkendelse.

---

## Design-system (1:1 — genopfind intet)

"Technical documentation"-stil inspireret af Stripe / Linear / Vercel docs.
Alle nye kapitler skal følge systemet præcist.

### Typografi

- **Geist** (300, 400, 500, 600, 700) — al brødtekst, overskrifter, navigation
- **Geist Mono** (400, 500, 600) — labels, metadata, sektionsnumre, tal i tabeller, kode, tekniske identifikatorer
- **ALDRIG Geist Mono til brødtekst**
- Import-link ligger i `<head>` — kopiér 1:1 fra eksisterende kapitel

### Farver (CSS variables)

```css
--ink:       #0a0a0a;   /* primær tekst, overskrifter */
--ink-2:     #262626;   /* brødtekst */
--ink-3:     #525252;   /* dæmpet tekst, labels */
--ink-4:     #a3a3a3;   /* meta-tekst, dividere */
--line:      #ededed;   /* primære linjer, rammer */
--line-2:    #f5f5f5;   /* sekundære linjer */
--bg:        #ffffff;   /* sidebaggrund */
--tint:      #fafafa;   /* kortbaggrund, subtile fremhævninger */
--signal:    #0ea5e9;   /* ENESTE accent-farve — brug meget sparsomt */
--paper-bg:  #f0f0ee;   /* ydre baggrund, "papir på bord"-look */

/* Callouts — diskret pastel */
--info-bg: #f0f9ff;  --info-border: #bae6fd;  --info-ink: #0c4a6e;
--warn-bg: #fffbeb;  --warn-border: #fde68a;  --warn-ink: #78350f;
--ok-bg:   #f0fdf4;  --ok-border:   #bbf7d0;  --ok-ink:   #14532d;
```

### Layout

- Hvid hovedsheet med let skygge på neutral `#f0f0ee`-baggrund ("papir på bord")
- `topbar` med logo-dot + brand-tekst øverst
- Valgfri sticky TOC-sidebar (260px) til venstre i `.layout`
- Main content `max-width: 720-760px`, centreret
- Sektioner indledes med `.divider`: `01 / SEKTIONSNAVN ─────`

### Godkendt komponent-bibliotek

Brug disse — opfind ikke nye. Fuld markup findes i eksisterende kapitler,
kopiér derfra:

| Komponent | Brug |
|---|---|
| `.topbar`, `.logo-dot`, `.chapter-hero`, `.chapter-badge` | Sidens chrome |
| `.divider` + `.divider-num` + `.divider-label` + `.divider-line` | Sektions-skel |
| `.callout` + `-info` / `-warn` / `-ok` / `-neutral` | Indrammede noter |
| `blockquote` + `<cite>` | Fremhævede citater (erstatter gammel `.insight`) |
| `<details class="deep-dive">` + `<summary>` | Kollapsible dybdedyk |
| `.decision-list` > `.decision-row` + `.decision-key` / `.decision-val` | Beslutnings-matrix |
| `.score-ladder` > `.score-row.strong/ok/weak/noise` | Score-intervaller |
| `.table-wrap` > `<table>` med `.col-label/.col-mono/.col-hit/.col-miss/.col-best` | Tabeller |
| `.glossary-grid` > `.glossary-item` + `.glossary-term` + `.pronounce` + `.glossary-def` | Ordliste |
| `.resource-list` > `.resource` + `.resource-type` + `.resource-title` + `.resource-desc` | Kilde-liste |
| `.diagram` > `<svg>` + `.diagram-caption` | SVG med neutrale gråtoner + sparsom signal-blå |
| `.tldr` (sort boks, hvid tekst) | TL;DR-opsummeringer øverst i kapitel |
| `.quick-ref` (sort boks, hvid mono-tekst) | Kort opslagsværk nederst |
| `.step-list` > `.step` + `.step-num` + `.step-body` | Nummererede trin (workflow) |
| `.token-viz` + `.tok` / `.tok.alt` | Tokenisering-visualisering (kapitel 02) |
| `.prompt-example` + `.prompt-header` + `.prompt-body` | Kodelignende prompt-eksempler (kapitel 02) |
| `.stat-row` > `.stat-block` + `.stat-num` + `.stat-label` | Store tal-blokke (kapitel 03) |

### Forbudt

- **Ikke** Fraunces, Newsreader, JetBrains Mono (gamle fonte — udskiftet)
- **Ikke** orange/brun palette (#c87642, #221e19 osv. — gammelt)
- **Ikke** italic i brødtekst som æstetisk valg — kun hvor det betyder noget semantisk
- **Ikke** emojis i sektionsnumre eller labels
- **Ikke** nye komponent-navne hvis en eksisterende passer

---

## Regler for leverance

1. **Altid færdige filer, aldrig kodefragmenter.** Ryan trækker filen ind i
   Finder, committer, pusher. Han åbner aldrig HTML-filer i en editor for at
   klippe noget ind.

2. **Nyt kapitel = 2 filer leveres samtidig:**
   - `kapitel-NN-kortnavn.html` (den nye)
   - Opdateret `index.html` (med link til det nye kapitel i listen)

3. **Opdatering af eksisterende kapitel** = én færdig fil (erstatter den gamle).

4. **Undtagelse:** bittesmå rettelser ("ret v1.1 til v1.2 i footeren") kan
   Ryan selv lave i TextEdit. Alt andet leveres færdigt.

---

## Workflow (kort version)

Fuld bruger-facing guide ligger i `workflow.html`. For Claude:

1. Ryan beder om ændring eller nyt kapitel
2. Claude leverer færdige fil(er)
3. Ryan trækker ind i lokal mappe, committer + pusher via GitHub Desktop
4. Live på GitHub Pages 30-60 sek senere

### Commit-besked-konvention

| Situation | Format |
|---|---|
| Nyt kapitel | `Add kapitel NN: [emne]` |
| Opdatering | `Update kapitel NN: [hvad blev ændret]` |
| Lille rettelse | `Fix [hvad]` |
| Design-ændring | `Redesign [hvad]: [ny stil]` |
| Omdøb/omstrukturér | `Rename [fra] til [til]` |

Kort, aktivt verbum først, max 50-60 tegn. Engelsk subject-line, dansk
detaljer hvis nødvendigt.

---

## Eksisterende kapitler

| Nr. | Emne | Dækker |
|---|---|---|
| **01** | Visuelle embeddings & VisualScanner | CLIP, embeddings, cosine similarity, MobileCLIP 2, Apple Silicon, hands-on findings fra "Bali på menuen"-materiale |
| **02** | RAG & token-økonomi | Tokens, context windows, RAG-arkitektur, chunking, vector databases, InterviewNavigator som eksempel |
| **03** | FCP Libraries & NFS | SQLite's begrænsninger over NFS, WAL, fillåsning, fsync, korruption, FCPMonitor-målinger fra 100 Knive marts 2026, RAID 6 write penalty |

---

## Når et nyt kapitel skal laves

Claude skal:

1. **Stil afklarende spørgsmål** — emne, målgruppe, vinkel, centrale pointer,
   hvilke tools/navne skal nævnes
2. **Foreslå filnavn** efter konventionen (`kapitel-NN-kortnavn.html`)
3. **Brug `kapitel-skabelon.html`** som udgangspunkt og design-systemet 1:1
4. **Lever begge filer:** nyt kapitel + opdateret `index.html` i samme besked
5. **Ingen italic-brug eller emoji-pynt.** Teksten skal være tæt, konkret,
   command-stil. Forklar tekniske koncepter fra bunden, men uden at tale ned.

Målgruppen kender feltet (post/TV) men er ikke nødvendigvis teknisk dyb.
Skriv så en kollega der har glemt halvdelen på et år stadig kan følge med.
