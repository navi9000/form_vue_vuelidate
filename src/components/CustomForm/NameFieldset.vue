<template>
  <fieldset class="name-fieldset">
    <Input
      :type="'text'"
      :name="'lastname'"
      :validations="{ required: 'Введите фамилию' }"
      >Фамилия</Input
    >
    <Input
      :type="'text'"
      :name="'firstname'"
      :validations="{ required: 'Введите имя' }"
      >Имя</Input
    >
    <Input :type="'text'" :name="'patronimic'">Отчество</Input>
    <Input
      :type="'date'"
      :name="'birthdate'"
      :validations="{
        required: 'Введите дату рождения',
      }"
      >Дата рождения</Input
    >
    <Input
      :type="'text'"
      :name="'phone'"
      :validations="{
        required: 'Введите номер телефона',
        phoneFormatValidator:
          'Номер должен состоять из 11 цифр, первая из которых является 7',
      }"
      >Номер телефона</Input
    >
    <Input :type="'radio'" :name="'gender'" :values="['Мужской', 'Женский']"
      >Пол</Input
    >
    <Input
      :type="'checkbox'"
      :name="'client_group'"
      :validations="{ required: 'Необходимо выбрать как минимум одну группу' }"
      :values="['VIP', 'Проблемные', 'ОМС']"
      >Группа клиентов</Input
    >
    <Input
      :type="'select'"
      :name="'doctor'"
      :values="['Иванов', 'Захаров', 'Чернышева']"
      >Лечащий врач</Input
    >
    <Input
      :type="'checkbox'"
      :name="'sms'"
      :values="['Не отправлять SMS']"
    ></Input>
    <div class="button-div" v-if="show()">
      <button type="button" class="button">Далее</button>
    </div>
  </fieldset>
</template>
<script>
import useVuelidate from "@vuelidate/core";
import Input from "./Input.vue";

export default {
  name: "NameFieldset",
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
.name-fieldset {
  grid-column: 1 / 2;
  grid-row: 1 / 4;
  @include fieldset;
}
</style>
