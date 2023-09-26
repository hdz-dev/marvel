<script>
  // @ts-nocheck


  import { onMount } from "svelte";
  //import { personajes } from "$lib/stores/stores.svelte";

  let hero = "";
  let personajes = [];
  async function api (values){
    await fetch("https://gateway.marvel.com/v1/public/characters?nameStartsWith=" 
    + values + 
    "&ts=1&apikey=a775c93a3b233f2658202a2accaf658c&hash=901d1f7b34bdd4f95d2de01608d28b7e")
      .then(response => response.json())
      .then(data => {
        console.log(data);
       personajes = data.data.results;
       console.log(personajes.data.results)
        // Aquí puedes realizar operaciones adicionales con los datos recibidos
      })
      .catch(error => console.error(error));
  };

  // onMount( () => {
  //   api(value)
    
  // });
</script>

<div class="flex flex-col justify-center">

  <div class=" flex flex-row m-auto bg-white w-2/3 rounded-3xl py-2 px-4">
    <input
      class="border-0 border-white"
      type="text"
      placeholder="Buscar..."
      bind:value={hero}
    />
    <img on:click={() => api(hero)} class="w-auto h-6" src="../busqueda.png" alt="" />
  </div>

  <div>
    {#if personajes}
      {#each personajes as personaje}
      <div class="flex flex-row bg-white w-2/3 rounded-xl py-2 px-4 mx-auto my-4">
        <div>
          <img src={personaje.thumbnail.path + "." + personaje.thumbnail.extension} alt="">
          <p class="text-center font-bold">{personaje.name}</p>
          <p class="text-sm">{personaje.description}</p>
          
        </div>
      </div>
        
      {:else}
        <!-- empty list -->
      {/each}
    {:else}
      <div>
        <p></p>
      </div>
    {/if}
  </div>
</div>
