<template>
     <div class="cartcontrol" >
     <transition name="show-cart-decrease">
            <div class="cart-decrease icon-remove_circle_outline" v-show="food.count > 0" @click.stop.prevent="decreaseCart($event)"></div>
     </transition>       
            <div class="cart-count" v-show="food.count > 0">{{food.count}}</div>
            <div class="cart-add icon-add_circle" @click.stop.prevent="addCart($event)"></div>
     </div>
</template>

<script>
import Vue from 'vue';
export default {
  props: {
  	food: {
  		type: Object
  	},
  	eventHub: {
  		type: Object
  	}
  },
  // created() {
  // 	console.log(this.food);
  // },
  methods: {
  	addCart(event) {
	if (!event._constructed) {
		return;
	}
          if (!this.food.count) {
               Vue.set(this.food, 'count', 1);
          } else {
        	    this.food.count++;
          }
          this.eventHub.$emit('cart-add', event.target);
     },
     decreaseCart(event) {
	if (!event._constructed) {
		return;
	}
	if (this.food.count) {
                this.food.count--;
	}
     }
  }
};
</script>

<style lang="less" rel="stylesheet/less">
.cartcontrol {
	font-size: 0;
	.cart-decrease {
	  transition: all .3s linear;
	  position: absolute;
	  left: -36px;
            &.show-cart-decrease-enter-active, &.show-cart-decrease-leave-active {
                 opacity: 1;
                 transform: rotate(0);
                 left: -36px;
                }
           &.show-cart-decrease-enter, &.show-cart-decrease-leave-active {
                opacity: 0;
                transform: rotate(180deg);
                left: 6px;
            }
	}

	.cart-decrease ,.cart-add{
		display: inline-block;
		padding: 6px;
		line-height: 24px;
                     font-size: 24px;
                     color: rgb(0,160,220);
	}
	.cart-count {
		display: inline-block;
		line-height: 24px;
                     font-size: 24px;
                     color: rgb(0,160,220);
                     vertical-align: top;
                     width: 12px;
                     padding-top: 6px;
                     line-height: 24px;
                     text-align: center;
                     font-size: 10px;
                     color: rgb(147,153,159);
	}
	.cart-add {
                    
	}
}
</style>

