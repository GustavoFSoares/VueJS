

<template>    
    <div>
        <h1 class="centralizado">{{ titulo }}</h1>

        <input type="search" class="filtro" @input="filtro = $event.target.value" placeholder="Filtro de titulos"></input>
        <ul class="lista-fotos">
            <li class="lista-fotos-item" v-for="foto of fotosComFiltro">

                <painel :titulo="foto.titulo">
                    <imagem :url="foto.url" :titulo="foto.titulo"/>
                    <botao 
                    tipo="button"
                    :confirmacao="false"
                    @botaoAtivado="remove(foto)"
                    rotulo="remover"
                    estilo="perigo"
                    />
                </painel>

            </li>
        </ul>

    </div>
</template>

<script>
import Painel from '../shared/painel/Painel.vue'
import ImagemResponsiva from "../shared/imagem-responsiva/Imagem-responsiva.vue";
import Botao from "../shared/botao/Botao.vue";

export default {

    components: {
        'painel': Painel,
        'imagem': ImagemResponsiva,
        'botao': Botao,
    },
    data() {
        return {
            titulo: 'Alurapic',
            fotos: [ ],
            filtro: '',
            
        }
    },
    methods: {
        remove(foto) {
                alert('removee  ')       
        }
    },
    created(){
        this.$http.get('http://localhost:3000/v1/fotos')
            .then(res => {
                res.json().then(fotos => {
                    this.fotos = fotos
                    console.log(fotos)
                }, err => {
                    console.log(err)
                })
            })
    },
    computed: {
        fotosComFiltro(){
            if(this.filtro){
                let exp = new RegExp(this.filtro.trim(), 'i')
                return this.fotos.filter( foto => exp.test(foto.titulo))
            } else {
                return this.fotos
            }
        }
    }
}
</script>

<style>
  .centralizado {
    text-align: center;
  }

  .lista-fotos {
    list-style: none;
  }

  .lista-fotos .lista-fotos-item {
    display: inline-block;
  }

  .imagem-responsiva {
      width: 100%;
  }

  .filtro {
      display: block;
      width: 100%
  }
    

</style>
