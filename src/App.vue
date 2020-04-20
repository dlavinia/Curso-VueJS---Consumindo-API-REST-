<template>
  <div id="app">

    <nav>
      <div class="nav-wrapper blue darken-1">
        <a href="#" class="brand-logo center">Produtos Front</a>
      </div>
    </nav>

    <div class="container">

      <form @submit.prevent="salvar">

          <label>Nome</label>
          <input type="text" placeholder="Nome" v-model="produto.nome">
          <label>Quantidade</label>
          <input type="number" placeholder="QTD" v-model="produto.quantidade">
          <label>Valor</label>
          <input type="text" placeholder="Valor" v-model="produto.valor">

          <button class="waves-effect waves-light btn-small">Salvar<i class="material-icons left">save</i></button>

      </form>

      <table>

        <thead>

          <tr>
            <th>NOME</th>
            <th>QTD</th>
            <th>VALOR</th>
            <th>OPÇÕES</th>
          </tr>

        </thead>

        <tbody>

          <tr v-for = "produto of produtos" :key="produto.id">

            <td>{{ produto.nome }}</td>
            <td>{{ produto.quantidade }}</td>
            <td>{{ produto.valor }}</td>
            <td>
              <button @click="editar(produto)" class="waves-effect btn-small blue darken-1"><i class="material-icons">create</i></button>
              <button @click="remover(produto)" class="waves-effect btn-small red darken-1"><i class="material-icons">delete_sweep</i></button>
            </td>

          </tr>

        </tbody>
      
      </table>

    </div>

  </div>
</template>
<script>

import Produto from "./services/produtos"

export default {

  data(){
    return{
        produto:{
          id: '',
          nome: '',
          quantidade: '',
          valor: '',
        },

      produtos: []
    }
  }
  ,mounted(){
    this.listar()
  }, 

  methods:{

    listar(){
      Produto.listar().then(resposta => {
        this.produtos = resposta.data
      })
    },

      salvar(){
        if (!this.produto.id) {
          Produto.salvar(this.produto).then(resposta => {
          this.produto = {}
          alert('Salvo com sucesso!')
          this.produto = resposta
          this.listar()
      })
        } else{
          Produto.atualizar(this.produto).then(resposta => {
          this.produto = {}
          alert('Atualizado com sucesso!')
          this.produto = resposta
          this.listar()
      })
        } 
        
      },

     editar(produto){
      this.produto = produto;  
      },

      remover(produto){

        if(confirm('Deseja excluir o produto?')){ 
          Produto.apagar(produto).then(resposta => {
          this.listar()
          this.produto = resposta
        })
        }
      }
      }
     
    }
</script>

<style>

</style>
