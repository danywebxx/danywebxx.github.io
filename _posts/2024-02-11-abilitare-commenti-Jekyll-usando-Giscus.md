---
layout: post
title:  "Abilitare i commenti su Jekyll usando Giscus"
date:   2024-02-11 16:00:00 +0100
categories: [blogging, tutorial]
tags: [appunti, github, jekyll] 
---
Ieri stavo ultimando la configurazione del blog e mi sono accorto di aver dimenticato di attivare i `commenti`.
I commenti in un blog sono importanti perchè ti permettono di comunicare con i lettori, leggere i loro suggerimenti, le critiche ed eventuali correzioni da fare agli articoli.

Tra le opzione rese disponibili da Jekyll ospitato su Github, quella più comoda è risultata [Giscus](https://giscus.app). L'attivazione ha richiesto meno di 30 minuti (da neofita) seguendo i consigli di [Martin](https://blog.martinp7r.com/posts/adding-giscus-comments-to-my-blog/) e [Seletz](https://seletz.github.io/posts/giscus-comments/).
                                                 
`Giscus` si basa sulle discussioni di Github. Queste possono essere abilitate nei repository ospitati offrendo commenti e statistiche sui siti web. 
Fortunatamente `chirpy`, il tema che ho scelto per il blog, ha la perfetta integrazione con Giscus.

## Configurazione

La configuraizone è semplice. Ecco i passaggi:

- Installare ed autorizzare l'app `giscus` nel repository Github del blog
- Configurare i parametri su <https://giscus.app>
  * Entrare nel repository del blog.
  * Abilitare la mappatura Pagina ↔️ Discussione
  * Usare la categoria "Announcements"
  * Abilitare le razioni per il post principale
  * Dallo snippet restituito si estrapolano i dati da inserire nel file `_config.yml` del blog
- Abilitare i commenti `giscus` inserendo i dati nel file `_config.yml` di cui sotto trovare un esempio.

```yaml
comments:
  active: "giscus"
  giscus:
    repo: "danywebxx/danywebxx.github.io" # <gh-username>/<repo>
    repo_id: "ricavato da pagina Giscus"
    category: "Announcements"
    category_id: "ricavato da pagina Giscus"
    mapping: "pathname" # optional, default to 'pathname'
    input_position: "bottom" # optional, default to 'bottom'
    lang: "it" # optional, default to the value of `site.lang`
    reactions_enabled: "1" # optional, default to the value of `1`
```
Se hai trovato inesattezze fammelo sapere lasciando un commento (così mi assicuro che i commenti funzionino correttamente ;-P).
