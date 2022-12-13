<template>
    <div class="form">
      <h3>Sign Up</h3>
      <label for="email">Email</label>
      <input type="email" name="email"  required v-model="email">
      <label for="password">Password</label>
      <input type="password" name="password" required v-model="password">
      <button @click="SignUp" class="SignUp">Sign Up</button>
      <h3 id="info">Password length must be at least 8 characters!</h3>
      <h3 id="info2">Email field can't be empty!</h3>
      <h3 id="info3">This email is already in use!</h3>
    </div>
  </template>
  
  <script>
  export default {
  name: "SignUp", 
  
  data: function() {
      return {
     email: '',
     password: '',
    }
    },
    methods: {
  
  
  SignUp() {
      
        if(this.email.length<1){
          document.getElementById("info2").style.display = "list-item";
        }
        else if(this.password.length < 8){
          document.getElementById("info").style.display = "list-item";
        } else{
      

        var data = {
          email: this.email,
          password: this.password
        };
        // using Fetch - post method - send an HTTP post request to the specified URI with the defined body
        fetch("http://localhost:3000/auth/signup", {
          method: "POST",
          headers: {
            "Content-Type": "application/json",
          },
            credentials: 'include', //  Don't forget to specify this if you need cookies
            body: JSON.stringify(data),
        })
        .then((response) => response.json())
        .then((data) => {
        console.log(data);
        this.$router.push("/");
        //location.assign("/");
        })
        .catch((e) => {

          if(e.toString().includes("duplicate")){
            document.getElementById("info3").style.display = "list-item";
          }

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
    margin-top:4em;
    background: rgb(167, 154, 154);
    text-align: left;
    padding: 40px;
    border-radius: 10px;
  }
  h3 {
    text-align: center;
    color: rgb(108, 205, 72);
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
    margin-top: 20px;
    color: white;
    border-radius: 20px;
    align-items: center;
    text-align: center;
    cursor: pointer;
    font-weight: bold;
  }

  button:hover{
    background: rgb(232, 59, 59);
  }

  button:active{
    background: rgb(132, 3, 3);
  }

  </style>