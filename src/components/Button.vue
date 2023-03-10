<script>
export default {
  props: {
    texto: String
  }
}
</script>

<template>
  <button>{{ texto }}</button>
</template>

<style scoped>
button {
  display: block;
  margin: 20px auto;
  height: 50px;
  width: 100%;
  max-width: 300px;
  border: none;
  border-radius: 10px;
  color: white;
  cursor: pointer;
}
</style>