<template>
  <div class="app">
    <ComponentHeader />
    <ComponentMain />
  </div>
</template>

<script>
import ComponentHeader from "./components/ComponentHeader.vue";
import ComponentMain from "./components/ComponentMain.vue";



export default {
  name: "App",
  components: {
    ComponentHeader,
    ComponentMain
  }
}

</script>

<style>
/* The reset css code below */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;

  font-family: 'Fira Sans', sans-serif;
}

a {
  text-decoration: none;
}
</style>
