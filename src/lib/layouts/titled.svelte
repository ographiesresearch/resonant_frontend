<script>
    import HeroPage from '$lib/components/HeroPage.svelte';
    // export let date
</script>
<HeroPage>
    <!-- <h1 class="title">{title}</h1> -->
    <!-- <hr> -->
    <div class = "has-background-dark p-3 has-text-white">
        <slot />
    </div>
</HeroPage>