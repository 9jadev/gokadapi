<template>
    <div class="main"> 
        <b-form inline class="center">
            <label class="sr-only" for="inline-form-input-name">Pickup Address</label>
            <b-input
                id="inline-form-input-name"
                class="mb-2 mr-sm-2 mb-sm-0"
                placeholder="Pickup Address"
                v-model="address"
                required
            ></b-input>

            <b-button variant="primary" :disabled="disabled" @click="pickup">Submit</b-button>
        </b-form>
    </div>
</template>
<script>
import axios from 'axios'
export default {
    data (){
        return {
            address: '' ,
            disabled: false,
            ress: {}
        }
    },
    methods: {
        pickup(){
            this.disabled = true
            axios.get('https://maps.googleapis.com/maps/api/geocode/json',{
                params:{
                    address: this.address,
                    key: process.env.VUE_APP_GOOGLE
                }
            })
            .then((response) => {
                this.ress = response.data.results[0].geometry.location;
                this.disabled = false
                this.$emit("pickup", response.data.results[0].geometry.location);
            })
            .catch((error) => {
                console.log(error)
                this.disabled = false
            });
        }
    }
}
</script>
<style scoped>
.main{
  margin: auto;
}
</style>