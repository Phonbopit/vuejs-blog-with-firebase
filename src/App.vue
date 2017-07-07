<template>
  <div id="app">
    <section class="hero is-info">
      <div class="hero-body">
        <div class="container">
          <h1 class="title">{{ title }}</h1>
          <h2>{{ subtitle }}</h2>
        </div>
      </div>
    </section>
    <section class="new-post">
      <div class="container">
        <div class="columns">
          <div class="column is-half is-offset-one-quarter">
            <h2 class="title">Create new post</h2>
            <form id="form" v-on:submit.prevent="addPost">
              <div class="field">
                <label class="label">Title</label>
                <p class="control">
                  <input class="input" type="text" placeholder="Title" v-model="newPost.title">
                </p>
              </div>
              <div class="field">
                <label class="label">Content</label>
                <p class="control">
                  <textarea class="textarea" placeholder="Put your content here." v-model="newPost.text"></textarea>
                </p>
              </div>
              <div class="field">
                <input type="submit" class="button is-primary" value="Add Post" :disabled="newPost.title === '' && newPost.text === ''">
              </div>
            </form>
          </div>
        </div>
      </div>
    </section>
    <hr>
    <section class="posts">
      <div class="container">
        <div class="columns">
          <div class="column is-half is-offset-one-quarter">
            <h2 class="title">Posts</h2>
            <div class="box" v-for="post in posts">
              <div class="media-content columns">
                <div class="column is-10">
                  <p><strong>{{post.title}}</strong>
                  <br>
                  {{post.text}}
                  </p>
                  <br>
                  <span class="date">On {{post.created_at}}</span>
                </div>
                <div class="column is-2 is-centered">
                  <a class="button is-danger is-outlined is-pulled-right" v-on:click="removePost(post)">Delete</a>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </section>
    <footer class="footer">
      <div class="container">
        <div class="content has-text-centered">
          <p>Powered by <a href="https://devahoy.com" target="_blank">DevAhoy</a></p>
        </div>
      </div>
    </footer>
  </div>
</template>

<script>
import Firebase from 'firebase'
import moment from 'moment'

import config from '../firebase.token'

let app = Firebase.initializeApp(config)
let db = app.database()

let postsRef = db.ref('posts')

export default {
  name: 'app',

  firebase: {
    posts: postsRef
  },

  data() {
    return {
      title: 'Vue Blog',
      subtitle: 'Blogging with Vue.js + Firebase Example',
      newPost: {
        title: '',
        text: ''
      }
    }
  },

  methods: {
    addPost: function() {

      this.newPost.created_at = moment().format('DD MMM, YYYY HH:mm:ss')
      postsRef.push(this.newPost)

      this.newPost.title = ''
      this.newPost.text = ''
    },
    removePost: function(post) {
      postsRef.child(post['.key']).remove()
    }
  }
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
}
.new-post {
  padding: 2em 0;
}

.posts {
  margin: 2em 0;
}

.posts .date {
  font-size: 0.75rem;
}

.is-centered {
  text-align: center;
  align-self: center;
}

.footer {
  padding: 3rem 1.5rem;
}
</style>
