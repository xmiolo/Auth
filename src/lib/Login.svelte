<script lang="ts">
  import { currentUser, pocketbase } from "./pocketbase";

    let username: string;
    let password: string;

    async function login() {
        await pocketbase.collection('users').authWithPassword(username, password);
    }   

    async function signUp() {
        try{
        const data = {
            username,
            password,
            passwordConfirm: password,
            name: 'NOME EMPTY',
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

        <button on:click={signUp}>Sign Up</button>
        <button on:click={login}>Login</button>
    </form>
{/if}