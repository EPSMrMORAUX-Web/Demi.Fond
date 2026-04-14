# 🏃 EPS DEMI-FOND — Application Web d'Évaluation

> Application mobile-first pour l'évaluation du demi-fond en EPS (Bac GT)  
> **3 × 1min30 · Gestion d'allure · VMA · Barème officiel**

---

## 📋 Présentation

**EPS Demi-Fond** est une application web progressive destinée aux séances d'évaluation en demi-fond au lycée. Elle remplace entièrement le suivi sous Excel en permettant :

- La **saisie en direct** des performances par les élèves eux-mêmes (sur smartphone)
- La **surveillance en temps réel** par l'enseignant depuis son propre appareil
- Le **calcul automatique** des notes selon le barème officiel Bac GT
- La **synchronisation Firebase** de toutes les données sans installation requise

L'application fonctionne dans n'importe quel navigateur mobile, sans téléchargement, directement depuis une URL hébergée.

---

## 🗂️ Structure du projet

```
eps-demi-fond.html      ← Application complète (fichier unique)
README.md               ← Ce fichier
TEST_CLAUDE.xlsx        ← Barème source (Excel de référence)
```

> L'application est contenue dans un **seul fichier HTML** (~3 000 lignes). Aucune dépendance externe hors Firebase et Google Fonts.

---

## ⚙️ Technologies utilisées

| Technologie | Usage |
|---|---|
| HTML / CSS / JavaScript | Application frontend (fichier unique) |
| Firebase Firestore | Stockage persistant des sessions et résultats |
| Firebase Realtime Database | Surveillance live des élèves (statut temps réel) |
| Google Fonts | Typographies : Barlow Condensed + DM Sans |

---

## 🔥 Configuration Firebase

Le projet utilise le projet Firebase `eps-demi-fond`.

```javascript
const firebaseConfig = {
  apiKey:            "AIzaSyCNSYunMTqq7ZNy8Y11Nr-gDtpc8kCiSJM",
  authDomain:        "eps-demi-fond.firebaseapp.com",
  projectId:         "eps-demi-fond",
  storageBucket:     "eps-demi-fond.firebasestorage.app",
  messagingSenderId: "906007293823",
  appId:             "1:906007293823:web:8f9222a4da13bb74d2344a",
  databaseURL:       "https://eps-demi-fond-default-rtdb.europe-west1.firebasedatabase.app"
};
```

### Règles Realtime Database (à configurer dans la console Firebase)

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

> ⚠️ Pour un déploiement en production, restreindre les règles d'accès selon vos besoins de sécurité.

### Collections Firestore utilisées

| Collection | Description |
|---|---|
| `sessions` | Sessions créées par l'enseignant (code, statut, classe) |
| `sessions/{id}/students` | Profils et résultats des élèves par session |
| `groups` | Groupes de 2 ou 3 élèves en attente |

### Nœuds Realtime Database

| Nœud | Description |
|---|---|
| `live/{sessionId}/{studentId}` | Statut en direct de chaque élève (actif, en course, absent) |

---

## 🚀 Déploiement

### Option 1 — Firebase Hosting (recommandé)

```bash
npm install -g firebase-tools
firebase login
firebase init hosting
# Placer eps-demi-fond.html dans le dossier public/
# Renommer en index.html ou configurer firebase.json
firebase deploy
```

### Option 2 — Tout serveur web statique

Déposer `eps-demi-fond.html` sur n'importe quel hébergement web statique (Netlify, Vercel, GitHub Pages, serveur Apache/Nginx…).

### Option 3 — Local (test)

Ouvrir directement `eps-demi-fond.html` dans un navigateur. Firebase fonctionnera si une connexion internet est disponible.

---

## 👩‍🏫 Guide Enseignant

### 1. Accès à l'espace enseignant

Sur l'écran d'accueil, cliquer sur **Enseignant** puis saisir le code :

```
epsdemifond
```

> ⚠️ Ce code est confidentiel. Ne pas le communiquer aux élèves.

### 2. Créer une session

