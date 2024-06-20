<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';

interface IPlayerProps {
    players: Player[];
};

const props = defineProps<IPlayerProps>();

const emit = defineEmits<{
    (e: "startGame"): void;
}>();

let playerSymbol = ref("");
const playerName = ref("");
let displayForm = ref(true);
let displayStartBtn = ref(false);
// let firstPlayer = ref(Math.floor(Math.random() * 2));

const handleSubmit = () => {

    if (props.players.length === 0) {
        playerSymbol.value = "X"

    } else if (props.players.length === 1) {
        playerSymbol.value = "O"
        displayForm.value = false;
        displayStartBtn.value = true;

    }
    props.players.push(new Player(playerName.value, playerSymbol.value, 0))
    playerName.value = "";
    localStorage.setItem("players", JSON.stringify(props.players));
};

const handleClick = () => {
    emit("startGame");
};


</script>

<template>
    <div>
        <form v-if="displayForm" @submit.prevent="handleSubmit">
            <input type="text" v-model="playerName" placeholder="Ange ditt namn">
            <button>Nästa</button>
        </form>
        <button v-if="displayStartBtn" @click="handleClick">Starta spel</button>
    </div>
</template>

<style scoped></style>
