
<script>
    import {link} from "svelte-spa-router";
   

    const get_spaces = async () => {
        // Appel de la requête
        const response = await fetch(import.meta.env.VITE_URL_DIRECTUS + "/items/space");
        // Extraction du json de la réponse
        const result = await response.json();
        //Extraction et retour de la liste
        return result.data;
    }
</script>


<h1 id="title1">Les espaces</h1>

{#await get_spaces()}
    <p>Chargement de la liste...</p>
{:then spaces} 
    {#each spaces as space}
        <section>
            <div>
                <h3>Espace N°{space.name}</h3>
                <p>Nombre maximum de personnes :{space.size}</p>
                <p>Prix: {space.price}€</p>
                <p>Se situe à {space.location}</p>
                <a href="/espaces/{space.id}" use:link>Voir plus</a>
            </div>
        </section>
        
    {/each}
{/await}
