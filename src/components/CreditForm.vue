
<script>
import axios from 'axios'
import { API_URL } from '../main'
import PopupWin from './PopupWin.vue'
export default {
    name: "CreditForm",
    data() {
        return {
            name: '',
            number: '',
            showPopUp: false
        };
    },
    methods: {
        async onSubmit() {
          try{
            const res = await axios.post(`${API_URL}/mail/application/credit`, {
                    name: this.name, phone: this.number
            });
            if (res.status === 200) {
                this.showPopUp = !this.showPopUp;
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
  <div class="form" @submit.prevent="onSubmit">
    <div class="form_container">
      <div class="form_title">
        <h1>хочешь</h1>
        <p>оформить автокредит легко и просто?</p>
      </div>
      <form class="form_action">
        <input required type="text" name="name" id="name" v-model="name" class="inputHome-1" placeholder="Имя">
        <input required type="tel" name="tel" id="tel" v-model="number" class="inputHome-2" placeholder="+7 (123) 456-78-90">
        <input type="submit" class="submitRequiredFormHome" name="submitForm" id="submitForm" value="Отправить заявку">
      </form>
    </div>
  </div>
  <PopupWin v-if="showPopUp"/>
</template>

<style scoped>

</style>