<script>
    import {marked} from 'marked';
    import Card from '$lib/components/Card.svelte';
    import variables from '$lib/config/variables.json';
    import geographies from '$lib/config/geographies.json';
    let vars = []
    Object.entries(variables).forEach((v) => {
        let d = v[1]
        d.description = marked(d.description);
        let geos = [];
        console.log(d.geographies);
        d.geographies.forEach((geo) => {
            geos.push(geographies[geo]);
        });
        d.geographies = geos;
        d.citation = marked(d.citation);
        d.description = marked(d.description);
        vars.push(d);
    });
</script>

<p class="subtitle p-3 mt-1 has-background-dark has-text-primary is-size-2 box shadow">
    Code Availability
</p>

<Card>
    <p class="has-text-white">The income ratio is defined as median family income in the past 12 months divided by the larger of the state and the Metropolitan Statistical Area MFIs.</p>
</Card>

<p class="subtitle p-3 mt-1 has-background-dark has-text-primary is-size-2 box shadow">
    Data Dictionary
</p>
{#each vars as v}
<Card>
    <div class="has-text-white">
    <p class="title has-text-white">
        {v.name}
    </p>
    <div class="columns">
        <div class="column data-source">
            <div class="block box shadow">
                <p class="has-text-weight-bold">Data Source</p>
                {#if v.tables} 
                    {#each v.tables as t}
                    <p><a href="{t.url}">{t.id}</a></p>
                    {/each}
                {/if}
                <p>{#if v.url}
                    <a href="{v.url}">{v.source}</a>
                {:else}
                    {v.source}
                {/if}</p>
                <p>{v.dates}</p>
            </div>
        </div>
        {#if v.geographies}
        <div class="column geographies">
            <div class="block box shadow">
                <p  class="has-text-weight-bold">Geography</p>
                {#each v.geographies as geo}
                    <p>{geo.name}</p>
                    <p>{geo.vintage}</p>
                {/each}
            </div>
        </div>
        {/if}
    </div>
    <p>{@html v.description}</p>
    <hr>
    <p class="is-size-7">{@html v.citation}</p>
    </div>
</Card>
{/each}