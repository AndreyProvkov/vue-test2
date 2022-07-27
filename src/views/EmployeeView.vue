<template>
  <ItemEmployee :item='$route.query.item' />
</template>

<script>
import ItemEmployee from '@/components/ItemEmployee.vue'

export default {
  name: 'EmployeeView',
  components: {
    ItemEmployee
  }
}
</script>