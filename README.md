# CYCLOPE 2 — Détection de feux et passages piétons

Projet académique de vision par ordinateur pour détecter les feux de circulation et les passages piétons, afin d’assister les personnes malvoyantes. Le dépôt regroupe des scripts Python d’entraînement YOLOv11, d’augmentation de données, de réglage et de prédiction.

## Contenu

- Entraînement et comparaison de variantes YOLOv11 avec PyTorch et Ultralytics.
- Scripts d’augmentation et de préparation des données.
- Configuration de datasets et de classes au format YAML.
- Export du modèle entraîné au format ONNX.

Les six classes déclarées dans la configuration principale sont `greenlight`, `greenlight_car`, `redlight`, `redlight_car`, `yellowlight_car` et `zebracrossing`.

## Structure

- `Codespython/` : scripts d’entraînement, d’augmentation, de tuning, d’annotation et de prédiction.
- `yamlFiles/` : configurations de datasets et variantes d’expériences.
- `comparatif_complet_toutes_metrics*.xlsx` : tableaux de comparaison des expériences.
- `powerbat.bat` : script Windows appelé à la fin de certains entraînements.

## Prérequis

- Python 3 et `pip`.
- PyTorch et Ultralytics YOLO.
- Un GPU CUDA est facultatif; l’entraînement peut aussi s’exécuter sur CPU, mais sera plus lent.
- Les poids `yolo11m.pt` et les images du dataset ne sont pas inclus dans ce dépôt.

Créer un environnement puis installer les dépendances principales :

```bash
python -m venv .venv
# Linux/macOS
source .venv/bin/activate
# Windows PowerShell : .venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install ultralytics torch
```

Pour un entraînement CUDA, installer la version de PyTorch compatible avec la version de CUDA de la machine en suivant les instructions officielles de PyTorch.

## Données et attribution

Le fichier `yamlFiles/data.yaml` référence des chemins externes (`../DatasetBase/...` et `../Dataset/...`). Placez les données localement et adaptez ces chemins à leur emplacement. Les images et les poids ne sont pas redistribués dans ce dépôt.

La configuration identifie le dataset Roboflow **traffic-hzr3j**, version 1, sous licence **CC BY 4.0** : [source du dataset](https://universe.roboflow.com/traffic-olffk/traffic-hzr3j/dataset/1). Respectez cette licence et conservez l’attribution lorsque vous réutilisez les données.

Le script `Codespython/train.py` attend le fichier `yolo11m.pt` à la racine du dépôt.

## Lancement de l’entraînement

Depuis la racine du dépôt, après avoir préparé le dataset et les poids :

```bash
python Codespython/train.py
```

Les sorties sont écrites dans `Dataset_Basev2/`, ignoré par Git.

> **Attention — extinction Windows :** `train.py` et d’autres scripts d’entraînement appellent `powerbat.bat`. Celui-ci programme l’arrêt du PC dans 300 secondes et attend une action clavier pour l’annuler. Ne lancez pas ces scripts sans surveillance. Cette étape est spécifique à Windows.

## Prédiction

`Codespython/prediction.py` contient des chemins Windows absolus (`P:\Videos\...`). Modifiez `source_images`, `model_path` et le dossier de sortie dans ce fichier avant de l’exécuter.

## Crédits

- Encadrement : Amine Khettat.
- Étudiants collaborateurs : Yannis Ngako, Wafeu Manuelle, Ali Makki, Alaa Nassar et Fatima Ismail.

## Limites

Les chemins des données et de prédiction dépendent de l’environnement local. Les versions des dépendances ne sont pas verrouillées et aucune métrique ne peut être reproduite sans récupérer les données, les poids et la configuration de l’expérience correspondante.
