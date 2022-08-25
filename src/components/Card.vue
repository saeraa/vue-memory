<template>
    <div class="card-container">
        <div class="content">
            <div class="front" @click="$emit('pickedCard', $event.target)"></div>
            <div class="back">{{ animal }}</div>
        </div>
    </div>
</template>

<script>
export default {
    name: "Card",
    emits: ["pickedCard"],
    props: ["animal", "pickCard"],
    methods: {
        selectedCard(card) {
            this.$emit("pickedCard", card)
        }
    }
}
</script>

<style>
</style>