<template>
  <div class="modal-field">
    <label :for="forId">{{label}}</label>
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: 'ContactModalField',
  props: ['label', 'forId']
}
</script>

<style scoped>
.modal-field {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  margin: 10px 0;
}
</style>
