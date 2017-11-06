<template>
  <div class="header">
	    <div class="content-wrapper clearfix">
		  	<div class="avatar">
		  		<img width="64" height="64" :src="seller.avatar">
		  	</div>
		  	<div class="content">
				<div class="title">
					<span class="brand"></span>
					<span class="name">{{seller.name}}</span>
				</div>
				<div class="description">
					{{seller.description}}/{{seller.deliveryTime}}分钟送达
				</div>
				<div class="support" v-if="seller.supports">
					<span class="icon" :class="classMap[seller.supports[0].type]"></span>
					<span class="text">{{seller.supports[0].description}} </span>
				</div>
				<div v-if="seller.supports" class="support-count"  @click="detailShow">
					<span>{{seller.supports.length}}个</span>
					<i class="icon-keyboard_arrow_right"></i>
				</div>
		  	</div>
	    </div>
    	<div class="bulletin-wrapper" @click="detailShow">
    		<span class="bulletin-title"></span><span class="bulletin-content">{{seller.bulletin}} </span>
    		<i class="icon-keyboard_arrow_right"></i>
    	</div>
    	<div class="hd-bg">
    		<img :src="seller.avatar" width="100%" height="150%">
    	</div>

    	<transition name="fade">
    	<div class="detail" v-show="detail">
			<div class="detail-wrappper clearfix">
				<div class="detail-main">
					<h1>{{seller.name}} </h1>
					<v-star class="score" :size='48' :score="seller.score"></v-star>
					<div class="hr">
						<div class="line"></div>
						<div class="hr-name">优惠信息</div>
						<div class="line"></div>
					</div>
					<div  class="supports" v-if=seller.supports>
						<div class="supports-item" v-for="(item,index) in seller.supports">
							<span class="icon" :class="classMap[seller.supports[index].type]"></span>
							<span class="text">{{seller.supports[index].description}} </span>
						</div>
					</div>
					<div class="hr">
						<div class="line"></div>
						<div class="hr-name">商家公告</div>
						<div class="line"></div>
					</div>
					<div class="notice">
						<p>{{seller.bulletin}}</p>
					</div>
					
				</div>
			</div>
			<div class="detail-close" @click="detailHidden">
				<i class="icon-close"></i>
			</div>
    	</div>
    	</transition>

  </div>
</template>

