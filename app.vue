<template>
  <div class="app">
    <HeaderApp />
    <main class="main">
      <BreadCrumb />
      <CardList @update:selectCategory = "categoryChange"/>
      <CardApp :selectedCategory="selectedCategory"/>
      <FeedbackApp />
    </main>
    <FooterApp />
  </div>
</template>

<script setup> 
  import  {ref, watch} from 'vue'

  const selectedCategory = ref(null)

  const categoryChange = (categoryId) => {
    selectedCategory.value = categoryId
    console.log('Изменение значения selectCategory')
  };

  watch(selectedCategory, (newValue) => {
    console.log('selectedCategory изменился:', newValue);
});
  
</script>

<style lang="scss">
@import url('./assets/style/global.scss');
</style> 