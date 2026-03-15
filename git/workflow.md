# Git - Workflow et commandes

## Workflow de base

```bash
git status                  # Voir les modifications
git add .                   # Stager tous les fichiers
git commit -m "message"     # Commiter
git push origin main        # Pousser vers le remote
git pull origin main        # Recuperer les changements
```

## Branches

```bash
git branch                      # Lister les branches locales
git branch -a                   # Lister toutes les branches (local + remote)
git checkout -b feature/name    # Creer et basculer sur une branche
git checkout main               # Retourner sur main
git merge feature/name          # Merger une branche dans la courante
git branch -d feature/name      # Supprimer une branche mergee
```

## Resolution de conflits

```bash
# 1. Puller ou merger -> conflit detecte
git merge feature/name

# 2. Ouvrir les fichiers en conflit et resoudre manuellement
#    <<<<<<< HEAD
#    votre code
#    =======
#    leur code
#    >>>>>>> feature/name

# 3. Marquer comme resolu et commiter
git add .
git commit -m "fix: resolve merge conflict"
```

## Historique

```bash
git log --oneline           # Historique compact
git log --graph --oneline   # Avec visualisation des branches
git diff                    # Changements non stages
git diff --staged           # Changements stages
git blame file.txt          # Qui a modifie chaque ligne
```

## Annuler des changements

```bash
git checkout -- file.txt    # Annuler les modifs d'un fichier (non stage)
git reset HEAD file.txt     # Destagner un fichier
git revert <commit>         # Creer un commit qui annule un autre
git stash                   # Mettre de cote les modifs en cours
git stash pop               # Recuperer les modifs mises de cote
```

## Bonnes pratiques de commit

- Commencer par un verbe : `add`, `fix`, `update`, `remove`, `refactor`
- Message court (< 50 caracteres) pour le titre
- Utiliser des prefixes : `feat:`, `fix:`, `docs:`, `ci:`, `refactor:`
- Un commit = un changement logique
