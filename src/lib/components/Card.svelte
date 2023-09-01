<script>
    export let status;
    export let priority = false;
    import { fade } from 'svelte/transition';
</script>

<div transition:fade = {{ duration: 600}} class="box block has-background-{status ? 'success' : 'danger'} shadow {priority && status ? 'priority' : null}">
    <slot/>
</div>

<style lang="scss">
.priority {
    outline-style: solid;
    outline-width: 0.3rem;
    outline-offset: -0.3rem;
    outline-color: $primary;
}
</style>