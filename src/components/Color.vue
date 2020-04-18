<template>
  <main>
    <div style="background:grey;" class="button">
      <div :style="`color:${color} !important`">Color</div>
      <input type="color" v-model="color" />
    </div>
  </main>
</template>

<script>
export default {
  data() {
    return {
      color: "#000000"
    };
  },
  watch: {
    color(x) {
      this.$emit("changeColor", x);
    }
  }
};
</script>

<style>
</style>