<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
      <div class="navbar-brand" href="#">Exemplo de acesso com Vue.js 2 & Firebase</div>
    </nav>
    <div id="app" class="container">
      <div class="panel panel-default">
        <div class="panel-heading">
          <h3 class="panel-title">Add New Books</h3>
        </div>
        <div class="panel-body">
          <form id="form" class="form-inline" v-on:submit.prevent="addBook">
            <div class="form-group">
              <label for="bookTitle">Title:</label>
              <input type="text" id="bookTitle" class="form-control" v-model="newBook.title">
            </div>
            <div class="form-group">
              <label for="bookAuthor">Author:</label>
              <input type="text" id="bookAuthor" class="form-control" v-model="newBook.author">
            </div>
            <div class="form-group">
              <label for="bookUrl">Url:</label>
              <input type="text" id="bookUrl" class="form-control" v-model="newBook.link">
            </div>
            <input type="submit" class="btn btn-primary" value="Add Book">
          </form>
        </div>
      </div>
      <div class="panel panel-default">
        <div class="panel-heading">
          <h3 class="panel-title">Book List</h3>
        </div>
        <div class="panel-body">
          <table class="table table-striped">
            <thead>
            <tr>
              <th>Title</th>
              <th>Author</th>
              <th></th>
            </tr>
            </thead>
            <tbody>
            <tr v-for="book in books" :key="book.id">
              <td><a v-bind:href="book.url">{{book.title}}</a></td>
              <td>{{book.author}}</td>
              <td><span class="glyphicon glyphicon-trash" aria-hidden="true" v-on:click="removeBook(book)"></span></td>
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
  apiKey: 'AIzaSyDTJ2yTw25kEk0cduNJTZvvXkfTfPRreEU',
  authDomain: 'vuejs-firebase-62b72.firebaseapp.com',
  databaseURL: 'https://vuejs-firebase-62b72.firebaseio.com',
  storageBucket: 'vuejs-firebase-62b72.appspot.com',
  messagingSenderId: '33182910000'
}

let app = Firebase.initializeApp(config)
let db = app.database()
let booksRef = db.ref('books')

export default {
  name: 'app',
  firebase: {
    books: booksRef
  },

  data () {
    return {
      newBook: {
        title: '',
        author: '',
        link: 'http://'
      }
    }
  },

  methods: {
    addBook: function () {
      booksRef.push(this.newBook)
      toastr.success('Livro inserido com sucesso!')
      this.newBook.title = ''
      this.newBook.author = ''
      this.newBook.link = ''
    },
    removeBook: function (book) {
      booksRef.child(book['.key']).remove()
      toastr.success('Livro removido com sucesso!')
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
