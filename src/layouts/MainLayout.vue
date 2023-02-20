<template>
  <q-layout view="lHh Lpr lFf" class="bg-white">
    <q-header elevated>
      <q-toolbar class="bg-ap-primary">
        <q-btn
          flat
          dense
          round
          icon="menu"
          aria-label="Menu"
          @click="toggleLeftDrawer"
        /> 
        <q-avatar>
          <q-img src="~assets/store.png" width="30px"></q-img>
        </q-avatar>

        <q-toolbar-title>
          PRODUCT STORE
        </q-toolbar-title>
        <q-badge class="alarm" color="red" floating>{{store.get_cant_product}}</q-badge>
        <q-btn-dropdown :auto-close="store.get_cant_product==0"   :disable="store.get_cant_product==0"  flat color="white" icon="las la-shopping-cart">
          <div v-if="!store.get_cant_product==0">
            <q-item-label class="bg-ap-light text-dark text-center" style="font-weight: bold" header>PRODUCTS</q-item-label>
            <q-list class="rounded-borders" v-for="item in store.get_products"
                    :key="item" >
              <q-item clickable v-ripple>
                <q-item-section avatar top>
                  <q-img  :src="item.image" height="40px" width="40px">

                  </q-img>
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
                    <span> total: </span>
                    <b class="text-red" >
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

    <!-- <q-drawer
      v-model="leftDrawerOpen"
      show-if-above
      bordered
    >
      <q-list>
        <q-item-label
          header
        >
          Essential Links
        </q-item-label>

        <EssentialLink
          v-for="link in essentialLinks"
          :key="link.title"
          v-bind="link"
        />
      </q-list>
    </q-drawer> -->

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
import { ref } from 'vue';
import EssentialLink, { EssentialLinkProps } from 'components/EssentialLink.vue';
import {useProducts} from 'src/stores/products'
import Product from "components/types/Product";
let store = useProducts()
const essentialLinks: EssentialLinkProps[] = [
  {
    title: 'Docs',
    caption: 'quasar.dev',
    icon: 'school',
    link: 'https://quasar.dev'
  },
  {
    title: 'Github',
    caption: 'github.com/quasarframework',
    icon: 'code',
    link: 'https://github.com/quasarframework'
  },
  {
    title: 'Discord Chat Channel',
    caption: 'chat.quasar.dev',
    icon: 'chat',
    link: 'https://chat.quasar.dev'
  },
  {
    title: 'Forum',
    caption: 'forum.quasar.dev',
    icon: 'record_voice_over',
    link: 'https://forum.quasar.dev'
  },
  {
    title: 'Twitter',
    caption: '@quasarframework',
    icon: 'rss_feed',
    link: 'https://twitter.quasar.dev'
  },
  {
    title: 'Facebook',
    caption: '@QuasarFramework',
    icon: 'public',
    link: 'https://facebook.quasar.dev'
  },
  {
    title: 'Quasar Awesome',
    caption: 'Community Quasar projects',
    icon: 'favorite',
    link: 'https://awesome.quasar.dev'
  }
];

const leftDrawerOpen = ref(false)

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