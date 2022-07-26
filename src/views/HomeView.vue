<template>
  <ListEmployee />
</template>

<script>
// @ is an alias to /src
import ListEmployee from '@/components/ListEmployee.vue'

export default {
  name: 'HomeView',
  components: {
    ListEmployee
  }
}
</script>
