<template>
<div class="shopcart-wrapper">
  <div class="shopcart">
    <div class="content" @click="toggleList">
    	<div class="content-left">
    		<div class="logo-wrapper">
    			<div class="logo" :class="{'highlight':totalCount>0}">
    				<i class="icon-shopping_cart"></i>
    			</div>
    			<div v-show="totalCount" class="num">{{totalCount}}</div>
    		</div>
    		<div class="price" :class="{'highlight':totalPrice>0}">
				${{totalPrice}}
    		</div>
    		<div class="desc">另需配送费${{deliveryPrice}}元</div>
    	</div>
    	<div class="content-right">
			<div class="pay" :class="payClass" @click.stop.prevent="pay">{{payDesc}}</div>
    	</div>
    </div> 

    <div class="ball-container">
    <transition-group name="drop">
		<div v-for="(ball,index) in balls" v-show="ball.show" class="ball" :key="index">
			<div class="inner"></div>
		</div>
	</transition-group>
    </div> 

    <transition name="fold">
	    <div class="shopcart-list"  v-show="listShow">
			<div class="list-header">
				<h1 class="title">购物车</h1>
				<span class="empty" @click="empty">清空</span>
			</div>
			
			<div class="list-content" ref="listContent">
				<ul>
					<li class="food" v-for="food in selectFoods">
						<span class="name">{{food.name}}</span>
						<div class="price">
							<span>${{food.price*food.count}}</span>
						</div>
						<div class="cartcontrol-wrapper">
							<cartcontrol :food='food'></cartcontrol>
						</div>
					</li>
				</ul>
			</div>
	    </div> 
    </transition>
  </div>
  <transition name="fade"><div  @click="hideList" class="list-mask" v-show="listShow"></div></transition>
 </div>
</template>

<script>
import cartcontrol from '../cartcontrol/cartcontrol.vue';
import bScroll from 'better-scroll';
export default {
  name: 'shopcart',
  props:{
  	selectFoods:{
  		type:Array,
  		default(){
  			return [];
  		}
  	},
  	deliveryPrice:{
  		type:Number,
  		default:0
  	},
  	minPrice:{
  		type:Number,
  		default:0
  	}
  },
  data(){
  	return {
  		balls:[
  			{
  				show:false
  			},
  			{
  				show:false
  			},
  			{
  				show:false
  			},
  			{
  				show:false
  			},
  			{
  				show:false
  			},
  		],
  		fold:true
  	}
  },
  computed:{
  	totalPrice(){
  		let total=0;
  		this.selectFoods.forEach((food)=>{
  			total+=food.price*food.count;
  		})
  		return total;
  	},
  	totalCount(){
  		let count=0;
  		this.selectFoods.forEach((food)=>{
  			count+=food.count;
  		})
  		return count;
  	},
  	payDesc(){
  		if(this.totalPrice===0){
  			return `${this.minPrice}元起送`;
  		}else if(this.totalPrice<this.minPrice){
  			let diff=this.minPrice-this.totalPrice;

  			return `还差${diff}元起送`
  		}else {
  			return '去结算';
  		}
  	},
  	payClass(){
  		if(this.totalPrice<this.minPrice){
  			return 'not-enough';
  		}else{
  			return 'enough';
  		}
  	},
  	listShow(){
  		if(!this.totalCount){
  			this.fold=true;
  			return false;
  		}
  		let show=!this.fold;
  		if(show){
  			this.$nextTick(()=>{
  				if(!this.scroll){
  					this.scroll=new bScroll(this.$refs.listContent,{
  						click:true
  					});
  				}else{
  					this.scroll.refresh();
  				}
  			})
  		}
  		return show;
  	}
  },
  methods:{
  	drop(el){
  		//console.log(el);
  	},
  	toggleList(){
  		if(!this.totalCount){
  			return;
  		}
  		this.fold=!this.fold;
  	},
  	empty(){
  		this.selectFoods.forEach((food)=>{
  			food.count=0;
  		})
  	},
  	hideList(){
  		this.fold=true;
  	},
  	pay(){
  		if(this.totalPrice<this.minPrice){
  			return;
  		};
  		window.alert(`需要自付${this.totalPrice}元`);
  	}
  },
  components:{
  	'cartcontrol':cartcontrol
  }
}
</script>

