# danyweb.it

Sito personale di Daniele Longhi, costruito con [Hugo](https://gohugo.io) e il tema [Blowfish](https://blowfish.page) (submodule in `themes/blowfish`, versione v2.106.0).

## Lavorare in locale

```bash
git clone --recurse-submodules https://github.com/danywebxx/danywebxx.github.io.git
cd danywebxx.github.io
hugo server -D        # anteprima su http://localhost:1313, -D mostra anche le bozze
```

Serve Hugo **extended 0.165.0** (Blowfish v2.106.0 supporta da 0.158 a 0.165).

## Dove sono le cose

- `content/_index.md` – testi della home
- `content/servizi.md`, `content/chi-sono.md` – pagine principali
- `content/posts/` – articoli ("Blog"); `draft: true` per le bozze
- `static/assets/<data>/` – immagini degli articoli (stessi URL del vecchio sito)
- `assets/img/avatar.gif` – foto profilo
- `config/_default/` – configurazione (menu, profilo, Google Analytics)
- `assets/css/custom.css` – ritocchi grafici

## Nuovo articolo

```bash
hugo new content posts/titolo-articolo.md
```

Il nome del file diventa l'URL: `/posts/titolo-articolo/`.
Per la copertina: `featureimage: assets/AAAA-MM-GG/immagine.jpg` (file in `static/assets/...`).

## Pubblicazione

Ogni push su `main` avvia `.github/workflows/hugo.yml`, che pubblica su GitHub Pages.

## Note sulla migrazione da Jekyll/Chirpy

- Tutti i 113 URL della vecchia sitemap esistono ancora: 105 identici, 8 reindirizzati
  (`/archives/` e `/page2-4/` verso `/posts/`, tag e categorie unificati).
- `/feed.xml` è mantenuto.
- `static/sw.min.js` disattiva il vecchio service worker (cache offline) di Chirpy nei browser
  dei visitatori abituali: non va cancellato per qualche mese.

## Personalizzazioni del tema

- `layouts/partials/head.html` è una copia di quello di Blowfish v2.106.0 con due modifiche
  (titolo SEO della home da `seoTitle`, niente immagine social doppia). Se aggiorni Blowfish,
  riallinea questo file alla nuova versione mantenendo le due modifiche commentate `danyweb:`.
- `layouts/partials/extend-head-uncached.html` aggiunge in home i dati strutturati schema.org `Person`.
- `assets/img/og-default.png` è l'immagine di anteprima per LinkedIn e i social quando una pagina non ha copertina.
- Negli articoli, `images:` indica l'immagine di anteprima social (di solito uguale a `featureimage`).

## Categorie e tag

Ogni articolo ha **una sola categoria**, tra:
`Microsoft 365 e Azure`, `Sicurezza`, `Reti e infrastruttura`, `Metodo e riflessioni`.
I tag servono per i dettagli (prodotti, tecnologie). Le vecchie categorie e i tag dismessi
sono reindirizzati tramite `aliases` nei file `_index.md` di `content/categories/` e `content/posts/`.
