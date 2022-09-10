<template>
  <div id="app">
    <div class="urna">

      <TelaUrna 
        :tela="tela"
        :numeroVoto="numeroVoto"
        :quantidadeNumeros="quantidadeNumeros"
        :candidato="candidato"
      />

      <TecladoUrna 
        :adicionarNumero = 'adicionarNumero'
        :corrigir = 'corrigir'
      />

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
  methods: {
    adicionarNumero(numero){
      //Verifica limite de numeros votados
      if(this.numeroVoto.length == this.quantidadeNumeros){
        return false;
      }
      //Adiciona o numero selecionado
      this.numeroVoto += ''+numero;

      //Verifica o candidato votado
      this.verificarCandidato();
    },
    verificarCandidato(){
      //Voto Incompleto
      if(this.numeroVoto.length < this.quantidadeNumeros){
        return false;
      }
      //Verifica Candidato existente
      if(this.candidatos[this.tela][this.numeroVoto]){
        this.candidato = this.candidatos[this.tela][this.numeroVoto];
        return true;
      }

      //Voto nulo
      this.candidato = {
        nome: 'Voto nulo',
        partido: 'Voto nulo',
        imagem: ''
      }
    },
    corrigir() {
      this.limpar()
    },
    limpar() {
      this.candidato = {},
      this.numeroVoto = ''
    }
  },
  data() {
    return {
      tela: 'prefeito',
      numeroVoto: '',
      quantidadeNumeros: 2,
      candidato:{},
      candidatos: {
                    "prefeito":{
                      "01":{
                        "nome": "Ash",
                        "partido": "Pokemon",
                        "imagem": "https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/ash.png"
                      },
                      "08":{
                        "nome": "Vegeta",
                        "partido": "Dragon Ball",
                        "imagem": "https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/vegeta.png"
                      }
                    },
                    "vereador":{
                      "01234":{
                        "nome": "Pikachu",
                        "partido": "Pokemon",
                        "imagem": "https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/pikachu.png"
                      },
                      "08001":{
                        "nome": "Goku",
                        "partido": "Dragon Ball",
                        "imagem": "https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/goku.png"
                      }
                    }
                  }
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
