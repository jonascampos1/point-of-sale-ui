<template>
  
  <v-content>
      <v-container fluid ma-0 pa-0 fill-height>
        <v-layout row>
          <v-flex xs4> Chat List </v-flex>
          <v-flex xs4> Main Chat</v-flex>
          <v-flex xs4> User Profile</v-flex>
        </v-layout>
      </v-container>
    </v-content>
    
</template>

<script lang='ts'>
import { defineComponent } from 'vue'

// Logo
import logo from '../assets/logo.svg'

export default defineComponent({
  name: 'HelloWorld',

  data () {
    return {
      logo: "",
    }
  },
})
</script>
