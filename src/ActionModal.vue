<style scoped src="./ModalCrearEditar.css"></style>

<template>
  <q-dialog v-model="isOpen" persistent>
    <q-card class="custom-modal">

      <q-form @submit.prevent="submit">
        <div class="acordion-style">
          <h2 class="modal-title semibold">{{ isEditing ? 'Editar ' + formLabel : 'Crear ' + formLabel }}</h2>
          <div class="row">

            <div v-for="(field, fIndex) in formInputs" :class="[
              `col-${12 / field.col}`,
              field.label === '' ? 'q-mt-md' : '',
              field.type === 'textarea' ? 'min-h-180 marginb-3' : '' // Agregar q-mb-md solo si el label está vacío
            ]" :key="fIndex">
              <label v-if="field.type !== 'checkbox'" for="description" class="custom-label">{{ field.label
              }}</label>
              <q-input v-if="field.type === 'text'" outlined :label="field.placeholder" class="custom-field"
                :required="field.required" :error-icon="field.required ? 'error' : ''" v-model="formData[field.nameField]"
                :rules="field.rules" :readonly="field.disabled" dense></q-input>

              <q-input v-if="field.type === 'number'" outlined :label="field.placeholder" type="number"
                class="custom-field" :required="field.required" :error-icon="field.required ? 'error' : ''"
                v-model="formData[field.nameField]" :dense="true" :rules="field.rules"></q-input>

              <q-input v-if="field.type === 'textarea'" outlined type="textarea" class="custom-field"
                :label="field.placeholder" :required="field.required" v-model="formData[field.nameField]"
                :rules="field.rules" dense></q-input>

              <q-select v-if="field.type === 'select'" outlined :placeholder="field.placeholder" :options="field.options"
                class="custom-field" :required="field.required" :error-icon="field.required ? 'error' : ''"
                v-model="formData[field.nameField]" :rules="field.disabled ? [] : field.rules" :readonly="field.disabled"
                dense></q-select>

              <q-select v-if="field.type === 'search'" v-model="formData[field.nameField]" :options="options(field)"
                use-input :required="field.required" :error-icon="field.required ? 'error' : ''"
                :rules="field.disabled ? [] : field.rules"
                @filter="(value, update) => handleFilterOptions(field, value, update)" :option-label="field.placeholder"
                :option-value="formData[field.label]" :disabled="field.disabled" class="custom-field" outlined dense>
                <template v-slot:no-option>
                  <q-item>
                    <q-item-section class="text-grey">
                      No results
                    </q-item-section>
                  </q-item>
                </template>
              </q-select>

              <q-toggle v-if="field.type === 'toggle'" v-model="formData[field.nameField]" :required="field.required"
                :rules="field.rules" :label="formData[field.nameField] ? 'Activo' : 'Inactivo'" color="secondary"
                class="!items-start" />
              <FileLoader v-if="field.type === 'file'" class="full-width" v-model="formData[field.nameField]"
                :accept-file="field.acceptFile" />

              <q-option-group v-if="field.type === 'checkbox' || field.type === 'radio'" :options="field.options"
                :type="field.type" :required="field.required" v-model:model-value="formData[field.nameField]"
                :rules="field.rules" inline />

              <DatePickerComponent v-if="field.type === 'date'" :mask="field?.mask ?? 'YYYY/MM/DD'" class="custom-field"
                :required="field.required" :error-icon="field.required ? 'error' : ''" :label="field.placeholder"
                v-model:input-data-prop="formData[field.nameField]" :rules="field.rules" />

              <TimePickerComponent v-if="field.type === 'time'" :mask="field?.mask ?? 'HH:mm'" class="custom-field"
                :required="field.required" :error-icon="field.required ? 'error' : ''"
                v-model:input-data-prop="formData[field.nameField]" :rules="field.rules" />
              <!-- Add more field types as needed -->
            </div>
          </div>
        </div>
        <div class="row justify-center q-mt-md">
          <q-btn label="Cancelar" color="accent" textColor="secondary"
            class="rounded-lg p-2 w-200px custom-field" @click="cancel"></q-btn>
          <q-btn :label="buttonLabel" :color="buttonEnabled ? 'gray-5' : 'secondary'"
            :class="buttonEnabled ? 'rounded-lg w-200px bold custom-field disabled-button' : 'rounded-lg p-2 w-200px bold custom-field'"
            type="submit" :disable="buttonEnabled"></q-btn>
        </div>
      </q-form>

    </q-card>
  </q-dialog>
</template>
<script lang="ts">
interface FormProps {
  type: "text" | "number" | "textarea" | "date" | "toggle" | "select" | "time" | "checkbox" | "file" | "search" | "radio",
  col: number,
  label: string,
  placeholder: string,
  required: boolean,
  nameField: string,
  rules: any[],
  disabled: boolean
  acceptFile?: string;
  options?: Array<{ label: string; value: any; }>;
  mask?: string;
  value?: any
}
import { defineComponent } from 'vue';
import DatePickerComponent from './components/DatePicker.vue';
import TimePickerComponent from './components/TimePicker.vue';
import FileLoader from './components/FileLoader.vue';

export default defineComponent({
  name: 'ActionModal',
  props: {
    isOpen: Boolean,
    isEditing: Boolean,
    formInputs: Array<FormProps | any>,
    formData: { type: Object, required: true },
    buttonLabel: String,
    buttonEnabled: Boolean,
    formLabel: String,
  },
  emits: ['cancel', 'submit'],
  setup(props, { emit }) {
    const cancel = () => {
      emit('cancel');
    };
    const submit = () => {
      emit('submit', props.formData);
    };
    const options = (formInput) => {
      if (formInput.filterValue === '')
        return formInput.options;
      return formInput.options.filter(option => option.toLowerCase().indexOf(formInput?.filterValue?.toLowerCase()) > -1);
    };
    const handleFilterOptions = (formInput, value, update) => update(() => { formInput.filterValue = value; });
    const handleFileChange = (value, field) => {
      props.formData[field.nameField] = value.target.files[0];
    };
    return {
      cancel,
      submit,
      options,
      handleFilterOptions,
      handleFileChange
    };
  },
  components: { FileLoader, DatePickerComponent, TimePickerComponent }
});
</script>
