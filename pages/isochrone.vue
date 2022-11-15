<template>
    <div>
        <div id="map">
            <div class="pa-2" style="position:absolute;top:0;z-index:999;width:100%;">
                <div class="pa-2 px-3" style="font-size:0.8em;background: rgba(0, 0, 0, 0.6);border-radius:5px;">
                    <div>Driving Time: 
                        <b>{{slider}} minutes</b></div>
                    <div>
                        <v-slider
                            v-model="slider"
                            max="60"
                            min="5"
                            color="cyan"
                            hide-details
                            @change="setIsochrone(slider)"
                        ></v-slider>
                    </div>
                </div>
            </div>

            <v-expand-transition>
                <div v-if="ptsWithin.features.length!==0" class="pa-5" style="padding-bottom:50px !important; background-color:#000000;position:absolute;bottom:0;z-index:999;width:100%;border-radius:20px 20px 0 0;">
                    <div style="max-height:300px !important; overflow-y: auto; ">
                    <div class="d-flex align-center justify-space-between mb-5" v-for="item,index in ptsWithin.features" :key="index">
                       
                        <div>
                            <b>{{item.properties.name}}</b><br/>
                            <div class="d-flex align-center">
                                <div class="d-flex align-center  mr-2">
                                    <v-icon small class="yellow--text mr-1">mdi-star</v-icon> <span style="font-size:0.8em">{{item.properties.point}}</span> 
                                </div>
                                <v-chip x-small class="cyan"><b>{{(findDistance(center,item.geometry.coordinates)/1000).toFixed(2)}} km</b></v-chip>
                            </div>
                        </div>
                            
                        <div><v-btn small text outlined><v-icon small class="mr-2">mdi-eye</v-icon>Details</v-btn></div>
                        
                        </div>
                    </div>
                </div>
        </v-expand-transition>

        </div>
    </div>
</template>

<script>
import axios from 'axios'
import * as turf from '@turf/turf'

export default {
    data(){
        return{
           slider:12,
           pointData:[],
           isochroneData:[],
           ptsWithin:{
            features:[]
           },
           center:[106.842775,-6.190301]
        }
    },
    mounted(){
      this.createMap()
    },
    methods:{
        createMap(){

            const mapboxgl = require('mapbox-gl/dist/mapbox-gl.js')
            mapboxgl.accessToken = process.env.VUE_APP_MAPBOX_TOKEN

            this.map = new mapboxgl.Map({
                container: 'map',
                style: 'mapbox://styles/mapbox/dark-v10',
                zoom: 11,
                center: [106.842775,-6.190301],
                attributionControl: false,
                preserveDrawingBuffer: true 
            })

            axios({
                url:'/ev/sample/sample-point-jkt.json'
            }).then(res=>{
                this.pointData = res.data
            })
            

            this.map.on('load',()=>{
                
             //adding image
            this.map.loadImage('/ev/img/marker2.png ', (
                    error,
                    image
                ) => {
                    if (error) throw error
                    this.map.addImage('marker', image)
                })



               this.setIsochrone(12)
            })
            
        },
        findDistance(coord1,coord2){
            return  turf.distance(
              coord1,
              coord2,
              { units: 'meters' }
            )
        },
        setIsochrone(time){
            if(this.map.getLayer('isochrone')){
                this.map.removeLayer('isochrone')
                this.map.removeSource('isochrone')
            } 
            if(this.map.getLayer('start_point')){
                this.map.removeLayer('start_point')
                this.map.removeSource('start_point')
            } 
            if(this.map.getLayer('dummypoint')){
                this.map.removeLayer('dummypoint')
                this.map.removeSource('dummypoint')
            } 
            var query =
                'https://api.mapbox.com/isochrone/v1/mapbox/driving' +
                '/' +
                106.842775 +
                ',' +
                -6.190301 +
                '?contours_minutes=' +
                time +
                '&polygons=true&access_token=' +
                process.env.VUE_APP_MAPBOX_TOKEN

                axios({
                    url: query
                }).then(res=>{

                    this.isochroneData = res.data
                    
                    this.map.addSource('isochrone', {
                            type: 'geojson',
                                data: res.data

                        })
                        this.map.addLayer({
                            id: 'isochrone',
                            type: 'fill',
                            source: 'isochrone',
                            paint: {
                                'fill-color': "#00c7b3",
                                'fill-opacity': 0.3,
                                'fill-outline-color':'#005494'
                            }
                        })

                        

                this.map.addLayer({
                        id: 'start_point',
                        type: 'symbol',
                        source: {
                            type: 'geojson',
                            data: {
                            type: 'Feature',
                            properties: {},
                            geometry: {
                                type: 'Point',
                                coordinates:  [106.842775,-6.190301]
                            }
                            }
                        },layout: {
                                'icon-image': 'marker',
                                'icon-size': 0.3,
                                "icon-allow-overlap" : true,
                                "text-allow-overlap": true
                            }
                    })

                    this.ptsWithin = turf.pointsWithinPolygon(
                        this.pointData,
                        turf.polygon(this.isochroneData.features[0].geometry.coordinates)
                    )

                    this.ptsWithin.features.forEach((item,index)=>{
                        item.properties.distance = this.findDistance(this.center,item.geometry.coordinates)
                    })

                    this.ptsWithin.features.sort((a,b) => (a.properties.distance > b.properties.distance) ? 1 : ((b.properties.distance > a.properties.distance) ? -1 : 0))

                    this.$forceUpdate()

                    this.map.addLayer({
                        'id': 'dummypoint',
                        'type': 'symbol',
                        'source': {
                            type: 'geojson',
                            data: this.ptsWithin
                        },
                        paint:{
                            'text-color':'#ffffff',
                        },
                        layout: {
                            'icon-image': 'marker',
                            'icon-size': 0.3,
                            "icon-allow-overlap" : true,
                            "text-allow-overlap": true
                        }
                    }); 

                            
                }).catch(err=>{
                    console.log(err)
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