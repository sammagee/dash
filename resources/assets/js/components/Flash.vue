<template>
  <transition name="slide-up" mode="out-in" appear>
    <div
      class="bg-grey-darkest fixed flex mb-4 ml-4 pin-b pin-l rounded shadow z-50"
      role="alert"
      v-show="show">
      <div class="flex h12 items-start justify-between overflow-hidden p-4 text-sm text-white">
        <div
          v-html="message"
          :title="message"></div>

        <button class="text-grey hover:text-grey-dark ml-6" @click="close">
          <svg class="fill-current transition-all w-4 h-4" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg">
            <path d="M19 6.41L17.59 5 12 10.59 6.41 5 5 6.41 10.59 12 5 17.59 6.41 19 12 13.41 17.59 19 19 17.59 13.41 12z"/>
            <path d="M0 0h24v24H0z" fill="none"/>
          </svg>
        </button>
      </div>
    </div>
  </transition>
</template>

<script>
  export default {
    name: 'flash',

    props: {
      dataMessage: {
        type: String
      }
    },

    data: () => ({
      message: this.dataMessage,
      show: false,
      type: 'info'
    }),

    created() {
      if (this.dataMessage) {
        this.flash()
      }

      window.events.$on(
        'flash', data => this.flash(data)
      )
    },

    methods: {
      flash (data) {
        if (data) {
          this.message = data.message;
          this.type = data.type;
        }

        this.show = true;
        this.hide();
      },

      hide () {
        setTimeout(() => {
          this.show = false;
        }, 5000);
      },

      close () {
        this.show = false;
      }
    }
  };
</script>
