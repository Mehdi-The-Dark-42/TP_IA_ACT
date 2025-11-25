# 📋 Documentation Technique - Diagnostic RIA

## Vue d'ensemble

**Diagnostic RIA** est une application web interactive développée en HTML5, CSS3 et JavaScript vanilla. Elle évalue la conformité des systèmes d'intelligence artificielle selon le **Règlement européen sur l'IA (RIA)**.

**Objectif :** Fournir aux entreprises une évaluation rapide (5-7 minutes) du niveau de risque de leurs systèmes IA et des obligations de conformité applicables.

---

## 📊 Architecture

### Structure générale

L'application suit une architecture **Single Page Application (SPA)** avec navigation multi-pages :

```
Page d'accueil (welcomePage)
    ↓
Formulaire de diagnostic (formPage)
    ↓
Rapport de résultats (resultsPage)
```

### Fonctionnement

- **État centralisé** : Objet `appState` qui stocke tous les données utilisateur
- **Navigation dynamique** : Système de pages avec affichage/masquage par classe CSS
- **Calcul dynamique** : Algorithme de scoring du risque basé sur les réponses
- **Responsive Design** : Adaptation mobile via media queries

---

## 🎨 Sections Principales

### 1. **Page d'accueil** (`welcomePage`)

**Rôle :** Présentation générale et value proposition

**Éléments clés :**
- Header avec icône et branding
- Intro texte explicatif
- Liste d'avantages avec icônes cochées
- CTA principal : "Commencer le diagnostic"
- Durée estimée : 5-7 minutes
- Footer avec crédit "AI Compliance Partners"

**Styles appliqués :**
- `.card` : Conteneur blanc avec ombre et arrondi
- `.header-section` : Gradient violet/bleu
- `.benefits-list` : Liste avec icônes de validation
- `.btn-primary` : Bouton CTA avec gradient

---

### 2. **Page formulaire** (`formPage`)

**Rôle :** Collecte des données pour le diagnostic

**Sections du formulaire :**

#### A. Informations entreprise
- Nom de l'entreprise * (required)
- Email * (required)
- Téléphone * (required)
- Secteur d'activité * (select dropdown)
  - Finance / Banque
  - Santé
  - Commerce / Retail
  - Industrie
  - Technologies
  - Ressources Humaines
  - Sécurité / Surveillance
  - Autre

#### B. Systèmes IA
- Utilisation actuelle d'IA * (toggle buttons)
  - "Oui" / "En projet"
- Type(s) de systèmes IA * (checkbox grid)
  - 👤 Reconnaissance faciale
  - 💼 Recrutement / RH
  - 💳 Scoring crédit
  - 📹 Surveillance
  - 💬 Chatbot / Assistant
  - 🎯 Recommandations
- Objectif principal (textarea)

#### C. Données et impact
- Types de données traitées * (checkbox grid)
  - 🔐 Données biométriques
  - 🏥 Données de santé
  - ⚖️ Données judiciaires
  - 👤 Données personnelles
  - 💼 Données professionnelles
  - 📈 Données comportementales
- Impact sur les utilisateurs * (select)
  - Critique (droits fondamentaux, emploi, crédit)
  - Important (décisions significatives)
  - Modéré (recommandations, suggestions)
  - Faible (filtres, tri basique)
- Niveau de conformité actuel (select)
  - Aucune démarche entreprise
  - En phase de réflexion
  - Mise en conformité en cours
  - Pensons être conformes

**Validation :**
- Champs obligatoires vérifiés avant génération du rapport
- Messages d'erreur avec `alert()` pour les omissions

---

### 3. **Page résultats** (`resultsPage`)

**Rôle :** Affichage du rapport personnalisé

**Éléments composants :**

#### Risk Result (Résultat du risque)
- Badge de niveau de risque avec couleur spécifique
- Score numérique sur 100
- 4 niveaux : Inacceptable / Élevé / Limité / Minimal

#### Recommendations Section (Recommandations)
- Liste numérotée des actions à entreprendre
- Personnalisée selon le niveau de risque
- 10 recommandations maximum par niveau

#### Estimates Grid (Estimations)
- Budget estimé pour la conformité
- Délai estimé du projet
- Design en 2 colonnes (responsive en 1 colonne mobile)

#### CTA Section (Appel à l'action)
- Message de suivi commercial
- 2 boutons :
  - Télécharger le rapport PDF
  - Nouveau diagnostic

---

## 💻 Logique JavaScript

### État de l'application (`appState`)

```javascript
const appState = {
    companyName: '',           // Nom entreprise
    email: '',                 // Email contact
    phone: '',                 // Téléphone
    sector: '',                // Secteur activité
    hasAI: '',                 // 'oui' ou 'projet'
    aiTypes: [],               // Array de types IA
    aiPurpose: '',             // Objectif IA (texte libre)
    dataTypes: [],             // Array de types données
    userImpact: '',            // Impact utilisateurs
    currentCompliance: ''       // Niveau conformité actuel
};
```

