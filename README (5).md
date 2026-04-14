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
eps-demi-fond-protected.html   ← Application protégée (fichier à déployer)
eps-demi-fond.html             ← Application source (ne pas distribuer)
README.md                      ← Ce fichier
```

> ⚠️ **Toujours utiliser `eps-demi-fond-protected.html`** pour le déploiement.  
> Le fichier source ne doit jamais être partagé avec les élèves.

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

> 🔒 **Les identifiants Firebase sont confidentiels.**  
> Ils sont intégrés et chiffrés dans le fichier protégé.  
> Pour y accéder, consulter la [console Firebase](https://console.firebase.google.com) → projet **eps-demi-fond**, ou contacter le responsable du déploiement.

### Règles Realtime Database (à configurer dans la console Firebase)

```json
{
  "rules": {
    ".read": true,
    ".write": true
  }
}
```

> ⚠️ Restreindre ces règles avant tout déploiement en production.

### Collections Firestore utilisées

| Collection | Description |
|---|---|
| `sessions` | Sessions créées par l'enseignant (code, statut, classe) |
| `sessions/{id}/students` | Profils et résultats des élèves par session |
| `groups` | Groupes de 2 ou 3 élèves |

### Nœuds Realtime Database

| Nœud | Description |
|---|---|
| `live/{sessionId}/{studentId}` | Statut en direct de chaque élève |

---

## 🚀 Déploiement

### Option 1 — Firebase Hosting (recommandé)

```bash
npm install -g firebase-tools
firebase login
firebase init hosting
# Placer eps-demi-fond-protected.html dans public/index.html
firebase deploy
```

### Option 2 — Serveur web statique

Déposer `eps-demi-fond-protected.html` sur n'importe quel hébergement statique (Netlify, Vercel, GitHub Pages, Apache/Nginx…).

### Option 3 — Local (test uniquement)

Ouvrir directement `eps-demi-fond-protected.html` dans un navigateur.

---

## 👩‍🏫 Guide Enseignant

### 1. Accès à l'espace enseignant

Sur l'écran d'accueil, cliquer sur **Enseignant** puis saisir le code d'administration.

> 🔒 **Le code d'accès enseignant est confidentiel.**  
> Transmis uniquement à l'enseignant responsable. Ne jamais le communiquer aux élèves.

### 2. Créer une session

1. Aller dans l'onglet **Sessions** → **+ Créer une session**
2. Choisir la classe (Première / Terminale)
3. Donner un nom à la session (ex : *Évaluation Demi-Fond S2*)
4. Saisir ou générer un **code session** (lettres et/ou chiffres, 1 à 6 caractères)
5. Définir le statut : **Ouverte** ou **Fermée**
6. Cliquer sur **Créer la session ✓**

### 3. Communiquer le code aux élèves

Projeter ou dicter le **code session** avant la séance.  
Sans ce code, aucun élève ne peut accéder à l'application.

### 4. Surveillance en direct (onglet Live 👁)

| Indicateur | Description |
|---|---|
| 🟢 Actif | L'élève est sur l'application |
| 🟡 Terminé | L'élève a fini ses 3 courses |
| 🔴 Absent | L'élève a quitté l'application |
| ⚠️ Sorties | Nombre de fois où l'élève a quitté l'app |

### 5. Résultats et export

L'onglet **Résultats** permet de filtrer par session et de consulter les scores.  
Le bouton **📄 Exporter PDF** déclenche l'impression/sauvegarde.

---

## 🏃 Guide Élève

### 1. Rejoindre une session

1. Ouvrir l'URL de l'application dans le navigateur du smartphone
2. Cliquer sur **Élève**
3. Saisir le **code session** via le clavier alphanumérique
4. Cliquer sur **Accéder →**

> Si le code est incorrect ou la session fermée, l'accès est refusé.

### 2. Remplir son profil

- **Prénom**, **Classe**, **Sexe**
- **VMA** en km/h → la distance cible est calculée automatiquement

### 3. Groupe

- **Créer** un groupe (2 ou 3 élèves) → partager le code affiché
- **Rejoindre** un groupe avec le code d'un camarade

### 4. Déroulement

Les rôles **Coureur** et **Juge** alternent automatiquement sur les 3 courses.

**Coureur :** lancer le chrono → courir 1:30 → saisir la distance réalisée  
**Juge :** chronométrer → noter les splits → observations → auto-évaluation

---

## 📊 Barème

### Performance /6

| Niveau | Garçons | Filles | Note |
|---|---|---|---|
| Très bonne maîtrise | ≥ 1600 m | ≥ 1400 m | 6 |
| Maîtrise satisfaisante | 1325 m | 1125 m | 4,5 |
| Maîtrise fragile | 1175 m | 975 m | 3 |
| Maîtrise insuffisante | 1025 m | 825 m | 1,5 |

### Maîtrise /6 — Écart à la ligne cible

| Écart | Points |
|---|---|
| 0 m (ligne exacte) | **2 pts** |
| ± 12,5 m (demi-plot) | **1,5 pt** |
| ± 25 m (un plot) | **0,75 pt** |
| > 25 m (hors zone) | **0,25 pt** |

### AFL2 — Rôle Juge /4

Évaluation sur 2 critères (degrés 1→4) : qualité des observations + communication & chronométrage.

### AFL3 — Autonomie /4

| Degré | Points |
|---|---|
| 1 — Passif | 0 |
| 2 — Partiel | 1 |
| 3 — Autonome | 2,5 |
| 4 — Très autonome | 4 |

### Score Final /20

```
Total = Performance(/6) + Maîtrise(/6) + AFL2(/4) + AFL3(/4)
```

---

## 🔒 Sécurité

### Protection du code source

Le fichier `eps-demi-fond-protected.html` utilise 3 couches de protection :

1. **Minification** — suppression des commentaires et espaces
2. **Compression + Base64** — le code est compressé puis encodé
3. **Chiffrement XOR** — le résultat est chiffré avec une clé secrète

Les informations suivantes sont **invisibles** dans le code source distribué :
- Code d'accès enseignant
- Clés et configuration Firebase
- Barèmes et logique de notation

### Détection des sorties

L'application détecte si un élève quitte la page (changement d'onglet, retour accueil…).

- Compteur de sorties synchronisé vers Firebase en temps réel
- Alerte rouge visible côté enseignant avec le nombre de sorties par élève

---

## 🛠️ Personnalisation (fichier source uniquement)

> ⚠️ Modifier uniquement `eps-demi-fond.html` (source), puis régénérer le fichier protégé.

| Élément | Emplacement dans le code source |
|---|---|
| Code d'accès enseignant | Variable `ADMIN_CODE` |
| Durée de course | Variable `RACE_DURATION` (en secondes) |
| Barème performance | Tableaux `PERF_BOYS` et `PERF_GIRLS` |
| Zones de maîtrise | Tableau `ZONE_TABLE` |

---

## 📱 Compatibilité

| Navigateur | Support |
|---|---|
| Chrome Android | ✅ Recommandé |
| Safari iOS | ✅ Compatible |
| Firefox Android | ✅ Compatible |
| Chrome Desktop | ✅ Compatible (enseignant) |

---

## 📁 Structure des données Firebase

```
Firestore
├── sessions/
│   └── {sessionId}/
│       ├── code, name, class, status
│       └── students/
│           └── {studentId}/
│               ├── name, sex, vma, class, status, exitCount
│               ├── course1/2/3 : { distance, target, zone, zonePoints }
│               ├── judge1/2/3  : { obs, comm, auto, afl2, afl3, splits }
│               └── finalScores : { total, perfNote, maitrNote, afl2, afl3 }
└── groups/
    └── {groupId}/
        ├── code, sessionId, size, status
        └── members: [{ id, name, sex, vma }]

Realtime Database
└── live/
    └── {sessionId}/
        └── {studentId}/
            ├── name, sex, vma, status, exitCount, ts
```

---

## 📄 Licence

Usage pédagogique interne — Lycée EPS.  
Développé pour le dispositif d'évaluation Demi-Fond Bac GT.
