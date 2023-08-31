<script>
    import { onDestroy, onMount, setContext } from 'svelte';
    import ForwardGeocoder from '$lib/components/Map/Geocoders/Forward.svelte';
    import ReverseGeocoder from '$lib/components/Map/Geocoders/Reverse.svelte';
    import SelectedGeometry from '$lib/components/Map/SelectedGeometry.svelte';
    import Marker from '$lib/components/Map/Marker.svelte';
    import InfoPanel from '$lib/components/InfoPanel/InfoPanel.svelte';
    import RippleLoader from '$lib/components/RippleLoader.svelte';
    
    import colors from '$lib/styles/app.module.scss';

    import 'mapbox-gl/dist/mapbox-gl.css';

    import { mapbox, key } from '$lib/scripts/utils';

    import Device from 'svelte-device-info';
    let mobile;

    export let mapbox_token;
    mapbox.accessToken = mapbox_token;

    export let props;
    
    let container;
    let map;

    // let lngLat;
    let gcResult;
    let selected;
    let marker;
    let lngLat;
    
    let loadingState = true;

    setContext(key, {
        getMap: () => map
    });

    function flyToLngLat(ll){
        map.flyTo({
            center: ll,
            zoom: (map.getZoom() > props.resultZoom) ? map.getZoom() : props.resultZoom,
            duration: props.resultFlyDur,
            essential: true
        });
    }

    $: (lngLat) ? flyToLngLat(lngLat) : null;

    onMount(() => {
        mobile = Device.isPhone;
        let mapOptions = {
            container: container,
            style: props.style,
            center: props.init.lngLat,
            zoom: (props.init.zoom.length === 2) ? props.init.zoom[0] : props.init.zoom,
            projection: props.projection,
            maxBounds: props.maxBounds
        }
        map = new mapbox.Map(mapOptions);
        
        map.on ('load', () => {
            map.addSource('adders', {
                type: 'vector',
                url: 'mapbox://ericrobskyhuntley.3tqai42w'
            });
            map.addSource('choropleth', {
                type: 'vector',
                url: 'mapbox://ericrobskyhuntley.b3m8uynm'
            })
            map.addLayer(
                {
                    id: 'choropleth-fill',
                    type: 'fill',
                    source: 'choropleth',
                    'source-layer': 'resonant_overlay-3wot0h',
                    paint: {
                        'fill-opacity': {
                            property: 'how_many',
                            stops: [[1, 0.5], [2, 1]]
                        },
                        'fill-color': colors.success,
                        'fill-outline-color': 'white',
                    }
                }
            )
            map.addLayer(
                {
                    id: "adders-shadow",
                    source: "adders",
                    "source-layer": "adders_dissolved-2x5v3w",
                    type: "line",
                    "line-join": "bevel",
                    "line-cap": "round",
                    paint: {
                        "line-color": "#000000",
                        "line-opacity": 1,
                        "line-width": 2,
                        "line-translate": [2,2]
                    }
                }
            )
            map.addLayer(
                {
                    id: 'adders-outlines',
                    source: 'adders',
                    'source-layer': 'adders_dissolved-2x5v3w',
                    type: 'line',
                    paint: {
                        'line-color': colors.primary,
                        'line-width': 2,
                    }
                }
            )
            map.addLayer(
                {
                    id: 'adders-fill',
                    type: 'fill',
                    source: 'adders',
                    'source-layer': 'adders_dissolved-2x5v3w',
                    paint: {
                        'fill-color': colors.primary
                    }
                }
            )
            map.moveLayer('choropleth-fill', 'waterway');
            map.setPaintProperty('adders-fill', 'fill-opacity', [
                'interpolate',
                ['exponential', 0.5],
                ['zoom'],
                props.init.zoom[1],
                0,
                props.resultZoom,
                0.1
            ])
            map.setPaintProperty('adders-shadow', 'line-width', [
                'interpolate',
                ['linear', 0.5],
                ['zoom'],
                props.init.zoom[1],
                1,
                props.resultZoom,
                3
            ])
        })
        map.once('zoomend', () => {
            loadingState = !loadingState
            map.setMinZoom((props.init.zoom.length === 2) ? props.init.zoom[1] : props.init.zoom);
        });
        map.on('style.load', () => {
            map.on('click', (e) => {
                lngLat = e.lngLat;
            })
            map.setFog({
                range: [9,20],
                color: colors.primary,
                'high-color': colors.danger,
                'horizon-blend': 0.02, // default: .1
                'space-color': '#000000', 
                'star-intensity': 0.1
            })


            if (props.init.zoom.length === 2) {
                map.flyTo({
                    center: props.init.LngLat,
                    zoom: props.init.zoom[1],
                    duration: props.init.zoomDur,
                    essential: true
                });
            }
        });
    });

    onDestroy(() => {
        if (map) {
             map.remove()
        };
    });
</script>

<div id ="map" class={(selected !== undefined && mobile) ? 'non-interactive' : null} bind:this={container}>
    {#if map}
        <RippleLoader bind:loadingState />
        <Marker bind:lngLat bind:marker />
        <ReverseGeocoder bind:lngLat bind:gcResult />
        <ForwardGeocoder bind:lngLat bind:gcResult bind:selected />
        <SelectedGeometry bind:selected bind:lngLat bind:loadingState/>
    {/if}
</div>
<InfoPanel bind:marker bind:gcResult bind:selected/>

<style>
    .non-interactive {
        pointer-events: none;
    }
</style>