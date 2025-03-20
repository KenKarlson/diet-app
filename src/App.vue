<template>
  <div class="container">
    <h1>Составление диеты</h1>

    <!-- Форма для ввода данных -->
    <form @submit.prevent="addFoodItem">
      <label for="foodItem">Продукт:</label>
      <input type="text" id="foodItem" v-model="newFoodItem" placeholder="Введите продукт" required />

      <label for="calories">Калории:</label>
      <input type="number" id="calories" v-model="newCalories" placeholder="Введите калории" required />

      <button type="submit">Добавить в диету</button>
    </form>

    <!-- Список добавленных продуктов -->
    <h2>Ваша диета:</h2>
    <ul>
      <Food-component v-for="(item, index) in dietItems" :key="index" :item="item" @remove="removeFoodItem(index)">
      </food-component>
    </ul>

    <!-- Общее количество калорий -->
    <p>Общее количество калорий: <span class="total-calories">{{ totalCalories }}</span></p>

    <!-- Календарь -->
    <!--Заменить на диаграмму дат каллорий в разрезе недели-->
    <h2>Календарь калорий</h2>
    <div>
      <Calendar :events="calendarEvents" @day-click="handleDayClick" />
    </div>
  </div>
</template>

<script>
import FoodComponent from './components/FoodComponent.vue';
import Calendar from 'vue-calendar-component';

export default {
  components: {
    FoodComponent,
    Calendar,
  },
  data() {
    return {
      newFoodItem: '',
      newCalories: 0,
      dietItems: [],
      calendarEvents: [], // События для календаря
      selectedDate: null, // Выбранная дата
    };
  },
  computed: {
    totalCalories() {
      return this.dietItems.reduce((sum, item) => sum + item.calories, 0);
    },
  },
  methods: {
    addFoodItem() {
      if (this.newFoodItem.trim() && this.newCalories > 0) {
        const foodItem = {
          name: this.newFoodItem,
          calories: parseInt(this.newCalories),
          date: this.selectedDate || new Date().toISOString().split('T')[0], // Используем выбранную дату или текущую
        };

        this.dietItems.push(foodItem);

        // Добавляем событие в календарь
        this.addCalendarEvent(foodItem);

        this.newFoodItem = '';
        this.newCalories = 0;
      }
    },
    removeFoodItem(index) {
      this.dietItems.splice(index, 1);
      this.updateCalendarEvents();
    },
    handleDayClick(date) {
      this.selectedDate = date.toISOString().split('T')[0]; // Устанавливаем выбранную дату
    },
    addCalendarEvent(foodItem) {
      const event = {
        date: foodItem.date,
        title: `${foodItem.name}: ${foodItem.calories} ккал`,
        class: 'calories-event',
      };
      this.calendarEvents.push(event);
    },
    updateCalendarEvents() {
      this.calendarEvents = this.dietItems.map((item) => ({
        date: item.date,
        title: `${item.name}: ${item.calories} ккал`,
        class: 'calories-event',
      }));
    },
  },
};
</script>

<style scoped>
.container {
  max-width: 800px;
  margin: 0 auto;
}

.calories-event {
  background-color: #28a745;
  color: white;
  border-radius: 4px;
  padding: 2px 5px;
}
</style>