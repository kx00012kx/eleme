<template>
  <div class="goods">

    <div class="menu-wrapper" ref="menu">
    	<ul>
    		<li v-for="(item,index) in goods" :class="{'current':currentIndex===index}" class="menu-item line-1px" @click="selectMenu(index,$event)">
    			<span class="text">
    				<span class="icon" :class=classMap[item.type]></span>{{item.name}}
    			</span>
    		</li>
    	</ul>
    </div>

    <div class="food-wrapper" ref="foods">
		<ul>
			<li v-for="item in goods" class="food-list-hook">
				<h1 class="title">{{item.name}}</h1>
				<ul >
					<li @click="selectFood(food,$event)"  v-for="food in item.foods" class="food-item line-1px">
						<div class="icon">
							<img :src="food.icon" width="100%" height="100%">
						</div>
						<div class="content">
							<h2 class="name">{{food.name}} </h2>
							<p class="desc"  v-show="food.description">{{food.description}}</p>
							<div class="extra">
								<span>月售{{food.sellCount}}份</span>
								<span>好评率{{food.rating}}</span>
							</div>
							<div class="price">
								<span>$</span><span class="newPrice">{{food.price}}</span>
								<span class="oldPrice" v-show="food.oldPrice">${{food.oldPrice}}</span>
							</div>
							<div class="cartcontrol-wrapper">
								<cartcontrol @_add="_drop" :food="food"></cartcontrol>
							</div>
						</div>
					</li>
				</ul>
			</li>
		</ul>
    </div>
	
	<div class="shop">
		<shopcart ref="shopcart" :select-foods="selectFoods" :delivery-price="seller.deliveryPrice" :min-price="seller.minPrice"></shopcart>
	</div>
	<food :food="selectedFood" ref="food"></food>
  </div>
</template>

<script>
import bScroll from 'better-scroll';
import shopcart from '../shopcart/shopcart.vue'
import cartcontrol from '../cartcontrol/cartcontrol.vue'
import food from '../food/food.vue'
export default {
  name: 'goods',
  props:{
  	seller:{
  		type:Object
  	}
  },
  data(){
  	return{
  		goods:[],
  		listHeight:[],
  		scrollY:0,
  		selectedFood:{}
  	}
  	
  },
  created(){
  	this.$http.get('/api/goods').then((response)=>{
  		response=response.body;     
      if(response.errno===0){
        this.goods=response.data;
        this.$nextTick(()=>{
        	this._initScroll();
        	this._calculateHeight();
        })
      }
  	});
  	this.classMap=['decrease', 'discount', 'special', 'invoice', 'guarantee']
  },
  computed:{
  	currentIndex(){
  		for(let i=0;i<this.listHeight.length;i++){
  			let height1=this.listHeight[i];
  			let height2=this.listHeight[i+1];
  			if(!height2 || this.scrollY>=height1&&this.scrollY<height2){
  				return i;
  			}
  		}
  		return 0;
  	},
  	selectFoods(){
  		let foods=[];
  		this.goods.forEach((good)=>{
  			good.foods.forEach((food)=>{
  				if(food.count){
  					foods.push(food);
  				}
  			})
  		})
  		return foods;
  	}
  },
  methods: {
  	selectMenu(index,event){
  		if(!event._constructed){ 			
  			return;
  		} 		
  		let foodList=this.$refs.foods.getElementsByClassName('food-list-hook');
  		let el=foodList[index];
  		this.foodsScroll.scrollToElement(el,300);
  	},
  	_drop(target){
  		this.$refs.shopcart.drop(target);
  	},
  	_initScroll(){
  		this.menuScroll=new bScroll(this.$refs.menu,{
  			click: true
  		});
  		this.foodsScroll=new bScroll(this.$refs.foods,{
  			probeType:3,
  			click: true
  		});
  		this.foodsScroll.on('scroll',(pos)=>{
  			this.scrollY=Math.abs(Math.round(pos.y))
  		})
  	},
  	_calculateHeight(){
  		let foodList=this.$refs.foods.getElementsByClassName('food-list-hook');
  		let height=0;
  		this.listHeight.push(height);

  		for(let i=0;i<foodList.length;i++){
  			height+=foodList[i].clientHeight;
  			this.listHeight.push(height);
  		}
  	},
  	selectFood(food,event){
  		if(!event._constructed){ 			
  			return;
  		} 
  		this.selectedFood=food;
  		
  		this.$refs.food.show();
  	}
  },
  components:{
  	'shopcart':shopcart,
  	'cartcontrol':cartcontrol,
  	'food':food
  }
}
</script>

