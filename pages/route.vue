<template>
    <div>
        <div id="map">
            <div class="pa-2" style="position:absolute;top:0;z-index:999;width:100%;">
                <div class="d-flex justify-space-between pa-2 px-3" style="font-size:0.8em;background: rgba(0, 0, 0, 0.6);border-radius:5px;">
                    <div>Ioniq 5 - Standard</div>
                    <div>
                        <v-btn x-small outlined @click="$router.push('/current-profile')">CHANGE</v-btn>
                        <v-btn x-small outlined @click="$router.push('/current-profile')"><v-icon x-small>mdi-cog</v-icon></v-btn>
                    </div>
                </div>
            </div>

            <div class="evLocation">
                
            </div>
            
            <v-expand-transition  v-show="routeStatus">
                <div class="pa-5" style="padding-bottom:50px !important; background-color:#000000;position:absolute;bottom:0;z-index:999;width:100%;border-radius:20px 20px 0 0;">
                    <v-autocomplete solo-inverted dense placeholder="Select Origin" :items="originItems" v-model="origin"></v-autocomplete>
                    <v-autocomplete solo-inverted dense placeholder="Select Destination" :items="destinationItems" v-model="destination"></v-autocomplete>
                    <div class="d-flex justify-end">
                        <v-icon small>mdi-map-marker</v-icon><u>Add Waypoints</u>
                    </div>
                    <v-btn :loading="routeLoader" class="cyan mt-5" block :disabled="origin==''||destination==''" @click="setRoute()">FIND BEST ROUTE</v-btn>
                </div>
             </v-expand-transition>

            <v-expand-transition v-if="routeStatus">
                <div  class="pa-5" style="padding-bottom:50px !important; background-color:#000000;position:absolute;bottom:0;z-index:999;width:100%;border-radius:20px 20px 0 0;">
                    <v-timeline
                        align-top
                        dense
                    >
                        <v-timeline-item
                            color="cyan"
                            small
                        >
                            <div style="font-size:0.7em">SPKLU Tengah Tani, Cirebon, Jawa Barat</div>
                            <div  class="teal pa-2 py-1 mb-1" style="font-size:0.7em; width:fit-content;border-radius: 10px;"><b>215 km</b> </div>
                            <div  style="font-size:0.7em"><v-icon small>mdi-battery-charging-50</v-icon> Estimated Battery on Arrival: 36%</div>
                        </v-timeline-item>

                    
                        <v-timeline-item
                            color="cyan"
                            small
                        >
                            <div style="font-size:0.7em">SPKLU Jl. Tol Cikopo - Palimanan</div>
                            <div  class="teal pa-2 py-1 mb-1" style="font-size:0.7em; width:fit-content;border-radius: 10px;"><b>236 km</b> </div>
                            <div  style="font-size:0.7em"><v-icon small>mdi-battery-charging-50</v-icon> Estimated Battery on Arrival: 38%</div>
                        </v-timeline-item>

                        <v-timeline-item
                            color="green"
                            small
                        >
                            <div style="font-size:0.7em">Alun-Alun Kidul St, Patehan, Kraton, Yogyakarta City, Special Region of Yogyakarta</div>
                            <div  class="teal pa-2 py-1 mb-1" style="font-size:0.7em; width:fit-content;border-radius: 10px;"><b>138 km</b> </div>
                            <div  style="font-size:0.7em"><v-icon small>mdi-battery-charging-50</v-icon> Estimated Battery on Arrival: 65%</div>
                        </v-timeline-item>
                    </v-timeline>
                    <div class="d-flex align-center justify-center">
                        <v-btn dense class="grey mr-2" @click="routeStatus = false"><v-icon>mdi-chevron-left</v-icon></v-btn>
                        <v-btn dense class="cyan">START NAVIGATION</v-btn>
                    </div>
                </div>
            </v-expand-transition>


        </div>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    data(){
        return{
            accessToken: '',
            routeLoader:false,
            map:{},
            originItems:["Thamrin City, Jl. Kebon Melati 1, Kebon Melati, Tanah Abang, Central Jakarta City, Jakarta 10230"],
            destinationItems:["Alun-Alun Kidul St, Patehan, Kraton, Yogyakarta City, Special Region of Yogyakarta"],
            origin:'',
            destination:'',
            routeStatus:false,
            departureCoordinate:{
                lng:106.8158946,
                lat:-6.1945217
            },
            arrivalCoordinate:[
                {
                    lng: 108.522258,
                    lat:-6.753362
                },
                {
                    lng:110.431578,
                    lat:-7.032622
                },
                {
                    lng:110.363263,
                    lat:-7.812466
                }
            ]
               
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
                zoom: 8,
                center: [107.182875,-6.591327],
                attributionControl: false,
                preserveDrawingBuffer: true 
            })

             //adding image
             this.map.loadImage('/img/startpoint.png ', (
                    error,
                    image
                ) => {
                    if (error) throw error
                    this.map.addImage('startpoint', image)
                })

             //adding image
             this.map.loadImage('/img/startpoint-2.png ', (
                    error,
                    image
                ) => {
                    if (error) throw error
                    this.map.addImage('startpoint2', image)
                })
             //adding image
            this.map.loadImage('/img/warehouse.png ', (
                    error,
                    image
                ) => {
                    if (error) throw error
                    this.map.addImage('marker', image)
                })

             //adding image
            this.map.loadImage('/img/marker2.png ', (
                    error,
                    image
                ) => {
                    if (error) throw error
                    this.map.addImage('marker2', image)
                })




            //adding dummy point

            this.map.on('load',()=>{
               
            })
            

        },
        setRoute(){
            this.routeLoader = true
            setTimeout(()=>{
                this.setMapRoute()
            },1000)
        },
        reset(){
            
            
          if(this.map.getLayer('route_serial')){
            this.map.removeLayer('route_serial')
            this.map.removeSource('route_serial')
          } 
          if(this.map.getLayer('dummypoint')){
            this.map.removeLayer('dummypoint')
            this.map.removeSource('dummypoint')
          } 
          if(this.map.getLayer('start_point')){
            this.map.removeLayer('start_point')
            this.map.removeSource('start_point')
          } 
          if(this.map.getLayer('end_point')){
            this.map.removeLayer('end_point')
            this.map.removeSource('end_point')
          } 
          let allLayers = this.map.getStyle().layers
            allLayers.forEach((item, index)=>{
                if (item.id.startsWith('routedummy')) {
                const id = item.id.split('routedummy').pop()

                // Remove route
                this.map.removeLayer('routedummy' + id)
                this.map.removeSource('routedummy' + id)
                }
                if (item.id.startsWith('icondummy')) {
                const id = item.id.split('icondummy').pop()

                // Remove route
                this.map.removeLayer('icondummy' + id)
                this.map.removeSource('icondummy' + id)
                }
            })

        },
        setMapRoute(){
            this.reset()
            this.map.addLayer({
                  'id': 'dummypoint',
                  'type': 'symbol',
                  'source': {
                    type: 'geojson',
                    data: 'sample/sample-point.json'
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

            let url = 'https://api.mapbox.com/directions/v5/mapbox/driving/' +
            this.departureCoordinate.lng +
            ',' +
            this.departureCoordinate.lat +
            ';'

            this.arrivalCoordinate.forEach((item, index)=>{
                url = url + item.lng + ',' + item.lat
                if(index!==this.arrivalCoordinate.length-1){
                    url = url + ';'
                }
            })

            url = url + '?geometries=geojson&access_token=' + process.env.VUE_APP_MAPBOX_TOKEN
            axios
            .get(url)
            .then((res) => {

                this.routeStatus = true
                this.map.flyTo({
                center:[ 108.527299,-9.544916], // Fly to the selected target
                duration: 5000, // Animate over 12 seconds
                zoom:6,
                essential: true // This animation is considered essential with
                //respect to prefers-reduced-motion
                });

                this.routeLoader=false
                console.log(res)

                 //draw route

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
                                coordinates: [ this.departureCoordinate.lng,  this.departureCoordinate.lat]
                            }
                            }
                        },layout: {
                                'icon-image': 'startpoint2',
                                'icon-size': 0.3,
                                "icon-allow-overlap" : true,
                                "text-allow-overlap": true
                            }
                    })
                
                    

                 this.map.addLayer({
                        id: 'end_point',
                        type: 'symbol',
                        source: {
                            type: 'geojson',
                            data: {
                            type: 'Feature',
                            properties: {},
                            geometry: {
                                type: 'Point',
                                coordinates: [ this.arrivalCoordinate[this.arrivalCoordinate.length-1].lng,  this.arrivalCoordinate[this.arrivalCoordinate.length-1].lat]
                            }
                            }
                        },layout: {
                                'icon-image': 'startpoint',
                                'icon-size': 0.3,
                                "icon-allow-overlap" : true,
                                "text-allow-overlap": true
                            }
                    })


                 this.map.addLayer({
                        id: 'route_serial',
                        type: 'line',
                        source: {
                            type: 'geojson',
                            data: {
                            type: 'Feature',
                            properties: {},
                            geometry: {
                                type: 'LineString',
                                coordinates: res.data.routes[0].geometry.coordinates
                            }
                            }
                        },
                        layout: {
                            'line-join': 'round',
                            'line-cap': 'round'
                        },
                        paint: {
                            'line-color': '#03fcb1',
                            'line-width': 3,
                            'line-opacity': 1
                        }
                    })

                res.data.routes[0].legs.forEach((item,index)=>{    

                    this.map.addLayer({
                        'id': 'routedummy'+index,
                        'type': 'symbol',
                        'source': {
                            type: 'geojson',
                            data: {
                            type: 'FeatureCollection',
                            features: [
                                {
                                type: 'Feature',
                                properties: {
                                    title: index + 1,
                                    distance: (item.distance/1000).toFixed(2) + ' km',
                                },
                                geometry: {
                                    type: 'Point',
                                    coordinates: [ this.arrivalCoordinate[index].lng,  this.arrivalCoordinate[index].lat]
                                }
                                }
                            ]
                            }
                        },
                        paint:{
                            'text-color':'#ffffff',
                        },
                        'layout': {
                            'text-field': ['get', 'distance'],
                            'text-variable-anchor': ['top', 'bottom', 'left', 'right'],
                            'text-radial-offset':1,
                            'text-justify': 'auto',
                                "icon-allow-overlap" : true,
                                "text-allow-overlap": true
                        }
                        });
                        
                        
                        if(index!==res.data.routes[0].legs.length-1){
                        this.map.addLayer({
                        'id': 'icondummy'+index,
                        'type': 'symbol',
                        'source': {
                            type: 'geojson',
                            data: {
                            type: 'FeatureCollection',
                            features: [
                                {
                                type: 'Feature',
                                geometry: {
                                    type: 'Point',
                                    coordinates: [ this.arrivalCoordinate[index].lng,  this.arrivalCoordinate[index].lat]
                                }
                                }
                            ]
                            }
                        },
                            layout: {
                                'icon-image': 'marker2',
                                'icon-size': 0.3,
                                "icon-allow-overlap" : true,
                                "text-allow-overlap": true
                            }
                        }); 
                    }
                   
                })
            })

            console.log(url)
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