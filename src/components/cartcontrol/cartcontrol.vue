<template>
  <div class="cartcontrol">
  	<transition name="move">
	   <div class="cart-decrease" v-show="food.count" @click.stop.prevent="decCart($event)">
			<div class="inner icon-remove_circle_outline"></div>
	   </div>
    </transition>
   <div class="cart-count" v-show="food.count">{{food.count}}</div>
   <div class="cart-add icon-add_circle" @click.stop.prevent="addCart($event)"></div>

  </div>
</template>

<script>
import Vue from 'vue';
export default {
	"name":"cartcontrol",
	props:{
		food:{
			type:Object
		}
	},
	methods:{
		addCart(event){
			if(!event._constructed){ 			
	  			return;
	  		} 
			if(!this.food.count){
				Vue.set(this.food,'count',1)
			}else{
				this.food.count++;
			}
			this.$emit('_add',event.target);
		},
		decCart(event){
			if(!event._constructed){ 			
	  			return;
	  		}
	  		if(this.food.count===0){
				return
			}else{
				this.food.count--;
			}
		}
	}
}
</script>

<style>
.cartcontrol {
	font-size: 0;
}
.cartcontrol .cart-decrease{
	display: inline-block;
	padding:6px;
}
.cart-decrease .inner{
	display: inline-block;
	font-size: 24px;
	color:rgb(0,160,220);
	line-height: 24px;
}
.cartcontrol .cart-add{
	display: inline-block;
	padding:6px;
	font-size: 24px;
	color:rgb(0,160,220);
	line-height: 24px;
}
.cartcontrol .cart-count{
	display: inline-block;
	vertical-align: top;
	width:12px;
	padding-top:6px;
	line-height:24px;
	text-align:center;
	font-size: 10px;
	color:rgb(147,153,159);
}

.move-enter-active, .move-leave-active , .move-enter-active .inner, .move-leave-active .inner{
  transition: all .5s
}
.move-enter, .move-leave-to {
  transform: translate3d(24px,0,0);
  opacity: 0;
}
.move-enter .inner, .move-leave-to .inner {
  transform: rotate(180deg);
}

</style>
