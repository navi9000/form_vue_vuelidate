<template>
  <div :class="{ 'start-page': true, 'start-page__leaving': isLeaving }">
    <div class="feedback" v-if="success">Клиент успешно создан</div>
    <button @click="createNewClient" class="button button__start-page">
      Новый клиент
    </button>
  </div>
</template>

<script>
export default {
  name: "StartPage",
  props: ["success"],
  data() {
    return {
      isLeaving: false,
    };
  },
  methods: {
    createNewClient() {
      this.isLeaving = true;
      this.$emit("newClient");
    },
  },
  emits: {
    newClient: null,
  },
};
</script>

<style lang="scss">
@import "../../styles/variables";
.button__start-page {
  margin-top: 50vh !important;
}

.start-page {
  opacity: 1;
  transition: all 1s ease-in;

  &__leaving {
    opacity: 0;
  }
}

.feedback {
  position: fixed;
  top: 150px;
  right: 50%;
  transform: translateX(50%);
  padding: 20px 100px;
  border: 2px solid $primaryColor;
  border-radius: 10px;
}
</style>