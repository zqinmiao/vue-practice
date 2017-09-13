<template>
  <form action="">
    <div>
      <label for="">用户名</label>
      <input type="text" v-model="username">
    </div>
    <div>
      <label for="">密码</label>
      <input type="text" v-model="password">
    </div>
    <div>
      <button type="button" @click="login">登陆</button>
    </div>
  </form>
</template>

<script>
  import axios from 'axios'
  export default {
    name: 'hello',
    data () {
      return {
        password: '',
        username:''
      }
    },
    mounted(){
      console.log('login');
    },
    methods:{
      login(){
          console.log(this.password);
        axios.post('/admin/account/login', {
          username: this.username,
          password: this.password,
        })
          .then(function (response) {
            console.log(response);
          })
          .catch(function (error) {
            console.log(error);
          });

      }
    }
  }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
