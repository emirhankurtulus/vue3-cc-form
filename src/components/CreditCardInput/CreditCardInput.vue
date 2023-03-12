<template>
  <div class="q-mb-md">
    <q-input
      class="col-xs-12 col-md-12"
      v-model="cardOwner"
      :rules="[(val) => !!val]"
      :maxlength="props.cardOwnerMaxLenght"
      :label="props.cardOwnerLabel"
      outlined
      clearable
    />

    <q-input
      class="col-xs-12 col-md-12"
      v-model="cardNumber"
      :rules="[(val) => !!val && result?.isValid]"
      :error="result?.isValid == false"
      :label="props.cardNumberLabel"
      :maxlength="props.cardNumberMaxLength"
      outlined
      clearable
      @update:model-value="cardValidation"
    >
      <template v-slot:prepend>
        <q-icon
          size="50px"
          v-if="
            result?.cardType != 'Unknown' && result?.cardType && props.showLogo
          "
        >
          <img alt="cardLogo" :src="logos[cardLogo]" />
        </q-icon>
      </template>
    </q-input>

    <div class="row">
      <q-input
        class="col-xs-6 col-md-6"
        v-model="cardDate"
        :error="!isDateValid"
        :rules="[(val) => !!val && isDateValid]"
        :label="props.cardDateLabel"
        mask="##/##"
        outlined
        clearable
        @update:model-value="cardDateValidation"
      />

      <q-input
        class="col-xs-6 col-md-6"
        v-model.number="cardCvv"
        :maxlength="props.cardCvvMaxLenght"
        type="number"
        :rules="[(val) => !!val && val >= 0]"
        oninput="javascript: if (this.value.length > this.maxLength ) this.value = this.value.slice(0, this.maxLength);"
        outlined
        :label="props.cardCvvLabel"
        clearable
      />
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";

import logos from "./images/logo";

const props = defineProps({
  showLogo: {
    type: Boolean,
    default: true, //Card Logo will have appeared.
  },
  cardOwnerMaxLenght: {
    type: Number,
    default: 50,
  },
  cardNumberMaxLength: {
    type: Number,
    default: 19,
  },
  cardCvvMaxLenght: {
    type: Number,
    default: 4,
  },
  cardOwnerLabel: {
    type: String,
    default: "Name and Surname",
  },
  cardNumberLabel: {
    type: String,
    default: "Card Number",
  },
  cardDateLabel: {
    type: String,
    default: "Expiration Date",
  },
  cardCvvLabel: {
    type: String,
    default: "CVV",
  },
});

const emit = defineEmits([
  "update:model-value",
  "cardOwner",
  "cardNumber",
  "cardDate",
  "cardCvv",
  "isValid",
  "message",
  "cardType",
  "result",
]);

const validate = require("cc-validate");

const cardNumber = ref(null);

const result = ref(null);

const isDateValid = ref(true);

const cardLogo = ref(null);

const cardCvv = ref(null);

const cardOwner = ref(null);

const cardDate = ref(null);

function cardValidation(val) {
  result.value = validate.isValid(val);
  cardNumber.value = result.value.cardNumber;
}

function cardDateValidation(val) {
  const current = new Date();

  let currentDate = `${current.getFullYear()}`;

  let currentMonth = current.getMonth();

  const currentYear = currentDate.slice(2, 4);

  const creditCardYear = val.slice(3, 5);

  const creditCardMonth = val.slice(0, 2);
  if (val.length == 5) {
    if (creditCardYear < currentYear || creditCardMonth > 12) {
      isDateValid.value = false;
    } else {
      if (creditCardYear == currentYear && creditCardMonth < currentMonth + 1) {
        isDateValid.value = false;
      } else {
        isDateValid.value = true;
      }
    }
  } else {
    isDateValid.value = false;
  }
}

function logoTrim() {
  cardLogo.value = result.value.cardType.replace(/\s/g, "");
}

function emits() {
  emit("update:model-value", cardNumber.value);
  emit("update:cardOwner", cardOwner.value);
  emit("update:cardDate", cardDate.value);
  emit("update:cardCvv", cardCvv.value);
  emit("update:cardType", result.value.cardType);
  emit("update:result", result.value);
  emit("update:isvalid", result.value.isValid);
  emit("update:message", result.value.message);
}

watch(() => result.value, logoTrim);
watch(() => cardOwner.value, emits);
watch(() => cardDate.value, emits);
watch(() => cardCvv.value, emits);
watch(() => cardNumber.value, emits);
</script>
