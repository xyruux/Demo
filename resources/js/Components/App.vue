<template>
  <v-app  >
    <loading ref="loading" /> 
   
    <v-main>
      <component :is="layout" v-if="layout" />
    </v-main>

    <ft />
  </v-app>
</template>

<script>
import ft from "../pages/projectC/Components/foot.vue";
import Loading from "./Loading";
import { mapGetters } from "vuex";
// Load layout components dynamically.
const requireContext = require.context("~/layouts", false, /.*\.vue$/);

const layouts = requireContext
  .keys()
  .map((file) => [file.replace(/(^.\/)|(\.vue$)/g, ""), requireContext(file)])
  .reduce((components, [name, component]) => {
    components[name] = component.default || component;
    return components;
  }, {});

export default {
  el: "#app",
  components: {
    Loading,
    ft,
  },

  data: () => ({
    layout: null,
    defaultLayout: "default",
  }),
 
  computed: mapGetters({
    user: "auth/user",
  }),
  metaInfo() {
    const { appName } = window.config;

    return {
      title: appName,
      titleTemplate: `%s · ${appName}`,
    };
  },

  mounted() {
    this.$loading = this.$refs.loading;
  },

  methods: {
    /**
     * Set the application layout.
     *
     * @param {String} layout
     */
    setLayout(layout) {
      if (!layout || !layouts[layout]) {
        layout = this.defaultLayout;
      }

      this.layout = layouts[layout];
    },
  },
};
</script>
