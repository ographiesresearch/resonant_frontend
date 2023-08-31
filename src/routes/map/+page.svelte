<script>
    import Map from '$lib/components/Map/Map.svelte';
    /** @type {import('./$types').PageData} */
    export let data
    import site_data from '$lib/config/instance.json';
</script>
<section>
    <Map 
        props={site_data.map}
        mapbox_token={data.mapbox_token}
    />
</section>