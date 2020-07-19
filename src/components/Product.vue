<template>
  <div class="product">
    <div class="product-image">
      <img :src="image" :alt="title" />
    </div>
    <div class="product-info">
      <h1>{{ title }}</h1>
      <template v-if="onSale">
        <p>{{ title }} On Sale!</p>
      </template>
      <template v-else>
        <p>{{ title }} just right for you!!</p>
      </template>
      <a :href="product.catalogUrl" target="_blank">go to similar products</a>
      <template v-if="inStock">
        <p v-if="inStock > 10">In Stock</p>
        <p v-else-if="inStock > 0 && inStock <= 10">Almost sold out</p>
        <p v-else class="outOfStock">Out of Stock</p>
      </template>
      <template v-else>
        <p class="outOfStock">Out of Stock</p>
      </template>
      <p>Shipping: {{ shipping }}</p>
      <Details :details="product.details" />
      <div
        class="color-box color-option"
        v-for="(variant, index) in product.variants"
        :key="variant.variantId"
        :style="{ backgroundColor: variant.variantColor }"
        @mouseover="updateProduct(index)"
      ></div>
      <div class="sizes-container">
        <div
          v-for="(size, index) in sizes"
          :key="index"
          :class="{ outOfStock: !inStock }"
        >{{ size }}</div>
      </div>
      <div class="actions">
        <button
          @click="addToCart"
          :disabled="!inStock"
          :class="{ disabledButton: !inStock }"
        >Add to cart</button>
        <button
          @click="removeFromCart"
          :disabled="!inCart"
          :class="{ disabledButton: !inCart }"
        >Remove item from cart</button>
      </div>
    </div>
    <ProductTabs :reviews="product.reviews" />
  </div>
</template>

<script>
import Details from './Details.vue';
import ProductTabs from './ProductTabs.vue';
import EventBus from '../event-bus';

export default {
  name: 'Product',
  props: {
    premium: {
      type: Boolean,
      required: true,
    },
    cart: {
      type: Array,
      required: true,
    },
  },
  components: { Details, ProductTabs },
  data: () => ({
    product: {
      name: 'Socks',
      vendor: 'Vue Matery',
      details: ['80% cotton', '20% polyester', 'Gender-neutral'],
      selectedVariant: 0,
      catalogUrl: 'https://google.com',
      variants: [
        {
          variantId: 2234,
          variantColor: 'green',
          variantImage:
            'https://www.vuemastery.com/images/challenges/vmSocks-green-onWhite.jpg',
          variantQuantity: 50,
          variantOnSale: true,
          sizes: [41, 36, 38, 43],
        },
        {
          variantId: 4432,
          variantColor: 'blue',
          variantImage:
            'https://www.vuemastery.com/images/challenges/vmSocks-blue-onWhite.jpg',
          variantQuantity: 10,
          variantOnSale: false,
          sizes: [44, 37, 41, 39, 42],
        },
      ],
      reviews: [],
    },
  }),
  methods: {
    addToCart() {
      this.$emit(
        'add-to-cart',
        this.product.variants[this.product.selectedVariant].variantId,
      );
      this.product.variants[this.product.selectedVariant].variantQuantity -= 1;
    },
    removeFromCart() {
      this.$emit(
        'remove-from-cart',
        this.product.variants[this.product.selectedVariant].variantId,
      );
      this.product.variants[this.product.selectedVariant].variantQuantity += 1;
    },
    updateProduct(index) {
      this.product.selectedVariant = index;
    },
  },
  computed: {
    title() {
      return `${this.product.vendor} ${this.product.name}`;
    },
    image() {
      return this.product.variants[this.product.selectedVariant].variantImage;
    },
    inStock() {
      return this.product.variants[this.product.selectedVariant]
        .variantQuantity;
    },
    onSale() {
      return this.product.variants[this.product.selectedVariant].variantOnSale;
    },
    sizes() {
      const arr = this.product.variants[this.product.selectedVariant].sizes;

      return arr.sort((a, b) => b - a);
    },
    shipping() {
      if (this.premium) {
        return 'Free';
      }

      return '3.99$';
    },
    inCart() {
      const itemsInCart = this.cart;
      let flag = false;

      if (
        itemsInCart.indexOf(
          this.product.variants[this.product.selectedVariant].variantId,
        ) !== -1
      ) {
        flag = true;
      }

      return flag;
    },
  },
  mounted() {
    EventBus.$on('review-submitted', (productReview) => {
      this.product.reviews.push(productReview);
    });
  },
};
</script>

<style scoped lang="scss">
.product-image {
  width: 80%;
}

.product-image,
.product-info {
  margin-top: 10px;
  width: 50%;
}

.color-box {
  display: inline-block;
  width: 40px;
  height: 40px;
  margin-top: 5px;
  margin-right: 5px;
  cursor: pointer;
}

.outOfStock {
  text-decoration: line-through;
}

.review-form {
  width: 400px;
  padding: 20px;
  margin: 40px;
  border: 1px solid #d8d8d8;
}

.sizes-container {
  display: flex;
  align-items: center;
  padding: 10px 0;
}

.sizes-container div {
  margin-right: 10px;
  padding: 5px;
  border: 1px solid green;
}

.actions {
  display: flex;
  align-items: center;
}

.actions button:first-child {
  margin-right: 10px;
}
</style>
