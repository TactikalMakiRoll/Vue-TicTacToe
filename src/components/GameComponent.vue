<template>
    <div class="game">
        <div :style="fieldStyle " class="field">
            <div ref="fieldArr" @click="makeMove(cellNum)" v-for="cellNum in field_size*field_size" :key="cellNum" class="field__cell">
                <img v-if="fieldArray[cellNum] === 'x'" src="../assets/icons8-multiply-80.png" alt="X">
                <img v-if="fieldArray[cellNum] === 'o'" src="../assets/icons8-o-80.png" alt="O">
            </div>
        </div>
    </div>
</template>

<script setup>
import {ref, defineProps, onMounted, computed} from "vue";

const props = defineProps(["field_size"]);

let fieldArray = ref(null);
fieldArray.value = [];  

const fieldStyle = computed(() => {
    let cellsFr = "1fr ".repeat(props.field_size);
    return {'grid-template-columns': cellsFr.slice(0,cellsFr.length-1),
            'grid-template-rows': cellsFr.slice(0,cellsFr.length-1)}
})

const fieldArr = ref();

const makeMove = function(cellNumber){
    cellNumber;
}


onMounted(()=>{
    // fieldArray.value = [];
    // for(let i=0;i<props.field_size;i++){
    //     let row = [].fill(" ",0,props.field_size);
    //     fieldArray.value.push(row); 
    // }
})

</script>

<style>
    .game{
        padding:2rem 0;
    }
    .field{
        display: grid;
        min-width: calc(12rem + 8px);
        max-width: min(50vw,75vh);
        aspect-ratio: 1;
        margin: 0 auto;

        border: 4px dashed white;
    }
    .field__cell{
        border: 2px solid white;
        width: 100%;
        height: 100%; 
        min-width:4rem;
        min-height:4rem;
        aspect-ratio: 1;
        object-fit: fill;
    }
</style>