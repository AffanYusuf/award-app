<template>
    <div>
        <center>
            <img src="" alt="">
            <h1><b style="color:gray;">AWARD</b></h1>
            <p>
                Enter your email address to sign in and continue.
            </p>
        </center>

        <b-form-input
        id="input-1"
        v-model="email"
        type="email"
        placeholder="Email Address"
        required
        ></b-form-input>
        <br>
        <b-button @click="onSubmit()" block type="submit" variant="secondary">Submit</b-button>
    </div>
  </template>
  
  <script>
  import axios from "axios";
  
  export default {
    name: "Login",
 
    data() {
      return {
          email: '',
      };
    },
    methods: {
      onSubmit() {
        const email = this.email;
        axios
        .post("http://localhost:5000/award-service/login", {email})
        .then(res => {
            axios.defaults.headers.common['Authorization'] = 'Bearer ' + res.data.data.bearerToken;
            localStorage.setItem( 'token', JSON.stringify(res.data.data.bearerToken) );
            this.$router.push({ name: "Home"})
        })
        .catch(() => alert('Invalid input email. No user found'));
      }
    },
    mounted () {
        localStorage.removeItem('token');
    }
  };
  </script>
  