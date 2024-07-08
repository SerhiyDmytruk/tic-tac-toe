<script setup>

import {ref} from 'vue';

const xPlayer = ref('X');
const oPlayer = ref('O');
let toggle = ref(false);
let winnerBlock = ref(false);
let title = xPlayer.value;

function turn() {
    toggle.value = !toggle.value

    if(toggle.value) return xPlayer.value;
    
    return oPlayer.value;
};

let row = ref(['','','', '','','', '','','']);

const tap = function(event, index, arr) {
    event.target.innerText = turn();
    title = event.target.innerText;
    event.target.disabled = true;
    arr[index] = event.target.innerText;

    if(winner(arr)) {
        winnerBlock.value = true;
        return false;
    }
    
}

function winner(squares) {
    const winComb = [
        [0, 1, 2],
        [3, 4, 5],
        [6, 7, 8],
        [0, 3, 6],
        [1, 4, 7],
        [2, 5, 8],
        [0, 4, 8],
        [2, 4, 6],
    ];

    for(let i = 0; i < winComb.length; i++) {
        const [a,b,c] = winComb[i];
        if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
            return squares[a];
        }
    }
    return null;
}

function clear() {
    row = ref(['','','', '','','', '','','']);
    winnerBlock.value = false;
}

</script>

<template>
    <div class="rounded-xl overflow-hidden w-1/2 m-auto">
        <div class="text-black text-3xl text-center font-bold">
            <p class="text-4xl">Tic-Tac-Toe</p>
            <span>Player <span>{{title}}</span>'s turn </span>

            <div v-if="winnerBlock" class="pt-8 text-6xl">
                <p>Player {{title}} win</p>
                <button 
                    class="bg-emerald-500 p-4 rounded-lg text-white"
                    @click="clear"
                >New Game!</button>
            </div>
            
        </div>
        <div class="relative rounded-xl overflow-auto p-8 not-prose">
            <div 
                class="grid grid-cols-3 gap-1 font-mono text-white text-sm text-center font-bold leading-10 rounded-lg">
                <div 
                v-for="(button, index) in row"
                :key="index"
                class="p-8 rounded-lg shadow-lg bg-pink-500">
                    <button 
                        class="w-full h-full text-5xl"
                        :class="{
                            'pointer-events-none': winnerBlock,
                        }"
                        @click="tap($event, index, row)">
                    </button>
                </div>
            </div>
        </div>
    </div>
</template>
