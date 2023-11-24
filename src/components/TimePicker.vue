<template>
  <q-input outlined v-model="dateTime" :mask="mask ? mask.replace(/\w/g, '#') : 'time'" dense>
    <template v-slot:append>
      <q-icon name="access_time" class="cursor-pointer">
        <q-popup-proxy cover transition-show="scale" transition-hide="scale">
          <q-time v-model="dateTime">
            <div class="row items-center justify-end">
              <q-btn v-close-popup label="Close" color="secondary" flat />
            </div>
          </q-time>
        </q-popup-proxy>
      </q-icon>
    </template>
  </q-input>
</template>

<script setup lang="ts">

import { computed } from "vue";

const props = defineProps<{
  inputDataProp?: any,
  disabled?: boolean,
  required?: boolean,
  mask?: string
}>();

const emit = defineEmits(['update:inputDataProp'])

const dateTime = computed({
  get() {
    return props.inputDataProp
  },
  set(value) {
    emit('update:inputDataProp', value)
  }
})
</script>
