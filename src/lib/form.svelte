<script lang="ts">
  import { createEventDispatcher } from "svelte";

  interface IcharactersPassword{
    passwordLength: number;
    hasUpercaseLetter: boolean;
    hasLowercaseLetter: boolean;
    hasNumbers: boolean;
    hasSimbols: boolean;
  }
  
  let charactersPassword: IcharactersPassword = {
    passwordLength: 5,
    hasUpercaseLetter: true,
    hasLowercaseLetter: true,
    hasNumbers: true,
    hasSimbols: true,
  }

  let passwordNivel = "Medium";

  const dispatch = createEventDispatcher();

  const relationOptions = {
    hasUpercaseLetter: 1,
    hasLowercaseLetter: 2,
    hasNumbers: 3,
    hasSimbols: 4
  }

  function getRandomInt(max): Number {
    return Math.floor((Math.random() * max) + 1);
  }

  function generatePassword(options: IcharactersPassword){
    const length = options.passwordLength;
    
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
    let password = '';
    primaryPassword.forEach(number => {
      let num;
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
      
    })
  return password;
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
      dispatch("onChangePassword", generatePassword(charactersPassword))
    }
    else{
      alert("marque pelo menos um checkbox")
    }
  }

  function setPasswordNivel(inputs: IcharactersPassword){
    let nivel = 0;

    switch(true){
      case(inputs.passwordLength >= 5 && inputs.passwordLength <= 7):
        nivel += 1; 
        break;
      case(inputs.passwordLength >= 8 && inputs.passwordLength <= 11):
        nivel += 2; 
        break;
      case(inputs.passwordLength >= 12 && inputs.passwordLength <= 15):
        nivel += 3; 
        break;
      default:
        break;
    }

    if(inputs.hasUpercaseLetter){
      nivel++;
    }
    if(inputs.hasLowercaseLetter){
      nivel++;
    }
    if(inputs.hasNumbers){
      nivel++;
    }
    if(inputs.hasSimbols){
      nivel++;
    }
  
    return nivel;

  }

  function changePasswordNivel(event){
    const form = event.currentTarget;

    const inputs = {
      passwordLength: Number(form["passwordLength"].value),
      hasUpercaseLetter: form["hasUpercaseLetter"].checked,
      hasLowercaseLetter: form["hasLowercaseLetter"].checked,
      hasNumbers: form["hasNumbers"].checked,
      hasSimbols: form["hasSimbols"].checked,
    }

    const nivel = setPasswordNivel(inputs);

    switch(true){
      case(nivel >= 2 && nivel <= 3):
        passwordNivel = "Easy";
        break;
      case(nivel >= 4 && nivel <= 5):
        passwordNivel = "Medium";
        break;
      case(nivel >= 6 && nivel <= 7):
        passwordNivel = "Hard";
        break;
      default:
        break;
    }

  }

</script>

<form class="bg-gray-800 rounded-sm lex flex-col space-y-3 w-full p-5" on:submit|preventDefault={changeOptionsPassword} on:change={changePasswordNivel}>
    <div class="space-y-2 flex-col flex">
      <div class="flex justify-between">
        <span class="font-bold text-lg">Character length</span>
        <span class="text-lg">{charactersPassword.passwordLength}</span>
      </div>
      <input type="range" name="passwordLength" id="passwordLength" class="accent-green-600" bind:value={charactersPassword.passwordLength} max="15" min="5"/>
    </div>
  
    <div class="flex flex-col items-start space-y-2">
      <div>
        <input type="checkbox" class="accent-green-600" bind:checked="{charactersPassword.hasUpercaseLetter}" name="hasUpercaseLetter" id="hasUpercaseLetter"/>
        <label for="">Include uppercase letters</label>
      </div>
      <div>
        <input type="checkbox" class="accent-green-600" bind:checked="{charactersPassword.hasLowercaseLetter}" name="hasLowercaseLetter" id="hasLowercaseLetter"/>
        <label for="">Include lowercase letters</label>
      </div>
      <div>
        <input type="checkbox" class="accent-green-600" bind:checked="{charactersPassword.hasNumbers}" name="hasNumbers" id="hasNumbers"/>
        <label for="">Include numbers</label>
      </div>
      <div>
        <input type="checkbox" class="accent-green-600" bind:checked="{charactersPassword.hasSimbols}" name="hasSimbols" id="hasSimbols"/>
        <label for="">Include simbols</label>
      </div>
      <div class="flex justify-between w-full px-5 py-3 bg-gray-900">
        <span class="font-bold">Strength:</span>
        <div class="flex justify-center gap-4">
          <div class="flex justify-center gap-1 items-center bg-gray-800 p-1">
            {#if (passwordNivel === "Easy")}
              <div class="h-2/3 w-1 bg-red-600"></div>
            {/if}
            {#if passwordNivel === "Medium"}
              <div class="h-2/3 w-1 bg-yellow-600"></div>
              <div class="h-2/3 w-1 bg-yellow-600"></div>
            {/if}
            {#if passwordNivel === "Hard"}
              <div class="h-2/3 w-1 bg-green-600"></div>
              <div class="h-2/3 w-1 bg-green-600"></div>
              <div class="h-2/3 w-1 bg-green-600"></div>
            {/if}
          </div>
        
          <span class="font-bold">{passwordNivel}</span>
        </div>
      </div>
    </div>
    <button class="bg-green-600 h-12 w-full hover:bg-green-700 active:bg-green-900">Generate</button>
  </form>