<template>
  <q-page>
    <div class="row ">
      <div class="col-6">
      <Categories :categories="categories"></Categories>
      </div>
      <div class="col-xs-12 col-md-6 flex flex-center q-pt-md ">
        <Vue3Lottie :animationData="PrincipalJSON" :width="400" :height="400"/>
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import {Vue3Lottie} from 'vue3-lottie'
import 'vue3-lottie/dist/style.css'
import PrincipalJSON from '../assets/JSON_LOTTIE/principal.json'
import {api} from "boot/axios";
import {onMounted,ref} from "vue";
import config from "src/conf/config";
import {Loading, Notify, QSpinnerTail} from "quasar";
import Categories from "components/HomePage/Categories.vue";

let categories=ref([])
onMounted(() => {
  loadData()
})

async function loadData() {
  const url = config.api.get_categories
  Loading.show({
    spinnerSize: 120,
    backgroundColor: 'white',
    message: 'Cargando...',
    messageColor: 'white',
    spinnerColor: 'ap-primary',
  })

   await api.get(url).then(resp => {
       categories.value=resp.data
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
