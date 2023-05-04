<script lang="ts" context="module">
  import Form from "./lib/form.svelte";

  export interface IcharactersPassword{
    passwordLength: number;
    hasUpercaseLetter: boolean;
    hasLowercaseLetter: boolean;
    hasNumbers: boolean;
    hasSimbols: boolean;
  }

  let password: string;

  let charactersPassword: IcharactersPassword = {
    passwordLength: 5,
    hasUpercaseLetter: true,
    hasLowercaseLetter: true,
    hasNumbers: true,
    hasSimbols: true,
  }

</script>

<main class="flex justify-center items-center bg-slate-900 w-full h-screen">
 
  <section class="flex flex-col justify-center items-center text-white space-y-3 w-5/12">
    <h1 class="">Password Generate</h1>
    <div class="bg-gray-800 rounded-sm space-x-2 w-full p-5 flex justify-between" >
      <span>{password}</span>
      <input type="checkbox" />
    </div>
    <Form bind:charactersPassword bind:password />
  </section>
  
</main>

<style>
  
</style>
