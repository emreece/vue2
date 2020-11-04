<template>
    <div id="app">
        <div class="container">
            <p v-if="isLoading">Loading</p>
            <div class="row">
                <div class="col-lg-6">
                    <cart :cart=products carttype="products" title="Products" :showtotal=false buttontext="Add to cart"  v-on:movingproduct="handlemovingproduct"></cart>
                </div>
                <div class="col-lg-6">
                <cart v-on:movingproduct="handlemovingproduct" :cart=cart carttype="cart" title="Cart" buttontext="Remove from cart"></cart>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import cart from '@/components/cart';
 

export default {
        data: function() {
              return  {
                  isLoading: true,
                  products:[],
                  cart:[]
              }
        },
        components : {cart},
        methods: {
            handlemovingproduct(item, cartType) {
                if (cartType === 'products') {
                    this.cart.push(item);
                }
                else {
                    this.products.push(item);
                }
            }
        },
        created() {
            fetch('/static/data.json')
                .then((res) => {return res.json()})
                .then((res) => {
                    this.isLoading = false; 
                    this.products = res.products;
                    this.cart = res.cart;
                })
        }
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
