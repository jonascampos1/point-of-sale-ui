<template>
  <v-app id="inspire">
    <v-app-bar
      app
      color="white"
      flat
    >
      <v-container class="py-0 fill-height">
        <v-col justify="stretch">
          <v-avatar
            class="mr-2"
            color="grey darken-1"
            size="40"
          ></v-avatar>
          <span>Jonas</span>
        </v-col>

       

        <v-spacer></v-spacer>

        <v-responsive  max-width="120">
          <v-btn :loading="loading" @click="logout" prepend-icon="mdi-logout">Logout</v-btn>
        </v-responsive>
      </v-container>
    </v-app-bar>

    <v-main class="bg-grey-lighten-4 ">
      <v-container>
        <v-row>
          <v-col cols="3">
            <v-sheet rounded="xl">
              <v-list rounded="xl" density="compact">
                <v-list-subheader>SERVICIOS</v-list-subheader>

                <v-list-item
                  v-for="(item, i) in menu_items"
                  :key="i"
                  :value="item"
                  active-color="red"
                >
                  <template v-slot:prepend >
                    <v-icon :icon="item.icon" class="mr-2"></v-icon>
                  </template>

                  <v-list-item-title v-text="item.text"></v-list-item-title>
                </v-list-item>
              </v-list>
            </v-sheet>
          </v-col>

          <v-col>
            <v-sheet
              min-height="70vh"
              rounded="xl"
            >
            
              <!--  -->
            </v-sheet>
            <v-footer color="transparent" 
              class="mt-2 pa-0" 
              rounded="xl">
                <v-text-field bg-color="white"
                  class="mt-0"
                  variant="outlined" 
                  color="orange"
                  placeholder="Código de producto">
                </v-text-field>
            </v-footer>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'test',

  data () {
      return {
        userinfo: {},
        loader: null,
        loading: false,
        menu_items: [
        { text: 'Punto de venta', icon: 'mdi-cart' },
        { text: 'Historial', icon: 'mdi-account' },
        { text: 'Opcion 3', icon: 'mdi-flag' },
      ],
      links: [
        'Dashboard',
        'Messages',
        'Profile',
        'Updates',
      ],
     }
  },
  methods:{
    logout(){
      this.loader = 'loading'
      localStorage.removeItem('user-info')
      setTimeout(()=>{
        this.$router.push({name: 'login'})
      },1000)
      
    }
  },
  watch: {
    loader () {
      const l = this.loader
      this[l] = !this[l]
      setTimeout(() => (this[l] = false), 1000)
      this.loader = null
    },
  },
  mounted(){
    let user = localStorage.getItem('user-info')
    this.userinfo = JSON.parse(user)
    if(!user){
      this.$router.push({name: 'login'})
      console.log(user)
    }
  }
})
</script>