<script>
    import LowIncome from '$lib/components/InfoPanel/LowIncome.svelte';
    import EnergyCommunities from '$lib/components/InfoPanel/EnergyCommunities.svelte';
    import IconStatus from '$lib/components/IconStatus.svelte';

    import { slide } from 'svelte/transition';

    export let gcResult;
    export let selected;
    export let marker;

    function closeInfo() {
        marker.remove();
        marker = undefined;
        add = undefined;
        selected = undefined;
    }
    
    function contextSearch(context, string) {
        let results;
        try {
            results = context.filter(function(entry) {
                return entry.id.includes(string);
            })[0].text;
        } catch (exception) {
            results = null
        }
        return results;
    }

    function parseContext(gcr){
        let address;
        if ('place_type' in gcr) {
            if (gcr.place_type.includes("poi") & 'address' in gcr) {
                address = gcr.address;
            } else if (gcr.place_type.includes("poi")) {
                address = gcr.text;
            } else if (gcr.place_type.includes("address")) {
                if (gcr.address !== null ) {
                    address = gcr.address.concat(" ", gcr.text);
                } else {
                    address = gcr.text;
                }
            } else {
                address = null;
            }
        } else {
            address = null;
        }
        let c = gcr.context;
        return {
            "address": (address) ? address : null,
            "muni": contextSearch(c , "place"),
            "state": contextSearch(c, "region"),
            "county": contextSearch(c, "district"),
            "zip": contextSearch(c, "postcode")
        }
    }

$: add = (gcResult) ? parseContext(gcResult) : undefined;

$: li_native = selected?.low_inc | selected?.native;
$: li_native_pct = (li_native) ? 10 : 0;

$: priority = selected?.pp | selected?.nrg_disadv;

$: nrg_comm_pct = (selected?.nrg_comm) ? 10 : 0;
$: qualifying_pct = li_native_pct + nrg_comm_pct;

</script>

{#if add || selected }
<div transition:slide|global = {{ duration: 500 }} class="info-box columns p-3">
    <div class="column is-offset-three-fifths is-two-fifths">
        <div id="address" class="box block shadow sticky-top">
            <button on:click={closeInfo} class="button span tag icon-text is-danger shadow is-medium block">
                <span>Close</span>
            </button>
                <div>
                {#if add?.address}
                    <p class="title">{add.address}</p>
                {/if}
                {#if add?.muni}
                    <p class="{(add.address) ? "subtitle" : "title"}">{add.muni}, {#if add.state}{add.state}{/if} {#if add.zip}{add.zip}{/if}</p>
                {/if}
                <div class="buttons has-text-white">
                <span class="button-passive button icon-text shadow block
                    {li_native  ? 'has-background-success' : 'has-background-danger'}
                    {priority ? 'priority' : null}">
                        <IconStatus status={selected?.li_native}/>
                        <span>Low-Income or Native Land</span>
                </span>
                <span class="button-passive button icon-text shadow block
                    {selected?.nrg_comm ? 'has-background-success' : 'has-background-danger'}">
                        <IconStatus status={selected?.nrg_comm}/>
                        <span>Energy Community</span>
                </span>
                </div>
                <div class="block box">
                    {#if qualifying_pct > 0}
                        This site is eligible for up to <code class="has-background-success has-text-white">{qualifying_pct}%</code> additional income tax credit on top of the base <code class="has-text-grey-darker">30%</code> credit.
                    {:else}
                        This site is <span class="has-text-white has-background-danger pl-2 pr-2 pt-1 pb-1">not eligible</span> for additional income tax credit on top of the base <code class="has-text-grey-darker">30%</code>.
                    {/if}
                    {#if priority }
                        It meets the IRS's <span class="has-text-grey-darker priority pl-2 pr-2 pt-1 pb-1">alternative selection criteria</span>.
                    {/if}
                </div>
                </div>
                <!-- <Tabs {items}/> -->
        </div>
        {#if selected}
        <div>
            <LowIncome
                data={selected}
                priority={priority}
            />
            <EnergyCommunities
                data={selected}
            />
        </div>
        {/if}
    </div>
</div>

{/if}
<style  lang="scss">
    #address {
        z-index: 40;
    }
    
    div {
        z-index: 20;
    }
    .button-passive {
        pointer-events: none;
    }
</style>