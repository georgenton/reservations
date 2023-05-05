<template>
  <header>
    <h1>Reservations</h1>
  </header>
  <div class="main">
    <div class="actions">
      <!--button @click="rewardsSwitch" class="button-blue"><span>Rewards</span></button-->
      <label class="label toggle">
        <h4>{{ rewards ? 'Rewards' : 'Regular' }}</h4>
        <input  @click="rewardsSwitch" type="checkbox" class="toggle_input" />
        <div class="toggle-control"></div>
      </label>
      <label class="label">
        Start Date
        <datepicker v-model="dateStart"></datepicker>
      </label>
      <label class="label">
        End Date
        <datepicker v-model="dateEnd"></datepicker>
      </label>
      <button @click="bestPrice" class="button-blue"><span>The best choice</span></button>
      <button @click="loadHotels" class="button-blue"><span>Reset results</span></button>
    </div>
    <ul>
      <Hotel v-for="hotel in listHotels"
             :key="hotel.id"
             :name="hotel.name"
             :image="hotel.image"
             :price="hotel.price"
             :rewards="hotel.rewards"
             :weekday="hotel.weekday"
             :rating="hotel.rating"
      ></Hotel>
    </ul>
  </div>
  <footer>
    <h4> All Rights Reserved</h4>
  </footer>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Datepicker from 'vuejs3-datepicker';
import Hotel from './components/Hotel.vue';

export default defineComponent({
  name: 'App',
  components: {
    Hotel,
    Datepicker,
  },
  data() {
    return {
      rewards: true,
      weekday: true,
      dateStart: '',
      dateEnd: '',
      listHotels: [
        {
          id: 0,
          name: '',
          price: 0,
          rating: '0.0',
          rewards: true,
          weekday: true,
        },
      ],

      dataHotels: [
        {
          id: 1,
          name: 'Lakewood',
          image: 'https://picsum.photos/id/14/300/400',
          regularWeekdayPrice: 100,
          rewardsWeekdayPrice: 80,
          regularWeekendPrice: 90,
          rewardsWeekendPrice: 80,
          rating: '3.0',
          rewards: true,
        },
        {
          id: 2,
          name: 'Bridgewood',
          image: 'https://picsum.photos/id/15/300/400',
          regularWeekdayPrice: 160,
          rewardsWeekdayPrice: 110,
          regularWeekendPrice: 60,
          rewardsWeekendPrice: 50,
          rating: '4.0',
          rewards: true,
        },
        {
          id: 3,
          name: 'Ridgewood',
          image: 'https://picsum.photos/id/16/300/400',
          regularWeekdayPrice: 220,
          rewardsWeekdayPrice: 100,
          regularWeekendPrice: 150,
          rewardsWeekendPrice: 40,
          rating: '5.0',
          rewards: true,
        },
      ],
    };
  },
  watch: {
    dateStart: {
      handler: 'dateStartChanged',
    },
    dateEnd: {
      handler: 'dateEndChanged',
    },
  },
  methods: {
    dateStartChanged() {
      if (String(this.dateStart).search('Sat') === 0 || String(this.dateStart).search('Sun') === 0) {
        this.weekday = false;
      } else {
        this.weekday = true;
      }
      this.loadHotels();
    },
    dateEndChanged() {
      if (String(this.dateEnd).search('Sat') === 0 || String(this.dateEnd).search('Sun') === 0) {
        this.weekday = false;
      } else {
        this.weekday = true;
      }
      this.loadHotels();
    },
    loadHotels() {
      this.listHotels.splice(0, this.listHotels.length);
      this.dataHotels.forEach((hotel) => {
        switch (this.rewards) {
          case true: {
            switch (this.weekday) {
              case true: {
                const tmpHotel = {
                  id: hotel.id,
                  name: hotel.name,
                  image: hotel.image,
                  price: hotel.rewardsWeekdayPrice,
                  rating: hotel.rating,
                  rewards: true,
                  weekday: true,
                };
                this.listHotels.push(tmpHotel);
                break;
              }
              case false: {
                const tmpHotel = {
                  id: hotel.id,
                  name: hotel.name,
                  image: hotel.image,
                  price: hotel.rewardsWeekendPrice,
                  rating: hotel.rating,
                  rewards: true,
                  weekday: false,
                };
                this.listHotels.push(tmpHotel);
                break;
              }
              default: {
                break;
              }
            }
            break;
          }
          case false: {
            switch (this.weekday) {
              case true: {
                const tmpHotel = {
                  id: hotel.id,
                  name: hotel.name,
                  image: hotel.image,
                  price: hotel.regularWeekdayPrice,
                  rating: hotel.rating,
                  rewards: false,
                  weekday: true,
                };
                this.listHotels.push(tmpHotel);
                break;
              }
              case false: {
                const tmpHotel = {
                  id: hotel.id,
                  name: hotel.name,
                  image: hotel.image,
                  price: hotel.regularWeekendPrice,
                  rating: hotel.rating,
                  rewards: false,
                  weekday: false,
                };
                this.listHotels.push(tmpHotel);
                break;
              }
              default: {
                break;
              }
            }
            break;
          }
          default: {
            break;
          }
        }
      });
    },
    bestPrice() {
      const tempHotel = this.listHotels;
      tempHotel.sort((a, b) => a.price - b.price);
      tempHotel.splice(1, this.listHotels.length);
      this.listHotels = tempHotel;
    },
    rewardsSwitch() {
      if (this.rewards) {
        this.rewards = false;
      } else {
        this.rewards = true;
      }
      this.loadHotels();
    },
  },
  mounted() {
    this.loadHotels();
  },
});
</script>

<style lang="scss">
@import '@/assets/scss/variables.scss';
</style>
