<template>
  <div class="popup">
    <h1>I'm a Popup</h1>
    <div>
      <p>Name: {{ name }}</p>
      <p>Age: {{ age }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'popup',
  props: {
    name: String,
    age: Number
  }
}
</script>

<style lang="scss">
.popup {
  position: fixed;
  bottom: 0;
  left: 0;
  width: 100%;
  min-height: 7rem;
  padding: 1rem;
  color: dodgerblue;
  text-align: center;
  background-color: #fff;
  box-shadow: 0 0 10px rgba(0, 0, 0, .25);
}
</style>
