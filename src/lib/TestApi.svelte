<script>
    // fonction d'initialisation du composant
    const init = async () => {
        const spaces = await get_spaces()
        console.log("spaces :", spaces);

        const token = await login();
        console.log("token :", token);

        const comments = await get_comments(token);
        console.log("comments :", comments);
    }


    const get_spaces = async () => {
        // Appel de la requête
        const response = await fetch(import.meta.env.VITE_URL_DIRECTUS + "/items/Spaces");
        // Extraction du json de la réponse
        const result = await response.json();
        //Extraction et retour de la liste
        return result.data;
    }

    // Fonction de récupération d'un token de connexion
    const login = async () => {
        // Appel de la requête
        const response = await fetch(import.meta.env.VITE_URL_DIRECTUS + "/auth/login", {
            method: "POST",
            headers: {
                "Content-type": "application/json"
            },
            body: JSON.stringify({
                email: "client@oclock.io",
                password: "1234"
            })
        });
        // Extraction du json de la réponse
        const result = await response.json();
        //Extraction et retour du token
        return result.data.access_token;
    }

    // Fonction de récupération de la liste des commentaires
    const get_comments = async (token) => {
        // Appel de la requête
        const response = await fetch(import.meta.env.VITE_URL_DIRECTUS + "/items/Comments", {
            headers: {          
                'Authorization': 'Bearer ' + token,
            }
        });
        // Extraction du json de la réponse
        const result = await response.json();
        //Extraction et retour de la liste
        return result.data;
    }

    init();
</script>