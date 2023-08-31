<script>
    export let title;
    // export let date
</script>
<section class="hero-bg-image">
<div class = "container">
    <h1 class="title">{title}</h1>
    <hr>
    <div class = "section">
        <slot />
    </div>
</div>
</section>