<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <form>
      <div id="">
      Email:<input v-model="email"  />
    </div>
    <div id="password">
      password<input v-model="password" id="password" type="password"/>
    </div>
    <div v-if="isSignIn">
      <button type="button" @click="signOut">Logout</button>
      <button type="button" @click="resetPassword">Reset Password</button>
    </div>
    <div v-else>
      <button type="button" @click="signIn">Sign in</button>
      <button type="button" @click="signUp">Sign up</button>
    </div>
    </form>

    <div v-if="isSignIn">
      <img :src="currentUser.photoURL" />
      ようこそ！{{currentUser.displayName}}さん！
      <ul>
        <li>Email:{{currentUser.email}}</li>
      </ul>

    </div>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import firebase from 'firebase/app'
import 'firebase/auth'

export default {
  name: 'app',
  components: {
    HelloWorld
  }, 
  created: function() {
    const config = {
      apiKey: "AIzaSyBT-s_6-NSIiRT33Kf4bjWUPxgQ2dcV9CU",
      authDomain: "vue-auth-894e4.firebaseapp.com",
      projectId: "vue-auth-894e4",
    };
    this.firebase = firebase
    this.firebase.initializeApp(config);
    this.firebase.auth().onAuthStateChanged((user) => {
      if (user) {
        this.currentUser = user
      } else {
        this.currentUser = null
      }
    });
    this.currentUser = this.firebase.auth().currentUser
  },
  data: function(){
    return {
      firebase: {},
      email: '',
      password: '',
      currentUser: null
    }
  },
  computed: {
    isSignIn: function() {
      return !!this.currentUser
    }
  },
  methods: {
    signIn: function() {
      this.firebase.auth().signInWithEmailAndPassword(this.email, this.password).catch(function(error) {
        // Handle Errors here.
        console.error(error)
        var errorCode = error.code;
        var errorMessage = error.message;
        // ...
      });

    },
    signUp: function() {
      this.firebase.auth().createUserWithEmailAndPassword(this.email, this.password).catch(function(error) {
        // Handle Errors here.
        console.error(error)
        const errorCode = error.code;
        const errorMessage = error.message;

        // ...
      });
    },
    resetPassword: function() {
      this.firebase.auth().sendPasswordResetEmail(this.currentUser.email).then(function(){
        console.log('email sent.')
      })
    },
    signOut: function() {
      this.firebase.auth().signOut()
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
  color: #2c3e50;
  margin-top: 60px;
}
</style>
