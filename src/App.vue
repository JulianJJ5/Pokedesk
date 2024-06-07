<template>
  <div>
    <img src="./../imagenes/fondo_pokemon.jpg" alt="background image" id="fondo" >
    <div id="contenedor">
      <div id="buscador">
        <input id="cajaDeTexto" type="text" placeholder="Ingrese el nombre o numero del pokemon" v-model="id" />
        <button @click="traerInformacion">Buscar</button>
      </div>

      <div id="cajaPrincipal">
        <div id="primeraLineaDeInformacion">
    <div id="nombreDelPokemon"><h1 v-show="nombreDelPokemon">{{ nombreDelPokemon }}</h1>
</div>

    <div id="cajaDelTipoDePokemon" v-show="categoriaDelPokemon">
      {{ categoriaDelPokemon }}
</div>

<div id="cajaDelTipo2DePokemon" v-show="categoria2DelPokemon">
      {{ categoria2DelPokemon }}
</div>
      <h2 v-show="numeroPokemon">{{ "#" + numeroPokemon }}</h2>
  </div>


        <img v-show="imgPokemon" :src="imgPokemon" alt="" />
      </div>

      <div id="fichaDelPokemon">
        <h1 v-show="pesoDelPokemon">{{ "Peso: " + pesoDelPokemon / 10 + " Kg" }}</h1>
        <h1 id="altura" v-show="alturaDelPokemon">{{ "Altura: " + alturaDelPokemon / 10 + " m" }}</h1>
      </div>

      <div id="contenedorEstadisticas">
        <div id="estadisticas">
          <div class="q-pa-md flex flex-center">
            <h1>HP: </h1>
            <q-circular-progress
              show-value
              font-size="25px"
              :value="maximoDeProgresoCircular(hpDelPokemon)"
              size="100px"
              :thickness="0.23"
              color="light-blue"
              track-color="transparent"
              class="q-ma-md"
            >
              {{ hpDelPokemon }}
            </q-circular-progress>
          </div>

          <div class="q-pa-md flex flex-center">
            <h1>Ataque: </h1>
            <q-circular-progress
              show-value
              font-size="25px"
              :value="maximoDeProgresoCircular(ataqueDelPokemon)"
              size="100px"
              :thickness="0.22"
              color="light-blue"
              track-color="transparent"
              class="q-ma-md"
            >
              {{ ataqueDelPokemon }}
            </q-circular-progress>
          </div>

          <div class="q-pa-md flex flex-center">
            <h1>Defensa: </h1>
            <q-circular-progress
              show-value
              font-size="25px"
              :value="maximoDeProgresoCircular(defensaDelPokemon)"
              size="100px"
              :thickness="0.22"
              color="light-blue"
              track-color="transparent"
              class="q-ma-md"
            >
              {{ defensaDelPokemon }}
            </q-circular-progress>
          </div>

          <div class="q-pa-md flex flex-center">
            <h1>Ataque Especial: </h1>
            <q-circular-progress
              show-value
              font-size="25px"
              :value="maximoDeProgresoCircular(ataqueEspecialDelPokemon)"
              size="100px"
              :thickness="0.22"
              color="light-blue"
              track-color="transparent"
              class="q-ma-md"
            >
              {{ ataqueEspecialDelPokemon }}
            </q-circular-progress>
          </div>

          <div class="q-pa-md flex flex-center">
            <h1>Defensa Especial: </h1>
            <q-circular-progress
              show-value
              font-size="25px"
              :value="maximoDeProgresoCircular(defensaEspecialDelPokemon)"
              size="100px"
              :thickness="0.22"
              color="light-blue"
              track-color="transparent"
              class="q-ma-md"
            >
              {{ defensaEspecialDelPokemon }}
            </q-circular-progress>
          </div>

          <div class="q-pa-md flex flex-center">
            <h1>Velocidad: </h1>
            <q-circular-progress
              show-value
              font-size="25px"
              :value="maximoDeProgresoCircular(velocidadDelPokemon)"
              size="100px"
              :thickness="0.22"
              color="light-blue"
              track-color="transparent"
              class="q-ma-md"
            >
              {{ velocidadDelPokemon }}
            </q-circular-progress>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>


<script setup>
import axios from "axios";
import { ref } from "vue";

let id = ref("");
let imgPokemon = ref("");
let numeroPokemon = ref("");
let nombreDelPokemon = ref("");
let pesoDelPokemon = ref("");
let alturaDelPokemon = ref("");
let categoriaDelPokemon = ref("");
let categoria2DelPokemon = ref("");
let hpDelPokemon = ref("");
let ataqueDelPokemon = ref("");
let defensaDelPokemon = ref("");
let ataqueEspecialDelPokemon = ref("");
let defensaEspecialDelPokemon = ref("");
let velocidadDelPokemon = ref("");

