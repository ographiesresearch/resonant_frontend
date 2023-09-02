<script>
    import HeroPage from '$lib/components/HeroPage.svelte';
    let title = "Documentation";
    let background = false;
</script>

<HeroPage {title} {background}>
    <slot/>
</HeroPage>