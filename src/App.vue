<template>
<credit-card-input></credit-card-input>
</template>

<script>
import { ref } from 'vue'
import CreditCardInput from './components/CreditCardInput'

export default {
  name: 'LayoutDefault',

  components: {
    CreditCardInput
  },

  setup () {
    return {
      leftDrawerOpen: ref(false)
    }
  }
}
</script>
