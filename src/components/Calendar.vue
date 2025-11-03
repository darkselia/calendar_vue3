<script setup>
import {computed, ref, watch} from "vue";

const modelValue = defineModel({
  type: String,
  default: new Date().toISOString().slice(0, 10)
});

const {locale} = defineProps({
  locale: {
    type: String,
    default: 'en',
    validator(value, props) {
      return ['en', 'ru'].includes(value);
    }
  }
});

const numberOfDaysInMonth = computed(() => {
  const year = displayedDate.value.getFullYear();
  const month = displayedDate.value.getMonth();
  return new Date(year, month + 1, 0).getDate();
});
const firstDayOfMonth = computed(() => {
  const year = displayedDate.value.getFullYear();
  const month = displayedDate.value.getMonth();
  let firstDayOfMonth = new Date(year, month, 1).getDay();
  if (locale === 'ru') {
    firstDayOfMonth--;
    if (firstDayOfMonth < 0) {
      firstDayOfMonth = 6;
    }
  }
  return firstDayOfMonth;
});

const displayedDate = ref(new Date(modelValue.value));
watch(() => modelValue.value, (newValue) => {
  displayedDate.value = new Date(newValue);
});

const monthNames = {
  'en': [
    'Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun',
    'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec'
  ],
  'ru': [
    'Янв', 'Февр', 'Март', 'Апр', 'Май', 'Июнь',
    'Июль', 'Авг', 'Сент', 'Окт', 'Нояб', 'Дек'
  ]
};

const weekDays = {
  'en': ['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat'],
  'ru': ['Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб', 'Вс']
};

function updateDateMonth(delta) {
  displayedDate.value.setMonth(displayedDate.value.getMonth() + delta);
  displayedDate.value = new Date(displayedDate.value);
}

function updateDate(currentDay) {
  displayedDate.value.setDate(currentDay);
  modelValue.value = displayedDate.value.toISOString().slice(0, 10);
}

function checkCurrentDay(currentDay) {
  const mv = new Date(modelValue.value);
  return currentDay === mv.getDate() &&
      displayedDate.value.getMonth() === mv.getMonth() &&
      displayedDate.value.getFullYear() === mv.getFullYear();
}
</script>

<template>
  <div class="calendar">
    <div class="header">
      <button class="button-clear arrows" @click="updateDateMonth(-1)">
        &ltrif;
      </button>
      <div>
        {{ monthNames[locale][displayedDate.getMonth()] }} {{ displayedDate.getFullYear() }}
      </div>
      <button class="button-clear arrows" @click="updateDateMonth(+1)">
        &rtrif;
      </button>
    </div>
    <div class="body">
      <div v-for="weekDay in weekDays[locale]" class="weeks-row">{{ weekDay }}</div>
      <div v-if="firstDayOfMonth!==0" :style="{
        'grid-column': `span ${firstDayOfMonth}`
      }"/>
      <button v-for="i in numberOfDaysInMonth"
              :class="{'selected-day': checkCurrentDay(i)}"
              class="button-clear button-days"
              @click="updateDate(i)">
        {{ i }}
      </button>
    </div>
  </div>
</template>

<style scoped>
.calendar {
  display: flex;
  flex-direction: column;
  width: 250px;
  border: 1px solid #ccc;
  padding: 10px;
  gap: 14px;
}

.button-clear {
  background: none;
  border: 1px solid transparent;
  cursor: pointer;
  padding: 5px 0;
}

.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.arrows {
  font-size: 24px;
  line-height: 1;
  padding: 0;
}

.body {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
  text-align: center;
  gap: 2px;
}

.weeks-row{
  margin-bottom: 5px;
  font-size: 12px;
}

.selected-day, .button-days:hover {
  border-color: rebeccapurple;
}

</style>