---
title: "Pubblicare un blog su GitHub Pages — guida rapida"
date: 2026-05-04
author: Mauro Ghiani
category: Odds
tags: [GitHub, Jekyll, GitHubPages, StaticSite, Markdown, DevTools]
excerpt: "Vuoi un blog tuo, gratis, senza server e senza WordPress? GitHub Pages più Jekyll è la combinazione più sottovalutata del web. Niente database, niente plugin da aggiornare, niente attacchi a CMS. Solo file Markdown, Git e un dominio del tipo tuonome.github.io."
---

Vuoi un blog tuo, gratis, senza server e senza WordPress?

**GitHub Pages + Jekyll** è la combinazione più sottovalutata del web.
Niente database, niente plugin da aggiornare, niente attacchi a CMS.
Solo file Markdown, Git e un dominio del tipo `tuonome.github.io`.

Questo blog gira così da anni — la stessa pipeline che vedete su [janmaru.github.io](https://janmaru.github.io). Di seguito i passaggi essenziali per replicarla.

---

## 1. Crea un account su GitHub

Vai su [github.com/signup](https://github.com/signup) e registrati.
Lo **username** che scegli diventa parte dell'URL del sito, quindi sceglilo con cura: sarà il tuo dominio per anni.

---

## 2. Installa Git

Su Windows: [git-scm.com/download/win](https://git-scm.com/download/win).

Verifica che sia installato:

```
git --version
```

Configura nome ed email una volta sola:

```
git config --global user.name "Tuo Nome"
git config --global user.email "tua@email.com"
```

---

## 3. (Opzionale) Installa Jekyll in locale

Se vuoi vedere il sito prima di pubblicarlo, ti serve Ruby + Jekyll.

- Ruby con DevKit: [rubyinstaller.org/downloads](https://rubyinstaller.org/downloads/) — scegli la versione **"Ruby+Devkit"**.
- Poi, da un terminale nuovo:

```
gem install jekyll bundler
```

Documentazione ufficiale: [jekyllrb.com/docs/installation/windows](https://jekyllrb.com/docs/installation/windows/).

Non è obbligatorio: GitHub costruisce il sito anche senza Jekyll locale. Però poter fare `bundle exec jekyll serve` e vedere il sito su `localhost:4000` ti risparmia un sacco di commit "fix typo".

---

## 4. Crea il repository su GitHub

Vai su [github.com/new](https://github.com/new):

- **Nome del repository**: `tuousername.github.io` — esattamente così, con il tuo username. Questa è la convenzione magica che fa pubblicare il sito sul dominio principale.
- **Visibilità**: Public.
- **Senza README**, per ora.

---

## 5. Prepara la struttura minima del sito

Un sito Jekyll come questo ha questa forma:

```
tuousername.github.io/
├── _config.yml          # configurazione del sito
├── _layouts/            # template HTML
├── _posts/              # articoli (formato YYYY-MM-DD-titolo.md)
├── index.html
└── assets/              # css, immagini, js
```

Riferimento completo: [jekyllrb.com/docs/structure](https://jekyllrb.com/docs/structure/).

Se vuoi partire ancora più snelli, puoi clonare un tema esistente da [jekyllrb.com/docs/themes](https://jekyllrb.com/docs/themes/) e modificarlo.

---

## 6. Carica i file su GitHub

Dal terminale, dentro la cartella del sito:

```
git init
git add .
git commit -m "primo commit"
git branch -M master
git remote add origin https://github.com/tuousername/tuousername.github.io.git
git push -u origin master
```

A questo punto i file sono su GitHub, ma il sito non è ancora pubblicato.

---

## 7. Attiva GitHub Pages

Sul repository:

- **Settings → Pages**
- **Source**: Deploy from a branch
- **Branch**: `master` (o `main`), cartella `/ (root)` → **Save**.

Guida ufficiale: [docs.github.com/en/pages/quickstart](https://docs.github.com/en/pages/quickstart).

---

## 8. Visita il sito

Dopo 1–2 minuti il sito è online su:

```
https://tuousername.github.io
```

Se non si vede, controlla la tab **Actions** del repository: ogni build di Jekyll è loggata lì, e gli errori (frontmatter rotto, plugin non supportato, ecc.) compaiono in chiaro.

---

## 9. Pubblicare un nuovo articolo

Crea un file in `_posts/` con nome nel formato `YYYY-MM-DD-titolo.md` e un frontmatter come questo:

```markdown
---
title: "Il mio primo articolo"
date: 2026-05-04
author: Tuo Nome
category: Generale
---

Contenuto in Markdown.
```

Poi:

```
git add .
git commit -m "nuovo articolo"
git push
```

GitHub Pages ricostruisce il sito in automatico. Niente FTP, niente CMS, niente upload manuali.

---

## Perché ne vale la pena

- **Zero costi di hosting** — GitHub Pages è gratis per repository pubblici.
- **Versionamento nativo** di ogni articolo: ogni modifica è un commit, ogni revisione è una diff.
- **Niente CMS da mantenere** — non c'è un WordPress da aggiornare, niente plugin da patchare, niente database da ripristinare.
- **I tuoi contenuti restano tuoi**, in Markdown, per sempre. Domani puoi spostarli su qualsiasi altra piattaforma con un `git clone`.

Lo svantaggio: se vuoi commenti, form, ricerca lato server, devi aggiungerli con servizi esterni (Disqus, Formspree, Algolia). È un compromesso, ma per un blog di scrittura tecnica vale eccome.

---

## Link utili

- [pages.github.com](https://pages.github.com)
- [jekyllrb.com](https://jekyllrb.com)
- [Markdown cheatsheet](https://www.markdownguide.org/cheat-sheet/)
- Il codice sorgente di questo blog: [github.com/janmaru/janmaru.github.io](https://github.com/janmaru/janmaru.github.io)