<style>
.shopcart-wrapper{
	width:100%;
	height:100%;
}
.shopcart{
	height:48px;
	width: 100%;
	position: fixed;
	bottom:0;
	left: 0;
	z-index: 99;	
}
.shopcart .content{
	display: flex;
	background: #141d27;
}

.shopcart .content .content-left{
	flex:1;
	font-size: 0;
}

.content-left .logo-wrapper{
	display: inline-block;
	position: relative;
	top:-10px;
	margin:0 12px;
	padding: 6px;
	box-sizing: border-box;
	width:56px;
	height:56px;
	border-radius:50%;
	background: #141d27;
	vertical-align: top;
}
.logo-wrapper .logo{
	width:100%;
	height:100%;
	border-radius:50%;
	text-align: center;
	background: #2b343c;
}
.logo-wrapper .highlight{
	background: rgb(0,160,220);
}
.icon-shopping_cart{
	line-height: 44px;
	font-size: 24px;
	color:#80858a;
}
.highlight .icon-shopping_cart{
	color:#fff;
}
.content-left .num{
	position: absolute;
	top:0;
	right:0;
	width:24px;
	height:16px;
	font-size: 9px;
	line-height: 16px;
	text-align: center;
	border-radius:8px;
	font-weight: 700;
	color:#fff;
	background: rgba(240,20,20,1);
	box-shadow:0px 4px 8px 0px rgb(0,0,0,.4);
}
.content-left .price{
	display: inline-block;
	line-height: 24px;
	margin: 12px ;
	vertical-align: top;
	box-sizing: border-box;
	padding-right:12px;
	border-right:1px solid rgba(255,255,255,.1);
	font-size: 16px;
	font-weight:700;
	color:rgba(255,255,255,.4);
}
.content-left .highlight{
	color:#fff;
}
.content-left .desc{
	display: inline-block;
	line-height: 24px;
	vertical-align: top;
	margin: 8px 0 0 0px;
	color:rgba(255,255,255,.4);
	font-size: 10px;
}
.shopcart .content .content-right{
	flex:0 0 105px;
	width:105px;
}
.content-right .pay{
	height:48px;
	line-height: 48px;
	text-align:center;
	font-size: 12px;
	color:rgba(255,255,255,.4);
	font-weight: 700;
	background: #2b343c;
}
.content-right .enough{
	background: green;
	color:#fff;
}

.ball-container .ball{
	position: fixed;
	left:32px;
	bottom:32px;
	z-index: 200;
}

.shopcart-list{
	position: absolute;
	top:0;
	left:00;
	z-index:-1;
	width:100%;
	transform :translate3d(0, -100%, 0);
}
.fold-enter-active, .fold-leave-active {
  transition: all .5s;
  transform: translate3d(0,-100%,0);
}
.fold-enter, .fold-leave-to {
  transform: translate3d(0,0,0);
}

.shopcart-list .list-header{
	height:40px;
	line-height:40px;
	padding:0 18px;
	background: #f3f5f7;
	border-bottom:1px solid rgba(7,17,27,0.1);
}
.list-header .title{
	float: left;
	font-size: 14px;
	color:rgb(7,17,27);
}
.list-header .empty{
	float: right;
	font-size: 12px;
	color:rgb(0,160,220);
}
.list-content{
	padding:0px 18px;
	max-height:217px;
	background: #fff;
	overflow: hidden;
	
}

.list-content .food{
	position: relative;
	padding:12px 0;
	box-sizing: border-box;
	border-bottom:1px solid rgba(7,17,27,0.1);
}
.list-content .food .name{
	font-size: 14px;
	line-height:24px;
	color:rgb(7,17,27);
}
.list-content .food .price{
	position: absolute;
	right:90px;
	bottom: 12px;
	line-height: 24px;
	font-size: 14px;
	font-weight: 700;
	color:rgb(240,20,20);
}
.list-content .food .cartcontrol-wrapper{
	position: absolute;
	right:0;
	bottom:6px;
}

.list-mask{
	position: fixed;
	top:0;
	left:0;
	width:100%;
	height:100%;
	z-index: 40;
	background: rgba(0,0,0,.5);
	backdrop-filter:blur(10px);
}

.fade-enter-active, .fade-leave-active{
  transition: all .5s
}
.fade-enter, .fade-leave-to {
  opacity: 0;
}

</style>
