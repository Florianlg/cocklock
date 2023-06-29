# CO'clock Working, Vite et bien

## Objectif

Préparer une base de projet autours de Vite pour avancer CO'clock Working. Cette base de projet nous permettra d'utiliser des outils comme SCSS et Svelte. Nous découvrirons ces outils dans les jours suivants.

## Comment ? 

### Énoncé débrouillard

L'objectif est d'initialiser un projet Vite en utilisant le template Svelte, et de faire en sorte qu'il affiche la page d'accueil du projet CO'clock Working 


### Étape 1

Après avoir cloné ce dépôt, initialise un projet vite. On peut initialiser un projet en utilisant divers template. Nous allons utiliser le template qui nous permettra d'utiliser Svelte, qui sera le Framework que nous utiliserons par la suite.


`npm create vite@latest . -- --template svelte`

Il faut ensuite lancer `npm install` pour télécharger toutes les dépendances

Nous pouvons enfin tester le bon fonctionnement en lançant la commande

`npm run dev`

### Étape 2

Regardons un peu la base de code générée. Les fichiers qui nous intéressent sont les suivants : 
- à la racine nous retrouvons différents fichiers de configuration.
- Nous retrouvons aussi le fichier `index.html`. Contrairement à ce qu'on pourrait imaginer, nous n'allons pas toucher à ce fichier. Vite va utiliser ce fichier comme base pour y insérer le code de nos différentes pages.
- Le dossier `src` est le dossier qui va contenir tout le site. Ici 2 fichiers vont nous intéresser
  - App.svelte, qui va pour l'instant contenir le code HTML de notre page d'accueil, on le fera évoluer plus tard.
  - main.js, qui est le point d'entrée de notre application. Il initialise le projet Svelte et surtout il inclut toutes les ressources de notre projet, dont notre CSS.
  

### Étape 3

Nous allons vider le fichier `App.svelte` de son contenu pour le remplacer par le HTML de la page d'accueil de notre projet CO'clock Working. Étant donné que Vite va inclure le code au sein d'une page web, nous allons seulement inclure le contenu du `<body>` de notre page.

Nous pouvons aussi supprimer le fichier `lib/Counter.svelte`. C'est un fichier de démonstration pour illustrer le fonctionnement de Svelte, nous verrons cela plus tard.

On peut tester en allant visiter l'URL fournie par la commande `npm run dev`. La page devrait afficher la home du projet, mais sans les styles. Nous allons remédier à ça.


### Étape 4

Dans cette étape, nous allons importer les styles de CO'clock Working dans notre site.  Un fichier app.css est présent, on voit qu'il est lu par Vite car il est importé dans `main.js`. Nous allons donc l'utiliser pour y stocker le CSS de notre site.

Nous allons donc copier les styles du projet dans ce fichier.

### Étape 5

C'est fini, nous n'avons plus qu'à tester ! 
