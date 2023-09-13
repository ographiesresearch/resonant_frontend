<script>
    import { onDestroy, onMount, setContext } from 'svelte';
    import ForwardGeocoder from '$lib/components/Map/Geocoders/Forward.svelte';
    import ReverseGeocoder from '$lib/components/Map/Geocoders/Reverse.svelte';
    import SelectedGeometry from '$lib/components/Map/SelectedGeometry.svelte';
    import Marker from '$lib/components/Map/Marker.svelte';
    import Legend from '$lib/components/Map/Legend.svelte';
    import InfoPanel from '$lib/components/InfoPanel/InfoPanel.svelte';
    import RippleLoader from '$lib/components/RippleLoader.svelte';
    
    import colors from '$lib/styles/app.module.scss';
    import sources from '$lib/config/sources.json';

    import 'mapbox-gl/dist/mapbox-gl.css';

    import { mapbox, key } from '$lib/scripts/utils';

    import Device from 'svelte-device-info';
    let mobile;

    export let mapbox_token;
    mapbox.accessToken = mapbox_token;

    export let props;
    
    let container;
    let map;

    let gcResult = undefined;
    let selected = undefined;
    let marker = undefined;
    let lngLat = undefined;
    
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
    
    function colorPicker(color) {
        if (color === "primary") {
            color = colors.primary;
        } else if (color === "success") {
            color = colors.success;
        } else if (color === "danger") {
            color = colors.danger;
        }
        return color;
    }

    function addSources(sources) {
        Object.entries(sources).forEach((layer) => {
            const [name, p] = layer;
            map.addSource(name, {
                type: 'vector',
                url: 'mapbox://'.concat(p.source)
            });
            p.mapStyles.forEach((style) => {
                style.source = name;
                style["source-layer"] = p.layer;
                if (style.type === "line") {
                    style.paint["line-color"] = colorPicker(style.paint["line-color"])
                } else if (style.type === "fill") {
                    style.paint["fill-color"] = colorPicker(style.paint["fill-color"])
                }
                if (style?.shadow) {
                    map.addLayer({
                        id: style.id.concat("-shadow"),
                        source: name,
                        "source-layer": p.layer,
                        type: "line",
                        "line-join": "bevel",
                        "line-cap": "round",
                        paint: {
                            "line-color": "#000000",
                            "line-opacity": 1,
                            "line-width": 2,
                            "line-translate": [2,2]
                        }
                    },
                    "building-number-label"
                    )
                    map.setPaintProperty(
                        style.id.concat("-shadow"), 
                        'line-width', [
                            'interpolate',
                            ['linear', 0.5],
                            ['zoom'],
                            props.init.zoom[1],
                            1,
                            props.resultZoom,
                            3
                    ])
                }
                if (style.type === "line") {
                    map.addLayer(
                        style,
                        "building-number-label"
                    )
                } else {
                    map.addLayer(
                        style,
                        "waterway"
                    )
                }
            })
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

        // disable map rotation using right click + drag
        map.dragRotate.disable();
 
        // disable map rotation using touch rotation gesture
        map.touchZoomRotate.disableRotation();
        
        map.on ('load', () => {
            addSources(sources);
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
        <Legend bind:marker title="Income Tax Credit Eligibility"/>
    {/if}
</div>
{#if marker }
<InfoPanel bind:marker bind:gcResult bind:selected/>
{/if}

<style>
    .non-interactive {
        pointer-events: none;
    }
</style>