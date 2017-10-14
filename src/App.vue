<template>
  <div>
    <router-view/>
    <section class="hero is-success is-medium">
  <!-- Hero head: will stick at the top -->
  <div class="hero-head">
    <nav class="navbar">
      <div class="container">
        <div class="navbar-brand">
          <FONT FACE="supermarket" SIZE="500px" style="margin-left: 10px">
       <b >Kin rai dee</b></FONT>
       <img src="./assets/มือ4.png" style="margin-left: -10px">
          <span class="navbar-burger burger" data-target="navbarMenuHeroA">
          </span>
        </div>
      </div>
    </nav>
  </div>
      </section>
      <section>
      <div class="container">
        <br>
        <div class="columns">
          <div class="column is-narrow is-4">
            <div>
              <FONT FACE="supermarket" SIZE=500px>
          <img src="./assets/add.png" width="40 px" height="40 px"><b >Menu</b></FONT>
              <label class="label">ชื่ออาหาร :</label><!--v-model เป็นการผูกตัวแปรเอาไว้เรียกใช้ได้-->
              <input class="input" type="text" placeholder="Normal input" v-model="name">
              <label class="label">ประเทศ :</label>
              <select class="input" name="" v-model="type">
                <option value="Thai">Thailand</option>
                <option value="Japanese" >Japan</option>
                <option value="French" >French</option>
                <option value="Chinese" >China</option>
                <option value="Vietnamese" >Vietnam</option>
                <option value="Italian" >Italia</option>
              </select>
              <label class="label">รูปภาพ (link) :</label>
              <input class="input" type="text" placeholder="Normal input" v-model="picture">
            </div><br>
            <div class="control">
            <button type="submit" class="button is-warning is-active" @click="addPost" style="margin-left: 120px">submit</button>
            </div>
          </div>
          <div class="column is-8 is-offset-1 ">
            <div class="box is-success">
              <FONT FACE="supermarket" SIZE="250px" >
          <b >หิวแล้ว~~ กิน <b style="color:#00CC66">{{food.name}} </b> กัน</b></FONT> <br> <br> <br>
          <br><br>
          <img :src="food.picture" width="300" height="250" hspace="130" style="margin-top: -80px">
          <div class="field has-addons">
          <div class="control is-expanded">
          <div class="select is-fullwidth">
            <select class="input" name="" style="width : 100%" v-model="t">
              <option value="Thai">Thailand</option>
              <option value="Japanese" >Japan</option>
              <option value="French" >French</option>
              <option value="Chinese" >China</option>
              <option value="Vietnamese" >Vietnam</option>
              <option value="Italian" >Italia</option>
           </select>
           </div>
           </div>
            <button type="button" name="button" class="button is-warning" style="width : 10%" @click="country(t)">Random</button>
            </div>

            <div class="box" v-for="data in foods" v-if="data.type === t">
              <div class="" >
                <button class="delete is-danger" aria-label="delete" @click = "deleteData(data)"></button>
                <img :src="data.picture" width="100 px" height="100 px">&nbsp;&nbsp;
                <FONT FACE="supermarket" font-size="40 px">{{data.name}}</FONT>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    </section>
  </div>
</template>

<script>
import firebase from 'firebase'
var config = {
  apiKey: 'AIzaSyAXvonR6MfDqW33oChm7Bp1FDtNgXICgOk',
  authDomain: 'vuefireassign.firebaseapp.com',
  databaseURL: 'https://vuefireassign.firebaseio.com',
  projectId: 'vuefireassign',
  storageBucket: '',
  messagingSenderId: '70574728321'
}
var vuefireassign = firebase.initializeApp(config)
var database = vuefireassign.database()
export default {
  data () {
    return {
      name: '',
      type: '',
      picture: '',
      food: []
    }
  },
  mounted () {
    var vm = this
    vm.$bindAsArray('foods', database.ref('foods'))
  },
  methods: {
    addPost () {
      this.$firebaseRefs.foods.push({
        name: this.name,
        type: this.type,
        picture: this.picture
      })
      this.name = ''
      this.type = ''
      this.picture = ''
      this.$swal(
        'successful',
        'Addmenu Complete',
        'success'
      )
    },
    deleteData (data) {
      console.log(data['.key'])
      this.$firebaseRefs.foods.child(data['.key']).remove()
    },
    country (t) {
      var a = this.foods.filter(food => food.type === t)
      var b = a[Math.round(Math.random() * (a.length - 0) + 0)]
      this.food = b
      console.log(this.food)
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
