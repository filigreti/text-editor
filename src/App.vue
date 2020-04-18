<template>
  <div id="app">
    <TextEditor />
  </div>
</template>

<script>
import TextEditor from "@/components/TextEditor";
export default {
  name: "App",
  components: {
    TextEditor
  }
};
</script>

<style>
* {
  font-family: "Muli";
  font-size: 1rem;
}
</style>
