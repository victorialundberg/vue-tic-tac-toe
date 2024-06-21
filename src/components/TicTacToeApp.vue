<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';
import AssignPlayers from './AssignPlayers.vue';
import PrintPlayers from './PrintPlayers.vue';
import TheGame from './TheGame.vue';
import PrintPoints from './PrintPoints.vue';

let players = ref<Player[]>([]);
let gameOn = ref(false);
let firstPlayer = ref(0);
let pointsOn = ref(false);

const storedPlayers = localStorage.getItem("players")
const startGame = () => {
    gameOn.value = true;
    pointsOn.value = false;
    console.log("THE GAMES HAVE BEGUN");
    firstPlayer.value = Math.floor(Math.random() * 2)
}

const continueGame = () => {
    console.log("fortsätt spela");
    gameOn.value = true;
};

const clearGame = (value: boolean) => {
    gameOn.value = value;
    pointsOn.value = false;
};

const clearPlayers = (value: Player[]) => {
    players.value = value;
    pointsOn.value = false;
};

const displayPoints = () => {
    pointsOn.value = true;
};
const displayGame = () => {
    pointsOn.value = false;
};

const updatePlayers = () => {
    players = storedPlayers ? JSON.parse(storedPlayers) : players;
}

</script>

<template>
    <PrintPlayers :players="players"></PrintPlayers>
    <AssignPlayers v-if="!gameOn" :players="players" @start-game="startGame" @continue-game="continueGame">
    </AssignPlayers>
    <TheGame v-if="gameOn" :first-player="firstPlayer" :players="players" :game-on="gameOn" :points-on="pointsOn"
        @clear-game="clearGame" @clear-players="clearPlayers" @play-again="startGame" @display-points="displayPoints"
        @display-game="displayGame" @update-players="updatePlayers">
    </TheGame>
    <PrintPoints v-if="pointsOn" :players="players"></PrintPoints>
</template>

<style scoped></style>
