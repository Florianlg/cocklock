<script>
    import Router from 'svelte-spa-router';

    import Space from "./lib/routes/Space.svelte";
    import Spaces from './lib/routes/Spaces.svelte';
    import Login from './lib/routes/Login.svelte';
    import Home from './lib/routes/Home.svelte';
    import Header from './lib/Header.svelte';
    import NotFound from './lib/routes/NotFound.svelte';

    const routes = {
      '/': Home,
      '/login': Login,
      '/espaces': Spaces,
      '/espaces/:id': Space,
      '*': NotFound
    };

</script>
<Header/>
<main aria-labelledby="title1">
    <Router {routes}/>
</main>