<script>
    import HeroPage from '$lib/components/HeroPage.svelte';
    import WelcomeButton from '$lib/components/WelcomeButton.svelte';

    export let authors;
    export let orgs;

    export let background = true;
    export let title = "Title";
    export let subtitle = "Longer description";
    export let description = "Lorem ipsum dolor sit amet, consectetur adipiscing elit."
</script>

<HeroPage {title} {background} {subtitle}>
    <div class="columns">
        <div class="column">
            <p class="p-3 box mb-2 has-background-dark has-text-white shadow">
                {description}
            </p>
            <WelcomeButton/>
        </div>
        <div class="column">
            <div class="box has-background-dark p-3 has-text-white mb-2 shadow">
                <span class="is-italic">This tool is provided by...</span>
                <ul>
                    {#each orgs as org}
                        <li><a href="{org.url}">{org.name}</a>{#if org.description}, {org.description}.{/if}</li>
                    {/each}
                </ul>
            </div>
            <div class="box has-background-dark p-3 has-text-white shadow">
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