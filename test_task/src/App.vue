<template>
  <div id="app">
    <!-- <router-view name="tabsBar"/> -->
    <tabs-bar/>
  </div>
</template>

<script>
import TabsBar from "./components/TabsBar.vue"

export default {
  components: {
    TabsBar,
  }    
}
</script>

<style lang="less">
  body {
    background-color:  #f6f6f6;
  }
</style>
