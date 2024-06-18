<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';

const gameBoard = ref([["", "", ""], ["", "", ""], ["", "", ""]]);
let gameDone = ref(false);


interface IGameProps {
    firstPlayer: number;
    players: Player[];
};
const props = defineProps<IGameProps>();

let playerTurn = ref(props.players[props.firstPlayer].symbol);

const draw = (x: number, y: number) => {
    if (playerTurn.value === "X") {
        gameBoard.value[x][y] = "X";
        playerTurn.value = "O"
    } else if (playerTurn.value === "O") {
        gameBoard.value[x][y] = "O";
        playerTurn.value = "X";
    }

};

</script>

<template>
    <h2>{{ players[firstPlayer].symbol }} gör första draget!</h2>
    <div class="grid">
        <div v-for="(row, rowIndex) in gameBoard" :key="rowIndex" class="row">
            <div v-for="(box, boxIndex) in row" :key="boxIndex" class="grid-box"
                @click="() => { draw(rowIndex, boxIndex) }">
                {{ gameBoard[rowIndex][boxIndex] }}
            </div>
        </div>
    </div>
    <div>
        <button>Visa poängställning</button>
        <button>Avsluta</button>
        <button v-if="gameDone">Spela igen</button>
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