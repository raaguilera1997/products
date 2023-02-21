<template>
  <q-page>
    <div class="row ">
      <div class="col-6">
      <show-product :products="products"></show-product>
      </div>
      <div class="col-xs-12 col-md-6 fixed-right" style="top: 23vw" >

<!--        <q-fab padding="10px" color="purple" icon="las la-backspace" label="BACK" direction="up" style="left: 20vw;bottom:5vw"></q-fab>-->
        <Vue3Lottie :animationData="ProductsJSON" :width="400" :height="400"/>
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import {Vue3Lottie} from 'vue3-lottie'
import 'vue3-lottie/dist/style.css'
import ProductsJSON from 'src/assets/JSON_LOTTIE/compra.json'
import {api} from "boot/axios";
import {onMounted,ref} from "vue";
import config from "src/conf/config";
import {Loading, Notify, QSpinnerTail} from "quasar";
import Product from "components/types/Product"
import {useRoute} from "vue-router";
import ShowProduct from "components/Products/components/ShowProduct.vue";
let route=useRoute()
let products=ref<Product[]>([])
onMounted(()=>{
  loadData()
})

  async function loadData() {
  let temp=<Product[]>[]
    const url = `${config.api.get_products_by_category}/${route.params.categoria}`
    Loading.show({
      spinnerSize: 120,
      backgroundColor: 'white',
      message: 'Loadding...',
      messageColor: 'white',
      spinnerColor: 'ap-primary',
    })

    await api.get(url).then(resp => {
      resp.data.map(item=>{
        let object={
          ...item,
          cant:0
        }
        temp.push(object)
      })
      products.value=temp
    }).catch((reason) => {
      return {status: reason.response.status, message: reason.response.data.message}
    }).finally(() => {
      Loading.hide()

    })
  }

</script>

<style lang="scss" scoped>
.q-page {
  background: transparent;
  position: relative;
  &::after {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    //background: rgb(0, 40, 255);
    width: 100%;
    height: 100%;
    z-index: -1;
    clip-path: polygon(80% 0%, 100% 0, 100% 100%, 0 99%, 0 33%);
  }
  &::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    background: #246b8c;
    width: 100%;
    height: 100%;
    //z-index: 2;
    opacity: 0.80;
    clip-path: polygon(100% 0, 0% 100%, 100% 100%);
  }
}


</style>