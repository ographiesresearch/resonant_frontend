<script>
    import HeroHome from '$lib/components/HeroHome.svelte';
    import site_data from '$lib/config/instance.json';
</script>

<HeroHome 
    title={site_data.title} 
    subtitle={site_data.subtitle} 
    description={site_data.description}
    authors={site_data.authors} 
    orgs ={site_data.orgs}
    />