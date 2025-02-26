<script>
    export let label = "";
    export let className = "";
    export let onClick;

</script>

<button class={className} on:click={onClick}>
    {label}
</button>