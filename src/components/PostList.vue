<template>
  <div v-if="posts.length > 0">
    <!--  <div v-show="posts.length > 0" >-->
    <h3>Список постов</h3>
    <transition-group name="post-list">
      <PostItem
          :post="post"
          v-for="post in posts"
          :key="post.id"
          @remove="$emit('remove',post)"
      />
    </transition-group>
  </div>
  <h2 v-else>
    <!--  <h2 v-show="posts.length === 0" >-->
    Список постов пуст
  </h2>
</template>

<script>
import PostItem from "@/components/PostItem";

export default {
  name: "PostList",
  components: {PostItem},
  props: { // Пропсы
    posts: {
      type: Array, // Что ожидаем на вход
      required: true
    }
  }
}
</script>

<style scoped>
.post-list-item {
  display: inline-block;
  margin-right: 10px;
}

.post-list-enter-active,
.post-list-leave-active {
  transition: all 0.3s ease;
}

.post-list-enter-from,
.post-list-leave-to {
  opacity: 0;
  transform: translateX(130px);
}
.post-list-move {
  transition: transform 0.3s ease;
}
</style>
