
<script setup>
import { ref, watchEffect } from 'vue'
import todoItem from './components/todoItem.vue'

const API_URL = `https://api.coingecko.com/api/v3/coins/`
const coinid = ref ('bitcoin')
const currentCoin = ref('bitcoin')
const coins = ref(null)

const conectado = ref(false)
const existemoneda = ref(false)


  // este se ejecuta al momento 
  // y se vuelve a ejecutar cuando coinid.value cambia
  watchEffect(async () => {
  const url = `${API_URL}${coinid.value}`
  try{
    conectado.value = true

  if ( await (await fetch(url)).ok ) {
       existemoneda.value = true 
       coins.value = await (await fetch(url)).json()
  }
  else existemoneda.value = false
  
  }
  catch (e) {
    if (e instanceof TypeError) {
        conectado.value = false;   
    } else {
      conectado.value = false;
       // sentencias para manejar cualquier excepción no especificada
       logMyErrors(e); // pasa el objeto de la excepción al manejador de errores

}
  }

})
           


      

const groceryList = ref([
  { id: 0, text: 'Vegetales' },
  { id: 1, text: 'Queso' },
  { id: 2, text: 'Cualquier cosa comestible' }
])

const message = ref('Hola Mundo de Vue')
const newmessage = ref('')
const madd = ref('')
const isRed = ref(true)
const color = ref('green')

const show = ref(true)
const list = ref([1, 2, 3])

const text = ref('Escribe algo Aquí')
const checked = ref(true)
const checkedNames = ref(['Juan'])
const picked = ref('Uno')
const selected = ref('A')
const tselected = ref('Básico')
const multiSelected = ref(['A'])

function cambiaCoin(){
  coinid.value = currentCoin.value 
}


function toggleRed() {
  isRed.value = !isRed.value
}


function toggleColor() {
  color.value = color.value === 'green' ? 'blue' : 'green'
  
  console.log(color)
}

function reverseMessage() {
  // Accede o cambia el valor de un ref atravez de su propiedad 
  message.value = message.value.split('').reverse().join('')
}

function changeMessage() {
  // Accede o cambia el valor de un ref atravez de su propiedad 
  message.value = newmessage.value
}

function notify() {
  alert('El link fue detenido!')
}
</script>

<template>
<h2>Tutorial de Vue usando vite + Composition API</h2>
 <select v-model="tselected">
    <option disabled value="">Seleccione el tutorial a ver:</option>
    <option>Básico</option>
    <option>Intermedio</option>
   <option>Avanzado</option>
  </select>
  <hr>

<div id="btutorial" class="doculto" v-if="tselected ==='Básico'">

<div class="general">
<!--
    Dentro de los templates no necesitas poner message.value, es automatico
  -->
  <h1>{{ message }}</h1>

  <!--
    llama a un metodo/funcion.
    la directiva @click es lo mismo que  v-on:click.
  -->
  <button @click="reverseMessage">Invertir mensaje</button>

  <!-- Tambien se puede usar una expresion in-line -->
<div class="container">
  <button @click="message += madd">Adicionnar :</button>  
  <input type="text" v-model="madd">
</div>

    <!-- y ser dinamicamente cambiado con la directiva v-model -->
<div class="container">
  <button @click="changeMessage">Cambia el mensaje:</button>
  <input type="text" v-model="newmessage" >
</div>

</div>
<br>

<div class="general">

  <!--
    Vue tambien no permite utilizar modificadores para tareas 
    mas comunes como e.preventDefault() y e.stopPropagation()
  -->
  <a href="https://vuejs.org" @click.prevent="notify">
    Un Vínculo con e.preventDefault()
  </a>

  <p>
    <span :title="message">
      Detenga su Mouse encima de mi por unos segundos para ver mi título de forma dinámica!      
    </span>
  </p>

   <!--
  los enlaces de clasestienen especial soporte para clases y arreglos
  -->
  <p :class="{ red: isRed }" @click="toggleRed">
    Esto debería ser rojo... clickeame para cambiarme.
  </p>

  <!-- los enlaces de estilos tambien soportan arreglos y objetos -->
   <p :style="{color}" @click="toggleColor">
    Esto deberia ser verde, y cambiar a azul cuando me das un click.
  </p>
  </div>
<br>
 <div class="general">
<!--
Tambien podemos mostrar contenido opcional o en un ciclo 
con las directivas v-if y v-for 
-->
  <button @click="show = !show">Esconder Lista</button>
  <button @click="list.push(list.length + 1)">Insertar número</button>
  <button @click="list.pop()">Borrar número</button>
  <button @click="list.reverse()">Invertir Lista</button>

  <ul v-if="show && list.length">
    <li v-for="item of list" :key="item.value">{{ item }}</li>
  </ul>
  <p v-else-if="list.length">La lista no está vacía, está oculta.</p>
  <p v-else>Lista vacía.</p>

</div> 
<br>

  <div class="general">
<!--
Se pueden crear enlaces hascia ambos lados usandola directiva v-model .
-->

  <h2>Entrada de Texto</h2>
  <div class="container">
  <input v-model="text"> {{ text }}
  </div>
  </div>
  <br>
  <div class="general">
  <h2>Selección simple</h2>
  <div class="container">
  <input type="checkbox" id="checkbox" v-model="checked">
  <label for="checkbox">Marcado: {{ checked }}</label>
  </div>
