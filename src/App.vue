<script setup>
import { ref } from 'vue';
import GameStage from './components/layout/GameStage.vue';
import ScoreCard from './components/layout/ScoreCardArea.vue';

const playerMoves = ref(0);
const totalMatches = ref(0);
const playTimer = ref(0);
const reset = ref(false);
const start = ref(true);
let intervalID;

function startTime() {
	setTimeout(() => {
		if (!intervalID) {
			intervalID = setInterval(() => {
				playTimer.value++;
			}, 2000);
		}
	}, 3000);
}
function stopTime() {
	clearInterval(intervalID);
	intervalID = null;
}

function clearEvent() {
	playTimer.value = 0;
	playerMoves.value = 0;
	totalMatches.value = 0;
	startTime();
	reset.value = !reset.value;
}
function resetEvent() {
	stopTime();
	reset.value = !reset.value;
}

function startGame() {
	startTime();
	start.value = false;
	reset.value = true;
}
</script>

<template>
	<ScoreCard
		:totalMatches="totalMatches"
		:playerMoves="playerMoves"
		:playTimer="playTimer"
		:start="start"
		:reset="reset"
		@resetEmit="resetEvent"
		@beginEmit="startGame"
		@playAgainEmit="clearEvent"
	/>
	<GameStage
		v-if="reset"
		:gameOver="totalMatches == 12"
		:start="start"
		@updateTotalMatches="totalMatches++"
		@updatePlayerMoves="playerMoves++"
		@startOver="resetEvent"
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