### Fonctions principales

#### `showPage(pageId)`
```javascript
// Affiche une page spécifique et masque les autres
function showPage(pageId) {
    document.querySelectorAll('.page').forEach(page => {
        page.classList.remove('active');
    });
    document.getElementById(pageId).classList.add('active');
}
```
**Utilisation :**
- `goToWelcome()` → affiche welcomePage
- `goToForm()` → affiche formPage
- Appelée après génération du rapport pour afficher resultsPage

---

#### `calculateRiskLevel()`
**Objectif :** Calculer le score de risque (0-100)

**Système de scoring :**

| Élément | Points |
|--------|--------|
| **Types IA** | |
| Reconnaissance faciale | +30 |
| Recrutement/RH | +25 |
| Scoring crédit | +25 |
| Surveillance | +35 |
| Chatbot | +10 |
| Recommandations | +5 |
| **Impact utilisateurs** | |
| Critique | +30 |
| Important | +20 |
| Modéré | +10 |
| **Données sensibles** | |
| Biométriques | +25 |
| Santé | +20 |
| Judiciaires | +20 |

**Classification du risque :**
- **< 20** → Minimal ✅
- **20-39** → Limité ℹ️
- **40-59** → Élevé ⚠️
- **≥ 60** → Inacceptable 🚫

---

#### `generateReport()`
**Processus :**
1. Extraction des données du formulaire dans `appState`
2. Validation des champs obligatoires
3. Calcul du niveau de risque
4. Génération des recommandations personnalisées
5. Estimation des coûts et délais
6. Affichage de la page résultats

**Recommandations par niveau :**

**Inacceptable :**
- ⛔ Système IA INTERDIT selon le RIA
- 🔄 Repenser l'approche technologique
- ⚖️ Consulter juriste spécialisé
- 📋 Évaluer alternatives conformes

**Élevé :**
- 📋 Documentation technique complète obligatoire
- 🔍 Évaluation par organisme notifié
- 👁️ Supervision humaine requise
- 🔐 Tests de robustesse renforcés
- 📊 Traçabilité données d'entraînement
- ✅ Enregistrement base européenne

**Limité :**
- 🔔 Informer utilisateurs de l'IA
- 📝 Transparence du fonctionnement
- 👤 Identifier contenu généré par IA
- 📋 Documentation processus décisionnels

**Minimal :**
- ✅ Aucune obligation RIA
- 💡 Bonnes pratiques éthiques
- 📊 Documentation volontaire
- 🔄 Suivi évolutions réglementaires

---

#### `resetApp()`
**Réinitialise :**
- L'objet `appState` à vide
- Le formulaire (`.reset()`)
- Supprime les classes `.active` des boutons
- Retour à la page d'accueil

---

### Gestion des événements

#### Toggle Buttons (Boutons exclusifs)
```javascript
if (e.target.classList.contains('toggle-btn')) {
    const field = e.target.dataset.field;
    const value = e.target.dataset.value;
    
    // Désactiver tous les boutons du groupe
    group.querySelectorAll('.toggle-btn').forEach(btn => {
        btn.classList.remove('active');
    });
    
    // Activer le bouton cliqué
    e.target.classList.add('active');
    appState[field] = value;
}
```

#### Checkbox Buttons (Sélection multiple)
```javascript
if (e.target.classList.contains('checkbox-btn')) {
    const field = e.target.dataset.field;
    const value = e.target.dataset.value;
    
    e.target.classList.toggle('active');
    
    if (e.target.classList.contains('active')) {
        appState[field].push(value);
    } else {
        appState[field] = appState[field].filter(item => item !== value);
    }
}
```

---

## 🎨 Système de styles CSS

### Variables de couleurs principales

| Usage | Couleur | Hex |
|-------|---------|-----|
| Primary Gradient | Violet → Bleu | #6a11cb → #2575fc |
| Risk Inacceptable | Rouge clair | #fee2e2 |
| Risk Élevé | Jaune clair | #fef3c7 |
| Risk Limité | Bleu clair | #dbeafe |
| Risk Minimal | Vert clair | #d1fae5 |
| Text Primary | Gris foncé | #111827 |
| Text Secondary | Gris moyen | #6b7280 |

### Composants réutilisables

**Boutons :**
- `.btn-primary` : CTA principal avec gradient
- `.btn-secondary` : Bouton gris neutre
- `.btn-white` : Bouton blanc avec ombre
- `.btn-dark` : Bouton violet foncé

**Grilles :**
- `.form-row` : 2 colonnes (responsive 1 col)
- `.checkbox-grid` : 2 colonnes (responsive 1 col)
- `.estimates-grid` : 2 colonnes (responsive 1 col)

**Cartes :**
- `.card` : Conteneur blanc principal
- `.form-section` : Section grise avec bordure
- `.risk-result` : Résultat risque avec gradient
- `.estimate-card` : Cartes budget/timeline

