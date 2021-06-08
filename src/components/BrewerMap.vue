<template>
    <div class="row map">
        <!-- <h2> Center is {{currentCenter}}, zoom is {{currentZoom}} </h2> -->
  
    <l-map 
      
      :zoom="currentZoom" 
      :center="currentCenter"
      @update:center="centerUpdated"
      @update:zoom="zoomUpdated"
    >
      <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer>
      <l-marker
        
        :key="index"
        v-for="(brewer, index) in brewers"
        :lat-lng="getCoord(brewer.lat,brewer.lng)"
        >
       <l-icon
        :icon-size="brewer.iconSize"
        :icon-url="icon"
        />
        <l-popup>
              Brasseur {{brewer.nameBrass}}
          </l-popup>
       
      </l-marker>

    <l-control-layers position="topright"></l-control-layers>
      <l-tile-layer
        v-for="tileProvider in tileProviders"
        :key="tileProvider.name"
        :url="tileProvider.url"
        :visible="tileProvider.visible"
        :name="tileProvider.name"
        layer-type="base"
      ></l-tile-layer>

    </l-map>
        <l-tile-layer :url="url" :attribution="attribution"></l-tile-layer> 
    </div>
</template>

<script>
// import { L } from 'vue2-leaflet'
import "leaflet/dist/leaflet.css";
import { LMap, LTileLayer, LMarker, LIcon, LPopup, LControlLayers }from "vue2-leaflet";
import beer from '../assets/img/icons/beer.svg'
import { latLng } from "leaflet";

const marker = LMarker
console.log(marker);

export default {
    name:"BrewerMap",
    props: {
        brewers: Array,
        

    },
   
    data: function () {
        return {

            

             

         tileProviders: [
        {
          name: "OpenStreetMap",
          url: "https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png",
          visible: true,
        },
        {
          name: "OpenTopoMap",
          url: "https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png",
          visible: false,
        },
        {
          name: "OpenForestMap",
          url: 'https://tile.thunderforest.com/transport/{z}/{x}/{y}.png?apikey=e955b6c9bc0b4ec6a38495e3f90d87b5',
          visible: false,
        },
       
        
      ],
            
                center: [46.7667, -1.51667],
                url: "https://{s}.tile.opentopomap.org/{z}/{x}/{y}.png",
                attribution: '&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
                marker:[46.7667, -1.51667],
                zoom: 8,
                currentCenter: [46.7667, -1.51667],
                currentZoom: 7,
                icon: beer,
                iconSize: [15, 15]
                
        }
        
      
    },
  components: {
        LMap,
        LTileLayer,
        LMarker,
        LIcon,
        LPopup,
        LControlLayers,
    },
  methods: {
    getCoord(a,b){
      return latLng(a,b)
    },

    // latLng: function(lat, lng) {
    //           return L.latLng(lat, lng);
              
    // },
    centerUpdated(center) {
      this.currentCenter = center;
      
    },
    zoomUpdated(zoom) {
      this.currentZoom = zoom;
      
    },

  },
    
}

</script>
<style scoped>
.row .map{
   height: 90vh;
   width: 600px;
}


</style>