1. Aller dans l'onglet **Sessions** → **+ Créer une session**
2. Choisir la classe (Première / Terminale)
3. Donner un nom à la session (ex : *Évaluation Demi-Fond S2*)
4. Saisir ou générer un **code session** (lettres et/ou chiffres, 1 à 6 caractères, ex : `EPS1A4`)
5. Définir le statut : **Ouverte** (accessible aux élèves) ou **Fermée**
6. Cliquer sur **Créer la session ✓**

### 3. Communiquer le code aux élèves

Projeter ou dicter le code session aux élèves avant la séance. Sans ce code, aucun élève ne peut accéder à l'application.

### 4. Surveillance en direct

L'onglet **Live 👁** affiche en temps réel :

| Indicateur | Description |
|---|---|
| 🟢 Actif | L'élève est sur l'application |
| 🟡 Terminé | L'élève a fini ses 3 courses |
| 🔴 Absent | L'élève a quitté l'application |
| ⚠️ Sorties | Nombre de fois où l'élève a quitté l'app |

### 5. Consulter et exporter les résultats

L'onglet **Résultats** permet de filtrer par session et de consulter les scores de chaque élève. Le bouton **📄 Exporter PDF** déclenche l'impression/sauvegarde en PDF.

---

## 🏃 Guide Élève

### 1. Rejoindre une session

1. Ouvrir l'URL de l'application dans le navigateur du smartphone
2. Cliquer sur **Élève**
3. Saisir le **code session** communiqué par l'enseignant via le clavier alphanumérique
4. Cliquer sur **Accéder →**

> Si le code est incorrect ou la session fermée, l'accès est refusé.

### 2. Remplir son profil

- **Prénom**
- **Classe** (Première / Terminale)
- **Sexe** (utilisé pour le barème différencié garçons/filles)
- **VMA** en km/h via le curseur → la distance cible par course est calculée automatiquement

### 3. Créer ou rejoindre un groupe

- **Créer** un groupe (2 ou 3 élèves) → partager le code de groupe affiché
- **Rejoindre** un groupe existant avec le code d'un camarade

### 4. Déroulement d'une course

Les rôles **Coureur** et **Juge** alternent automatiquement entre les 3 courses.

**En tant que Coureur :**
1. Consulter la distance cible affichée
2. Lancer le chronomètre (1 min 30)
3. À la fin : saisir la distance réellement parcourue
4. La note de maîtrise est calculée automatiquement

**En tant que Juge :**
1. Chronométrer le coureur
2. Enregistrer des temps intermédiaires (splits)
3. Saisir des observations qualitatives
4. S'auto-évaluer sur 4 critères (degrés 1 à 4)

---

## 📊 Barème

### Note de Performance /6

Basé sur la distance totale parcourue sur les 3 courses.

| Niveau | Garçons (distance) | Filles (distance) | Note |
|---|---|---|---|
| Très bonne maîtrise | ≥ 1600 m | ≥ 1400 m | 6 |
| Maîtrise satisfaisante | 1325 m | 1125 m | 4,5 |
| Maîtrise fragile | 1175 m | 975 m | 3 |
| Maîtrise insuffisante | 1025 m | 825 m | 1,5 |

> Tableau complet issu du fichier `TEST_CLAUDE.xlsx`, feuille *PERF. Bac GT*.

### Note de Maîtrise /6

Basé sur l'écart entre la distance réalisée et la ligne cible à chaque course.

| Écart à la ligne cible | Points |
|---|---|
| 0 m (ligne exacte) | **2 pts** |
| ± 12,5 m (demi-plot) | **1,5 pt** |
| ± 25 m (un plot entier) | **0,75 pt** |
| > 25 m (hors zone) | **0,25 pt** |

> Maximum : 3 courses × 2 pts = **6 pts**

### AFL2 — Rôle de Juge /4

Évaluation automatique sur 2 critères (degrés 1 à 4) :
- Qualité des observations (concentration, précision, variété)
- Communication & chronométrage (échanges avec le coureur, splits)

### AFL3 — Autonomie /4

Évaluation sur le degré d'autonomie dans la gestion de la séance (échauffement, récupération, projet de course).

