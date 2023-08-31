<script>
    import WelcomeButton from '$lib/components/WelcomeButton.svelte';
    import { onMount } from 'svelte';
    import { slide, fade } from 'svelte/transition';

    export let authors;
    export let orgs;

    export let title = "Title";
    export let subtitle = "Longer description";
    export let description = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Ut volutpat, augue vitae tempus porta, nulla ipsum ultrices quam, eget rutrum nisl lorem sit amet orci. Morbi laoreet, risus non congue cursus, mi sem tincidunt est, in condimentum tortor nisi quis lectus. Aliquam vel tellus auctor sapien ornare vulputate eu pulvinar ligula. Donec gravida erat et ornare commodo. Proin commodo turpis eget gravida lacinia. Aliquam mollis interdum ex, in porttitor justo dictum vitae. Donec viverra libero non suscipit mattis. Praesent ut risus et nibh volutpat suscipit vel rhoncus mauris. Ut quis commodo lacus. Maecenas eu metus venenatis felis egestas varius. Integer vehicula sed dolor non volutpat. Sed aliquam sit amet enim vel efficitur."
    let loadState = false;
    onMount(() => loadState = true);
</script>

{#if loadState}
<section transition:fade = {{ duration: 1000}} class="hero hero-solar is-fullheight-with-navbar">
    <div class="container">
        <div class="hero-body">
            <div class="">
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
            </div>
        </div>
    </div>
</section>
{/if}