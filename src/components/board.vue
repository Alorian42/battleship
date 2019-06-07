<template lang="html">
  <div class="board">
    <div class="row" v-for="(row, rowNumber) in matrix">
      <square 
        v-for="(square, index) in row" 
        @build="handleBuild(rowNumber, index)"
        :state="square"
        :mode="mode"
      ></square>
    </div>
    <div class="ships">
      <div class="ships-item" v-for="(ship, cost) in freeShips">Свободно {{ cost }}-палубных кораблей: {{ ship }}</div>
    </div>
  </div>
</template>

<script lang="js">
  import Square from "./square.vue";

  export default  {
    name: 'board',
    components: {
			Square
		},
    props: {
      matrix: {
        type: Array,
        default: function() {
          return new Array();
        }
      },
      mode: {
        type: Number,
        default: 0
      }
    },
    mounted() {

    },
    data() {
      return {
        freeShips: {
          1: 4,
          2: 3,
          3: 2,
          4: 1
        }
      }
    },
    methods: {
      handleBuild: function(rowNumber, index) {
        const action = {
          rowNumber,
          index,
          value: 4
        };

        const validation = this.validBuild(action);
        if (validation) {
          this.$emit('change', action);
        }
      },
      validBuild: function({rowNumber, index}) {
        const envState = {
          right: true,
          left: true,
          top: true,
          bottom: true,
          rightTop: true,
          leftTop: true,
          rightBottom: true,
          leftBottom: true,
        };

        // cases on first row
        if (rowNumber === 0) {
          // case on first index
          if (index === 0) {
            envState.right = this.matrix[rowNumber][index + 1] === 0;
            envState.rightBottom = this.matrix[rowNumber + 1][index + 1] === 0;
            envState.bottom = this.matrix[rowNumber + 1][index] === 0;
          }
          // case on last index
          else if (index === 9) {
            envState.left = this.matrix[rowNumber][index - 1] === 0;
            envState.leftBottom = this.matrix[rowNumber + 1][index - 1] === 0;
            envState.bottom = this.matrix[rowNumber + 1][index] === 0;
          }
          // general index && first row
          else { 
            envState.right = this.matrix[rowNumber][index + 1] === 0;
            envState.left = this.matrix[rowNumber][index - 1] === 0;
            envState.rightBottom = this.matrix[rowNumber + 1][index + 1] === 0;
            envState.leftBottom = this.matrix[rowNumber + 1][index - 1] === 0;
            envState.bottom = this.matrix[rowNumber + 1][index] === 0;
          }
        }
        // cases on last row
        else if (rowNumber === 9) {
          // case on first index
          if (index === 0) {
            envState.right = this.matrix[rowNumber][index + 1] === 0;
            envState.rightTop = this.matrix[rowNumber - 1][index + 1] === 0;
            envState.top = this.matrix[rowNumber - 1][index] === 0;
          }
          // case on last index
          else if (index === 9) {
            envState.left = this.matrix[rowNumber][index - 1] === 0 ;
            envState.leftTop = this.matrix[rowNumber - 1][index - 1] === 0;
            envState.top = this.matrix[rowNumber - 1][index] === 0;
          }
          // general index && last row
          else
          {
            envState.right = this.matrix[rowNumber][index + 1] === 0;
            envState.left = this.matrix[rowNumber][index - 1] === 0;
            envState.rightTop = this.matrix[rowNumber - 1][index + 1] === 0;
            envState.leftTop = this.matrix[rowNumber - 1][index - 1] === 0;
            envState.top = this.matrix[rowNumber - 1][index] === 0;
          }
        }
        // case on first index && general row
        else if (index === 0) {
            envState.right = this.matrix[rowNumber][index + 1] === 0;
            envState.top = this.matrix[rowNumber - 1][index] === 0;
            envState.rightTop = this.matrix[rowNumber - 1][index + 1] === 0;
            envState.rightBottom = this.matrix[rowNumber + 1][index + 1] === 0;
            envState.bottom = this.matrix[rowNumber + 1][index] === 0;
        }
        // case on last index && general row
        else if (index === 9) {
            envState.left = this.matrix[rowNumber][index - 1] === 0;
            envState.leftTop = this.matrix[rowNumber - 1][index - 1] === 0;
            envState.top = this.matrix[rowNumber - 1][index] === 0;
            envState.leftBottom = this.matrix[rowNumber + 1][index - 1] === 0;
            envState.bottom = this.matrix[rowNumber + 1][index] === 0;
        }
        // general case
        else {
          envState.left = this.matrix[rowNumber][index - 1] === 0;
          envState.right = this.matrix[rowNumber][index + 1] === 0;
          envState.rightTop = this.matrix[rowNumber - 1][index + 1] === 0;
          envState.leftTop = this.matrix[rowNumber - 1][index - 1] === 0;
          envState.top = this.matrix[rowNumber - 1][index] === 0;
          envState.rightBottom = this.matrix[rowNumber + 1][index + 1] === 0;
          envState.leftBottom = this.matrix[rowNumber + 1][index - 1] === 0;
          envState.bottom = this.matrix[rowNumber + 1][index] === 0;
        } 

        const corners = envState.rightTop && envState.leftTop && envState.rightBottom && envState.leftBottom;

        return  envState.right && envState.left && envState.top && envState.bottom && 
          corners;
      }
    },
    computed: {
    },
}
</script>

<style scoped lang="scss">
	$borderColor: black;
	.board {
		display: flex;
		border: 2px solid $borderColor;

		height: 340px;
		width: 340px;
		flex-wrap: wrap;
	}

	.row {
		display: flex;

		width: 340px;
		height: 34px;

		flex-wrap: nowrap;
	}
</style>
