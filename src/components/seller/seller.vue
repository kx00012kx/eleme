
<template>
	<div class="seller" ref="seller">
		<div class="seller-wrapper">
			<div class="overview">
				<div class="title">{{seller.name}}</div>
				<div class="desc">
					<star class="star" :size="36" :score="seller.score"></star>
					<span class="text">评论{{seller.ratingCount}}</span>
					<span class="text">月售{{seller.sellCount}}单</span>
				</div>
				<ul class="remark">
					<li class="block">
						<h2>起送价</h2>
						<div class="content">
							<span class="stress">{{seller.minPrice}}</span>元
						</div>
					</li>
					<li class="block">
						<h2>商家配送</h2>
						<div class="content">
							<span class="stress">{{seller.deliveryPrice}}</span>元
						</div>
					</li>
					<li class="block">
						<h2>平均配送时间</h2>
						<div class="content">
							<span class="stress">{{seller.deliveryTime}}</span>分钟
						</div>
					</li>
				</ul>
				<div class="favorite" @click="toggleFavorite($event)">
					<span class="icon-favorite" :class="{'active':favorite}"></span>
					<span class="text">{{favoriteText}}</span>
				</div>
			</div>
			<split></split>
			<div class="bulletin">
				<h1 class="title">公告与活动</h1>
				<div class="content-wrapper">
					<p class="content">{{seller.bulletin}}</p>
				</div>
				<div  class="supports" v-if=seller.supports>
					<div class="supports-item" v-for="(item,index) in seller.supports">
						<span class="icon" :class="classMap[seller.supports[index].type]"></span>
						<span class="text">{{seller.supports[index].description}} </span>
					</div>
				</div>
			</div>
			<split></split>
			<div class="pics">
				<h1 class="title">商家实景</h1>
				<div class="pic-wrapper" ref="picwrapper">
					<ul class="pic-list" ref="piclist">
						<li class="pic-item" v-for="pic in seller.pics">
							<img :src="pic" width="120" height="100">
						</li>
					</ul>
				</div>
			</div>
			<split></split>
			<div class="info">
				<h1 class="title">商家信息</h1>
				<ul>
					<li class="info-item" v-for="info in seller.infos">{{info}}</li>
				</ul>
			</div>
		</div>
		
	</div>
</template>


