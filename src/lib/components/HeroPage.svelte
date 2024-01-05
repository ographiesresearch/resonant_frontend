<script>
    import { onMount } from 'svelte';
    import { fade } from 'svelte/transition';
    import bg from '$lib/background.jpg';
    export let background;
    export let title;
    export let subtitle;
    let loadState = false;
    onMount(() => loadState = true);
</script>

<section class="hero {(background) ? 'hero-bg-image' : null} is-fullheight-with-navbar"
    style={(background) ? `background-image: url(${bg});` : null}>
    <div class="container">
        <div class="hero-body">
            <div class="">
                {#if title && loadState}
                <p transition:fade = {{ duration: 200 }} class="title p-3 box has-background-dark has-text-primary is-size-1 is-size-2-mobile shadow">
                    {title}
                </p>
                {/if}
                {#if subtitle && loadState}
                <p transition:fade = {{ duration: 200, delay: 50 }} class="subtitle p-3 mt-1 box has-background-dark has-text-primary is-size-2 is-size-3-mobile shadow">
                    {subtitle}
                </p>
                {/if}
                {#if loadState}
                <div transition:fade = {{ duration: 200, delay: 100 }}>
                    <slot/>
                </div>
                {/if}
            </div>
        </div>
    </div>
</section>
