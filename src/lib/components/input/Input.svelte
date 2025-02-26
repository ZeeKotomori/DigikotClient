<script>
    export let type = "text";
    export let placeholder = "";
    export let value = "";
    export let className = "";
    export let disabled = false;

    import { createEventDispatcher } from "svelte";
    const dispatch = createEventDispatcher();

    const handleInput = (event) => {
        value = event.target.value;
        dispatch("input", value);
    };
</script>

<input
    type={type}
    placeholder={placeholder}
    bind:value
    class="px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500 focus:border-blue-500 transition disabled:bg-gray-200 disabled:cursor-not-allowed {className}"
    on:input={handleInput}
    {disabled}
/>
