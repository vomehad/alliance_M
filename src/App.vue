<script setup>
import {onMounted } from 'vue'
let iconType = false
const iconObserv = () => {
 const observer = new IntersectionObserver(
      entry => {
       if(entry.isIntersecting){
         
         iconType = true
         console.log(iconType)
       }
       else {
        iconType = false
         console.log(iconType)
       }
   },
   {
     root: null,
     rootMargin: "0px",
     threshold: 0.8,
   }
 )
 observer.observe(document.querySelector('.buffer') || null)
}

onMounted(iconObserv)
</script>

<template>
  <div class="buffer"></div>
  <MyHeader :iconType="iconType"/>
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
