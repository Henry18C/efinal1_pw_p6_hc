<template>
  <div class="contenedor" v-if="isPlaying">
    <h1 for="">Puntaje: {{ puntaje }} --- Intentos: {{ intetos }}</h1>

    <PokemonVue
      v-for="(imagen, index) in imagenes"
      :key="index"
      :imagen="imagen"
      :nombre="nombres[index]"
    />
    <button @click="jugar">Jugar</button>
  </div>
  <div v-else>
    <div v-if="showWin" id="window">
      <h1 id="hwin">
        Puntaje: {{ puntaje }} <br />
        Felicitaciones has ganado un premio de $10.000,00
      </h1>
      <div><button @click="resetGame">Nuevo Juego</button></div>
    </div>
    <div v-if="showLose" id="window">
      <h1 id="hlose">
        Has utilizado tus 5 intentos <br> El juego ha termindo, intentalo nuevamente
      </h1>
      <div><button @click="resetGame">Nuevo Juego</button></div>
    </div>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";
import PokemonVue from "./components/Pokemon.vue";

export default {
  name: "App",
  components: {
    PokemonVue,
  },
  data() {
    return {
      isPlaying: true,
      imagenes: [
        "https://via.placeholder.com/250",
        "https://via.placeholder.com/250",
        "https://via.placeholder.com/250",
      ],
      pokeimagenes: [
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/1.svg",
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/2.svg",
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/3.svg",
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/4.svg",
        "https://raw.githubusercontent.com/PokeAPI/sprites/master/sprites/pokemon/other/dream-world/5.svg",
      ],
      nombres: [],
      puntaje: 0,
      intetos: 0,
      showWin: false,
      showLose: false,
    };
  },
  methods: {
    async jugar() {
      this.intetos++
      let pokenames = [];
      const imagenesApi = [];
      const nombresApi = [];
      const paginas = [1, 2, 3, 4, 5];

      const datos = paginas.map((pagina) =>
        fetch(`https://pokeapi.co/api/v2/pokemon/${pagina}`).then((r) =>
          r.json()
        )
      );
      try {
        const resultados = await Promise.all(datos);

        pokenames = resultados.map((nombre) => nombre.forms[0].name);
        console.log(pokenames);
        console.log(pokenames[1]);
      } catch (error) {
        console.log(error);
      }

      for (let i = 0; i < this.imagenes.length; i++) {
        let r = Math.floor(Math.random() * 5);
        imagenesApi.push(this.pokeimagenes[r]);
        nombresApi.push(pokenames[r]);
      }
      this.imagenes = imagenesApi;
      this.nombres = nombresApi;

      let numPoke1=0;
      let numPoke2=0;
      let numPoke3=0;
      let numPoke4=0;
      let numPoke5=0;

      for (let pokename of this.nombres) {
        if (pokename == "bulbasaur") {
          numPoke1++;
        } else if (pokename == "ivysaur") {
          numPoke2++;
        } else if (pokename == "venusaur") {
          numPoke3++;
        } else if (pokename == "charmander") {
          numPoke4++;
        } else if (pokename == "charmeleon") {
          numPoke5++;
        }
      }

      if(numPoke1==3||numPoke2==3||numPoke3==3||numPoke4==3||numPoke5==3){
      this.puntaje=this.puntaje+5;
    }else if (numPoke1==2||numPoke2==2||numPoke3==2||numPoke4==2||numPoke5==2){
      this.puntaje=this.puntaje+3;
    }

    if(this.intetos<=5 && this.puntaje>=10){
      this.isPlaying=false;
      this.showWin=true;

    }else if(this.intetos==5 && this.puntaje<10){
      this.isPlaying=false;
      this.showLose=true;

    }


    },
    resetGame(){
      window.location.reload()
    }
  },
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

.contenedor {
  display: grid;
  justify-items: center;
  align-content: center;
  border: solid black;

  grid-template-columns: repeat(3, 300px);
  grid-template-rows: 20px 350px 30px;
  gap: 50px;
}
h1 {
  grid-column: span 3;
}

#hwin{
  color: red;
}

#hlose{
  color: blue;
}

button{
  grid-column: span 3;
  align-content: center;
  border: solid black;
  width: 100px;
  height: 20px;
}
#window{
  border: solid black;
}

</style>
