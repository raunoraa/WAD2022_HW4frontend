<template>
    <div class="A Post">
      <div id="form">
        <h3>A Post</h3>
        <label for="title">Title: </label>
        <input name="type" type="text" id="title" required v-model="post.title" />
        <label for="body">Body: </label>
        <input name="body" type="text" id="body" required v-model="post.body" />

        <!--
        <label for="url">Url: </label>
        <input name="url" type="text" id="url" required v-model="post.urllink" />
        -->
        <div class="buttons">
            <button @click="updatePost" class="updatePost">Update Post</button>
            <button @click="deletePost" class="deletePost">Delete Post</button>
        </div>
      </div>
    </div>
  </template>
  
  
  <script>
  export default {
    name: "APost",
    data() {
        const dateP = new Date();
      const dateString = dateP.toDateString();
      return {
        post: {
          id: "",
          title: "",
          body: "",
          urllink: "",
          date: dateString,
        },
      };
    },
    methods: {
      fetchAPost(id) {
        // fetch one post with the specied id (id)
        fetch(`http://localhost:3000/api/posts/${id}`)
          .then((response) => response.json())
          .then((data) => (this.post = data))
          .catch((err) => console.log(err.message));
      },
      updatePost() {
        // using Fetch - put method - updates a specific post based on the passed id and the specified body
        fetch(`http://localhost:3000/api/posts/${this.post.id}`, {
          method: "PUT",
          headers: {
            "Content-Type": "application/json",
          },
          body: JSON.stringify(this.post),
        })
          .then((response) => {
            console.log(response.data);
            //this.$router.push("/apost/" + this.post.id);
            // We are using the router instance of this element to navigate to a different URL location
            //this.$router.push("/");
            location.assign("/");
          })
          .catch((e) => {
            console.log(e);
          });
      },
      deletePost() {
        // using Fetch - delete method - delets a specific post based on the passed id
        fetch(`http://localhost:3000/api/posts/${this.post.id}`, {
          method: "DELETE",
          headers: { "Content-Type": "application/json" },
        })
          .then((response) => {
            console.log(response.data);
            // We are using the router instance of this element to navigate to a different URL location
            //this.$router.push("/");
            location.assign("/");
            //maybe location.assign is needed not router push
          })
          .catch((e) => {
            console.log(e);
          });
      },
    },
    mounted() {
      // call fetchAPost() when this element mounts, and pass to it a route parameter  (id)
      // Route parameters (this.$route.params.id) are named URL segments that are used to capture the values specified at their 
      // position in the URL. The captured values are populated in the req.params object, with the name 
      // of the route parameter specified in the path as their respective keys
      this.fetchAPost(this.$route.params.id);
    },
  };
  </script>
  
  <style scoped>
  #form {
    max-width: 420px;
    margin: 30px auto;
    margin-top:3em;
    background: rgb(167, 154, 154);
    text-align: left;
    padding: 40px;
    border-radius: 10px;
  }
  h3 {
    text-align: center;
    color: rgb(8, 110, 110);
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
    cursor: pointer;
    font-weight: bold;
  }
  button:hover{
    background: rgb(7, 96, 96);
  }
  button:active{
    background: rgb(5, 76, 76);
  }
  .buttons{
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    margin-top:1em;
  }
  </style>