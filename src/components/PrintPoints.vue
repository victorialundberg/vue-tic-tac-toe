<script setup lang="ts">


const displayGame = () => {
    emit("displayGame");
}
const emit = defineEmits<{
    (e: "displayGame"): void;
}>();
</script>

<template>
    <table>
        <tr>
            <th>Namn</th>
            <th>Poäng</th>
        </tr>
        <tr>
            <td>Spelare 1</td>
            <td>Poäng 1</td>
        </tr>
        <tr>
            <td>Spelare 2</td>
            <td>Poäng 2</td>
        </tr>
    </table>
    <div>

    </div>
</template>