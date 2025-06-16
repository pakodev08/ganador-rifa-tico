<script>
  import { onMount, } from "svelte";
  import ws from "./assets/ws.svg";

  let host = `https://backend-rifas-tico.onrender.com/flash/ticketselected`;
  
  let userWinner = $state({})
  
  let inputValue = $state('')
  let allData = $state([])
let test = $state({})
let finded = $state([])
let sendInfo = `https://api.whatsapp.com/send?phone=`

let secondInfo = `&text=Hola.%20Ha%20ganado%20el%20premio%20de%20300$%20de%20Tico%20Rifas`
  onMount( async() => {
      const response = await fetch(host)
    const data = await response.json()
    allData = data

    allData.map(user => {
        if(user.phone.startsWith(`04`)){
          const disminuido = user.phone.slice(-10)
          const tody2 = `58${disminuido}`
          user.phone = tody2
      }

    })

  
  })

  const handleChange = (e) => {
    e.preventDefault()
    finded = allData.filter(user => 
      user.tickets.some(ticket => 
        ticket.value.toString().includes(inputValue)
      )
    )
  }
  
</script>

<form action="">
  <label for="searchUser">Buscar
    <input type="text" name="" id="searchUser" bind:value={inputValue} oninput={handleChange}> 
  </label>
</form>

  <h1>Usuarios</h1>

{#if inputValue === ``}


<article class="main">
  


{#each allData as item}
<article class="card">
<!-- <section> -->

  <p>{item.name.toLowerCase()}</p>
   <p class="ticket">{item.tickets.map(ticket => ticket.value).join(', ')}</p>
  <a href={`${sendInfo}${item.phone}${secondInfo}`} target="_blank">
    <img src={ws} alt=""></a>
  </article>
  <!-- </section> -->
    {/each}
  
  
  </article>
  {:else}
  {#each finded as item}
<article class="card">

  <p>{item.name.toLowerCase()}</p>
 <p class="ticket">{item.tickets.map(ticket => ticket.value).join(', ')}</p>
  <a href={`${sendInfo}${item.phone}${secondInfo}`} target="_blank">
    <img src={ws} alt=""></a>
  </article>
    {/each}

{/if}


<style>

  .main{
      display: flex;
    flex-wrap: wrap;
    flex-direction: column;
    width: 95%;
    margin: 0 auto;
    padding: 6px;
    justify-content: space-between;
    align-items: center;
    border-radius: 20px;
    gap: 10px;
  }
  .card{
    display: flex;
    /* flex-wrap: wrap; */
    width: 95%;
    margin: 0 auto;
    padding: 6px;
    justify-content: space-between;
    align-items: center;
    border-radius: 20px;
    gap: 20px;
    
    
    &:nth-child(odd){
      background: #a0b7ff;
    }
    &:nth-child(even){
      background: #dee8ff;
    }
  }
  img{
    aspect-ratio: 1/1;
    width: 30px

  }
</style>