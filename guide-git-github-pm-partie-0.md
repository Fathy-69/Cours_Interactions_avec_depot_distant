# Guide opérationnel Git & GitHub pour PM SaaS

## Partie 0 — Préparer le terrain

> Version 0.1 — Livraison 1
> Public cible : Product Manager SaaS souhaitant maîtriser Git et GitHub comme un développeur, dans une stack Python/Django + SQL + MongoDB, sur Windows 11 (avec bascule future vers Linux).
> Environnement de travail : WSL2 + Ubuntu + VS Code.

---

## Sommaire de la Partie 0

- Chapitre 0.1 — Comprendre l'écosystème : Git, GitHub, VS Code, WSL
- Chapitre 0.2 — Installer WSL2 et Ubuntu sur Windows 11
- Chapitre 0.3 — Installer VS Code et le connecter à WSL2
- Chapitre 0.4 — Installer Git et configurer votre identité

**Durée totale estimée** : 1h30 à 2h.
**Livrable final** : un environnement de travail opérationnel (WSL2 + Ubuntu + VS Code + Git configuré), prêt pour les manipulations Git de la Partie 1.

---

## Chapitre 0.1 — Comprendre l'écosystème : Git, GitHub, VS Code, WSL

**Objectif d'apprentissage**
À la fin de ce chapitre, vous saurez expliquer en une minute, à un collègue non-technique, ce que sont Git, GitHub, VS Code et WSL, et comment ces quatre briques s'articulent. Cette clarté conceptuelle est le socle de tout ce qui suit.

**Durée estimée** : 20 minutes de lecture, 0 manipulation.

**Prérequis** : aucun.

---

### Le modèle mental en une image

Avant de toucher à la moindre commande, vous devez visualiser cette architecture. Lisez-la deux fois.

```
┌─────────────────────────────────────────────────────────────────┐
│  VOTRE MACHINE (Windows 11)                                      │
│                                                                  │
│  ┌─────────────────────────┐      ┌───────────────────────────┐ │
│  │  VS Code (Windows)       │◄────►│  WSL2 — Ubuntu Linux      │ │
│  │  Éditeur de code         │      │                           │ │
│  │  Interface visuelle      │      │  ┌─────────────────────┐  │ │
│  │                          │      │  │  Git (logiciel)     │  │ │
│  │  Terminal intégré ───────┼─────►│  │  Versionne vos      │  │ │
│  │                          │      │  │  fichiers en local  │  │ │
│  │                          │      │  └──────────┬──────────┘  │ │
│  │                          │      │             │             │ │
│  │                          │      │  ┌──────────▼──────────┐  │ │
│  │                          │      │  │  Votre projet       │  │ │
│  │                          │      │  │  (dossier + .git/)  │  │ │
│  │                          │      │  └─────────────────────┘  │ │
│  └─────────────────────────┘      └────────────┬──────────────┘ │
│                                                │                 │
└────────────────────────────────────────────────┼─────────────────┘
                                                 │
                                                 │ git push / pull
                                                 │ via SSH
                                                 ▼
                              ┌──────────────────────────────────┐
                              │  GITHUB.COM (serveur distant)    │
                              │                                  │
                              │  ┌────────────────────────────┐  │
                              │  │  Copie de votre projet     │  │
                              │  │  + Issues + Pull Requests  │  │
                              │  │  + Actions (CI/CD)         │  │
                              │  └────────────────────────────┘  │
                              └──────────────────────────────────┘
```

### Décomposition brique par brique

**Git** est un **logiciel** que vous installez sur votre machine. Il a une seule mission : surveiller un dossier et enregistrer toutes les modifications de ses fichiers, à votre demande, pour pouvoir revenir en arrière, comparer des versions, ou fusionner des changements faits en parallèle. Git fonctionne **entièrement en local**, sans Internet. Il n'a pas d'interface graphique native : on le pilote via la **ligne de commande** dans un **terminal**.

**GitHub** est un **service en ligne** (un site web + une API) qui héberge des copies distantes des projets versionnés par Git. GitHub n'est pas Git ; c'est un *hébergeur* de projets Git, parmi d'autres (GitLab, Bitbucket…). GitHub ajoute par-dessus Git des fonctionnalités collaboratives : Issues (tickets), Pull Requests (demandes d'intégration de code), Actions (automatisations), Projects (gestion de tâches).

> ⚠️ **Confusion fréquente** : "pousser sur GitHub" ne veut pas dire que GitHub fait quelque chose de magique. Cela veut dire que **votre logiciel Git local envoie une copie de vos commits à GitHub via Internet**. GitHub ne fait que stocker ce que vous lui envoyez.

