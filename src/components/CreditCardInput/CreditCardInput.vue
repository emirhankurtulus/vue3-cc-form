<template>
  <div class="q-mb-md">
    <q-input
      class="col-xs-12 col-md-12"
      v-model="cardOwner"
      :dense="dense"
      maxlength="50"
      label="Card Owner"
      outlined
      clearable
    />

    <q-input
      class="col-xs-12 col-md-12"
      v-model="cardNumber"
      :rules="result?.isValid"
      :error="result?.isValid == false"
      :dense="dense"
      outlined
      label="Card Number"
      maxlength="19"
      clearable
      @update:model-value="cardValidation"
    >
      <template v-slot:prepend>
        <q-icon
          size="50px"
          v-if="result?.cardType != 'Unknown' && result?.cardType"
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
        :rules="[isDateValid]"
        label="Card Date"
        mask="##/##"
        outlined
        clearable
        @update:model-value="cardDateValidation"
      />

      <q-input
        class="col-xs-6 col-md-6"
        v-model.number="cardCvv"
        :maxlength="3"
        outlined
        label="Cvv"
        clearable
      />
    </div>
  </div>
</template>

<script setup>
import { ref, watch } from "vue";

import logos from "./images/logo";

const validate = require("cc-validate");
const cardNumber = ref(null);
const result = ref(null);
const isDateValid = ref(true);
const cardLogo = ref(null);
const cardCvv = ref(null);

function cardValidation() {
  result.value = validate.isValid(cardNumber.value);
  cardNumber.value = result.value.cardNumber;
}

function cardDateValidation(val) {
  const current = new Date();
  let date = `${current.getFullYear()}`;
  date = date.slice(2, 4);
  const cardVal = val.slice(3, 5);
  const month = val.slice(0, 2);

  if (cardVal < date || month > 12 || val < 5) {
    isDateValid.value = false;
  } else {
    isDateValid.value = true;
  }
}

function logoTrim() {
  cardLogo.value = result.value.cardType.replace(/\s/g, "");
}

watch(() => result.value, logoTrim);
</script>
