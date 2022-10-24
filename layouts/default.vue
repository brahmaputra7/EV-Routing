<template>
  <v-app dark>
    
  <div class="d-flex align-center justify-center pageContainer">
    <div class="evContainer">
    <nuxt/>
    </div>
    </div>
  </v-app>
</template>

<style>
@import url('https://fonts.googleapis.com/css2?family=Montserrat:ital,wght@0,100;0,200;0,300;0,400;0,500;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,500;1,600;1,700;1,800;1,900&display=swap');
#app {
  font-family: 'Montserrat', sans-serif;
}


.pageContainer {
  background-color:#000000; 
  min-height:100vh;
}
.evContainer {
  background-color:#101010;
  width:350px;
  height:700px;
  border-radius:20px;
  overflow-y:auto;
}

@media only screen and (max-width: 600px) {
  .evContainer {
    width:100%;
    min-height:100vh;
  }
}

/* width */
::-webkit-scrollbar {
  width: 4px;
}

/* Track */
::-webkit-scrollbar-track {
  background: #000;
}

/* Handle */
::-webkit-scrollbar-thumb {
  background: #0d0d0d;
}

/* Handle on hover */
::-webkit-scrollbar-thumb:hover {
  background: #555;
}

</style>

<script>
export default {
  data () {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: 'mdi-apps',
          title: 'Welcome',
          to: '/'
        },
        {
          icon: 'mdi-chart-bubble',
          title: 'Inspire',
          to: '/inspire'
        }
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: 'Vuetify.js'
    }
  }
}
</script>
