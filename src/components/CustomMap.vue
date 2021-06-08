<template>
<div class="container">
    <div class="row">
      <div class="col-12">
        <h1 class="text-center text-info">BRASSEURS DE FRANCE</h1>
      </div>
         <!-- <input class="input"
        type="text"
        placeholder="Entrez votre code departement"
        v-model="brewers"
        v-on:keypress="getBrewers"
      /> -->
    </div>
 <div class="row">
    <div class="col-6 brew-list">

      <div class="card">
        <ul class="list-group list-group-flush">
          <BrewerList
          v-on:mouse-over-brewer="mouseOverBrewer"  
          v-on:mouse-left-brewer="mouseLeftBrewer"  
          :brewers="brewers"/>
        </ul> 
      </div>
    </div>
      <div class="col-6">
         <BrewerMap :brewers="brewers"/>
      </div>
 </div>
  
</div>
</template>

<script>

import axios from "axios";
import BrewerList from "./BrewerList";
import BrewerMap from "./BrewerMap";

export default {
    name: 'Brewers',

  components: {
      BrewerList,
      BrewerMap
  
  },
  data() {
    return {
      brewers: [],
      normalIcon: [20,20],
      largeIcon: [40,40],
      url_brewers: "http://localhost:3000/brewers"
    }
  },
  mounted: function() {

      axios.get('http://localhost:3000/brewers')
           .then((r) => {
               this.brewers = r.data.filter(r => r.ville_departement == '85')
                   .map(r => {
                     r.iconSize = this.normalIcon;
                     return r;
                     
                   })
            
           })
  },

  methods: {
    mouseOverBrewer: function (id) {
      this.brewers[id].iconSize = this.largeIcon;
      console.log(id);
    },
    mouseLeftBrewer: function (id) {
      this.brewers[id].iconSize = this.normalIcon;
      console.log(id);
    },

    getBrewers(e) {
      if (e.key == 'enter') {
        axios
             .get(`${this.url_brewers}q=${this.brewers}`)
             .then((r) => {
               this.brewers = r.data.filter(r => r.ville_departement )
                   .map(r => {
                     r.iconSize = this.normalIcon;
                     return r;
                   })
                  this.requete = "" ;
             })
      }
    }
  },

};
</script>

<style lang="scss" scoped>

.brew-list{
    overflow-y: scroll;
    height: 95vh;
    li{
        &:hover{
            background: darkgrey;
        }
    }
}
@media screen and (max-width:620px){
  .row{
    display: grid;
    grid-auto-columns: 1fr;
    justify-content: center;
    margin: auto;
    overflow-x: hidden;
    
  }
  .brew-list{
    width: 800px;
    display: flex;
    justify-content: center;
    margin: auto;
    overflow: hidden;
  }
 
}


</style>