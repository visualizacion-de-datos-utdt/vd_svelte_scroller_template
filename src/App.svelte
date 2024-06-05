<script>
  import Scroller from "@sveltejs/svelte-scroller"
  import {onMount} from "svelte"
  import * as d3 from "d3"

  import Medallero from "./components/Medallero.svelte"
  import DebugScroller from "./components/DebugScroller.svelte"
  import Loremipsum from "./components/Loremipsum.svelte"

  /* Variables para la data del medallero */
  let deportistas = []
  let filteredDeportistas = []

  /* Variables para el scroller */
  let count
  let index
  let offset
  let progress
  let top = 0.1
  let threshold = 0.5
  let bottom = 0.9

  onMount(() => {
    d3.csv("./data/deportistas.csv", d3.autoType).then(data => {
      deportistas = data
      filteredDeportistas = deportistas
    })
  })

  $: {
    // Es un observer que se ejecuta cuando cambia el valor de index
    switch (index) {
      case 0:
        filteredDeportistas = deportistas
        break
      case 1:
        filteredDeportistas = deportistas.filter(d => d.genero === "F")
        break
      case 2:
        filteredDeportistas = deportistas.filter(d => d.genero === "M")
        break
      case 3:
        filteredDeportistas = deportistas.filter(d => d.continente === "América")
        break
      default:
        filteredDeportistas = deportistas
    }
    console.log(filteredDeportistas)
  }
</script>

<main>
  <div class="header">
    <img src="/images/olympics-logo.png" width="100" alt="anillos" />
    <h3 class="headline">
      <b>Triunfos Olímpicos</b>
      Medallas, alturas y continentes
    </h3>
    <p class="bajada">Explorando los logros olímpicos a través de datos</p>
    <div class="lorem_ipsum">
      <Loremipsum />
    </div>
    
  </div>

  <DebugScroller
    index={index}
    count={count}
    offset={offset}
    progress={progress}
  />

  <Scroller
    top={top}
    threshold={threshold}
    bottom={bottom}
    bind:count={count}
    bind:index={index}
    bind:offset={offset}
    bind:progress={progress}
  >
    <div slot="background">
      <Medallero deportistas={filteredDeportistas} />
    </div>

    <div slot="foreground" style="padding: 0 0 0 50%;">
      <section>
        <div class="epi_foreground">
          <h3>Seccion {index + 1}</h3>
          <p>Todos los deportistas</p>
        </div>
      </section>
      <section>
        <div class="epi_foreground">
          <h3>Seccion {index + 1}</h3>
          <p>Deportistas femeninas</p>
        </div>
      </section>
      <section>
        <div class="epi_foreground">
          <h3>Seccion {index + 1}</h3>
          <p>Deportistas masculinos</p>
        </div>
      </section>
      <section>
        <div class="epi_foreground">
          <h3>Seccion {index + 1}</h3>
          <p>Deportistas americanos</p>
        </div>
      </section>
    </div>
  </Scroller>
</main>

<div class="lorem_ipsum">
  <Loremipsum />
</div>

<style>
  .header {
    display: flex;
    justify-content: center;
    align-items: center;
    flex-direction: column;
    margin-top: 50px;
    margin-bottom: 80px;
  }
  .headline {
    font-size: 30px;
    line-height: 1.2;
    font-weight: normal;
    text-align: center;
    margin: 20px;
  }
  .bajada {
    font-size: 18px;
    font-weight: normal;
    text-align: center;
    margin: 10px;
  }
  .headline b {
    display: block;
  }

  /* Estilos para el scroller */
  [slot="background"] {
    /* background-color: rgba(255, 62, 0, 0.05);
    border-top: 2px solid #ff3e00;
    border-bottom: 2px solid #ff3e00; */
    font-size: 1.4em;
    /* overflow: hidden; */
    padding: 1em;
  }

  [slot="foreground"] {
    pointer-events: none;
  }

  [slot="foreground"] section {
    pointer-events: all;
  }

  section {
    display: flex;
    justify-content: end;
    align-items: center;
    height: 100vh;
    border: 1px solid rgba(0, 0, 0, 0.4);
    color: white;
    padding: 1em;
    margin: 0 0 2em 0;
  }
  .epi_foreground {
    padding: 20px;
    max-width: 150px;
    background-color: rgba(0, 0, 0, 0.5);
  }
  .lorem_ipsum {
    margin: 100px auto;
    max-width: 740px;
  }
</style>
