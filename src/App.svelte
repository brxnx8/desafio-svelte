<script lang="ts">
  import Form from "./lib/form.svelte";



  let password: string;

  function changePassword(event: CustomEvent){
    password = event.detail
    console.log(password)
  } 



</script>

<main class="flex justify-center items-center bg-slate-900 w-full h-screen">
 
  <section class="flex flex-col justify-center items-center text-white space-y-3 w-5/12">
    <h1 class="">Password Generate</h1>
    <div class="bg-gray-800 rounded-sm space-x-2 w-full p-5 flex justify-between" >
      <div class="font-extrabold text-lg tracking-widest">{password ? password : "PASSWORD"}</div>
      <input type="checkbox" />
    </div>
    <Form on:onChangePassword={changePassword} />
  </section>
  
</main>

<style>
  
</style>
