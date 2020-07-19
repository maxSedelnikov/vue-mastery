<template>
  <div id="app">
    <NavBar />
    <div class="cart">
      <p>Cart({{ cart.length }})</p>
    </div>
    <Product
      :premium="true"
      :cart="cart"
      @add-to-cart="updateCart"
      @remove-from-cart="updateCartOnRemove"
    />
  </div>
</template>

<script>
import NavBar from './components/NavBar.vue';
import Product from './components/Product.vue';

export default {
  name: 'App',
  components: { NavBar, Product },
  data: () => ({
    cart: [],
  }),
  methods: {
    updateCart(id) {
      this.cart.push(id);
    },
    updateCartOnRemove(id) {
      this.cart = this.cart.filter((item) => item !== id);
    },
  },
  computed: {},
};
</script>

<style lang="scss">
body {
  font-family: tahoma;
  color: #282828;
  margin: 0px;
}

.product {
  display: flex;
  flex-flow: wrap;
  padding: 1rem;
}

img {
  border: 1px solid #d8d8d8;
  max-width: 420px;
  width: 70%;
  margin: 40px;
  box-shadow: 0px 0.5px 1px #d8d8d8;
}

a {
  text-decoration: none;
  color: inherit;
  padding-bottom: 5px;
  border-bottom: 1px solid green;
}

ul {
  padding: 0;
  margin: 0;
  list-style: none;
}

.cart {
  margin-right: 25px;
  float: right;
  border: 1px solid #d8d8d8;
  padding: 5px 20px;
}

button {
  margin-top: 30px;
  border: none;
  background-color: #1e95ea;
  color: white;
  height: 50px;
  width: 120px;
  font-size: 14px;
  cursor: pointer;
}

.disabledButton {
  background-color: #d8d8d8;
}

input {
  width: 100%;
  height: 25px;
  margin-bottom: 20px;
}

textarea {
  width: 100%;
  height: 60px;
}

.tab {
  margin-left: 20px;
  cursor: pointer;
}

.activeTab {
  color: #16c0b0;
  text-decoration: underline;
}
</style>
