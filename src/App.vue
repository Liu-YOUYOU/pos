<template>
  <div id="app">
    <leftnav></leftnav>
    <div class="main">
      <router-view/>
    </div>
  </div>
</template>

<script>
  import leftnav from '@/components/common/leftnav'
export default {
  name: 'app',
  components: {
    leftnav
  }
}
</script>

<style>
  #app {
    font-family: '微软雅黑','Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
  }
  .main{
    float: left;
    width: 95%;
    background: #EFF2F7;
    height: 100%;
    overflow: hidden;
  }
</style>
