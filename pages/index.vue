<template>
  <div>
      <div class="pa-5" >
        Find your vehicle
        <v-text-field hide-details class="mt-2" solo-inverted dense prepend-inner-icon="mdi-magnify"></v-text-field>
      </div>
      <div>
        <div v-for="item,index in CarsData" :key="index">
          <div class="px-5 py-2"><b>{{item.brand}}</b></div>
          <div v-for="item2,index2 in item.items" :key="index2" class="px-5 py-5" style="border-bottom:1px solid #262626;cursor:pointer" v-ripple @click="$router.push('/current-profile')">
            {{item2.name}}
          </div>
        </div>
      </div>
  </div>
</template>

<style>
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
