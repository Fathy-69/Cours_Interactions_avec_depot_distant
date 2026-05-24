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