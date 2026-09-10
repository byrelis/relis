# relis — Cuoio e Carta

Tema Shopify del negozio **relis**, costruito su [Shopify Horizon](https://github.com/Shopify/horizon).

Catalogo di un solo prodotto, la **Relis 42**, una borsa da viaggio in tre
colorazioni. Tema chiaro unico, fondo beige, marrone come colore del marchio,
zero ombre.

---

## Da dove viene

| Cosa | Origine |
|---|---|
| Struttura del tema | Horizon `4.1.5`, commit `8b42ace` (31 agosto 2026) |
| Design system | Claude Design, progetto `relis — Cuoio e Carta` |

Il ramo `main` resta Horizon puro. Il lavoro sta su `relis-design`.

```sh
git fetch upstream
git checkout main && git merge upstream/main   # aggiorna la base
git checkout relis-design && git merge main    # porta l'aggiornamento nel tema
```

---

## Come e organizzato

Il principio: **quello che si puo dire con un'impostazione si dice con
un'impostazione**, non in CSS. Cosi resta modificabile dall'editor tema senza
toccare il codice, e gli aggiornamenti da upstream continuano ad applicarsi.

```
config/settings_data.json      la palette, la forma, i bottoni, la tipografia
assets/relis.css               i token, il bridge su Horizon, le classi .rl-*
snippets/relis-fonts.liquid    le quattro famiglie, ospitate nel tema
templates/index.json           la home, composta con sezioni native
sections/header-group.json     banda inchiostro + barra sticky
sections/footer-group.json     quattro colonne su beige alternato
blocks/relis-*.liquid          i tre blocchi che Horizon non ha
```

### `assets/relis.css`

Quattro parti, in quest'ordine:

1. **Token** — il design system, verbatim. Fonte di verita.
2. **Bridge** — gli stessi token rimappati sui nomi di variabile di Horizon
   (`--font-body--family`, `--shadow-button`, `--style-border-radius-*`…).
   E qui che header, card, bottoni, campi, cassetto carrello e checkout
   ereditano il design **senza che il loro markup venga toccato**.
3. **Classi** — le classi pubbliche `.rl-*` e lo scope `.on-ink`.
4. **Innesti** — le poche cose che nessun campo dell'editor sa esprimere.
   Tenuto corto di proposito; ogni regola ha il suo perche scritto accanto.

Il file e caricato in `layout/theme.liquid` **dopo** `color-palette`: rimappa
variabili che Horizon ha appena dichiarato, quindi deve arrivare per ultimo.

### I blocchi aggiunti

Horizon non aveva un equivalente, e il design system li prescrive:

- `relis-specs` + `_relis-spec-row` — tabella specifiche
- `relis-message` — messaggio di sistema col filetto verticale da 2px
- `relis-wordmark` — il logotipo come testo vivo

---

## Cosa manca per andare online

### 1. Le fotografie

Non sono nel tema perche sono **contenuto, non tema**: vanno caricate
dall'editor nei campi immagine delle sezioni, e sul prodotto dall'admin.

Le sezioni che aspettano uno scatto: apertura (editoriale 21:9), banda
editoriale (21:9), *Cosa ci sta dentro* (dettaglio 1:1), *Concia vegetale*
(modella 4:5).

I quattro tagli del sistema: prodotto **3:4**, modella **4:5**, editoriale
**21:9**, dettaglio **1:1**. Il fondale degli scatti prodotto e avorio caldo
`#F3EEE4`, mai bianco da e-commerce.

### 2. I menu

Da creare in *Negozio online > Menu di navigazione* e collegare:

- `main-menu` — Relis 42, Colorazioni, Chi siamo, Aiuto
- `footer` — collegato tre volte nel piede; per avere tre elenchi diversi
  servono tre menu separati (Negozio, Aiuto, Legali)

### 3. Una decisione sul modello di prodotto

**Serve una scelta, e cambia come si carica il catalogo.** Il design mostra
tre card, una per colorazione, ognuna con il suo nome e il suo scatto. In
Shopify si puo fare in due modi:

- **Tre prodotti** in una collezione *Colorazioni* — corrisponde al disegno
  alla lettera, ogni colorazione ha URL, scatti e giacenza propri.
- **Un prodotto con opzione Colorazione** — piu corretto come inventario,
  ma la home mostrerebbe una card sola.

La sezione *Colorazioni* punta oggi alla collezione `all`. Ho lasciato
questa decisione aperta perche e una scelta di catalogo, non di tema.

### 4. La licenza di Open Sauce Sans

Il font del logotipo e ospitato nel tema, preso da Fontsource. La licenza
web va verificata prima dell'apertura. E il punto 4 delle questioni aperte
nel readme del design system.

### 5. I dati della Relis 42

Misure, capacita, peso, materiale definitivo, prezzo, tempi di spedizione.
Nel tema compaiono come `—`, ed e voluto: **i dati mancanti si dichiarano**,
mai `0,00`, mai un numero verosimile messo li per riempire.

---

## Scarti noti dal design system

Cose che il disegno prescrive e che Horizon non concede senza riscriverne
il markup. Nessuna e bloccante; vanno decise, non subite.

**Le icone.** Il sistema non ne ha, ed e una scelta: dove un'interfaccia
normale mette un glifo, relis scrive la parola — *Cerca*, *Carrello*,
*Chiudi*. Horizon ha icone in testata, carrello e selettori. La ricerca l'ho
spenta (il catalogo e di un articolo), ma carrello e cassetto usano ancora i
glifi di Horizon. Toglierli significa modificare i suoi snippet, e ogni
modifica li e un conflitto in piu ai prossimi aggiornamenti.

**La scala tipografica nei menu.** I preset dell'editor si fermano a sette
gradini (paragraph, h1-h6). La scala relis ne ha dodici. Ho rimappato i
sette sulla scala vera in CSS e assegnato `h6` all'eyebrow, cosi il timbro
e componibile dall'editor. I gradini che restano fuori — `display`, `lead`,
`price`, `button`, `micro` — si raggiungono con i token.

---

## Sviluppo

```sh
shopify theme dev      # anteprima locale collegata al negozio
shopify theme check    # 0 errori; i 6 warning sono di Horizon upstream
shopify theme push     # carica sul negozio
```
