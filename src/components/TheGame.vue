<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';
import { defineEmits } from 'vue';

const gameBoard = ref([["", "", ""], ["", "", ""], ["", "", ""]]);
let gameDone = ref(true);
let firstMove = ref(true);
let gameOn = ref(true);

interface IGameProps {
    firstPlayer: number;
    players: Player[];
    gameOn: boolean;
};
const props = defineProps<IGameProps>();

let playerTurn = ref(props.players[props.firstPlayer].symbol);

const makeMove = (x: number, y: number) => {


    if (gameBoard.value[x][y] === "") {
        if (playerTurn.value === "X") {
            gameBoard.value[x][y] = "X";
            playerTurn.value = "O"
        } else if (playerTurn.value === "O") {
            gameBoard.value[x][y] = "O";
            playerTurn.value = "X";
        }
    }
    firstMove.value = false;



    // function for calculating win
};

// function for calculating win

const playAgain = () => {
    console.log("före");

    emit('playAgain');
    console.log("efter");
    playerTurn = ref(props.players[props.firstPlayer].symbol);
    console.log(props.firstPlayer);


    gameBoard.value = [["", "", ""], ["", "", ""], ["", "", ""]];
    firstMove.value = true;
};

const clearGame = () => {
    gameOn.value = false;
    emit("clearGame", gameOn.value);
    emit("clearPlayers", []);
    gameBoard.value = [["", "", ""], ["", "", ""], ["", "", ""]];
};
const emit = defineEmits<{
    (e: "clearGame", value: boolean): void;
    (e: "clearPlayers", value: Player[]): void;
    (e: "playAgain"): void;
}>();

</script>

<template>
    <h2 v-if="firstMove">{{ playerTurn }} gör första draget!</h2>
    <div class="grid">
        <div v-for="(row, rowIndex) in gameBoard" :key="rowIndex" class="row">
            <div v-for="(box, boxIndex) in row" :key="boxIndex" class="grid-box"
                @click="() => { makeMove(rowIndex, boxIndex) }">
                {{ gameBoard[rowIndex][boxIndex] }}
            </div>
        </div>
    </div>
    <div>
        <button>Visa poängställning</button>
        <button @click="playAgain">Spela igen</button>
        <button v-if="gameDone" @click="clearGame">Avsluta</button>
    </div>
</template>

<style scoped>
.grid-box {
    margin: 3px;
    padding: 0;
    width: 100px;
    height: 100px;
    border: 2px solid black;


    &:hover {
        background-color: lightgray;
        cursor: pointer;
    }
}

.row {
    display: flex;
}
</style>