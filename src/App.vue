<script setup>

import CalendarComponent from "@/components/CalendarComponent.vue";
import MenuComponent from "@/components/MenuComponent.vue";
import {onMounted, ref} from "vue";

const selectedLanguage = ref(0);
const selectedDate = ref("");

const handleDateSelected = (date) => {
  selectedDate.value = date.toLocaleDateString();
};

onMounted(()=>{
  const isToday = new Date();
  selectedDate.value = isToday.toLocaleDateString();
})
</script>

<template>
  <menu-component
      v-model:selectedLanguage="selectedLanguage"
      v-model:date="selectedDate"/>
  <calendar-component
      :language="selectedLanguage"
      @dateSelected="handleDateSelected"/>
</template>

<style scoped>

</style>
