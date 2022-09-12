<template>
  <div
      class="modal"
      v-if="show"
      @click="hideModalWindow"
  >
    <div
        @click.stop
        class="modal-content">
      <slot/>
    </div>
  </div>
</template>

<script>
export default {
  name: "MyModalWindow",
  props: {
    show: {
      type: Boolean,
      default: false
    }
  },
  methods: {
    hideModalWindow() {
      this.$emit('update:show',false)
    }
  }
}
</script>

<style scoped>
.modal {
  display: flex;
  position: fixed;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  background: rgba(0, 0, 0, 0.5);
}

.modal-content {
  padding: 20px;
  margin: auto;
  background: white;
  border-radius: 12px;
  min-height: 50px;
  min-width: 300px;
}
</style>
