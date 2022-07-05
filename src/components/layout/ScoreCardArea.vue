<script setup>
import { ref } from 'vue';

// Props
defineProps(['playerMoves', 'totalMatches', 'playTimer', 'start', 'reset']);
// Emits
const emit = defineEmits(['resetEmit', 'beginEmit', 'playAgainEmit']);
// Vars
const loading = ref(false);
// Functions
function formatTimer(sec) {
	let minute = Math.floor(sec / 60);
	let seconds = (sec % 60).toString().length == 1 ? `0${sec % 60}` : sec % 60;
	return `${minute}: ${seconds}`;
}

// "Memory loading..." card delay,then start game
function startGame() {
	loading.value = true;
	setTimeout(() => {
		emit('beginEmit');
		loading.value = false;
	}, 2000);
}
</script>

<template>
	<div v-if="!start" id="scoreCard" class="unselectableHTML">
		<h1>MEMORY Score</h1>
		<p>
			Moves <span> {{ playerMoves }}</span>
		</p>
		<p>
			Matches <span>{{ totalMatches }}/12</span>
		</p>
		<p>
			Time
			<span>{{ formatTimer(playTimer) }}</span>
		</p>
		<button v-if="reset" @click.prevent="$emit('resetEmit')">Reset</button>
		<button v-else @click.prevent="$emit('playAgainEmit')">Play Again</button>
	</div>
	<transition name="animateShallWePlay" appear>
		<div v-if="start" id="scoreCard">
			<p v-if="!loading" class="shallWe">Shall we play a game?</p>
			<button v-if="!loading" @click.prevent="startGame">START</button>
			<div v-else class="animateLoading">
				<p>Loading Memory...</p>
			</div>
		</div>
	</transition>
</template>

<style lang="scss">
#scoreCard {
	background-color: #fff;
	color: black;
	display: flex;
	flex-wrap: wrap;
	justify-content: center;
	align-self: center;
	position: relative;
	flex: 0 0 150px;
	box-shadow: 2px 2px 5px rgb(50, 50, 50);
	padding: 1rem;
	border-radius: 5px;
	z-index: 13;
	min-height: 150px;
	min-width: 200px;
	h1 {
		text-align: center;
		padding-bottom: 0.5rem;
	}
	h1,
	p {
		flex: 1 1 100%;
		justify-self: space-between;
	}
	.shallWe {
		padding-top: 2rem;
		text-align: center;
	}

	span {
		float: right;
	}
	button {
		padding: 0rem 1rem;
		height: 2rem;
		color: #fff;
		background-color: var(--blue);
		border-radius: 50px;
		border: none;
		cursor: pointer;
		box-shadow: 2px 2px 5px rgb(50, 50, 50);
	}
	button:hover {
		box-shadow: 2px 2px 2px #333;
		filter: brightness(125%);
	}
	button:active {
		filter: brightness(100%);
		color: #fff;
		border-color: #fff;
		border: none;
		box-shadow: inset 2px 2px 5px rgb(50, 50, 50);
	}
}
/*
- unselectableHTML in global.class
- animation in global.css
- @media in global.css
*/
</style>
