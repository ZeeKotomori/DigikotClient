<script>
    import Input from "./input/Input.svelte";
    import { goto } from "$app/navigation";
    import Btn from "./button/Btn.svelte";

    let email = "";
    let phoneNumber = "";
    let username = "";
    let name = "";
    let password = "";
    let confirmPassword = "";
    let errorMessage = "";

    const gotoLogin = () => {
        goto("/login");
    };

    const gotoStartUp = () => {
        goto("/");
    };

    const handleRegister = async () => {
        if(password !== confirmPassword) {
            errorMessage = "Password and Confirm Password must be the same";
            return;
        }

        try {
            const response = await fetch("http://localhost:3000/api/v1/auth/register", {
                method: "POST",
                headers: {
                    "Content-Type" : "application/json",
                },
                body: JSON.stringify({ email, username, name, phoneNumber, password, confirmPassword}),
            });
            
            const data = await response.json();

            if (!response.ok) {
                errorMessage = data.msg || "Registrasi gagal";
            } 
            else {
                alert("Registrasi berhasil! Silakan Isi Otp.");
                goto("/verify-otp");
            }
        } catch (error) {
            errorMessage = "Terjadi kesalahan, coba lagi.";
            console.error(error);
        }
    }

</script>

<div class="bg-white h-screen py-6 px-6">
    <div class="w-full h-1/12">
        <button on:click={gotoStartUp} class="text-4xl mb-5">&#8592;</button>
    </div>
    <div class="flex pl-4 flex-col items-start gap-2 h-1/12 mb-2">
        <h1 class="text-xl font-bold">Join us for Indonesia</h1>
        <p class="text-sm text-gray-500">we will send you the code</p>
    </div>
    <div class="flex flex-col items-center justify-between h-10/12 gap-4">
        <div class="flex flex-col w-full gap-2 mb-2">
            <Input bind:value={email} className="bg-white text-black rounded-xl w-full text-lg" placeholder="Email" type="email" />
            <Input bind:value={phoneNumber} className="bg-white text-black rounded-xl w-full text-lg" placeholder="Phone" type="number" />
            <Input bind:value={username} className="bg-white text-black rounded-xl w-full text-lg" placeholder="Username" type="text" />
            <Input bind:value={name} className="bg-white text-black rounded-xl w-full text-lg" placeholder="Name" type="text" />
            <Input bind:value={password} className="bg-white text-black rounded-xl w-full text-lg" placeholder="Password" type="password" />
            <Input bind:value={confirmPassword} className="bg-white text-black rounded-xl w-full text-lg" placeholder="Confirm Password" type="password" />
            <p class="text-center text-sm text-gray-600 mt-4">
                Sudah punya akun? <a on:click={gotoLogin} class="text-green-600">Login</a>
            </p>
            {#if errorMessage}
            <p class="text-red-500 text-sm text-center mb-4">{errorMessage}</p>
        {/if}
        </div>
        <Btn onClick={handleRegister} className="justify-self-end bg-green-600 text-white px-4 py-2 rounded-xl text-lg w-full max-w-xs" label="Continue With Phone"/>
    </div>
</div>