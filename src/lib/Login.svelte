<script lang="ts">
    import { currentUser, pocketbase } from "./pocketbase";
    let username: string;
    let password: string;
    let name: string;
    let isSignUpVisible = false;
    
    async function login() {
      await pocketbase.collection('users').authWithPassword(username, password);
    }
    async function signUp() {
      try{
        const data = {
          username,
          password,
          passwordConfirm: password,
          name,
        };
        const createdUser = await pocketbase.collection('users').create(data);
        await login();
      } catch (err) {
        console.log(err);
      }
    }
    function signOut(){
      pocketbase.authStore.clear();
    }
  </script>
  {#if $currentUser}
    <h1>Logged in as {$currentUser.username}</h1>
    <button on:click={signOut}>Logout</button>
  {:else}
    {#if isSignUpVisible}
      <form on:submit|preventDefault>
        <input
          placeholder="username"
          type="text"
          bind:value={username}
        />
        <input
          placeholder="password"
          type="password"
          bind:value={password}
        />
        <input
          placeholder="name"
          type="text"
          bind:value={name}
        />
        <button on:click={signUp}>Sign Up</button>
        <button on:click={() => isSignUpVisible = false}>Back to Login</button>
      </form>
    {:else}
      <form on:submit|preventDefault>
        <input
          placeholder="username"
          type="text"
          bind:value={username}
        />
        <input
          placeholder="password"
          type="password"
          bind:value={password}
        />
        <button on:click={login}>Login</button>
        <button on:click={() => isSignUpVisible = true}>Sign Up</button>
      </form>
    {/if}
  {/if}