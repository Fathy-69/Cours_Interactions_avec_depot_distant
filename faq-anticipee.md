# FAQ anticipée — Live #1
## Les 10 questions les plus probables, avec réponses prêtes à dire

> **Usage de cette FAQ** :
> - À avoir sur ton second écran ou imprimée pendant le live, comme antisèche.
> - Si tu sèches sur une question, jette un œil discret — pas de honte.
> - Si une question correspond à une de ces 10, tu réponds avec assurance ; sinon, tu peux toujours dire *"bonne question, je creuse et je poste la réponse dans le canal d'ici lundi"*.
>
> **Format** : chaque question contient une **réponse courte** (15-30 secondes à dire) et une **réponse longue** (si la personne creuse). Choisis selon le contexte du moment.

---

## Q1 — Pourquoi WSL2 plutôt qu'installer Linux directement (dual-boot) ?

**Réponse courte** *(à dire en live)*

> Le dual-boot fonctionne très bien, mais il a deux gros défauts pour apprendre : tu redémarres ton PC à chaque fois que tu veux changer d'OS — au bout de deux semaines, tu cesses d'utiliser Linux par paresse. Et l'installation est risquée, on touche à tes partitions, on peut perdre des données. WSL2 te donne 95% des bénéfices de Linux sans aucun de ces inconvénients : Linux est intégré à Windows, accessible en une commande, sans redémarrage, sans risque sur tes données. Microsoft l'a conçu exactement pour notre cas d'usage.

**Réponse longue**

Le dual-boot reste pertinent dans deux cas :
1. Tu basculades définitivement à Linux comme OS principal — Windows ne te sert plus.
2. Tu fais du dev très spécialisé qui exige un Linux natif (kernel hacking, drivers).

Pour 99% des devs, et 100% des débutants en formation, WSL2 est suffisant et meilleur. La seule chose que WSL2 ne fait pas aussi bien que Linux natif : les interfaces graphiques Linux (GUI) — mais on ne fait quasi jamais de GUI en dev moderne, on travaille en CLI ou via des apps web.

---

## Q2 — Est-ce qu'il faut payer GitHub ?

**Réponse courte**

> Non, GitHub est gratuit pour quasi tout. Tu peux créer un nombre illimité de repos publics ET privés gratuitement, avec des collaborateurs, depuis 2019. La version payante est pour les entreprises avec des besoins avancés (sécurité, conformité, support). Pour toute notre formation, le compte gratuit est largement suffisant.

**Réponse longue**

Les plans GitHub :
- **Free** : illimité repos publics et privés, 2 000 minutes/mois de GitHub Actions, 500 Mo de stockage Packages.
- **Pro** (~4 €/mois) : plus de minutes Actions, plus de stockage, accès à des features comme les protected branches sur repos privés. **Pas nécessaire pour débuter.**
- **Team / Enterprise** : pour les organisations.

Anecdote utile : avec ton statut étudiant, tu peux demander GitHub Student Developer Pack qui te donne Pro gratuit + des dizaines d'outils premium (Notion, JetBrains IDEs, Heroku...). À faire dès que ton statut Graduate Dev IA est établi.

---

## Q3 — C'est quoi exactement un commit ?

**Réponse courte**

> Un commit, c'est un instantané, une photo, de l'état de ton dossier à un moment précis. Tu décides toi-même quand tu prends la photo, et tu l'accompagnes d'un petit message qui explique ce que tu as changé. Cette photo est gravée dans l'historique de Git, et tu pourras toujours y revenir, même dans 10 ans.

**Réponse longue**

