<template>
  <div class="field">
    <label
      :for="$props.name"
      :class="{ field__label: true, field__label_required: $props.validations }"
    >
      <slot></slot>
    </label>

    <div>
      <!-- type: radio or checkbox -->
      <div v-if="$props.type === 'radio' || $props.type === 'checkbox'">
        <div
          class="field__radio-container"
          v-for="key of $props.values"
          :key="key"
        >
          <input
            :type="$props.type"
            :name="$props.name"
            :id="key"
            :value="key"
            :class="{
              field__radio: $props.type === 'radio',
              field__checkbox: $props.type === 'checkbox',
              'field__border-error': showError($props.name),
            }"
            @change="getChecked"
            v-model="$data[key]"
          />
          <label :for="key">{{ " " + key }}</label>
        </div>
      </div>
      <!-- type: options -->
      <select
        v-else-if="$props.type === 'select'"
        :name="$props.name"
        :id="$props.name"
        :class="{
          field__select: true,
          field__select_single: !$props.multiple,
          field__select_multiple: $props.multiple,
          'field__border-error': showError($props.name),
        }"
        v-model="$data[$props.name]"
        :multiple="$props.multiple"
      >
        <option v-if="!$props.multiple" disabled value="">Выбрать</option>
        <option
          v-for="(selection, index) in $props.values"
          :value="$props.values[index]"
          :key="selection"
        >
          {{ selection }}
        </option>
      </select>

      <!-- type: text or date  -->
      <div v-else>
        <input
          :type="$props.type"
          :id="$props.name"
          :name="$props.name"
          :class="{
            'field__text-input': true,
            'field__border-error': showError($props.name),
          }"
          v-model.trim="$data[$props.name]"
        />
      </div>

      <!-- error message -->
      <div v-if="showError($props.name)" class="field__error-message">
        {{ getErrorMessage }}
      </div>
    </div>
  </div>
</template>

<script>
import useVuelidate from "@vuelidate/core";
import * as validators from "@vuelidate/validators";

const customValidators = {
  ...validators,
  phoneFormatValidator: { $validator: validators.helpers.regex(/^7\d{10}$/) },
};

export default {
  name: "Input",
  props: ["name", "validations", "type", "values", "multiple"],
  setup() {
    return { v$: useVuelidate() };
  },
  methods: {
    showError(element) {
      if (this.$props.validations) {
        return this.v$.$dirty && this.v$[element].$invalid;
      }
      return false;
    },
    getChecked() {
      if (this.$props.type === "checkbox") {
        const filtered = this.$props.values.filter((el) => this.$data[el]);
        this.$data[this.$props.name] = filtered;
      }
    },
    initialArrayValues() {
      let checkboxValues = {};
      this.$props.values.forEach((value) => {
        checkboxValues = { ...checkboxValues, [value]: false };
      });
      return checkboxValues;
    },
  },
  computed: {
    getErrorMessage() {
      let messages = Object.keys(this.$props.validations)
        .map(
          (el) =>
            this.v$[this.$props.name][el].$invalid &&
            this.v$[this.$props.name][el].$message
        )
        .filter((el) => el !== false);
      return messages[0];
    },
  },
  data() {
    if (this.$props.type === "checkbox") {
      return {
        ...this.initialArrayValues(),
        [this.$props.name]: [],
      };
    }
    return {
      [this.$props.name]: "",
    };
  },
  validations() {
    if (this.$props.validations) {
      return {
        [this.$props.name]: Object.assign(
          ...[...Object.keys(this.$props.validations)].map((key, index) => {
            return {
              [key]: validators.helpers.withMessage(
                Object.values(this.$props.validations)[index],
                customValidators[key]
              ),
            };
          })
        ),
      };
    }
  },
};
</script>

<style lang="scss">
@import "../../styles/variables";
.field {
  display: grid;
  grid-template-columns: 156px auto;
  min-height: 40px;

  &__label {
    line-height: 40px;
    &_required {
      position: relative;
      &::after {
        content: "*";
        position: absolute;
        top: -2px;
      }
    }
  }

  &__text-input {
    width: 100%;
    height: 20px;
    padding: 0 10px;
    box-sizing: border-box;
    margin-top: 10px;
    border-radius: 10px;
    border: 1px solid #000000;
  }
  &__radio-container {
    margin: 1px 0;
  }
  &__radio {
    cursor: pointer;
    transform: translateY(1px);
  }
  &__select {
    width: 100%;
    border-radius: 10px;
    overflow: hidden;
    outline: none;
    appearance: none;
    border: 1px solid #000000;
    & option {
      cursor: pointer;
    }

    &_multiple {
      padding: 5px 0;
      box-sizing: border-box;
      height: 62px;
      margin: 10px 0;
      & option {
        padding: 0 10px;
        box-sizing: border-box;
      }
    }
    &_single {
      height: 20px;
      padding: 0 10px;
      box-sizing: border-box;
      margin-top: 10px;
      cursor: pointer;
    }
  }
  &__checkbox {
    margin-top: 12px;
    cursor: pointer;
  }

  &__border-error {
    border-color: $errorColor;
  }

  &__error-message {
    color: $errorColor;
    margin: 0 10px;
    font-size: small;
  }
}
</style>