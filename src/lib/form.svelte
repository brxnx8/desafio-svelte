<script type="module" lang="ts">
    import type { IcharactersPassword } from "../App.svelte";
    
    export let charactersPassword: IcharactersPassword;

    export let password: string;

    const relationOptions = {
      hasUpercaseLetter: 1,
      hasLowercaseLetter: 2,
      hasNumbers: 3,
      hasSimbols: 4
    }

    function getRandomInt(max: Number): Number {
      return Math.floor((Math.random() * max) + 1);
    }

    function generatePassword(options: IcharactersPassword){
      const length = options.passwordLength;

      password = '';
      
      let primaryPassword: Number[] = []
 
      for (let i = 0; i < length; i++){
        let quitWhile = true
        do{
          const number = getRandomInt(4);
          for (const option in options){
            if(options[option]){
              if(number === relationOptions[option]){
                primaryPassword.push(number);
                quitWhile = false;
              }
            }
          }
        }while(quitWhile)
      }

      primaryPassword.forEach(number => {
        let num:Number;
        switch(true){
          case(number === relationOptions.hasUpercaseLetter):
            num = getRandomInt(26);
            password += String.fromCharCode(num + 64);
            break; 

          case(number === relationOptions.hasLowercaseLetter):
            num = getRandomInt(26);
            password += String.fromCharCode(num + 96);
            break;

          case(number === relationOptions.hasNumbers):
            num = getRandomInt(9);
            password += num.toString();
            break;

          case(number === relationOptions.hasSimbols):
            num = getRandomInt(4);
            password += String.fromCharCode(num + 34);
            break;

          default:
            break;
        }

        console.log(password)
        
      })

    }

    function changeOptionsPassword(event: SubmitEvent){
      const form = event.target;
      charactersPassword = {
        passwordLength: Number(form["passwordLength"].value),
        hasUpercaseLetter: form["hasUpercaseLetter"].checked,
        hasLowercaseLetter: form["hasLowercaseLetter"].checked,
        hasNumbers: form["hasNumbers"].checked,
        hasSimbols: form["hasSimbols"].checked,
      }
      if(charactersPassword.hasLowercaseLetter || charactersPassword.hasUpercaseLetter || charactersPassword.hasNumbers || charactersPassword.hasSimbols){
        generatePassword(charactersPassword)
      }
      else{
        alert("marque pelo menos um checkbox")
      }
    }
</script>

<form class="bg-gray-800 rounded-sm lex flex-col space-y-3 w-full p-5" on:submit|preventDefault={changeOptionsPassword}>
    <div class="space-y-2 flex-col flex">
      <div class="flex justify-between">
        <span>Character length</span>
        <span>{charactersPassword.passwordLength}</span>
      </div>
      <input type="range" name="passwordLength" id="passwordLength" bind:value={charactersPassword.passwordLength} max="15" min="5"/>
    </div>
  
    <div class="flex flex-col items-start space-y-2">
      <div>
        <input type="checkbox" bind:checked="{charactersPassword.hasUpercaseLetter}" name="hasUpercaseLetter" id="hasUpercaseLetter"/>
        <label for="">Include uppercase letters</label>
      </div>
      <div>
        <input type="checkbox" bind:checked="{charactersPassword.hasLowercaseLetter}" name="hasLowercaseLetter" id="hasLowercaseLetter"/>
        <label for="">Include lowercase letters</label>
      </div>
      <div>
        <input type="checkbox" bind:checked="{charactersPassword.hasNumbers}" name="hasNumbers" id="hasNumbers"/>
        <label for="">Include numbers</label>
      </div>
      <div>
        <input type="checkbox" bind:checked="{charactersPassword.hasSimbols}" name="hasSimbols" id="hasSimbols"/>
        <label for="">Include simbols</label>
      </div>
      <div class="flex justify-between w-full px-5 py-3 bg-gray-900">
        <span>strength</span>
        <span>10</span>
      </div>
    </div>
    <button class="bg-green-500 h-12 w-full">Generate</button>
  </form>