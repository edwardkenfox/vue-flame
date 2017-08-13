<template>
  <div id="app">
    <Matrix
      :rows="rows"
      ref="matrix">
    </Matrix>
    <button
      @click="toggleFlame"
      v-text="toggleText">
    </button>
  </div>
</template>

<script>
import Matrix from './components/Matrix'

const MATRIX_SIZE = 40
const createRandomArray = (length, max) => [...new Array(length)].map(() => Math.round(Math.random() * max))

export default {
  name: 'app',
  data () {
    return {
      rows: [
        ...[...new Array(MATRIX_SIZE)].map(() => [...new Array(MATRIX_SIZE)].map(() => null))
      ],
      running: false,
      intervalId: null
    }
  },
  components: {
    Matrix
  },
  computed: {
    toggleText () {
      return this.running ? 'Stop' : 'Start'
    }
  },
  methods: {
    recaulcate () {
      this.$refs.matrix.recaulcate()
    },
    toggleFlame () {
      if (this.running) {
        clearInterval(this.intervalId)
      } else {
        const vm = this
        this.intervalId = setInterval(() => {
          vm.rows.splice(0, 1, createRandomArray(MATRIX_SIZE, 100))
          vm.$nextTick(() => {
            vm.recaulcate()
          })
        }, 50)
      }
      this.running = !this.running
    }
  }
}
</script>

<style>
:root {
  --cell-size: 8px;
}
</style>
