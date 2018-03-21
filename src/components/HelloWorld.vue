<template>
  <div class="hello">
    <input type="text" v-model="newTodo"> <button @click="add()">ok</button> <br>
    <div v-for="(d, index) in data" :key="index">
      {{d.text}}
      <input type="text" :value="d.text" @input="update(d, $event.target.value)">
      <button @click="del(d)">x</button><br>
    </div>
  </div>
</template>

<script>
import Firebase from 'firebase'
var config = {
  apiKey: 'AIzaSyCBdN7bhiFcwSYmK02pAIru4OQmLA8h3Ag',
  authDomain: 'test-e7aa1.firebaseapp.com',
  databaseURL: 'https://test-e7aa1.firebaseio.com',
  projectId: 'test-e7aa1',
  storageBucket: 'test-e7aa1.appspot.com',
  messagingSenderId: '955818511566'
}
let app = Firebase.initializeApp(config)
let db = app.database()
let dataRef = db.ref('data')
export default {
  name: 'HelloWorld',
  firebase: {
    data: dataRef
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      newTodo: ''
    }
  },
  methods: {
    add () {
      dataRef.push({
        text: this.newTodo,
        done: false
      })
      this.newTodo = ''
    },
    del (data) {
      dataRef.child(data['.key']).remove()
    },
    update (data, text) {
      dataRef.child(data['.key'] + '/num').set(text)
      // dataRef.child(data['.key']).set({
      //   text: text,
      //   num: data.num
      // })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
