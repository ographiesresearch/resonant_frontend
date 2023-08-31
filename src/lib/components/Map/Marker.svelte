<script>
    import { getContext } from 'svelte';
    import { mapbox, key } from '$lib/scripts/utils';
    import colors from '$lib/styles/app.module.scss';

    const { getMap } = getContext(key)
    const map = getMap()

    export let lngLat;
    export let marker;

    function addMarker(ll) {
        (marker !== undefined) ? marker.remove() : null;
        if (ll !== undefined) {
            marker = new mapbox.Marker({
                    color: colors.primary
                }).setLngLat(ll);
            marker.addTo(map);
        }
    }

    $: addMarker(lngLat);
</script>