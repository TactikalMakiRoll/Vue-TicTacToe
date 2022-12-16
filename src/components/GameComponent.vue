<template>
    <div class="game">
        <div :style="fieldStyle " class="field">
            <div ref="fieldArr" @click="makeMove(cellNum-1)" v-for="cellNum in field_size*field_size" :key="cellNum" class="field__cell">
                <img class="field__icon" v-if="fieldArray[Math.floor((cellNum-1)/field_size)][(cellNum-1)%field_size] === 'o'" src="../assets/icons8-o-80.png" alt="O">
                <img class="field__icon" v-if="fieldArray[Math.floor((cellNum-1)/field_size)][(cellNum-1)%field_size] === 'x'" src="../assets/icons8-multiply-80.png" alt="X">
                <!-- {{fieldArray[Math.floor((cellNum-1)/field_size)][(cellNum-1)%field_size]}} -->
            </div>
        </div>
    </div>
</template>

<script setup>
import {ref, defineProps, onMounted, computed} from "vue";

const props = defineProps(["field_size"]);

let fieldArray = ref([[],[],[],[],[],[],[],[]]);  

const fieldStyle = computed(() => {
    let cellsFr = "1fr ".repeat(props.field_size);
    return {'grid-template-columns': cellsFr.slice(0,cellsFr.length-1),
            'grid-template-rows': cellsFr.slice(0,cellsFr.length-1)}
})

const fieldArr = ref();


const checkWin = function(){
    let finished = true;
    for(let i=0;i<props.field_size;i++){
        for(let j=0;j<props.field_size;j++){
            if(fieldArray.value[i][j]===undefined){
                finished = false;
                break;
            }
        }
    }
    return finished;
}

const botMove = function(){
    if(!checkWin()){
        let x = Math.floor(Math.random()*props.field_size);
        let y = Math.floor(Math.random()*props.field_size);
        while(fieldArray.value[x][y]!==undefined){
            x = Math.floor(Math.random()*props.field_size);
            y = Math.floor(Math.random()*props.field_size);
        }
        console.log("x,y\n" + x + " " + y)
        fieldArray.value[x][y] = 'o';
    }
}

const makeMove = function(cellNumber){
    if(fieldArray.value[Math.floor(cellNumber/props.field_size)][cellNumber%props.field_size]===undefined){
        fieldArray.value[Math.floor(cellNumber/props.field_size)][cellNumber%props.field_size] = 'x';
        console.log("somebody, make a move");
        console.log(fieldArray.value);
        botMove();
    }
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
        display: inline-grid;
        aspect-ratio: 1;
        margin: 0 auto;
        border: 4px dashed white;
    }
    .field__cell{
        min-width:4.5rem;
        min-height:4.5rem;
        aspect-ratio: 1;
        object-fit: fill;
        border: 4px solid rgb(43, 1, 82);
        background-color: rgb(209, 209, 209);
        color:black;
        font-size:2em;
    }
    .field__cell:empty:hover{
        cursor:pointer;
    }
    .field__icon{
        width: 4rem;
    }
</style>