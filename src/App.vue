<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="navbar-brand" href="#">Exemplo de acesso com Vue.js 2 & Firebase</div>
    </nav>
    <div id="app" class="container">
      <div class="panel panel-default">
        <div class="panel-heading">
          <h3 class="panel-title">Adicionar novo item</h3>
        </div>
        <div class="panel-body">
          <form id="form" class="form-inline" v-on:submit.prevent="addItem">
            <div class="form-group">
              <label for="bookTitle">Título:</label>
              <input type="text" id="bookTitle" class="form-control" v-model="newItem.titulo">
            </div>
            <div class="form-group">
              <label for="bookAuthor">Autor:</label>
              <input type="text" id="bookAuthor" class="form-control" v-model="newItem.autor">
            </div>
            <div class="form-group">
              <label for="bookUrl">Url:</label>
              <input type="text" id="bookUrl" class="form-control" v-model="newItem.url">
            </div>
            <input type="submit" class="btn btn-primary" value="Adicionar">
          </form>
        </div>
      </div>
      <div class="panel panel-default">
        <div class="panel-heading">
          <h3 class="panel-title">Listagem</h3>
        </div>
        <div class="panel-body">
          <table class="table table-striped">
            <thead>
            <tr>
              <th>Título</th>
              <th>Autor</th>
              <th>Ações</th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="item in itens" :key="item.id">
              <td><a v-bind:href="item.url">{{item.titulo}}</a></td>
              <td>{{item.autor}}</td>
              <td><span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeItem(item)"></span></td>
            </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import Firebase from 'firebase'
import toastr from 'toastr'
let config = {
  // preencha com as informações vindas do seu banco de dados firebase
  // acesse firebase.google.com, crie o seu cadastro e monte sua base de dados
  apiKey: '',
  authDomain: '',
  databaseURL: '',
  storageBucket: '',
  messagingSenderId: ''
}

let app = Firebase.initializeApp(config)
let db = app.database()
let bdRef = db.ref('itens')

export default {
  name: 'app',
  firebase: {
    itens: bdRef
  },

  data () {
    return {
      newItem: {
        titulo: '',
        autor: '',
        url: 'http://'
      }
    }
  },

  methods: {
    addItem: function () {
      bdRef.push(this.newItem)
      toastr.success('Item inserido com sucesso!')
      this.newBook.title = ''
      this.newBook.author = ''
      this.newBook.link = ''
    },
    removeItem: function (item) {
      bdRef.child(item['.key']).remove()
      toastr.success('Item removido com sucesso!')
    }
  }
}
</script>
<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    color: #2c3e50;
    margin-top: 20px;
  }
</style>
