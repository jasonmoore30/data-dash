<template>
  <v-container fluid>
    <v-slide-y-transition mode="out-in">
      <v-layout column align-center>
        <div>
          <span> Greenville County Data Hub </span>
          <br/>
    	    <span> Community Engaged Learning </span>
            <div>
              <mapbox
                access-token = "pk.eyJ1IjoibWFkZWxpbmUtYmF1ZXIiLCJhIjoiY2o5YThnaHZhMHp4cjMzbzc4YzdzbTAzeiJ9.FppQOJ6wceaF1EUtP9ud1A"
                :map-options="{
                  style: 'mapbox://styles/madeline-bauer/cjckx62ub26ky2rlop4peaylr',
                  center: [-82.300316, 34.857192],
                  zoom: 9
              }"
                @map-load="mapLoaded"
                @map-click="mapClicked"
              >
              </mapbox>
                  </div>
            </div>
      </v-layout>
    </v-slide-y-transition>
  </v-container>
</template>

<script>
import Mapbox from 'mapbox-gl-vue'
import mapboxgl from 'mapbox-gl'
export default {
     data() {
       return {}
     },
     components: {
         Mapbox
     },
 methods: {
    mapLoaded(map) {
      map.addSource({
        id: 'greenville-lib',
        type: 'symbol',
        source: {
          type: 'features',
          style: 'mapbox://styles/madeline-bauer/cjckx62ub26ky2rlop4peaylr'// Your Mapbox tileset Map ID
        },
        'source-layer': 'Bright', // name of tilesets

        'layout': {
          'icon-image': '{icon}-15',
          'text-field': '{title}',
          'text-font': ['Open Sans Semibold', 'Arial Unicode MS Bold'],
          'text-offset': [0, 0.6],
          'text-anchor': 'top'
        }
      });
    },
    mapClicked(map, e) {
      this.addPopUp(map, e);
    },
    mapMouseMoved(map, e) {
      const features = map.queryRenderedFeatures(e.point, {
        layers: ['points']
      });
      map.getCanvas().style.cursor = (features.length) ? 'pointer' : '';
    },
    addPopUp(map, e) {
      const features = map.queryRenderedFeatures(e.point, {
        layers: ['points']
      });
      if (!features.length) {
        return;
      }
 
      const feature = features[0];
 
      const popupContent = Vue.extend({
        template: '<button @click="popupClicked">Click Me!</button>',
        methods: {
          popupClicked() {
            alert('Popup Clicked!');
          },
        }
      });
 
      // Populate the popup and set its coordinates
      // based on the feature found.
      const popup = new mapboxgl.Popup()
        .setLngLat(feature.geometry.coordinates)
        .setHTML('<div id="vue-popup-content"></div>')
        .addTo(map);
 
      new popupContent().$mount('#vue-popup-content');
  	}
  }
 }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
#map {
  width: 100%;
  height: 500px;
}
</style>
