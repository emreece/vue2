<template>
<div class="cart-wrapper">
<h2>{{title}}</h2> 
    <div class="cart">
        <div class="item card my-2 p-3" v-bind:key="index" v-for="(item,index)  in cart">
            <h1>{{item.name}}</h1>
            <p>{{item.price}} $</p>
            <div v-if="item.isAvaliable">In Stock</div>
            <div v-if="!item.isAvaliable">No Stock</div>
            <a v-if="item.isAvaliable" href="#" v-on:click="moveToProducts(index)">{{buttontext}}</a>
        </div>
        <div class="cart-info"><h3>({{cartsCount.number}} {{cartsCount.message}}) <span v-if="showtotal">Total:{{cartTotal}} $</span></h3></div>
    </div>
</div>
</template>
<script>
export default {
    name:'cart',
    props: {
        cart: {type:Array, required:true, default:() => {return []} },
        // targetcart: {type:Array, required:true, default:() => {return []}},
        title: {type:String, required:true},
        buttontext: {type:String, required:true},
        carttype: {type:String, required:true},
        showtotal: {type:Boolean, required:false, default:true}
    },
    computed: {
        cartTotal() {
            let total = 0;
            this.cart.forEach(element => {
                total += parseFloat(element.price);
            });
            return total.toFixed(2);
        },
        cartsCount() {
            let total = 0;
            let data = {};
            this.cart.forEach(element => {
                total++;
            });
            if(total == 0) { data.message = 'No Item'; data.number = '';} 
            else if (total == 1) { data.message = ' item'; data.number = total;  } 
            else { data.message = 'items'; data.number = total;  }
            return data;
        },
        isProducts() {
            return this.cartType === 'products'
        },
        isCart() {
            return this.cartType === 'cart'
        }
    },
    methods: {
        moveToProducts(index) {
            const item = this.cart.splice(index,1)[0];
            // this.targetcart.push(item); moved to handle function
            this.$emit('movingproduct', item, this.carttype)
        }
    },
    };
</script>