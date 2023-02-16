<template>
  <div class="q-pa-lg">
    <div class="row q-col-gutter-md ">

      <q-list
          v-for="item in products"
          :key="item"
          class="col-6"
      >
        <q-card class="my-card q-pa-md " style="height: 25vw">
          <div class="text-h6 q-mb-xs">{{item.title}}</div>
          <div class="q-pt-md" style="text-align: center">
            <q-img :src="item.image"  style="width:130px;height:130px;"/>
          </div>
          <q-card-actions class="absolute-bottom-left" align="left">
            <div class="text-ap-primary-dark" style="font-weight: bold">${{item.price}}</div>

          </q-card-actions>

          <q-card-actions class="absolute-bottom-right" align="right">
            <q-btn flat round color="red" icon="las la-info-circle" />
            <q-btn flat round color="primary" icon="las la-shopping-cart" size="lg" />

          </q-card-actions>
        </q-card>
      </q-list>
    </div>
  </div>
</template>

<script setup lang="ts">


const props = defineProps({
  products: { type: [], required: true },
})
</script>

<style scoped>

</style>