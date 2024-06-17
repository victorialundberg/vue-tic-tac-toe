<script setup lang="ts">
import { Player } from '../models/Player'
interface IPlayerProps {
    players: Player[];
};

const props = defineProps<IPlayerProps>();


</script>

<template>
    <div v-for="(player, i) in props.players" :key="i">
        <h4>{{ player.name }} - {{ player.symbol }}</h4>
    </div>

</template>