<template>
  <div class="App">
    <h1>Страница с постами</h1>
    <MyInput
      v-model="searchQuery"
      placeholder="Поиск..."
    />
    <div class="app_btns">
      <MyButton
          @click="showModalWindow"
      >
        Создать
      </MyButton>
      <MySelect
          v-model="selectedSort"
          :options="sortOptions"
      />
    </div>
    <MyModalWindow v-model:show="isModalWindowVisible">
      <PostForm
          @create="createPost"
      />
    </MyModalWindow>
    <PostList
        v-if="!isPostsLoading"
        :posts="sortedAndSearchedPosts"
        @remove="removePost"
    /> <!--:posts - название пропса-->
    <div v-else>Идёт загрузка...</div>
<!--  Вариант с постраничной пагинацией 1/4
      <div class="page__wrapper">
      <div
          v-for="pageNumber in totalPages"
          :key="pageNumber"
          class="page"
          :class="{
            'current-page': page === pageNumber
          }"
          @click="changePage(pageNumber)"
      >
        {{pageNumber}}
      </div>
    </div>
    -->
  </div>
</template>

<!--Описываем логику, создаем функции и т.п.-->
<script>
import PostForm from "@/components/PostForm"; // Импорт компонентов извне
import PostList from "@/components/PostList";
import MyButton from "@/components/UI/MyButton";
import axios from "axios";
import MySelect from "@/components/UI/MySelect";
import MyInput from "@/components/UI/MyInput";

export default {
  components: {MyInput, MySelect, MyButton, PostList, PostForm}, // Регистрируем компоненты, которые будем использовать
  data() { // Описываем модели (state)
    return {
      posts: [],
      isModalWindowVisible: false,
      isPostsLoading: false,
      selectedSort: '',
      searchQuery: '',
      page: 1,
      limit: 10,
      totalPages: 0,
      sortOptions: [
        {value: 'title', name: 'По названию'},
        {value: 'body', name: 'По описанию'}
      ]
    }
  },
  methods: { // Описываем функции-обработчики и т.п.

    createPost(post) { // post - this.post в this.$emit
      this.posts.push(post)
      this.isModalWindowVisible = false
    },
    removePost(post) {
      this.posts = this.posts.filter(({id}) => id !== post.id)
    },
    showModalWindow() {
      this.isModalWindowVisible = true
    },
    /*
      Вариант с постраничной пагинацией 2/4
      changePage(pageNumber){
        this.page = pageNumber
      },
    */


    /* Вариант с постраничной пагинацией 3/4
    * async fetchPosts() {
      try {
        this.isPostsLoading = true
        const {data,headers} = await axios.get('https://jsonplaceholder.typicode.com/posts?',{
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        })
        this.totalPages = Math.ceil(headers['x-total-count'] / this.limit)
        this.posts = data
      } catch ({message}) {
        alert(`${message}`)
      } finally {
        this.isPostsLoading = false
      }
    },
    * */
    async loadMorePosts() {
      try {
        this.isPostsLoading = true
        const {data,headers} = await axios.get('https://jsonplaceholder.typicode.com/posts?',{
          params: {
            _page: this.page,
            _limit: this.limit,
          }
        })
        this.totalPages = Math.ceil(headers['x-total-count'] / this.limit)
        // Посты не перезаписываем, а добавляем в конце списка для бесконечной ленты
        this.posts = [...this.posts, ...data]
      } catch ({message}) {
        alert(`${message}`)
      } finally {
        this.isPostsLoading = false
      }
    },
  },
  mounted() {
    // this.fetchPosts() // Вариант с постраничной пагинацией
  },
  computed: { // Вычисляемые значения (аналог useMemo). Должна что-то возвращать
    sortedPosts(){ // Чтобы не мутировать список, делаем копию
      return [...this.posts].sort((a,b) => a[this.selectedSort]?.localeCompare(b[this.selectedSort]))
    },
    sortedAndSearchedPosts(){
      return this.sortedPosts.filter(({title}) => title.toLowerCase().includes(this.searchQuery.toLowerCase()))
    }
  },
  watch: { /*
  Наблюдаемые значения (аналог useEffect with deps)
  */
   /* Вариант с постраничной пагинацией 4/4
      page(){
        this.fetchPosts()
      }
    */
  }
}
</script>

<style> // Стили, которые будет использовать компонент
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

.App {
  padding: 20px;
}

.app_btns {
  margin: 15px 10px;
  display: flex;
  justify-content: space-between;
}
.page__wrapper{
  display: flex;
  margin-top: 15px;
}
.page{
  border: 1px solid black;
  padding: 10px;
  cursor: pointer;
}
.current-page{
  border: 2px solid teal;
}

</style>
