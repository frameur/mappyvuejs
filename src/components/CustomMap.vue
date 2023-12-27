<template>
  <div class="container">
    <div class="row">
      <div class="col-12">
        <h1 class="text-center text-info">BRASSEURS DE FRANCE</h1>
      </div>
      <div class="input-group">
        <input
          type="text"
          class="form-control"
          v-model="departmentCode"
          @keypress="getBrewers"
        />
        <div class="input-group-append">
          <button
            class="btn btn-outline-secondary"
            type="button"
            @click="getBrewers"
          >
            Rechercher
          </button>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-6 brew-list">
        <div class="card">
          <ul class="list-group list-group-flush">
            <BrewerList
              v-on:mouse-over-brewer="mouseOverBrewer"
              v-on:mouse-left-brewer="mouseLeftBrewer"
              :brewers="brewers"
            />
          </ul>
        </div>
      </div>
      <div class="col-6">
        <BrewerMap :brewers="brewers" />
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import BrewerList from './BrewerList'
import BrewerMap from './BrewerMap'

export default {
  name: 'Brewers',

  components: {
    BrewerList,
    BrewerMap,
  },
  data() {
    return {
      brewers: [],
      departmentCode: '',
      normalIcon: [20, 20],
      largeIcon: [40, 40],
      url_brewers: 'http://localhost:3000/brewers',
    }
  },
  mounted: function() {
    axios.get('http://localhost:3000/brewers').then((r) => {
      this.brewers = r.data
        .filter((r) => r.ville_departement == '85')
        .map((r) => {
          r.iconSize = this.normalIcon
          return r
        })
    })
  },

  methods: {
    mouseOverBrewer: function(id) {
      this.brewers[id].iconSize = this.largeIcon
      console.log(id)
    },
    mouseLeftBrewer: function(id) {
      this.brewers[id].iconSize = this.normalIcon
      console.log(id)
    },

    getBrewers(e) {
      if (e.key == 'enter' || e.type == 'click') {
        axios.get(`${this.url_brewers}q=${this.departmentCode}`).then((r) => {
          this.brewers = r.data
            .filter((r) => r.ville_departement == this.departmentCode)
            .map((r) => {
              r.iconSize = this.normalIcon
              return r
            })
          this.departmentCode = ''
        })
      }
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
    }
  }
}
@media screen and (max-width: 620px) {
  .row {
    display: grid;
    grid-auto-columns: 1fr;
    justify-content: center;
    margin: auto;
    overflow-x: hidden;
  }
  .brew-list {
    width: 800px;
    display: flex;
    justify-content: center;
    margin: auto;
    overflow: hidden;
  }
}
</style>
