<template>
  <main>
    <div :style="`background:${color}`" class="button">
      <div>Background</div>
      <input type="color" v-model="color" />
    </div>
  </main>
</template>

<script>
export default {
  data() {
    return {
      color: "#ffffff"
    };
  },
  watch: {
    color(x) {
      this.$emit("changeBackground", this.color);
    }
  }
};
</script>

<style>
.button {
  height: 2.5rem;
  width: 10rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  color: grey;
  padding: 0 5px;
  border: 1px solid grey;
}
</style>