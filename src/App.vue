<template>
  <div>
    <component
      :is="currentPage"
      @newClient="toggleIsStartPage"
      @formSubmitted="toggleIsStartPage"
      :success="showSuccessFeedback"
    />
  </div>
</template>

<script>
import { defineAsyncComponent } from "vue";
import StartPage from "./components/StartPage";
export default {
  name: "App",
  components: {
    StartPage,
    CustomForm: defineAsyncComponent(() => import("./components/CustomForm")),
  },
  data() {
    return {
      isStartPage: true,
      showSuccessFeedback: false,
    };
  },
  computed: {
    currentPage() {
      return this.isStartPage ? "StartPage" : "CustomForm";
    },
  },
  methods: {
    toggleIsStartPage(data = null, success = false) {
      if (data) {
        for (let entry of data.entries()) {
          console.log(entry[0], ": ", entry[1]);
        }
      }
      this.showSuccessFeedback = success;
      setTimeout(
        function () {
          this.isStartPage = !this.isStartPage;
        }.bind(this),
        1000
      );
    },
  },
};
</script>

<style lang="scss">
@import "./styles/variables";

* {
  margin: 0;
  padding: 0;
  font-family: sans-serif;
}

body {
  background-color: $secondaryColor;
}

.button {
  border: none;
  background-color: $primaryColor;
  border-radius: 5px;
  cursor: pointer;
  font-weight: bolder;
  box-sizing: border-box;
  padding: 5px 10px;
  color: #ffffff;
  margin: 0 auto;
  display: block;
  font-size: 1.5em;
  transition: all ease-out 0.2s;

  &:hover {
    box-shadow: 2px 2px 7px black;
  }
  &:active {
    transform: translate(2px, 2px);
    box-shadow: none;
  }
}
</style>