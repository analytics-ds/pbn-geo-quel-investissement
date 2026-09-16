# Hugo Site Factory

Ce repo est un template pour creer des sites blogs statiques avec Hugo, optimises SEO/GEO, heberges gratuitement sur GitHub Pages.

## Comment ca marche

Ce repo ne contient pas de site. Il contient les **instructions et templates** pour que Claude Code genere un site complet automatiquement.

### Premier lancement

1. L'utilisateur connecte Claude Code a ce repo
2. L'utilisateur tape `/create-site`
3. Claude pose les questions necessaires (nom du site, couleurs, categories, etc.)
4. Claude genere tout le site Hugo, les fichiers SEO, et configure le deploiement
5. L'utilisateur push sur GitHub, active GitHub Pages, le site est en ligne

### Utilisation courante

- `/create-article-geo` : creer un nouvel article de blog (choix parmi plusieurs types : article standard, comparatif). Push automatiquement sur GitHub si le repo est configure
- `/create-article-auto` : **publication automatique** d'un article evergreen SEO (bilingue FR + EN) a partir de la roadmap `roadmap.yaml`. Full auto, aucun input humain. Conçue pour etre declenchee par une routine planifiee (ex: 2x/semaine a 3h du mat via `/schedule`). C'est la **methode 1** (CCR cloud). Voir section "Publications evergreen automatiques" plus bas
- `/create-article-seo` : **production polyvalente locale** d'articles evergreen SEO bilingues FR+EN. Tourne sur le Mac de Damien avec Opus 4.7 (analyse SERP avec fetch concurrents reel, maillage cross-batch). 3 modes au choix : (A) suivre la roadmap.yaml du blog, (B) roadmap externe fournie, (C) KW a la demande dans le chat. 3 strategies de scheduling au choix : garder dates source / cascade depuis date X / prochain slot dispo. Articles ecrits avec `publishDate` futur ou today selon mode. C'est la **methode 2** (batch local + GitHub Actions cron). Voir section "Publications evergreen automatiques" plus bas
- `/seo-setup` : generer ou mettre a jour les fichiers SEO techniques de base (robots.txt, llms.txt, sitemap, structured data)
- `/seo` : mode interactif pour modifier/ajouter des elements SEO (meta tags, JSON-LD, audit on-page, etc.)
- `/serve` : lancer le serveur Hugo en local (previsualisation sur `http://localhost:1313/`)
- `/share` : lancer Hugo + ngrok pour partager le site via un lien public (accessible par n'importe qui)
- `/github-setup` : creer un repo GitHub, push le code et activer GitHub Pages (mise en ligne du site)
- `/github-deploy` : push les modifications vers GitHub et declencher le deploiement

## Structure du repo

```
.claude/
├── scripts/
│   └── fetch-image.sh           ← Image libre de droit : cascade Pexels/Unsplash/Openverse + visuel genere
├── skills/
│   ├── create-site.md           ← Workflow creation de site complet
│   ├── create-article-geo.md        ← Workflow creation d'article (multi-types)
│   ├── create-article-auto/     ← Methode 1 : publication automatique d'article evergreen SEO depuis la roadmap (CCR cloud)
│   ├── create-article-seo/ ← Methode 2 : production polyvalente locale (3 modes + 3 strategies de scheduling)
│   ├── seo-setup.md             ← Workflow fichiers SEO techniques (baseline)
│   ├── seo.md                   ← Mode interactif SEO (modifications ponctuelles)
│   ├── serve.md                 ← Lancer le serveur Hugo en local
│   ├── share.md                 ← Lancer Hugo + ngrok (partage public)
│   ├── github-setup.md          ← Creer un repo GitHub + activer GitHub Pages
│   └── github-deploy.md         ← Push et deployer sur GitHub Pages
└── templates/
    ├── data/
    │   ├── authors.yaml          ← 6 auteurs partages avec bios FR/EN, expertise, topics
    │   └── avatar-prompts.md     ← Prompts de generation des 6 avatars (Midjourney/DALL-E)
    ├── hugo-workflow.yml         ← GitHub Actions CI/CD
    ├── roadmap-template.yaml     ← Squelette commente de la roadmap editoriale evergreen (pour /create-article-auto)
    ├── main.css                  ← Design system CSS complet (variables, composants, responsive, a11y)
    ├── articles/                 ← Templates d'articles par type
    │   ├── article-standard.md   ← Article informatif SEO + GEO (type par defaut)
    │   └── geo-comparatif.md     ← Article comparatif avec mise en avant
    ├── seo/                      ← Fichiers SEO techniques (editables)
    │   ├── robots.txt            ← Modele robots.txt
    │   ├── llms.txt              ← Modele llms.txt
    │   └── structured-data/      ← Schemas JSON-LD
    │       ├── article.json      ← Article (avec image et @id croise)
    │       ├── organization.json ← Organization (avec @id, logo, expertise)
    │       ├── author.json       ← Person (auteur)
    │       ├── breadcrumb.json   ← BreadcrumbList
    │       ├── website.json      ← WebSite (avec publisher @id)
    │       └── faq.json          ← FAQPage (genere auto depuis frontmatter)
    ├── layouts/
    │   ├── baseof.html           ← Layout de base (skip-to-content, fonts non-bloquantes, favicon)
    │   ├── home.html             ← Page d'accueil
    │   ├── list.html             ← Pages de liste
    │   ├── single.html           ← Page article (breadcrumb, TOC sticky, image hero, auteur, articles similaires)
    │   ├── sitemap-html.html     ← Page plan du site (liste toutes les pages)
    │   └── 404.html              ← Page 404 custom
    └── partials/
        ├── header.html           ← Header sticky (backdrop-filter, aria-labels, mobile menu)
        ├── footer.html           ← Footer (aria-label, lien plan du site)
        └── seo-head.html         ← Meta tags SEO complets + JSON-LD auto (OG avec image, Twitter, canonical, hreflang, author, article:section, FAQPage auto, Organization @id)
```

## Contexte du site

> Cette section est remplie automatiquement par le skill `/create-site`.
> Elle permet a Claude de connaitre le contexte du site pour les futures actions.

- **Nom du site** : [non defini]
- **Description (FR)** : [non defini]
- **Description (EN)** : [non defini]
- **URL** : [non defini]
- **Couleurs** : [non defini]
- **Polices** : [non defini]
- **Langue principale** : [non defini] (la version EN en sous-dossier `/en/` est TOUJOURS active)
- **Categories (FR ↔ EN)** : [non defini — format : "Nom FR / Name EN" pour chaque categorie]
- **Auteur principal du site** : [ID-slug de l'auteur dans data/authors.yaml le plus pertinent pour la thematique du site, utilise par defaut si `/create-article-geo` ne trouve pas de match clair]

## Suivi des publications (MEMORY.md)

Le fichier `MEMORY.md` a la racine trace tous les articles publies, classes par semaine. Il est mis a jour automatiquement par `/create-article-geo` et `/create-article-seo`.

**Repere indicatif : 4 articles par semaine.** C'est un rythme cible pour eviter la publication en masse. **Ce n'est pas un blocage.**

Comportement par skill :
- `/create-article-geo` (creation manuelle interactive) : si 4 articles ou plus deja publies cette semaine, **simple warning** affiche a l'utilisateur. L'utilisateur peut continuer en validant. Ne JAMAIS bloquer.
- `/create-article-seo` (batch local methode 2) : meme principe, warning soft en pre-validation, jamais de blocage.
- `/create-article-auto` (routine cron methode 1, mardi/vendredi) : **la regle ne s'applique pas**. La routine publie systematiquement l'entree eligible de la roadmap, sans lire le quota hebdo. Aucun check, aucun warning.

## Garde-fou cannibalisation a l'ajout dans la roadmap

Quand l'utilisateur (ou Claude pour son compte) ajoute un nouveau mot-cle dans `roadmap.yaml` (ou dans une roadmap externe en mode B/C de `/create-article-seo`), executer un check de cannibalisation **soft** avant insertion :

1. Lire toutes les entrees existantes de `roadmap.yaml` (tous statuts confondus : todo, queued, done, failed).
2. Pour chaque entree existante, comparer le `kw` propose au `kw` existant :
   - Tokeniser les deux KW (lowercase, retirer stop words FR/EN courants : le/la/les/de/du/des/un/une/a/au/aux/the/of/and/or/in/on/for...).
   - Calculer l'overlap : nombre de tokens communs / nombre de tokens du KW le plus court.
   - Drapeau "risque" si overlap >= 50% OU si l'un des KW est sous-string complet de l'autre.
3. Si au moins un drapeau "risque" est leve : afficher un warning a l'utilisateur listant les KW suspects et demander confirmation explicite ("Cannibalisation potentielle avec : [liste]. Ajouter quand meme ? oui/non").
4. Si aucun risque : ajouter directement, pas de warning.

Ce garde-fou est purement informatif. L'utilisateur peut toujours forcer l'ajout. L'objectif est juste de l'avertir avant qu'il ne commande deux articles qui se cannibaliseraient en SERP.

## Regles generales

- **Bilinguisme obligatoire (langue principale + EN)** : tous les blogs generes par ce template sont bilingues. La langue principale est servie a la racine (`/`), la version anglaise en sous-dossier `/en/`. Hugo gere le multilingue via la convention de dossiers `content/` (principale) et `content/en/` (anglais). Chaque article et page a une paire de fichiers avec un `translationKey` identique dans le frontmatter. Le header contient un language switcher automatique. Les balises hreflang sont generees automatiquement par le partial `seo-head.html`. **Ne JAMAIS generer un site ou un article dans une seule langue** — c'est systematiquement FR + EN (ou la langue principale + EN)
- Toujours utiliser `relURL` dans les templates Hugo pour les liens (compatibilite GitHub Pages)
- Les articles vont dans `content/blog/` (langue principale) et `content/en/blog/` (anglais)
- Les slugs sont en minuscules, sans accents, mots separes par des tirets
- Ne JAMAIS utiliser `&` dans les noms de categories ou de tags — toujours remplacer par "et" (Hugo genere un double tiret `--` dans le slug, ce qui casse les URLs)
- Le ton des articles est impersonnel (pas de je/tu/nous/vous) sauf instruction contraire
- Les specs d'article (mots minimum, H2, blocs obligatoires) dependent du type choisi — lire les `<!-- NOTES POUR CLAUDE -->` dans chaque template d'article
- Chaque article doit contenir au minimum 3 liens internes contextuels vers d'autres articles du blog. L'ancre de chaque lien doit contenir le mot-cle principal de l'article cible. **Maillage intra-langue uniquement** : un article FR ne mail que des articles FR, un article EN ne mail que des articles EN (le lien vers la traduction est gere par le language switcher du header)
- **Systeme d'auteurs partage** : 6 auteurs fictifs definis dans `data/authors.yaml` (copie depuis `.claude/templates/data/authors.yaml` a la creation du site). Chaque auteur a un id-slug, un nom, un type (person/organization), un avatar, des `jobTitle`/`role`/`bio` bilingues FR/EN, une liste d'`expertise` et une liste de `topics` (mots-cles pour la selection automatique). Les auteurs disponibles : `thomas-durand` (tech), `magalie-ergoz` (mode/beaute), `claire-beaumont` (maison/habitat), `laura-verdier` (sante/bien-etre), `kevin-moreau` (transport/mobilite), `sophie-martin` (finance/patrimoine)
- **Selection automatique de l'auteur** : dans le frontmatter d'un article, le champ `author` contient l'**ID slug** de l'auteur (ex: `author: thomas-durand`), pas son nom complet. La skill `/create-article-geo` selectionne automatiquement l'auteur le plus pertinent selon les `topics` et `expertise` qui matchent avec le sujet de l'article. Si aucun match clair, l'auteur principal du site (defini dans la section "Contexte du site" de ce CLAUDE.md) est utilise
- **Avatars des auteurs** : fichiers WebP 512x512 dans `static/images/authors/[id].webp`. Style unifie "flat illustration portrait". Prompts de generation documentes dans `.claude/templates/data/avatar-prompts.md`. Si l'avatar est manquant, un placeholder coloree avec la 1ere lettre du nom s'affiche
- **JSON-LD Author** : le partial `seo-head.html` genere automatiquement un schema.org/Person (ou Organization) complet depuis les donnees de `data/authors.yaml` (name, jobTitle, description, knowsAbout, image, sameAs, worksFor)
- **Bloc auteur en bas d'article** : le layout `single.html` affiche automatiquement un encart avec avatar, nom, role, bio complete et expertise de l'auteur, traduit dans la langue de l'article (FR ou EN)
- Les templates SEO dans `.claude/templates/seo/` sont editables par l'utilisateur — toujours lire la version en place avant de generer
- Pour ajouter un nouveau type d'article, creer un `.md` dans `.claude/templates/articles/` — il sera automatiquement propose par `/create-article-geo`
- Pour ajouter un schema JSON-LD, creer un `.json` dans `.claude/templates/seo/structured-data/` et utiliser `/seo` pour l'integrer
- Chaque article doit avoir un champ `lastmod` dans le frontmatter (= date de derniere modification). Il est utilise par le sitemap XML, le sitemap HTML et le schema JSON-LD
- Quand un article est modifie, toujours mettre a jour le champ `lastmod` avec la date du jour
- Le sitemap HTML (`/plan-du-site/`) se regenere automatiquement a chaque build Hugo
- Toujours build et verifier (`hugo`) avant de commit
- Chaque article doit avoir un champ `faq` dans le frontmatter (liste de questions/reponses) pour generer automatiquement le schema FAQPage JSON-LD. Minimum 3 questions
- Chaque article a une image hero OBLIGATOIRE, recuperee automatiquement par `.claude/scripts/fetch-image.sh` au moment de `/create-article-geo`. L'image vient de la **cascade Pexels -> Unsplash -> Openverse -> visuel de charte genere** (revue le 2026-09-04 : Openverse n'est plus la source nominale, il ne produisait que 15 photos pertinentes sur 45 heros mesures). Les cles `PEXELS_API_KEY` et `UNSPLASH_ACCESS_KEY` viennent du **prompt de la routine** ou du `.env` local, **jamais du repo** qui est public ; sans cle la cascade demarre a Openverse et l'article sort quand meme. Le registre `.claude/hero-sources.json` empeche deux articles de porter la meme photo. **Controle visuel obligatoire avant publication, quelle que soit la banque.** Convertie en WebP automatiquement si `cwebp` est installe. Stockee dans `static/images/blog/[slug].webp`
- Le frontmatter contient 3 champs lies a l'image : `image` (chemin Hugo), `imageAlt` (texte alternatif FR, max 125 car), `imageCredit` (attribution du photographe). Ces 3 champs sont remplis automatiquement par le script
- L'image est affichee : (1) dans les cards de la homepage et des pages de liste, (2) en bannière cote a cote avec le titre sur la page article, (3) dans og:image pour les partages sociaux, (4) dans le schema Article JSON-LD
- Le credit photo est affiche sous l'image de l'article (petite mention en italique alignee a droite). Obligatoire pour respecter les licences CC BY et CC BY-SA
- Les Google Fonts sont chargees en non-bloquant (media="print" + swap JS) pour de meilleures performances
- Le layout inclut un lien "Skip to content" pour l'accessibilite
- Les navigations ont des `aria-label` pour les lecteurs d'ecran
- Le CSS respecte `prefers-reduced-motion` pour desactiver les animations si l'utilisateur le demande
- Les articles affichent une table des matieres (TOC) sticky en sidebar, generee automatiquement par Hugo
- Les articles similaires sont affiches en bas de page, calcules par Hugo via la config `[related]` dans hugo.toml

## Publications evergreen automatiques

En plus des articles GEO (geo-comparatif, rediges a la main via `/create-article-geo`), chaque blog peut publier automatiquement des articles evergreen SEO. Deux methodes coexistent dans le reseau, le choix se fait par blog en fonction du contexte (modele, frequence, fetch concurrents, maillage).

### Methode 1 : CCR cloud auto (`/create-article-auto`)

- **Skill** : `/create-article-auto`
- **Execution** : sandbox cloud Anthropic (CCR), declenchee par une routine `/schedule` (cron 2x/semaine, mardi + vendredi 3h du matin)
- **Modele** : `claude-sonnet-5`
- **Analyse concurrentielle** : via l'outil natif `WebSearch` (execute cote serveur, insensible au proxy d'egress du sandbox). **Plus de SerpAPI.** Repli "kw seul" si WebSearch indispo. Ne pas fetcher les pages concurrentes (WebFetch bloque) : analyse sur titres + snippets uniquement
- **Maillage cross-batch** : non (1 article a la fois)
- **Publication** : push immediat -> en ligne tout de suite
- **Cas d'usage** : tient la cadence sans intervention humaine, ideal pour les blogs avec roadmap stable
- **Exemple en prod dans le reseau** : `magazine-como`

