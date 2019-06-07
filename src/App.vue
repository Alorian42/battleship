<template>
	<div id="app">
		<board ref="boardPlayer" :matrix="matrixPlayer" :mode="modePlayer" @change="handleChange"></board>
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
			this.matrixPlayer = this.generateEmptyMatrix();
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
			}
		}
	};
</script>

<style>
</style>
