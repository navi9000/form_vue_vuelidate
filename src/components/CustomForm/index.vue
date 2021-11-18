<template>
  <transition name="load">
    <form v-if="isLoaded" class="form" @submit.prevent="submit">
      <NameFieldset v-if="show('NameFieldset')" :screen="screenWidth" />
      <AddressFieldset v-if="show('AddressFieldset')" :screen="screenWidth" />
      <CredentialsFieldset v-if="show('CredentialsFieldset')" />
      <div class="button-div" v-if="show('CredentialsFieldset')">
        <input type="submit" class="button" />
      </div>
    </form>
  </transition>
</template>

<script>
import { useVuelidate } from "@vuelidate/core";
import NameFieldset from "./NameFieldset.vue";
import AddressFieldset from "./AddressFieldset.vue";
import CredentialsFieldset from "./CredentialsFieldset.vue";

export default {
  name: "CustomForm",
  components: {
    NameFieldset,
    AddressFieldset,
    CredentialsFieldset,
  },
  emits: {
    formSubmitted: null,
  },
  setup() {
    const v$ = useVuelidate({ $lazy: true });
    return { v$ };
  },
  data() {
    return {
      isLoaded: false,
      screenWidth: undefined,
      currElOnSmallScreen: "NameFieldset",
    };
  },
  methods: {
    toggleLoadedStatus() {
      this.isLoaded = !this.isLoaded;
    },
    handleScreenWidthChange() {
      this.screenWidth = window.screen.width;
    },
    show(element) {
      if (this.screenWidth >= 764 || this.currElOnSmallScreen === element) {
        return true;
      }
      return false;
    },
    async submit() {
      const isCorrectForm = await this.v$.$validate();
      this.v$.$touch();

      const formData = new FormData(document.querySelector(".form"));

      if (!isCorrectForm) return;

      this.$emit("formSubmitted", formData, true);
    },
  },
  mounted() {
    this.toggleLoadedStatus();
    this.handleScreenWidthChange();
    window.addEventListener("resize", this.handleScreenWidthChange);
  },
  unmounted() {
    window.removeEventListener("resize", this.handleScreenWidthChange);
  },
};
</script>

<style lang="scss">
.load-enter-active {
  opacity: 1;
  transition: opacity 0.5s ease-out;
}
.load-enter-from {
  opacity: 0;
}

.form {
  min-height: calc(100vh - 20px);
  max-width: 994px;
  margin: 10px auto;
  display: grid;
  gap: 10px;
  @media (max-width: 1024px) {
    max-width: 100%;
    margin: 10px;
  }
  @media (max-width: 767px) {
    display: block;
    gap: unset;
  }
}
.button-div {
  grid-column: 2 / 3;
  grid-row: 3 / 4;
}
</style>