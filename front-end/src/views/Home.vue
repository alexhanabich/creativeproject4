<template>
  <div class="home">
    <section class="image-gallery">
      <div class="image" v-for="item in items" :key="item.id">
        <h1>{{item.title}}</h1>
        <img :src="item.path" />
        <h2>{{item.description}}</h2>
        <h2 v-if = "item.likes != 1">Votes: {{item.likes}}  <button class="auto" @click="addLike(item)"><i class="far fa-thumbs-up fa-2x"></i></button></h2>
        <h2 v-else> Vote: {{item.likes}}  <button class="auto" @click="addLike(item)"><i class="far fa-thumbs-up fa-2x"></i></button></h2>
        <h1>Share Your Thoughts!</h1>

        <div class="comment">
          <div class="input">
            <input v-model="addedName" placeholder="Name">
            <textarea v-model="addedComment"></textarea>
            <br />
            <button @click="addComment(item)">Comment</button>
          </div>
          <div class="speech-bubble">
            <div v-for="comment in item.comments" :key="comment.index"> 
              <hr>
              <div class ="top">
                <p class ="author"><i>{{comment.author}}</i></p>
                <p>{{comment.timestamp}}</p>
              </div>
              <p>{{comment.text}}</p>
              
            </div>
            <div v-for="comment in comments" :key="comment.index"> 
              <hr>
              <div class ="top">
                <p class ="author"><i>{{comment.author}}</i></p>
                <p>{{comment.timestamp}}</p>
              </div>
              <p>{{comment.text}}</p>
              
            </div>
          </div>
        </div>

        
      </div>
    </section>
  </div>
</template>

<script>
// @ is an alias to /src
  import axios from 'axios';
  import moment from 'moment';

  export default {
    name: 'Home',
    data() {
      return {
      items: [],
      addedName: '',
      addedComment: '',
      commentIndex: '',
      //comments: [],
      commentData: {},
      comments:[],
      position: 0,
      
      }
    },
  created() {
    this.getItems();
  },
  methods: {
    async addComment(item) {
      try {
        item.index += 1;
        this.comments.push({
          author: this.addedName,
          text: this.addedComment,
          timestamp: moment().format('MMMM Do h:mm a'),
        });
        this.position += 1;
        await axios.put("/api/items/" + item._id, {
          title: item.title,
          description: item.description,
          likes: item.likes,
          index: item.index,
          //comments: this.commentData
          author: this.addedName,
          text: this.addedComment,
          timestamp: moment().format('MMMM Do h:mm a'),
        });
        this.addedName = '';
        this.addedComment = '';
      } catch (error) {
        console.log(error);
      }
    },
    async addLike(item) {
      try {
        item.likes += 1;
        await axios.put("/api/items/" + item._id, {
          title: item.title,
          description: item.description,
          likes: item.likes,
          index: item.index,
          comments: item.comments,
        });
        this.getItems();
        return true;
      } catch (error) {
        console.log(error);
      }
    },
    async getItems() {
      try {
        let response = await axios.get("/api/items");
        this.items = response.data;
        return true;
      } catch (error) {
        console.log(error);
      }
    },
  },
}
</script>

<style scoped>
.image h1 {
  font-size: 150%;
}
.image h2 {
  font-size: 150%;
  font-style: italic;
  display: flex;
  justify-content: space-between;
}
/* Masonry */
*,
*:before,
*:after {
  box-sizing: inherit;
}
.image-gallery {
  display: flex;
  justify-content: space-between;
  column-count: 2;
  column-gap: 30px;
}
.image {
  margin: 0 10% 5% 10%;
  
  display: inline-block;
  width: 100%;
}

.image img {
  width: 100%;
  height: 300px;
  
}

button {
  height: 50px;
  background: #000;
  color: white;
  border: none;
  border-radius: 7px;
}

.auto {
  margin-left: auto;
  transition-duration: 0.1s;
}

.auto:hover {
  background-color: white;
  color: black;
}

.input {
  display: flex;
  flex-direction: column;
}

input {
  width: 30%;
  margin: 0 0 5pt 0;
}

.author {
  font-weight: bold;
  color: gray;
}

.top {
  display: flex;
  justify-content: space-between;
}

.speech-bubble {
	position: relative;
	background: #FEAD42;
	border-radius: .4em;
}

.speech-bubble:after {
	content: '';
	position: absolute;
	top: 0;
	left: 50%;
	width: 0;
	height: 0;
	border: 2px solid black;
	border-bottom-color: #FEAD42;
	border-top: 0;
	border-left: 0;
	margin-left: -1px;
	margin-top: -2px;
}

input,
textarea {
  border-radius: .4em;
}

p {
  margin-left: 10px;
  margin-right: 10px;
  margin-top: 10px;
  margin-bottom: 5px;
}

/* Masonry on large screens */
@media only screen and (min-width: 1024px) {
  .image-gallery {
    
    column-count: 2 !important;
    display: flex;

    
  }
}

/* Masonry on medium-sized screens */
@media only screen and (max-width: 1023px) and (min-width: 768px) {
  .image-gallery {
    column-count: 2 !important;
    display: flex;
    flex-wrap: wrap;
  }
}

/* Masonry on small screens */
@media only screen and (max-width: 767px) and (min-width: 540px) {
  .image-gallery {
    display: flex;
    flex-wrap: wrap;
    column-count: 1;
  }
}
</style>