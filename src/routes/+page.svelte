<script>
import { createClient } from '@supabase/supabase-js'
import { onMount } from 'svelte';

// Create a single supabase client for interacting with your database
const supabase = createClient('https://uioaemhkajajdgxqskwv.supabase.co', 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InVpb2FlbWhrYWphamRneHFza3d2Iiwicm9sZSI6ImFub24iLCJpYXQiOjE2OTM4NzM5MjIsImV4cCI6MjAwOTQ0OTkyMn0.UhssrleA4-TDRaeAdWSGM3h2mEeFdjUN9VBprrQ5JyI')
let user;

onMount(async () => {
    user = await supabase.auth.getUser();
    console.log("onMount -> user:", user);

    if (user) {
        console.log("onMount -> user:", user);
    } else {
        user = null;
    }
});

async function getDataTest() {
    let { data, error } = await supabase
    .from('test1')
    .select('id, name');

    if(data) {
        console.log("data:", data);
    }

    if(error) {
        console.log("error:", error);
    }
}

async function addDataTest() {

const { data, error } = await supabase
  .from('test1')
  .insert([
    { name: 'Superman' },
  ])
  .select()

    if(data) {
        console.log("data:", data);
    }
    
    if(error) {
        console.log("error:", error);
    }

}

async function signUpTest() {
    let { data, error } = await supabase.auth.signUp({
        email: 'kangan.dtp@gmail.com',
        password: 'abc123'
    })

    if(data) {
        console.log("data:", data);
    }

    if(error) {
        console.log("error:", error);
    }
}

async function loginSuccessTest() {

    let { data, error } = await supabase.auth.signInWithPassword({
        email: 'kangan.dtp@gmail.com',
        password: 'abc123'
    })

    if(data) {
        console.log("data:", data);
        user = await supabase.auth.getUser();
        console.log("user:", user);
    }

    if(error) {
        console.log("error:", error);
    }

}



async function signInGitHub() {
    console.log("signInGitHub");
    const { data, error } = await supabase.auth.signInWithOAuth({
        provider: 'github',
    });

    if(data) {
        console.log("data:", data);
        user = await supabase.auth.getUser();
        console.log("user:", user);
    }

    if(error) {
        console.log("error:", error);
    }
}

async function signout() {
    const { error } = await supabase.auth.signOut();

    if(error) {
        console.log("error:", error);
    }

    user = null;
}


</script>

<button on:click={()=>getDataTest()}>Get Data</button>

<button on:click={()=>addDataTest()}>Insert Data</button> 
<br><hr>

<button on:click={()=>signout()}>Sign Out</button>

<br><hr>

<button on:click={()=>signUpTest()}>Sign Up</button>
<button on:click={()=>loginSuccessTest()}>Login Success</button>

<br><hr>

<button on:click={()=>signInGitHub()}>GitHub SignIn</button>

<br><hr>

{#if user}
    <h1>{user.data.user.email} Logged in</h1>
{:else}
    <h1>Not logged in</h1>
{/if}