<template>
  <div class="row justify-center full-height full-width text-center">
    <credit-card-input
      v-model="cardNumber"
      v-model:cardOwner="cardOwner"
      v-model:cardDate="cardDate"
      v-model:cardCvv="cardCvv"
      v-model:result="result"
    />

    <credit-card-input
      v-model="cardNumber"
      v-model:cardOwner="cardOwner"
      v-model:cardDate="cardDate"
      v-model:cardCvv="cardCvv"
      v-model:result="result"
      showLogo="false"
      cardOwnerMaxLenght="20"
      cardCvvMaxLenght="3"
      cardOwnerLabel="Card Owner Name"
      cardNumberLabel="Credit Card Number"
      cardDateLabel="Credit Card Expiration Date"
      cardCvvLabel="Card Verification Value"
    />
  </div>
</template>

<script setup>
import { ref } from "vue";
import CreditCardInput from "./components/CreditCardInput";

const cardNumber = ref(null);
const cardOwner = ref(null);
const cardDate = ref(null);
const cardCvv = ref(null);
const result = ref(null);
</script>
