<template>
  <div class="pagination-container">
    <!-- Контент страницы -->
     <template v-for="item in itemsToRender">
      <slot name="default" :item="item" />
     </template>
  </div>
</template>

<script>
export default {
  name: 'UiPaginationView',

  props: {
    page: {
      type: Number,
      required: true,
      validator: (value) => value > 0,
    },

    perPage: {
      type: Number,
      required: true,
      validator: (value) => value > 0,
    },

    items: {
      type: Array,
      required: true,
    },
  },

  computed: {
    itemsToRender() {
      const end = this.perPage * this.page;
      const start = end - this.perPage;

      return this.items.slice(start, end);
    }
  },
};
</script>

<style></style>
