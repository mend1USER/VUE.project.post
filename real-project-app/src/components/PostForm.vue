<template>
   <form @submit.prevent>
    <h4>Создание поста</h4>

    <my-input v-model="post.title"  type="text" placeholder="Название"/>
    <my-input v-model="post.body" type="text" placeholder="Описание"/>
    <my-button class="btn" style="align-self: flex-end; margin-top: 15px;" @click="createPost" >Создать</my-button>
</form>
</template>

<script>
import MyButton from "@/components/UI/MyButton.vue";
import MyInput from "@/components/UI/MyInput.vue";

export default {
  components: {MyInput, MyButton},
  data() {
    return {
      post: {
        title: '',
        body: ''
      }
    }
  },
   methods: {
    createPost() {
      this.post.id = Date.now();
      this.$emit('create', this.post)
      this.post = {
        title: '',
        body: ''
      }
    }
   }
}
</script>

<style scoped>
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
</style>