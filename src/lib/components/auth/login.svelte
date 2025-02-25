<script>
  import { onMount } from "svelte";
  import { goto } from "$app/navigation";

  let email = "";
  let password = "";
  let errorMessage = "";
  let successMessage = "";

  const gotoRegister = () => {
    goto("/register");
  };

  const handleLogin = async () => {
    errorMessage = "";
    successMessage = "";

    if (!email || !password) {
      errorMessage = "Email dan password harus diisi!";
      return;
    }

    try {
      const response = await fetch("http://localhost:3000/api/v1/auth/login", {
        method: "POST",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ email, password }),
      });

      const data = await response.json();

      if (!response.ok) {
        throw new Error(data.message || "Login gagal");
      }

      successMessage = "Login berhasil!";
      console.log("Token:", data.token);
    } catch (error) {
      errorMessage = error.message;
    }
  };
</script>

<div class="min-h-screen flex items-center justify-center bg-gray-100">
  <div class="w-full max-w-md bg-white p-8 shadow-lg rounded-xl">
    <h2 class="text-2xl font-semibold text-green-600 text-center mb-6">
      Login
    </h2>

    {#if errorMessage}
      <p class="text-red-500 text-sm text-center mb-4">{errorMessage}</p>
    {/if}

    {#if successMessage}
      <p class="text-green-500 text-sm text-center mb-4">{successMessage}</p>
    {/if}

    <form on:submit|preventDefault={handleLogin}>
      <div class="mb-4">
        <label class="block text-gray-700 text-sm font-medium mb-1">Email</label>
        <input
          type="email"
          bind:value={email}
          placeholder="Masukkan email"
          class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-green-600 focus:outline-none"
        />
      </div>

      <div class="mb-4">
        <label class="block text-gray-700 text-sm font-medium mb-1">Password</label>
        <input
          type="password"
          bind:value={password}
          placeholder="Masukkan password"
          class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-green-600 focus:outline-none"
        />
      </div>

      <button
        type="submit"
        class="w-full bg-green-600 text-white py-2 rounded-lg hover:bg-green-700 transition"
      >
        Masuk
      </button>
    </form>

    <p class="text-center text-sm text-gray-600 mt-4">
      Belum punya akun? <a on:click={gotoRegister} class="text-green-600">Daftar</a>
    </p>
  </div>
</div>