**VS Code** (Visual Studio Code) est un **éditeur de code** développé par Microsoft. C'est l'outil dans lequel vous écrivez et modifiez vos fichiers (Python, SQL, Markdown, etc.). VS Code n'est pas Git, mais il intègre nativement deux choses qui nous intéressent : un **terminal intégré** (où vous taperez vos commandes Git) et une **interface visuelle Git** (qui montre les modifications en couleurs dans la marge). VS Code est notre poste de pilotage.

**WSL2** (Windows Subsystem for Linux, version 2) est une **technologie Microsoft** qui fait tourner un vrai noyau Linux à l'intérieur de Windows 11, de manière transparente. Concrètement, vous installez **Ubuntu** (une distribution Linux populaire) à l'intérieur de Windows, et vous pouvez ouvrir un terminal Ubuntu sans quitter Windows. Tous les outils de développement professionnels (Git, Python, Docker, Node.js…) tournent dans Ubuntu, comme sur un serveur de production. Quand vous basculerez plus tard sur Linux natif, vos habitudes seront déjà les bonnes.

### Comment ces quatre briques travaillent ensemble

Voici le flux de travail que vous adopterez, et que vous pratiquerez dans tous les chapitres suivants :

1. Vous ouvrez **VS Code** sur Windows.
2. VS Code se connecte à **WSL2 / Ubuntu** via une extension officielle (transparent : vous ne voyez pas la différence avec un VS Code natif Linux).
3. Vous éditez vos fichiers Python/Django dans VS Code.
4. Dans le **terminal intégré** de VS Code (qui est en réalité un terminal Ubuntu via WSL2), vous tapez des commandes **Git** pour enregistrer vos modifications.
5. Quand vous voulez synchroniser avec votre équipe, vous tapez `git push` : Git envoie vos modifications à **GitHub** via Internet.
6. Vos collègues récupèrent vos modifications avec `git pull` depuis leur propre machine.

### Vérification de compréhension

Avant de passer au chapitre suivant, vous devez pouvoir répondre à ces questions sans relire :

1. Si Internet tombe en panne, puis-je continuer à utiliser Git ? *(Oui, Git est local. Seuls `push` et `pull` nécessitent Internet.)*
2. GitHub peut-il fonctionner sans Git ? *(Non, GitHub est un hébergeur de projets Git.)*
3. VS Code remplace-t-il Git ? *(Non, VS Code est un éditeur. Git reste un outil distinct, piloté en ligne de commande dans le terminal de VS Code.)*
4. Pourquoi installer WSL2 plutôt que d'utiliser Git directement sous Windows ? *(Pour travailler dans le même environnement que les serveurs de production, éviter les pièges de fins de ligne et de chemins, et préparer la bascule future vers Linux natif.)*

Si vous hésitez sur l'une de ces réponses, relisez le chapitre. Si tout est clair, on passe à l'installation.

---

## Chapitre 0.2 — Installer WSL2 et Ubuntu sur Windows 11

**Objectif d'apprentissage**
À la fin de ce chapitre, vous aurez un environnement Ubuntu Linux opérationnel à l'intérieur de votre Windows 11, avec un compte utilisateur configuré, accessible en une seule commande depuis n'importe quel terminal Windows.

**Durée estimée** : 30 à 45 minutes (dont 10-15 minutes de téléchargement selon votre connexion).

**Prérequis** :
- Windows 11 à jour (version 22H2 ou supérieure recommandée).
- Droits administrateur sur la machine.
- Connexion Internet stable.
- Environ 2 Go d'espace disque libre.

---

### Modèle mental

WSL2 ne crée **pas** une partition de disque. Il ne crée **pas** une machine virtuelle au sens classique (VirtualBox/VMware). Il utilise une technologie de virtualisation ultra-légère intégrée à Windows (Hyper-V) pour faire tourner un vrai noyau Linux dans un container quasi-invisible. Concrètement :

- Vous ne redémarrez **pas** votre machine pour passer à Linux : vous ouvrez juste un terminal.
- Vos fichiers Linux sont stockés dans un fichier virtuel (`ext4.vhdx`) géré par Windows.
- Vous pouvez accéder à vos fichiers Windows depuis Linux (via `/mnt/c/`) et inversement, mais **on évitera de le faire** pour des raisons de performance et de propreté. Règle d'or : **vos projets de développement vivent dans Ubuntu, pas dans Windows.**

### Étape 1 — Vérifier votre version de Windows 11

Ouvrez le menu Démarrer, tapez `winver`, appuyez sur Entrée. Une fenêtre s'affiche.

