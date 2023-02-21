<template>
  <q-layout view="lHh Lpr lFf" class="bg-white">
    <q-header elevated>
      <q-toolbar class="bg-ap-primary">
<!--        <q-btn-->
<!--          flat-->
<!--          dense-->
<!--          round-->
<!--          icon="menu"-->
<!--          aria-label="Menu"-->
<!--          @click="toggleLeftDrawer"-->
<!--        /> -->
        <q-avatar>
          <q-img src="~assets/store.png" width="30px"></q-img>
        </q-avatar>

        <q-toolbar-title>
          PRODUCT STORE

        </q-toolbar-title>
        <q-badge class="alarm" color="red" floating>{{store.get_cant_product}}</q-badge>


        <q-btn v-if="nameRoute=='Product'" flat rounded icon="las la-backspace" @click="router.push({name:'Home'})">
          <q-tooltip class="bg-grey-3 text-dark" style="font-weight: bold" content-style="font-size: 13px">
            Back</q-tooltip>
        </q-btn>
        <q-btn-dropdown rounded :auto-close="store.get_cant_product==0"   :disable="store.get_cant_product==0"  flat color="white" icon="las la-shopping-cart">
          <div v-if="!store.get_cant_product==0">
            <q-item-label class="bg-ap-light text-dark text-center" style="font-weight: bold" header>PRODUCTS</q-item-label>
            <q-list class="rounded-borders" v-for="item in store.get_products"
                    :key="item" >
              <q-item clickable v-ripple>
                <q-item-section avatar top>
                  <div class="q-pt-md">
                    <q-img  :src="item.image"  width="40px">
                    </q-img>
                  </div>



                </q-item-section>

                <q-item-section>
                  <q-item-label >{{cortarTextoConPuntos(item.title,20)}}</q-item-label>
                  <q-item-label style="font-size: 12px">
                    <span> cantidad: </span>
                    <span class="text-dark" style="font-weight: bold">
                          {{ item.cantidad }}
                        </span>
                  </q-item-label>
                  <q-item-label style="font-size: 12px">
                    <span> price: </span>
                    <span class="text-ap-primary" style="font-weight: bold">
                          {{ item.price }}
                        </span>
                  </q-item-label>
                  <q-item-label style="font-size: 12px">
                    <span> total: </span>
                    <b class="text-ap-primary-dark" >
                      {{ item.cantidad*item.price }}
                    </b>
                  </q-item-label>
                </q-item-section>

                <q-item-section side>
                  <div class="row">
                    <q-btn flat round color="ap-primary" icon="las la-plus-circle" @click="increaseProduct(item)">
                      <q-tooltip class="bg-grey-3 text-dark" style="font-weight: bold" content-style="font-size: 13px" >
                        increase  </q-tooltip>
                    </q-btn>
                    <q-btn flat round color="ap-primary" icon="las la-minus-circle" @click="decreaseProduct(item)" >
                      <q-tooltip class="bg-grey-3 text-dark" style="font-weight: bold" content-style="font-size: 13px">
                        reduce </q-tooltip>
                    </q-btn>
                    <q-btn flat round color="ap-primary" icon="las la-trash-alt" @click="deleteProduct(item)" >
                      <q-tooltip class="bg-grey-3 text-dark" style="font-weight: bold" content-style="font-size: 13px">
                        delete </q-tooltip>
                    </q-btn>
                  </div>

                </q-item-section>

              </q-item>

            </q-list>
            <q-item class="bg-ap-light text-dark text-center" style="font-weight: bold">
              <q-item-section >TOTAL AMOUNT: {{store.get_monto_total}}</q-item-section>
            </q-item>
          </div>


        </q-btn-dropdown>
<!--
        <div>Quasar v{{ $q.version }}</div> -->
      </q-toolbar>
    </q-header>

<!--    <q-drawer-->
<!--        v-model="leftDrawerOpen"-->
<!--        show-if-above-->
<!--        :width="250"-->
<!--        :breakpoint="500"-->
<!--        bordered-->
<!--        class="bg-grey-3"-->
<!--    >-->
<!--      <q-item-label class="text-dark" header style="font-weight: bold" >PRODUCT STORE</q-item-label>-->


<!--      <q-list>-->
<!--        <template v-for="(menuItem, index) in essentialLinks" :key="index">-->
<!--          <q-item clickable :active="menuItem.label === 'Outbox'" v-ripple>-->
<!--            <q-item-section avatar>-->
<!--              <q-icon :name="menuItem.icon" />-->
<!--            </q-item-section>-->
<!--            <q-item-section>-->
<!--              <q-item-label>{{menuItem.title}}</q-item-label>-->
<!--              <q-item-label caption lines="2">{{menuItem.description}}</q-item-label>-->
<!--            </q-item-section>-->
<!--          </q-item>-->
<!--          <q-separator  />-->
<!--        </template>-->

<!--      </q-list>-->




    <q-page-container class="bg-grey-1">
      <transition
          enter-active-class="animated slideInRight"
          leave-active-class="animated fadeOut"
          mode="out-in"
      >
        <router-view/>
      </transition>
    </q-page-container>
  </q-layout>
</template>

<script setup lang="ts">
import {computed, ref} from 'vue';
import EssentialLink, { EssentialLinkProps } from 'components/EssentialLink.vue';
import {useProducts} from 'src/stores/products'
import Product from "components/types/Product";
import {useRoute, useRouter} from "vue-router";
let route=useRoute()
let router=useRouter()
let store = useProducts()
const essentialLinks: EssentialLinkProps[] = [
  {
    title: 'ELECTRONICS',
    description: 'Here you will find all electronic products',
    icon: 'link'
  },
  {
    title: 'JEWELERY',
    description: 'Here you can find all the jewelry',
    icon: 'link',

  },
  {
    title: "MEN'S CLOTHING",
    description: "Here you can find all men's clothes",
    icon: 'link'

  },
  {
    title: "WOMEN'S CLOTHING",
    description: "Here you can find all women's clothes",
    icon: 'link'

  },

];

const leftDrawerOpen = ref(false)

let nameRoute=computed(()=>{
  return route.name
})
function increaseProduct(value) {
 value.cantidad++

}
function decreaseProduct(value) {
 value.cantidad--
  if(value.cantidad==0){
    deleteProduct(value)
  }
}
function toggleLeftDrawer() {
  leftDrawerOpen.value = !leftDrawerOpen.value
}
function cortarTextoConPuntos(texto, limite) {
  var puntosSuspensivos = "...";
  if(texto.length > limite)
  {
    texto = texto.substring(0,limite) + puntosSuspensivos;
  }

  return texto;
}
function deleteProduct(product: Product) {
  store.deleteProduct(product.id)
  product.cantidad = 0
}
</script>
<style>
.alarm{
  margin-top: 5px;
  margin-right: 50px;
}
</style>