### Methode 2 : batch local + GitHub Actions cron (`/create-article-seo`)

- **Skill** : `/create-article-seo` polyvalente
- **Execution** : Mac de Damien (local), Opus 4.7 sans contrainte
- **Modele** : Opus 4.7 (qualite max, pas de bug timeout)
- **Fetch concurrents** : marche normalement, analyse SERP avec lecture des 3-5 pages concurrentes
- **Maillage cross-batch** : oui (les articles produits dans une meme batch se citent entre eux)
- **3 modes au choix** :
  - **(A) Roadmap blog** : N premieres entrees `todo` triees par scheduled_date
  - **(B) Roadmap externe** : roadmap fournie par l'utilisateur (Sheet, KW client)
  - **(C) KW a la demande** : 1 ou plusieurs KW dans le chat
- **3 strategies de scheduling** :
  - Garder les `scheduled_date` source (defaut)
  - Cascade remapping a partir d'une date X (decale en avant)
  - Prochain slot dispo dans la cadence (mardi/vendredi non occupe)
- **Publication** : article ecrit avec `publishDate` futur. Hugo (`buildFuture: false`) le masque jusqu'a la date. GitHub Actions cron mardi/vendredi 3h Paris rebuild le site, l'article apparait automatiquement quand sa date est arrivee.
- **Cas d'usage** : production en lot mensuelle, qualite max, maillage interne propre
- **Exemple en prod dans le reseau** : `ma-bonne-sante`

