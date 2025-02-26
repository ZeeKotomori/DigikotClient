<script>
    import BtnMove from "./button/BtnMove.svelte";
    import { goto } from "$app/navigation";
</script>

<style>
    .image {
        width: 200px;
        height: auto;
        margin-bottom: 40px;
    }
</style>

<div class="container flex flex-col items-center justify-between bg-white text-center h-screen py-6 px-10">
    <img src="/Digikot_Horizontal_Logo.svg" alt="Digikot Logo" class="w-20 h-auto mb-5" />
    <img src="/Illustration_Angkot.png" alt="Angkot Illustration" class="image" />
    <BtnMove to="/register" label="Continue With Phone" className="bg-green-600 text-white px-4 py-2 rounded-xl text-lg w-full max-w-xs"/>
</div>