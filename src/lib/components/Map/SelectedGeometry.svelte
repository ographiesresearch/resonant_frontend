<script>
    import { mapbox, post_15_nmtc } from '$lib/scripts/utils';

    export let selected;
    export let lngLat;
    export let loadingState;
    import sources from '$lib/config/sources.json';

    async function addGeometry(ll) {
        let queryLayer;
        Object.entries(sources).forEach((layer) => {
            const [name, p] = layer;
            if (p.queryLayer) {
                queryLayer = p.source
            }
        })
        const endpoint = `https://api.mapbox.com/v4/${queryLayer}/tilequery/`;
        const f = 'json'
        const query = `${endpoint}${ll.lng},${ll.lat}.${f}?access_token=${mapbox.accessToken}`;
        loadingState = !loadingState;
        await fetch(query)
            .then((d) => {
                return d.json();
            })
            .then((d) => {
                d = (d.features.length > 0) ? d.features[0].properties : undefined;
                if (post_15_nmtc) {
                    if (d?.deprec && !(d?.nrg_comm || d?.native) ) {
                        d = undefined;
                    }
                }
                return d;
            })
            .then((d) => {
                selected = d;
                loadingState = !loadingState;
            })
    }
    
    $: (lngLat) ? addGeometry(lngLat) : null;
</script>