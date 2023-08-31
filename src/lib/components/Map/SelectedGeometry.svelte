<script>
    import { mapbox } from '$lib/scripts/utils';

    export let selected;
    export let lngLat;
    export let loadingState;

    async function addGeometry(ll) {
        const endpoint = 'https://api.mapbox.com/v4/ericrobskyhuntley.b3m8uynm/tilequery/'
        const f = 'json'
        const query = `${endpoint}${ll.lng},${ll.lat}.${f}?access_token=${mapbox.accessToken}`;
        loadingState = !loadingState;
        await fetch(query)
            .then((d) => {
                return d.json();
            })
            .then((d) => {
                d = (d.features.length > 0) ? d.features[0].properties : undefined;
                return d;
            })
            .then((d) => {
                selected = d;
                loadingState = !loadingState;
            })
    }
    
    $: (lngLat) ? addGeometry(lngLat) : null;
</script>