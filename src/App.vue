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
        :adicionarNumero="adicionarNumero"
        :corrigir="corrigir"
        :confirmar="confirmar"
        :votarBranco="votarBranco"
      />
    </div>
  </div>
</template>

<script>
import "@/assets/css/global.css";
import TecladoUrna from "@/components/Teclado.vue";
import TelaUrna from "@/components/Tela.vue";
import confirmAudio from '@/assets/audios/confirm.wav';
import keyAudio from '@/assets/audios/key.wav';

export default {
  name: "App",
  components: {
    TecladoUrna,
    TelaUrna,
  },
  methods: {
    adicionarNumero(numero) {
      //Executa som da tecla
      this.executarSom(keyAudio);
      //Verifica limite de numeros votados
      if (this.numeroVoto.length == this.quantidadeNumeros) {
        return false;
      }
      //Adiciona o numero selecionado
      this.numeroVoto += "" + numero;

      //Verifica o candidato votado
      this.verificarCandidato();
    },
    verificarCandidato() {
      //Voto Incompleto
      if (this.numeroVoto.length < this.quantidadeNumeros) {
        return false;
      }
      //Verifica Candidato existente
      if (this.candidatos[this.tela][this.numeroVoto]) {
        this.candidato = this.candidatos[this.tela][this.numeroVoto];
        return true;
      }

      //Voto nulo
      this.candidato = {
        nome: "Voto nulo",
        partido: "Voto nulo",
        imagem: "",
      };
    },
    corrigir() {
      //Executa som da tecla
      this.executarSom(keyAudio);
      this.limpar();
    },
    limpar() {
      (this.candidato = {}), (this.numeroVoto = "");
    },
    confirmar() {
      if (this.numeroVoto.length < this.quantidadeNumeros) {
        return false;
      }
      return this.avancarTela();
    },
    avancarTela() {
      //Executa som de confirmação
      this.executarSom(confirmAudio);
      //Vereador
      if (this.tela == "prefeito") {
        this.tela = "vereador";
        this.quantidadeNumeros = 5;
        return this.limpar();
      }
      // Finalização
      this.tela = 'fim';

      // instancia atual
      var me = this;
      // Voltar ao início
      setTimeout(function() {
        me.tela = 'prefeito';
        me.quantidadeNumeros = 2;
        return me.limpar();
      }, 3000);
    },
    votarBranco(){
      // Tela de Finalização não permite mais nada
      if(this.tela == 'fim') return false;
      this.limpar();
      this.avancarTela();
    },
    executarSom(arquivoSom){
      if(arquivoSom){
        let audio = new Audio(arquivoSom);
        audio.play();
      }
    }
  },
  data() {
    return {
      tela: "prefeito",
      numeroVoto: "",
      quantidadeNumeros: 2,
      candidato: {},
      candidatos: {
        prefeito: {
          "01": {
            nome: "Ash",
            partido: "Pokemon",
            imagem:
              "https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/ash.png",
          },
          "08": {
            nome: "Vegeta",
            partido: "Dragon Ball",
            imagem:
              "https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/vegeta.png",
          },
        },
        vereador: {
          "01234": {
            nome: "Pikachu",
            partido: "Pokemon",
            imagem:
              "https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/pikachu.png",
          },
          "08001": {
            nome: "Goku",
            partido: "Dragon Ball",
            imagem:
              "https://raw.githubusercontent.com/william-costa/wdev-urna-eletronica-resources/master/images/goku.png",
          },
        },
      },
    };
  },
};
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
