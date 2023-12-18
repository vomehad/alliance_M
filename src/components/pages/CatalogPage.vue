<script>
import CarInfo from "../CarInfo.vue";
import FilterCars from "../FilterCars.vue";
import axios from "axios";
import { API_URL } from "../../main";
import CreditForm from '../CreditForm.vue';
import PopupSubmit from '../PopupSubmit.vue';
import PopupWin from '../PopupWin.vue';

export default {
  components: {
    CarInfo,
    FilterCars,
    CreditForm,
    PopupSubmit,
    PopupWin,
  },
  data() {
    CreditForm
    return {
      pageName: 'Каталог',
      cars: [],
      params: {},
      page: {
        current: 0,
        last: 0,
      },
      key: 0,
      app: {
        phone: '9182599393'
      },
      modal: false,
      modalSuccess: false,
      reserved_car: {},
    }
  },
  methods: {
    showModal(car) {
      this.modal = !this.modal;
      this.reserved_car = car;
    },
    showModalSuccess() {
      this.modalSuccess = true;
      setTimeout(() => this.modalSuccess = false, 3000)
    },
    async getCarList(data) {
      if (data?.brandList?.length) {
        this.params.brand = this.setBrand(data);
      }

      if (data?.modelList?.length) {
        this.params.model = this.setModel(data);
      }

      if (this.hasOption(data, 'year')) {
        this.params.year = this.setYear(data);
      }

      if (this.hasOption(data, 'mileage')) {
        this.params.mileage = this.setMileage(data);
      }

      if (this.hasOption(data, 'wheel_drive')) {
        this.params.wheel_drive = this.setWheelDrive(data);
      }

      if (this.hasOption(data, 'kpp')) {
        this.params.kpp = this.setKpp(data);
      }

      if (this.hasOption(data, 'fuel')) {
        this.params.fuel = this.setFuel(data);
      }

      this.params.page = 1;
      try {
        const response = await axios.get(`${API_URL}/api/car/list`, {params: this.params});
        const { data: { data: list, current_page, last_page } } = response;

        this.cars = list;
        this.page.current = current_page;
        this.page.last = last_page;
      } catch (e) {
        console.log(e)
      }
    },
    hasOption(data, property) {
      if (data !== undefined && data[property] !== undefined) {
        Object.keys(data[property])
            .filter(key => !data[property][key])
            .forEach(key => delete data[property][key]);
        return !!Object.values(data[property]).length;
      } else {
        return false;
      }
    },
    setBrand : (data) => data.brandList.join(","),
    setModel: (data) => data.modelList.join(","),
    setYear: (data) => {
      let params = '';

      if (data.year?.from) {
        params = `${data.year.from},`;
      }

      if (data.year?.to) {
        params = params.length ? (params + data.year.to) : `,${data.year.to}`;
      }

      return params;
    },
    setMileage: (data) => {
      let params = '';

      if (data.mileage?.from) {
        params += data.mileage.to;
      }

      if (data.mileage?.to) {
        params = params.length ? (params + data.mileage.to) : `,${data.mileage.to}`;
      }

      return params;
    },
    setWheelDrive: (data) => {
      let params = [];

      params.push(data.wheel_drive?.fwd ? 'fwd' : '');
      params.push(data.wheel_drive?.rwd ? 'rwd' : '');
      params.push(data.wheel_drive?.awd ? 'awd' : '');

      return params.join(',');
    },
    setKpp: (data) => {
      let params = [];

      params.push(data.kpp?.dct ? 'dct' : '');
      params.push(data.kpp?.akpp ? 'akpp' : '');
      params.push(data.kpp?.mkpp ? 'mkpp' : '');
      params.push(data.kpp?.cvt ? 'cvt' : '');

      return params.join(',');
    },
    setFuel: (data) => {
      let params = [];

      params.push(data.fuel?.benzin ? 'benzin' : '');
      params.push(data.fuel?.electro ? 'electro' : '');
      params.push(data.fuel?.diesel ? 'diesel' : '');
      params.push(data.fuel?.hybrid ? 'hybrid' : '');
      params.push(data.fuel?.hbo ? 'hbo' : '');

      return params.join(',');
    },
    async getMoreCar() {
      try {
        this.params.page = (this.page.current < this.page.last) ? this.page.current + 1 : this.page.current;
        const response = await axios.get(`${API_URL}/api/car/list`, {params: this.params});
        const { data: { data: list, current_page, last_page }, } = response;

        this.cars = [...this.cars, ...list];
        this.page.current = current_page;
        this.page.last = last_page;
        this.key++;
      } catch (e) {
        console.log(e)
      }
    },
    async clear() {
      try {
        this.params = {};
        this.params.page = this.page.current = 1;
        const response = await axios.get(`${API_URL}/api/car/list`, {params: this.params});
        const { data: { data: list, current_page, last_page }, } = response;

        this.cars = list;
        this.page.current = current_page;
        this.page.last = last_page;
        this.key++;
      } catch (e) {
        console.log(e)
      }
    },
    isMorePages() {
      return this.page.current !== this.page.last;
    },
    async getPageCar (page) {
      try {
        this.params.page = page;
        const response = await axios.get(`${API_URL}/api/car/list`, {params: this.params});
        const { data: { data: list, current_page, last_page }, } = response;

        this.cars = list;
        this.page.current = current_page;
        this.page.last = last_page;
        this.key++;
      } catch (e) {
        console.log(e)
      }
    },
    async getAppNumber() {
      try {
        const response = await axios.get(`${API_URL}/api/number/app`);
        const { data: { number } } = response;
        this.app.phone = number;
      } catch (e) {
        console.log(e);
      }
    },
  },
  mounted() {
    this.getCarList();
    this.getAppNumber();
  }
}
</script>

