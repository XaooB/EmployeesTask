<template>
  <button v-if="status" v-on:click="update">{{updating ? 'Zapisywanie..' : 'Zapisz zmiany'}}</button>
  <button v-else v-on:click="toggleEdit">Edytuj</button>
</template>
<script>
export default {
  props: {
    toggleEdit: Function,
    update: Function,
    status: Boolean,
    updating: Boolean
  }
};
</script>
<style lang="scss" scoped>
button {
  padding: 8px 10px;
  border-radius: 12px;
  background: none;
  border: 1px solid #ee324e;
  color: #ee324e;
  outline: none;
  white-space: nowrap;
}
</style>
