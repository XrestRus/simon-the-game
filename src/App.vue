<template>
    <div id="app">
        <div class="game-layout">
            <div class="buttons">
                <button-game v-for="b in buttons"
                             :key="b.id"
                             :color="b.color"
                             :audio="b.audio"
                             @click="move(b)"
                             ref="buttonGame"
                />
            </div>
            <info-bar :levels="levels"
                      :current-rounds="currentRounds"
                      v-model="currentLevel"
                      @startAction="start"
            />
        </div>
        <turn :is-turn="isBotTurn" />
    </div>
</template>

<script>
    import ButtonGame from "@/components/ButtonGame";
    import InfoBar from "@/components/InfoBar";
    import { levels } from "@/utilits/levels";
    import {buttons} from "@/utilits/buttons";
    import Turn from "@/components/Turn";

    export default {
        name: 'App',

        components: {
            Turn,
            InfoBar,
            ButtonGame
        },

        data() {
            return {
                buttons: buttons,
                levels: levels,
                currentLevel: levels[2],
                currentMoves: [],
                currentRounds: null,
                isPlayerTurn: false,
                isBotTurn: false,

                TIMEPAUSE: 500,
            }
        },

        methods: {
            start(e, refButton) {
                refButton.disabled = true;

                this.reset();
                this.initRound();

                refButton.disabled = false;
            },

            reset() {
                this.currentRounds = 0;
                this.currentMoves = [];
            },

            async initRound() {
                this.isPlayerTurn = false;
                this.currentRounds++;

                await this.generationOfMove();

                this.isPlayerTurn = true;
            },

            async generationOfMove() {
                this.isBotTurn = true;
                for (let i = 0; i < this.currentRounds; i++) {
                    await new Promise((res) =>
                        setTimeout(() => {

                            let rnd = Math.floor(Math.random() * 4);
                            this.currentMoves.push(rnd)
                            this.$refs.buttonGame[rnd].clickAction();

                            res();
                        }, this.currentLevel.value + this.TIMEPAUSE)
                    );
                }
                this.isBotTurn = false;
            },

            move(button) {
                if (this.isPlayerTurn) {
                    if (this.currentMoves[0] === button.id) {
                        this.currentMoves.shift();
                    } else {
                        alert(`Вы проиграли, ваш раунд: ${ this.currentRounds }`)
                        this.isPlayerTurn = false;
                        this.reset();
                    }
                } else {
                    console.log(button);
                }
                if (this.currentMoves.length === 0 && this.isPlayerTurn) {
                    setTimeout(() => this.initRound(), this.TIMEPAUSE)
                }
            }
        }
    }
</script>

<style>
    * {
        padding: 0;
        margin: 0;
        box-sizing: border-box;
    }

    :root {
        --color-turn: #ff000066;
        --color-shadow-turn: #ff0000;

        --color-border: #0000002b;
    }

    #app {
        min-height: 100vh;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .game-layout {
        display: flex;
        flex-direction: column;
        border-radius: 20px 20px 0 0;
        box-shadow: 0 1px 6px 0 var(--color-border);
    }

    .buttons {
        display: grid;
        grid-template-columns: 1fr 1fr;
        grid-gap: 1rem;
        max-width: 400px;
        max-height: 400px;
        height: 79vw;
        width: 79vw;
        overflow: hidden;
        border-radius: 100%;
        box-shadow: 0 0 3px 4px var(--color-border);
        border: 6px solid transparent;
        margin: 1rem;
    }


</style>
