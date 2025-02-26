<script>
    import Input from "./input/Input.svelte";
    import { goto } from "$app/navigation";

    let password = "";
    let email = "";
    let errorMessage = "";

    const gotoMaps = () => {
        goto("/maps");
    };

    const gotoRegister = () => {
        goto("/register",  {replaceState: true});
    };

    const handleLogin = async () => {
        try {
            const response = await fetch("http://localhost:3000/api/v1/auth/login", {
                method: "POST",
                headers: {
                    "Content-Type" : "application/json",
                },
                body: JSON.stringify({ password, email}),
            });
            
            const data = await response.json();

            if (!response.ok) {
                errorMessage = data.msg || "Email atau Password salah";
            } 
            else {
                gotoMaps();
            }
        } catch (error) {
            errorMessage = "Terjadi kesalahan, coba lagi.";
            console.error(error);
        }
    }

</script>

<div class="bg-white h-screen py-6 px-6">
    <div class="w-full h-1/12">
        <button on:click={gotoRegister} class="text-4xl mb-5">&#8592;</button>
    </div>
    <div class="flex pl-4 flex-col items-start gap-2 h-1/12">
        <h1 class="text-xl font-bold">Login</h1>
    </div>
    <div class="flex flex-col items-center justify-between h-10/12 gap-4">
        <div class="flex flex-col w-full gap-2 mb-2">
            <Input bind:value={email} className="bg-white text-black rounded-xl w-full text-lg" placeholder="Email" type="email" />
            <Input bind:value={password} className="bg-white text-black rounded-xl w-full text-lg" placeholder="Password" type="password" />
        {#if errorMessage}
            <p class="text-red-500 text-sm text-center mb-4">{errorMessage}</p>
        {/if}
        </div>
        <button on:click={handleLogin} class="justify-self-end bg-green-600 text-white px-4 py-2 rounded-xl text-lg w-full max-w-xs">Login</button>
    </div>
</div>