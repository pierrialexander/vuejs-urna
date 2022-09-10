<template>
  <div id="app">
    <div class="urna">
      <TecladoUrna />
    </div>
  </div>
</template>

<script>
import '@/assets/css/global.css'
import TecladoUrna from '@/components/Teclado.vue';

export default {
  name: 'App',
  components: {
    TecladoUrna
  }
}
</script>

<style>
#app {
  background-color: var(--background-color);
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
}

.urna {
  width: 1000px;
  height: 500px;
  background-color: var(--ballot-box-background-color);
  padding: 30px;
  border-radius: 5px;
  display: flex;
}
</style>