async function traerInformacion() {
  try {
    let url = await axios.get(`https://pokeapi.co/api/v2/pokemon/${id.value}`);
    let data = url.data;
    imgPokemon.value = data.sprites.other["official-artwork"].front_default;
    nombreDelPokemon.value = data.species.name;
    numeroPokemon.value = data.id;
    pesoDelPokemon.value = data.weight;
    alturaDelPokemon.value = data.height;
    categoriaDelPokemon.value = data.types[0].type.name;

    if (data.types.length > 1) {
      categoria2DelPokemon.value = data.types[1].type.name;
    }

    hpDelPokemon.value = data.stats[0].base_stat;
    ataqueDelPokemon.value = data.stats[1].base_stat;
    defensaDelPokemon.value = data.stats[2].base_stat;
    ataqueEspecialDelPokemon.value = data.stats[3].base_stat;
    defensaEspecialDelPokemon.value = data.stats[4].base_stat;
    velocidadDelPokemon.value = data.stats[5].base_stat;
  
    document.getElementById("fondo").style.display = "none";
    cambiarColorDelBodySegunTipoDePokemon();
    cambiarColorSegunTipoDePokemonParaElBotonDelTipo();
    cambiarColorSegunTipo2DePokemonParaElBotonDelTipo();

  } catch (error) {
    console.error("Error al traer la información del Pokémon", error);
  }
}

function maximoDeProgresoCircular(value) {
  return (value / 255) * 100;
}

let colorSegunTipoDePokemon = {
  fire: "#FF5733",
  grass: "#4CAF50",
  water: "#2196F3",
  electric: "#FFC107",
  ground: "#795548",
  poison: "#9C27B0",
  rock: "#607D8B",
  bug: "#CDDC39",
  dragon: "#673AB7",
  psychic: "#FF4081",
  ghost: "#607D8B",
  dark: "#212121",
  steel: "#9E9E9E",
  fairy: "#E91E63",
  fighting: "#FF5252",
  normal: "#FFFFFF",
  ice: "#03A9F4",
  flying: "#03A9F4"
};

let colorSegunTipoDePokemonParaElBotonDelTipo = {
  fire: "#F13F19",
  grass: "#1DD024",
  water: "#2482CD",
  electric: "#FFAF00",
  ground: "#945B47",
  poison: "#820797",
  rock: "#4B88A5",
  bug: "#C5D714",
  dragon: "#5A1CC6",
  psychic: "#F32A6E",
  ghost: "#2A79A0",
  dark: "#171313",
  steel: "#93A7B1",
  fairy: "#FF007B",
  fighting: "#F93333",
  normal: "#EBEBEB",
  ice: "#038DF4",
  flying: "#0392F4"
};

let cambiarColorDelBodySegunTipoDePokemon = () => {
  let color = colorSegunTipoDePokemon[categoriaDelPokemon.value];
  document.body.style.backgroundColor = color;
};

let cambiarColorSegunTipoDePokemonParaElBotonDelTipo = () => {
  let color = colorSegunTipoDePokemonParaElBotonDelTipo[categoriaDelPokemon.value];
  document.querySelector('#cajaDelTipoDePokemon').style.backgroundColor = color;
};


let cambiarColorSegunTipo2DePokemonParaElBotonDelTipo = () => {
  let color = colorSegunTipoDePokemonParaElBotonDelTipo[categoria2DelPokemon.value];
  document.querySelector('#cajaDelTipo2DePokemon').style.backgroundColor = color;
};
</script>

<style>
#fondo{
  width: 100%;
  height: 100vh;
  position: fixed;
  top: 0px;
  right:0%;
}

#contenedor {
  align-items: center;
  position: relative;
  height: 110vh;
}

#buscador {
  width: 100%;
  display: flex;
  justify-content: center;
  gap: 3%;
  margin-bottom: 20px;
  padding-bottom: 2%;
}

#buscador input {
  width: 40%;
  border: 2px solid #000;
  border-radius: 20px;
  font-size: 1.3rem;
  padding: 0.8%;
  outline: none;
}

#buscador input:focus {
  border-color: #1e90ff;
}

#buscador button {
  border: 2px solid #000;
  border-radius: 20px;
  font-size: 1.3rem;
  padding: 0.8% 2%;
  cursor: pointer;
  background-color: #fff;
  transition: background-color 0.3s;
}

#buscador button:hover {
  background-color: #f0f0f0;
}

#cajaPrincipal {
  position: relative;
  right: 40%;
  top: -50px;
  z-index: 1;
}

#cajaPrincipal img {
  border-radius: 10px;

}

#cajaPrincipal h1 {
font-weight: bold;
text-transform: capitalize;
font-size: 70px !important;
position: relative;
top: 90px;
text-shadow: 
    -5px -1px 0 #000000,  
    1px -1px 0 #000000,
    -1px 1px 0 #000000,
    1px 1px 0 #000000;  
}

#cajaPrincipal h2 {
font-weight: bold;
text-transform: capitalize;
font-size: 50px;
position: relative;
bottom: 450px;
left: 160px;
text-shadow: 
    -4px -1px 0 #000000,  
    1px -1px 0 #000000,
    -1px 1px 0 #000000,
    1px 1px 0 #000000;  
}

