<template>
<home></home>
</template>

<script>
import home from './components/home.vue';
export default {
  name: 'App',
  components: {
    home
  },
}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Anonymous+Pro&display=swap');

*{
  padding: 0;
  margin: 0;
  font-family: 'Anonymous Pro', monospace;
}
body{
  overflow: hidden;
}
</style>
