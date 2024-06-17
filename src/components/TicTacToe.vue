<script setup lang="ts">
import { ref } from 'vue';
import { Player } from '../models/Player';
import AssignPlayers from './AssignPlayers.vue';
import PrintPlayers from './PrintPlayers.vue';

const players = ref<Player[]>([]);


</script>

<template>
    <PrintPlayers :players="players"></PrintPlayers>
    <AssignPlayers :players="players"></AssignPlayers>
</template>

<style scoped></style>
