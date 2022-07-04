<script setup>
// Props
defineProps(['playerMoves', 'totalMatches', 'playTimer', 'start']);
// Emits
const emit = defineEmits(['resetStage', 'beginThis']);
</script>

<template>
	<div v-if="!start" id="scoreCard" class="unselectableHTML">
		<h1>MEMORY Score</h1>
		<p>
			Player Moves <span> {{ playerMoves }}</span>
		</p>
		<p>
			Total Matches <span>{{ totalMatches }}/12</span>
		</p>
		<p>
			PlayTime <span>{{ playTimer }}</span>
		</p>
		<button @click="$emit('resetStage')">RESET</button>
	</div>
	<div v-if="start" id="scoreCard">
		<h1>Accessing Memory...</h1>
		<p>Shall we play a game?</p>
		<button @click="$emit('beginThis')">Okay</button>
	</div>
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
	span {
		float: right;
	}
	button {
		padding: 0rem 1rem;
		height: 2rem;
		color: #fff;
		background-color: var(--blue);
		border-radius: 50px;
		border: 2px solid #fff;
		cursor: pointer;
	}
	button:hover {
		box-shadow: 2px 2px 2px #333;
		filter: brightness(125%);
	}
	button:active {
		filter: brightness(100%);
		color: var(--yellow);
		border-color: #fff;
		box-shadow: -2px -2px 2px #333;
	}
}
</style>
