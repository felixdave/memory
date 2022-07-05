<script setup>
import { ref } from 'vue';
import PlayingCard from '../ui/PlayingCard';
// Props
defineProps(['start', 'gameOver']);
// Emits
const emit = defineEmits([
	'updateTotalMatches',
	'updatePlayerMoves',
	'startOver',
]);
// Vars
const cardPairs = ref(12);
const allCards = ref([]);
const hand = ref([]);
const displayMatch = ref(false);
// Funcs
function initGame() {
	const tempArr = [];

	// Generate a doubled ordered array
	for (let i = 0; i < cardPairs.value; i++) {
		tempArr.push(i + 1, i + 1);
	}
	// Shuffle array - (Fisher-Yates Shuffle)
	// Loop from end, pick up card, get random card, and swap
	for (let i = tempArr.length - 1; i > 0; i--) {
		const j = Math.floor(Math.random() * (i + 1));
		// Assign random to current index and visa versa
		[tempArr[i], tempArr[j]] = [tempArr[j], tempArr[i]];
	}
	return (allCards.value = tempArr);
}
// Func Match
function pairMatch() {
	let el1 = document.querySelector(`[data-index='${hand.value[0]}']`);
	let el2 = document.querySelector(`[data-index='${hand.value[1]}']`);
	el1.classList.add('animateCardFade');
	el2.classList.add('animateCardFade');
	displayMatch.value = true;
	emit('updateTotalMatches');
	//  this removes the Match element
	setTimeout(() => {
		displayMatch.value = false;
		hand.value = [];
	}, 1500);
}
// Func No Match
function pairNoMatch() {
	setTimeout(() => {
		hand.value = [];
	}, 700);
}
// Func Selected Card
function addCardToHand(index) {
	// Adds first card index
	if (hand.value.length == 0) {
		emit('updatePlayerMoves');

		hand.value.push(index);
	} // Adds second card index
	else if (hand.value.length < 2 && hand.value[0] != index) {
		emit('updatePlayerMoves');
		hand.value.push(index);
		allCards.value[hand.value[0]] == allCards.value[hand.value[1]]
			? pairMatch()
			: pairNoMatch();
	}
}
// Func Face up/down against selection
function checkCards(index) {
	if (index == hand.value[0] || index == hand.value[1]) {
		return true;
	} else {
		return false;
	}
}
// Invocations
initGame();
</script>

<template>
	<div id="gameStage" v-if="!start">
		<TransitionGroup name="animateStart" appear>
			<!-- Animation in CSS & Here for effect using index-->
			<PlayingCard
				v-for="(card, index) in allCards"
				:style="{ transitionDelay: `${index * 0.1}s` }"
				:key="index"
				:value="card"
				:isFaceUp="checkCards(index)"
				:index="index"
				@selectedCardEmit="addCardToHand(index)"
			/>
		</TransitionGroup>
	</div>
	<Transition name="animateMatch" :duration="500">
		<div v-if="displayMatch" class="matchDisplay">
			<h1>MATCH!</h1>
			<PlayingCard class="card1" :value="allCards[hand[0]]" :isFaceUp="true" />
			<PlayingCard class="card2" :value="allCards[hand[1]]" :isFaceUp="true" />
		</div>
	</Transition>
	<Transition name="animateMatch" :duration="500">
		<div
			v-if="gameOver"
			@click="$emit('startOver')"
			class="unselectableHTML matchDisplay"
		>
			<h1>Game Over</h1>
			<PlayingCard class="card1" :value="'U'" :isFaceUp="true" />
			<PlayingCard class="card2" :value="1" :isFaceUp="true" />
		</div>
	</Transition>
</template>

<style lang="scss">
#gameStage {
	width: 100vmin;
	max-width: 800px;
	border-radius: 5px;
	background-color: rgba(255, 255, 255, 0.5);
	box-shadow: 2px 2px 50px rgb(50, 50, 50);
	box-sizing: border-box;
	font-size: 10vmin;
	padding: 2rem;
	position: relative;
	display: grid;
	transition: all 0.5s;
	grid-template-columns: repeat(6, 1fr);
	grid-template-rows: repeat(4, 1fr);
	align-content: flex-start;
	gap: 1rem;
	z-index: 5;
}
.matchDisplay {
	display: flex;
	flex-wrap: wrap-reverse;
	flex-basis: 100%;
	justify-content: center;
	align-items: flex-start;
	background: rgba(0, 0, 0, 0.5);
	font-size: 15vmin;
	position: fixed;
	height: 100vh;
	width: 150vw;
	z-index: 20;
	gap: 1rem;
	overflow: hidden;
	h1 {
		flex: 1 0 100%;
		text-align: center;
		background-color: var(--yellow);
		transform: rotate(-8deg);
		width: 2000px;
		align-self: flex-end;
		z-index: 12;
	}
	.card1,
	.card2 {
		height: 50vmin;
		box-shadow: 2px 2px 5px #333;
		z-index: 13;
	}
	.card1 {
		transform: rotate(-9deg);
	}
	.card2 {
		transform: rotate(9deg);
	}
}

// @media in global.css
// animation in global.css
</style>
