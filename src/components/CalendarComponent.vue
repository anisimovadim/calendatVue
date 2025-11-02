<script setup>
import { ref, computed } from 'vue';

const props = defineProps({
  language: Number
})
const emits = defineEmits(['dateSelected']);

const weeks = ref(['Вс', 'Пн', 'Вт', 'Ср', 'Чт', 'Пт', 'Сб']);
const weeksEn = ref(['Sun', 'Mon', 'Tue', 'Wed', 'Thu', 'Fri', 'Sat']);

const usedWeeks = computed(() => props.language === 0 ? weeks.value : weeksEn.value);

const currentDate = ref(new Date());
const formattedDate = computed(() =>
    currentDate.value.toLocaleString(props.language === 0 ? 'ru-RU' : 'en-US', { month: 'short', year: 'numeric' })
)

const getCalendarDays = (year, month)=>{
  const firstDay = new Date(year, month, 1).getDay();
  const daysInMonth = new Date(year, month + 1, 0).getDate();

  const days = [];

  for (let i = 0; i < firstDay; i++) {
    days.push(null);
  }

  for (let d = 1; d <= daysInMonth; d++) {
    days.push(new Date(year, month, d));
  }

  return days;
}

const calendarDays = computed(() => {
  const year = currentDate.value.getFullYear();
  const month = currentDate.value.getMonth();
  return getCalendarDays(year, month);
})

const onPrevMonth = () => {
  currentDate.value = new Date(
      currentDate.value.getFullYear(),
      currentDate.value.getMonth() - 1,
      1
  )
}
const onNextMonth = () => {
  currentDate.value = new Date(
      currentDate.value.getFullYear(),
      currentDate.value.getMonth() + 1,
      1
  )
}

const today = new Date();
const selectedDate = ref(null);

const isToday = (date) => {
  return (
      date &&
      date.getDate() === today.getDate() &&
      date.getMonth() === today.getMonth() &&
      date.getFullYear() === today.getFullYear()
  )
}

const onSelectDate = (date)=>{
  console.log(date);
  selectedDate.value = date;
  emits('dateSelected', date);
}

const isSelectedDay = (date) => {
  return selectedDate.value &&
      date &&
      date.getDate() === selectedDate.value.getDate() &&
      date.getMonth() === selectedDate.value.getMonth() &&
      date.getFullYear() === selectedDate.value.getFullYear();
}
</script>

<template>
  <div class="calendar">
    <div class="calendar__header">
      <button class="prev" type="button" @click="onPrevMonth">‹</button>
      <span class="month">{{ formattedDate }}</span>
      <button class="next" type="button" @click="onNextMonth">›</button>
    </div>

    <div class="calendar__table">
      <table class="table">
        <thead>
        <tr>
          <th v-for="day in usedWeeks" :key="day">{{ day }}</th>
        </tr>
        </thead>
        <tbody>
        <tr v-for="weekIndex in Math.ceil(calendarDays.length / 7)" :key="weekIndex">
          <td
              v-for="dayIndex in 7"
              :key="dayIndex"
              class="day"
              @click="onSelectDate(calendarDays[(weekIndex - 1) * 7 + dayIndex - 1])"
              :class="{
                today: isToday(calendarDays[(weekIndex - 1) * 7 + dayIndex - 1]),
                isSelectDay: isSelectedDay(calendarDays[(weekIndex - 1) * 7 + dayIndex - 1])
              }"
          >
              <span v-if="calendarDays[(weekIndex - 1) * 7 + dayIndex - 1]">
                {{ calendarDays[(weekIndex - 1) * 7 + dayIndex - 1].getDate() }}
              </span>
          </td>
        </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<style scoped>
.calendar{
  width: 280px;
  margin: auto;
  border: 1px solid #ccc;
  padding: 10px;
}
.calendar__header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 10px;
}
button {
  background: none;
  border: none;
  cursor: pointer;
  font-size: 18px;
}
.table {
  width: 100%;
  border-collapse: collapse;
  text-align: center;
}
th, td {
  width: 14%;
  height: 36px;
}
.today {
  border: 1px solid deepskyblue;
}
.isSelectDay {
  background-color: rgba(245, 7, 43, 0.98);
}
.day{
  cursor: pointer;
}
</style>
