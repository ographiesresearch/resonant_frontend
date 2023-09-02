<script>
    export let status;
    export let priority = false;
    import { fade } from 'svelte/transition';
</script>

<div transition:fade = {{ duration: 600}} class="box block shadow {(status === true) ? 'has-background-success' : null } {(status === false) ? 'has-background-danger' : null } {(status === undefined) ? 'has-background-grey-darker' : null } {priority && status ? 'priority' : null}">
    <slot/>
</div>