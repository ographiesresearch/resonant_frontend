<script>
    export let status;
    export let priority = false;
</script>

<div class="box block shadow {(status === true) ? 'has-background-success' : null } {(status === false) ? 'has-background-danger' : null } {(status === undefined) ? 'has-background-grey-darker' : null } {priority && status ? 'priority' : null}">
    <slot/>
</div>