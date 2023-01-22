<template>
  <v-app :theme="themeColor" id="inspire">
    <v-app-bar app
      flat
      color="grey-darken-3"
    >
      <v-container class="py-0 fill-height">
        <v-col justify="stretch">
          <v-avatar
            class="mr-2"
            color="grey-darken-1"
            size="40"
          ></v-avatar>
          <span>{{ userinfo.username }}</span>
        </v-col>

       

        <v-spacer></v-spacer>

        <v-responsive  max-width="120">
          <v-btn :loading="loading" @click="logout" prepend-icon="mdi-logout">Logout</v-btn>
        </v-responsive>
      </v-container>
    </v-app-bar>

    <v-main class="bg-main">
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
                  @click="handleFunc(item.action)"
                >
                  <template v-slot:prepend>
                    <v-icon :icon="item.icon" class="mr-2"></v-icon>
                  </template>

                  <v-list-item-title v-text="item.text"></v-list-item-title>
                </v-list-item>
              </v-list>
            </v-sheet>
          </v-col>

          <v-col>
            <v-sheet
              rounded="xl"
            >
            
            <v-table fixed-header height="400px" density="compact">
                <thead>
                  <tr>
                    <th class="text-left">
                      Producto
                    </th>
                    <th width="100" class="text-center">
                      Cant.
                    </th>
                    <th width="50" class="text-right">
                      Precio
                    </th>
                    <th width="50" class="text-right">
                      Subtotal
                    </th>
                  </tr>
                </thead>
                <tbody>
                  <tr
                    v-for="(item, index) in product_items"
                    
                  >
                    
                    <td><v-icon @click="deleteItem(index)" color="red">mdi-delete</v-icon> {{ item.product_name }}</td>
                    <td class="text-center">
                      <v-icon @click="decrement(item)" size="x-small" class="mr-2">mdi-minus-circle</v-icon>
                      {{ item.quantity }} 
                      <v-icon @click="increment(item)" class="ml-2" size="x-small">mdi-plus-circle</v-icon>
                      </td>
                    <td class="text-right">${{ item.price }}</td>
                    <td class="text-right">${{ item.price*item.quantity }}</td>
                  </tr>
                </tbody>
              </v-table>
              <v-col class="text-right">
                 <h2>Total: ${{ total }}</h2>
              </v-col>
            </v-sheet>
            <v-col>
              <v-row>Método de pago</v-row> 
                <v-row justify="space-between">
                  <v-btn-toggle 
                    variant="outlined"
                    color="red"
                    v-model="metodo_pago"                          
                  >
                    <v-btn value="efectivo">
                      <span class="hidden-sm-and-down">Efectivo</span>
                      <v-icon end size="x-large">
                        mdi-cash
                      </v-icon>
                    </v-btn>

                    <v-btn value="tarjeta">
                      <span class="hidden-sm-and-down">Tarjeta</span>
                      <v-icon end size="x-large">
                        mdi-credit-card
                      </v-icon>
                    </v-btn>
                    <v-btn value="credito">
                      <span class="hidden-sm-and-down">Crédito</span>
                      <v-icon end size="x-large">
                        mdi-receipt
                      </v-icon>
                    </v-btn>
                  </v-btn-toggle>
                  <v-btn color="red" @click="addSale">Registrar venta</v-btn>
                </v-row>
              </v-col>
            <v-footer color="transparent"  
              class="mt-3 pa-0" 
              rounded="xl">
                <v-text-field ref="product_input"
                  class="mt-0"
                  variant="outlined"
                  color="red"
                  label="Código de producto"
                   v-model="product_id" @keydown.enter.prevent="seekProduct" >
                </v-text-field>
            </v-footer>
          </v-col>
        </v-row>
        <v-snackbar
      v-model="snackbar"
      :timeout="snackbar_timeout"
    >
      {{ snackbar_msg }}

      <template v-slot:actions>
        <v-btn
          color="pink"
          variant="text"
          @click="snackbar = false"
        >
          Close
        </v-btn>
      </template>
    </v-snackbar>
      </v-container>
    </v-main>
    <v-dialog width="450"
      v-model="msgWindow.show"
      transition="dialog-bottom-transition"
    >
      

      <v-card>
        <v-toolbar
              color="red"
              :title="msgWindow.title"
            ></v-toolbar>
        <v-card-text class="text-center">
          {{ msgWindow.msg }}
        </v-card-text>
        <v-card-actions>
          <v-btn color="primary" block @click="msgWindow.show = false">Entendido</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-app>
