<template>
  <div class="q-pa-md">
    <q-card class="my-card" style="max-width: 1000px">
      <q-input outlined v-model="cardOwner" :dense="dense" label="Card Owner" />
      <q-input outlined v-model="cardNumber" :dense="dense" label="Card Number" @update:model-value="cardValidation" />
      <div class="row">
        <q-input outlined v-model="cardDate" label="Card Date" mask="##/##" :rules="[isDateValid]"
          @update:model-value="cardDateValidation" />
        <q-input outlined v-model.number="cardCvv" label="Cvv" />
      </div>
    </q-card>
  </div>
  <p>CardDate:{{ isDateValid }}</p>
</template>

<script setup>
import { ref } from "vue";
const validate = require("cc-validate");

const cardNumber = ref(null);
const result = ref(null);
const cardOwner = ref(null);
const cardCvv = ref(null);
const date = ref(null)
const isDateValid = ref(null)
const cardVal = ref(null)
const cardDate = ref(null)

function cardValidation() {
  result.value = validate.isValid(cardNumber.value);
  cardNumber.value = result.value.cardNumber;
}

function cardDateValidation(val) {
  const current = new Date();
  date.value = `${current.getFullYear()}`;
  date.value = date.value.slice(2, 4);
  cardVal.value = val.slice(3, 5);
  if((cardVal.value<date.value)&&cardDate.value.length==5){
    isDateValid.value= false
  }
}

</script>

<style lang="scss">

</style>
