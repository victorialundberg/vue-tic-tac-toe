<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';
import { defineEmits } from 'vue';


let gameBoard = ref()
let displayPlayAgain = ref(false);
let firstMove = ref(true);
let gameOn = ref(true);
let displayWinner = ref(false);
let displayTied = ref(false);
let turnCounter = ref(0);
let winner = ref<Player>();
let displayBoard = ref(true);

let firstPlayerTurn = ref<Player>()
let playerTurn = ref();
let playerX = ref();
let playerO = ref();

let lastMove = ref();

interface IGameProps {
    firstPlayer: number;
    players: Player[];
    gameOn: boolean;
    pointsOn: boolean;
};
let emit = defineEmits<{
    (e: "clearGame", value: boolean): void;
    (e: "clearPlayers", value: Player[]): void;
    (e: "playAgain"): void;
    (e: "displayPoints"): void;
    (e: "displayGame"): void;
}>();
const props = defineProps<IGameProps>();

const storedPlayers = localStorage.getItem("players")
const storedGameBoard = localStorage.getItem("gameBoard");
// const storedLastMove = localStorage.getItem("lastMove");

if (localStorage.length !== 0) {
    gameBoard.value = storedGameBoard ? JSON.parse(storedGameBoard) : [["", "", ""], ["", "", ""], ["", "", ""]]
    firstPlayerTurn.value = storedPlayers ? JSON.parse(storedPlayers)[props.firstPlayer] : null;
    playerTurn.value = firstPlayerTurn.value?.symbol;
    playerX.value = storedPlayers ? JSON.parse(storedPlayers)[0] : null;
    playerO.value = storedPlayers ? JSON.parse(storedPlayers)[1] : null;
    if (winner) {
        // displayBoard.value = false;
        displayPlayAgain.value = true;
        firstMove.value = false;
    }
    // lastMove.value = storedLastMove;
} else {
    playerTurn.value = props.players[props.firstPlayer].symbol;
    playerX.value = props.players[0];
    playerO.value = props.players[1];
}



const makeMove = (x: number, y: number) => {


    if (gameBoard.value[x][y] === "") {
        if (playerTurn.value === "X") {
            gameBoard.value[x][y] = "X";
            calculateWin(playerTurn.value);
            // localStorage.setItem("lastMove", "X");
            playerTurn.value = "O"
        } else if (playerTurn.value === "O") {
            gameBoard.value[x][y] = "O";
            calculateWin(playerTurn.value);
            // localStorage.setItem("lastMove", "O");
            playerTurn.value = "X";
        }
    }
    firstMove.value = false;
    turnCounter.value++;
    localStorage.setItem("gameBoard", JSON.stringify(gameBoard.value));
};

const calculateWin = (playerTurn: string) => {
    if (gameBoard.value[0][0] === playerTurn && gameBoard.value[0][1] === playerTurn && gameBoard.value[0][2] === playerTurn ||
        gameBoard.value[1][0] === playerTurn && gameBoard.value[1][1] === playerTurn && gameBoard.value[1][2] === playerTurn ||
        gameBoard.value[2][0] === playerTurn && gameBoard.value[2][1] === playerTurn && gameBoard.value[2][2] === playerTurn ||

        gameBoard.value[0][0] === playerTurn && gameBoard.value[1][0] === playerTurn && gameBoard.value[2][0] === playerTurn ||
        gameBoard.value[0][1] === playerTurn && gameBoard.value[1][1] === playerTurn && gameBoard.value[2][1] === playerTurn ||
        gameBoard.value[0][2] === playerTurn && gameBoard.value[1][2] === playerTurn && gameBoard.value[2][2] === playerTurn ||

        gameBoard.value[0][0] === playerTurn && gameBoard.value[1][1] === playerTurn && gameBoard.value[2][2] === playerTurn ||
        gameBoard.value[0][2] === playerTurn && gameBoard.value[1][1] === playerTurn && gameBoard.value[2][0] === playerTurn
    ) {
        displayWinner.value = true;
        if (playerTurn === playerX.value.symbol) {
            props.players[0].points++
            winner = playerX;
        } else if (playerTurn === playerO.value.symbol) {
            props.players[1].points++
            winner = playerO;
        }
        displayPlayAgain.value = true;
        displayBoard.value = false;
        localStorage.setItem("players", JSON.stringify(props.players));
    }
    else if (turnCounter.value === 8) {
        displayTied.value = true;
        displayPlayAgain.value = true;
        displayBoard.value = false;
    }
    console.log(winner.value?.name);


};


const playAgain = () => {
    console.log("före");

    emit('playAgain');
    console.log("efter");
    playerTurn = ref(props.players[props.firstPlayer].symbol);
    console.log(props.firstPlayer);

    localStorage.removeItem("gameBoard");
    gameBoard.value = [["", "", ""], ["", "", ""], ["", "", ""]];
    firstMove.value = true;
    displayWinner.value = false;
    displayTied.value = false;
    displayPlayAgain.value = false;
    turnCounter.value = 0;
    displayBoard.value = true;

};

const clearGame = () => {
    gameOn.value = false;
    emit("clearGame", gameOn.value);
    emit("clearPlayers", []);
    localStorage.clear();
    gameBoard.value = [["", "", ""], ["", "", ""], ["", "", ""]];
};

const displayPoints = () => {
    emit("displayPoints");
};

const displayGame = () => {
    emit("displayGame");
    console.log("clicked");

};



</script>

<template>
    <h2 v-if="firstMove">{{ playerTurn }} gör första draget</h2>
    <div v-if="displayBoard" class="grid">
        <div v-for="(row, rowIndex) in gameBoard" :key="rowIndex" class="row">
            <div v-for="(box, boxIndex) in row" :key="boxIndex" class="grid-box"
                @click="() => { makeMove(rowIndex, boxIndex) }">
                {{ gameBoard[rowIndex][boxIndex] }}
            </div>
        </div>
    </div>
    <h2 v-if="displayTied">Det blev oavgjort!</h2>
    <h2 v-if="displayWinner">{{ winner?.name }} vann!</h2>

    <div>
        <button v-if="!pointsOn" @click="displayPoints">Visa poängställning</button>
        <button v-else @click="displayGame">Stäng poängställning</button>
        <button v-if="displayPlayAgain" @click="playAgain">Spela igen</button>
        <button @click="clearGame">Avsluta</button>
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