<template>
  <div class="main">
    <div class="container">
    <button v-if = "authResult" @click="Logout" class="logoutb">Logout</button>
    </div>
    <div id="post-list">
          <div class="item" v-for="post in posts" :key="post.id">
            <!-- / We are putting an anchor for each post, when we click on it, we will be directed to the specific post view (/apost/) /  -->
            <!-- <a class="singlepost" :href="'/apost/' + post.id"> -->
            <a class="singlepost" @click="GoToAPost(post.id)">
              
              <div class="post-header">
                <p>{{post.title}}</p>
                <p>{{post.date}}</p>
              </div>

              <div class="post-body">
                <p class="normal">{{ post.body }}</p>
              </div>
              <!-- <span class="url"> <b>Url:</b> {{ post.urllink }} </span> <br /> -->
            </a>
          </div>

    </div>
    <div class="buttonContainer">
      <button @click="GoToAddPost">Add Post</button>
      <button @click="DeleteAllPosts">Delete All</button>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import auth from "../auth";

export default {
  name: "HomeView",
  components: {
  },
   data: function() {
    return {
    posts:[ ],
    authResult: auth.authenticated()
    }
  },
  methods: {
    GoToAPost(id){

      let string = "/apost/"+id
      this.$router.push(string)
    },
    DeleteAllPosts(){ //it has a weird bug, which occurs rarely under very specific conditions (haven't been able to reproduce it myself after adding another location.assign), in which case, the page view will still display some of the posts although they are deleted in the database
      let p =null;
      for (p of this.posts) {

        
        fetch(`http://localhost:3000/api/posts/${p.id}`, {
        method: "DELETE",
        headers: { "Content-Type": "application/json" },
      })
        .then((response) => {
          console.log(response.data);
          // We are using the router instance of this element to navigate to a different URL location
          //this.$router.push("/");
          location.assign("/");
        })
        .catch((e) => {
          console.log(e);
        }); 
        location.assign("/");
      }
    },
    GoToAddPost(){
      this.$router.push("/addpost")
    },
    Logout() {
      fetch("http://localhost:3000/auth/logout", {
          credentials: 'include', //  Don't forget to specify this if you need cookies
      })
      .then((response) => response.json())
      .then((data) => {
        console.log(data);
        console.log('jwt removed');
        //console.log('jwt removed:' + auth.authenticated());
        this.$router.push("/login");
        //location.assign("/");
      })
      .catch((e) => {
        console.log(e);
        console.log("error logout");
      });
    },
    fetchPosts() {
      // You should remember how Fetch API works
      // fetch is a GET request unless stated otherwise. Therefore, it will fetch all posts from the database
      fetch(`http://localhost:3000/api/posts/`)
        .then((response) => response.json())
        .then((data) => (this.posts = data))
        .catch((err) => console.log(err.message));
    }
  }, 
  mounted() {

          // call fetchPosts() when this element (AllPosts) mounts 
    this.fetchPosts();
    console.log("mounted");

      /*
        fetch('https://jsonplaceholder.typicode.com/posts')
        .then((response) => response.json())
        .then(data => this.posts = data)
        .catch(err => console.log(err.message))
        */
    }
};
</script>

<style scoped>
body{
  margin: 20px 40px;
  font-size: 1.2rem;
  letter-spacing: 1px;
  position: relative;
}

.main{
  margin-top:3em;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.item{
  margin: 2em;
  
  /*
  display: flex;
  flex-direction: column;
  */
  /*align-items: center;*/
}

.singlepost{

  background: rgb(147, 194, 16);
  padding: 2em;
  border-radius: 1em;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  inline-size: 600px;
  overflow-wrap: break-word;
}

@media (max-width:800px) {
  .singlepost{
    inline-size: 300px;
  }
}

.singlepost:hover{
  background: rgb(133, 176, 17);
}

.singlepost:active{
  background: rgb(109, 143, 18);
}

h3{
    margin: 0;
  padding: 0;
  font-family: 'Quicksand', sans-serif;
  color: #444;
  background: #7e9756;
}

/*
nav{
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 80px;
}
*/

/*
input{
  padding: 10px 12px;
  border-radius: 4px;
  border: 1px solid #ddd;
  font-size: 1em;
  width: 100%;
}

label{
  display: block;
  margin: 20px 0 10px;
}
*/
button{
  margin-top: 30px;
  margin-left:2em;
  margin-right:2em;
  margin-bottom:3em;
  border-radius: 36px;
  background: #a8995b;
  border:0;
  font-weight: 700;
  font-size: 0.8em;
  display: block;
  padding: 10px 16px;
  letter-spacing: 2px;
  cursor: pointer;
}
/*
nav{
  display: flex;
  align-items: center;
}
*/
.container {
  display: flex;
  justify-content: center;
}


/*
#post-list {
  margin-bottom: 30px;
  padding: 10px 20px;
  margin: auto;
  width: 50%;
  border-radius: 20px;
}
#post-list ul {
  padding: 0;
}
#post-list li {
  display: inline-block;
  margin-right: 10px;
  margin-top: 10px;
  padding: 20px;
  background: rgba(255, 255, 255, 0.7);
}
*/

p{
  margin:1em;
  font-weight: bold;
}

.post-header{
  display: flex;
  flex-direction:row;
  justify-content: space-between;
  padding: 1em;
  background: rgb(175, 175, 231);
  margin-bottom:1em;
  border-radius: 1em;
}

.post-body{
  padding: 2em;
  background: rgb(192, 162, 162);
  text-align: left;
  overflow-wrap: break-word;
  border-radius: 1em;
}

.normal{
  font-weight: normal;
}

.logoutb{
  margin-top:0;
  margin-bottom:0;
  padding-left: 3em;
  padding-right:3em;
}

.buttonContainer{
  display: flex;
  flex-direction: row;
}

button:hover{
  background: #c1af68;
}

button:active{
  background: #d28b71;
}

</style>
