<template>
  <form @submit.prevent>
    <h4>Создание поста</h4>
    <MyInput
        v-model="post.title"
        type="text"
        placeholder="Название"
    />
    <MyInput
        v-model="post.body"
        type="text"
        placeholder="Описание"
    />
    <MyButton
        class="form__btn"
        @click="createPost"
    >
      Создать
    </MyButton>
  </form>
</template>

<script>
export default {
  name: "PostForm",
  data() {
    return {
      post: {
        title: '',
        body: ''
      }
    }
  },
  methods: {
    clearInputValues() {
      this.title = ''
      this.body = ''
    },
    createPost() {
      this.post.id = Date.now()
      /* Подписка на событие, чтобы отдать значение наверх, передаём созданный пост
      * @create в родителе -> 'create' в дочернем компоненте
      * */
      this.$emit('create', this.post)
      this.clearInputValues()
    }
  },
  watch: {
/*
    post: {
      handler(newValue){ // handler
        console.log(newValue)
      },
      deep: true // Для наблюдаемых свойств в объектах
    }
*/
  }
}
</script>

<style scoped>


form {
  display: flex;
  flex-direction: column;
}

.form__btn {
  align-self: flex-end;
  margin-top: 15px;
}

h4 {
  margin: 0;
}

</style>
