<script>
import Content from './components/layout/Content.vue';
import Game from './components/puzzle/Game.vue';

export default {
    name: 'App',
    components: {
        Content,
        Game,
    },
    data() {
        return {
            title: 'Play the 15 Puzzle Game',
        };
    },
};
</script>

<template>
    <div id="#app">
        <Content>
            <Game />
        </Content>
    </div>
</template>

<style>
body {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;

    background-color: #fff;
    color: #2c3e52;

    -webkit-tap-highlight-color: rgba(0, 0, 0, 0);
    -webkit-tap-highlight-color: transparent;

    font-size: 1rem;
}
</style>

<style src="./assets/styles/normalize.css"></style>
