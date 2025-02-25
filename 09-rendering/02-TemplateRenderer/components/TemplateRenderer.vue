<script>
import { defineComponent, h } from 'vue';
import { compile } from 'vue';

export default {
  name: 'TemplateRenderer',

  props: {
    template: {
      type: String,
      required: true,
    },

    bindings: {
      type: Object,
      default: () => ({}),
    },

    components: {
      type: [Object, Array],
      default: () => [],
    },
  },

  computed: {
    renderFunction() {
      return compile(this.template);
    },

    componentFromTemplate() {
      return defineComponent({
        name: 'TemplateRendererInternal',
        props: ['bindings'],
        components: this.components,
        render: this.renderFunction,
      })
    }
  },

  render() {
    return h(this.componentFromTemplate, {bindings: this.bindings})
  }
};
</script>
