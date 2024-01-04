<template>
  <shopHeader></shopHeader>
  <div class="main__main"><shopPage></shopPage></div>
  <shopFooter></shopFooter>
</template>

<script>
import shopPage from './components/shopPage.vue'
import shopHeader from './components/shopHeader.vue'
import shopFooter from './components/shopFooter.vue'

export default {
  name: 'App',
  components: {
    shopPage,
    shopHeader,
    shopFooter
  }
}
</script>

<style>
html,body{
  margin: 0;
}

.main__main{
  background-color: rgb(66, 66, 66);
}
</style>
