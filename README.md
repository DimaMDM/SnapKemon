# SnapKemon
Application mobile permettant d'identifier des cartes Pokémon - Projet étudiant

Projet de fin d'année BUT Informatique — IUT de Metz (Promotion 2023-2026)

---

## Comment ça marche ?

1. **Détection** : YOLOv12 INT8 localise la carte dans le flux caméra en temps réel
2. **Redressement** : YOLOv8 Keypoint détecte les 4 coins et corrige l'inclinaison
3. **Identification** : Les descripteurs ORB de la carte sont comparés à la base locale
   via une approche hybride FLANN → BFMatcher
4. **Résultat** : Nom, série et prix de la carte via l'API Pokémon TCG

---

## Stack technique

| Domaine | Technologie |
|--------|-------------|
| Mobile | React Native 0.82 + TypeScript |
| Détection temps réel | YOLOv12 INT8 (TFLite) |
| Scan précis | YOLOv8 Keypoint (TFLite) |
| Identification | ORB + FLANN + BFMatcher |
| Base de données locale | Format .BYTES (20 000+ cartes FR/EN) |
| Données cartes | Pokémon TCG API V2 |
| CI/CD | GitLab |

---

## Performances

| Condition | Latence |
|-----------|---------|
| Conditions optimales | ~300ms |
| Appareil sous charge | ~700ms |

Précision de détection : **95,8%** minimum (classe la moins performante)

> La base locale utilise un nombre de points FLANN réduit pour limiter
> la taille de l'APK. Des faux négatifs occasionnels peuvent survenir.

---

## Architecture
```
src/
├── components/    # Composants React (caméra, UI)
├── services/      # YOLOv8, API Pokémon TCG, pipeline de détection
├── utils/         # Optimiseur, debug, parseurs
└── config/        # Configuration API
```

---

## Équipe

| Membre | Rôle |
|--------|------|
| Dimitri Makogon | Backend, pipeline CV, architecture |
| Pierre-Yves Legall | Frontend, UI/UX |
| Imrane Ouhemmi | Entraînement IA, dataset |
| Baptiste Perri | Qualité, rendus, intégration |

---

## Téléchargement

📦 [Télécharger l'APK](https://github.com/DimaMDM/SnapKemon/releases/tag/SnapKemon) — Compatible Android

---

## Installation (développeurs)
```sh
git clone <repository-url>
cd SnapKemon
npm install
```

Créer `android/local.properties` :
```properties
# Windows
sdk.dir=C:\\Users\\VOTRE_NOM\\AppData\\Local\\Android\\Sdk

# Mac/Linux
sdk.dir=/home/VOTRE_NOM/Android/Sdk
```

Lancer :
```sh
npm start        # Metro bundler
npm run android  # Build Android
```
