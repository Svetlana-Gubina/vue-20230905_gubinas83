<template>
  <router-link class="tabs__tab" exactActiveClass="tabs__tab_active" :to="to">
    <slot/>
  </router-link>
</template>

<script>
export default {
  name: 'UiTabsLink',

  props: {
    to: {
      type: String,
      required: true
    }
  }
};
</script>

<style scoped>
/* _tabs.css */
.tabs__tab {
  display: inline-flex;
  padding: 10px 0;
  font-weight: 400;
  font-size: 18px;
  text-decoration: none;
  line-height: 28px;
  color: var(--grey-8);
  border-bottom: 2px solid transparent;
  cursor: pointer;
  margin-right: 18px;
  transition-duration: 0.2s;
  transition-property: border-color, color;
  box-shadow: none;
  background-color: transparent;
  outline: none;
  position: relative;
  z-index: 1;
}

.tabs__tab:hover,
.tabs__tab.tabs__tab_active {
  border-bottom-color: var(--blue);
  color: var(--blue);
}
</style>