#primeraLineaDeInformacion{
width: 15%; 
position: relative;
left: 65%;
height: 200px;
margin-right: 0%;
}

#primeraLineaDeInformacion h2{
  position: relative;
  bottom: -30%;
  left: 0px;
  font-size: 55px !important;
}

#contenedorEstadisticas{
  position: relative;
  top: -830px;
  left: 40%;
  width: 62%;
}

#estadisticas {
  text-align: center;
  margin-bottom: 20px;
  display: grid;
  position: relative;
  grid-template-columns: repeat(3, 1fr);
  gap: 0px;

}

#estadisticas h1 {
  width: 38%;
  font-size: 210%;
  text-shadow: 
    -2px -1px 0 #000000,  
    1px -1px 0 #000000,
    -1px 1px 0 #000000,
    1px 1px 0 #000000;  

}

#fichaDelPokemon{
  display: flex;
  position: relative;
  left: 46%;
  width: 60%;
  gap: 0px;
  top: -250px;
  position: relative;
  text-shadow: 
    -2px -1px 0 #000000,  
    1px -1px 0 #000000,
    -1px 1px 0 #000000,
    1px 1px 0 #000000; 
}

#fichaDelPokemon h1{
  font-size: 40px !important;
}

#altura{
  padding-left: 10%;
}



#cajaDelTipoDePokemon{
  border: 2px solid black;
  position: relative;
  font-size: 40px;
  border-radius: 50px;
  margin-bottom: 10px;
}

#cajaDelTipo2DePokemon{
  border: 2px solid black;
  position: relative;
  font-size: 40px ;
  border-radius: 50px;
}

#nombreDelPokemon{
  position: relative;
  right: 150%;
  font-size: 160px;
}



.q-circular-progress {
  width: 100px;
  height: 100px;
  position: relative;
  text-shadow: 
    -2px -1px 0 #000000,  
    1px -1px 0 #000000,
    -1px 1px 0 #000000,
    1px 1px 0 #000000;  
}

.q-circular-progress::before,
.q-circular-progress::after {
  content: '';
  position: absolute;
  border-radius: 50%;
}

.q-circular-progress::before {
  top: 11px; 
  left: 11px; 
  right: 11px; 
  bottom: 11px; 
  box-shadow: inset 0 0 5px rgb(0, 0, 0); /* Borde interno */
}

.q-circular-progress::after {
  top: 0px; 
  left: 0px; 
  right: 0px; 
  bottom: 0px; 
  box-shadow: 0 0 5px rgb(0, 0, 0); /* Borde externo */
}

/* Para pantallas entre 1809px y 2560px */
@media screen and (max-width: 2560px) and (min-width: 1809px) {
  #cajaPrincipal h1 {
    font-size: 3rem;
  }

  #cajaPrincipal h2 {
    font-size: 2rem;
  }

  #fichaDelPokemon h1 {
    font-size: 1.5rem;
  }

  #buscador input {
    font-size: 1.5rem;
    padding: 15px;
  }

  #buscador button {
    font-size: 1.5rem;
    padding: 15px 30px;
  }
}

/* Para pantallas entre 1025px y 1809px */
@media screen and (max-width: 1809px) and (min-width: 1025px) {
  #cajaPrincipal h1 {
    font-size: 2.5rem;
  }

  #cajaPrincipal h2 {
    font-size: 1.8rem;
  }

  #fichaDelPokemon h1 {
    font-size: 1.2rem;
  }

  #buscador input {
    font-size: 1.3rem;
    padding: 12px;
  }

  #buscador button {
    font-size: 1.3rem;
    padding: 12px 25px;
  }
}

/* Para pantallas entre 768px y 1024px */
@media screen and (max-width: 1024px) and (min-width: 768px) {
  #cajaPrincipal h1 {
    font-size: 2rem;
  }

  #cajaPrincipal h2 {
    font-size: 1.5rem;
  }

  #fichaDelPokemon h1 {
    font-size: 1rem;
  }

  #buscador input {
    font-size: 1.2rem;
    padding: 10px;
  }

  #buscador button {
    font-size: 1.2rem;
    padding: 10px 20px;
  }
  
  #estadisticas{
    grid-template-columns: repeat(2, 1fr);

  }
}

/* Para pantallas entre 460px y 767px */
@media screen and (max-width: 767px) and (min-width: 460px) {
  #cajaPrincipal h1 {
    font-size: 1.5rem;
  }

  #cajaPrincipal h2 {
    font-size: 1.2rem;
  }

  #fichaDelPokemon h1 {
    font-size: 0.8rem;
  }

  #buscador input {
    font-size: 1rem;
    padding: 8px;
  }

  #buscador button {
    font-size: 1rem;
    padding: 8px 15px;
  }
  #estadisticas{
    grid-template-columns: repeat(1, 1fr);

  }
}
</style>
