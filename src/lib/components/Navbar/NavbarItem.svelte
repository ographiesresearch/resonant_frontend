<script>
    export let name;
    export let path;
    function handleClick() {
      document.getElementById("navMenu").classList.toggle('is-active');
    }
</script>
<a on:click={handleClick} href="{path}" class="navbar-item has-background-dark has-text-weight-bold is-capitalized">
    <span class="tag is-size-6 has-background-dark has-text-white">
    {name}
    </span>
</a>