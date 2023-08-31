<script>
    import { onMount } from 'svelte';
    import { fade } from 'svelte/transition';
    let loadState = false;
    onMount(() => loadState = true);
</script>


{#if loadState}
<section transition:fade = {{ duration: 1000}} class="hero hero-bg-image is-fullheight-with-navbar">
    <div class="container">
        <div class="hero-body">
            <div class="">
                <slot/>
            </div>
        </div>
    </div>
</section>
{/if}