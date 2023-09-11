<script>
    import Navbar from '$lib/components/Navbar/Navbar.svelte';
    import site_data from '$lib/config/instance.json';
    import "$lib/styles/style.css";
    import favicon from '$lib/favicon.png';
</script>
<svelte:head>
  <link rel="icon shortcut" href={favicon}>
</svelte:head>
<title>{site_data.title}</title>
<Navbar pages={site_data.pages}/>
<slot />