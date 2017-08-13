<template>
  <div>
    <Cell
      v-for="(cell, index) in row"
      :cell-index="index"
      :rows="rows"
      :row-index="rowIndex"
      :key="index"
      ref="cells">
    </Cell>
  </div>
</template>

<script>
import Cell from './Cell'

export default {
  name: 'row',
  components: {
    Cell
  },
  props: {
    rows: {
      type: Array, required: true
    },
    rowIndex: {
      type: Number, required: true
    }
  },
  computed: {
    row () {
      return this.rows[this.rowIndex]
    }
  },
  methods: {
    recaulcate () {
      this.$refs.cells.map(cell => cell.recaulcate())
    }
  }
}
</script>

<style scoped>
.row {
  height: var(--cell-size);
}
</style>
