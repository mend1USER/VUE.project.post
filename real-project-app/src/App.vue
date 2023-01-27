<template>
  <div class="app">
    <h1>Страница с постами</h1>
    <my-input v-model="searchQuery"/>
    <div class="app__btns">
      <my-button @click="showDialog">
        Создать публикацию
      </my-button>
      <my-select
         v-model="selectedSort"
         :options="sortOptions"
        />
    </div>
    <my-dialog v-model:show="dialogVisible">
    <post-form @create="createPost"></post-form>
    </my-dialog>
    <post-list
        :posts="sortedAndSearchedPosts"
        @remove="removePost"
        v-if="!isPostsLoading"></post-list>
  <div v-else>Идет загрузка...</div>
  </div>
  </template>

<script>
import PostForm from "@/components/PostForm.vue";
import PostList from "@/components/PostList.vue";
import MyDialog from "@/components/UI/MyDialog.vue";
import MyButton from "@/components/UI/MyButton.vue";
import axios from "axios";
import MySelect from "@/components/UI/MySelect.vue";
import MyInput from "@/components/UI/MyInput.vue";

export default {
  components: {
    MyInput,
    MySelect,
    MyButton,
    PostList, PostForm
  },
  data() {
    return {
      posts:[],
      dialogVisible: false,
      isPostsLoading: false,
      selectedSort: '',
      searchQuery: ``,
      sortOptions: [
        {value: 'title', name: 'По названию'},
        {value: 'body', name: 'По содержанию'},
      ]
    }
  },
  methods: {
    createPost(post){
       this.posts.push(post);
       this.dialogVisible = false;
    },
    removePost(post) {
      this.posts = this.posts.filter(p => p.id !== post.id)
    },
    showDialog() {
      this.dialogVisible = true;
    },
    async fetchPosts() {
       try{
         this.isPostsLoading = true;
           const response = await axios.get('https://jsonplaceholder.typicode.com/posts?_limit=10');
           this.posts = response.data;
       } catch (e) {
         alert('Error123')
       } finally {
         this.isPostsLoading = false;
       }
    }
  },
  mounted() {
    this.fetchPosts();
  },
  computed: {
    sortedPosts() {
      return[...this.posts].sort((post1, post2) => {return post1[selectedSort]?.localeCompare(post2[selectedSort])
    })
      sortedAndSearchedPosts: {
        return  this.sortedPosts.filter(post => post.title.includes(this.searchQuery))
      }
  },
  watch: {

    },
  }
}

</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
.app{
  padding: 20px;
}
.post{
  padding: 15px;
  border: 2px solid teal;
  margin-top: 15px;
}

form{
  display: flex;
  flex-direction: column;
}
.input{
  width: 100%;
  border: 1px solid teal;
  padding: 10px 15px;
  margin-top: 15px;
}

.btn{
  margin-top: 15px;
  align-self: flex-end;
  padding: 10px 15px;
  background: none;
  color: teal;
  border: 1px solid teal;
}
.app__btns{
  margin: 15px 0;
  display: flex;
  justify-content: space-between;
}
</style>