### Responsive Design

**Breakpoint :** `@media (max-width: 768px)`

Adaptations :
- Grilles 2 colonnes → 1 colonne
- Button-row flex → flex-column
- Réduction padding sur mobile

---

## 📋 Validation et règles métier

### Champs obligatoires
1. Nom entreprise
2. Email
3. Téléphone
4. Secteur activité
5. Utilisation IA (toggle)
6. Type(s) IA (au moins 1)
7. Impact utilisateurs

### Règles conditionnelles
- **Mini 1 type IA** : `if (appState.aiTypes.length === 0)`
- **Email valide** : `type="email"` HTML5
- **Téléphone** : `type="tel"` (pattern optionnel)

### Messages d'erreur
Affichés via `alert()` :
- "Veuillez remplir tous les champs obligatoires"
- "Veuillez indiquer si vous utilisez actuellement l'IA"
- "Veuillez sélectionner au moins un type de système IA"
- "Veuillez sélectionner l'impact sur les utilisateurs"

---

## 📦 Architecture des données

### Flow de données

```
Utilisateur remplit formulaire
    ↓
  [Extraction appState]
    ↓
  [Validation]
    ↓
  [Calcul risque + scoring]
    ↓
  [Génération recommandations]
    ↓
  [Affichage résultats]
```

### Structures de données

**Recommandations (Object)** :
```javascript
const recommendations = {
    inacceptable: [...],
    eleve: [...],
    limite: [...],
    minimal: [...]
}
```

**Coûts estimés (Object)** :
```javascript
const estimatedCost = {
    inacceptable: "Non applicable",
    eleve: "15 000€ - 50 000€",
    limite: "3 000€ - 8 000€",
    minimal: "1 000€ - 3 000€"
}
```

**Délais estimés (Object)** :
```javascript
const timeline = {
    inacceptable: "Arrêt immédiat requis",
    eleve: "6-12 mois",
    limite: "2-4 mois",
    minimal: "1-2 mois"
}
```

---

## 🔄 Cycle de vie de l'application

1. **Chargement initial** : `DOMContentLoaded`
   - Appelle `showPage('welcomePage')`
   - Affiche page d'accueil

2. **Navigation formulaire** : Clic "Commencer"
   - Appelle `goToForm()`
   - Affiche formulaire avec tous les champs vides

3. **Remplissage formulaire** :
   - Listeners sur toggle/checkbox pour mise à jour `appState`
   - Champs text/select mis à jour manuellement dans `generateReport()`

4. **Génération rapport** : Clic "Générer diagnostic"
   - Validation formulaire
   - Calcul risque
   - Affichage résultats

5. **Actions finales** :
   - "Nouveau diagnostic" → réinitialise et retour accueil
   - "Retour" → `goToWelcome()`

---

## 🚀 Fonctionnalités avancées possibles

### À implémenter
1. **Export PDF** : Utiliser library `jspdf` ou `html2pdf`
2. **Stockage local** : LocalStorage pour sauvegarder brouillons
3. **Partage rapport** : Lien unique générés après diagnostic
4. **Backend** : API pour envoyer données + générer devis
5. **Tracking analytics** : Google Analytics pour suivi conversions
6. **Intégration CRM** : Envoi automatique données à CRM commercial

### Optimisations possibles
1. Séparation HTML/CSS/JS dans fichiers distincts
2. Minification et bundling
3. Service Worker pour offline mode
4. Progressive Web App (PWA)

---

## 📝 Guide de maintenance

### Modifier les recommandations
Localiser l'objet `recommendations` dans la fonction `generateReport()` et modifier les textes pour chaque niveau de risque.

### Ajuster le système de scoring
Modifier les valeurs dans `calculateRiskLevel()` pour les points attribués par type de risque.

### Ajouter un nouveau type de système IA
1. Ajouter bouton checkbox dans formulaire
2. Mettre à jour array `aiTypes` et scoring

### Changer l'estimation de coûts/délais
Modifier les objets `estimatedCost` et `timeline` dans `generateReport()`.

---

## 🔐 Sécurité et conformité

### Points de sécurité à noter
- **Pas de backend** : Aucune donnée sensible transmise
- **Client-side only** : Données stockées en mémoire uniquement
- **HTML5 Validation** : Vérification minimale type (email, tel)
- **XSS Prevention** : Utilisation de `.textContent` et `.innerHTML` contrôlé

### À renforcer pour production
1. Validation backend stricte
2. Chiffrement transmission données
3. GDPR compliance (politiques données)
4. Rate limiting APIs
5. WAF (Web Application Firewall)

---

## 📞 Support et contact

**Organisation :** AI Compliance Partners
- Email : mehdiahnou@gmail.com
- Téléphone : +33 6 34 39 90 56
- Spécialité : Réglementation IA & Conformité RIA

---

**Document généré** : 25/11/2025  
**Version** : 1.0  
**Auteur** : AI Compliance Partners
