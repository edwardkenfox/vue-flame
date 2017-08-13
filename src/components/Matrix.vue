<template>
  <div class="matrix">
    <Row
      class="row"
      v-for="(row, index) in rows"
      :rows="rows"
      :row-index="index"
      :key="index"
      ref="rows">
    </Row>
  </div>
</template>

<script>
import Row from './Row'

export default {
  name: 'matrix',
  components: {
    Row
  },
  props: {
    rows: {
      type: Array, required: true
    }
  },
  methods: {
    recaulcate () {
      this.$refs.rows.map(row => row.recaulcate())
    }
  }
}
</script>

<style scoped>
.matrix {
  transform: scaleY(-1);
}
</style>
