<template>
  <label for="" class="grid">
    <q-file v-model="value" outlined :dense="dense" use-chips append :multiple="!multiple"
      class="h-fit" :accept="acceptFile" @rejected="rejected" :rules="rules" max-files="20" />
    <figure v-if="!value || value.length === 0" class="flex tw-items-center tw-justify-center !tw-text-sm">
      <div v-if="!dense">
        <q-icon name="add_to_drive" color="grey-6" size="1.5rem"></q-icon>
        <b class="block">Upload files here</b>
      </div>
      <div v-else>
        <b class="block">Upload files here</b>
      </div>
    </figure>
  </label>
</template>

<script lang="ts" setup>
import { computed, ref } from 'vue'

const props = defineProps<{
  dense?: boolean,
  multiple?: boolean,
  modelValue: any,
  acceptFile?: string,
  rules?: any[]
}>()

const emit = defineEmits(['update:modelValue', 'rejected'])

const value = computed({
  get() {
    return props.modelValue
  },
  set(val) {
    emit('update:modelValue', val)
  }
})

const rejected = (reject) => {
  emit('rejected', reject)
}
</script>

<style scoped>
.grid {
  display: grid;
  height: 160px;
  /* place-content: center; */
}

.grid>* {
  grid-area: 1/1;

}

.grid>figure {
  text-align: center;
}

.q-field__control {
  height: 100% !important;
}

.h-full {
  height: 100%;
}
</style>

<style >
.grid .q-field--auto-height .q-field__control {
  height: 100% !important;
}
</style>
