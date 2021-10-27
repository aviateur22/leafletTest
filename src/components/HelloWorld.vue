<template>
<div class="container">
  <button @click="add">Ajouter marker</button>
  <div class="user-info">
    <h2>info utilisateur cliquées</h2>
      {{ this.user}}
  </div>
  <div class="map" style="height: 80vh">
    <LMap :zoom="zoom" :center="center" @click="createMarker">
      <LTileLayer :url="url"  :attribution="attribution"></LTileLayer>  
      <LPolygon :lat-lngs="this.auterive.latLng" :color="this.auterive.color"></LPolygon>
      <LMarker @click="infoMarker(marker)" @dragend="updateMarkerPosition($event,marker)" :draggable="true" :lat-lng=marker.coordinate v-for="(marker,id) in markers" :key="id" >
        
      </LMarker>
    </LMap>
  </div>
</div>

</template>

<script>

import L from 'leaflet';
import { LMap, LTileLayer, LMarker,LPolygon } from 'vue2-leaflet';

export default {
  name: 'HelloWorld',
  components: {
    LMap,
    LTileLayer,
    LMarker,
    LPolygon
  },
  data() {
    return {
      url: "https://{s}.tile.osm.org/{z}/{x}/{y}.png",
      attribution:'&copy; <a href="http://osm.org/copyright">OpenStreetMap</a> contributors',
      zoom: 6,
      infoMarkerVisibility:false,
      auterive:{
        latLng:[[43.356636,1.434777],[43.358326,1.43801228],[43.356636,1.434777],]
        ,
        color:'red'

      },
      user:null,
      center: [47.013220, -1.219482],
      bounds: null,      
      myMarker:{id:"Cyrille", coordinate: L.latLng(46.013220, -1.219482)},
      enalyMarker:{id:"Enaly", coordinate: L.latLng(46.013220, -1.219482)},
      markers:[        
      {id:"Celine", coordinate: L.latLng(47.013220, -1.219482)},
      {id:"boys",coordinate:L.latLng(47.413220, -1.219482)},
      {id:"Helixia", coordinate:L.latLng(47.913220, -1.219482)}
      ]
    };
  },
  props: {
    msg: String    
  },
  methods:{

    // eslint-disable-next-line no-unused-vars
    updateMarkerPosition(event, markerDragged){
      //const newCoordinate = event.target._latlng;
      console.log(markerDragged.id)
      const markerUpdated = this.markers.filter((marker)=>{
        return marker.id === markerDragged.id
      });
      console.log(markerUpdated[0].id);
       markerUpdated[0].coordinate.lat=event.target._latlng.lat;
       markerUpdated[0].coordinate.lng=event.target._latlng.lng;
      console.log(event.target._latlng); //.latLng + '' + 'Coordinate initiale :' +  marker.coordinate

    },

    createMarker(event){
     this.markers.push(this.enalyMarker),
      console.log(event.latlng)
    },

    add(){

     //this.markers.push(this.myGeoPosition);
     this.myMarkerIndex = this.markers.push(this.myMarker)-1;

    },
    infoMarker(marker){

      console.log(marker.id)
      this.user = marker.id;

      this.infoMarkerVisibility = !this.infoMarkerVisibility;


    },

  /**
   * 
   */
    updatePosition(){

      setTimeout(() => this.updatePosition(), 3000);

      if(this.myMarkerIndex>-1){       
        this.markers.splice(this.myMarkerIndex,1,{id:"Cyrille", coordinate: L.latLng(this.myMarker.coordinate.lat +=0.01, -1.219482)});
        console.log('ma position : '+ this.myMarker.coordinate + '\n' + this.markers[this.markers.length-1]);   

      }
        

      

    }
  },
  mounted(){
    window.addEventListener('keydown',this.updatePosition);
    this.updatePosition();
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.container{
  display: flex;

}

.map{
  height: 80vh;
  width: 100%;
}

h3 {
  margin: 40px 0 0;
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
</style>
