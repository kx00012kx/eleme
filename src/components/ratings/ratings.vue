<template>
  <div class="ratings" ref="ratings">
    <div class="ratings-content">
		<div class="ratings-overview">
			<div class="overview-left">
				<h1 class="score">{{this.seller.score}} </h1>
				<div class="title">综合评分</div>
				<div class="rank">高于周边商家{{seller.rankRate}}</div>
			</div>

			<div class="overview-right">
				<div class="score-wrapper">
					<span class="title">服务态度</span>
					<star class="star" :size="36" :score="seller.serviceScore"></star>
					<span class="score">{{seller.serviceScore}} </span>
				</div>
				<div class="score-wrapper">
					<span class="title">商品评分</span>
					<star class="star" :size="36" :score="seller.foodScore"></star>
					<span class="score">{{seller.foodScore}} </span>
				</div>
				<div class="delivery-wrapper">
					<span class="title">送达时间</span>
					<span class="delivery">{{seller.deliveryTime}}分钟</span>
				</div>
			</div>
		</div>
		<split></split>
		<ratingselect @increment="incrementTotal" :ratings="ratings" :selectType="selectType" :onlyContent="onlyContent"></ratingselect>
		<div class="rating-wrapper">
			<ul>
				<li v-show="needShow(rating.rateType, rating.text)" class="rating-item" v-for="rating in ratings">
					<div class="avatar">
						<img width="28" height="28" :src="rating.avatar">
					</div>
					<div class="content">
						<div class="name">{{rating.username}} </div>
						<div class="star-wrapper">
							<star :size="24" :score="rating.score"></star>
							<span v-show="rating.deliveryTime" class="delivery">{{rating.deliveryTime}}分钟</span>
						</div>
						<p class="text">{{rating.text}}</p>
						<div class="recommend" v-show="rating.recommend && rating.recommend.length ">
							<span class="icon-thumb_up"></span>
							<span class="item" v-for="item in rating.recommend">{{item}}</span>
						</div>
						<div class="time">{{rating.rateTime | formatDate}} </div>
					</div>
				</li>
			</ul>
		</div>
    </div>
  </div>
</template>

<script>
import bScroll from 'better-scroll';
import star from '../star/star.vue';
import split from '../split/split.vue';
import ratingselect from '../ratingselect/ratingselect.vue';
import {formatDate} from '../../common/js/data.js';
 const all=2;
  const ERR_OK=0;
export default {

  name: 'ratings',
  props:{
  	seller:{
  		type:Object
  	}
  },
  data(){
  	return {
  		ratings:[],
  		onlyContent:true,
  		selectType:all
  	}
  },
  created(){
  	this.$http.get('/api/ratings').then((response)=>{
  		response=response.body;
  		if(response.errno===ERR_OK){
  			this.ratings=response.data;
  			this.$nextTick(()=>{
  				this.scroll=new bScroll(this.$refs.ratings,{click:true})
  			}) 			
  		}
  	})
  },
  methods:{
  	needShow(type,text){
  		if (this.onlyContent && !text) {
          return false;
        }
        if (this.selectType === all) {
          return true;
        } else {
          return type === this.selectType;
        }
  	},
  	incrementTotal(type, data) {
  		console.log(this[type],data)
        this[type] = data;
        this.$nextTick(() => {
          this.scroll.refresh();
        });
    }
  },
  filters: {
    formatDate(time){
  		let date=new Date(time);
  		return formatDate(date,'yyyy-MM-dd hh:mm');
  	}
  },
  components:{
    'star':star,
    'split':split,
    'ratingselect':ratingselect
  }
}
</script>

<style>
.ratings{
	position: absolute;
	top:174px;
	left:0;
	bottom:0;
	width:100%;
	overflow: hidden;
}
.ratings-overview{
	display: flex;
	padding:18px 0;
}
.ratings-overview .overview-left{
	flex:0 0 137px;
	border-right:1px solid rgba(7,17,27,.1);
	width:137px;
	text-align:center;
	padding:6px 0;
}
@media only screen and (max-width: 320px){
	.ratings-overview .overview-left{
		flex:0 0 120px;
		width:120px;
	}
} 
.ratings-overview .overview-left .score{
	line-height: 28px;
	font-size: 24px;
	color:rgb(255,153,0);
	margin-bottom: 6px;
}
.ratings-overview .overview-left .title{
	line-height: 12px;
	font-size: 12px;
	color:rgb(7,17,27);
	margin-bottom: 8px;
}
.ratings-overview .overview-left .rank{
	line-height: 10px;
	font-size: 10px;
	color:rgb(147,153,159);
}
.ratings-overview .overview-right{
	flex:1;
	padding:6px 0 6px 24px;
}
@media only screen and (max-width: 320px){
	.ratings-overview .overview-right{		
		padding:6px 0 6px 6px;
	}
} 
.ratings-overview .overview-right .score-wrapper{	
	margin-bottom:8px;
	font-size: 0;
}
.ratings-overview .overview-right .score-wrapper .title{
	font-size: 12px;
	color:rgb(7,17,27);
	line-height: 18px;
	vertical-align: top;
}
.ratings-overview .overview-right .score-wrapper .star{
	display: inline-block;
	margin-left: 12px;
	line-height: 18px;
	vertical-align: top;
}
.ratings-overview .overview-right .score-wrapper .score{
	display: inline-block;
	line-height: 18px;
	vertical-align: top;
	color:rgb(255,153,0);
	font-size: 12px;
	margin-left: 4px;
}
.delivery-wrapper{
	font-size: 0;
}
.delivery-wrapper .title{
	font-size: 12px;
	color:rgb(7,17,27);
	line-height: 18px;
	vertical-align: top;
}
.delivery-wrapper .delivery{
	margin-left: 12px;
	font-size: 12px;
	color:rgb(147,153,159);
	vertical-align: top;
	line-height: 18px;
}
.rating-wrapper{
	padding:0 18px;	
}
.rating-wrapper .rating-item{
	display: flex;
	padding:18px 0;
	border-bottom:1px solid rgba(7,17,27,.1);
}
.rating-item .avatar{
	flex:0 0 28px;
	width:28px;
	margin-right: 12px;
}
.rating-item .avatar img{
	border-radius: 50%;
}
.rating-item .content{
	position: relative;
	flex:1;
}
.rating-item .content .name{
	margin-bottom: 4px;
	line-height: 12px;
	font-size: 10px;
	color: rgb(7,17,27);
}
.rating-item .content .star-wrapper{
	margin-bottom:6px;
	font-size: 0;
}
.rating-item .content .star-wrapper .star{
	display: inline-block;
	margin-right:6px;
	vertical-align: top;
}
.rating-item .content .star-wrapper .delivery{
	display: inline-block;
	vertical-align: top;
	color:rgb(147,153,159);
}
.rating-item .content .text{
	font-size: 12px;
	line-height: 18px;
	margin-bottom:8px;
	color:rgb(7,17,27);
}
.rating-item .content .recommend{
	line-height: 16px;
}
.recommend .icon-thumb_up,.recommend .item{
	display: inline-block;
	margin:0 8px 4px 0;
	font-size: 9px;

}
.recommend .icon-thumb_up{
	color:rgb(0,160,220);
}
.recommend .item{
	padding:0 6px;
	border:1px solid rgba(7,17,27,.1);
	border-radius:1px;
	line-height: 16px;
	vertical-align: top;
	background: #fff;
}
.rating-item .content .time{
	position: absolute;
	top:0;
	font-size: 10px;
	right:0;
	color:rgb(147,153,159);
}
</style>