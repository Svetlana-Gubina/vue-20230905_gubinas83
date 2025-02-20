<template>
  <UiInput :type="type" :step="step" v-model="modelValueProxy">
    <template v-for="slotName in Object.keys($slots)" #[slotName]>
      <slot :name="slotName"/>
    </template>
  </UiInput>
</template>

<script>
import UiInput from './UiInput.vue';

export default {
  name: 'UiInputDate',

  props: {
    type: {
      type: String,
      default: "date"
    },
    modelValue: {
      type: Number,
      default: 0
    },
    step: {
      type: Number,
      default: 0
    },
  },

  computed: {
    modelValueProxy: {
      get() {
        return this.modelValue.toString();
      },
      set(value) {
        this.$emit('update:modelValue', new Date(value))
      },
    },
  },

  emits: ['update:modelValue'],

  components: { UiInput },
};
</script>
