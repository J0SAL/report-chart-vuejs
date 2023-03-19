<template>
  <div class="hello" style="position: fixed; bottom: 0; width: 100%; margin-bottom: 10px;">
    Made by <a href="https://github.com/J0SAL" target="_blank" rel="noreferrer">Joy Almeida</a>
  </div>
</template>

<script>
export default {
  name: 'HelloWorld',
  props: {
    msg: String
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
