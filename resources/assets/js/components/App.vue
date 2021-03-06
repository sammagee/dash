<template>
  <div id="app" class="h-full">
    <loading ref="loading" />

    <transition name="fade" mode="out-in">
      <component v-if="layout" :is="layout" />
    </transition>

    <flash data-message=""></flash>
  </div>
</template>

<script>
  import Loading from './Loading'

  // Load layout components dynamically.
  const requireContext = require.context('~/layouts', false, /.*\.vue$/)

  const layouts = requireContext.keys()
    .map(file =>
      [file.replace(/(^.\/)|(\.vue$)/g, ''), requireContext(file)]
    )
    .reduce((components, [name, component]) => {
      components[name] = component.default || component
      return components
    }, {})

  export default {
    el: '#app',

    components: {
      Loading
    },

    data: () => ({
      layout: null,
      defaultLayout: 'default'
    }),

    metaInfo () {
      const { appName } = window.config

      return {
        titleTemplate: (title) => {
          return title ? `${title} — ${appName}` : appName
        }
      }
    },

    mounted () {
      this.$loading = this.$refs.loading
    },

    methods: {
      /**
       * Set the application layout.
       * 
       * @param {String} layout
       */
      setLayout (layout) {
        if (!layout || !layouts[layout]) {
          layout = this.defaultLayout
        }

        this.layout = layouts[layout]
      }
    }
  }
</script>
