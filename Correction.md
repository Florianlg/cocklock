# Correction E15

Nous avons les requêtes fetch qui fonctionne dans le composant `TestApi.svelte`

Nous allons les utiliser dans un nouveau composant `Space.svelte` et l'appeler depuis le composant principal `App.svelte`.

Prendre le contenu de la balise `main` pour le mettre dans un composant `Home.svlete`

```js
<script>
    import Stats from "./Stats.svelte";
</script>

<h1 id="title1">Co'CoClock Working</h1>

<section aria-label="Introduction">
    <h2>Qui sommes-nous ?</h2>
    <p>...</p>
</section>

<Stats />
```

Appeler le composant `Space.svelte` dans `App.svlete`

Ajouter les fonctions de requêtes dans le nouveau composant.

Il va falloir modifier tout ça pour ne récupérer que les données d'un espace en particulier, on commence par définir une propriété `space_id` au composant.

```js
export let space_id;
```

Pour ensuite l'appeler comme ça dans `App.svelte`

```js
<main aria-labelledby="title1">
    <Spaces space_id="1"/>
</main>
```

`get_spaces` devient `get_space` avec un paramètre `id` pour filtrer la requête

```js
const get_space = async (id) => {
    // Appel de la requête
    const response = await fetch(import.meta.env.VITE_URL_DIRECTUS + "/items/Spaces/" + id);
    // Extraction du json de la réponse
    const result = await response.json();
    //Extraction et retour de la liste
    return result.data;
}
```

Init change un peu

```js
// fonction d'initialisation du composant
const init = async () => {
    const space = await get_space(space_id)
    console.log("space :", space);

    const token = await login();
    console.log("token :", token);
    
    ...
}
```

Pour récupérer les commentaires, nous avons besoin de la liste des réservations de l'espace. On crée une fonction qui récupère uniquement les id de résa

```js
// Fonction de récupération de la liste des ids de résa
const get_resa_ids = async (token, id) => {
    // Création du endpoint avec filtre et fields
    let endpoint = import.meta.env.VITE_URL_DIRECTUS + "/items/Reservations?";
    endpoint += "filter[space_id][_eq]=" + id;
    endpoint += "&fields=space_id";

    // Appel de la requête
    const response = await fetch(endpoint, {
        headers: {          
            'Authorization': 'Bearer ' + token,
        }
    });
    // Extraction du json de la réponse
    const result = await response.json();

    //Extraction des ids pour les mettre dans un tableau
    let ids = [];
    result.data.forEach(item => {
        ids.push(item.space_id);
    });
    return ids;
}
```

On modifie `get_comments` pour récupérer uniquement les commentaires des réservations trouvées

```js
// Fonction de récupération de la liste des commentaires
const get_comments = async (token, resa_ids) => {
    let endpoint = import.meta.env.VITE_URL_DIRECTUS + "/items/Comments?";
    endpoint += "filter[resa_id][_in]=" + resa_ids.join(',');

    // Appel de la requête
    const response = await fetch(endpoint, {
        headers: {          
            'Authorization': 'Bearer ' + token,
        }
    });
    // Extraction du json de la réponse
    const result = await response.json();
    //Extraction et retour de la liste
    return result.data;
}
```

Enfin on modifie `init` pour tout récupérer

```js
const init = async () => {
    ...
    
    const resa_ids = await get_resa_ids(token, space_id);
    console.log("resa_ids :", resa_ids);
    
    const comments = await get_comments(token, resa_ids);
    console.log("comments :", comments);
}
```

## Bonus

On supprime l'appel de `get_space` de la fonction `init` pour le mettre dans une structure `#await :then`
```js
{#await get_space(space_id)}
    <p>Récupération des données de l'espace</p>
{:then data} 
    <h2>Informations</h2>
    <ul>
        <li>Nom : {data.name}</li>
        <li>Surface : {data.size}</li>
        <li>Equipements : {data.equipments}</li>
    </ul>
{/await}
```

Dans la partie `:then` on va rajouter la récupération des commentaires.

`init` devient `load_comments` et retourne la liste des commentaires trouvés

```js
const load_comments = async () => {
    const token = await login();
    const resa_ids = await get_resa_ids(token, space_id);
    const comments = await get_comments(token, resa_ids);
    return comments
}
```

Puis on ajoute le block `#await :then` pour récupérer et afficher la liste

```js
{#await load_comments()}
    <p>Récupération des commentaires</p>
{:then comments} 
    <h2>Commentaires</h2>
    <ul>
    {#each comments as comment}
        <li>{comment.title}</li>
    {/each}
    </ul>
{/await}
```