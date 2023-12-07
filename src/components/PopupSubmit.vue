<script>
import axios from 'axios';
import {API_URL} from "../main";

export default {
  name: "PopupSubmit",
  data(){
    return {
      name: "",
      phone: "",
    }
  },
  props: {
    car: {}
  },
  watch: {
    tel() {
      console.log('hui')
      return this.phone;
    }
  },
  methods: {
    async sendFormData() {
      try {
        const data = {name: this.name, number: this.phone};
        const res = await axios.post(`${API_URL}/api/mail/application/${this.car.id}`, data)
        const {data: {data: {success, message}}} = res;

        if (success) {
          this.$emit('modal'); // закрываем форму
          this.$emit('modal-success'); // вызываем модалку с успехом
        }
      } catch(e) {
        console.log(e.response.data.message);
      }
    },
    close() {
      this.$emit('modal');
    },
    carName() {
      const configuration = this.car.configuration;
      const model = configuration.model;
      const brand = model.brand

      return `${brand.name} ${model.name} ${configuration.name}`;
    }
  }
}

</script>

<template>
  <div class="popup">
    <div class="popup__body">
      <div class="popup__content deletePadding">
        <div class="submit">
          <div class="submit_container">
            <div class="flex justify-between items-center">
              <h2 class="submit_title">Оставить заявку</h2>
              <button
                  @click="close"
                  class="close-popup close-modal"
              >
                <svg width="40" height="40" viewBox="0 0 40 40" fill="none" xmlns="http://www.w3.org/2000/svg">
                  <path d="M30 10L10 30" stroke="white" stroke-width="1.1" stroke-linecap="round" stroke-linejoin="round"/>
                  <path d="M10 10L30 30" stroke="white" stroke-width="1.1" stroke-linecap="round" stroke-linejoin="round"/>
                </svg>
              </button>
            </div>
            <p v-if="car">на авто: {{ carName() }}</p>
            <form class="submit_form" @submit.prevent="sendFormData">
              <input
                  required
                  minlength="2"
                  placeholder="ФИО"
                  v-model="name"
                  class="input top"
              />
              <input
                  required
                  type="tel"
                  v-model="phone"
                  placeholder="+7 (___) ___-__-__"
                  v-mask="['+7 (###) ### ##-##']"
                  class="input bottom"
              />
              <div class="btn-link popup-link w-100">
                <button class="btn zayavka-btn w-100">Отправить</button>
              </div>
            </form>
            <p>Нажимая на кнопку “Отправить”, вы даете согласие на обработку персональных данных</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
  .input {
    border-radius: 8px;
    border: 1px solid rgba(255, 255, 255, 0.6);
    background: #F4F6F8;
    display: -webkit-box;
    display: -ms-flexbox;
    display: flex;
    height: 48px;
    width: 100%;
    padding: 16px 20px;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
    -ms-flex-direction: column;
    flex-direction: column;
    -webkit-box-pack: center;
    -ms-flex-pack: center;
    justify-content: center;
    -webkit-box-align: center;
    -ms-flex-align: center;
    align-items: center;
    -ms-flex-item-align: stretch;
    -ms-grid-row-align: stretch;
    align-self: stretch;
    outline: none;
  }

  .top {
    margin: 0px 0px 1rem 0px;
  }

  .bottom {
    margin: 0px 0px 24px 0px;
  }
</style>