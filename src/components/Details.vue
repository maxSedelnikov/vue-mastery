<template>
  <ul class="qualities">
    <li v-for="(detail, index) in details" :key="index">
      {{ detail }}
    </li>
  </ul>
</template>

<script>
export default {
  name: 'Details',
  props: {
    details: {
      type: Array,
      required: true,
    },
  },
};
</script>

<style lang="scss" scoped>
.qualities {
  max-width: 300px;
  width: 100%;
  padding: 10px;
  border: 1px dashed #ccc;
}
</style>
