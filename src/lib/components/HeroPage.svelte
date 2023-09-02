<script>
    import { onMount } from 'svelte';
    import { fade, slide } from 'svelte/transition';
    let loadState = false;
    export let background;
    export let title;
    export let subtitle;
    onMount(() => loadState = true);
</script>


{#if loadState}
<section transition:fade = {{ duration: 1000}} class="hero {(background) ? 'hero-bg-image' : null} is-fullheight-with-navbar">
    <div class="container">
        <div class="hero-body">
            <div class="">
                {#if title}
                <p transition:slide = {{ duration: 800}} class="title p-3 box has-background-dark has-text-primary is-size-1 shadow">
                    {title}
                </p>
                {/if}
                {#if subtitle}
                <p class="subtitle p-3 mt-1 box has-background-dark has-text-primary is-size-2 shadow">
                    {subtitle}
                </p>
                {/if}
                <slot/>
            </div>
        </div>
    </div>
</section>
{/if}