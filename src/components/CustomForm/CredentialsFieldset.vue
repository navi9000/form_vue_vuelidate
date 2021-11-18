<template>
  <fieldset class="credentials-fieldset">
    <Input
      :type="'select'"
      :name="'credentials_type'"
      :validations="{ required: 'Необходимо указать тип документа' }"
      :values="['Паспорт', 'Свидетельство о рождении', 'Вод. удостоверение']"
      >Тип документа</Input
    >
    <Input :type="'text'" :name="'number'">Номер</Input>
    <Input :type="'text'" :name="'region'">Область</Input>
    <Input :type="'text'" :name="'issuer'">Кем выдан</Input>
    <Input
      :type="'date'"
      :name="'issued_on'"
      :validations="{ required: 'Необходимо указать дату выдачи' }"
      >Дата выдачи</Input
    >
  </fieldset>
</template>
<script>
import useVuelidate from "@vuelidate/core";
import Input from "./Input.vue";

export default {
  name: "CredentialsFieldset",
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
.credentials-fieldset {
  grid-column: 2 / 3;
  grid-row: 2 / 3;
  @include fieldset;
}
</style>