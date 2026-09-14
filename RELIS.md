# relis

Tema Shopify del negozio **relis**, costruito su [Shopify Horizon](https://github.com/Shopify/horizon).

Due pezzi in catalogo: **Fuga 26**, la borsa da due notti (44 × 32 × 18,5 cm, 26 L,
79 €), e **Rituale**, il beauty case che ci sta dentro (27 × 9 × 22 cm, 29 €). Insieme
sono **il set**, 99 € invece di 108 €. Due colori, Marrone Autunno e Azzurro Polvere,
e un solo interno.

---

## Da dove viene

| Cosa | Origine |
|---|---|
| Struttura del tema | Horizon `4.1.5`, commit `8b42ace` (31 agosto 2026) |
| Identita e pagine | Claude Design — `Home.dc.html` e `Fuga-26.dc.html` |

**Attenzione alla fonte.** Nel progetto Claude Design la cartella `_ds/` contiene ancora
il design system della prima versione, «Cuoio e Carta»: serif dritto, angoli a 2px,
azzurro vietato come colore d'interfaccia. Le pagine aggiornate non lo usano piu — lo
caricano ma scrivono ogni stile inline con i valori nuovi. Il tema segue le pagine.
Se il design system in `_ds/` viene aggiornato, e da li che vanno riletti i token.

Il ramo `main` resta Horizon puro. Il lavoro sta su `relis-design`; la pagina d'attesa
sul ramo `attesa`.

```sh
git fetch upstream
git checkout main && git merge upstream/main   # aggiorna la base
git checkout relis-design && git merge main    # porta l'aggiornamento nel tema
```

---

## L'identita in breve

| | |
|---|---|
| Titoli | Bodoni Moda, **sempre corsivo** — dal nome in card alla copertina |
| Corpo | Hanken Grotesk |
| Timbri, misure, prezzi | DM Mono |
| Logotipo | Open Sauce Sans 600, minuscolo |
| Fondo · testo | `#F1EEE9` · `#1C1411` |
| Marrone Autunno | `#4B2E27` — timbri su chiaro, sezione Dentro |
| Azzurro Polvere | `#7D93AE` — barra annunci, sezione Regali |
| Azioni | pill `999px` — bottoni, carrello, quantita, pastiglie |
| Foto | a filo, 4:5 quasi ovunque, 16:10 per il set |

---

## Come e organizzato

Il principio: **quello che si puo dire con un'impostazione si dice con
un'impostazione**, non in CSS. Cosi resta modificabile dall'editor tema, e gli
aggiornamenti da upstream continuano ad applicarsi.

```
config/settings_data.json         palette, pill, pastiglie, tipografia di ripiego
assets/relis.css                  token, bridge su Horizon, classi .rl-*, innesti
snippets/relis-fonts.liquid       le quattro famiglie, ospitate nel tema
templates/index.json              la home
templates/product.fuga-26.json    la pagina Fuga 26
sections/header-group.json        barra annunci azzurra + barra sticky
sections/footer-group.json        striscia + piede su inchiostro
sections/relis-copertina.liquid   la copertina in dissolvenza
sections/relis-striscia.liquid    la striscia a righe sopra il piede
blocks/relis-*.liquid             elenco numerato, tabella misure, logotipo, messaggio
locales/it.json                   una riga: il messaggio del carrello vuoto
```

### `assets/relis.css`

1. **Token** — l'identita. Fonte di verita.
2. **Bridge** — gli stessi token sui nomi di variabile di Horizon. E qui che testata,
   card, bottoni, campi, cassetto e checkout ereditano l'identita **senza che il loro
   markup venga toccato**. I sette preset tipografici dell'editor sono ridichiarati:
   h1-h4 sono il corsivo a quattro misure (pagina, sezione, sottotitolo, card), h5 il
   mono delle misure, h6 il timbro.
3. **Classi** — `.rl-*` per i blocchi relis.
4. **Innesti** — le poche cose che nessun campo sa esprimere, ciascuna col suo perche.

### Perche la copertina e una sezione custom

La slideshow di Horizon scorre di lato e ha solo velature verticali. Il design vuole
due scatti che si sovrappongono in dissolvenza (900ms), ognuno con una velatura
**orizzontale** che copre la meta sinistra, in tono opposto allo scatto. Forzare la
slideshow con il CSS sarebbe stato fragile a ogni aggiornamento.

Il cambio automatico si ferma sotto il puntatore e con il fuoco da tastiera, e non parte
per chi ha chiesto meno movimento — un contenuto che si muove da solo deve potersi
fermare.

---

## Cosa serve dall'admin

### 1. I prodotti

- **Fuga 26** con opzione *Colore*: Marrone Autunno, Azzurro Polvere. Assegna il
  template **product.fuga-26** in *Prodotto → Modello tema*.
- **Rituale**, stessa opzione colore.
- **Il set**, 99 €. Il design lo vende come coppia a colori incrociati (Fuga 26 marrone
  con Rituale azzurro, e viceversa): la cosa piu semplice e un prodotto con due varianti.

Per le pastiglie colore: in *Impostazioni → Metaoggetti* collega ai valori di colore il
marrone `#4B2E27` e l'azzurro `#7D93AE`.

### 2. Due metafield di prodotto

In *Impostazioni → Dati personalizzati → Prodotti*:

| Metafield | Tipo | Esempio |
|---|---|---|
| `custom.misure` | Testo a riga singola | `44 × 32 × 18,5 cm · 26 L` |
| `custom.sottotitolo` | Testo a riga singola | `La borsa da due notti.` |

Compaiono nelle card della home e sotto il titolo in pagina prodotto. Finche mancano, la
riga semplicemente non c'e — il template non si rompe.

### 3. Le fotografie

Sono contenuto, non tema: si caricano dall'editor. Dal progetto Claude Design,
cartella `assets/foto/`:

| Dove | Scatto |
|---|---|
| Copertina 01 | `copertina-ordinata.png` — inquadratura 78% |
| Copertina 02 | `copertina-gate.png` — inquadratura 72% |
| La collezione | una foto per prodotto: `studio-azzurro`, `dallalto-marrone`, `collezione-set` |
| Dentro | `dallalto-firma.png` |
| Fuori | `milano-flash.png` |
| Regali, e Rituale nella pagina Fuga 26 | `collezione-set.png` |
| Galleria Fuga 26 | `dallalto-firma`, `studio-marrone`, `studio-azzurro`, `dallalto-marrone` |

### 4. I menu

- `main-menu` — Home, Borse, Rituale, Le misure, Regali
- tre menu per il piede — Negozio, Aiuto, Legali — da collegare alle tre colonne
  dall'editor

### 5. I collegamenti

Bottoni e link puntano a *tutti i prodotti* finche Fuga 26 e il set non esistono. Vanno
ripuntati dall'editor: «Scopri Fuga 26» alla borsa, «Scopri il set» e «Aggiungi il set»
al set.

---

## Scarti noti dal design

Cose che il design prescrive e che Horizon non concede senza riscriverne il markup.

- **Selettore di quantita in pagina prodotto.** Il design non lo ha. Horizon lo rende
  sempre, senza un'impostazione per spegnerlo: e rimasto, portato alla pill.
- **Bottoni di pagamento rapido** (Shop Pay e simili). Non sono nel design; li governa
  l'impostazione pagamenti del negozio, non il tema.
- **«Aggiungi Rituale · set da 99 €»** nel design aggiunge al carrello il set nel colore
  incrociato. Qui e un collegamento alla pagina del set: aggiungere un altro prodotto
  da questa pagina richiede logica che il tema non ha.
- **I nomi dei colori sotto le pastiglie.** Horizon mostra il nome del colore scelto
  sopra le pastiglie, non sotto ciascuna.
- **Il contatore del carrello** e scritto da Horizon; la pill a contorno e un innesto.

---

## Sviluppo

```sh
shopify theme dev --store xfng1r-30.myshopify.com   # chiede la password del negozio
shopify theme check                                  # 0 errori; i warning sono di Horizon
```