### Principe commun aux 2 methodes

- **SEO pur**, pas GEO : pas de "prompt GEO", pas de "En bref numerote". Juste un mot-cle SEO cible, analyse SERP, structure Hn basee sur les concurrents, redaction optimisee.
- **Bilingue FR + EN** comme tous les articles du reseau (trad directe de la version FR).
- **Human in the loop** uniquement sur la roadmap : c'est l'humain qui decide des mots-cles a cibler et de leur date de publication.

### Roadmap editoriale

Fichier : `roadmap.yaml` a la racine du blog. Format documente dans `.claude/templates/roadmap-template.yaml`.

Chaque entree = 1 article a publier. Champs editables par l'humain :
- `kw` (obligatoire) : mot-cle SEO principal dans la langue principale du blog
- `category` (obligatoire) : doit matcher une categorie definie dans `hugo.toml`
- `scheduled_date` (obligatoire) : date a partir de laquelle l'agent peut publier (YYYY-MM-DD)
- `status` : `todo` | `done` | `failed`

Champs remplis par l'agent (ne pas toucher sauf pour reactiver un `failed`) :
- `published_date`, `published_url_fr`, `published_url_en`, `error`

### Comment l'humain modifie la roadmap

- **Ajouter une entree** : copier un bloc existant, remplir `kw` + `category` + `scheduled_date`, laisser les autres champs tels quels, garder `status: todo`.
- **Reporter une entree** : modifier `scheduled_date`.
- **Annuler une entree non encore traitee** : supprimer le bloc, ou passer `status` a `done` manuellement (l'agent l'ignorera).
- **Debloquer un `failed`** : corriger la cause (ex: `kw` trop concurrentiel, category invalide), repasser `status: todo`, vider `error`.

