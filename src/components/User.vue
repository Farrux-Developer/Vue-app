<script>
export default {
  props: {
    user: { type: Object, required: true },
    index: { type: Number, required: true },
    deleteUser: {
      type: Function,
      required: true,
    },
  },
};
</script>

<template>
  <div>
    <h1>Users {{ index + 1 }}</h1>
    <p>Name: {{ user.name }}</p>
    <p>Email: {{ user.email }} - {{ user.password }}</p>
    <button @click="deleteUser(index)">Delete</button>
    <button @click="$emit('edit', index)">Change</button>
  </div>
</template>

<style scoped></style>