**Output attendu** : "Version 22H2" ou supérieure (23H2, 24H2…). Si vous êtes en dessous de 22H2, lancez Windows Update et mettez à jour avant de continuer.

### Étape 2 — Ouvrir PowerShell en administrateur

1. Cliquez sur le menu Démarrer.
2. Tapez `PowerShell`.
3. Sur l'icône "Windows PowerShell" qui apparaît, faites un **clic droit → Exécuter en tant qu'administrateur**.
4. Une fenêtre bleue (ou noire selon votre thème) s'ouvre. Le prompt ressemble à : `PS C:\WINDOWS\system32>`.

> ⚠️ **Vigilance** : si vous ne lancez pas PowerShell en admin, la commande suivante échouera avec une erreur de privilèges. Ne sautez pas cette étape.

### Étape 3 — Installer WSL2 et Ubuntu en une commande

Dans la fenêtre PowerShell admin, tapez **exactement** :

```powershell
wsl --install
```

Appuyez sur Entrée.

**Ce qui se passe** : Windows active les composants nécessaires (Hyper-V, plateforme de machine virtuelle), télécharge le noyau Linux WSL2, et installe Ubuntu (la distribution par défaut). C'est un processus automatique de 5 à 15 minutes.

**Output attendu** (extrait) :

```
Installing: Virtual Machine Platform
Virtual Machine Platform has been installed.
Installing: Windows Subsystem for Linux
Windows Subsystem for Linux has been installed.
Installing: Ubuntu
Ubuntu has been installed.
The requested operation is successful. Changes will not be effective until the system is rebooted.
```

### Étape 4 — Redémarrer la machine

C'est **obligatoire**. Sans redémarrage, WSL2 n'est pas opérationnel.

1. Fermez tout votre travail en cours.
2. Menu Démarrer → bouton Marche/Arrêt → **Redémarrer**.

> ⚠️ **Vigilance** : ne choisissez pas "Arrêter" puis "Démarrer". Sur Windows 11, "Arrêter" utilise un mode de démarrage rapide qui ne réinitialise pas certains composants. Il faut bien **Redémarrer**.

### Étape 5 — Configurer votre utilisateur Ubuntu

Au redémarrage, une fenêtre Ubuntu noire s'ouvre **automatiquement** (si elle ne s'ouvre pas, allez dans le menu Démarrer et cliquez sur "Ubuntu"). Le système finit de s'installer (1-2 minutes), puis vous demande :

```
Please create a default UNIX user account. The username does not need to match your Windows username.
For more information visit: https://aka.ms/wslusers
Enter new UNIX username:
```

**Choisissez un nom d'utilisateur Linux** :
- En minuscules uniquement.
- Sans espaces ni caractères spéciaux.
- Court (ex: `julien`, `pmarc`, `dev`).
- Ce nom est **indépendant** de votre nom Windows.

Tapez votre choix, appuyez sur Entrée.

```
New password:
```

**Choisissez un mot de passe Linux**. Important :
- Il sera demandé à chaque commande `sudo` (commande administrateur Linux).
- **Pendant la saisie, rien ne s'affiche à l'écran** (ni étoiles, ni points). C'est normal, c'est une sécurité Unix. Tapez à l'aveugle.
- Confirmez en le retapant.

**Output attendu après confirmation** :

```
Installation successful!
Welcome to Ubuntu 24.04 LTS (GNU/Linux ...)
...
votre-nom@nom-machine:~$
```

Cette dernière ligne (`votre-nom@nom-machine:~$`) est votre **prompt Ubuntu**. Vous êtes maintenant dans Linux. Le `$` indique que vous êtes utilisateur normal (pas root).

### Étape 6 — Première vérification

Dans votre terminal Ubuntu, tapez :

```bash
whoami
```

**Output attendu** : votre nom d'utilisateur Linux.

Puis :

```bash
lsb_release -a
```

**Output attendu** :

```
Distributor ID: Ubuntu
Description:    Ubuntu 24.04 LTS
Release:        24.04
Codename:       noble
```

Si vous obtenez ces deux résultats, **WSL2 et Ubuntu sont opérationnels**.

### Étape 7 — Mettre à jour le système Ubuntu

Bonne pratique systématique sur Linux après installation. Tapez :

```bash
sudo apt update && sudo apt upgrade -y
```