Demander a Claude "ajoute telle entree a la roadmap du blog X" ou "passe la roadmap de X ca" fonctionne aussi, tant que le format YAML reste respecte.

### Execution

- **Manuelle (test methode 1)** : se placer dans le dossier du blog, taper `/create-article-auto`. L'agent prend la prochaine entree eligible et deroule.
- **Planifiee (production methode 1)** : routine `/schedule` qui lance `/create-article-auto` dans le contexte du blog, 2x/semaine (mardi + vendredi, 3h du mat recommande pour minimiser les conflits avec les autres consultants).
- **Batch (methode 2)** : se placer dans le dossier du blog, taper `/create-article-seo`. La skill propose les 3 modes (A/B/C) puis les 3 strategies de scheduling. Articles produits avec `publishDate` futur, Hugo les masque, le cron GitHub Actions du blog (mardi/vendredi 3h Paris) les rend visibles automatiquement quand leur date arrive.

### Echecs

Une entree qui echoue passe en `status: failed` avec `error: "[etape] [message]"`. Elle n'est **pas retentee automatiquement**. L'humain corrige, repasse en `todo`, l'agent la reprendra au lancement suivant.

Le suivi des articles publies en auto se fait via :
- Le champ `published_date` / URLs de chaque entree de la roadmap
- Le `MEMORY.md` a la racine du blog (suffixe ` | auto` sur les lignes generees par cette skill)

## Comment repondre a l'utilisateur

- Tutoiement, ton decontracte
- Pas de jargon technique sans explication
- Reponses structurees avec listes a puces
- Pas d'emoji sauf demande explicite
