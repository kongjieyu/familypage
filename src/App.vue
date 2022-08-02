<template>
  <router-view/>
</template>

<style lang="scss">
body {
  margin: 0;
  padding: 0;
  box-sizing: border-box; 
}
#app {
  height: 100vh;
  overflow: hidden;
}
a {
  text-decoration: none;
}
::-webkit-scrollbar {
    width: 6px;
    height: 12px;
    background-color: #fff;
}
 
::-webkit-scrollbar-thumb {
    display: block;
    min-height: 12px;
    min-width: 6px;
    border-radius: 6px;
    background-color: rgb(217, 217, 217);
}
 
::-webkit-scrollbar-thumb:hover {
    display: block;
    min-height: 12px;
    min-width: 6px;
    border-radius: 6px;
    background-color: rgb(159, 159, 159);
}
</style>
