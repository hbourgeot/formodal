<template>
  <q-input outlined v-model="dateData" :mask="mask ? mask.replace(/\w/g, '#') : 'date'" class="col-6" :label="label"
    :disable="disabled" :dense="true">
    <template v-slot:append>
      <q-icon name="event" class="cursor-pointer">
        <q-popup-proxy cover transition-show="scale" transition-hide="scale">
          <q-date v-model="dateData" :options="options" :mask="mask">
            <div class="row items-center justify-end">
              <q-btn v-close-popup label="Close" color="secondary" flat></q-btn>
            </div>
          </q-date>
        </q-popup-proxy>
      </q-icon>
    </template>
  </q-input>
</template>

<script setup lang="ts">

import { computed } from 'vue';
import moment from "moment";

const emit = defineEmits(['update:inputDataProp'])

const props = withDefaults(defineProps<{
  inputDataProp?: string | Date | undefined,
  disabled?: boolean,
  required?: boolean,
  options?: any,
  label?: string,
  mask?: string
}>(), {
  mask: ""
  // target: ""
})


const dateData = computed({
  get() {
    return props.inputDataProp
  },
  set(value) {
    emit('update:inputDataProp', value)
  }
})
</script>
