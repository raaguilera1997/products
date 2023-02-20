<template>
  <div class="q-pa-lg">
    <div class="row q-col-gutter-md ">

      <q-list
          v-for="item in products"
          :key="item"
          class="col-6"
      >
        <q-card class="q-pa-md my-card " style="height: 35vw">
          <div class="text-h6 q-mb-xs">{{item.title}}</div>
          <div class="absolute-center q-mt-xl">
            <q-img  :src="item.image"  style="width:130px"/>
              <q-rating v-model="item.rating.rate" class="q-pt-md"  size="1.8em" :max="5" color="ap-primary" />

          </div>

          <q-card-actions class="absolute-bottom-left " align="left">
            <div class="text-dark q-pb-sm" style="font-weight: bold">${{item.price}}</div>

          </q-card-actions>
          <q-card-actions class="absolute-bottom-right" align="right">
            <q-btn class="BtnShopping q-pt-xl" flat  rounded  icon="las la-info-circle" size="md">
              <q-tooltip class="bg-grey-3 text-justify" anchor="top middle" self="bottom middle"
                         max-width="16rem">
                <q-item-label class="text-black text-caption" style="font-size: 11px">
                  <b> Description: </b>
                  <span>
                          {{ item.description }}
                        </span>
                </q-item-label>
              </q-tooltip>
            </q-btn>
            <q-btn class="BtnShopping q-pt-xl" flat  rounded  icon="las la-shopping-cart" size="md">
              <q-tooltip class="bg-grey-3 text-dark" style="font-weight: bold" content-style="font-size: 13px">
                Add to Cart</q-tooltip>
            </q-btn>

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

<style lang="scss" scoped>
.container {
  display: flex;
  justify-content: center;
}

.my-card:hover {
  transform: translateZ(50px) scale(0.98);
  cursor: pointer;
  background: lightcyan;
}

.my-card {
  /*transform: translateZ(50px)scale(0.95);*/
  transition-duration: 0.5s
}
.BtnShopping{
  background: white;
  color: #1379a8

}

.BtnShopping:hover{
  background:  #1379a8;
  color: white

}





</style>