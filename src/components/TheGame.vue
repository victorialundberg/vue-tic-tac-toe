<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';

const gameBoard: number[][] = [[1, 2, 3], [4, 5, 6], [7, 8, 9]];
let gameDone = ref(false);

interface IGameProps {
    firstPlayer: number;
    players: Player[];
};
const props = defineProps<IGameProps>();

const draw = (i: number) => {
    console.log(i);


};

</script>

<template>
    <h2>{{ players[firstPlayer].symbol }} gör första draget!</h2>
    <div class="grid">
        <div v-for="(row, i) in gameBoard" :key="i" class="row">
            <div v-for="(i) in row" :key="i" class="grid-box" @click="() => { draw(i) }"></div>
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