<script setup>
import Calendar from "./components/Calendar.vue";
import {ref} from "vue";

const selectedDate = ref('2025-11-03');
const selectedLocale = ref('en');
const text = ref('Date not changed yet');

</script>

<template>
  <h1>Calendar component demonstration</h1>
  <div class="controls">
    <div>Date in format 2025-11-03</div>
    <input v-model="selectedDate" type="text" placeholder="2025-11-03"/>
    <div>Locale</div>
    <select v-model="selectedLocale">
      <option value="en">English</option>
      <option value="ru">Русский</option>
    </select>
    <p>{{ text }}</p>
    <p>Selected date: {{ selectedDate }}</p>
  </div>

  <Calendar v-model="selectedDate"
            :locale="selectedLocale"
            @date-selected="text = 'Date changed to ' + $event"/>
</template>

<style>
body {
  font-family: Arial, sans-serif;
  padding: 0 15px;
}

.controls {
  display: grid;
  grid-template-columns: auto 1fr;
  gap: 10px;
  justify-items: start;
  margin-bottom: 15px;
}

</style>
