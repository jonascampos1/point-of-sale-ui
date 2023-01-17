<template>
  <v-container fluid fill-height>
    <v-layout fill-height>
    <v-col class="text-xl-caption text-xs-body-2">
      <v-row justify="center">
        <v-img
          contain
          lazy-src="src/assets/images/pos.png"
          max-height="45"
          max-width="74"
          src="src/assets/images/pos.png">
        </v-img>
        <h1>Punto de venta</h1>
      </v-row>
      
      <v-row justify="center">
        
      <v-card max-width="350" title="Login" class="v-col-sm-6  bg-grey-lighten-4" elevation="2">
        <v-col justify="center" class="mt-0 pl-2 pr-2">

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

      <v-row justify="center">
        <v-col cols="3" justify="center">
          <v-alert v-model="login_success" transition="fade-transition" class="mt-5"
            density="comfortable"
            type="success"
            variant="tonal">
            Acceso concedido
          </v-alert>
          <v-alert v-model="login_error" transition="fade-transition" class="mt-5"
            density="comfortable"
            type="warning"
            variant="tonal">
            Acceso denegado
          </v-alert>
        </v-col>
      </v-row>

    </v-col>

  </v-layout>
  </v-container>
</template>

<script lang='ts'>
import { defineComponent } from 'vue'
import axios from 'axios'

export default defineComponent({
  name: 'HelloWorld',

  data () {
      return {
        username: '',
        valid: false,
        otp: '',
        show1: false,
        password: '',
        login_success: false,
        login_error: false,
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
            this.login_success=true
            setTimeout(()=>{
              this.login_success=false
            },2000)
          })
          .catch(err => { 
            this.login_error=true
            setTimeout(()=>{
              this.login_error=false
            },2000)
          })
      }
    }
})
</script>
