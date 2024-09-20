# Orga App

Une application Vue.js pour la gestion quotidienne des tâches avec un design adaptatif et des fonds dynamiques.

## Fonctionnalités

- Ajout de journées avec tâches prioritaires, secondaires et optionnelles
- Notation des journées (1-10)
- Sauvegarde des données
- Affichage inversé des journées (récentes en premier)
- Interface responsive (mobile, tablette, desktop)
- Fonds dégradés aléatoires

## Installation
```bash
git clone [URL_DU_REPO]
cd orga-app
npm install
npm run serve
```

Accédez à `http://localhost:8080` dans votre navigateur.

## Utilisation

1. Cliquez sur "Ajouter une journée"
2. Remplissez les tâches
3. Notez la journée
4. Sauvegardez

Les journées existantes sont en lecture seule.

## Structure

- `src/components/Orga.vue`: Composant principal (journée)
- `src/App.vue`: Composant racine
- `src/main.js`: Point d'entrée
- `src/styles/`: Styles globaux

## API

Utilise une API locale sur `http://localhost:3000` pour la persistance des données.