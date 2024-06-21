<script setup lang="ts">
import { Player } from '../models/Player';
// Get from state instead for local storage

interface IPointsProps {
    players: Player[];
}

defineProps<IPointsProps>();

</script>

<template>
    <table>
        <tr>
            <th>Namn</th>
            <th>Poäng</th>
        </tr>
        <tr>
            <td>{{ players[0].name }}</td>
            <td>{{ players[0].points }}</td>
        </tr>
        <tr>
            <td>{{ players[1].name }}</td>
            <td>{{ players[1].points }}</td>
        </tr>
    </table>
    <div>

    </div>
</template>