<script>
    import { push } from "svelte-spa-router";

    let email;
    let pwd;

    const handleSubmit = async (event) => {
        event.preventDefault();

        const token = await login();
        localStorage.setItem('token', token);
        push('/');
    }

    const login = async () => {
        const response = await fetch(import.meta.env.VITE_URL_DIRECTUS + "/auth/login", {
            method: "POST",
            headers: {
                "content-type": "application/json"
            },
            body: JSON.stringify({
                "email": email,
                "password": pwd
            })
        })
        const json = await response.json();
        return json.data.access_token;
    }
</script>

<main aria-labelledby="title1">
    <h2 id="title1">Se connecter</h2>


    <form on:submit={handleSubmit} aria-label="Informations de connexion">


        <label for="email">Email</label>
        <input required type="email" name="email" id="email" placeholder="ex : m.dupont@monmail.com" bind:value={email}>


        <label for="password">Mot de passe</label>
        <input required type="password" name="password" id="password" placeholder="***********" bind:value={pwd}>


        <input class="sub" type="submit" value="Se connecter">


    </form>
    
</main>