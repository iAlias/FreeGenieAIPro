# FreeGenie AI Pro

**Contenuti, immagini e SEO generati dall'AI, dentro WordPress.**

FreeGenie AI Pro scrive articoli completi a partire da un prompt, ne genera le immagini e ne
ottimizza i meta tag SEO. Puoi pubblicare a mano dall'editor oppure lasciare che il plugin
pubblichi da solo, secondo un calendario che decidi tu.

---

## Funzionalità

- ✍️ **Articoli completi** da un prompt: titolo d'impatto + testo
- 🖼️ **Generazione immagini** con più provider, con ordine di preferenza configurabile
- 🔁 **Rigenerazione** di un singolo articolo o di una singola immagine
- 🔎 **SEO**: meta description e title integrati con **Yoast SEO** e **Rank Math**
- 🗓️ **Pianificazione**: scegli i giorni della settimana e quanti articoli al giorno; la
  pubblicazione avviene automaticamente via WP-Cron
- 🧩 **Metabox nell'editor** per generare contenuto e immagini direttamente dal post
- 📋 **Log di debug** azzerabile dalle impostazioni

---

## Provider supportati

| Ambito | Provider |
|---|---|
| **Testo** | OpenAI · Cohere · DeepAI · Hugging Face |
| **Immagini** | Pexels · Pixabay · Pollinations · Hugging Face · OpenAI |

L'ordine di preferenza per le immagini è configurabile dal pannello
(predefinito: Pexels → Pixabay → Pollinations → Hugging Face → OpenAI → Cohere → DeepAI).

---

## Installazione

1. Copia la cartella del plugin in `wp-content/plugins/` (o carica lo zip da
   **Plugin → Aggiungi nuovo → Carica plugin**)
2. Attiva **FreeGenie AI Pro**
3. Vai su **FreeGenie AI Pro** nel menu di amministrazione e inserisci le chiavi API

---

## Configurazione

Nel pannello **FreeGenie AI Pro** puoi impostare:

| Impostazione | Descrizione |
|---|---|
| **Chiavi API** | OpenAI, Cohere, DeepAI, Hugging Face, Unsplash, Pexels, Pixabay |
| **Giorni di pubblicazione** | I giorni della settimana in cui pubblicare |
| **Articoli al giorno** | Quanti contenuti generare ogni giorno (0–24) |
| **Ordine provider immagini** | Quale provider usare per primo |

---

## Utilizzo

- **Manuale**: apri un post, usa la metabox per generare articolo e immagine, poi rigenera singole parti se serve.
- **Automatico**: imposta giorni e quantità giornaliera e lascia fare a WP-Cron.

---

## Requisiti

- WordPress
- PHP 7.4 o superiore
- Almeno una chiave API tra i provider che vuoi usare

---

## Licenza

GPL-3.0-or-later. Vedi il file [LICENSE](LICENSE).
