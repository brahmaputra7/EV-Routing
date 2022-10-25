<template>
    <div>
        <div id="map">
            <div class="pa-2" style="position:absolute;top:0;z-index:999;width:100%;">
                <div class="d-flex justify-space-between pa-2 px-3" style="font-size:0.8em;background: rgba(0, 0, 0, 0.6);border-radius:5px;">
                    <div>Ioniq 5 - Standard</div>
                    <div>
                        <v-btn x-small outlined>CHANGE</v-btn>
                        <v-btn x-small outlined><v-icon x-small>mdi-cog</v-icon></v-btn>
                    </div>
                </div>
            </div>
            
            <div class="pa-5" style="padding-bottom:50px !important; background-color:#000000;position:absolute;bottom:0;z-index:999;width:100%;border-radius:20px 20px 0 0;">
                <v-text-field solo-inverted dense></v-text-field>
                <v-text-field solo-inverted dense></v-text-field>
                <div class="d-flex justify-end">
                    <v-icon small>mdi-map-marker</v-icon><u>Add Waypoints</u>
                </div>
                <v-btn class="cyan mt-5" block>FIND BEST ROUTE</v-btn>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return{
            accessToken: '',
            map:{}
        }
    },
    mounted(){
      this.createMap()
    },
    methods:{
        createMap(){
            const mapboxgl = require('mapbox-gl/dist/mapbox-gl.js')

            mapboxgl.accessToken = 'pk.eyJ1Ijoic3BhdGlhIiwiYSI6ImNreGZzODVjaTA0c2Eyb29hcnphMWk1cjcifQ.MLFsmOmwkn2UG2Lr_N_bZg'

            this.map = new mapboxgl.Map({
                container: 'map',
                style: 'mapbox://styles/mapbox/dark-v10',
                zoom: 11,
                center: [106.816666, -6.200000],
                attributionControl: false,
                preserveDrawingBuffer: true 
            })

        }
    }
}
</script>

<style lang="scss">
#map {
    width:100%;
    height:700px;
}
@media only screen and (max-width: 600px) {
  #map {
    min-height:100vh;
  }
}
</style>