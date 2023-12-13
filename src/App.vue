<script setup>

</script>

<template>
  <div class="buffer"></div>
  <MyHeader/>
  <RouterView />
  <MyFooter/>
</template>

<style>
  .buffer {
    height: 6rem;
    width: 100%;
    @media screen and (max-width: 900px) {
      height: 5rem;
    }
  }
</style>
