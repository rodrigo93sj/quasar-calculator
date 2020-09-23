<template>
  <div id="q-app">
    <router-view />
  </div>
</template>
<script>
export default {
  name: 'App'
}
</script>
<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  outline: 0;
}

#q-app {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  height: 100vh;
  color: #fff;
  background: linear-gradient(to bottom, #537895, #09203f )
}
</style>
