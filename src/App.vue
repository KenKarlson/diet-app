<template>
  <div id="app">
    <div class="container">
      <h1>Составление диеты</h1>

      <!-- Форма для ввода данных -->
      <form @submit.prevent="addFoodItem">
        <label for="foodItem">Продукт:</label>
        <input type="text" id="foodItem" v-model="newFoodItem" placeholder="Введите продукт" required>

        <label for="calories">Калории:</label>
        <input type="number" id="calories" v-model="newCalories" placeholder="Введите калории" required>

        <button type="submit">Добавить в диету</button>
      </form>

      <!-- Список добавленных продуктов -->
      <h2>Ваша диета:</h2>
      <ul>
        <FoodItem v-for="(item, index) in dietItems" :key="index" :item="item" @remove="removeFoodItem(index)" />
      </ul>

      <!-- Общее количество калорий -->
      <p>Общее количество калорий: <span class="total-calories">{{ totalCalories }}</span></p>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  components: {}
}
</script>

<style></style>
