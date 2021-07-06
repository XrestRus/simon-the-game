<template>
    <div class="info-bar">
        <div class="status">
            <button class="start"
                    @click="e => startAction(e)"
                    ref="buttonStart"
            >
                Начать
            </button>
            <h1 class="status__present"
                v-show="currentRounds"
            >
                Раунд: {{ currentRounds }}
            </h1>
        </div>
        <div class="levels">
            <fieldset class="levels__group">
                <legend>
                    Уровни игры
                </legend>
                <label class="group__item"
                       v-for="level of levels"
                       :key="level.id"
                       :for="level.name"
                >
                    <input type="radio"
                           name="level"
                           :id="level.name"
                           :checked="currentLevel.id === level.id"
                           @change="_ => $emit('change', level)"
                    >
                    <span>{{ level.title }}</span>
                </label>
            </fieldset>
        </div>
    </div>
</template>

<script>

    export default {
        name: "InfoBar",

        model: {
            prop: 'currentLevel',
            event: 'change'
        },

        props: {
            levels: {
                type: Array,
                default: null
            },
            currentLevel: {
                type: Object,
                default: null
            },
            currentRounds: {
                type: Number,
                default: 1
            }
        },

        methods: {
            startAction(e) {
                this.$emit('startAction', e, this.$refs['buttonStart']);
            }
        }
    }
</script>

<style scoped>
    .info-bar {
        display: flex;
        flex-wrap: wrap;
        flex-direction: column;
        border-radius: 20px 20px 0 0;
        padding: 1rem;
        box-shadow: 0 1px 6px 0 var(--color-border);
        user-select: none;
    }

    .status {
        display: flex;
        justify-content: space-between;
        flex-wrap: wrap;
    }

    .start {
        flex: 1;
        margin: 0 1rem 0 0;
        max-width: 250px;
        min-height: 40px;
    }

    .levels{
        display: flex;
        align-items: center;
        flex-wrap: wrap;
    }

    .levels__group {
        display: flex;
        flex: 1;
        padding: 0.2rem;
        flex-wrap: wrap;
        border: 1px solid var(--color-border);
    }

    .group__item {
        display: flex;
        flex-wrap: wrap;
        flex: 1;
    }

    .group__item input {
        flex: 1;
    }

    .group__item span {
        flex: 1;
    }
</style>