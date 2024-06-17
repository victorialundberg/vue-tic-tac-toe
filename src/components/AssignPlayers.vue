<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';

interface IPlayerProps {
    players: Player[];
};

const props = defineProps<IPlayerProps>();

console.log(props.players.length);



const buttonText: string = "Nästa";
const playerName = ref("");
const handleSubmit = () => {
    console.log("submit", playerName.value);
    playerName.value = "";
};
</script>

<template>
    <div>
        <form @submit.prevent="handleSubmit">
            <input type="text" v-model="playerName" placeholder="Ange ditt namn">
            <button>{{ buttonText }}</button>
        </form>
    </div>
</template>

<style scoped></style>
