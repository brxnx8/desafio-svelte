<script type="module" lang="ts">
    import type { IcharactersPassword } from "../App.svelte";
    

    export let charactersPassword: IcharactersPassword;
</script>

<form class="bg-gray-800 rounded-sm lex flex-col space-y-3 w-full p-5" >
    <div class="space-y-2 flex-col flex">
      <div class="flex justify-between">
        <span>Character length</span>
        <span>10</span>
      </div>
      <input type="range" />
    </div>
  
    <div class="flex flex-col items-start space-y-2">
      <div>
        <input type="checkbox" bind:checked="{charactersPassword.hasUpercaseLetter}" />
        <label for="">Include uppercase letters</label>
      </div>
      <div>
        <input type="checkbox" bind:checked="{charactersPassword.hasLowercaseLetter}" />
        <label for="">Include lowercase letters</label>
      </div>
      <div>
        <input type="checkbox" bind:checked="{charactersPassword.hasNumbers}" />
        <label for="">Include numbers</label>
      </div>
      <div>
        <input type="checkbox" bind:checked="{charactersPassword.hasNumbers}" />
        <label for="">Include simbols</label>
      </div>
      <div class="flex justify-between w-full px-5 py-3 bg-gray-900">
        <span>strength</span>
        <span>10</span>
      </div>
    </div>
    <button class="bg-green-500 h-12 w-full">Generate</button>
  </form>