<script lang="ts">
  import Form from "./lib/form.svelte";

  let copied = false;

  let password: string;

  function changePassword(event: CustomEvent){
    password = event.detail;
  } 

  function toCopy(){
    if(password){
      navigator.clipboard.writeText(password);
      copied = true;
      setTimeout(()=>{
        copied = false;
      }, 2000)
  }
    }
    


</script>

<main class="flex justify-center items-center bg-slate-900 w-full h-screen">
 
  <section class="flex flex-col justify-center items-center text-white space-y-3 w-5/12">
    <h1 class="text-4xl font-extrabold mb-2 animate-pulse">Password Generate</h1>
    <div class="bg-gray-800 rounded-sm space-x-2 w-full p-6 flex justify-between items-center " >
      <div class="font-extrabold text-lg tracking-widest font-mono ">{password ? password : ""}</div>
      <div on:click={toCopy} class="flex gap-2 items-center justify-center">
        <svg xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke-width="1.5" stroke="currentColor" class="w-6 h-6 text-green-600 cursor-pointer hover:text-green-700"  >
          <path stroke-linecap="round" stroke-linejoin="round" d="M9 12h3.75M9 15h3.75M9 18h3.75m3 .75H18a2.25 2.25 0 002.25-2.25V6.108c0-1.135-.845-2.098-1.976-2.192a48.424 48.424 0 00-1.123-.08m-5.801 0c-.065.21-.1.433-.1.664 0 .414.336.75.75.75h4.5a.75.75 0 00.75-.75 2.25 2.25 0 00-.1-.664m-5.8 0A2.251 2.251 0 0113.5 2.25H15c1.012 0 1.867.668 2.15 1.586m-5.8 0c-.376.023-.75.05-1.124.08C9.095 4.01 8.25 4.973 8.25 6.108V8.25m0 0H4.875c-.621 0-1.125.504-1.125 1.125v11.25c0 .621.504 1.125 1.125 1.125h9.75c.621 0 1.125-.504 1.125-1.125V9.375c0-.621-.504-1.125-1.125-1.125H8.25zM6.75 12h.008v.008H6.75V12zm0 3h.008v.008H6.75V15zm0 3h.008v.008H6.75V18z" />
        </svg>
        {#if copied}
          <span class="text-green-600 text-sm">Copied!</span>
        {/if}
      </div>
    </div>
    <Form on:onChangePassword={changePassword} />
  </section>
  
</main>

<style>
  
</style>
