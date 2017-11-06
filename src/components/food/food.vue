<template>
	<transition name="aaa">
	<div v-show="showFlag" class="food" ref="food">
		<div class="food-content">
			<div class="image-header">
				<img :src="food.image">
				<div class="back" @click="hide">
					<i class="icon-arrow_lift"></i>
				</div>
			</div>
			<div class="content">
				<h1 class="title">{{food.name}}</h1>
				<div class="detail">
					<span class="sell-count">月售{{food.sellCount}}</span>
					<span class="rating">好评率{{food.rating}}</span>
				</div>
				<div class="price">
					<span class="now">${{food.price}}</span><span class="old" v-show="food.oldPrice">${{food.oldPrice}}</span>
				</div>
				<div class="cartcontrol-wrapper">
					<cartcontrol :food="food"></cartcontrol>
				</div>
				<transition name="fade">
					<div @click.shop.prevent="addFirst($event)" class="buy" v-show="!food.count||food.count===0">加入购物车</div>
				</transition>
			</div>
			<split v-show="food.info"></split>
			<div class="info" v-show="food.info">
				<h1 class="title">商品信息</h1>
				<p class="text">{{food.info}} </p>
			</div>
			<split></split>
			<div class="ratings">
				<h1 class="title"></h1>
				<ratingselect  @increment="incrementTotal" :ratings="food.ratings" :selectType="selectType" :onlyContent="onlyContent" :desc="desc"></ratingselect>
				<div class="rating-wrapper">
					<ul v-show="food.ratings&&food.ratings.length">
						<li v-for="rating in food.ratings"  v-show="needShow(rating.rateType,rating.text)" class="rating-item">
							<div class="user">
								<span class="name">{{rating.username}} </span>
								<img class="avatar" width="12" height="12" :src="rating.avatar">
							</div>
							<div class="time">{{rating.rateTime | formatDate}} </div>
							<p class="text">
								<span :class="{'icon-thumb_up':rating.rateType===0,'icon-thumb_down':rating.rateType===1}"></span>
							{{rating.text}} </p>
						</li>
					</ul>
					<div class="no-rating" v-show="!food.ratings||!food.ratings.length"> 暂无评价</div>
				</div>
			</div>

		</div>		
	</div>
	</transition>
</template>

<script>
import bScroll from 'better-scroll';
import cartcontrol from '../cartcontrol/cartcontrol.vue';
import vue from 'vue';
import split from '../split/split.vue';
import ratingselect from '../ratingselect/ratingselect.vue';
import {formatDate} from '../../common/js/data.js';

const positive=0;
const negative=1;
const all=2;

export default {
  name: 'food',
  props:{
  	food:{
  		type:Object
  	}
  },
  data(){
  	return {
  		showFlag:false,
  		selectType:all,
  		onlyContent:false,
  		desc:{
  			all:'全部',
  			positive:'推荐',
  			negative:'吐槽'
  		}
  	}
  },
  methods:{
  	show(){
  		this.showFlag=true;
  		this.$nextTick(()=>{
  			if(!this.scroll){
  				this.scroll=new bScroll(this.$refs.food,{
  					click:true
  				})
  			}
  		})
  	},
  	incrementTotal(type, data) {
        this[type] = data;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
      },
  	hide(){
  		this.showFlag=false;
  	},
  	addFirst(event){
  		if(!event._constructed){
  			return;
  		}
  		vue.set(this.food,'count',1)
  	},
  	needShow(type,text){
  		if (this.onlyContent && !text) {
          return false;
        }
        if (this.selectType === all) {
          return true;
        } else {
          return type === this.selectType;
        }
  	}
  },
  components:{
  	'cartcontrol':cartcontrol,
  	'split':split,
  	'ratingselect':ratingselect
  },
  filters:{
  	formatDate(time){
  		let date=new Date(time);
  		return formatDate(date,'yyyy-MM-dd hh:mm');
  	}
  }

}
</script>

