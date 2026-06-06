# **LabelSync Pro**

``` Aperçu des labels ```

![Capture d'écran LabelSync Pro 1](screenshot/label_1.png)

---

# 🇫🇷 Version Française

## Description

`labelsync-pro` est un outil d'automatisation GitHub qui synchronise un ensemble standardisé d'étiquettes (labels) sur vos repositories GitHub. Il est conçu pour maintenir une cohérence visuelle et fonctionnelle à travers tous vos projets en appliquant automatiquement un ensemble prédéfini d'étiquettes au dernier repository créé.

## Fonctionnalités

- **Synchronisation automatique des étiquettes** : Applique un ensemble standardisé d'étiquettes sur **tous** vos repositories non-archivés (mode `all`) ou uniquement sur le dernier créé (mode `latest`).
- **Exécution programmée** : S'exécute automatiquement chaque jour à **19h15 UTC** (= 20h15 FR hiver / 21h15 FR été) ou peut être déclenchée manuellement.
- **Parallélisation** : Traite jusqu'à 5 repositories en parallèle via `matrix`, avec `fail-fast: false`.
- **Mode dry-run** : Simulez les changements sans les appliquer (`workflow_dispatch` → `dry_run: true`).
- **Opt-out par repo** : Excluez un repository soit en lui ajoutant le **topic GitHub `no-labelsync`**, soit en le listant dans `.github/config/exclude.json`.
- **Réutilisable** : Appelable comme *reusable workflow* depuis n'importe quel autre repo (`workflow_call`).
- **Validation** : `labels.json` et `exclude.json` sont validés contre un JSON Schema sur chaque PR (workflow `lint.yml`).
- **Personnalisation facile** : Configuration via un fichier JSON pour définir vos propres étiquettes.

## Étiquettes prédéfinies

Le workflow inclut un ensemble complet d'étiquettes prédéfinies pour différents types de contributions :

- 🛠️ Chore : Maintenance ou tâches techniques
- ✨ Feature : Nouvelle fonctionnalité ou amélioration
- 🐛 Fix : Correction de bugs ou d'erreurs
- 🚑 Hotfix : Corrections urgentes en production
- ♻️ Refactor : Réorganisation ou optimisation du code
- 🚀 Release : Livraison d'une version stable
- 📦 Update : Mises à jour ou modifications générales
- ⚙️ CI/CD : Changements liés à l'intégration continue et au déploiement
- Et bien d'autres...

## Installation

1. **Cloner ce repository** :
   ```bash
   git clone https://github.com/j-ned/labelsync-pro.git
   ```

2. **Configurer le token GitHub** :
   - **Recommandé** : créez un *fine-grained PAT* avec accès à tous vos repos et les permissions `Metadata: Read` + `Issues: Read & Write` (suffisant pour gérer les labels).
   - Alternative : un *classic PAT* avec scope `repo` complet (plus de droits que nécessaire).
   - Ajoutez ce token comme secret dans votre repository sous le nom `LABELGITHUB_TOKEN`.

3. **Personnaliser les étiquettes (optionnel)** :
   - Modifiez le fichier `.github/config/labels.json` selon vos besoins.
   - Ajoutez les repos à exclure dans `.github/config/exclude.json` (format `owner/name`).

## Utilisation

### Exécution automatique

Le workflow s'exécute automatiquement chaque jour à **19h15 UTC** pour synchroniser les étiquettes sur **tous vos repositories non-archivés** (hors exclusions).

### Exécution manuelle

Vous pouvez également déclencher le workflow manuellement :

1. Accédez à l'onglet "Actions" de votre repository GitHub
2. Sélectionnez le workflow "Synchronisation des Labels"
3. Cliquez sur "Run workflow" et choisissez :
   - **mode** : `all` (tous les repos) ou `latest` (uniquement le dernier créé)
   - **dry_run** : `true` pour simuler sans appliquer

### Réutilisable depuis un autre repository

Ce workflow peut être appelé depuis n'importe quel repo via `workflow_call` :

```yaml
jobs:
  labels:
    uses: j-ned/labelsync-pro/.github/workflows/manage-labels.yml@master
    with:
      mode: all
      dry_run: false
      labels_path: .github/config/labels.json   # chemin dans VOTRE repo
    secrets:
      LABELGITHUB_TOKEN: ${{ secrets.LABELGITHUB_TOKEN }}
```

> 💡 Pour la stabilité, pinnez sur un tag ou un SHA plutôt que sur `@master`.

### Exclure un repository

Deux méthodes :

- **Topic GitHub** : ajoutez le topic `no-labelsync` à votre repo (Settings → Topics).
- **Fichier d'exclusion** : ajoutez le `owner/name` dans `.github/config/exclude.json`.

## Configuration

Vous pouvez personnaliser les étiquettes en modifiant le fichier `.github/config/labels.json`. Chaque étiquette est définie avec les propriétés suivantes :

```json
{
  "name": "Nom de l'étiquette",
  "description": "Description de l'étiquette",
  "color": "code couleur hexadécimal sans #"
}
```

## Exemple de configuration

```json
[
  { "name": "🐛 Bug", "description": "Quelque chose ne fonctionne pas", "color": "d73a4a" },
  { "name": "📚 Documentation", "description": "Améliorations ou ajouts à la documentation", "color": "0075ca" }
]
```

## Contribution

Les contributions sont les bienvenues ! N'hésitez pas à ouvrir une issue ou à soumettre une pull request pour améliorer ce projet.

## Licence

Ce projet est libre d'utilisation.

---

# 🇺🇸 English Version

## Description

`labelsync-pro` is a GitHub automation tool that synchronizes a standardized set of labels across your GitHub repositories. It's designed to maintain visual and functional consistency across all your projects by automatically applying a predefined set of labels to your most recently created repository.

## Features

- **Automatic label synchronization**: Applies a standardized set of labels to **all** your non-archived repositories (`all` mode) or only the most recently created one (`latest` mode).
- **Scheduled execution**: Runs automatically every day at **19:15 UTC** or can be triggered manually.
- **Parallel execution**: Processes up to 5 repositories in parallel via `matrix`, with `fail-fast: false`.
- **Dry-run mode**: Simulate changes without applying them (`workflow_dispatch` → `dry_run: true`).
- **Per-repo opt-out**: Skip a repository either by adding the **`no-labelsync` GitHub topic** or by listing it in `.github/config/exclude.json`.
- **Reusable**: Callable as a *reusable workflow* from any other repo (`workflow_call`).
- **Validation**: `labels.json` and `exclude.json` are validated against a JSON Schema on every PR (`lint.yml` workflow).
- **Easy customization**: Simple JSON-based configuration.

## Predefined Labels

The workflow includes a comprehensive set of predefined labels for different types of contributions:

- 🛠️ Chore: Maintenance or technical tasks
- ✨ Feature: New functionality or enhancement
- 🐛 Fix: Bug fixes or error corrections
- 🚑 Hotfix: Urgent production fixes
- ♻️ Refactor: Code reorganization or optimization
- 🚀 Release: Stable version delivery
- 📦 Update: Updates or general modifications
- ⚙️ CI/CD: Changes related to continuous integration and deployment
- And many more...

## Installation

1. **Clone this repository**:
   ```bash
   git clone https://github.com/j-ned/labelsync-pro.git
   ```

2. **Configure GitHub token**:
   - **Recommended**: create a *fine-grained PAT* with access to all your repos and the `Metadata: Read` + `Issues: Read & Write` permissions (sufficient for label management).
   - Alternative: a *classic PAT* with full `repo` scope (more permissions than needed).
   - Add this token as a secret in your repository under the name `LABELGITHUB_TOKEN`.

3. **Customize labels (optional)**:
   - Modify the `.github/config/labels.json` file according to your needs.
   - Add repos to skip in `.github/config/exclude.json` (`owner/name` format).

## Usage

### Automatic execution

The workflow runs automatically every day at **19:15 UTC** to synchronize labels on **all your non-archived repositories** (excluding skipped ones).

### Manual execution

You can also trigger the workflow manually:

1. Go to the "Actions" tab of your GitHub repository
2. Select the "Synchronisation des Labels" workflow
3. Click "Run workflow" and choose:
   - **mode**: `all` (every repo) or `latest` (only the most recently created)
   - **dry_run**: `true` to simulate without applying

### Reusable from another repository

This workflow can be called from any repo via `workflow_call`:

```yaml
jobs:
  labels:
    uses: j-ned/labelsync-pro/.github/workflows/manage-labels.yml@master
    with:
      mode: all
      dry_run: false
      labels_path: .github/config/labels.json   # path inside YOUR repo
    secrets:
      LABELGITHUB_TOKEN: ${{ secrets.LABELGITHUB_TOKEN }}
```

> 💡 For stability, pin to a tag or SHA rather than `@master`.

### Skipping a repository

Two options:

- **GitHub topic**: add the `no-labelsync` topic to your repo (Settings → Topics).
- **Exclusion file**: add `owner/name` to `.github/config/exclude.json`.

## Configuration

You can customize labels by modifying the `.github/config/labels.json` file. Each label is defined with the following properties:

```json
{
  "name": "Label name",
  "description": "Label description",
  "color": "hexadecimal color code without #"
}
```

## Configuration Example

```json
[
  { "name": "🐛 Bug", "description": "Something isn't working", "color": "d73a4a" },
  { "name": "📚 Documentation", "description": "Improvements or additions to documentation", "color": "0075ca" }
]
```

## Contributing

Contributions are welcome! Feel free to open an issue or submit a pull request to improve this project.

## License

This project is free to use.
