<script>
<<<<<<< HEAD
import { defineComponent, h } from 'vue';
import { compile } from 'vue';
=======
import { compile, defineComponent, h } from 'vue';
>>>>>>> 021fe5c96b4a1e220f56b2fac41dddf94487796b

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
        components: this.components,
        props: ['bindings'],
        render: this.renderFunction,
      });
    },
  },

  render() {
    return h(this.componentFromTemplate, { bindings: this.bindings });
  },
};
</script>
