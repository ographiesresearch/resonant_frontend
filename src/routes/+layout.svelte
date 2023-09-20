<script>
    import Navbar from '$lib/components/Navbar/Navbar.svelte';
    import site_data from '$lib/config/instance.json';
    import "$lib/styles/style.css";
    import favicon from '$lib/favicon.png';
</script>
<svelte:head>
  <!-- Google tag (gtag.js) --> <script async src="https://www.googletagmanager.com/gtag/js?id=G-QWSCPX7GJY"></script> <script> window.dataLayer = window.dataLayer || []; function gtag(){dataLayer.push(arguments);} gtag('js', new Date()); gtag('config', 'G-QWSCPX7GJY'); </script>
  <meta name="viewport" content="initial-scale=1,maximum-scale=1,user-scalable=no" />
  <meta charset="utf-8" />
  <link rel="icon shortcut" href={favicon}>
  <title>{site_data.title}</title>
</svelte:head>
<Navbar pages={site_data.pages}/>
<slot />