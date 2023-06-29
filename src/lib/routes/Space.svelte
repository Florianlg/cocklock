<script>
    import Login from "./Login.svelte";
    export let params = {};
    const space_id = params.id;

    let commentTitle = "";
    let commentRate = 5;
    let commentDesc = "";

    let comments = [];

    const handleSubmitForm = async (event) => {
        event.preventDefault();
        // Crée un nouveau commentaire
        const new_comment = await post_comment();
        // Vide le text area
        commentTitle = "";

        commentDesc = "";
        //AJout du commentaire à la liste
        comments.push(new_comment);
        //Je rafraichis ma liste pour que Svelte s'en apercoive
        comments = [...comments]; 
    }

    const load_comments = async (comments) => {
        const token = localStorage.getItem("token");
        const resa_ids = await get_resa_ids(token, space_id);
        comments = await get_comments(token, resa_ids);
        return comments;
    };
    const get_space = async (id) => {
        // Appel de la requête
        const response = await fetch(
            import.meta.env.VITE_URL_DIRECTUS + "/items/space/" + id
        );
        // Extraction du json de la réponse
        const result = await response.json();
        //Extraction et retour de la liste
        return result.data;
    };

    // Fonction de récupération de la liste des ids de résa
    const get_resa_ids = async (token, id) => {
        // Création du endpoint avec filtre et fields
        let endpoint = import.meta.env.VITE_URL_DIRECTUS + "/items/booking?";
        endpoint += "filter[space_id][_eq]=" + id;
        endpoint += "&fields=space_id";

        // Appel de la requête
        const response = await fetch(endpoint, {
            headers: {
                Authorization: "Bearer " + token,
            },
        });
        // Extraction du json de la réponse
        const result = await response.json();

        //Extraction des ids pour les mettre dans un tableau
        let ids = [];
        result.data.forEach((item) => {
            ids.push(item.space_id);
        });
        return ids;
    };

    // Fonction de récupération de la liste des commentaires
    const get_comments = async (token, resa_ids) => {
        let endpoint = import.meta.env.VITE_URL_DIRECTUS + "/items/comment?";
        endpoint += "filter[space_id][_in]=" + resa_ids.join(",");

        // Appel de la requête
        const response = await fetch(endpoint, {
            headers: {
                Authorization: "Bearer " + token,
            },
        });
        // Extraction du json de la réponse
        const result = await response.json();
        //Extraction et retour de la liste
        return result.data;
    };

        // Ajout d'un commentaire en BDD avec l'api
    const post_comment = async () => {
        const response = await fetch(import.meta.env.VITE_URL_DIRECTUS + "/items/comment", {
            method: "POST",
            headers: {
                "content-type": "application/json",
                "Authorization": "Bearer " + localStorage.getItem('token')
            },
            body: JSON.stringify({
                title: commentTitle,
                rate: commentRate,
                textarea: commentDesc,
                space_id: space_id
            })
        })

        // Extrait le token et le retourne
        const json = await response.json();
        return json.data;
    }
</script>

{#await get_space(space_id)}
    <p>Récupération des données de l'espace</p>
{:then data}
    <h2>Informations</h2>
    <ul>
        <li>Nom : {data.name}</li>
        <li>Surface : {data.size}</li>
        <li>Prix : {data.price}</li>
    </ul>

    {#await load_comments(comments)}
        <p>Récupération des commentaires</p>
    {:then list}
        <h2>Commentaires</h2>

        {#each list as comment}
            <div>
                <h3 id="comment-2">{comment.title}</h3>
                <i
                    >Posté le <time datetime={comment.date_created}
                        >{comment.date_created}</time
                    ></i
                >
                <p>{comment.textarea}</p>
            </div>
        {/each}

        <form on:submit={handleSubmitForm} aria-labelledby="form-title">
            <h3 id="form-title">Ajouter un commentaire</h3>

            <label for="title">Titre</label>
            <input name="title" id="title" bind:value={commentTitle}/>

            <label for="rate">Note</label>
            <input name="rate" type="number" max="5" min="1" id="rate" bind:value={commentRate}/>

            <label for="comment">Texte du commentaire</label>
            <textarea name="comment" id="comment" cols="30" rows="10" bind:value={commentDesc}/>

            <input type="submit" value="Commenter" />
        </form>
    {:catch}
        Connectez-vous afin de voir les commentaires
        <Login />
    {/await}
{/await}
