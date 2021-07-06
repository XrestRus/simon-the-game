<template>
    <div v-show="isTurn" class="loader"></div>
</template>

<script>
    export default {
        name: "Turn",

        props: {
            isTurn: {
                type: Boolean,
                default: false
            }
        }
    }
</script>

<style scoped>

    .loader {
        width: 20px;
        height: 20px;
        background: var(--color-turn);
        position: fixed;
        right: 0;
        top: 0;
        border-radius: 100%;
        margin: 1rem;
        animation: load 1.5s infinite;
    }

    @keyframes load {
        0% {
            box-shadow: 0 0 0 0;
        }
        50% {
            box-shadow: 0 0 20px 0 var(--color-shadow-turn),
                    0 0 8px 0 var(--color-shadow-turn);
        }
        100% { box-shadow: 0 0 0 0; }
    }
</style>