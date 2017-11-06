<template>
	<div class="ratingselect">
		<div class="rating-type line-1px">
			<span @click="select(2,$event)" class="block positive" :class="{'active':selectType===2}">{{desc.all}} <span class="count">{{ratings.length}}</span></span>
			<span @click="select(0,$event)" class="block positive" :class="{'active':selectType===0}">{{desc.positive}} <span class="count">{{getPositive.length}}</span></span>
			<span @click="select(1,$event)" class="block negative" :class="{'active':selectType===1}">{{desc.negative}} <span class="count">{{getNegative.length}}</span></span>
		</div>
		<div @click="toggleCount($event)" class="switch" :class="{'on':onlyContent}">
			<span class="icon-check_circle"></span>
			<span class="text">只看有内容的评价</span>
		</div>
	</div>
</template>

<script>
const POSITIVE=0;
const NEGATIVE=1;
const ALL=2;

export default {
  props:{
  	ratings:{
  		type:Array,
  		default(){
  			return []
  		}
  	},
  	selectType:{
  		type:Number,
  		default:ALL
  	},
  	onlyContent:{
  		type:Boolean,
  		default:false
  	},
  	desc:{
  		type:Object,
  		default(){
  			return{
  				all:'全部',
  				positive:'满意',
  				negative:'吐槽'
  			}  			
  		}
  	}
  },
  methods:{
  	select(type,event){
  		if(!event._constructed){
  			return;
  		}
  		this.selectType=type;
  		this.$emit('increment', 'selectType', type);
  	},
  	toggleCount(event){
		if(!event._constructed){
		  	return;
  		}
  		this.onlyContent=!this.onlyContent;
  		this.$emit('increment','onlyContent',this.onlyContent);
  	}
  },
  computed:{
  	getPositive(){
  		return this.ratings.filter((rating)=>{
  			return rating.rateType===POSITIVE;
  		})
  	},
  	getNegative(){
  		return this.ratings.filter((rating)=>{
  			return rating.rateType===NEGATIVE;
  		})
  	}
  }
}
</script>

<style>
.ratingselect .rating-type{
	padding:18px 0;
	margin:0 18px;
}
.rating-type .block{
	display: inline-block;
	border-radius: 2px;
	padding:8px 12px;
	margin-right: 8px;
	color: rgb(77,85,93);
	line-height: 16px;
	font-size: 12px;
}
.rating-type .positive{
	background: rgba(0,160,220,.2);
}
.rating-type .negative{
	background: rgba(77,85,93,.2);
}
.rating-type .block .count{
	margin-left:2px;	
	font-size: 8px;
}
.rating-type .active{
	color:#fff;
	background: rgb(0,160,220);
}
.switch{
	padding: 12px 18px;
	line-height:24px;
	border-bottom: 1px solid rgba(7,17,27,.1);
	color:rgb(147,153,159);
	font-size: 0;
}
.switch .icon-check_circle{
	margin-right:4px;
	font-size: 24px;
	display: inline-block;
	vertical-align: middle;
}
.switch .text{
	font-size: 12px;
	display: inline-block;
	vertical-align: middle;
}
.on .icon-check_circle{
	color:#00c850;
}
</style>