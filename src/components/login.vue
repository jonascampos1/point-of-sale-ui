<template>
  <v-container fluid class="fill-height">
    <v-col class="text-xl-caption text-xs-body-2">
      <v-row justify="center">
      <v-card max-width="350" title="Login" class="v-col-sm-6  bg-grey-lighten-4" elevation="2">
        <v-col justify="center" class="mt-0 pl-2 pr-2">
          <!--<v-icon size="small" color="grey" slot="prepend">
            mdi-account
          </v-icon>-->

          <v-form ref="form" v-model="valid" lazy-validation @submit.prevent="submit" id="loginForm">
            <v-text-field
              v-model="username"
              label="Usuario"
              :rules="[UsernameRules.required, UsernameRules.nonumbers]">
            </v-text-field>
            
            <v-text-field
              v-model="password"
              :append-icon="show1 ? 'mdi-eye' : 'mdi-eye-off'"
              :rules="[rules.required, rules.min, rules.numbers]"
              :type="show1 ? 'text' : 'password'"
              name="password"
              label="Pin"
              hint="Pin aceptado"
              counter
              @click:append="show1 = !show1">
            </v-text-field>
            <v-row justify="end" class="mr-10">
              <v-btn type="submit" form="loginForm" :disabled="!valid" elevation="2" @cklick="submit">Login</v-btn>
            </v-row>
          </v-form>
        </v-col>
      </v-card>
      </v-row>
    </v-col>
  </v-container>


<!--
  <v-container class="grey lighten-5">
    <v-row no-gutters>
      <v-col
        v-for="n in 3"
        :key="n"
        cols="12"
        sm="4"
      >
        <v-card
          class="pa-2"
          outlined
          tile
        >
          One of three columns
        </v-card>
      </v-col>
    </v-row>
  </v-container>
  -->
</template>

<script lang='ts'>
import { defineComponent } from 'vue'
import axios from 'axios'
// Logo
import logo from '../assets/logo.svg'

export default defineComponent({
  name: 'HelloWorld',

  data () {
      return {
        username: '',
        valid: false,
        otp: '',
        show1: false,
        password: '',
        rules: {
          numbers: value => Number(value) || 'Solo números',
          required: value => !!value || '(6 digitos).',
          min: v => v.length == 6 || '6 dígitos',
          emailMatch: () => (`The email and password you entered don't match`),
        },
        UsernameRules: {
          nonumbers: value => this.onlyleters(value) || 'Solo letras',
          required: value => !!value || '(Solo letras).',
          min: v => v.length >= 3 || 'Mínimo 3 caracteres',
        },
      }
    },
    methods:{
      onlyleters(v){
        if(/^[A-Za-z]+$/.test(v)) return true
      },
      submit(){
        const userdata = { username : this.username , password: this.password }
          axios.post('http://127.0.0.1:3001/api/v1/login', userdata)
          .then(res => {
            // do something with res
            alert('Success')
          })
          .catch(err => { 
            alert('Acceso denegado')
          })
      }
    }
})
</script>