<style>
@import '../../common/css/mixin.css';
img{
	display: block;
}
.goods{
	width:100%;
	display: flex;
	position: fixed;
	top:175px;
	bottom: 46px;
	overflow: hidden;
}
.goods .menu-wrapper{
	flex:0 0 80px;
	width:80px;
	background: #f3f5f7;
}
.goods .food-wrapper{
	flex:1;
}

.menu-item{
	display:table;
	height:54px;
	width:100%;
	margin:0 auto;
}

.current{
	position: relative;
	background: #fff;
	font-size: 700;
	border:none;
	margin-top: -1px;
	z-index:10;
}

.menu-item .text{
	display: table-cell;
	width:56px;
	font-size: 12px;
	line-height: 14px;
	vertical-align: middle;
	color:rgb(7,17,27);
	padding:0 12px;
}


.food-wrapper .title{
	border-left: 2px solid #d9dde1;
	font-size: 12px;
	color:rgb(147,153,159);
	padding-left: 14px;
	line-height: 26px;
	background: #f3f5f7;
	height:26px;
} 


.food-wrapper .food-item{
	margin:0 18px;
	padding:18px 0;	
	display: flex;
}
.food-wrapper .food-item:last-child:after{
	display: none;
}

.food-wrapper .food-item .icon{
	flex: 0 0 57px;
	height:57px;
	height:57px;
	margin-right: 10px;
}

.food-wrapper .food-item .content{
	flex: 1;
}
.content .name{
	padding:2px 0 4px;
	font-size: 14px;
	color:rgb(7,17,27);
}
.content .desc, .content .extra{
	font-size: 10px;
	color:rgb(147,153,159);
	padding:3px 0;
	line-height: 12px;
}

.content .extra span:last-child{
	margin-left: 12px;
}

.content .price span{
	font-size: 10px;
	color:rgb(250,20,20);
	font-weight: normal;
}

.content .price .newPrice{
	font-size: 14px;
	line-height: 14px;
	font-weight: 700;
}
.content .price .oldPrice{
	color:rgb(147,153,159);
	text-decoration:line-through;
	font-weight: 700;
}

.menu-wrapper .icon{
	display: inline-block;	
	vertical-align: middle;
}
.menu-wrapper .decrease{
	margin-right: 2px;
	width:12px;
	height:12px;
	background: url(decrease_3@2x.png) no-repeat top left/12px 12px;
}
.menu-wrapper .discount{
	margin-right: 2px;
	width:12px;
	height:12px;
	background: url(discount_3@2x.png) no-repeat top left/12px 12px;
}
.menu-wrapper .special{
	margin-right: 2px;
	width:12px;
	height:12px;
	background: url(special_3@2x.png) no-repeat top left/12px 12px;
}
.menu-wrapper .invoice{
	margin-right: 2px;
	width:12px;
	height:12px;
	background: url(invoice_3@2x.png) no-repeat top left/12px 12px;
}
.menu-wrapper .guarantee{
	margin-right: 2px;
	width:12px;
	height:12px;
	background: url(guarantee_3@2x.png) no-repeat top left/12px 12px;
}

@media (-webkit-min-device-pixel-ratio: 3),(min-device-aspect-ratio: 3){
	.menu-wrapper .decrease{
		margin-right: 2px;
		width:12px;
		height:12px;
		background: url(decrease_3@3x.png) no-repeat top left/12px 12px;
	}
	.menu-wrapper .discount{
		margin-right: 2px;
		width:12px;
		height:12px;
		background: url(discount_3@3x.png) no-repeat top left/12px 12px;
	}
	.menu-wrapper .special{
		margin-right: 2px;
		width:12px;
		height:12px;
		background: url(special_3@3x.png) no-repeat top left/12px 12px;
	}
	.menu-wrapper .invoice{
		margin-right: 2px;
		width:12px;
		height:12px;
		background: url(invoice_3@3x.png) no-repeat top left/12px 12px;
	}
	.menu-wrapper .guarantee{
		margin-right: 2px;
		width:12px;
		height:12px;
		background: url(guarantee_3@3x.png) no-repeat top left/12px 12px;
	}
}

.cartcontrol-wrapper{
	position: absolute;
	right:0px;
	bottom:7px;
}


</style>
