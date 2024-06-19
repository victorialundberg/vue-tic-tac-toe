<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';
import AssignPlayers from './AssignPlayers.vue';
import PrintPlayers from './PrintPlayers.vue';
import TheGame from './TheGame.vue';

const players = ref<Player[]>([]);
let gameOn = ref(false);
let firstPlayer = ref(0);

const startGame = (index: number) => {
    gameOn.value = true;
    console.log("THE GAMES HAVE BEGUN");
    firstPlayer.value = index;
}

const clearGame = (value: boolean) => {
    gameOn.value = value;
};

const clearPlayers = (value: Player[]) => {
    players.value = value;
};

</script>

<template>
    <PrintPlayers :players="players"></PrintPlayers>
    <AssignPlayers v-if="!gameOn" :players="players" @start-game="startGame"></AssignPlayers>
    <TheGame v-if="gameOn" :first-player="firstPlayer" :players="players" :game-on="gameOn" @clear-game="clearGame"
        @clear-players="clearPlayers">
    </TheGame>
</template>

<style scoped></style>
