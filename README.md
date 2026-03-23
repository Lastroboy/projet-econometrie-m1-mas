# 🎗️ Analyse économétrique des facteurs de survie du cancer du sein

> Projet réalisé dans le cadre du Master 1 MAS — Université de Rennes  
> **Noa Delaporte · Sarah Scalart**

---

## 📋 Présentation du projet

Ce projet vise à quantifier, par une approche économétrique, l'influence des **facteurs cliniques, biologiques et socio-démographiques** sur la **durée de survie** (en mois) de patientes diagnostiquées avec un cancer du sein.

### Question de recherche

> *Quels sont les facteurs cliniques, socio-démographiques et biologiques qui influencent la durée de survie de la tumeur des patients atteints d'un cancer du sein ?*

---

## 📦 Données

| Caractéristique | Détail |
|---|---|
| **Source** | Programme SEER — National Cancer Institute (NCI), via Kaggle |
| **Période** | Diagnostics réalisés entre 2006 et 2010, mise à jour 2017 |
| **Échantillon** | 4 024 patientes anonymisées |
| **Couverture géographique** | États-Unis |

### Variables retenues (9 sur 16 initiales)

| Variable | Type | Description |
|---|---|---|
| `Age` | Quantitative | Âge au diagnostic (années) |
| `Marital.Status` | Catégorielle | État matrimonial (Marié, Célibataire, Divorcé, Veuve, Séparé) |
| `X6th.Stage` | Catégorielle | Stade clinique AJCC (IIA, IIB, IIIA, IIIB, IIIC) |
| `Grade` | Catégorielle ordinale | Degré d'agressivité tumorale (1 à 4) |
| `Tumor.Size` | Quantitative | Taille de la tumeur (mm) |
| `Estrogen.Status` | Binaire | Statut récepteurs œstrogènes (Positif / Négatif) |
| `Progesterone.Status` | Binaire | Statut récepteurs progestérone (Positif / Négatif) |
| `Reginol.Node.Positive` | Quantitative | Nombre de ganglions lymphatiques positifs |
| `Survival.Months` *(Y)* | Quantitative | **Variable dépendante** — durée de survie en mois |

> Les 7 variables écartées (`Race`, `A.Stage`, `T.Stage`, `N.Stage`, `Regional.Node.Examined`, `Status`, `Differentiate`) ont été supprimées pour cause de redondance, de multicolinéarité ou de non-pertinence clinique.

---

## 🔬 Méthodologie

### Modèle de référence

Le cadre retenu est celui de la **régression linéaire multiple** estimée par la méthode des **Moindres Carrés Ordinaires (MCO)** :

$$Y_i = \beta_0 + \beta_1 X_{1i} + \beta_2 X_{2i} + \cdots + \beta_k X_{ki} + \varepsilon_i$$

Chaque coefficient $\beta_k$ représente l'effet marginal d'une variable sur le nombre de mois de survie.

### Stratégie de comparaison de modèles

Quatre modèles ont été construits et mis en concurrence :

| Modèle | Description | Objectif |
|---|---|---|
| **Modèle A** | MCO linéaire complet | Estimation de l'effet marginal de tous les facteurs |
| **Modèle B** | Log-MCO | Transformation `log(Y)` pour corriger l'asymétrie et stabiliser la variance |
| **Modèle C** | MCO réduit (variables cliniques seules) | Référence pour évaluer l'apport des variables socio-démographiques |
| **Modèle D** ✅ | Log-MCO réduit | **Modèle retenu** — le plus efficient selon le R² ajusté et les tests statistiques |

### Étapes de validation

1. **Analyse descriptive** : histogrammes, boxplots, tableaux de fréquence (analyses univariées et bivariées)
2. **Estimation MCO** et comparaison des 4 modèles
3. **Tests de validation des hypothèses MCO** :
   - Test de Jarque-Bera (normalité des résidus)
   - Test de Breusch-Pagan (homoscédasticité)
   - Détection de la multicolinéarité (VIF)
4. **Correction de l'hétéroscédasticité** : estimation robuste **Huber-White (HC1)**
5. **Analyses complémentaires** :
   - Termes d'interaction (effets croisés entre variables)
   - Terme quadratique sur `Tumor.Size` pour capturer une relation non linéaire

---

## 📊 Résultats principaux

### Facteurs les plus significatifs (modèle D — Log-MCO robuste)

- **Statut Estrogène Négatif** : facteur le plus déterminant — réduit significativement et robustement l'espérance de survie (*p < 0.001*)
- **Statut Progestérone Négatif** : effet négatif significatif sur la survie (*p ≈ 0.02*)
- **Nombre de ganglions positifs (`Reginol.Node.Positive`)** : influence négative directe (*p ≈ 0.012*)
- **Taille de la tumeur (`Tumor.Size`)** : relation non strictement linéaire — l'impact s'accentue pour les tumeurs les plus volumineuses (confirmé par le terme quadratique)

### Effets d'interaction détectés

- L'envahissement ganglionnaire est **statistiquement plus grave** chez les patientes avec un statut estrogène négatif — les facteurs biologiques agissent comme **modulateurs** des facteurs cliniques.

### Limites du modèle

- **R² ajusté ~ 4,5%** : pouvoir explicatif modeste, révélateur de la complexité biologique du phénomène et de l'absence de variables importantes dans le jeu de données (traitements suivis, antécédents génétiques, mode de vie).

---

## 📁 Structure du dépôt

```
├── data/
│   └── Breast_Cancer.csv          # Données SEER (source : Kaggle)
├── projet_econometrie.Rmd         # Document R Markdown principal
├── projet_econometrie.html        # Rapport compilé
└── README.md
```

---

## 🛠️ Environnement technique

- **Langage** : R
- **Format** : R Markdown (`.Rmd`), rendu avec `rmdformats::robobook`
- **Packages principaux** : `ggplot2`, `gridExtra`, `lmtest`, `sandwich`, `car`

---

## 📚 Références

- **Données** : National Cancer Institute. (2017). *SEER Breast Cancer Data*.  
  → [Kaggle](https://www.kaggle.com/datasets/reihanenamdari/breast-cancer/data) · [Zenodo](https://zenodo.org/records/5120960)

- **Référence scientifique** : Donegan, W. L. (1992). *Prognostic factors: Stage and receptor status in breast cancer*. Cancer, 70(S4), 1755–1760.

---

## 👩‍🎓 Auteurs

**Noa Delaporte** & **Sarah Scalart** — Master 1 MAS, Université de Rennes

> *La rédaction de ce document a bénéficié, en partie, de l'aide d'une IA générative.*