Techniquement, un commit contient :
- Une **empreinte unique** (un hash SHA-1, du genre `a3f8b2e...`) qui identifie ce commit
- Un **pointeur vers le commit parent** (le commit précédent dans l'historique)
- L'**état complet de tous les fichiers** au moment du commit (ou plus précisément, les différences avec le parent, optimisé en interne)
- Les **métadonnées** : ton nom, ton email, la date, le message

C'est cette structure de pointeurs entre commits qui permet à Git de reconstruire n'importe quelle version antérieure du projet.

Analogie : c'est comme un game save dans un jeu vidéo. Tu sauvegardes quand tu veux. Tu peux revenir à n'importe quelle sauvegarde. Tu peux même faire des "branches" depuis une vieille sauvegarde, comme un univers parallèle.

---

## Q4 — Pourquoi `git add` puis `git commit` ? Pourquoi pas tout en une commande ?

**Réponse courte**

> Bonne question, c'est la première chose qui frustre quand on débute. La séparation en deux étapes existe pour te donner du contrôle. Imagine : tu as modifié 10 fichiers dans la matinée, sur 3 sujets différents. Au lieu de faire un commit fourre-tout "j'ai bossé ce matin", tu peux faire 3 commits propres : `git add` les fichiers du sujet 1 → commit, puis sujet 2 → commit, puis sujet 3 → commit. Tu obtiens un historique lisible. C'est l'antichambre du commit, et c'est puissant une fois qu'on s'y habitue.

**Réponse longue**

Il existe un raccourci pour les cas simples : `git commit -am "message"` qui fait `add` et `commit` en une seule étape. Mais ça ne fonctionne que pour les fichiers déjà suivis par Git (pas les nouveaux fichiers).

Le vrai gain de la staging area apparaît quand tu maîtrises Git :
- `git add -p` te permet de stager **une partie d'un fichier** (certaines lignes seulement). Tu peux donc avoir un fichier modifié sur 2 sujets différents et faire 2 commits propres.
- Tu peux faire `git add` puis te dire "non, ce n'est pas prêt", retirer avec `git restore --staged`, modifier encore, et stager à nouveau.

Quand tu auras compris la staging area en profondeur, tu réaliseras que c'est l'une des fonctionnalités les plus intelligentes de Git.

---

## Q5 — Et si je fais une bêtise, je peux annuler ?

**Réponse courte**

> Oui, presque toujours. Git est paranoïaque, il garde une trace de tout, même de tes erreurs. Tu peux annuler un commit, revenir à une version antérieure, "défaire" un add, restaurer un fichier supprimé... Il y a une dizaine de commandes pour ça, on les verra dans le prochain live. Règle : tant que tu n'as pas fait `git push --force` (qu'on évitera longtemps), tu peux récupérer ton travail. Git est un filet de sécurité, pas un piège.

**Réponse longue**

Hiérarchie de récupération, du plus simple au plus avancé :
- **Annuler une modification non encore commitée** : `git restore fichier.txt`
- **Sortir un fichier de la staging area** : `git restore --staged fichier.txt`
- **Modifier le dernier commit** (ajouter un fichier oublié, corriger le message) : `git commit --amend`
- **Annuler un commit en créant un commit "inverse"** : `git revert hash`
- **Revenir en arrière de plusieurs commits** : `git reset --hard hash` (⚠️ destructif sur les changements non commitées)
- **Récupérer un commit "perdu"** : `git reflog` te montre TOUS les commits que tu as faits, même ceux que tu pensais avoir supprimés. Tu peux ressusciter un commit d'il y a 2 semaines avec son hash.

Le seul cas où tu peux vraiment perdre du travail : `git push --force` sur un repo distant, qui écrase l'historique. À éviter sauf cas très spécifique.

---

## Q6 — C'est quoi la différence entre Git et SVN / Mercurial / autres ?

**Réponse courte**

> Ce sont tous des systèmes de gestion de versions, mais Git a une particularité : il est **distribué**. Chez SVN, il y a un serveur central et tout le monde y est connecté. Chez Git, chaque développeur a une copie complète de l'historique sur sa machine — tu peux travailler hors ligne, faire 50 commits, et synchroniser après. C'est ce qui a fait gagner Git à partir de 2010. Aujourd'hui, plus de 95% des projets utilisent Git.

**Réponse longue**

Bref historique :
- **SVN** (Subversion, 2000) : centralisé. Encore utilisé dans certaines grosses boîtes, surtout en France dans le secteur public ou bancaire. Fonctionne bien, mais lent et fragile en cas de panne du serveur central.
- **Mercurial** (2005) : distribué comme Git, philosophie un peu différente, syntaxe parfois plus simple. Utilisé par Facebook (en interne) et Mozilla pendant longtemps. Aujourd'hui marginal.
- **Git** (2005) : créé par Linus Torvalds en 2 semaines pour gérer le code du noyau Linux. Il voulait remplacer un outil propriétaire (BitKeeper) qui avait coupé l'accès au noyau. Il est devenu le standard mondial.
- **Fossil**, **Pijul**, **Sapling** : alternatives modernes, marginales mais intéressantes.

Pour ta carrière : connaître Git est un prérequis non-négociable. SVN peut être un plus dans certains contextes legacy.

---

