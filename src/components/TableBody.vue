<template>
  <tbody>
    <tr v-for="item in data" :key="item.o_id">
      <TableBodyItem :data="item" />
    </tr>
  </tbody>
</template>

<script>
import TableBodyItem from './TableBodyItem.vue';
export default {
  props: ["data"],
  components: { TableBodyItem }
}

</script>

<style></style>