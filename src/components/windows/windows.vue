<template>
  <q-dialog v-model="activeWindow" persistent :full-width="fullWidth" @show="fireEventShow" @before-hide="hide">
    <q-card :style="style">
      <q-card-section class="row items-center q-pa-sm bg-ap-primary text-white">
        <div class="text-h6">{{ title }}</div>
        <q-space />
        <q-btn icon="close" flat round dense v-close-popup />
      </q-card-section>


      <q-separator />

      <q-card-section class="scroll bg-grey-1"  style="max-height: 70vh" :class="bodyClass">
        <slot name="body" />
      </q-card-section>

      <q-separator />

      <q-card-actions :align="alignActions">
        <slot name="footer" />
      </q-card-actions>
    </q-card>
  </q-dialog>
</template>

<script>
export default {
  name: 'Window',
  props: {
    title: {
      type: String
    },
    active: {
      type: Boolean,
      default: false
    },
    style: {
      type: String,
      default: ''
    },

    fullWidth: {
      type: Boolean,
      default: false
    },
    bodyClass: {
      type: String,
      default: ''
    },
    alignActions: {
      type: String,
      default: 'right'
    }
  },
  watch: {
    active: function (newVal, oldVal) {
      if (newVal !== oldVal) {
        this.activeWindow = newVal
      }
    }
  },
  data () {
    return {
      activeWindow: false
    }
  },
  methods: {
    getSlotRefByName (name) {
      // return this.$scopedSlots.body()[0].context.$refs[name]
    },
    show () {
      this.activeWindow = true
    },
    hide () {
      this.activeWindow = false
      this.$emit('close')
    },
    fireEventShow () {
      this.$emit('show')
    }
  }
}
</script>
