<template>
  <div
    class="cell"
    :class="[flameColor]">
  </div>
</template>

<script>
export default {
  name: 'cell',
  data () {
    return {
      calculatedValue: null
    }
  },
  props: {
    cellIndex: {
      type: Number, required: true
    },
    rows: {
      type: Array, required: true
    },
    rowIndex: {
      type: Number, required: true
    }
  },
  computed: {
    cellValue () {
      return this.rows[this.rowIndex][this.cellIndex]
    },
    flameColor () {
      return `cell-color-${Math.floor(this.calculatedValue / 10)}`
    }
  },
  methods: {
    recaulcate () {
      // Naively retreiving the cell value from the rowIndex and cellIndex and
      // modifying the value expecting everything to work reactively didn't behave
      // as I expected causing UI thread blocking procedure with the setInterval and
      // associated computed propery calls, so here I took a super dirty way to avoid the performance issue.
      if (this.rowIndex === 0) {
        this.calculatedValue = this.cellValue
      } else {
        const row = this.$root.$refs.app.$refs.matrix.$refs.rows[this.rowIndex - 1].$refs.cells
        let prev, current, next

        if (this.cellIndex === 0) {
          prev = row[row.length - 1]
          current = row[this.cellIndex]
          next = row[this.cellIndex + 1]
        } else if (this.cellIndex === row.length - 1) {
          prev = row[this.cellIndex - 1]
          current = row[this.cellIndex]
          next = row[0]
        } else {
          prev = row[this.cellIndex - 1]
          current = row[this.cellIndex]
          next = row[this.cellIndex + 1]
        }

        if (this.rowIndex === 1) {
          const value = [prev.calculatedValue, current.calculatedValue, next.calculatedValue].reduce((n, m) => {
            return n + m
          }) / 3

          this.calculatedValue = this.calculatedValue = Math.floor(value)
        } else {
          const prevRow = this.$root.$refs.app.$refs.matrix.$refs.rows[this.rowIndex - 2].$refs.cells
          let prevRowCell = prevRow[this.cellIndex]

          const value = [prev.calculatedValue, current.calculatedValue, next.calculatedValue, prevRowCell.calculatedValue].reduce((n, m) => {
            return n + m
          }) / 4

          this.calculatedValue = Math.floor(value)
        }
      }
    }
  },
  mounted () {
    this.recaulcate()
  }
}
</script>

<style scoped>
.cell {
  display: inline-block;
  height: var(--cell-size);
  width: var(--cell-size);
}
.cell-color-0 {
  background-color: rgb(0, 0, 0);
}
.cell-color-1 {
  background-color: rgb(45, 8, 9);
}
.cell-color-2 {
  background-color: rgb(93, 15, 11);
}
.cell-color-3 {
  background-color: rgb(196, 12, 6);
}
.cell-color-4 {
  background-color: rgb(255, 20, 20);
}
.cell-color-5 {
  background-color: rgb(248, 96, 66);
}
.cell-color-6 {
  background-color: rgb(255, 204, 60);
}
.cell-color-7 {
  background-color: rgb(255, 210, 108);
}
.cell-color-8 {
  background-color: rgb(255, 240, 173);
}
.cell-color-9 {
  background-color: rgb(255, 255, 255);
}
</style>
