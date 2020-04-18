<template>
  <main>
    <div class="button">
      <div>Font-size</div>
      <select v-model="num" name id>
        <option v-for="num in numbers" :key="num" :value="num">{{num}}</option>
      </select>
    </div>
  </main>
</template>

<script>
export default {
  data() {
    return {
      num: 16
    };
  },
  watch: {
    num(x) {
      this.$emit("changefont", this.num);
    }
  },
  computed: {
    numbers() {
      var numberArray = [];
      for (var i = 16; i <= 50; i++) {
        numberArray.push(i);
      }
      return numberArray;
    }
  }
};
</script>

<style>
</style>