<script>
import star from '../star/star.vue'
export default {
  name: 'header',
  props: {
      seller: {
        type: Object
      }
  },
  data() {
  	return {
  		detail:false
  	}
  },
  created() {
      this.classMap = ['decrease', 'discount', 'special', 'invoice', 'guarantee'];
  },
  methods:{
  	detailShow:function(){
  		this.detail=true;
  	},
  	detailHidden:function(){
  		this.detail=false;
  	}
  },
  components:{
  	'v-star':star
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}

.header{
	background:rgba(7,17,27,.5);
	color:#fff;
	position: relative;
}

.content-wrapper{
	padding:24px 12px 18px 24px;
	position: relative;
	font-size:0 ;
}

.avatar{
	float: left;
}

.avatar img{
	border-radius: 5px;
	margin-right: 16px;
}

.clearfix::after{
	content:'';
	display:block;
	width:0;
	height:0;
	clear:both;
}
.title{
	margin-top: 2px;
}
.title .name{
	font-size: 16px;
	color: #fff;
	font-weight: block;
	line-height: 18px;
	margin-left: 12px;
}
.title .brand{
	display: inline-block;
	width:30px;
	height:18px;
	vertical-align: top;
	background: url(brand@2x.png) no-repeat top left/30px 18px;
}

@media (-webkit-min-device-pixel-ratio: 3),(min-device-aspect-ratio: 3){
	.title .brand{
		background: url(brand@3x.png) no-repeat top left/30px 18px;
	}
}

.description{
	font-size: 12px;
	font-weight: 200;
	line-height: 12px;
	margin-top:8px;
}

.support{
	margin-top: 10px;
	font-size: 0;
}
.support .icon{
	display: inline-block;
	width:12px;
	height:12px;
	vertical-align: top;
}
.support .decrease{
	background: url(decrease_1@2x.png) no-repeat top left/12px 12px;
}
.support .discount{
	background: url(discount_1@2x.png) no-repeat top left/12px 12px;
}
.support .special{
	background: url(special_1@2x.png) no-repeat top left/12px 12px;
}
.support .invoice{
	background: url(invoice_1@2x.png) no-repeat top left/12px 12px;
}
.support .guarantee{
	background: url(guarantee_1@2x.png) no-repeat top left/12px 12px;
}

@media (-webkit-min-device-pixel-ratio: 3),(min-device-aspect-ratio: 3){
	.support .decrease{
		background: url(decrease_1@3x.png) no-repeat top left/12px 12px;
	}
	.support .discount{
		background: url(discount_1@3x.png) no-repeat top left/12px 12px;
	}
	.support .special{
		background: url(special_1@3x.png) no-repeat top left/12px 12px;
	}
	.support .invoice{
		background: url(invoice_1@3x.png) no-repeat top left/12px 12px;
	}
	.support .guarantee{
		background: url(guarantee_1@3x.png) no-repeat top left/12px 12px;
	}
}
.support .text{
	font-size: 10px;
	font-weight: 200;
	line-height: 12px;
	margin-left: 4px;
}

.support-count{
	height:24px;
	padding:0 8px;
	border-radius: 16px;
	background: rgba(000,000,000,.2);
	line-height: 24px;
	font-size: 10px;
	position: absolute;
	right:12px;
	bottom:16px;
}
.support-count>i{
	vertical-align: middle;
}
.bulletin-wrapper{
	padding:0 22px 0 12px;
	position: relative;
	height:28px;
	background: rgba(000,000,000,.2);
	overflow: hidden;
	white-space: nowrap;
	text-overflow: ellipsis;
	font-size: 10px;
	line-height: 28px;
}
.bulletin-title{
	display: inline-block;
	width:20px;
	height:12px;
	background: url(bulletin@2x.png) no-repeat top left/20px 12px;
	margin-right: 4px;
	vertical-align: middle;
}


.bulletin-content{
	vertical-align: middle;
}
.bulletin-wrapper>i{
	position: absolute;
	width:14px;
	height:14px;
	top:8px;
	right:8px;
}

@media (-webkit-min-device-pixel-ratio: 3),(min-device-aspect-ratio: 3){
	.bulletin-wrapper .bulletin-title{
		background: url(bulletin@3x.png) no-repeat top left/20px 12px;

	}
	.bulletin-wrapper>i{		
		top:11px;		
	}
}
.hd-bg{
	position: absolute;
	top:0;
	left: 0;
	z-index: -1;
	width:100%;
	height:100%;
	overflow: hidden;
	
}
.hd-bg>img{
	filter: blur(10px);
}
.detail{
	position: fixed;
	top:0;
	left:0;
	z-index: 100;
	background: rgba(7,17,27,.8);
	width:100%;
	height:100%;
	overflow: auto;
	backdrop-filter:blur(10px);
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5s
}
.fade-enter, .fade-leave-to {
  opacity: 0
}

.detail-wrappper{
	min-height: 100%;
	
}
.detail-main{
	padding-bottom: 64px;
}
.detail-close{
	margin-top: -64px;
	text-align: center;
	line-height: 64px;
	font-size:32px; 
	color:rgba(255,255,255,.5);
}

.detail-main h1{
	text-align: center;
	padding-top: 64px;
	font-size: 16px;
	font-weight: 700;
	line-height: 16px;
}

.score {
	margin: 16px 0 28px 0;
	text-align: center;
	line-height: 24px;
	height: 24px;
}

.hr{
	width: 80%;
	display: flex;
	margin:28px auto 24px;
}

.hr .line{
	flex:1;
	position: relative;
	top:-6px;
	border-bottom: 1px solid rgba(255,255,255,0.2)
}
.hr .hr-name{
	padding:0 12px;
	font-size: 14px;
	font-weight: 700;
}

.supports {
	width:80%;
	margin:auto;
}

.supports-item{
	margin-left: 12px;
	margin-bottom: 12px;
}

.supports-item .icon{
	display: inline-block;
	width:16px;
	height:16px;
	margin-right: 6px;
	vertical-align: middle;
}

.supports .decrease{
	background: url(decrease_2@2x.png) no-repeat top left/16px 16px;
}
.supports .discount{
	background: url(discount_2@2x.png) no-repeat top left/16px 16px;
}
.supports .special{
	background: url(special_2@2x.png) no-repeat top left/16px 16px;
}
.supports .invoice{
	background: url(invoice_2@2x.png) no-repeat top left/16px 16px;
}
.supports .guarantee{
	background: url(guarantee_2@2x.png) no-repeat top left/16px 16px;
}

@media (-webkit-min-device-pixel-ratio: 3),(min-device-aspect-ratio: 3){
	.supports .decrease{
		background: url(decrease_1@3x.png) no-repeat top left/12px 12px;
	}
	.supports .discount{
		background: url(discount_1@3x.png) no-repeat top left/12px 12px;
	}
	.supports .special{
		background: url(special_1@3x.png) no-repeat top left/12px 12px;
	}
	.supports .invoice{
		background: url(invoice_1@3x.png) no-repeat top left/12px 12px;
	}
	.supports .guarantee{
		background: url(guarantee_1@3x.png) no-repeat top left/12px 12px;
	}
}

.supports-item .text{
	font-size: 12px;
	line-height: 12px;
	vertical-align: middle;
}
.notice{
	margin:0 auto;
	box-sizing: border-box;
	width:80%;
	padding:0 12px;
	font-size: 12px;
	line-height: 24px;
	
}
.notice>p{
	text-align: justify;
}
</style>
