<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Vue.js 2 | Firebase Sample App</h1>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
      <h3>Add Album</h3>
      </div>
      <div class="panel-body">
      <form id="form" class="form-inline" v-on:submit.prevent="addAlbum">
        <div class="form-group">
          <label for="albumTitle">Title:</label>
          <input type="text" id="albumTitle" class="form-control" v-model="newAlbum.title">
        </div>
        <div class="form-group">
          <label for="albumArtist">Artist:</label>
          <input type="text" id="albumArtist" class="form-control" v-model="newAlbum.artist">
        </div>
        <div class="form-group">
          <label for="albumUrl">Url:</label>
          <input type="text" id="albumUrl" class="form-control" v-model="newAlbum.url">
        </div>
        <input type="submit" class="btn btn-primary" value="Add Album">
      </form>
      </div>
    </div>

    <div class="panel panel-default">
      <div class="panel-heading">
        <h3>Albums List</h3>
      </div>
      <div class="panel-body">
        <table class="table table-striped">
          <thead>
            <tr>
              <th>Album</th>
              <th>Artist</th>
              <th>Delete</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="album in albums">
              <td>
                <a v-bind:href="album.url">{{album.title}}</a>
              </td>
              <td>
                {{album.artist}}
              </td>
              <td>
                <span class="glyphicon glyphicon-trash" v-on:click="removeAlbum(album)"></span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
import Hello from './components/Hello'

import Firebase from 'firebase'

import toastr from 'toastr'

let config = {
  apiKey: "AIzaSyCCR5yLGUzVn5vLr7URKab1WUKp5ZeF-bQ",
  authDomain: "vuejs-firebase-01-5e7f3.firebaseapp.com",
  databaseURL: "https://vuejs-firebase-01-5e7f3.firebaseio.com",
  storageBucket: "vuejs-firebase-01-5e7f3.appspot.com",
  messagingSenderId: "82690815155"
}

let app = Firebase.initializeApp(config)
let db = app.database()

let albumsRef = db.ref('albums')

export default {
  name: 'app',
  firebase: {
    albums: albumsRef
  },
  data () {
    return {
      newAlbum: {
        title: '',
        artist: '',
        url: ''
      }
    }
  },
  methods: {
    addAlbum: function() {
      albumsRef.push(this.newAlbum)
      this.newAlbum.title = ''
      this.newAlbum.artist = ''
      this.newAlbum.url = ''
      toastr.success("Album added")
    },
    removeAlbum: function(album) {
      albumsRef.child(album['.key']).remove()
      toastr.success("Album removed")
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
  margin-top: 60px;
}
</style>