## Q7 — Pourquoi `main` et pas `master` ?

**Réponse courte**

> Historiquement, la branche principale s'appelait `master`. En 2020, suite au mouvement Black Lives Matter, GitHub et la plupart des projets ont décidé de remplacer `master` par `main` parce que le terme "master" évoque la relation maître/esclave. Aucune raison technique, c'est purement social et symbolique. Aujourd'hui, tous les nouveaux projets sont en `main`. Tu peux encore croiser `master` sur de vieux projets, c'est exactement la même chose.

**Réponse longue**

Précisions :
- Le nom `master` venait à l'origine de l'idée de "master copy" (la copie maîtresse, la version de référence), pas directement d'une analogie maître/esclave. Mais l'industrie a quand même choisi de migrer pour éviter toute ambiguïté.
- Quelques projets ont choisi d'autres noms : `trunk` (héritage SVN), `default`, `develop`. Mais `main` est devenu le standard.
- Au niveau technique, tu peux nommer ta branche principale comme tu veux. Git s'en moque. Ce qui compte c'est la convention dans ton équipe.
- Si tu rejoins un projet existant en `master`, ne le renomme pas tout seul — c'est une décision d'équipe.

---

## Q8 — C'est quoi la différence entre un fork, un clone et une branche ?

**Réponse courte**

> Trois mots qui se ressemblent mais qui veulent dire des choses différentes :
> - **Cloner**, c'est télécharger un repo de GitHub vers ta machine. Tu en as une copie locale.
> - **Forker**, c'est faire une copie d'un repo GitHub vers TON compte GitHub. Tu en deviens propriétaire (de la copie). C'est ce qu'on fait quand on veut contribuer à un projet open source sans avoir le droit de pousser dessus directement.
> - **Brancher**, c'est créer une nouvelle branche à l'intérieur d'un repo. C'est local à un repo, pas une copie séparée.

**Réponse longue**

Pour bien comprendre la hiérarchie :

```
Repo original (sur GitHub, propriétaire = quelqu'un d'autre)
   │
   ├── Tu peux le CLONER → copie locale chez toi
   │       └── Dans cette copie, tu peux créer des BRANCHES
   │
   └── Tu peux le FORKER → copie sur ton compte GitHub
           └── Et ensuite cloner ton fork → copie locale chez toi
                   └── Dans cette copie, tu peux créer des BRANCHES
```

Cas d'usage typiques :
- **Tu travailles sur ton propre projet** : tu n'as ni cloné ni forké, tu as fait `git init` localement et tu as créé un repo GitHub vide pour le synchroniser.
- **Tu rejoins un projet de ton équipe** : tu **clones** le repo de l'équipe.
- **Tu veux contribuer à un projet open source** (ex: ajouter une feature à React) : tu **forkes** le repo, tu **clones** ton fork, tu codes, tu pousses sur ton fork, tu ouvres une Pull Request vers le repo original.

---

## Q9 — Est-ce que ChatGPT ou Copilot remplacent l'apprentissage de Git ?

**Réponse courte**

> Non, mais ils accélèrent énormément. Tu vas utiliser Copilot dans VS Code pour le code, et ChatGPT pour comprendre les messages d'erreur Git. C'est même fortement recommandé. Mais il faut comprendre les concepts de base : si tu ne sais pas ce qu'est un commit, ChatGPT te donnera des commandes que tu ne sauras pas évaluer, et tu vas faire des bêtises sans le savoir. Apprendre Git avec ChatGPT comme tuteur est une excellente approche. Apprendre Git en demandant à ChatGPT de tout faire à ta place ne marche pas.

**Réponse longue**

Mes recommandations pratiques :
- **Copilot dans VS Code** : excellent pour le code (Python, JavaScript), mais aussi pour suggérer des messages de commit. Active-le dès que tu es à l'aise avec l'éditeur.
- **ChatGPT / Claude pour les erreurs Git** : copie-colle l'erreur, demande l'explication. C'est plus rapide que Stack Overflow.
- **ChatGPT pour expliquer une commande complexe** : "qu'est-ce que fait `git rebase -i HEAD~3` ?" → tu auras une explication claire.
- **ChatGPT/Claude pour générer une commande** : "j'ai 5 commits que je veux fusionner en 1, comment faire ?" → réponse pertinente.

