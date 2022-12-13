<template>
    <div class="form">
      <h3>Log In</h3>
      <label for="email">Email</label>
      <input type="email" name="email"  required v-model="email">
      <label for="password">Password</label>
      <input type="password" name="password" required v-model="password">
      <div class="container">
        <button @click="LogIn"  class="center">Log In</button>
        <button @click='this.$router.push("/signup")' class="center">Sign Up</button>
      </div>
      <h3 id="info">Password length must be at least 8 characters!</h3>
      <h3 id="info2">Email field can't be empty!</h3>
      <h3 id="info3">Wrong email or password!</h3>
    </div>
  </template>
  
  <script>
  export default {
  name: "LogIn", 
  
  data: function() {
      return {
     email: '',
     password: '',
    }
    },
    methods: {
  
  
  LogIn() {


    if(this.email.length<1){
      document.getElementById("info").style.display = "none";
      document.getElementById("info3").style.display = "none";
          document.getElementById("info2").style.display = "list-item";
        }
        else if(this.password.length < 8){
          document.getElementById("info2").style.display = "none";
          document.getElementById("info3").style.display = "none";
          document.getElementById("info").style.display = "list-item";
        } else{

        var data = {
          email: this.email,
          password: this.password
        };
        // using Fetch - post method - send an HTTP post request to the specified URI with the defined body
        fetch("http://localhost:3000/auth/login", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
            credentials: 'include', //  Don't forget to specify this if you need cookies
            body: JSON.stringify(data),
        })
        .then((response) =>
        { 
          response.json()
          if(response.status===401){
            document.getElementById("info").style.display = "none";
            document.getElementById("info2").style.display = "none";
            document.getElementById("info3").style.display = "list-item";
          }
        })
        .then((data) => {
        
        this.$router.push("/");
        //location.assign("/");
        console.log(data);

        })
        .catch((e) => {

          console.log(e);
          console.log("error");
        });
      }
      },
    }, 
    }
  
  </script>
  
  <style scoped>
  .form {
    max-width: 420px;
    margin: 30px auto;
    margin-top: 4em;
    background: rgb(167, 154, 154);
    text-align: left;
    padding: 40px;
    border-radius: 10px;
  }
  h3 {
    text-align: center;
    color: rgb(108, 205, 72);
  }
  label {
    color: rgb(8, 110, 110);
    display: inline-block;
    margin: 25px 0 15px;
    font-size: 0.8em;
    text-transform: uppercase;
    letter-spacing: 1px;
    font-weight: bold;
  }
  input {
    display: block;
    padding: 10px 6px;
    width: 100%;
    box-sizing: border-box;
    border: none;
    border-bottom: 1px solid white;
    color: blue;
  }
  button {
    background: rgb(8, 110, 110);
    border: 0;
    padding: 10px 20px;
    margin: 20px 20px 20px 20px;
    color: white;
    border-radius: 20px;
    align-items: center;
    text-align: center;
  }
  .center {
    margin: auto;
    border: 0;
    padding: 10px 20px;
    margin-top: 20px;
    width: 30%; 
    cursor:pointer;
    font-weight: bold;
  }

  .center:hover{
    background: rgb(232, 59, 59);
  }

  .center:active{
    background: rgb(132, 3, 3);
  }

  .container {
    display: flex;
    justify-content: center;
  }

  #info{
    color: red;
    display: none;
    list-style-type: none;
  }

  #info2{
    color: red;
    display: none;
    list-style-type: none;
  }

  #info3{
    color: red;
    display: none;
    list-style-type: none;
  }

  </style>