</template>

<script lang="ts">
import { defineComponent } from 'vue'


export default defineComponent({
  name: 'test',
  
  data () {
      return {
        bg_main : 'red',
        themeColor: 'light',
        msgWindow: {
          show: false,
          title: '',
          msg: ''
        },
        metodo_pago: 'efectivo',
        finded: false,
        snackbar: false,
        snackbar_msg: '',
        snackbar_timeout: 2000,
        db: [
          { product_name: 'Gansito', product_id: 1, quantity: 1, price: 15 },
          { product_name: 'Café sencillo', product_id: 2, quantity: 1, price: 35 },
          { product_name: 'Galletas Chokis', product_id: 3, quantity: 1, price: 18 },
        ],
        product_id: '',
        total: 0,
        product_items: [],
        userinfo: {},
        loader: null,
        loading: false,
        menu_items: [
        { text: 'Punto de venta', icon: 'mdi-cart', action: 'pos'},
        { text: 'Opcion 2', icon: 'mdi-account', action: 'test'},
        { text: 'Opcion 3', icon: 'mdi-flag', action: 'test2' },
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
      
    },
    handleFunc(action){
      if(action=="pos"){
        this.$router.push({name: 'home'})
      }else if(action=="test"){
        this.$router.push({name: 'test'})
      }
    },
    focusInput() {
      this.$refs.product_input.focus();
    },
    getTotal(){
      this.total=0
      this.product_items.forEach( val =>{
        this.total += val.price*val.quantity
      })
    },
    deleteItem(index) {
      this.product_items.splice(index, 1)
      this.getTotal()
    },
    increment(item){
      item.quantity +=1
      this.getTotal()
    },
    decrement(item){
      if(item.quantity > 1){
        item.quantity -=1
      }
      this.getTotal()
    },
    seekProduct(){
      this.finded = false
      this.db.forEach(val => {
        if (val.product_id == this.product_id){
          this.finded=true
          let new_product = 
          { product_name : val.product_name,
            product_id: val.product_id,
            quantity: val.quantity,
            price: val.price,
            index: this.product_items.length
          }
          console.log(new_product)
          this.product_items.push(new_product)
          this.getTotal()
          this.product_id = ''
        }
      })
      if (!this.finded) {
        this.snackbar= true
        this.snackbar_msg = "Producto no encontrado"
      }
    },
    addSale(){
      if(this.product_items.length == 0){
        this.msgWindow.msg ='Primero necesitas registrar un producto'
        this.msgWindow.title = 'Error'
        this.msgWindow.show = true
      }      
    },
    check_scheme(){
      if (window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches) {
        this.themeColor = 'dark'
        this.bg_main = 'bg-black-lighten-1'
      }
      window.matchMedia('(prefers-color-scheme: dark)').addEventListener('change', event => {
        const newColorScheme = event.matches ? "dark" : "light";
          this.themeColor = newColorScheme
      })
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
    this.check_scheme()
    this.focusInput()
    this.getTotal()
    let user = localStorage.getItem('user-info')
    if(user){
      this.userinfo = JSON.parse(user)
    }
  },
  beforeCreate(){
    let usercheck = localStorage.getItem('user-info')
    if(!usercheck){
      this.$router.push({ name: 'login' })
    }else{
      this.userinfo = JSON.parse(usercheck)
    }
  }
})
</script>
<style>
.bg-main{
  background: var(--color-background-mute);
}

</style>