| Degré | Description | Points |
|---|---|---|
| 1 | Passif / séance inadaptée | 0 |
| 2 | Partiel / suit les autres | 1 |
| 3 | Autonome / séance adaptée | 2,5 |
| 4 | Très autonome / anticipé | 4 |

### Score Final /20

```
Total = Performance(/6) + Maîtrise(/6) + AFL2(/4) + AFL3(/4)
```

---

## 🔒 Sécurité & Surveillance

### Détection des sorties d'application

L'application détecte automatiquement si un élève quitte la page (changement d'onglet, retour à l'accueil, etc.) via les événements `visibilitychange` et `blur` du navigateur.

- Un **compteur de sorties** est incrémenté à chaque sortie
- Le compteur est **synchronisé en temps réel** vers Firebase
- L'enseignant voit une **alerte rouge** et le nombre de sorties de chaque élève

> Note : la surveillance est indicative. L'accès à l'écran de l'élève reste techniquement impossible depuis un navigateur web standard.

---

## 🛠️ Personnalisation

### Modifier le mot de passe enseignant

Dans le fichier `eps-demi-fond.html`, rechercher et modifier :

```javascript
const ADMIN_CODE = 'epsdemifond';
```

### Modifier les barèmes

Les tableaux de barème sont définis en début de script :

```javascript
// Barème performance garçons : [distance totale (m), note /6]
const PERF_BOYS = [
  [1600, 6], [1500, 6], [1450, 5.75], ...
];

// Barème performance filles
const PERF_GIRLS = [
  [1400, 6], [1275, 6], ...
];

// Zones de maîtrise : [écart max (m), points, label, couleur]
const ZONE_TABLE = [
  { maxGap: 0,        pts: 2,    label: 'Ligne exacte' },
  { maxGap: 12.5,     pts: 1.5,  label: '±12,5 m'     },
  { maxGap: 25,       pts: 0.75, label: '±25 m'        },
  { maxGap: Infinity, pts: 0.25, label: 'Hors zone'    },
];
```

### Modifier la durée de course

```javascript
const RACE_DURATION = 90; // secondes (1 min 30 par défaut)
```

---

## 📱 Compatibilité

| Navigateur | Support |
|---|---|
| Chrome Android | ✅ Recommandé |
| Safari iOS | ✅ Compatible |
| Firefox Android | ✅ Compatible |
| Chrome Desktop | ✅ Compatible (développement / enseignant) |

> L'interface est optimisée pour une utilisation **en extérieur sur smartphone** : grands boutons, fort contraste, lisible en plein soleil.

---

## 📁 Données Firebase — Structure

```
Firestore
├── sessions/
│   ├── {sessionId}/
│   │   ├── code: "EPS1A4"
│   │   ├── name: "Évaluation S2"
│   │   ├── class: "Terminale"
│   │   ├── status: "active"
│   │   └── students/
│   │       └── {studentId}/
│   │           ├── name, sex, vma, class
│   │           ├── status, exitCount, lastSeen
│   │           ├── course1: { distance, target, zone, zonePoints }
│   │           ├── course2: { ... }
│   │           ├── course3: { ... }
│   │           ├── judge1/2/3: { obs, comm, auto, afl2, afl3, splits }
│   │           └── finalScores: { total, perfNote, maitrNote, afl2, afl3 }
└── groups/
    └── {groupId}/
        ├── code, sessionId, size, status
        └── members: [{ id, name, sex, vma }]

Realtime Database
└── live/
    └── {sessionId}/
        └── {studentId}/
            ├── name, sex, vma, status
            ├── exitCount
            └── ts (timestamp)
```

---

## 👨‍💻 Développement

Cloner ou télécharger le fichier `eps-demi-fond.html`, ouvrir dans un navigateur. Toute modification peut être faite directement dans ce fichier unique.

Pour tester sans Firebase (développement local uniquement), il suffit de désactiver temporairement la vérification Firebase dans `validateSession()` — attention à ne jamais déployer cette version en production.

---

## 📄 Licence

Usage pédagogique interne — Lycée EPS.  
Développé spécifiquement pour le dispositif d'évaluation Demi-Fond Bac GT.