<script>
import star from '../star/star.vue'
import split from '../split/split.vue'
import bScroll from 'better-scroll';
import {saveToLocal,loadFromLocal} from '../../common/js/store.js';
export default {
	name:'seller',
	props:{
		seller:{
			type:Object
		}
	},
	data(){
		return{
			favorite:(()=>{
				return loadFromLocal(this.seller.id,'favorite',false)
			})()
		}
	},
	computed:{
		favoriteText(){
			return this.favorite ? '已收藏' : '收藏';
		}
	},
	created(){
		

	    if (!this.scroll) {
	        this.$nextTick(() => {
	          this.scroll = new bScroll(this.$refs.seller, {click: true});
	        });
	    } else {
	    	this.scroll.refresh();
	    }
		this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
		if(this.seller.pics){
			this.$nextTick(()=>{
				let picWidth=120;
				let margin=6;
				let width=(picWidth+margin)*this.seller.pics.length-margin;
				this.$refs.piclist.style.width=width+'px';
				this.picScroll=new bScroll(this.$refs.picwrapper,{
					scrollX:true,
					eventPassthrough: 'vertical'
				})
			})
		}
		
	},
	methods:{
		toggleFavorite(event){
			if(!event._constructed){
				return;
			}
			this.favorite=!this.favorite;
			console.log(this.favorite);
			saveToLocal(this.seller.id,'favorite',this.favorite)
		}
	},
	components:{
		star,
		split
	}
}
</script>
<style type="text/css">
.seller{
	position: absolute;
	top:174px;
	bottom:0;
	left:0;
	width:100%;
	overflow: hidden;
}
.seller .seller-wrapper .overview{
	padding:18px;
	position: relative;
}
.seller .seller-wrapper .overview .title{
	margin-bottom:8px;
	line-height: 14px;
	font-size: 14px;
	color:rgb(7,17,27);
	box-sizing: border-box;
	width:100%;
}
.seller .seller-wrapper .overview .desc{
	border-bottom:1px solid rgba(7,17,27,.1);	
	padding-bottom: 18px;
	font-size: 0;
	box-sizing: border-box;
	width:100%;
}
.seller .seller-wrapper .overview .desc .star{
	display: inline-block;
	margin-right: 8px;
	vertical-align: top;
}
.seller .seller-wrapper .overview .desc .text{
	margin-right:12px;
	line-height: 18px;
	font-size: 10px;
	display: inline-block;
	vertical-align: top;
}
.seller .remark{
	display: flex;
	padding:18px;
	box-sizing: border-box;
	width:100%;
}
.seller .remark .block{
	flex: 1;
	text-align: center;
	border-right:1px solid rgba(7,17,27,.1);
}
.seller .remark .block:last-child{
	border-right: none;
}
.seller .remark .block h2{
	line-height: 24px;
	font-size: 10px;
	color:rgb(147,153,159);
}
.seller .remark .block .content{
	color:rgb(7,17,27);
	line-height: 24px;
	font-size: 10px;
}
.seller .remark .block .content .stress{
	font-size: 24px;
}
.seller .favorite{
	position: absolute;
	right:11px;
	top:18px;
	text-align: center;
	width:50px;
}
.seller .favorite .icon-favorite{
	display: block;
	line-height: 24px;
	color:#d4d6d9;
	font-size: 24px;
}
.seller .favorite .active{
	color:rgb(240,20,20);
}
.seller .favorite .text{
	font-size: 10px;
	line-height: 10px;
	color:rgb(7,17,27);
}
.seller .bulletin{
	padding:18px 18px 0 18px;
}
.seller .bulletin .title{
	font-size: 14px;
	color:rgb(7,17,27);
	line-height: 14px;
	margin-bottom:8px;
}
.seller .bulletin .content-wrapper{
	padding:0 12px 16px 12px;
	border-bottom:1px solid rgba(7,17,27,.1);
}
.seller .bulletin .content-wrapper .content{
	line-height: 24px;
	font-size: 12px;
	color:rgb(240,20,20);
}
.seller .supports{
	font-size: 0;
}
.seller .supports .supports-item{
	padding:16px 12px;
	border-bottom:1px solid rgba(7,17,27,.1);
}
.seller .supports .supports-item:last-child{
	border-bottom:none;
}
.seller .supports .icon{
	display: inline-block;
	width:16px;
	height:16px;
	vertical-align: top;	
}
.seller .supports .decrease{
	background: url(decrease_4@2x.png) no-repeat top left/16px 16px;
}
.seller .supports .discount{
	background: url(discount_4@2x.png) no-repeat top left/16px 16px;
}
.seller .supports .special{
	background: url(special_4@2x.png) no-repeat top left/16px 16px;
}
.seller .supports .invoice{
	background: url(invoice_4@2x.png) no-repeat top left/16px 16px;
}
.seller .supports .guarantee{
	background: url(guarantee_4@2x.png) no-repeat top left/16px 16px;
}

@media (-webkit-min-device-pixel-ratio: 3),(min-device-aspect-ratio: 3){
	.seller .supports .decrease{
		background: url(decrease_4@3x.png) no-repeat top left/16px 16px;
	}
	.seller .supports .discount{
		background: url(discount_4@3x.png) no-repeat top left/16px 16px;
	}
	.seller .supports .special{
		background: url(special_4@3x.png) no-repeat top left/16px 16px;
	}
	.seller .supports .invoice{
		background: url(invoice_4@3x.png) no-repeat top left/16px 16px;
	}
	.seller .supports .guarantee{
		background: url(guarantee_4@3x.png) no-repeat top left/16px 16px;
	}
}
.seller .supports-item .text{
	font-size: 12px;
	font-weight: 200;
	line-height: 16px;
	margin-left: 8px;
	vertical-align: top;
	color:rgb(7,17,27);
}

.pics{
	padding:18px;
}
.pics .title{
	font-size: 14px;
	color:rgb(7,17,27);
	line-height: 14px;
	margin-bottom:12px;
}
.pics .pic-wrapper{
	width:100%;
	overflow:hidden;
	white-space: nowrap;
}
.pics .pic-list{
	font-size: 0;
}
.pics .pic-list .pic-item{
	display: inline-block;
	margin-right: 6px;
	width:120px;
	height:90px;
}

.seller .info {
	padding:18px;
}
.seller .info .title{
	padding-bottom: 12px;
	line-height: 14px;
	border-bottom:1px solid rgba(7,17,27,.1);
	color: rgb(7,17,27);
	font-size: 14px;
}
.seller .info .info-item{
	padding:16px 12px;
	line-height: 16px;
	border-bottom:1px solid rgba(7,17,27,.1);
	color: rgb(7,17,27);
	font-size: 12px;
}
.seller .info .info-item:last-child{
	border-bottom:none;
}
</style>