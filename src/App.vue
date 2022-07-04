<script setup>
import { ref } from 'vue';
import GameStage from './components/layout/GameStage.vue';
import ScoreCard from './components/layout/ScoreCardArea.vue';
// import { ref } from "vue";
const playerMoves = ref(0);
const totalMatches = ref(0);
const playTimer = ref(0);
const reset = ref(false);
const begin = ref(true);

function clearStage() {
	playerMoves.value = 0;
	totalMatches.value = 0;
	playTimer.value = 0;
	console.log('reset');
}
</script>

<template>
	<ScoreCard
		:playerMoves="playerMoves"
		:totalMatches="totalMatches"
		:playTimer="playTimer"
		:start="begin"
		@resetStage="clearStage"
		@beginThis="begin = !begin"
	/>
	<GameStage
		@updateTotalMatches="totalMatches++"
		@updatePlayerMoves="playerMoves++"
		:resetCards="reset"
		:displayWin="totalMatches == 12"
		:start="begin"
	/>
</template>

<style>
#app {
	display: flex;
	justify-content: space-evenly;
	align-items: center;
	height: 100vh;
}

/* @media in global.css  */
</style>
