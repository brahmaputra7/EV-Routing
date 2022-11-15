<template>
  <div class="pt-10 pa-5 landingPage" style="text-align:center">
    <img src="logo.png" width="200px"/>
    <div class="cyan--text"><b>ELECTRIC VEHICLE</b></div>
    <div style="font-size:0.8em">Solution Module</div>
    <v-btn class="mt-3 cyan"  @click="$router.push('/find-vehicle')"  block>EV Route Plan</v-btn>
    <v-btn class="mt-3 cyan" @click="$router.push('/isochrone')"  block>Isochrone Nearest SPKLU</v-btn>
  </div>
</template>

<style>
.landingPage {
  background:url('/ev/bg.jpg');background-size: cover;
  height:100vh;
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
      CarsData:[
        {
          brand:'Hyundai',
          items:[
            {
              name:'Ioniq 5 - Standard Range (2022)',
              img:'img/ioniq5.png'
            },
            {
              name:'Kona - Single Motor (2021)',
              img:'img/kona.png'
            }
          ]
        },
        {
          brand:'Tesla',
          items:[
            {
              name: 'Model 3 - Rear-Wheel Drive',
              img:'img/model3-1.png'
            },
            {
              name:'Model 3 - Performance',
              img:'img/model3-2.png'
            },
            {
              name:'Model Y - Long Range',
              img:'img/modelY-1.png'
            },
            {
              name:'Model Y - Performance',
              img:'img/modelY-2.png'
            },
            {
              name:'Model S - Plaid',
              img:'img/modelS-1.png'
            }
          ]
        },
        {
          brand:'Nissan',
          items:[
            {
              name:'Kicks e-Power',
              img:'img/kicks.png'
            }]
        }
      ]
    }
  }
}
</script>
