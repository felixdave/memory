<script setup>
// Props
defineProps(['value', 'isFaceUp', 'index']);
// Emits
defineEmits(['selectedCardEmit']);
</script>

<template>
	<div class="gameCard" :data-index="index" @click="$emit('selectedCardEmit')">
		<!-- Used for flip effect -->
		<div class="cardInner unselectableHTML" :class="{ rotate: isFaceUp }">
			<span class="cardFront">{{ value }}</span>
			<span class="cardBack"></span>
		</div>
	</div>
</template>

<style lang="scss">
.gameCard {
	font-family: 'Saira Extra Condensed', sans-serif;
	font-weight: bold;
	border-radius: 3px;
	cursor: pointer;
	background-color: transparent;
	perspective: 1000px;
	aspect-ratio: 1/1.5;

	span {
		color: red;
		position: absolute;
		width: 100%;
		height: 100%;
		-webkit-backface-visibility: hidden;
		backface-visibility: hidden;
		display: flex;
		justify-content: center;
		align-items: center;
		border-radius: 5px;
		box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2);
		box-sizing: border-box;
	}

	.cardInner {
		position: relative;
		width: 100%;
		height: 100%;
		text-align: center;
		transition: transform 0.5s;
		transform-style: preserve-3d;
	}

	.cardBack {
		background-color: #357dc6;
		border: 3px solid white;
		width: 100%;
		color: black;
	}
	.cardFront {
		background-color: #fff;
		color: red;
		transform: rotateY(180deg);
	}
}

.rotate {
	transform: rotateY(180deg);
}

/* 
- .unselectableHTML in global.class
- animation in global.css
- @media in global.css  
*/
</style>
