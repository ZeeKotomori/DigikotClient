<script>
  import { goto } from "$app/navigation";

  let username = "";
  let name = "";
  let email = "";
  let password = "";
  let confirmPassword = "";
  let errorMessage = "";

  const gotoLogin = () => {
    goto("/login");
  };

  const handleRegister = async () => {
    if (!email || !password || !username || !name) {
      errorMessage = "Username, Name, Email, dan password harus diisi!";
      return;
    }

    try {
      const response = await fetch("http://localhost:3000/api/v1/auth/register", {
        method: "POST",
        headers: {
          "Content-Type" : "application/json",
        },
        body: JSON.stringify({ email, username, name, password, confirmPassword}),
      })

      if (!response.ok) {
        throw new Error(data.message || "Register gagal");
      } else {
        goto("/login", {replaceState: true});
      }
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

    <form on:submit|preventDefault={handleRegister}>
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
        <label class="block text-gray-700 text-sm font-medium mb-1">Username</label>
        <input
          type="text"
          bind:value={username}
          placeholder="Masukkan username"
          class="w-full px-4 py-2 border rounded-lg focus:ring-2 focus:ring-green-600 focus:outline-none"
        />
      </div>

      <div class="mb-4">
        <label class="block text-gray-700 text-sm font-medium mb-1">Name</label>
        <input
          type="text"
          bind:value={name}
          placeholder="Masukkan name"
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

      <div class="mb-4">
        <label class="block text-gray-700 text-sm font-medium mb-1">Confirm Password</label>
        <input
          type="password"
          bind:value={confirmPassword}
          placeholder="Masukkan email"
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
        Sudah punya akun? <a on:click={gotoLogin} class="text-green-600">Login</a>
      </p>
  </div>
</div>
