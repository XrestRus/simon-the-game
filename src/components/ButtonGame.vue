<template>
    <button class="button"
            :style="{ background: color }"
            :class="{ 'button--active': isActive  }"
            @click="clickAction"
    >
    </button>
</template>

<script>
    export default {
        name: "ButtonGame",

        props: {
            color: {
                type: String
            },
            audio: {
                type: String,
                required: true,
            }
        },

        data() {
            return {
                isActive: false,
                sound: null,
            }
        },

        created() {
            this.sound = new Audio(this.audio);
            this.sound.crossOrigin="anonymous";
        },

        methods: {
            clickAction() {
                this.isActive = true;
                this.sound.play();

                setTimeout(() => {
                    this.isActive = false;
                }, 500);


                this.$emit('click');
            }
        }
    }
</script>

<style scoped>
    .button {
        outline: none;
        border: 0;
        transition: .3s;
    }

    .button--active {
        filter: opacity(0.5);
        transform: scale(1.1);
    }

    .button:hover {
        transform: scale(1.1);
    }


</style>