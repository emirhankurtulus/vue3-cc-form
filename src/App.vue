<template>
<card-form></card-form>
</template>

<script>
import { ref } from 'vue'
import CardForm from './components/CardForm.vue'

export default {
  name: 'LayoutDefault',

  components: {
    CardForm
  },

  setup () {
    return {
      leftDrawerOpen: ref(false)
    }
  }
}
</script>
