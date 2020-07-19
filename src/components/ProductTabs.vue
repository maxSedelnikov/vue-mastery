<template>
  <div>
    <span
      class="tab"
      :class="{ activeTab: selectedTab === tab }"
      v-for="(tab, index) in tabs"
      :key="index"
      @click="selectedTab = tab"
      >{{ tab }}</span
    >
    <div v-show="selectedTab == 'Reviews'">
      <h2>Reviews</h2>
      <p v-if="!reviews.length">There are no reviews yet</p>
      <ul>
        <li v-for="(review, index) in reviews" :key="index">
          <p>{{ review.name }}</p>
          <p>Rating {{ review.rating }}</p>
          <p>{{ review.review }}</p>
          <p>Going to recomend this product: {{ review.willRecomend }}</p>
        </li>
      </ul>
    </div>
    <div v-show="selectedTab == 'Make a review'">
      <ProductReview />
    </div>
  </div>
</template>

<script>
import ProductReview from './ProductReview.vue';

export default {
  name: 'ProductTabs',
  props: {
    reviews: {
      type: Array,
      required: true,
    },
  },
  components: { ProductReview },
  data: () => ({
    tabs: ['Reviews', 'Make a review'],
    selectedTab: 'Reviews',
  }),
};
</script>

<style></style>
