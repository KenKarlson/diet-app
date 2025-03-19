<template>
  <li>
    <span>{{ item.name }}</span>
    <span>{{ item.calories }} ккал</span>
    <button @click="removeItem">Удалить</button>
  </li>
</template>

<script>
export default {
  props: {
    item: {
      type: Object,
      required: true,
    },
  },
  methods: {
    removeItem() {
      this.$emit('remove');
    },
  },
};
</script>

<style scoped>
li {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: #f8f9fa;
  padding: 10px;
  margin-bottom: 5px;
  border-radius: 4px;
}

button {
  background-color: #dc3545;
  color: #fff;
  border: none;
  border-radius: 4px;
  padding: 5px 10px;
  cursor: pointer;
}

button:hover {
  background-color: #c82333;
}
</style>