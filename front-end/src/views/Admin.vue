<template>
<div class="admin">
  <h1>Settings!</h1>
    <!-- <div class="heading">
      <div class="circle">1</div>
      <h2>What Do You Have in Your Mind?</h2>
    </div> -->
    <h3>Add up to Two Food!</h3>
    <div class="add">
      <div class="form">
        <div class ="formTwo">
          <input v-model="title" placeholder="Title">
          <textarea v-model="description" placeholder="Description"></textarea>
          <p></p>
        </div>
        <input type="file" name="photo" @change="fileChanged">
        <div class="suggestions" v-if="suggestions.length < 2">
          <button @click="upload">Upload</button>
        </div>
        <div v-else>
          <button @click="alert">Upload</button>
        </div>
      </div>
      <div class="upload" v-if="addItem">
        <h2>{{addItem.title}}</h2>
        <img :src="addItem.path" />
        <h3>{{addItem.description}}</h3>
      </div>
    </div>
    <!-- <div class="heading">
  <div class="circle">2</div>
    <h2>Edit/Delete an Item</h2>
  </div> -->
  <h3>Edit/Delete an Item</h3>

  <div class="edit">
    <div class="form">
      <input v-model="findTitle" placeholder="Search">
      <div class="suggestions" v-if="suggestions.length > 0">
        <div class="suggestion" v-for="s in suggestions" :key="s.id" @click="selectItem(s)">{{s.title}}</div>
      </div>
    </div>
    <div class="upload" v-if="findItem">
      <input v-model="findItem.title">
      <p></p>
      <img :src="findItem.path" />
      <textarea v-model="findItem.description"></textarea>
      <div class="actions" v-if="findItem">
        <button @click="deleteItem(findItem)">Delete</button>
        <button @click="editItem(findItem)">Edit</button>
      </div>
    </div>
  </div>
</div>

</template>

<script>
  import axios from 'axios';
  export default {
    name: 'Admin',
    data() {
      return {
        title: "",
        file: null,
        addItem: null,
        items: [],
        findTitle: "",
        findItem: null,
        description: "",
        likes: 0,
        comments: [],
        //ADD
        //author: "",
        //text: "",
      }
    },
    computed: {
      suggestions() {
        let items = this.items.filter(item => item.title.toLowerCase().startsWith(this.findTitle.toLowerCase()));
        return items.sort((a, b) => a.title > b.title);
      }
    },
    created() {
      this.getItems();
    },
    methods: {
      fileChanged(event) {
        this.file = event.target.files[0]
      },
      alert() {
        alert("You can add only two menu! Please delete one before you proceed.")
      },
      async upload() {
        try {
          const formData = new FormData();
          formData.append('photo', this.file, this.file.name)
          let r1 = await axios.post('/api/photos', formData);
          let r2 = await axios.post('/api/items', {
            title: this.title,
            path: r1.data.path,
            description: this.description,
            likes: this.likes,
            //index: this.index,
            comments: this.comments,
            //ADD
            //author: this.author,
            //text: this.text,
          });
          this.addItem = r2.data;
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
      selectItem(item) {
        this.findTitle = "";
        this.findItem = item;
      },
      async deleteItem(item) {
        try {
          await axios.delete("/api/items/" + item._id);
          this.findItem = null;
          this.getItems();
          return true;
        } catch (error) {
          console.log(error);
        }
      },
      async editItem(item) {
        try {
          await axios.put("/api/items/" + item._id, {
            title: item.title,
            description: item.description,
            likes: item.likes,
            index: item.index,
            comments: item.comments,
            //author: item.author,
            //text: item.text,
          });
          this.findItem = null;
          this.getItems();
          return true;
        } catch (error) {
          console.log(error);
        }
      },
    }
  }
</script>

<style scoped>

h3 {
  font-size: 25px;
}

.admin {
  margin: 0 5em 1.5em 5em;
}
.image h2 {
  font-style: italic;
  font-size: 1em;
}

.heading {
  display: flex;
  margin-bottom: 20px;
  margin-top: 20px;
}

.heading h2 {
  margin-top: 8px;
  margin-left: 10px;
}

.add,
.edit {
  display: flex;
}

/* .circle {
  border-radius: 50%;
  width: 18px;
  height: 18px;
  padding: 8px;
  background: #333;
  color: #fff;
  text-align: center
} */

/* Form */
input,
textarea,
select,
button {
  font-family: 'Indie Flower', cursive;
  font-size: 1em;
  margin-bottom: 5pt;
  margin-top: 5pt;
}

.form {
  margin-right: 50px;
}

/* Uploaded images */
.upload h2 {
  margin: 0px;
}

.upload img {
  max-width: 300px;
}

/* Suggestions */
.suggestions {
  width: 200px;
  border: 1px solid #ccc;
}

.formTwo {
  display: flex;
  flex-direction: column;
}

.suggestion {
  min-height: 20px;
}

.suggestion:hover {
  background-color: #5BDEFF;
  color: #fff;
}

.upload {
  display: flex;
  flex-direction: column;
}

h1 {
  margin: 0 0 0 0;
  padding-top: 20px;
}
textarea {
   border: 1px solid #ccc;
}
</style>
