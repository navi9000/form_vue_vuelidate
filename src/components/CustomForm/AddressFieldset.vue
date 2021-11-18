<template>
  <fieldset class="address-fieldset">
    <Input :type="'text'" :name="'zip'">Индекс</Input>
    <Input :type="'text'" :name="'district'">Область</Input>
    <Input
      :type="'text'"
      :name="'town'"
      :validations="{ required: 'Введите город' }"
      >Город</Input
    >
    <Input :type="'text'" :name="'street'">Улица</Input>
    <Input :type="'text'" :name="'house'">Дом</Input>
    <div class="button-div" v-if="show()">
      <button type="button" class="button">Далее</button>
    </div>
  </fieldset>
</template>
<script>
import useVuelidate from "@vuelidate/core";
import Input from "./Input.vue";

export default {
  name: "AddressFieldset",
  props: ["screen"],
  components: {
    Input,
  },
  methods: {
    show() {
      return this.$props.screen <= 767;
    },
  },
  setup() {
    return { v$: useVuelidate() };
  },
};
</script>

<style lang="scss">
@import "../../styles/variables";
.address-fieldset {
  grid-column: 2 / 3;
  grid-row: 1 / 2;
  @include fieldset;
}
</style>