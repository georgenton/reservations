<template>
  <div class="card">
    <div
      class="featured-image"
      v-bind:style="{ backgroundImage: 'url(' + image + ')' }">
    </div>
    <div class="content">
      <span :class="rewards ? 'rewards' : 'regular'">Client: {{ rewards ? 'Rewards' : 'Regular' }}</span>
      <div class="title"><h3>Hotel {{ name }}</h3></div>
      <div class="chips">
        <a :class="weekday ? 'chip' : 'chip large'">
          Price: {{ weekday ? 'Weekday ' + price + ' USD' : 'Weekend ' + price + ' USD' }}
        </a>
      </div>
      <p class="hotel-rating">
        <span class="hotel-rating-display" :data-rating="rating"></span>
        <span itemprop="ratingValue">{{ rating }}</span>
        <span class="hotel-rating-count"></span>
      </p>
    </div>
  </div>

</template>

<script lang="ts">
import { defineComponent } from 'vue';

export default defineComponent({
  name: 'HotelComponent',
  props: {
    id: {
      required: true,
      type: Number,
      default: 0,
    },
    name: {
      required: true,
      default: 'John doe',
      type: String,
    },
    image: {
      required: true,
      default: 'John doe',
      type: String,
    },
    price: {
      required: true,
      type: Number,
      default: 0,
    },
    weekday: {
      required: true,
      type: Boolean,
      default: true,
    },
    rewards: {
      required: true,
      type: Boolean,
      default: true,
    },
    rating: {
      required: true,
      type: String,
      default: '0.0',
    },
  },
});
</script>

<style scoped lang="scss">

</style>
