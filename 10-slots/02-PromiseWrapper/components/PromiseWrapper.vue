<template>
  <div v-if="promiseState === 'pending'">
    <slot name="pending"></slot>
  </div>
  <div v-if="promiseState === 'rejected'">
    <slot name="rejected" :error="promise.error"></slot>
  </div>
  <div v-if="promiseState === 'fulfilled'">
    <slot name="fulfilled" :result="promise.result"></slot>
  </div>
</template>

<script>
export default {
  name: 'PromiseWrapper',

  props: {
    promise: {
      type: Promise,
      required: true,
    },
  },

  computed: {
    promiseState() {
      return this.promise.state;
    }
  },

  mounted(){
    console.log(this.promise)
  },
};
</script>
