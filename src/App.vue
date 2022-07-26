<template>
  <v-app>
    <v-main>
      <ListEmployee />
    </v-main>
  </v-app>
</template>

<script>
import ListEmployee from "@/components/ListEmployee.vue"

export default {
  name: 'App',
  components: {
    ListEmployee
  },
  data: () => ({
    //
  }),
};
</script>
