# Documentation PHP et Symfony

[![Documentation Build](https://github.com/owner/repo/actions/workflows/deploy.yml/badge.svg)](https://github.com/owner/repo/actions/workflows/deploy.yml)

Ce dépôt contient la documentation générée avec MkDocs.

Le projet utilise le plugin **mkdocs-wikilinks-plugin** (via l'alias `ezlinks`)
pour permettre les liens de type `[[Nom-de-page]]` dans la documentation.

## Prérequis

- Python 3.11 ou supérieur
- `pip` pour installer les dépendances
- Optionnel : un environnement virtuel avec `python -m venv mkdocs-env`

## Installation

```bash
git clone https://github.com/owner/repo.git
cd repo
python -m venv mkdocs-env  # optionnel mais recommandé
source mkdocs-env/bin/activate
pip install -r requirements.txt
```

## Exécuter MkDocs en local

Une fois les dépendances installées, lancez :

```bash
mkdocs serve
```

La documentation sera alors disponible sur <http://127.0.0.1:8000> et se
rechargera automatiquement à chaque modification. Pour vérifier qu'il n'y a pas
d'erreurs, exécutez :

```bash
mkdocs build --strict
```

## Publication sur GitHub Pages

Le fichier [`deploy.yml`](deploy.yml) définit un workflow GitHub Actions lancé à
chaque push sur la branche `main` :

1. Installation des dépendances via `pip install -r requirements.txt`.
2. Construction du site avec `mkdocs build --strict`.
3. Déploiement vers la branche `gh-pages` à l'aide de `mkdocs gh-deploy --force`.

GitHub Pages sert ensuite le contenu de cette branche.

## Contribuer

Les contributions sont les bienvenues ! Pour proposer une amélioration :

1. Forkez ce dépôt et créez votre branche de travail.
2. Vérifiez localement que `mkdocs build --strict` s'exécute sans erreur.
3. Ouvrez une pull request décrivant vos changements.
