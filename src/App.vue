<template>
  <main class="app">
    <h1 v-if=!field_size class="app__title"><img src="./assets/icons8-vue-js.svg" alt="Vue"> Tic Tac Toe</h1>
    <div v-if=!field_size class="options">
      <h2 class="options__description">
        A simple Tic Tac Toe app build with Vue 3 Composition API
      </h2>
      <h2 class="options__choose">
        Choose the number of cells
      </h2>
      <button @click="()=>{field_size = 3}" class="options__size-btn">3x3</button>
      <button @click="()=>{field_size = 5}" class="options__size-btn">5x5</button>
      <button @click="()=>{field_size = 7}" class="options__size-btn">7x7</button>
    </div>
    <GameComponentVue v-if="field_size" :field_size="field_size" />
  </main>
</template>

<script setup>
import { ref } from "vue";
import GameComponentVue from "./components/GameComponent.vue";

const field_size = ref(null)


</script>

<style>
  .app{
    text-align: center;
    padding: 2rem 2rem 0 2rem;
  }
  .app__title{
    font-size:3em;
    font-weight: bold;
  }
  .options{
    padding: 2rem 0;
  }
  .options__description{
    font-size:1.1em;
  }
  .options__choose{
    font-size: 1.25em;
    margin: 2rem 0 2rem 0;
  }
  .options__size-btn{
    appearance: none;
    border: dashed 3px white;
    color:white;
    font-weight: bold;
    font-size:1.2em;
    padding: 0.5rem 1.25rem;
    background-color:rgba(0,0,0,0);
  }
  .options__size-btn:hover{
    cursor:pointer;
    background-color:rgb(196, 125, 38);
  }
  .options__size-btn + .options__size-btn{
    margin-left:1.5rem;
  }
</style>
