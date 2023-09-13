<script>
    import colors from '$lib/styles/app.module.scss';
    export let title = "Legend";
    export let marker;
    import { fade } from 'svelte/transition';
    let visible = true;
    import Device from 'svelte-device-info';
    let mobile = Device.isPhone;
    $: (marker && mobile) ? visible = false : visible = true;
</script>

{#if visible}
<div id="legend" transition:fade = {{ duration: 600}} class="legend has-background-white shadow">
    <p class="is-size-7 has-text-weight-bold mb-2">{title}</p>
    <div class="is-size-7">
    <div><span style="outline-color: {colors.primary}; outline-width: 2px"></span>Alternative Selection Criteria</div>
    <div><span style="background-color: {colors.success}"></span>20% <em class="has-text-grey-light">(Energy and Low-Income)</em></div>
    <div><span style="background-color: {colors.success}80"></span>10% <em  class="has-text-grey-light">(Energy or Low-Income)</em></div>
    </div>
</div>
{/if}

<style>
    .legend {
        border-radius: 3px;
        bottom: 40px;
        box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
        font: 12px/20px 'Helvetica Neue', Arial, Helvetica, sans-serif;
        padding: 10px;
        position: absolute;
        left: 10px;
        z-index: 1;
        /* Copied from mapboxgl-ctrl-geocoder */
        /* width: 33.3333%; */
        max-width: 360px;
        padding: 1rem;
    }
     
    .legend div span {
        /* border-radius: 0%; */
        outline-color:white;
        outline-style: solid;
        outline-width: 1px;
        display: inline-block;
        height: 10px;
        margin-right: 5px;
        width: 10px;
    }
</style>
    