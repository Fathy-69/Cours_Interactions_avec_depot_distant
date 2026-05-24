# 🚀 Aide-mémoire Git : Gérer son dépôt distant

Ce fichier rassemble les commandes essentielles et les solutions aux erreurs courantes rencontrées lors de la synchronisation avec GitHub.

---

## 📋 La Routine Globale (Quand tout va bien)

À chaque fois que tu as fini de travailler et que tu veux envoyer tes modifications :

```bash
# 1. Ajouter tous les fichiers modifiés au point de contrôle
git add .

# 2. Enregistrer les modifications avec un message descriptif
git commit -m "Mon message clair et concis"

# 3. Envoyer le travail sur GitHub
git push origin main




Voici la version finale condensée et structurée en un seul bloc Markdown pour ton fichier `AIDE_MEMOIRE_GIT.md`.

```markdown
# 🚀 Aide-mémoire Git : Gérer son dépôt distant

Ce guide contient les commandes essentielles et les solutions aux erreurs de synchronisation avec GitHub.

---

## 📋 1. Routine Quotidienne (Workflow)

Utilise ces étapes pour envoyer ton travail quand tout est en ordre :

```bash
# Ajouter tous les fichiers modifiés
git add .

# Enregistrer les modifications localement
git commit -m "Description de mon changement"

# Envoyer vers GitHub
git push origin main

```

---

## 🛠️ 2. Résolution des Problèmes Courants

### A. Erreur d'Authentification (Token)

**Problème :** GitHub refuse ton mot de passe habituel.
**Solution :**

1. Génère un **Token (PAT)** sur GitHub : *Settings > Developer settings > Personal access tokens*.
2. Coche la case **`repo`**.
3. Utilise ce token (`ghp_...`) à la place du mot de passe dans le terminal.

*Pour ne plus avoir à le taper :*

```bash
git config --global credential.helper store

```

### B. Erreur de Rejet (Pull before Push)

**Problème :** `[rejected] main -> main (fetch first)`. GitHub a des versions que tu n'as pas.
**Solution :**

```bash
# Configurer la fusion par défaut (une seule fois)
git config pull.rebase false

# Récupérer et fusionner les fichiers distants
git pull origin main --allow-unrelated-histories

```

### C. Conflit de Fusion (Conflict README.md)

**Problème :** `Automatic merge failed`. Le même fichier a été modifié des deux côtés.
**Solution :**

1. Ouvrir le fichier : `nano README.md`
2. Supprimer les balises `<<<<<<<`, `=======`, `>>>>>>>` et garder le texte voulu.
3. Valider la correction :

```bash
git add README.md
git commit -m "Fix: résolution du conflit"
git push origin main

## 💡 3. Astuces Terminal & Fichiers

| Action | Commande |
| --- | --- |
| **Modifier un fichier** | `nano nom_du_fichier.md` (Ctrl+O pour sauver, Ctrl+X pour quitter) |
| **Renommer un fichier** | `mv ancien_nom.md nouveau_nom.md` |
| **Voir l'état (status)** | `git status` |
| **Lister les fichiers** | `ls` |

---

*Guide de secours généré pour le projet RepoDistantTest.*

```

```