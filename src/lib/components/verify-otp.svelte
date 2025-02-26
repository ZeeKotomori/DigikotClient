<script>
    import Input from "./input/Input.svelte";
    import { goto } from "$app/navigation";

    let otp = "";
    let email = "";
    let errorMessage = "";

    const gotoLogin = () => {
        goto("/login");
    };

    const gotoRegister = () => {
        goto("/register",  {replaceState: true});
    };

    const handleOtp = async () => {
        try {
            const response = await fetch("http://localhost:3000/api/v1/auth/verify-otp", {
                method: "POST",
                headers: {
                    "Content-Type" : "application/json",
                },
                body: JSON.stringify({ otp, email}),
            });
            
            const data = await response.json();

            if (!response.ok) {
                errorMessage = data.msg || "OTP atau Email salah gagal";
            } 
            else {
                gotoLogin();
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
    <div class="flex pl-4 flex-col items-start gap-2 h-1/12 mb-2">
        <h1 class="text-xl font-bold">Join us for Indonesia</h1>
        <p class="text-sm text-gray-500">Masukan OTP</p>
    </div>
    <div class="flex flex-col items-center justify-between h-10/12 gap-4">
        <div class="flex flex-col w-full gap-2 mb-2">
            <Input bind:value={email} className="bg-white text-black rounded-xl w-full text-lg" placeholder="Email" type="email" />
            <Input bind:value={otp} className="bg-white text-black rounded-xl w-full text-lg" placeholder="OTP" type="number" />
        {#if errorMessage}
            <p class="text-red-500 text-sm text-center mb-4">{errorMessage}</p>
        {/if}
        </div>
        <button on:click={handleOtp} class="justify-self-end bg-green-600 text-white px-4 py-2 rounded-xl text-lg w-full max-w-xs">Send OTP</button>
    </div>
</div>