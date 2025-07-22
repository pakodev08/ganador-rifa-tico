<script>
  import { onMount, } from "svelte";
  import ws from "./assets/ws.svg";

  let host = `https://backend-tico.onrender.com/guerra/users`;
  // let host = `http://localhost:4005/guerra/users`;
  
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
    
    console.log(data.user)
    allData = data.user

    allData.map(user => {
      user.tlf = user.tlf.toString()
        if(user.tlf.startsWith(`4`)){
          const disminuido = user.tlf.slice(-10)
          const tody2 = `58${disminuido}`
          user.tlf = tody2
      }

    })

  
  })

  const handleChange = (e) => {
    e.preventDefault()
    finded = allData.filter(user => 
      user.numbers.some(ticket => 
        ticket.number.toString().includes(inputValue)
      )
    )
    // finded = allData.filter(user => 
    //   user.name.toLowerCase().includes(inputValue.toLowerCase())
    // )
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

  <p>{item.name.toLowerCase()}</p>
   <p class="ticket">{item.numbers.map(ticket => ticket.number).join(', ')}</p>
  <a href={`${sendInfo}${item.tlf}${secondInfo}`} target="_blank">
    <img src={ws} alt=""></a>
    <!-- <p>{item.tlf}</p> -->
  </article>
  <!-- </section> -->
    {/each}
  
  
  </article>
  {:else}
  {#each finded as item}
<article class="card">

  <p>{item.name.toLowerCase()}</p>
 <p class="ticket">{item.numbers.map(ticket => ticket.number).join(', ')}</p>
 <a href={`${sendInfo}${item.tlf}${secondInfo}`} target="_blank">
   <img src={ws} alt=""></a>
   <!-- <p>{item.tlf}</p> -->
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