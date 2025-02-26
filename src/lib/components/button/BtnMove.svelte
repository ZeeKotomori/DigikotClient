<script>
    import { goto } from "$app/navigation";
    
    export let to = "/";
    export let label = "";
    export let className = "";

    const navigate = () => {
        console.log("Navigating to", to);
        goto(to);
    }
</script>

<button class={className} on:click={navigate}>
    {label}
</button>