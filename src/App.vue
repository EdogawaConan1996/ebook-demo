<template>
  <div id="app">
    <e-book />
  </div>
</template>

<script>


import EBook from "@/components/EBook";
const html = document.querySelector('html')
let fontSize = window.innerWidth / 10 > 50 ? 50 : window.innerWidth / 10
html.style.fontSize = `${fontSize}px`

export default {
  name: 'app',
  components: {
    EBook
  }
}
</script>

<style>
#app {}
</style>
