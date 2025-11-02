<script setup>
const props = defineProps({
  selectedLanguage: Number,
  date: String
});
const emits = defineEmits(['update:selectedLanguage', 'update:date']);

const updateLanguage = (event) => {
  console.log(event.target.value)
  emits('update:selectedLanguage', Number(event.target.value))
};
</script>

<template>
<div class="menu">
  <div class="menu__select-language">
    <label for="selectLanguage">{{ props.selectedLanguage === 0 ? 'Выберите язык' : 'Select language' }}</label>
    <select name="selectLanguage" id="selectLanguage" :value="props.selectedLanguage" @change="updateLanguage">
      <option value="0">Русский</option>
      <option value="1">Английский</option>
    </select>
  </div>
  <div class="menu__input-date">
    <label for="date">{{ props.selectedLanguage === 0 ? 'Дата' : 'Date' }}</label>
    <input type="text" id="date" :value="props.date" readonly>
  </div>
</div>
</template>

<style scoped>
.menu{
  margin-bottom: 25px;
  display: flex;
  justify-content: space-between;
}
label{
  display: block;
}
select, input{
  background: white;
  outline: none;
  border: 1px solid #ccc;
}
</style>