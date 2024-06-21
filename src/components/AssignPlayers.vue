<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';

let continueGame = ref(false);

interface IPlayerProps {
    players: Player[];
};

const props = defineProps<IPlayerProps>();

const emit = defineEmits<{
    (e: "startGame"): void;
    (e: "continueGame"): void;
}>();



let playerSymbol = ref("");
const playerName = ref("");
let displayForm = ref(true);
let displayStartBtn = ref(false);

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
if (localStorage.length !== 0) {
    continueGame.value = true;
    displayForm.value = false;

}

</script>

<template>
    <div>
        <button v-if="continueGame" @click="$emit('continueGame')">Fortsätt spela</button>
        <form v-if="displayForm" @submit.prevent="handleSubmit">
            <input type="text" v-model="playerName" placeholder="Ange spelarnamn">
            <button>Nästa</button>
        </form>
        <button v-if="displayStartBtn" @click="handleClick">Starta spel</button>
    </div>
</template>

<style scoped></style>
