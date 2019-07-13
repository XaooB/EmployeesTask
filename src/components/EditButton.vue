<template>
  <button v-on:click="editEmployee">Edytuj</button>
</template>
<script>
export default {
  props: {
    id: Number
  },
  methods: {
    editEmployee() {
      console.log("Clicked: ", this.id);
    }
  }
};
</script>

