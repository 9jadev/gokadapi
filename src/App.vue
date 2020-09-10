<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> -->

    <b-container class="bv-example-row">
      <b-row>
        <b-col></b-col>
        <b-col cols="8">
          <div class="main">
            <h1>Gokada Estimation Api </h1>

            <div class="form-row align-items-center"> 
              <pickup-address @pickup="dopickup" :res="res"></pickup-address>
            </div>

            <div class="form-row align-items-center"> 
              <delivery-address @delivery="dodeliver" :res="res"></delivery-address>
            </div>

              <b-button @click="estimate" v-show="this.pickup != null && this.delivery != null" size="lg">Estimate Price</b-button>
          </div>
        </b-col>
        <b-col></b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'

import PickupAddress from './components/PickupAddress.vue'
import DeliveryAddress from './components/DeliveryAddress.vue'
import axios from 'axios'
export default {
  name: 'App',
  components: {
    // HelloWorld
    PickupAddress,
    DeliveryAddress
  },
  data (){
      return {
        pickup: null,
        res: '',
        delivery: null
      }
  },
  methods: {
    dopickup(res){
      console.log(res)
      this.pickup = res
    },
    estimate(){
      let forms = {
        api_key: '2m7kdxnQSOjov7eIETMawpmeVtoMUlAwnAAdW40jhCbG5P2YzNENJpJmdCPA_test',
        pickup_latitude: this.pickup.lat,
        pickup_longitude: this.pickup.lng,
        delivery_latitude: this.delivery.lat,
        delivery_longitude: this.delivery.lng,
      }
      console.log(forms)
      axios.post('https://api.gokada.ng/api/developer/order_estimate',forms,{
                headers: {
                  Accept : 'application/json'
                }
            })
            .then((response) => {
                console.lo(response)
            })
            .catch((error) => {
                console.log(error);
            });
    },
    dodeliver(res){
      console.log(res)
      this.delivery = res
    }
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
.main{
  text-align: center;
}
</style>