⚠️ Ce qu'il ne faut pas faire :
- Exécuter une commande Git sans la comprendre, surtout si elle contient `--force`, `reset --hard`, ou modifie l'historique distant.
- Te reposer sur Copilot pour faire le travail à ta place — tu n'apprends rien.

---

## Q10 — Combien de temps avant que je sois autonome avec Git ?

**Réponse courte**

> Pour les usages quotidiens basiques (commit, push, pull, branche, merge simple) : 2 à 3 semaines de pratique régulière. Pour être à l'aise avec les conflits de merge, le rebase, la résolution d'incidents : 2 à 3 mois. Pour vraiment maîtriser (rebase interactif, cherry-pick, reflog, hooks) : 1 an. Mais la bonne nouvelle, c'est que tu peux faire 95% de ce dont tu as besoin avec 10 commandes seulement. Le reste, tu Google quand tu en as besoin.

**Réponse longue**

Étapes typiques d'apprentissage de Git :

| Niveau | Compétences | Temps de pratique |
|---|---|---|
| Survie | init, add, commit, status, log | 1 semaine |
| Quotidien | branch, checkout, merge, push, pull, clone | 2-3 semaines |
| Confort | resolution de conflits, .gitignore, remote, fetch, rebase simple | 1-2 mois |
| Maîtrise | rebase interactif, cherry-pick, stash, reflog, bisect | 6-12 mois |
| Expert | hooks, submodules, custom workflows, internals (objects, refs) | 2+ ans |

Ce qui accélère :
- **Faire des erreurs** dans des repos de test (sans pression).
- **Travailler à plusieurs** : c'est en collaborant qu'on rencontre les vrais cas (conflits, rebases).
- **Lire l'historique** des projets open source — `git log` sur React, Vue, Django t'apprend les bonnes pratiques de commits.

Ce qui ralentit :
- Apprendre Git tout seul sur un projet personnel (pas de conflits, pas de PR, pas de revue).
- Apprendre les commandes par cœur sans comprendre les concepts.
- Avoir peur des erreurs et ne jamais oser tester.

---

## Bonus — 3 questions piège possibles

### B1 — "Pourquoi tu ne nous fais pas installer en direct, ce serait plus efficace ?"

> Bonne question. Trois raisons : un, j'ai 10 personnes avec 10 configurations différentes, on perdrait 30 minutes à débugger les cas particuliers ; deux, l'installation prend 1h à elle seule, on n'aurait pas le temps de comprendre ce qu'on installe ; trois, je préfère qu'on parte tous avec le même modèle mental clair, et que vous installiez ensuite chez vous, à votre rythme, en sachant ce que vous faites. Le live d'aujourd'hui pose les concepts. La semaine pose les commandes.

### B2 — "T'es PM mais tu nous parles comme un dev, comment ça se fait ?"

> Honnêtement, je suis PM en train de devenir dev — comme vous. Je ne maîtrise rien de plus que vous, j'ai juste préparé ce live en amont avec un guide structuré. Mon objectif c'est de comprendre les outils que mes futurs équipes utilisent, et de partager le chemin avec vous. Je suis pas un sachant, je suis un éclaireur. Si je me plante sur un point, dites-le moi.

### B3 — "Tu as utilisé une IA pour préparer ce live ?"

> Oui, totalement assumé. J'ai coconstruit ce guide et ces slides avec Claude (un assistant IA d'Anthropic), en plusieurs sessions de challenge mutuel. Je lui ai donné le cahier des charges, il m'a remis en question, on a itéré. C'est exactement la posture que vous aurez en formation Graduate Dev IA : l'IA est un partenaire de pensée, pas un oracle. Je peux vous montrer la conversation si ça vous intéresse, c'est instructif.

---

## Mémo : que faire si tu ne sais vraiment pas répondre

Trois formules à avoir en tête :

1. **L'aveu honnête** : *"Très bonne question, je n'ai pas la réponse précise. Je note, je creuse cette semaine, je poste la réponse dans le canal d'ici lundi."*

2. **Le retour à la communauté** : *"Bonne question. Quelqu'un d'autre dans la salle a peut-être la réponse ? Sinon je note."*

3. **Le hors-scope assumé** : *"Bonne question, mais on déborde du sujet du live. Je propose qu'on en reparle en privé après, ou au prochain live."*

⚠️ À ne JAMAIS faire :
- Inventer une réponse que tu n'es pas sûr de toi.
- Bredouiller sans assumer que tu ne sais pas.
- Renvoyer la personne sèchement vers Google.
