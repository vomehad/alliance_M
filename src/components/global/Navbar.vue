<script>

export default {
  props: {
    show: false,
  }
}
</script>

<template>
  <nav class="_active">
    <div v-show="show" class="menu_body _active">
      <ul class="menu_list">
        <li class="menu_item"><router-link to="/" class="menu_link"></router-link></li>
        <li class="menu_item"><router-link  to="/catalog" class="menu_link">Каталог</router-link></li>
        <li class="menu_item"><router-link to="/credit" class="menu_link">Кредит</router-link></li>
        <li class="menu_item"><router-link to="/about" class="menu_link">О компании</router-link></li>
        <li class="menu_item"><router-link to="/contacts" class="menu_link">Контакты</router-link></li>
      </ul>
    </div>
  </nav>
</template>

<style scoped>

</style>