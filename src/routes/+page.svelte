<script>
import { createClient } from '@supabase/supabase-js'

// Create a single supabase client for interacting with your database
const supabase = createClient('https://uioaemhkajajdgxqskwv.supabase.co', 'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6InVpb2FlbWhrYWphamRneHFza3d2Iiwicm9sZSI6ImFub24iLCJpYXQiOjE2OTM4NzM5MjIsImV4cCI6MjAwOTQ0OTkyMn0.UhssrleA4-TDRaeAdWSGM3h2mEeFdjUN9VBprrQ5JyI')
let user;

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
        email: 'a.nguyen@kangan.edu.au',
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
        email: 'a.nguyen@kangan.edu.au',
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


</script>

<button on:click={()=>getDataTest()}>Get Data</button>

<button on:click={()=>addDataTest()}>Insert Data</button> 
<br><hr>

<button on:click={()=>signUpTest()}>Sign Up</button>
<button on:click={()=>loginSuccessTest()}>Login Success</button>

<br><hr>

{#if user}
    <h1>{user.data.user.email} Logged in</h1>
{:else}
    <h1>Not logged in</h1>
{/if}