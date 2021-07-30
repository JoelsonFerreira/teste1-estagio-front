<template>
  <Header />
  <main>
    <DataContainer />
  </main>
</template>

<script>
import Header from './components/Header.vue';
import DataContainer from './components/DataContainer.vue';

export default {
  name: 'App',
  components: {
    Header,
    DataContainer
  }
};
</script>

<style>
* {
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}

main {

  background: #F1F1F1;
  height: calc(100vh - 87px);
  width: 100%;
  z-index: 0;

  display: flex;
  justify-content: center;
  align-items: center;
  
  padding: 50px;
}
</style>
