<template>
  <div class="card brewer-list">
    <span class="brewer-count"
      ><strong> {{ remaining }} </strong> brasseurs
    </span>
    <ul class="list-group list-group-flush">
      <li
        @mouseover="mouseOver(index)"
        @mouseleave="mouseLeave(index)"
        :key="index"
        v-for="(brewer, index) in brewers"
        class="list-group-item"
      >
        Brasseur {{ brewer.nameBrass }}, {{ brewer.nameTown }},
        {{ brewer.ville_departement }}
      </li>
    </ul>
  </div>
</template>
<script>
export default {
  name: 'BrewerList',
  props: {
    brewers: Array,
  },
  methods: {
    mouseOver: function(index) {
      this.$emit('mouse-over-brewer', index)
    },
    mouseLeave: function(index) {
      this.$emit('mouse-left-brewer', index)
    },
  },
  computed: {
    remaining() {
      return this.brewers.filter((brewer) => !brewer.completed).length
    },
  },
}
</script>
<style lang="scss" scoped>
.brew-list {
  overflow-y: scroll;
  height: 95vh;
  li {
    &:hover {
      background: darkgrey;
      color: white;
      cursor: pointer;
    }
  }
}
</style>
