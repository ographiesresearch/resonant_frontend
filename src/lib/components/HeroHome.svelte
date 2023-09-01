<script>
    import HeroPage from '$lib/components/HeroPage.svelte';
    import WelcomeButton from '$lib/components/WelcomeButton.svelte';
    import { slide } from 'svelte/transition';

    export let authors;
    export let orgs;

    export let title = "Title";
    export let subtitle = "Longer description";
    export let description = "Lorem ipsum dolor sit amet, consectetur adipiscing elit."
</script>

<HeroPage>
    <p transition:slide = {{ duration: 800}} class="title p-3 has-background-dark has-text-primary is-size-1 shadow">
        {title}
    </p>
    <p class="subtitle p-3 mt-1 has-background-dark has-text-primary is-size-2 shadow">
        {subtitle}
    </p>
    <div class="columns">
        <div class="column">
            <p class="p-3 mb-2 has-background-dark has-text-white shadow">
                {description}
            </p>
            <WelcomeButton/>
        </div>
        <div class="column">
            <div class="has-background-dark p-3 has-text-white mb-2 shadow">
                <span class="is-italic">This tool is provided by...</span>
                <ul>
                    {#each orgs as org}
                        <li><a href="{org.url}">{org.name}</a>{#if org.description}, {org.description}.{/if}</li>
                    {/each}
                </ul>
            </div>
            <div class="has-background-dark p-3 has-text-white shadow">
                <span class="is-italic">Built by...</span>
                <ul>
                    {#each authors as author}
                        <li><a href="{author.url}">{author.name}</a>, {author.role}</li>
                    {/each}
                </ul>
            </div>
        </div>
    </div>
</HeroPage>