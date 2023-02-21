<template>
  <div class="q-pa-lg">
    <div class="row q-col-gutter-md ">
      <q-intersection
          v-for="index in categories.length"
          :key="index"
          transition="scale"
          class="col-6"
      >
        <q-card  class="my-card q-pa-md" @click="router.push({name:'Product',params:{categoria:categories[index-1]}})" style="width:90%;height: 35vh">
          <img :src="index==1?'src/assets/electronics.jpg'
          :index==2?'src/assets/jewelery.jpg'
          :index==3?'src/assets/mens.webp'
          :index==4?'src/assets/woman.webp'
          :''

       " style="width: 100%;height: 65%"/>
          <q-card-section>
            <div class="text-subtitle1" style="font-weight: bold" >{{ categories[index-1].toUpperCase()}}</div>
            <div class="text-caption  text-grey-7" style="font-weight: bold">
            {{index==1?'Here you will find all electronic products':index==2?'Here you can find all the jewelry':index==3?"Here you can find all men's clothes":index==4?"Here you can find all woman's clothes":""}}
            </div>
          </q-card-section>
        </q-card>
      </q-intersection>
    </div>
  </div>
</template>

<script setup lang="ts">
import {onMounted, ref} from "vue";
import {useRouter} from "vue-router";
const props = defineProps({
  categories: { type: [], required: true },
})
const router=useRouter()
</script>

<style lang="scss" scoped>
.example-item{
  height: 40%;
  width: 40%;
}


.my-card:hover{
  /*transform: translateZ(50px)scale(0.95);*/

  margin-top: -5px;
  margin-bottom: 5px;
  cursor: pointer;
  background: lightblue;
}
.my-card{
  /*transform: translateZ(50px)scale(0.95);*/
  transition-duration: 0.5s
}
</style>