**Décomposition de la commande** :
- `sudo` : exécute la commande avec les privilèges administrateur (équivalent de "Exécuter en tant qu'admin"). Le mot de passe Linux vous sera demandé.
- `apt update` : rafraîchit la liste des paquets disponibles.
- `&&` : enchaîne deux commandes (la seconde ne s'exécute que si la première réussit).
- `apt upgrade -y` : installe les mises à jour. Le `-y` répond automatiquement "oui" à toutes les confirmations.

**Output attendu** : une cascade de lignes de téléchargement et d'installation, puis un retour au prompt. Durée : 2 à 10 minutes selon les mises à jour disponibles.

### Étape 8 — Vérifier que vous êtes bien sur WSL2 (et non WSL1)

WSL existe en deux versions. WSL2 est **bien plus performant** et c'est ce qui doit être installé par défaut sur Windows 11. Vérification depuis PowerShell (pas Ubuntu) :

```powershell
wsl --list --verbose
```

**Output attendu** :

```
  NAME      STATE           VERSION
* Ubuntu    Running         2
```

La colonne `VERSION` doit indiquer **2**. Si elle indique 1, exécutez `wsl --set-version Ubuntu 2` et patientez.

### Points de vigilance critiques

> ⚠️ **Ne stockez jamais vos projets de code dans `/mnt/c/`**. Ce dossier représente votre disque Windows vu depuis Linux. Les performances Git y sont **catastrophiques** (10 à 50 fois plus lentes). Tous vos projets doivent vivre dans votre dossier Ubuntu, par exemple `/home/votre-nom/projets/`.

> ⚠️ **N'éditez jamais les fichiers Linux directement depuis l'Explorateur Windows** en allant dans `\\wsl$\Ubuntu\...`. Cela peut corrompre les permissions Unix. Utilisez toujours VS Code (qu'on configurera au prochain chapitre) ou le terminal Ubuntu.

> ⚠️ **Sauvegarde** : votre dossier Ubuntu est un fichier virtuel. Si vous désinstallez WSL ou Ubuntu, **tout le contenu est perdu**. Quand on aura GitHub configuré, vos projets seront sauvegardés en ligne automatiquement. En attendant, ne stockez rien d'irremplaçable dans Ubuntu.

### Vérification de maîtrise

Vous savez faire les choses suivantes sans aide ?

1. Ouvrir un terminal Ubuntu depuis Windows.
2. Connaître votre nom d'utilisateur Linux et votre mot de passe.
3. Exécuter une commande `sudo` et savoir qu'il faut taper le mot de passe à l'aveugle.
4. Vérifier la version de WSL depuis PowerShell.
5. Expliquer pourquoi on ne stocke pas les projets dans `/mnt/c/`.

Si oui, on passe au chapitre 0.3 : installer VS Code et le connecter à WSL.

---

## Chapitre 0.3 — Installer VS Code et le connecter à WSL2

**Objectif d'apprentissage**
À la fin de ce chapitre, vous aurez VS Code installé sur Windows, configuré pour se connecter de manière transparente à votre environnement Ubuntu/WSL2, et vous saurez ouvrir un dossier Linux dans VS Code et y exécuter des commandes via le terminal intégré.

**Durée estimée** : 20 à 30 minutes.

**Prérequis** : Chapitre 0.2 terminé (WSL2 + Ubuntu fonctionnels).

---

### Modèle mental

Il existe plusieurs façons de combiner VS Code et WSL, et la confusion est fréquente. Voici le bon modèle :

- **VS Code s'installe sur Windows** (pas dans Ubuntu). C'est une application Windows classique.
- VS Code dispose d'une **extension officielle Microsoft appelée "WSL"** qui lui permet de se connecter à Ubuntu et d'y exécuter ses opérations (édition de fichiers, terminal, débogueur) **comme si VS Code tournait nativement dans Linux**.
- Vous gardez donc **un seul VS Code** sur votre machine, mais il peut basculer entre deux modes :
  - Mode **local Windows** (rarement utilisé pour vous : éditer des fichiers Windows classiques)
  - Mode **WSL** (votre mode de travail principal : éditer dans Ubuntu)

Visuellement, quand VS Code est connecté à WSL, vous voyez en bas à gauche de la fenêtre une étiquette verte ou bleue indiquant `WSL: Ubuntu`. C'est votre repère.

```
┌──────────────────────────────────────────────────────┐
│  VS Code (Windows)                                    │
│                                                       │
│  Fichiers édités ───► transmis à ───► Ubuntu (WSL2)  │
│  Terminal intégré ──► exécute dans ──► Ubuntu        │
│                                                       │
│  [≡ WSL: Ubuntu]  ← indicateur en bas à gauche       │
└──────────────────────────────────────────────────────┘
```

### Étape 1 — Télécharger et installer VS Code

1. Ouvrez votre navigateur (Chrome ou Firefox) sur Windows.
2. Allez sur l'adresse : `https://code.visualstudio.com/`
3. Cliquez sur le bouton **Download for Windows** (téléchargement automatique de la version stable, x64, User Installer).
4. Une fois le fichier `.exe` téléchargé, double-cliquez dessus pour lancer l'installation.
5. Acceptez les conditions de licence.
6. **Important** : sur l'écran "Tâches supplémentaires", **cochez les cases suivantes** :
   - ☑ Ajouter une action "Ouvrir avec Code" au menu contextuel des fichiers de l'Explorateur Windows
   - ☑ Ajouter une action "Ouvrir avec Code" au menu contextuel des dossiers de l'Explorateur Windows
   - ☑ Inscrire Code comme éditeur pour les types de fichiers pris en charge
   - ☑ Ajouter à PATH (recommandé)
7. Cliquez sur Suivant → Installer → Terminer.

**Vérification** : VS Code s'ouvre automatiquement à la fin de l'installation. Vous voyez la page d'accueil "Get Started".

> ⚠️ **Vigilance** : si vous travaillez sur une machine professionnelle gérée par une DSI, certaines de ces options peuvent être verrouillées. Ce n'est pas bloquant pour la suite, mais signalez-le moi si vous rencontrez un problème.

### Étape 2 — Installer l'extension officielle WSL

L'extension WSL est ce qui permet la connexion transparente entre VS Code et Ubuntu.

1. Dans VS Code, regardez la barre verticale d'icônes à gauche (la "Activity Bar").
2. Cliquez sur l'icône **Extensions** (elle ressemble à 4 carrés, le quatrième détaché). Raccourci clavier : `Ctrl+Shift+X`.
3. Dans la barre de recherche en haut, tapez exactement : `WSL`
4. Le premier résultat doit être :

   ```
   WSL
   Microsoft
   ```

   Vérifiez bien que l'éditeur est **Microsoft** (l'extension officielle, pas une copie tierce).

5. Cliquez sur le bouton **Install**.
6. L'installation prend quelques secondes. Le bouton se transforme en "Disable" et "Uninstall" quand c'est terminé.

### Étape 3 — Première connexion à WSL depuis VS Code

Maintenant, connectons VS Code à Ubuntu.

1. Dans VS Code, regardez en **bas à gauche** de la fenêtre : vous voyez une petite icône bleue ou verte qui ressemble à `><` (deux chevrons opposés). C'est le bouton "Open a Remote Window".
2. Cliquez dessus.
3. Une palette de commandes s'ouvre en haut de l'écran. Choisissez :

   ```
   Connect to WSL
   ```

4. VS Code se reconfigure (1 à 2 minutes la première fois : il installe un petit serveur dans Ubuntu pour communiquer avec lui).
5. Une nouvelle fenêtre VS Code s'ouvre. **Vérifiez en bas à gauche** : vous devez voir l'étiquette `WSL: Ubuntu` (en vert ou bleu selon votre thème).

**Output attendu** : VS Code affiche un écran d'accueil légèrement différent, et l'étiquette `WSL: Ubuntu` est visible en bas à gauche. Vous êtes maintenant connecté à Ubuntu via VS Code.

### Étape 4 — Ouvrir le terminal intégré (côté Ubuntu)

C'est ici que la magie opère. Le terminal de VS Code, quand vous êtes en mode WSL, est un **vrai terminal Ubuntu**.

1. Dans VS Code (mode WSL), ouvrez le menu **Terminal** en haut → **New Terminal**. Raccourci clavier : `` Ctrl+` `` (la touche backtick).
2. Un panneau de terminal s'ouvre en bas de la fenêtre.
3. Le prompt qui s'affiche est exactement le même que dans votre fenêtre Ubuntu standalone :

   ```
   votre-nom@nom-machine:~$
   ```

4. Vérification rapide. Tapez :

   ```bash
   pwd
   ```

   **Output attendu** :

   ```
   /home/votre-nom
   ```

   `pwd` signifie "print working directory" — ça affiche le dossier dans lequel vous êtes. Vous êtes dans votre dossier personnel Ubuntu (`/home/votre-nom`), confirmation que vous travaillez bien dans Linux.

### Étape 5 — Créer un dossier de travail et l'ouvrir dans VS Code

Préparons votre dossier de projets, où vivront tous vos repos Git.

1. Dans le terminal VS Code (mode WSL), tapez :

   ```bash
   mkdir -p ~/projets
   ```

   **Décomposition** :
   - `mkdir` : "make directory", crée un dossier.
   - `-p` : "parent", crée les dossiers parents si nécessaire et ne renvoie pas d'erreur si le dossier existe déjà.
   - `~` : raccourci Linux qui signifie "mon dossier personnel" (équivalent de `/home/votre-nom`).
   - `~/projets` : crée le dossier `projets` dans votre dossier personnel.

2. Vérifiez :

   ```bash
   ls ~
   ```

   **Output attendu** : la liste des dossiers et fichiers de votre home, dont `projets`.

3. Maintenant, ouvrez ce dossier dans VS Code. Toujours dans le terminal :

   ```bash
   code ~/projets
   ```

   **Ce qui se passe** : la commande `code` (installée automatiquement par l'extension WSL) ouvre VS Code dans le dossier indiqué. Une nouvelle fenêtre VS Code s'ouvre, déjà en mode WSL, avec le dossier `projets` chargé dans l'explorateur de gauche.

> ⚠️ **Vigilance** : si la commande `code` ne fonctionne pas (`command not found`), c'est que l'extension WSL n'a pas terminé son setup. Fermez tout VS Code et rouvrez-le ; reconnectez à WSL via le bouton en bas à gauche ; réessayez. Cela suffit dans 99% des cas.

### Étape 6 — Tester le cycle complet

Faisons un test simple pour vérifier que tout fonctionne ensemble.

1. Dans VS Code (mode WSL, dossier `projets` ouvert), créez un fichier de test. Cliquez avec le bouton droit dans l'explorateur de gauche → **New File**.
2. Nommez-le `test.md`.
3. Tapez dans le fichier :

   ```markdown
   # Test
   Mon premier fichier édité dans VS Code via WSL2.
   ```

4. Sauvegardez avec `Ctrl+S`.
5. Dans le terminal VS Code, tapez :

   ```bash
   ls ~/projets
   ```

   **Output attendu** : `test.md`

6. Affichez le contenu :

   ```bash
   cat ~/projets/test.md
   ```

   **Output attendu** : exactement ce que vous avez tapé dans VS Code.

Si tout cela fonctionne, **vous avez un environnement de travail opérationnel** : VS Code édite, le terminal Ubuntu exécute, les fichiers vivent dans Linux.

### Points de vigilance

> ⚠️ **Toujours vérifier l'étiquette en bas à gauche.** Si elle indique simplement le nom de votre projet sans `WSL: Ubuntu`, vous êtes en mode Windows local. Vos opérations Git ne se feront pas au bon endroit. Reconnectez-vous via le bouton `><`.

> ⚠️ **Ne jamais utiliser le chemin `\\wsl$\Ubuntu\home\...` depuis VS Code.** Toujours passer par la commande `code ~/dossier` depuis le terminal Ubuntu, ou par "Open Folder" en mode WSL.

> ⚠️ **Une seule fenêtre VS Code par projet.** Évitez d'ouvrir plusieurs fenêtres simultanées sur le même projet : risque de confusion avec les sauvegardes.

### Vérification de maîtrise

Avant de passer au chapitre suivant, vous devez savoir faire ces actions seul :

1. Ouvrir VS Code en mode WSL (étiquette `WSL: Ubuntu` visible).
2. Ouvrir le terminal intégré et constater que c'est un terminal Ubuntu.
3. Créer un dossier dans `~/projets/` via la commande `mkdir`.
4. Ouvrir ce dossier dans VS Code via la commande `code`.
5. Créer un fichier dans VS Code et le retrouver via `ls` dans le terminal.

Si tout est validé, on installe Git.

---

## Chapitre 0.4 — Installer Git et configurer votre identité

**Objectif d'apprentissage**
À la fin de ce chapitre, Git sera installé dans votre environnement Ubuntu, configuré avec votre identité (nom et email) et avec quelques réglages essentiels qui vous éviteront des heures de débogage plus tard. Vous saurez vérifier votre configuration et la modifier si besoin.

**Durée estimée** : 15 à 20 minutes.

**Prérequis** : Chapitres 0.1, 0.2 et 0.3 terminés.

---

### Modèle mental

Trois choses à comprendre avant d'installer.

**1. Git va probablement déjà être installé.**
Ubuntu inclut souvent une version de Git préinstallée. On va le vérifier et, si nécessaire, le mettre à jour vers la version la plus récente.

**2. Git a besoin de savoir qui vous êtes.**
Chaque commit (chaque enregistrement que vous ferez) est signé par un nom et un email. Ce n'est **pas une authentification** (rien n'est vérifié à ce stade), c'est une simple étiquette descriptive qui restera attachée à chacun de vos commits, à vie. **Le choix du nom et de l'email est donc important** car ils seront publics dès que vous pousserez sur GitHub.

**3. Git a deux niveaux de configuration.**
- **Configuration globale** (`--global`) : valable pour tous vos projets sur cette machine. C'est celle qu'on va définir ici.
- **Configuration locale** (par projet) : surcharge la globale pour un projet spécifique. Utile si vous contribuez à un projet open source avec un email différent.

### Étape 1 — Vérifier si Git est déjà installé

Dans le terminal de VS Code (mode WSL), tapez :

```bash
git --version
```

**Trois cas possibles** :

**Cas A — Git est installé et récent** (version 2.40 ou supérieure) :

```
git version 2.43.0
```

Dans ce cas, vous pouvez passer directement à l'étape 3, mais je vous recommande quand même de faire l'étape 2 pour être sûr d'avoir la dernière version stable.

**Cas B — Git est installé mais ancien** (version inférieure à 2.40) :

```
git version 2.34.1
```

Faites l'étape 2 pour mettre à jour.

**Cas C — Git n'est pas installé** :

```
git: command not found
```

Faites l'étape 2 pour installer.

### Étape 2 — Installer ou mettre à jour Git

Dans le terminal Ubuntu, tapez successivement :

```bash
sudo apt update
```

Entrez votre mot de passe Linux (rappel : la saisie est invisible). Cette commande rafraîchit la liste des paquets disponibles.

Puis :

```bash
sudo apt install git -y
```

**Décomposition** :
- `apt install` : installe un paquet.
- `git` : le nom du paquet.
- `-y` : répond automatiquement "oui" aux confirmations.

**Output attendu** (extrait) :

```
Reading package lists... Done
Building dependency tree... Done
The following NEW packages will be installed:
  git ...
...
Setting up git ... 
```

Si Git était déjà installé, la commande mettra à jour vers la version la plus récente disponible dans les dépôts Ubuntu.

**Vérification** :

```bash
git --version
```

**Output attendu** : `git version 2.4x.x` (ou supérieur).

> ⚠️ **Vigilance** : pour avoir la toute dernière version de Git (parfois plus récente que celle d'Ubuntu), il existe un dépôt PPA officiel. Pour ce guide, **la version standard d'Ubuntu suffit largement**. Ne complexifiez pas inutilement votre installation.

### Étape 3 — Configurer votre identité Git

C'est le moment important. Choisissez bien votre nom et votre email.

**Recommandations** :
- **Nom** : votre vrai nom et prénom, en clair (`Jean Dupont`). Pas de pseudo cryptique, sauf si vous voulez rester anonyme sur vos contributions publiques.
- **Email** : utilisez **le même email que celui qui sera associé à votre compte GitHub**. C'est crucial : GitHub fait le lien entre vos commits et votre profil grâce à cet email. Si les deux ne correspondent pas, vos commits apparaîtront comme "anonymes" sur GitHub.

> ⚠️ **Considération vie privée** : l'email que vous configurez ici sera **public** dans l'historique de tous les repos publics où vous contribuerez. Si vous voulez le masquer, GitHub propose un **email "noreply"** de la forme `12345678+votre-pseudo@users.noreply.github.com` qu'on configurera lors de la création du compte GitHub (Chapitre 2.1). Pour l'instant, utilisez votre email normal ; on pourra le changer plus tard sans difficulté.

Dans le terminal Ubuntu, tapez (en remplaçant par vos vraies informations) :

```bash
git config --global user.name "Jean Dupont"
```

```bash
git config --global user.email "jean.dupont@exemple.com"
```

**Décomposition** :
- `git config` : commande de configuration de Git.
- `--global` : applique à tous les projets de cette machine.
- `user.name` et `user.email` : les deux clés de configuration de l'identité.
- Les guillemets entourent la valeur (obligatoires si elle contient des espaces).

**Aucun output n'est retourné**. C'est normal. Pas de message = succès.

### Étape 4 — Configurer la branche par défaut

Depuis 2020, la convention sur GitHub et la plupart des projets est que la branche principale s'appelle **`main`** (et non plus `master`, terme historique abandonné pour des raisons sociétales). Configurons Git pour qu'il crée par défaut une branche `main` à chaque nouveau projet :

```bash
git config --global init.defaultBranch main
```

### Étape 5 — Configurer l'éditeur par défaut

Quand Git a besoin que vous tapiez un message long (par exemple un message de commit détaillé, ou pendant un rebase interactif), il ouvre un éditeur. Par défaut, sur Ubuntu, c'est `nano` ou `vim`. Pour un débutant, **autant utiliser VS Code**, que vous connaissez déjà :

```bash
git config --global core.editor "code --wait"
```

**Décomposition** :
- `core.editor` : la clé qui définit l'éditeur de Git.
- `code --wait` : la commande qui ouvre VS Code et attend que vous fermiez le fichier pour rendre la main à Git.

> ⚠️ **Vigilance** : cette configuration ne fonctionne **que parce que vous êtes en mode WSL via VS Code**. La commande `code` est disponible dans le terminal grâce à l'extension WSL installée au Chapitre 0.3. Si vous n'avez pas suivi le Chapitre 0.3, cette commande échouera.

### Étape 6 — Configurer le comportement des fins de ligne

Détail technique mais important. Linux et Windows codent les fins de ligne différemment (LF pour Linux, CRLF pour Windows). Comme vous travaillez en WSL (Linux) mais que vos fichiers peuvent être lus sur des machines Windows, on configure Git pour **stocker les fichiers en LF** (norme Linux/Unix, standard sur les serveurs) :

```bash
git config --global core.autocrlf input
```

**Signification** : `input` = "à la sauvegarde, convertir CRLF vers LF si nécessaire ; à l'extraction, ne rien convertir". C'est la valeur recommandée pour Linux/Mac/WSL.

> ⚠️ **Si vous travailliez en Git natif sous Windows** (ce que nous avons écarté), la valeur recommandée serait `core.autocrlf true`. Différence subtile mais piégeuse — c'est l'une des raisons pour lesquelles on a choisi WSL2.

### Étape 7 — Configurer le pull par défaut en mode "fast-forward only"

C'est un réglage qui évite des merges parasites involontaires lors des `git pull`. On reverra le concept en détail Chapitre 2.4. Pour l'instant, copiez :

```bash
git config --global pull.ff only
```

Si plus tard vous voulez revenir au comportement par défaut (merge automatique), il suffira de changer cette valeur.

### Étape 8 — Vérifier la configuration complète

Affichez toute votre configuration globale d'un coup :

```bash
git config --global --list
```

**Output attendu** :

```
user.name=Jean Dupont
user.email=jean.dupont@exemple.com
init.defaultbranch=main
core.editor=code --wait
core.autocrlf=input
pull.ff=only
```

Si vous voyez ces six lignes (avec **vos** nom et email), la configuration est complète.

### Étape 9 — Localiser le fichier de configuration

Pour information : toute cette configuration est stockée dans un fichier texte caché dans votre home. Vous pouvez le voir :

```bash
cat ~/.gitconfig
```

**Output attendu** :

```ini
[user]
        name = Jean Dupont
        email = jean.dupont@exemple.com
[init]
        defaultBranch = main
[core]
        editor = code --wait
        autocrlf = input
[pull]
        ff = only
```

Ce fichier est éditable directement avec un éditeur de texte. C'est utile à savoir : si un jour vous voulez tout réinitialiser, vous pouvez supprimer ce fichier et recommencer.

### Points de vigilance

> ⚠️ **L'email Git doit correspondre à un email GitHub vérifié** pour que vos commits soient associés à votre profil. On configurera GitHub au Chapitre 2.1 avec l'email choisi ici.

> ⚠️ **Ne configurez `user.name` et `user.email` qu'avec des informations que vous assumez d'être publiques.** Tout commit poussé sur un repo public est consultable par n'importe qui dans le monde, y compris l'email associé.

> ⚠️ **La configuration est par machine**. Si vous changez d'ordinateur ou réinstallez Ubuntu, il faudra refaire ces 6 commandes. C'est l'occasion de noter votre `.gitconfig` quelque part (ou de le versionner sur GitHub plus tard, dans un repo "dotfiles").

### Vérification de maîtrise

Avant de passer à la Partie 1, vous devez savoir :

1. Vérifier la version de Git installée (`git --version`).
2. Afficher votre configuration globale (`git config --global --list`).
3. Modifier votre nom ou email Git (en relançant la commande `git config --global user.name "..."`).
4. Expliquer pourquoi l'email Git doit correspondre à l'email GitHub.
5. Localiser le fichier `~/.gitconfig`.

Si tout est validé : **votre environnement de travail est opérationnel**. Vous avez Windows 11 + WSL2/Ubuntu + VS Code connecté à WSL + Git configuré avec votre identité. Vous êtes prêt à entrer dans le cœur du sujet : faire vos premiers commits Git en local.

---

## Récapitulatif de la Partie 0

À ce stade du guide, vous avez :

- Compris l'architecture Git/GitHub/VS Code/WSL et leur articulation.
- Installé WSL2 avec Ubuntu 24.04 sur Windows 11.
- Installé VS Code et configuré l'extension WSL.
- Installé et configuré Git (identité, branche par défaut, éditeur, fins de ligne).
- Un dossier `~/projets/` prêt à accueillir vos repos.

**Temps total réel** : 1h30 à 2h selon votre rythme et la qualité de votre connexion Internet.

**Prochaine livraison** : Partie 1 — Git en local sans GitHub (7 chapitres, environ 3-4h de travail).
