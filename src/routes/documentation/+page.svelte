<script>
    import {marked} from 'marked';
    import { onMount } from 'svelte';

    import Card from '$lib/components/Card.svelte';
    import GitHubIcon from '$lib/components/GitHubIcon.svelte';
    import variables from '$lib/config/variables.json';
    import site_data from '$lib/config/instance.json';
    import geographies from '$lib/config/geographies.json';
    let vars = [];

    Object.entries(variables).forEach((v) => {
        let d = v[1]
        let geos = [];
        d.description = marked(d.description);
        if(typeof(d.geographies[0]) === "string") {
            d.geographies.forEach((g) => {
                geos.push(geographies[g]);
            });
            d.geographies = geos;
        } 
        d.citation = marked(d.citation);
        d.description = marked(d.description);
        vars.push(d);
    });
</script>

{#if site_data.licensing && site_data.repos}
<p class="subtitle p-3 mt-1 has-background-dark has-text-primary is-size-2 box shadow">
    Code Availability
</p>
<Card>
    <div class="has-text-white">
    {site_data.licensing}
    {#if site_data.repos}
    <div class="mt-2 columns">
    {#each site_data.repos as repo}
    <div class="column repo">
        <div class="block box shadow">
            <span class="icon-text">
                <span class="icon mr-5 is-medium">
                    <GitHubIcon/>
                </span>
                <span>
                    <div class="has-text-weight-bold">
                        <a href="{repo.url}">{repo.name}</a>
                    </div>
                    <div>
                        <a href="https://github.com/{repo.owner}/">@{repo.owner}</a>
                    </div>
                </span>
            </span>
            <div>{repo.description}</div>
        </div>
    </div>
    {/each}
    </div>
    {/if}
    </div>
</Card>
{/if}
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