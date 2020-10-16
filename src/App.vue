<template>
  <div id="app">
    <img alt="Vue logo" src="./assets/logo.png">
    <h1 v-text="title"></h1>
    <ul>
      <li v-for='item in cartList' :key='item.id'>
        <h2>
          {{item.title}}
        </h2>
        <p>
          ${{item.price}}
        </p>
      </li>
    </ul>
    <MyCart :cart='cartList' title='title' /> 
  </div>
</template>

<script>
import MyCart from './components/Cart'

export default {
  name: 'App',
  data() {
    return {
      cartList:[],
      title: "Shopping Cart"
    };
  },
  async created() {
    try {
      const res = await this.$http.get('/api/cartList');
      this.cartList = res.data.result;
    } catch (error) {
      console.log(error);
    }
  },
  components: {
    MyCart
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
