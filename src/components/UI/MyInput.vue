<template>
<!--
  :value - значение инпута
  @input - событие
 -->
  <input
      :value="modelValue"
      @input="updateInput"
      type="text"
      class="input"
  >
</template>

<script>
export default {
  name: "MyInput",
  /*
   modelValue - сама модель для 2х-стороннего связывания для обработки событий
  */
  props: {
    modelValue: [String, Number]
  },
  methods: {
    /*
    * update:modelValue - указываем какую модель обновляем
    * event.target.value - указываем на что обновляем
    * */
    updateInput(event){
      this.$emit('update:modelValue',event.target.value)
    }
  }
}
</script>

<style scoped>
.input {
  width: -webkit-fill-available;
  margin-top: 15px;
  padding: 10px 15px;
  border: 1px solid teal;
}
</style>
