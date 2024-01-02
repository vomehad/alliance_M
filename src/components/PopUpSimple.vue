
<script>
import axios from 'axios'
import { API_URL } from '../main'
import PopupWin from './PopupWin.vue'
export default {
    name: "PopUpSimple",
    data() {
        return {
            name: '',
            number: '',
            showPopUpWin:false
        };
    },
    props:{
      showPopUp: false
    },
    methods: {
        async onSubmit() {
          try{
            const res = await axios.post(`${API_URL}/api/mail/application/credit`, {
                    name: this.name, number: this.number
            });
            if (res.status === 200) {
                this.showPopUp = false;
                this.showPopUpWin = true
                setTimeout(()=>{
                  this.showPopUpWin = false;
                },3000)
            }
            else {
                console.log('err');
            }}catch(e){
              console.log(e);
          }
        }
          
            
    },
    components: { PopupWin }
}
</script>

<template>
  <div v-if="showPopUp" class="form simple-form" @submit.prevent="onSubmit()">
    <div class="form_container">
      <form class="form_action">
        <input required type="text" name="name" id="name" v-model="name" class="inputHome-1" placeholder="Имя">
        <input required type="tel" name="tel" id="tel" v-model="number" class="inputHome-2" placeholder="+7 (123) 456-78-90">
        <input type="submit" class="submitRequiredFormHome" name="submitForm" id="submitForm" value="Отправить заявку">
      </form>
    </div>
  </div>
  <PopupWin v-if="showPopUpWin"/>
</template>

<style scoped>
.simple-form {
  position: absolute;
  top: 25%;
  left: 30%;
  display: grid;
  justify-content: center;
  z-index: 100;
  background: #000;
  @media (max-width:600px) {
    left: 20%;
    top: 30vh;
  }
}
</style>