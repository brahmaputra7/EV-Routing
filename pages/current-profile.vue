<template>
  <div class="pa-5">
    <div class="carContainer pa-5" data-aos="zoom-in" style="text-align: center;">
      <img src="/img/ioniq5.png" /> 
      <b>IONIQ 5</b><br/>
      Standard Range (2022)
    </div>
    <div class="d-flex justify-center pt-4">
      <div style="font-size:0.7em" class="mr-5">
        <div class="mb-2"><v-icon small>mdi-battery-charging-high</v-icon> 58 kWh<br/></div>
        <v-icon small>mdi-ray-start-arrow</v-icon> 390 km (WLTP)
      </div>
      <div style="font-size:0.7em">
        <div class="mb-2"><v-icon small>mdi-speedometer</v-icon> 185 km/h<br/></div>
        <v-icon small>mdi-car-battery</v-icon> 125 kWh
      </div>
    </div>
    <div class="mt-3">
      <div class="d-flex justify-space-between align-center" style="font-size:0.8em">
        <b>Battery Level</b>
        <div>{{batteryLevel}}%</div>
      </div>
      <div> 
          <v-slider
            v-model="batteryLevel"
            max="100"
            color="cyan"
            hide-details
          ></v-slider>
        </div>

        
      <div class="d-flex justify-space-between align-center" style="font-size:0.8em">
        <b>Weight</b>
        <div>{{weight}} person</div>
      </div>
      <div> 
          <v-slider
            v-model="weight"
            max="7"
            color="cyan"
            hide-details
          ></v-slider>
        </div>

              
      <div class="d-flex justify-space-between align-center" style="font-size:0.8em">
        <b>User Behavior</b>
      </div>
      <div style="text-align: center;"> 
        <v-btn-toggle
          v-model="btnToggle"
          color="cyan"
          group
        >
          <v-btn value="eco">
            ECO
          </v-btn>

          <v-btn value="normal">
            Normal
          </v-btn>

          <v-btn value="sporty">
            Sporty
          </v-btn>
        </v-btn-toggle>
        </div>

        

        
      <div class="d-flex justify-space-between align-center" style="font-size:0.8em">
        <b>Tire Pressure</b>
        <div>STD +{{tirePressure}} BAR</div>
      </div>
      <div> 
          <v-slider
            v-model="tirePressure"
            max="1"
            min="-1"
            step="0.1"
            color="cyan"
            hide-details
          ></v-slider>
        </div>
        
        <div class="d-flex justify-space-between align-center" style="font-size:0.8em">
          <b>Speed Limiter</b>
          <div>{{speedLimiter}} km/h</div>
        </div>
        <div> 
            <v-slider
              v-model="speedLimiter"
              max="200"
              min="50"
              step="1"
              color="cyan"
              hide-details
            ></v-slider>
          </div>

    </div>
  </div>
</template>

<style>
.carContainer {
  width:100%;
  background-color:#1F1F1F;
  border-radius:10px;
  border:8px solid #009292;
  box-shadow: 0px 4px 4px rgba(0, 0, 0, 0.25);
}
</style>

<script>
import Logo from '~/components/Logo.vue'
import VuetifyLogo from '~/components/VuetifyLogo.vue'

export default {
  components: {
    Logo,
    VuetifyLogo
  },
  data(){
    return {
      batteryLevel:80,
      weight:3,
      btnToggle:'sporty',
      tirePressure:0,
      speedLimiter:90
    }
  }
}
</script>
