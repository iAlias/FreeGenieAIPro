# FreeGenie AI Pro

**Contenuti, immagini e SEO generati dall'AI, dentro WordPress.**

[![Piattaforma](https://img.shields.io/badge/piattaforma-WordPress-21759b?logo=wordpress&logoColor=white)](https://wordpress.org/)
[![PHP](https://img.shields.io/badge/PHP-7.4%2B-777bb4?logo=php&logoColor=white)](https://www.php.net/)
[![Versione](https://img.shields.io/badge/versione-1.4.2-orange)](freegenie-ai-pro.php)
[![Licenza](https://img.shields.io/badge/licenza-MIT-green)](LICENSE)

🇬🇧 [Read in English](README.md)

FreeGenie AI Pro scrive articoli completi a partire da un prompt, ne genera
le immagini in evidenza e ne ottimizza i meta tag SEO — direttamente
nell'area di amministrazione di WordPress. Puoi pubblicare a mano
dall'editor oppure lasciare che il plugin pubblichi da solo, secondo un
calendario che decidi tu.

---

## Indice

- [Funzionalità](#funzionalità)
- [Provider supportati](#provider-supportati)
- [Installazione](#installazione)
- [Configurazione](#configurazione)
- [Utilizzo](#utilizzo)
- [Come funziona la pubblicazione automatica](#come-funziona-la-pubblicazione-automatica)
- [Integrazione SEO](#integrazione-seo)
- [Log di debug](#log-di-debug)
- [Requisiti](#requisiti)
- [Struttura del progetto](#struttura-del-progetto)
- [Licenza](#licenza)

---

## Funzionalità

- ✍️ **Articoli completi** da un prompt: titolo d'impatto e testo completo,
  generati in un'unica chiamata dall'editor del post.
- 🖼️ **Generazione immagini** con più provider, con ordine di preferenza
  configurabile.
- 🔁 **Rigenerazione** — rigenera un singolo articolo o solo la sua immagine
  in evidenza senza ripartire da zero.
- 🔎 **SEO** — filtri per meta description e title integrati con **Yoast
  SEO** e **Rank Math**.
- 🗓️ **Pianificazione** — scegli i giorni della settimana e quanti articoli
  al giorno; la pubblicazione avviene automaticamente via WP-Cron.
- 🧩 **Metabox nell'editor** — genera testo e immagini direttamente dalla
  schermata del post, con un prompt immagine facoltativo e separato.
- 🖼️ **Generatore immagini in blocco** — rigenera le immagini in evidenza di
  tutti gli articoli pubblicati in un intervallo di date, dalla schermata
  delle impostazioni.
- 📋 **Log di debug** consultabile e azzerabile dalle impostazioni.

---

## Provider supportati

| Ambito | Provider |
|---|---|
| **Testo** | OpenAI · Cohere · DeepAI · Hugging Face |
| **Immagini** | Pexels · Pixabay · Pollinations · Hugging Face · OpenAI |

L'ordine di preferenza per le immagini è configurabile, e i provider vengono
provati in sequenza finché uno non ha successo (ordine predefinito: Pexels →
Pixabay → Pollinations → Hugging Face → OpenAI → Cohere → DeepAI). Anche
Unsplash è supportato come fonte di immagini quando è configurata una
relativa chiave.

---

## Installazione

1. Copia la cartella del plugin in `wp-content/plugins/` (oppure carica lo
   zip da **Plugin → Aggiungi nuovo → Carica plugin**).
2. Attiva **FreeGenie AI Pro**.
3. Vai su **FreeGenie AI Pro** nel menu di amministrazione e inserisci le
   chiavi API.

---

## Configurazione

Nel pannello **FreeGenie AI Pro** puoi impostare:

| Impostazione | Descrizione |
|---|---|
| **Chiavi API** | OpenAI, Cohere, DeepAI, Hugging Face, Unsplash, Pexels, Pixabay |
| **Giorni di pubblicazione** | I giorni della settimana in cui pubblicare |
| **Articoli al giorno** | Quanti contenuti generare ogni giorno (0–24) |
| **Ordine provider immagini** | Trascina per decidere quale provider provare per primo |

Serve almeno una chiave per un provider di testo e una per un provider di
immagini perché la generazione vada a buon fine; i provider senza chiave
vengono semplicemente saltati a favore del successivo nell'ordine di
priorità.

---

## Utilizzo

- **Manuale** — apri un post, usa la metabox per generare articolo e
  immagine, poi rigenera singole parti se serve.
- **Automatico** — imposta giorni e quantità giornaliera e lascia fare a
  WP-Cron.
- **Immagini in blocco** — dalla schermata delle impostazioni, scegli un
  intervallo di date e rigenera l'immagine in evidenza di ogni articolo
  pubblicato in quel periodo.

---

## Come funziona la pubblicazione automatica

Lo scheduler (`includes/class-scheduler.php`) aggancia un evento WP-Cron
orario (`fgp_hourly`). A ogni esecuzione controlla se oggi è uno dei giorni
configurati e se il conteggio delle pubblicazioni di oggi è ancora sotto il
limite giornaliero impostato; in caso positivo:

1. Sceglie un argomento a caso da un elenco integrato di categorie
   (Tecnologia, Intelligenza Artificiale, Smart Home, Blockchain, Mobile,
   Mobilità, Scienza & Ricerca).
2. Crea un post in bozza e lo assegna alla categoria corrispondente.
3. Genera l'articolo completo tramite `FreeGenie_AI_Pro_Generator`.
4. Pubblica il post e incrementa un contatore giornaliero (transient).

Modificare le impostazioni di pianificazione azzera e riprogramma
automaticamente l'evento.

---

## Integrazione SEO

`includes/class-seo.php` si aggancia ai plugin SEO già attivi sul sito:

- Compila `wpseo_metadesc` (Yoast SEO) e `rank_math/frontend/description`
  (Rank Math) con i primi 155 caratteri del contenuto del post, quando non è
  stata impostata manualmente una meta description.
- Usa il titolo del post come `wpseo_title` di riserva quando non è stato
  impostato manualmente un SEO title.

I valori già impostati nei plugin SEO hanno sempre la priorità: FreeGenie si
limita a riempire i vuoti.

---

## Log di debug

La schermata delle impostazioni include un pannello **Debug Log** che legge
`wp-content/fgp-debug.log` per diagnosticare generazioni fallite, e un
pulsante **Svuota Log** (protetto da nonce) per azzerarlo.

---

## Requisiti

- WordPress
- PHP 7.4 o superiore
- Almeno una chiave API per un provider di testo e una per un provider di
  immagini
- WP-Cron attivo (predefinito in WordPress) per la pubblicazione automatica

---

## Struttura del progetto

```
freegenie-ai-pro.php        Bootstrap del plugin: menu admin, impostazioni, metabox, handler AJAX
includes/
  class-generator.php       Chiama i provider di testo/immagini e assembla l'articolo
  class-scheduler.php       Pubblicazione automatica basata su WP-Cron
  class-seo.php             Integrazione meta con Yoast SEO / Rank Math
admin.js                    Comportamento della schermata admin (riordino drag&drop, generatore in blocco)
admin-styles.css            Stile della schermata admin
```

---

## Licenza

MIT — vedi il file [LICENSE](LICENSE) per i dettagli.
