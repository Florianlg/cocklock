<script>
    import {link} from 'svelte-spa-router';
import logo from '../assets/coclockworking.png';

const nav_links = [
    {
        "label": "Page d'accueil du site",
        "text": "Accueil",
        "url": "/"
    },
    {
        "label": "Page de réservation de salles",
        "text": "Réservation",
        "url": "/espaces"
    },
    {
        "label": "Formulaire de connexion au site",
        "text": "Se connecter",
        "url": "/login"
    },
    {
        "label": "Formulaire d'inscription au site",
        "text": "S'enregistrer",
        "url": "/signin"
    },
];
</script>


<nav aria-label="header navigation">
    <a href="/" class="logo" title="Co'Clock Working" aria-label="accueil du site">
        <img src="{logo}" alt="Logo de l'entreprise">
    </a>

    <ul id="menu2" class="links" role="menu" aria-labelledby="menubutton">
      {#each nav_links as nav}
        <li role="none">
            <a role="menuitem" aria-label="{nav.label}" href="{nav.url}" use:link>{nav.text}</a>
        </li>
      {/each}
    </ul>

    <button type="button"
        id="menubutton"
        aria-haspopup="true"
        aria-controls="menu2">
        <i class="fa-solid fa-bars"></i>
    </button>
</nav>