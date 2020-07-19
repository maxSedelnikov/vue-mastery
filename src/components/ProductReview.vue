<template>
  <form class="review-form" @submit.prevent="onSubmit">
    <p>
      <label for="name">Name:</label>
      <input id="name" v-model="name" placeholder="name" required />
    </p>

    <p>
      <label for="review">Review:</label>
      <textarea id="review" v-model="review" required></textarea>
    </p>

    <p>
      <label for="rating">Rating:</label>
      <select id="rating" v-model.number="rating" required>
        <option>5</option>
        <option>4</option>
        <option>3</option>
        <option>2</option>
        <option>1</option>
      </select>
    </p>

    <p class="recommendation">
      <span class="header">Would you reccomend this product?</span>
      <span class="container">
        <label for="yes">Yes</label>
        <input type="radio" name="recos" id="yes" value="yes" v-model="willRecomend" required />
        <label for="no">No</label>
        <input type="radio" name="recos" id="no" value="no" v-model="willRecomend" required />
      </span>
    </p>

    <p>
      <input type="submit" value="Submit" />
    </p>
  </form>
</template>

<script>
import EventBus from '../event-bus';

export default {
  name: 'ProductReview',
  data: () => ({
    name: null,
    review: null,
    rating: null,
    willRecomend: null,
  }),
  methods: {
    onSubmit() {
      const productReview = {
        name: this.name,
        review: this.review,
        rating: this.rating,
        willRecomend: this.willRecomend,
      };

      EventBus.$emit('review-submitted', productReview);
      this.name = null;
      this.review = null;
      this.rating = null;
      this.willRecomend = null;
    },
  },
};
</script>

<style lang="scss" scoped>
.recommendation {
  display: flex;
  flex-direction: column;
}
.recommendation .container {
  display: flex;
}

.recommendation span {
  padding: 10px 0;
}
</style>
