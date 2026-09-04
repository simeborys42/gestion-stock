# DATAWORLD Stock — version bureau Windows

## 🟢 Vous n'êtes pas développeur ? Faites compiler le .exe gratuitement par GitHub

Pas besoin d'installer Node.js ni de taper une seule commande. Suivez ces
étapes dans votre navigateur :

1. Créez un compte gratuit sur https://github.com (si vous n'en avez pas).
2. Cliquez sur "New repository" (Nouveau dépôt), donnez-lui un nom
   (ex. `dataworld-stock`), laissez-le en **Public**, puis "Create repository".
3. Sur la page du dépôt, cliquez sur "uploading an existing file" (ou
   "Add file" → "Upload files"), puis glissez-déposez **tout le contenu**
   de ce dossier (y compris le sous-dossier `.github`) et validez ("Commit
   changes").
4. Allez dans l'onglet "Actions" du dépôt. Une compilation démarre
   automatiquement (icône orange qui devient verte après 3-5 minutes).
5. Cliquez sur la compilation terminée (coche verte), puis tout en bas de
   la page, téléchargez le fichier `dataworld-stock-installeur-windows.zip`.
6. Dézippez-le : vous obtenez l'installeur `.exe`. Envoyez-le à vos équipes,
   double-clic pour installer sur chaque poste Windows.

C'est entièrement gratuit pour un dépôt public. Si une étape bloque, montrez-moi une capture d'écran de la conversation, je vous guide.

---

## Pour un développeur (méthode manuelle)

Ce projet transforme le logiciel de gestion de stock en une vraie application
Windows (.exe), avec un petit serveur central qui garde les données
synchronisées entre tous les sites.

## Ce dont vous avez besoin

- [Node.js](https://nodejs.org) version 18 ou plus (installe automatiquement `npm`)
- Un accès Internet le temps d'installer les dépendances (`npm install`)
- Windows 10/11 pour fabriquer l'installeur `.exe` (ou une machine capable de
  faire une "cross-build" Windows — plus simple de builder directement sous Windows)

## Installation

```bash
npm install
```

## Tester en développement

Deux commandes séparées, ou une seule qui lance les deux :

```bash
npm run dev
```

Puis ouvrez http://localhost:5173 dans votre navigateur (le serveur de données
tourne en parallèle sur le port 4000).

## Fabriquer l'application Windows (.exe)

```bash
npm run dist
```

Le résultat (un installeur `.exe`) apparaît dans le dossier `release/`.
Double-cliquez dessus sur un poste Windows pour installer l'application.

## Comment ça marche, concrètement

- L'application `.exe` contient à la fois l'interface **et** un petit serveur
  qui garde les données dans un fichier (`state.json`, dans le dossier de
  données de l'utilisateur Windows — pas besoin de base de données à installer).
- **Un seul poste doit faire tourner cette application en continu** : celui
  qui sert de "serveur central" (par exemple le poste du siège à Yaoundé).
- **Les autres sites** n'ont pas besoin d'installer le `.exe` : ils ouvrent
  simplement un navigateur et vont sur `http://<adresse-du-poste-serveur>:4000`
  (adresse IP locale si tout le monde est sur le même réseau, ou adresse
  publique si le serveur est hébergé en ligne).
- Si vos sites sont dans des villes différentes (pas le même réseau local),
  il faut héberger ce serveur quelque part d'accessible depuis Internet — un
  petit serveur/VPS bon marché (5-10 $/mois chez un hébergeur comme
  Contabo, Hetzner, DigitalOcean…) fonctionne très bien. Dans ce cas, faites
  tourner uniquement `server/index.js` (`npm run start` après `npm run build`)
  sur ce serveur, et tous les sites — y compris le siège — accèdent au
  logiciel depuis leur navigateur, à la même adresse.

## Icône de l'application (facultatif)

Ajoutez un fichier `electron/icon.ico` (256x256 recommandé), puis ajoutez
cette ligne dans `package.json`, section `build.win` :

```json
"icon": "electron/icon.ico"
```

## Sauvegardes

Le fichier de données se trouve à :
`%APPDATA%\dataworld-stock-desktop\data\state.json` (sur le poste serveur).
Faites-en une copie régulière (ex. sur une clé USB ou un espace cloud) pour
éviter toute perte en cas de panne.
