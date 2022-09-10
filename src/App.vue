<template>
  <div id="app">
    <div class="urna">

      <TelaUrna 
        :tela="tela"
        :numeroVoto="numeroVoto"
        :quantidadeNumeros="quantidadeNumeros"
      />

      <TecladoUrna />

    </div>
  </div>
</template>

<script>
import '@/assets/css/global.css'
import TecladoUrna from '@/components/Teclado.vue';
import TelaUrna from '@/components/Tela.vue';

export default {
  name: 'App',
  components: {
    TecladoUrna,
    TelaUrna
  },
  data() {
    return {
      tela: 'vereador',
      numeroVoto: '2',
      quantidadeNumeros: 2
    }
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
  justify-content: space-between;
}

</style>