</div>
<br>
<div class="general">
  <!--
   y enlazar multiples checkboxes al mismo arreglo usando v-model 
  -->
  <h2>Selección múltiple</h2>
  <div class="containerm">
    <div class="containeb">
       <div class="containerl">
        <input type="checkbox" id="juan" value="Juan" v-model="checkedNames">
        <label for="Juan">Juan</label> 
      </div>
      <div class="containerl">
        <input type="checkbox" id="pedro" value="Pedro" v-model="checkedNames">
        <label for="pedro">Pedro</label>
      </div>
      <div class="containerl">
        <input type="checkbox" id="alberto" value="Alberto" v-model="checkedNames">
        <label for="alberto">Alberto</label>
      </div>
    </div>    
    <div class="containeb">
     Nombres marcados: <pre><span class="coin"> {{ checkedNames }}</span></pre>
    </div>
  </div>
  </div>
  <br>
  <div class="general">
  <h2>Radio</h2>
  <div class="container">
  <input type="radio" id="uno" value="Uno" v-model="picked">
  <label for="uno">Uno</label></div>
  <br>
  <div class="container">
  <input type="radio" id="dos" value="Dos" v-model="picked">
  <label for="dos">Dos</label></div>
  <br>
  <span>Seleccionado: <span class="coin"> {{ picked }}</span></span>
</div>
<br>
<div class="general">
  <h2 >Lista de Selección simple</h2>
  <select v-model="selected">
    <option disabled value="">Seleccione uno por favor.</option>
    <option>A</option>
    <option>B</option>
   <option>C</option>
  </select>
  <span> Seleccionado: <span class="coin">{{ selected }}</span></span>
  </div>
<br>
<div class="general">
   <h2>Lista de Selección múltiple</h2>
  <select v-model="multiSelected" multiple style="width:100px">
    <option>A</option>
    <option>B</option>
    <option>C</option>
  </select>
  <span> Seleccionado: <span class="coin"> {{ multiSelected }}</span></span>
  </div>
  <br>
<div class="general">
  <ol>
    <!--
      ahora mostramos una lista de tareas con objetos 
      con el contenido dinamico,por lo que necesitamos ponerle una key
      para poder usar el v-for.
      
    -->
  <todoItem
      v-for="item in groceryList"
      :todo="item"
      :key="item.id"
    ></todoItem>
  </ol>
  </div>

  <h2>Fin del Tutorial Básico</h2>
  </div> 

  <div v-else-if="tselected ==='Intermedio'">

  <div class="container">
    <button @click="cambiaCoin">Moneda :</button>  
    <input type="text" v-model="currentCoin"> Sugerencias: bitcoin, ethereum, litecoin, cardano, solana, tether
  </div>
  <div class="general" v-if="existemoneda && conectado">
    <img :src='coins.image.small' alt="Cardano IMG">
    <p> Moneda:  <span class="coin"> {{coins.name}}</span></p>
    <p> Símbolo:  <span class="coin"> {{coins.symbol}}</span></p>
    <p> Precio ahora:  <span class="coin"> {{coins.market_data.current_price.usd}} USD</span></p>
    <p> <span class="coin">{{coins.symbol}}</span> es una : <span class="coin">{{coins.categories[0]}}
    </span></p>
  </div>

  <div class="general" v-else-if="!existemoneda && conectado">
    <p> Moneda:   <span class="red"> No encontrada</span></p>
    <p> Símbolo:  <span class="coin"> - </span></p>
    <p> Precio ahora:  <span class="coin"> - </span></p> 
  </div>

   <div class="general" v-else-if="!conectado">
    <p> Moneda:   <span class="red"> No Tienes conección a Internet</span></p>
    <p> Símbolo:  <span class="coin"> - </span></p>
    <p> Precio ahora:  <span class="coin"> - </span></p> 
  </div>
  

  
  </div>

  <div v-else-if="tselected ==='Avanzado'">


  <h1>Aqui va el tutorial Avanzado!</h1>
  </div>
  
  

</template>

<style>
*{
  font-family: sans-serif;
}

.coin{
  font-weight: bold;
}

button, a, input, h1, h2, h3, h4, text  {
  display: flex;
  margin: 3px;
}

.general{
  background-color: rgb(250, 250, 250);
  padding: 3px;
  padding-left: 10px;
  padding-top: 5px;
  border: 1px solid;
  border-radius: 5px;
  border-color: black;
  width: 60vmax;
}

.container{
  background-color: rgb(250, 250, 250);
  width:50vmax;
  border-radius: 3px;

  display: flex;
  flex-direction: row;
  justify-content: left;
  align-items: center;
}
.containerm{
  background-color: rgb(250, 250, 250);
  width:50vmax;
  border-radius: 3px;
  margin: 3px;

  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: center;
  
}
.containerb{
  background-color: rgb(250, 250, 250); 
  display: block;   
}
.containerl{
  background-color: rgb(250, 250, 250);
  display: flex;
  flex-direction: row;
  justify-content: left;
  align-items: center;
}



.red {
  color: red;
}



</style>