<template>
	<div id="app">
		<board ref="boardPlayer" :matrix="matrixPlayer" :mode="modePlayer" @change="handleChange"></board>
		<button @click="handleSwitch" class="switch-button">Switch mode</button>
	</div>
</template>

<script>
	import Board from "./components/board.vue";

	export default {
		name: "app",
		components: {
			Board
		},
		data: function() {
			return {
				matrixPlayer: [],
				modePlayer: 0
			};
		},
		mounted: function() {
			const loaded = window.localStorage.getItem("matrix");
			if (loaded) {
				this.matrixPlayer = JSON.parse(loaded);
			} else {
				this.matrixPlayer = this.generateEmptyMatrix();
			}
		},
		methods: {
			generateEmptyMatrix: function() {
				return [
					[0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
					[0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
					[0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
					[0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
					[0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
					[0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
					[0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
					[0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
					[0, 0, 0, 0, 0, 0, 0, 0, 0, 0],
					[0, 0, 0, 0, 0, 0, 0, 0, 0, 0]
				];
			},
			handleChange: function(params) {
				this.matrixPlayer[params.rowNumber][params.index] = params.value;
				this.$refs.boardPlayer.$forceUpdate();
				this.saveState();
			},
			saveState: function() {
				window.localStorage.setItem(
					"matrix",
					JSON.stringify(this.matrixPlayer)
				);
			},
			handleSwitch: function() {
				if (this.modePlayer === 0) {
					this.modePlayer = 1;
				} else {
					this.modePlayer = 0;
				}
			}
		}
	};
</script>

<style>
	.switch-button {
		position: absolute;
		left: 350px;
	}
</style>