<style>
.food{
	position: fixed;
	left:0;
	top:0;
	bottom:48px;
	z-index: 30;
	width:100%;
	background: #fff;

}
.aaa-enter-active, .aaa-leave-active {
  transition: all .3s linear;
  transform: translate3d(0,0,0);
}
.aaa-enter, .aaa-leave-to {
  transform: translate3d(100%,0,0);
}
.image-header{
	position: relative;
	width:100%;
	height:0;
	padding-top:100%;
}
.image-header img{
	position: absolute;
	top:0;
	left: 0;
	width:100%;
	height:100%;
}
.image-header .back{
	position: absolute;
	top:10px;
	left: 0;
	border-radius: 50%;
	border:1px solid #FFF;
	background: rgba(0,0,0,.3);

}
.back .icon-arrow_lift{
	display: block;
	padding: 10px;
	font-size: 20px;
	color:#fff;
	text-align: center;
}
.food .food-content .content{
	position: relative;
	padding:18px;
}

.food .content .title{
	line-height: 14px;
	margin-bottom:8px;
	font-size:14px;
	font-weight: 700;
	color:rgb(7,17,27);
}
.food .content .detail{
	margin-bottom:18px;
	line-height: 10px;
	font-size: 0;
}
.food .content .detail .sell-count{
	font-size: 10px;
	color:rgb(147,153,159);
	margin-right:12px;
}
.food .content .detail .rating{
	font-size: 10px;
	color:rgb(147,153,159);
}
.food .content .price{
	font-weight: 700;
	line-height: 24px;
}
.food .content .price .now{
	font-size: 14px;
	line-height: 14px;
	font-weight: 700;
	margin-right: 12px;
}
.food .content .price .old{
	color:rgb(147,153,159);
	text-decoration:line-through;
	font-weight: 700;
}
.food .cartcontrol-wrapper{
	position: absolute;
	right:12px;
	bottom: 12px;
}

.food .buy{
	position: absolute;
	right:18px;
	bottom:18px;
	z-index: 10;
	height:24px;
	line-height: 24px;
	padding:0 12px;
	box-sizing: border-box;
	font-size: 10px;
	border-radius:12px;
	color:#fff;
	background: rgb(0,160,220);
}

.food .info{
	padding:18px;
}
.food .info .title{
	line-height: 14px;
	margin-bottom:6px;
	font-size: 14px;
	color:rgb(7,17,27);
}
.food .info .text{
	line-height: 24px;
	padding:0 8px;
	font-size: 12px;
	color:rgb(77,85,93);
}

.food .rating{
	padding:18px;
}
.food .rating .title{
	line-height: 14px;
	margin-left:18px;
	font-size: 14px;
	color:rgb(7,17,27);
}

.food .rating-wrapper{
	padding:0 18px;
}
.food .rating-wrapper .rating-item{
	position: relative;
	padding: 16px 0;
	border-bottom:1px solid rgba(7,17,27,0.1);
}
.rating-item .user{
	position: absolute;
	right:0;
	top:16px;
	font-size: 0;
	line-height: 12px;
}
.rating-item .user .name{
	display: inline-block;
	vertical-align: middle;
	font-size: 10px;
	color: rgb(147,153,159);
	margin-right:6px;
}
.rating-item .user .avatar{
	border-radius: 50%;
	display: inline-block;
	vertical-align: middle;
}

.rating-item .time{
	line-height: 12px;
	font-size: 10px;
	color: rgb(147,153,159);
	margin-bottom:6px;
}
.rating-item .text{
	line-height: 16px;
	font-size: 12px;
	color:rgb(7,17,27);
}
.rating-item .icon-thumb_up,.rating-item .icon-thumb_down{
	line-height: 16px;
	margin-right:4px;
	font-size:12px;
}
.rating-item .icon-thumb_up{
	color:rgb(0,160,220);
}
.rating-item .icon-thumb_down{
	color:rgb(147,153,159);
}
.no-rating{
	padding:16px 0;
	font-size: 12px;
	color:rgb(147,153,159);
}
</style>
