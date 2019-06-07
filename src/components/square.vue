<template lang="html">
  <div 
    class="square noselect" 
    :class="{
      'empty': state === 0,
      'missed': state === 1,
      'part-destroyed': state === 2,
      'full-destroyed': state === 3,
      'your-ship': state === 4,
    }"
    @click="handleClick"
  >
    {{ state }}
  </div>
</template>

<script lang="js">
  /*
    States:
      0 - empty
      1 - missed
      2 - part destroyed
      3 - full destroyed
      4 - ship

    Modes:
      0 - building
      1 - attacking
      2 - friendly board
  */
  export default  {
    name: 'square',
    props: {
      state: {
        type: Number,
        default: 0
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

      }
    },
    methods: {
      handleClick: function() {
        if (this.mode === 0 && this.state === 0) {
          this.$emit('build');
        }
      }
    },
    computed: {

    },
}
</script>

<style scoped lang="scss">
	$borderColor: black;

	.noselect {
		-webkit-touch-callout: none; /* iOS Safari */
		-webkit-user-select: none; /* Safari */
		-khtml-user-select: none; /* Konqueror HTML */
		-moz-user-select: none; /* Firefox */
		-ms-user-select: none; /* Internet Explorer/Edge */
		user-select: none; /* Non-prefixed version, currently
																										                                  supported by Chrome and Opera */
	}

	.square {
		width: 30px;
		height: 30px;
		border: 2px solid $borderColor;

		text-align: center;
		line-height: 30px;

		cursor: pointer;
	}

	.empty {
		background-color: transparent;
	}

	.missed {
		background-color: rgb(112, 107, 107);
		cursor: not-allowed;
	}

	.part-destroyed {
		background-color: rgb(219, 82, 82);
		cursor: not-allowed;
	}

	.full-destroyed {
		background-color: rgb(240, 35, 35);
		cursor: not-allowed;
	}

	.your-ship {
		background-color: rgb(59, 240, 35);
		cursor: not-allowed;
	}
</style>
