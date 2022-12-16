<template>
    <div class="game">
        <div v-if="!isFinished" :style="fieldStyle " class="field">
            <div ref="fieldArr" @click="makeMove(cellNum-1)" v-for="cellNum in field_size*field_size" :key="cellNum" class="field__cell">
                <img class="field__icon" v-if="fieldArray[Math.floor((cellNum-1)/field_size)][(cellNum-1)%field_size] === 'o'" src="../assets/icons8-o-80.png" alt="O">
                <img class="field__icon" v-if="fieldArray[Math.floor((cellNum-1)/field_size)][(cellNum-1)%field_size] === 'x'" src="../assets/icons8-multiply-80.png" alt="X">
            </div>
        </div>
        <div class="retry" v-else>
            <h2 class="retry__header">{{isFinished}}</h2>
            <button class="retry__btn" @click="reset">Retry?</button>
        </div>
    </div>
</template>

<script setup>
import {ref, defineProps, computed} from "vue";

const props = defineProps(["field_size"]);

let fieldArray = ref([[],[],[],[],[],[],[],[]]);  
let isFinished = ref(false);

const fieldStyle = computed(() => {
    let cellsFr = "1fr ".repeat(props.field_size);
    return {'grid-template-columns': cellsFr.slice(0,cellsFr.length-1),
            'grid-template-rows': cellsFr.slice(0,cellsFr.length-1)}
})

const fieldArr = ref();

const reset = function(){
    fieldArray.value = [[],[],[],[],[],[],[],[]];
    isFinished.value = false;   
}

const checkDirs = function(row, column, symbol, winTresh){
    let concec = 1;
    let start_row = row;
    let start_column = column;
    while(row<props.field_size){
        row+=1;
        if(fieldArray.value[row][column] === symbol){
            concec++;
        }
        else{
            break;
        }
    }
    if(concec>=winTresh){
        return "finished";
    }

    concec = 1;
    row = start_row;
    while(column<props.field_size){
        column+=1;
        if(fieldArray.value[row][column] === symbol){
            concec++;
        }
        else{
            break;
        }
    }
    if(concec>=winTresh){
        return "finished";
    }

    concec = 1;
    column = start_column;
    while(column<props.field_size && row<props.field_size){
        column+=1;
        row+=1;
        if(fieldArray.value[row][column] === symbol){
            concec++;
        }
        else{
            break;
        }
    }
    if(concec>=winTresh){
        return "finished";
    }

    concec = 1;
    row = start_row;
    column = start_column;
    while(column<props.field_size && row<props.field_size){
        column-=1;
        row+=1;
        if(fieldArray.value[row][column] === symbol){
            concec++;
        }
        else{
            break;
        }
    }
    if(concec>=winTresh){
        return "finished";
    }
}

const checkWin = function(){

    let size = props.field_size;
    let winTresh = 3;
    
    if(size>3){
        winTresh = 4;
    }

    for(let i=0;i<size;i++){
        for(let j=0;j<size;j++){
            if(fieldArray.value[i][j]==='x'){
                if(checkDirs(i,j,'x',winTresh)==="finished"){
                    return true;
                }
            }
            else if(fieldArray.value[i][j]==='o'){
                if(checkDirs(i,j,'o',winTresh)==="finished"){
                    return true;
                }
            }
        }
    }

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
    let x = Math.floor(Math.random()*props.field_size);
    let y = Math.floor(Math.random()*props.field_size);
    while(fieldArray.value[x][y]!==undefined){
        x = Math.floor(Math.random()*props.field_size);
        y = Math.floor(Math.random()*props.field_size);
    }
    fieldArray.value[x][y] = 'o';
    if(checkWin()){
        highlightWinner('o');
        isFinished.value = null;
        setTimeout(()=>{
            isFinished.value = "CPU Won!";
        },3000)
    }
}

const makeMove = function(cellNumber){
    if(fieldArray.value[Math.floor(cellNumber/props.field_size)][cellNumber%props.field_size]===undefined && isFinished.value===false){
        fieldArray.value[Math.floor(cellNumber/props.field_size)][cellNumber%props.field_size] = 'x';
        if(checkWin()){
            highlightWinner('x');
            isFinished.value = null;
            setTimeout(()=>{
                isFinished.value = "You Won!";
            },3000)
        }
        else{
            botMove();
        }
    }
}

const highlightWinner = function(symbol){
    for(let i=0;i<props.field_size;i++){
        for(let j=0;j<props.field_size;j++){
            if(fieldArray.value[i][j] === symbol){
                fieldArr.value[i*props.field_size+j].style.background = "green";
            }
            else if(fieldArray.value[i][j] === undefined){
                fieldArr.value[i*props.field_size+j].style.background = "white";
            }
            else{
                fieldArr.value[i*props.field_size+j].style.background = "red";
            }
        }
    }
}

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

    .retry{
        font-size: 2em;
    }

    .retry__header{
        font-weight: bold;
    }

    .retry__btn{
        margin-top:1rem;
        appearance: none;
        border: dashed 3px white;
        color:white;
        font-weight: bold;
        font-size:1.2em;
        padding: 0.5rem 1.25rem;
        background-color:rgba(0,0,0,0);
    }
    .retry__btn:hover{
        cursor:pointer;
        background-color:rgb(196, 125, 38);
    }
</style>