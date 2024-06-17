<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player'
import AssignPlayers from './AssignPlayers.vue'

const players = ref<Player[]>([new Player("victoria", "X", 0), new Player("victoria", "X", 0)]);

</script>

<template>
    <div v-for="(player, i) in players" :key="i">
        {{ player.name }}
    </div>
    <AssignPlayers :players="players"></AssignPlayers>
</template>

<style scoped></style>