<template>
  <main class="main">
    <div class="wrapper">
      <MyBreadcrumbs :page="pageName" />
      <div class="cars catalog-cars">
        <h1 class="title">каталог</h1>
        <div class="select-filter">
          <div class="flex items-center">
            <div id="select-catalog"></div>
          </div>
          <a href="#popup" class="popup_btn popup-link">
            <img src="@img/icons/filter-filter.svg" alt="Filter">
          </a>
        </div>
        <div class="cars_container">
          <div class="cars_buy car catalogCars">
            <ul class="car_list" :key="key">
              <CarInfo v-for="car in cars" :key="car.id" :car="car" :app="app" @show-modal="showModal"/>
              <li class="zero_item" v-show="cars.length === 0"><span>По данным параметрам поиска, нету подходящих автомобилей</span></li>
            </ul>
          </div>
          <FilterCars @get-cars="getCarList" @clear="clear" :params="params"/>
        </div>
      </div>

      <div class="watch pagination-watch">
        <div
            v-if="isMorePages()"
            class="watch-link"
            id="show-more"
        >
          <button
              type="button"
              class="btn-watch"
              @click="getMoreCar"
          >Посмотреть еще</button>
        </div>
      </div>

      <div class="pagination">
        <div
            v-for="number in page.last"
            :key="number"
            :class="{ 'pagination-prev': (number === 1), 'pagination-next': (number === page.last) }"
        >
          <img
              v-if="number === 1"
              src="@img/icons/arrows-directions-left.svg"
              alt="Left"
          />
          <span
              class="pagination-link"
              :class="{ 'pagination-active': (number === page.current) }"
              @click="getPageCar(number)"
          >{{ number }}</span>
          <img
              v-if="number === page.last"
              src="@img/icons/arrows-directions-right.svg"
              alt="Right"
          />
        </div>
      </div>

     <CreditForm/>
     <PopupSubmit
          v-if="modal"
          @modal="showModal"
          :car="reserved_car"
          @modal-success="showModalSuccess"
      />
      <PopupWin v-if="modalSuccess" />
    </div>

  </main>

</template>

<style scoped>
.pagination-link {
  cursor: pointer;
}
</style>